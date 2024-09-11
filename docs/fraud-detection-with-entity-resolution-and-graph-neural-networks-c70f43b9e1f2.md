# 用实体解析和图神经网络进行欺诈检测

> 原文：[https://towardsdatascience.com/fraud-detection-with-entity-resolution-and-graph-neural-networks-c70f43b9e1f2?source=collection_archive---------12-----------------------#2023-08-24](https://towardsdatascience.com/fraud-detection-with-entity-resolution-and-graph-neural-networks-c70f43b9e1f2?source=collection_archive---------12-----------------------#2023-08-24)

## 一本关于如何利用实体解析提升机器学习检测欺诈的实用指南

[](https://medium.com/@stefan.berkner?source=post_page-----c70f43b9e1f2--------------------------------)[![Stefan Berkner](../Images/a84ebfb24744984c0de8f2e77c2070e6.png)](https://medium.com/@stefan.berkner?source=post_page-----c70f43b9e1f2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c70f43b9e1f2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c70f43b9e1f2--------------------------------) [Stefan Berkner](https://medium.com/@stefan.berkner?source=post_page-----c70f43b9e1f2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F704fdfc8efaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffraud-detection-with-entity-resolution-and-graph-neural-networks-c70f43b9e1f2&user=Stefan+Berkner&userId=704fdfc8efaa&source=post_page-704fdfc8efaa----c70f43b9e1f2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c70f43b9e1f2--------------------------------) ·7 分钟阅读·2023 年 8 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc70f43b9e1f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffraud-detection-with-entity-resolution-and-graph-neural-networks-c70f43b9e1f2&user=Stefan+Berkner&userId=704fdfc8efaa&source=-----c70f43b9e1f2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc70f43b9e1f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffraud-detection-with-entity-resolution-and-graph-neural-networks-c70f43b9e1f2&source=-----c70f43b9e1f2---------------------bookmark_footer-----------)![](../Images/e57d32077ac975a1f0e648b92dcf4e75.png)

由作者使用 Bing Image Creator 生成的图神经网络的表示

在线欺诈对金融、电子商务及其他相关行业是一个不断增长的问题。为了应对这一威胁，组织利用基于机器学习和行为分析的欺诈检测机制。这些技术能够实时检测到异常模式、异常行为和欺诈活动。

不幸的是，通常只考虑当前交易，例如一个订单，或者流程仅基于客户档案中的历史数据，该档案通过客户 ID 进行识别。然而，专业的欺诈者可能会使用低价值的交易来创建客户档案，从而建立其档案的正面形象。此外，他们可能会同时创建多个类似的档案。只有在欺诈发生后，受攻击的公司才会意识到这些客户档案彼此之间的关系。

使用实体解析可以轻松地将不同的客户档案合并为一个 360° 客户视图，从而看到所有历史交易的完整情况。在机器学习中使用这些数据，例如使用神经网络或简单的线性回归，已经可以为最终模型提供额外的价值，而真正的价值在于还要查看各个交易之间是如何相互连接的。这就是图神经网络（GNN）发挥作用的地方。除了查看从交易记录中提取的特征外，它们还提供了查看从图边生成的特征（交易如何相互链接）或仅仅是实体图的一般布局的可能性。

# 示例数据

在我们深入细节之前，我有一个声明要在这里说明：我是一名开发者和实体解析专家，而不是数据科学家或 ML 专家。虽然我认为总体方法是正确的，但我可能没有遵循最佳实践，也不能解释某些方面，例如隐藏节点的数量。请将本文作为灵感来源，并根据自己的经验来考虑 GNN 的布局或配置。

本文旨在聚焦于从实体图布局中获得的见解。为此，我创建了一个小型 Golang 脚本，用于生成实体。每个实体被标记为欺诈或非欺诈，并由记录（订单）和边（这些订单如何链接）组成。请参见以下单个实体的示例：

```py
{
  "fraud":1,
  "records":[
    {
      "id":0,
      "totalValue":85,
      "items":2
    },
    {
      "id":1,
      "totalValue":31,
      "items":4
    },
    {
      "id":2,
      "totalValue":20,
      "items":9
    }
  ],
  "edges":[
    {
      "a":1,
      "b":0,
      "R1":1,
      "R2":1
    },
    {
      "a":2,
      "b":1,
      "R1":0,
      "R2":1
    }
  ]
}
```

每条记录有两个（潜在的）特征，总值和购买的项目数量。然而，生成脚本完全随机化了这些值，因此它们在猜测欺诈标签时不会提供价值。每条边还包含两个特征 R1 和 R2。例如，这些特征可能表示记录 A 和 B 是否通过类似的姓名和地址（R1）或通过类似的电子邮件地址（R2）进行链接。此外，我故意省略了所有与本示例无关的属性（如姓名、地址、电子邮件、电话号码等），这些属性通常在实体解析过程之前是相关的。由于 R1 和 R2 也是随机化的，它们也不会为 GNN 提供价值。然而，根据欺诈标签，边的布局有两种可能方式：星形布局（fraud=0）或随机布局（fraud=1）。

这个想法是非欺诈客户更可能提供准确匹配的相关数据，通常是相同的地址和名字，仅有少量拼写错误。因此，新交易可能会被[识别为重复](https://tilores.io/data-deduplication-software)。

![](../Images/3dcf16149f9257f18987e730f5cbbe08.png)

去重实体（图片由作者提供）

欺诈客户可能会想掩盖他们仍然是计算机背后的同一个人，使用各种名字和地址。然而，实体解析工具仍然可能识别出相似性（例如地理和时间相似性、电子邮件地址中的重复模式、设备ID等），但实体图可能看起来更复杂。

![](../Images/bfeca362a7a7664ad1c8a7dae83a857a.png)

复杂的、可能是欺诈性的实体（图片由作者提供）

为了让它不那么简单，生成脚本还具有5%的错误率，这意味着当实体具有星形布局时，会被标记为欺诈，而随机布局则被标记为非欺诈。此外，还有一些情况下数据不足以确定实际布局（例如只有一两条记录）。

```py
{
  "fraud":1,
  "records":[
    {
      "id":0,
      "totalValue":85,
      "items":5
    }
  ],
  "edges":[

  ]
}
```

实际上，你很可能会从所有三种特征（记录属性、边属性和边布局）中获得有价值的见解。以下代码示例将考虑这一点，但生成的数据则没有。

# 创建数据集

示例使用了python（数据生成除外）和[DGL](https://www.dgl.ai)，并且使用了[pytorch后台](https://pytorch.org)。你可以在[github](https://gist.github.com/stefan-berkner-tilotech/06a9ebaa4e4e52282785e8d5f2b6429c#file-er-and-gnn-ipynb)上找到完整的jupyter笔记本、数据和生成脚本。

让我们从导入数据集开始：

```py
import os

os.environ["DGLBACKEND"] = "pytorch"
import pandas as pd
import torch
import dgl
from dgl.data import DGLDataset

class EntitiesDataset(DGLDataset):
    def __init__(self, entitiesFile):
        self.entitiesFile = entitiesFile
        super().__init__(name="entities")

    def process(self):
        entities = pd.read_json(self.entitiesFile, lines=1)

        self.graphs = []
        self.labels = []

        for _, entity in entities.iterrows():
            a = []
            b = []
            r1_feat = []
            r2_feat = []
            for edge in entity["edges"]:
                a.append(edge["a"])
                b.append(edge["b"])
                r1_feat.append(edge["R1"])
                r2_feat.append(edge["R2"])
            a = torch.LongTensor(a)
            b = torch.LongTensor(b)
            edge_features = torch.LongTensor([r1_feat, r2_feat]).t()

            node_feat = [[node["totalValue"], node["items"]] for node in entity["records"]]
            node_features = torch.tensor(node_feat)

            g = dgl.graph((a, b), num_nodes=len(entity["records"]))
            g.edata["feat"] = edge_features
            g.ndata["feat"] = node_features
            g = dgl.add_self_loop(g)

            self.graphs.append(g)
            self.labels.append(entity["fraud"])

        self.labels = torch.LongTensor(self.labels)

    def __getitem__(self, i):
        return self.graphs[i], self.labels[i]

    def __len__(self):
        return len(self.graphs)

dataset = EntitiesDataset("./entities.jsonl")
print(dataset)
print(dataset[0])
```

这处理了实体文件，这是一个JSON行文件，每行表示一个实体。在迭代每个实体时，它生成边特征（形状为[e, 2]的长张量，e=边数）和节点特征（形状为[n, 2]的长张量，n=节点数）。然后，它根据a和b（每个形状为[e, 1]的长张量）构建图，并将边和图特征分配给该图。所有生成的图都被添加到数据集中。

# 模型架构

现在我们已经准备好数据了，我们需要考虑GNN的架构。这是我想到的，但可能需要根据实际需求进行更多调整：

```py
import torch.nn as nn
import torch.nn.functional as F
from dgl.nn import NNConv, SAGEConv

class EntityGraphModule(nn.Module):
    def __init__(self, node_in_feats, edge_in_feats, h_feats, num_classes):
        super(EntityGraphModule, self).__init__()
        lin = nn.Linear(edge_in_feats, node_in_feats * h_feats)
        edge_func = lambda e_feat: lin(e_feat)
        self.conv1 = NNConv(node_in_feats, h_feats, edge_func)

        self.conv2 = SAGEConv(h_feats, num_classes, "pool")

    def forward(self, g, node_features, edge_features):
        h = self.conv1(g, node_features, edge_features)
        h = F.relu(h)
        h = self.conv2(g, h)
        g.ndata["h"] = h
        return dgl.mean_nodes(g, "h")
```

构造函数接受节点特征的数量、边特征的数量、隐藏节点的数量和标签（类别）的数量。然后创建两个层次：[NNConv层](https://docs.dgl.ai/en/latest/generated/dgl.nn.pytorch.conv.NNConv.html)，根据边和节点特征计算隐藏节点，然后是[GraphSAGE层](https://docs.dgl.ai/en/latest/generated/dgl.nn.pytorch.conv.SAGEConv.html)，根据隐藏节点计算结果标签。

# 训练和测试

快完成了。接下来我们准备数据进行训练和测试。

```py
from torch.utils.data.sampler import SubsetRandomSampler
from dgl.dataloading import GraphDataLoader

num_examples = len(dataset)
num_train = int(num_examples * 0.8)

train_sampler = SubsetRandomSampler(torch.arange(num_train))
test_sampler = SubsetRandomSampler(torch.arange(num_train, num_examples))

train_dataloader = GraphDataLoader(
    dataset, sampler=train_sampler, batch_size=5, drop_last=False
)
test_dataloader = GraphDataLoader(
    dataset, sampler=test_sampler, batch_size=5, drop_last=False
)
```

我们使用80/20的比例进行随机抽样，并为每个样本创建数据加载器。

最后一步是用我们的数据初始化模型，进行训练，然后测试结果。

```py
h_feats = 64
learn_iterations = 50
learn_rate = 0.01

model = EntityGraphModule(
    dataset.graphs[0].ndata["feat"].shape[1],
    dataset.graphs[0].edata["feat"].shape[1],
    h_feats,
    dataset.labels.max().item() + 1
)
optimizer = torch.optim.Adam(model.parameters(), lr=learn_rate)

for _ in range(learn_iterations):
    for batched_graph, labels in train_dataloader:
        pred = model(batched_graph, batched_graph.ndata["feat"].float(), batched_graph.edata["feat"].float())
        loss = F.cross_entropy(pred, labels)
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

num_correct = 0
num_tests = 0
for batched_graph, labels in test_dataloader:
    pred = model(batched_graph, batched_graph.ndata["feat"].float(), batched_graph.edata["feat"].float())
    num_correct += (pred.argmax(1) == labels).sum().item()
    num_tests += len(labels)

acc = num_correct / num_tests
print("Test accuracy:", acc)
```

我们通过提供节点和边缘的特征大小（在我们的例子中均为2），隐藏节点（64）以及标签数量（2，因为只有欺诈或非欺诈）来初始化模型。然后，优化器以0.01的学习率初始化。之后我们运行总共50次训练迭代。一旦训练完成，我们使用测试数据加载器测试结果，并打印出结果准确率。

对于各种运行，我的典型准确率在70%到85%之间。然而，也有一些例外情况，准确率降到大约55%。

# 结论

鉴于我们示例数据集中唯一可用的信息是节点如何连接的解释，初步结果看起来非常有前景，并且建议使用真实世界数据和更多训练可能会实现更高的准确率。

显然，在处理真实数据时，布局并不那么一致，也没有明显的布局与欺诈行为之间的关联。因此，你还应该考虑边缘和节点的特征。本文的关键要点应是实体解析提供了使用图神经网络进行欺诈检测的理想数据，并且应视为欺诈检测工程师工具库的一部分。

*最初发布于* [*https://tilores.io*](https://tilores.io/content/Fraud-Detection-with-Entity-Resolution-and-Graph-Neural-Networks)*.*
