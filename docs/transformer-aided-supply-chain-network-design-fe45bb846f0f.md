# 变压器辅助的供应链网络设计

> 原文：[https://towardsdatascience.com/transformer-aided-supply-chain-network-design-fe45bb846f0f?source=collection_archive---------9-----------------------#2023-02-17](https://towardsdatascience.com/transformer-aided-supply-chain-network-design-fe45bb846f0f?source=collection_archive---------9-----------------------#2023-02-17)

## 使用变压器来帮助解决供应链中的经典问题——设施选址问题

[](https://medium.com/@guanx92?source=post_page-----fe45bb846f0f--------------------------------)[![Guangrui Xie](../Images/def9aa637424a88d75a6a3bb103350bc.png)](https://medium.com/@guanx92?source=post_page-----fe45bb846f0f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe45bb846f0f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fe45bb846f0f--------------------------------) [Guangrui Xie](https://medium.com/@guanx92?source=post_page-----fe45bb846f0f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F495b92f0c66d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformer-aided-supply-chain-network-design-fe45bb846f0f&user=Guangrui+Xie&userId=495b92f0c66d&source=post_page-495b92f0c66d----fe45bb846f0f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe45bb846f0f--------------------------------) ·15 min read·2023年2月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe45bb846f0f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformer-aided-supply-chain-network-design-fe45bb846f0f&user=Guangrui+Xie&userId=495b92f0c66d&source=-----fe45bb846f0f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe45bb846f0f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformer-aided-supply-chain-network-design-fe45bb846f0f&source=-----fe45bb846f0f---------------------bookmark_footer-----------)![](../Images/bd64ec628e3e129b102daba43f9a1307.png)

照片由[Mika Baumeister](https://unsplash.com/es/@mbaumi?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

由于ChatGPT的广泛智能来完成各种任务，最近成为了一个热门话题。ChatGPT的核心模型是变压器，最初在谷歌研究团队的著名论文[*Attention is all you need*](https://arxiv.org/pdf/1706.03762.pdf)中首次提出。变压器使用注意力机制和位置编码一次处理整个句子，而不是像递归神经网络（如LSTM、GRU）那样逐字处理。这样可以减少递归过程中的信息丢失，从而使变压器比传统的递归神经网络更能有效地学习长距离上下文依赖关系。

变压器的应用不仅限于语言模型。自首次提出以来，研究人员已将其应用于许多其他任务，如时间序列预测、计算机视觉等。我一直在思考的问题是，变压器能否用于帮助解决供应链领域的实际运筹学（OR）问题？在本文中，我将展示一个使用变压器来辅助解决供应链领域经典OR问题——设施选址问题的例子。

# 设施选址问题

让我们首先回顾一下这个问题的基本设置。我在[我的第一篇文章](https://medium.com/towards-data-science/some-thoughts-on-synergies-between-operations-research-and-machine-learning-921d78ed4bd5)中也简要讨论了这个问题。

假设一家公司想要在*I*个候选地点中建设配送中心（DCs），以将成品运送给*J*个客户。每个地点*i*都有其关联的存储容量，最多可以存放*m_i*单位产品。 在地点*i*建立一个DC需要固定的建设费用*f_i*。从地点*i*向客户*j*运送每单位产品需要运费*c_ij*。每个客户*j*的需求为*d_j*，且所有客户的需求必须得到满足。设二进制变量*y_i*表示是否在地点*i*建立一个DC，而*x_ij*表示从地点*i*向客户*j*运送的产品量。目标是最小化总成本的优化问题可以表述如下：

![](../Images/40d846413209efd414da65ab02859c3c.png)

设施选址问题的公式化（图由作者提供）

这是一个混合整数线性规划（MILP）问题，可以使用任何商业或非商业求解器（例如 CPLEX、GUROBI、SCIP）来解决。然而，当模型变得很大（*I* 和 *J* 很大）时，求解模型可能会花费很长时间。这个问题的主要复杂性来自整数变量 *y_i’*s，因为这些变量是需要在分支定界算法中进行分支的。如果我们能找到一种更快的方法来确定 *y_i’*s 的值，那将节省大量的求解时间。这就是变压器发挥作用的地方。

# 变压器辅助解决方案方法

这种解决方案方法的基本思想是通过从大量实例的设施选址问题的解决方案中学习，训练一个变压器来预测 *y_i’*s 的值。这个问题可以被公式化为一个分类问题，因为 *y_i’*s 只能取 0 或 1 的值，对应两个类别。类别 0 表示站点 *i* 没有被选中用于建立 DC，类别 1 表示站点 *i* 被选中。一旦训练完成，变压器可以一次性预测每个 *y_i* 属于哪个类别，从而节省大量用于整数变量分支的时间。下图展示了这里采用的变压器架构。

![](../Images/3eb9052c67956f25dcd012391d2866d2.png)

本文采用的变压器模型的架构（作者提供的图像）

如上图所示，每个候选 DC 站点和客户的向量表示（包含每个候选 DC 站点和客户的相关特征）被传递到一个线性层。线性层为每个候选 DC 站点和客户生成一个嵌入向量。这些嵌入向量随后通过论文中描述的 *N* 个标准编码块 [*Attention is all you need*](https://arxiv.org/pdf/1706.03762.pdf) 进行处理。在编码块中，多头注意机制允许每个候选 DC 站点关注所有客户和其他候选 DC 站点的信息。然后，编码块的输出通过一个线性层 + softmax 层作为解码过程。

我为这个特定问题对变压器架构所做的一些调整是：

1.  由于从优化问题的角度来看，输入序列 *V_dc1, …, V_cusJ* 的确切顺序并不重要，因此去除了位置编码。换句话说，随机打乱输入序列对变压器的输出不应影响设施选址问题的解决。

1.  输出层为输入序列中的每个元素 *V_dc1, …, V_cusJ* 生成一个二维向量。这个二维向量包含该元素属于类别 0 和类别 1 的概率。注意，这个二维向量对于元素 *V_cus1, …, V_cusJ* 实际上是无效的，因为我们不能在客户的站点上建立 DC。因此，在训练变换器时，我们对 *V_cus1, …, V_cusJ* 对应的输出进行掩蔽，以计算损失函数，因为我们不关心这些输出是什么。

在这个设施选址问题中，我们假设单位运输成本 *c_ij* 与站点 *i* 和客户 *j* 之间的距离成正比。我们定义 *V_dci* 为 *[dci_x, dci_y, m_i, f_i, 0]*，其中 *dci_x* 和 *dci_y* 是候选 DC 站点 *i* 的坐标，*0* 表示这是一个候选 DC 站点特征的向量表示；我们定义 *V_cusj* 为 *[cusj_x, cusj_y, d_j, 0, 1]*，其中 *cusj_x* 和 *cusj_y* 是客户 *j* 的坐标，*0* 是 *V_dci* 中 *f_i* 的对应项，表示在客户处没有固定费用，并且 *1* 表示这是客户的向量表示。因此，输入序列中的每个元素是一个 5 维向量。通过第一层线性层，我们将每个向量投影到一个高维嵌入向量，并将其输入到编码器块中。

要训练变换器，我们首先需要构建一个数据集。为了设置数据集中的标签，我们使用优化求解器（即 SCIP）求解大量设施选址问题的实例，然后记录每个实例的 *y_i’*s 的值。每个实例的坐标、*d_j’*s、*m_i’*s 和 *f_i’*s 用于构建变换器的输入序列。整个数据集被分成训练集和测试集。

一旦变换器训练完成，它可以用来预测每个候选DC站点 *i* 属于哪个类别，换句话说，就是 *y_i* 是 0 还是 1。然后我们将每个 *y_i* 固定为其预测值，并用剩余的变量 *x_ij’*s* 来求解 MILP。注意，一旦 *y_i’*s 的值被固定，设施选址问题变成一个 LP，这使得使用优化求解器解决起来更为简单。得到的解可能是次优的，但对于大规模实例可以节省大量的求解时间。

变换器辅助解决方案方法的一个警告是，变换器给出的预测* y_i *值有时可能导致MILP不可行，因为变换器的预测并不完全准确。不可行性是由于* m_i * y_i *的总和小于* d_j *的总和，这意味着如果我们按照预测的* y_i *值操作，将没有足够的供应来满足所有客户的总需求。在这种情况下，我们保留* y_i *为1的值，并在剩余的候选DC站点中搜索，以弥补总供应和需求之间的差距。实际上，我们需要解决如下的另一个优化问题：

![](../Images/9f98fb0bfb3931f261f93658268536a6.png)

解决不可行性的问题表述（图片由作者提供）

这里，* I_0 *是预测* y_i *值为0的候选DC站点集合，* I_1 *是预测* y_i *值为1的候选DC站点集合。这本质上是一个背包问题，可以通过优化求解器解决。然而，对于大型实例，该问题仍可能消耗大量求解时间。因此，我决定采用启发式方法解决这个问题——解决分数背包问题的贪婪算法。具体来说，我们计算每个候选DC站点在* I_0 *中的比例* f_i / m_i *，并根据比例对站点进行升序排序。然后我们从第一个站点到最后一个站点逐个选择，直到满足约束条件。完成此过程后，将* I_0 *中所选站点的* y_i *值与* I_1 *中的所有站点设置为1，其余站点的* y_i *值设置为0。然后将这种配置输入优化求解器以求解其余的* x_ij *变量。

# 数值实验

我随机创建了800个设施选址问题实例，包含100个候选DC站点和200个客户。我使用了560个实例进行训练，240个实例进行测试。非商业求解器SCIP被用于解决这些实例。生成实例的代码如下。

```py
from pyscipopt import Model, quicksum
import numpy as np
import pickle

rnd = np.random
models_output = []
for k in range(1000):
    info = {}
    np.random.seed(k)
    n_dc = 100
    n_cus = 200
    c = []
    loc_x_dc = rnd.rand(n_dc)*100
    loc_y_dc = rnd.rand(n_dc)*100
    loc_x_cus = rnd.rand(n_cus)*100
    loc_y_cus = rnd.rand(n_cus)*100
    xy_dc = [[x,y] for x,y in zip(loc_x_dc,loc_y_dc)]
    xy_cus = [[x,y] for x,y in zip(loc_x_cus,loc_y_cus)]
    for i in xy_dc:
        c_i = []
        for j in xy_cus:
            c_i.append(np.sqrt((i[0]-j[0])**2 + (i[1]-j[1])**2)*0.01)
        c.append(c_i)    

    f= []
    m = []
    d = []

    for i in range(n_dc):
        f_rand = np.round(np.random.normal(100,15))
        if f_rand < 40:
            f.append(40)
        else:
            f.append(f_rand)
        m_rand = np.round(np.random.normal(70,10))
        if m_rand < 30:
            m.append(30)
        else:
            m.append(m_rand)
    for i in range(n_cus):
        d_rand = np.round(np.random.normal(20,5))
        if d_rand < 5:
            d.append(5)
        else:
            d.append(d_rand)

    f = np.array(f)
    m = np.array(m)
    d = np.array(d)
    c = np.array(c)

    model = Model("facility selection")
    x,y = {},{}
    x_names = ['x_'+str(i)+'_'+str(j) for i in range(n_dc) for j in range(n_cus)]
    y_names = ['y_'+str(i) for i in range(n_dc)]
    for i in range(n_dc):
        for j in range(n_cus):
            x[i,j] = model.addVar(name=x_names[i*n_cus+j])
    for i in range(n_dc):
        y[i] = model.addVar(name=y_names[i],vtype='BINARY')
    model.setObjective(quicksum(f[i]*y[i] for i in range(n_dc))+quicksum(c[i,j]*x[i,j] for i in range(n_dc) for j in range(n_cus)), 'minimize')
    for j in range(n_cus):
        model.addCons(quicksum(x[i,j] for i in range(n_dc)) == d[j])
    for i in range(n_dc):
        model.addCons(quicksum(x[i,j] for j in range(n_cus)) <= m[i]*y[i])
    model.optimize()

    y_sol = []
    for i in range(n_dc):
        y_sol.append(model.getVal(y[i]))
    x_sol = []
    for i in range(n_dc):
        x_i = []
        for j in range(n_cus):
            x_i.append(model.getVal(x[i,j]))
        x_sol.append(x_i)
    obj_vol = model.getObjVal()

    info['f'] = f
    info['m'] = m
    info['d'] = d
    info['c'] = c
    info['y'] = y_sol
    info['x'] = x_sol
    info['obj'] = obj_vol
    info['xy_dc'] = xy_dc
    info['xy_cus'] = xy_cus
    models_output.append(info)

with open('models_output.pkl', 'wb') as outp:
    pickle.dump(models_output, outp)
```

然后我们读取解决方案并创建用于训练变换器的数据集。请注意，在构建数据集时，我将笛卡尔坐标系转换为极坐标系，因为后者能提高变换器的准确性。

```py
from sklearn.preprocessing import MinMaxScaler
import torch

with open('models_output.pkl', 'rb') as f:
    models_output = pickle.load(f)

def convert_coord(xy):
    r = np.sqrt((xy[0]-50)**2 + (xy[1]-50)**2)
    theta = np.arctan2(xy[1],xy[0])
    return [theta, r]

dataset = []
for k in range(len(models_output)):
    data = {}
    xy_site = []
    new_xy_dc = [convert_coord(i) for i in models_output[k]['xy_dc']]
    new_xy_cus = [convert_coord(i) for i in models_output[k]['xy_cus']]
    xy_site.extend(new_xy_dc)
    xy_site.extend(new_xy_cus)
    d_site = []
    d_site.extend([[i] for i in models_output[k]['m']])
    d_site.extend([[i] for i in models_output[k]['d']])
    f_site = []
    f_site.extend([[i] for i in models_output[k]['f']])
    f_site.extend([[0] for i in range(200)])
    i_site = []
    i_site.extend([[0] for i in range(100)])
    i_site.extend([[1] for i in range(200)])
    x = np.concatenate((xy_site, d_site), axis=1)
    x = np.concatenate((x, f_site), axis=1)
    x = np.concatenate((x, i_site), axis=1)
    if k == 0:
        scaler_x = MinMaxScaler()
        x = scaler_x.fit_transform(x)
    else:
        x = scaler_x.transform(x)
    x = np.expand_dims(x,axis=1)
    y = []
    y_cus = [2 for i in range(200)]
    y.extend(models_output[k]['y'])
    y.extend(y_cus)
    x = torch.from_numpy(x).float()
    y = torch.from_numpy(np.array(y)).long()
    data['x'] = x
    data['y'] = y
    dataset.append(data)
    mask = []
    mask_true = [True for i in range(100)]
    mask_false = [False for i in range(200)]
    mask.extend(mask_true)
    mask.extend(mask_false)
```

然后我们定义变换器的架构。这里的嵌入向量是256维的，多头注意力机制中的头数是8，编码器块的数量是3。我没有实验太多的超参数设置，因为这个设置已经达到了令人满意的准确性。可以进行更多的超参数调整，以进一步提高变换器的准确性。

```py
import torch
from torch import nn, Tensor
import torch.nn.functional as F
from torch.nn import TransformerEncoder, TransformerEncoderLayer

class TransformerModel(nn.Module):
    def __init__(self, n_class=2, d_input=5, d_model=256, nhead=8, d_hid=256, nlayers=3, dropout=0.5):
        super().__init__()
        encoder_layers = TransformerEncoderLayer(d_model, nhead, d_hid, dropout)
        self.transformer_encoder = TransformerEncoder(encoder_layers, nlayers)
        self.d_model = d_model
        self.encoder = nn.Linear(d_input, d_model)
        self.decoder = nn.Linear(d_model, n_class)

        self.init_weights()

    def init_weights(self) -> None:
        initrange = 0.1
        self.decoder.weight.data.uniform_(-initrange, initrange)

    def forward(self, src: Tensor) -> Tensor:
        src = self.encoder(src)
        output = self.transformer_encoder(src)
        output = self.decoder(output)
        return output
```

然后我们使用先前创建的数据集对变换器进行训练和测试。

```py
import torch.optim as optim

def acc(y_pred, y_test):
    y_pred_softmax = torch.log_softmax(y_pred, dim = 1)
    _, y_pred_tags = torch.max(y_pred_softmax, dim = 1)
    correct_pred = (y_pred_tags == y_test).float()
    acc = correct_pred.sum() / len(correct_pred)
    return acc

LEARNING_RATE = 0.0001
EPOCHS = 100
model = TransformerModel()
criterion = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=LEARNING_RATE)
n_class = 2

for e in range(1,EPOCHS+1):
    model.train()
    e_train_loss = []
    e_train_acc = []
    for i in range(560):
        optimizer.zero_grad()
        y_pred = model(dataset[i]['x'])
        train_loss = criterion(y_pred.view(-1,n_class)[mask], dataset[i]['y'][mask])
        train_acc = acc(y_pred.view(-1,n_class)[mask], dataset[i]['y'][mask])
        train_loss.backward()
        optimizer.step()
        e_train_loss.append(train_loss.item())
        e_train_acc.append(train_acc.item())
    with torch.no_grad():
        model.eval()
        e_val_loss = []
        e_val_acc = []
        for i in range(560,800):
            y_pred = model(dataset[i]['x'])
            val_loss = criterion(y_pred.view(-1,n_class)[mask], dataset[i]['y'][mask])
            val_acc = acc(y_pred.view(-1,n_class)[mask], dataset[i]['y'][mask])
            e_val_loss.append(val_loss.item())
            e_val_acc.append(val_acc.item())
    print('epoch:', e)
    print('train loss:', np.mean(e_train_loss))
    print('train acc:', np.mean(e_train_acc)) 
    print('val loss:', np.mean(e_val_loss))
    print('val acc:', np.mean(e_val_acc))

torch.save(model.state_dict(), desired_path)
```

在训练后，变压器在包含240个实例的测试集上达到了约92%的准确率。考虑到测试集没有不平衡（测试集中的标签0和1分别占46.57%和53.43%），并且我们的变压器并不是总是预测相同的类别，92%的准确率应该是一个令人满意的分数。

最后，我们测试了变压器辅助解决方案方法与仅使用SCIP求解器直接求解MILP的比较。主要目的是证明变压器有助于减少大量的求解时间，而不会显著恶化解决方案质量。

我创建了另外100个随机未见的实例，参数为*m_i, f_i, d_j*，这些实例遵循与用于训练变压器的实例相同的分布，并对每种解决方案方法进行应用。我测试了3种情况，不同的候选DC站点和客户数量，分别是（n_dc=100, n_cus=200）、（n_dc=200, n_cus=400）和（n_dc=400, n_cus=800）。代码如下。

```py
import time
from bisect import bisect

def resolve_infeasibility(m,f,d,y):
    idx = np.arange(len(y))
    idx = idx[y==0]
    m = m[idx]
    f = f[idx]
    r = f/m
    r_idx = np.argsort(r)
    m_sort = m[r_idx]
    idx = idx[r_idx]
    m_sort_sum = np.cumsum(m_sort)
    up_to_idx = bisect(m_sort_sum,d)
    idx = idx[:up_to_idx+1]
    for i in idx:
        y[i] = 1
    return y

transformer_model = TransformerModel()
transformer_model.load_state_dict(torch.load(desired_path))

obj_transformer = []
gap_transformer = []
time_transformer = []
obj_original = []
gap_original = []
time_original = []

rnd = np.random

for k in range(800,900):
    np.random.seed(k)
    n_dc = 100
    n_cus = 200
    c = []
    loc_x_dc = rnd.rand(n_dc)*100
    loc_y_dc = rnd.rand(n_dc)*100
    loc_x_cus = rnd.rand(n_cus)*100
    loc_y_cus = rnd.rand(n_cus)*100
    xy_dc = [[x,y] for x,y in zip(loc_x_dc,loc_y_dc)]
    xy_cus = [[x,y] for x,y in zip(loc_x_cus,loc_y_cus)]
    for i in xy_dc:
        c_i = []
        for j in xy_cus:
            c_i.append(np.sqrt((i[0]-j[0])**2 + (i[1]-j[1])**2)*0.01)
        c.append(c_i)    

    f= []
    m = []
    d = []

    for i in range(n_dc):
        f_rand = np.round(np.random.normal(100,15))
        if f_rand < 40:
            f.append(40)
        else:
            f.append(f_rand)
        m_rand = np.round(np.random.normal(70,10))
        if m_rand < 30:
            m.append(30)
        else:
            m.append(m_rand)
    for i in range(n_cus):
        d_rand = np.round(np.random.normal(20,5))
        if d_rand < 5:
            d.append(5)
        else:
            d.append(d_rand)

    f = np.array(f)
    m = np.array(m)
    d = np.array(d)
    c = np.array(c)

    start_time = time.time()
    model = Model("facility selection")
    x,y = {},{}
    x_names = ['x_'+str(i)+'_'+str(j) for i in range(n_dc) for j in range(n_cus)]
    y_names = ['y_'+str(i) for i in range(n_dc)]
    for i in range(n_dc):
        for j in range(n_cus):
            x[i,j] = model.addVar(name=x_names[i*n_cus+j])
    for i in range(n_dc):
        y[i] = model.addVar(name=y_names[i],vtype='BINARY')
    model.setObjective(quicksum(f[i]*y[i] for i in range(n_dc))+quicksum(c[i,j]*x[i,j] for i in range(n_dc) for j in range(n_cus)), 'minimize')
    for j in range(n_cus):
        model.addCons(quicksum(x[i,j] for i in range(n_dc)) == d[j])
    for i in range(n_dc):
        model.addCons(quicksum(x[i,j] for j in range(n_cus)) <= m[i]*y[i])
    model.optimize()
    time_original.append(time.time()-start_time)
    obj_original.append(model.getObjVal())
    gap_original.append(model.getGap())

    start_time = time.time()
    xy_site = []
    new_xy_dc = [convert_coord(i) for i in xy_dc]
    new_xy_cus = [convert_coord(i) for i in xy_cus]
    xy_site.extend(new_xy_dc)
    xy_site.extend(new_xy_cus)
    d_site = []
    d_site.extend([[i] for i in m])
    d_site.extend([[i] for i in d])
    f_site = []
    f_site.extend([[i] for i in f])
    f_site.extend([[0] for i in range(200)])
    i_site = []
    i_site.extend([[0] for i in range(100)])
    i_site.extend([[1] for i in range(200)])
    x = np.concatenate((xy_site, d_site), axis=1)
    x = np.concatenate((x, f_site), axis=1)
    x = np.concatenate((x, i_site), axis=1)
    x = scaler_x.transform(x)
    x = np.expand_dims(x,axis=1)
    x = torch.from_numpy(x).float()

    transformer_model.eval()
    y_pred = transformer_model(x)
    y_pred_softmax = torch.log_softmax(y_pred.view(-1,2)[mask], dim = 1)
    _, y_pred_tags = torch.max(y_pred_softmax, dim = 1)
    if np.sum(m*y_pred_tags.numpy()) < np.sum(d):
        y_pred_corrected = resolve_infeasibility(m,f,np.sum(d)-np.sum(m*y_pred_tags.numpy()),y_pred_tags.numpy())
    else:
        y_pred_corrected = y_pred_tags.numpy()

    model = Model("facility selection with transformer")
    x,y = {},{}
    x_names = ['x_'+str(i)+'_'+str(j) for i in range(n_dc) for j in range(n_cus)]
    y_names = ['y_'+str(i) for i in range(n_dc)]
    for i in range(n_dc):
        for j in range(n_cus):
            x[i,j] = model.addVar(name=x_names[i*n_cus+j])

    for i in range(n_dc):
        if y_pred_corrected[i] == 1:
            y[i] = model.addVar(name=y_names[i],vtype='BINARY',lb=1)
        else:
            y[i] = model.addVar(name=y_names[i],vtype='BINARY',ub=0)
    model.setObjective(quicksum(f[i]*y[i] for i in range(n_dc))+quicksum(c[i,j]*x[i,j] for i in range(n_dc) for j in range(n_cus)), 'minimize')
    for j in range(n_cus):
        model.addCons(quicksum(x[i,j] for i in range(n_dc)) == d[j])
    for i in range(n_dc):
        model.addCons(quicksum(x[i,j] for j in range(n_cus)) <= m[i]*y[i])
    model.optimize()
    time_transformer.append(time.time()-start_time)
    obj_transformer.append(model.getObjVal())
    gap_transformer.append(model.getGap())
```

每种解决方案方法在每种情况下的平均求解时间（单位：秒，在配备Intel(R) Core(TM) i7–10750H CPU，6核心的笔记本电脑上）和目标值报告在下表中。

![](../Images/7e7206c42444311c29c69e97b2353154.png)

从参数遵循与变压器训练集相同分布的实例中获得的测试结果（作者提供的图片）。

我们可以看到，在所有情况下，变压器辅助解决方案方法消耗的时间约为仅使用SCIP解决方案方法的2%，而目标值恶化仅约1%。

到目前为止，我们仅在参数遵循与训练集相同分布的实例上测试了变压器辅助的解决方案方法。如果我们将其应用于参数遵循不同分布的实例会怎么样？为了测试这一点，我生成了100个测试实例，参数为*m_i, f_i, d_j*，这些参数遵循双倍均值和双倍标准差的正态分布，使用以下代码。

```py
 f = []
m = []
d = []

for i in range(n_dc):
    f_rand = np.round(np.random.normal(200,30))
    if f_rand < 80:
        f.append(80)
    else:
        f.append(f_rand)
    m_rand = np.round(np.random.normal(140,20))
    if m_rand < 60:
        m.append(60)
    else:
        m.append(m_rand)
for i in range(n_cus):
    d_rand = np.round(np.random.normal(40,10))
    if d_rand < 10:
        d.append(10)
    else:
        d.append(d_rand)
```

从这些实例中获得的结果报告在下表中。

![](../Images/d6842de075bc800a3a9cc8bf3edba2c8.png)

从参数遵循双倍均值和标准差的分布的实例中获得的测试结果，与变压器的训练集相比（作者提供的图片）。

我们看到，变压器辅助的解决方案方法消耗的时间约为仅使用SCIP解决方案方法的2%，而目标值恶化约3%，比之前的测试稍高，但仍在接受范围内。

我们生成了另一组100个测试实例，参数为*m_i, f_i, d_j*，这些参数遵循半均值和标准差的正态分布，使用以下代码。

```py
f = []
m = []
d = []

for i in range(n_dc):
    f_rand = np.round(np.random.normal(50,7.5))
    if f_rand < 20:
        f.append(20)
    else:
        f.append(f_rand)
    m_rand = np.round(np.random.normal(35,5))
    if m_rand < 15:
        m.append(15)
    else:
        m.append(m_rand)
for i in range(n_cus):
    d_rand = np.round(np.random.normal(10,2.5))
    if d_rand < 3:
        d.append(3)
    else:
        d.append(d_rand)
```

结果报告在下表中。

![](../Images/bd3081cc3722f32bd78bc6d249f68531.png)

从参数分布的实例中获得的测试结果，其分布的均值和标准差为训练集的一半（图像来源：作者）。

我们看到，变压器辅助的解决方法仍然大大节省了解决时间，然而，解质量也有显著下降。因此，变压器确实有些过拟合于训练集中实例的参数分布。

# 结论

在这篇文章中，我训练了一个变压器模型，以帮助解决供应链领域的经典MILP——设施选址问题。通过学习SCIP提供的数百个实例的解决方案，该变压器能够预测整数变量应取的正确值。然后，我们将SCIP求解器中的整数变量固定为变压器模型提供的预测值，并解决其余变量。数值实验表明，当应用于参数遵循相同或类似分布的实例时，变压器辅助的解决方法显著减少了解决时间，同时仅有轻微的解质量下降。

将机器学习（ML）应用于加速MILP求解是ML和OR社区中的新兴研究主题，如[我的第一篇文章](https://medium.com/towards-data-science/some-thoughts-on-synergies-between-operations-research-and-machine-learning-921d78ed4bd5)中提到的。现有的研究思路包括监督学习（从求解器中学习）和强化学习（通过探索解空间和观察奖励来学习解决MILP本身）等。本文的核心思想属于前者（监督学习），并利用变压器模型中的注意力机制来学习为整数变量做决策。由于变压器模型从求解器中学习，变压器辅助的解决方法的解质量永远无法超越求解器，但我们获得了在求解时间上的巨大节省。

使用预训练的机器学习模型生成MILP的部分解法可能是加快大实例商业应用求解过程的一个有前景的想法。在这里，我选择了设施选址问题作为MILP的示例进行实验，但相同的想法适用于其他问题，如分配问题。此方法最适合用于我们希望重复解决具有相同或类似分布的参数的大型MILP实例的情况，因为预训练的机器学习模型往往会过拟合于训练实例中的参数分布。提高这种方法鲁棒性的一种方式可能是包括更多具有更广泛分布的实例，以减少过拟合，这可以成为我未来文章中的探索方向。

感谢阅读！
