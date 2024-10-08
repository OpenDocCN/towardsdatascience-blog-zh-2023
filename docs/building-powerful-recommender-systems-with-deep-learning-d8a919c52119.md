# 使用深度学习构建强大的推荐系统

> 原文：[`towardsdatascience.com/building-powerful-recommender-systems-with-deep-learning-d8a919c52119`](https://towardsdatascience.com/building-powerful-recommender-systems-with-deep-learning-d8a919c52119)

![](img/8f6471ecbf2d007861bcf4ab5b5aa65a.png)

作者插图

## 使用 PyTorch 库 TorchRec 的逐步实现

[](https://linafaik.medium.com/?source=post_page-----d8a919c52119--------------------------------)![Lina Faik](https://linafaik.medium.com/?source=post_page-----d8a919c52119--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d8a919c52119--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8a919c52119--------------------------------) [Lina Faik](https://linafaik.medium.com/?source=post_page-----d8a919c52119--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8a919c52119--------------------------------) ·阅读时长 7 分钟·2023 年 7 月 3 日

--

在正确的时间向客户推荐合适的产品是各行各业普遍面临的挑战。例如，银行家不断寻找向现有或潜在客户推荐高度相关服务的机会。零售商努力推荐符合客户口味的吸引人产品。类似地，社交网络旨在构建引人入胜的动态，以促进用户采纳。

尽管这是一个被广泛研究的用例，但由于问题的独特性，取得令人满意的性能结果仍然困难重重。主要原因包括存在大量分类数据，通常会导致稀缺问题，以及用例的计算方面，带来扩展性问题。直到最近，推荐模型才开始利用神经网络。

在这种背景下，Meta 开发并公开了一个深度学习推荐模型（DRLM）。该模型因结合了协同过滤和预测分析的原理，并适用于大规模生产而特别出色。

## **目标**

本文的**目标**是通过使用 PyTorch 库 TorchRec 的逐步实现，帮助你有效解决自己的推荐系统问题。

阅读本文后，你将理解：

1.  DLRM 模型如何工作？

1.  DLRM 模型有什么独特之处，使其强大而具有可扩展性？

1.  你如何从头到尾实现自己的推荐系统？

> *本文需要对推荐系统问题有一般性的了解，并熟悉 pytorch 库。本文所述的实验使用了库* [*TorchRec*](https://pytorch.org/torchrec/) *和 PyTorch。你可以在 GitHub 上找到代码* [*这里*](https://github.com/linafaik08/recommender_systems_dlrm) *。*

[](https://github.com/linafaik08/recommender_systems_dlrm?source=post_page-----d8a919c52119--------------------------------) [## GitHub - linafaik08/recommender_systems_dlrm

### 通过在 GitHub 上创建账户来为 linafaik08/recommender_systems_dlrm 的开发做出贡献。

github.co](https://github.com/linafaik08/recommender_systems_dlrm?source=post_page-----d8a919c52119--------------------------------)

# 1. 解码 DLRM 模型

让我们首先深入了解 DLRM 模型的复杂性，并探索其基本原理和机制。

## 1.1. 模型设计概述

为了提供一个更具体的例子，我们来考虑一个在线零售商希望为每个访问其网站的客户创建个性化推荐的场景。

为了实现这一点，零售商可以训练一个模型，该模型预测客户购买特定产品的概率。该模型根据各种因素为每个客户的每个产品分配一个分数。推荐流通过对这些分数进行排序来构建。

在这种情况下，模型可以从涵盖每个客户和产品一系列信息的历史数据中学习。这包括诸如客户年龄和产品价格等数值变量，以及产品类型、颜色等类别特征。

DLRM 模型的优势在于：它具有利用数值变量和类别变量的非凡能力，即使在处理大量独特类别时也是如此。这使得模型能够全面分析和理解特征之间的复杂关系。要理解原因，我们来看一下图 1 中的架构模型。

![](img/a5d7e4f4d21de2f6f97990b514200e4b.png)

图 1 — DLRM 模型架构，由作者绘制，灵感来自 [5]

**类别特征**

DLRM 为每个类别特征学习一个嵌入表，并使用这些表将这些变量映射到稠密表示。因此，每个类别特征都表示为相同长度的向量。

**数值特征**

DLRM 通过一个称为底部 MLP 的多层感知器（MLP）处理数值特征。该 MLP 的输出维度与之前的嵌入向量相同。

**成对交互**

DLRM 计算所有嵌入向量与处理过的数值特征之间的点积。这使得模型能够包括二阶特征交互。

**连接与最终输出**

DLRM 将这些点积与处理过的数值特征进行拼接，并将结果输入到另一个 MLP 中，称为顶部 MLP。最终的概率通过将此 MLP 的输出传递到 sigmoid 函数获得。

## 1.2\. 模型实现

尽管模型在理论上具有良好的潜力，但其实际实施仍然存在计算难题。

通常，推荐系统涉及处理大量数据。特别是使用 DLRM 模型会引入非常多的参数，超过了常见深度学习模型。因此，这增加了其实现所需的计算需求。

1.  DLRM 中大多数参数可归因于嵌入，因为它们由多个表组成，每个表都需要大量内存。这使得 DLRM 在内存容量和带宽方面都具有较高的计算需求。

1.  尽管 MLP 参数的内存占用较小，但仍然需要大量计算资源。

为了缓解内存瓶颈，DLRM 依赖于一种独特的模型并行（用于嵌入）和数据并行（用于 MLP）的组合。

# 2\. 从概念到实现：构建自定义推荐系统的逐步指南

本节提供了从头到尾实现自己推荐系统的详细逐步指南。

## 2.1\. 数据转换和批处理构建

第一步是将数据转换为张量并将其组织成批次，以输入到模型中。

为了说明这个过程，让我们以这个数据框为例。

![](img/d1afe7be20a6083ca02cc8a257e052a7.png)

对于稀疏特征，我们需要将值拼接成一个向量并计算长度。这可以使用 *KeyedJaggedTensor.from_lengths_sync* 函数来完成，该函数接受两个元素作为输入。以下是 Python 脚本的一个示例：

```py
values = sample[cols_sparse].sum(axis=0).sum(axis=0)
values = torch.tensor(values).to(device)
# values = tensor([1, 0, 2, 0, 2, 2, 0, 2, 0, 1, 0, 1, 2, 0], device='cuda:0')

lengths = torch.tensor(
    pd.concat([sample[feat].apply(lambda x: len(x)) for feat in cols_sparse],
              axis=0).values,
    dtype=torch.int32
).to(self.device)
# lengths = tensor([1, 1, 1, 1, 1, 2, 3, 2, 2, 0], device='cuda:0', dtype=torch.int32)

sparse_features = KeyedJaggedTensor.from_lengths_sync(
  keys=cols_sparse,
  values=values,
  lengths=lengths
)
```

对于密集特征和标签，过程更为简单。这是 Python 脚本的一个示例：

```py
dense_features = torch.tensor(sample[cols_dense].values, dtype=torch.float32).to(device)
labels = torch.tensor(sample[col_label].values, dtype=torch.int32).to(device)
```

通过使用前一步骤的输出，可以构建一个批次。这是 Python 脚本的一个示例：

```py
batch = Batch(
  dense_features=dense_features,
  sparse_features=sparse_features,
  labels=labels,
).to(device)
```

要获取更全面的实现，可以参考 GitHub [batch.py](https://github.com/linafaik08/recommender_systems_dlrm/blob/main/src/batch.py) 文件及相关 GitHub [仓库](https://github.com/linafaik08/recommender_systems_dlrm/)。

## 2.2\. 模型初始化和优化设置

下一步涉及初始化模型，如以下 Python 代码所示：

```py
# Initialize the model and set up optimization

# Define the dimensionality of the embeddings used in the model
embedding_dim = 10

# Calculate the number of embeddings per feature
num_embeddings_per_feature = {c: len(v) for c, v in map_sparse.items()}

# Define the layer sizes for the dense architecture
dense_arch_layer_sizes = [512, 256, embedding_dim]

# Define the layer sizes for the overall architecture
over_arch_layer_sizes = [512, 512, 256, 1]

# Specify whether to use Adagrad optimizer or SGD optimizer
adagrad = False

# Set the epsilon value for Adagrad optimizer
eps = 1e-8

# Set the learning rate for optimization
learning_rate = 0.01

# Create a list of EmbeddingBagConfig objects for each sparse feature
eb_configs = [
    EmbeddingBagConfig(
        name=f"t_{feature_name}",
        embedding_dim=embedding_dim,
        num_embeddings=num_embeddings_per_feature[feature_name + '_enc'],
        feature_names=[feature_name + '_enc'],
    )
    for feature_idx, feature_name in enumerate(cols_sparse)
]

# Initialize the DLRM model with the embedding bag collection and architecture specifications
dlrm_model = DLRM(
    embedding_bag_collection=EmbeddingBagCollection(
        tables=eb_configs, device=device
    ),
    dense_in_features=len(cols_dense),
    dense_arch_layer_sizes=dense_arch_layer_sizes,
    over_arch_layer_sizes=over_arch_layer_sizes,
    dense_device=device,
)

# Create a DLRMTrain instance for handling training operations
train_model = DLRMTrain(dlrm_model).to(device)

# Choose the appropriate optimizer class for the embedding parameters
embedding_optimizer = torch.optim.Adagrad if adagrad else torch.optim.SGD

# Set the optimizer keyword arguments
optimizer_kwargs = {"lr": learning_rate}
if adagrad:
    optimizer_kwargs["eps"] = eps

# Apply the optimizer to the sparse architecture parameters
apply_optimizer_in_backward(
    optimizer_class=embedding_optimizer,
    params=train_model.model.sparse_arch.parameters(),
    optimizer_kwargs=optimizer_kwargs,
)

# Initialize the dense optimizer with the appropriate parameters
dense_optimizer = KeyedOptimizerWrapper(
    dict(in_backward_optimizer_filter(train_model.named_parameters())),
    optimizer_with_params(adagrad, learning_rate, eps),
)

# Create a CombinedOptimizer instance to handle optimization
optimizer = CombinedOptimizer([dense_optimizer])
```

然后可以使用以下代码训练和评估模型：

```py
loss, (loss2, logits, labels) = train_model(batch)
```

要获取更全面的实现，可以参考 GitHub [model.py](https://github.com/linafaik08/recommender_systems_dlrm/blob/main/src/model.py) 文件及相关 GitHub [仓库](https://github.com/linafaik08/recommender_systems_dlrm/)。

# 关键要点

✔ DLRM 模型提供了一种有效结合数值和类别特征的引人注目的方法，使用嵌入技术，使得模型能够捕捉复杂的模式和关系。

✔ 尽管其架构需要相当的计算资源，但其实现结合了模型并行和数据并行的独特组合，使得模型在生产环境中具有可扩展性。

✔ 然而，由于数据的有限性，该模型的性能尚未在各种真实世界的数据集上进行广泛测试。这引发了对其在实际场景中有效性的担忧。

✔ 此外，该模型需要调整大量参数，这进一步使得过程复杂化。

✔ 考虑到这一点，像 LGBM 这样更简单的模型可能提供类似的性能，并且具有更简单的实现、调优和长期维护，而不会有相同的计算开销。

# 参考文献

[1] M Naumov 等，[用于个性化和推荐系统的深度学习推荐模型](https://arxiv.org/abs/1906.00091?fbclid=IwAR1yd08rme7ON-rIRO0IiooMJEtv0JBYyWPpLuAlli3zmPy2XmJP92UtM7k)，2019 年 5 月

[2] [Facebook 团队的 DLRM 模型初步实现的 Github 仓库](https://github.com/facebookresearch/dlrm?fbclid=IwAR1YrymUGB7IG8k2x7h4cw7JXRkdS8NcrITUUQ71K2GrbVE5CNxMFf2esFQ)，开放源代码

[3] DLRM：一种先进的开源深度学习推荐模型，Meta AI 博客，2019 年 7 月

[4] 现代生产推荐系统的 Pytorch 库，[torchec](https://pytorch.org/blog/introducing-torchrec/)

[5] Vinh Nguyen, Tomasz Grel 和 Mengdi Huang，[优化 NVIDIA GPU 上的深度学习推荐模型](https://developer.nvidia.com/blog/optimizing-dlrm-on-nvidia-gpus/)，2020 年 6 月
