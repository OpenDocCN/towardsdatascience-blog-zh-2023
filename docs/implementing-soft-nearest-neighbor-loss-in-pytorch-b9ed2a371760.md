# 在PyTorch中实现软最近邻损失

> 原文：[https://towardsdatascience.com/implementing-soft-nearest-neighbor-loss-in-pytorch-b9ed2a371760?source=collection_archive---------4-----------------------#2023-11-27](https://towardsdatascience.com/implementing-soft-nearest-neighbor-loss-in-pytorch-b9ed2a371760?source=collection_archive---------4-----------------------#2023-11-27)

## 数据集的类邻域可以通过软最近邻损失来学习

[](https://medium.com/@afagarap?source=post_page-----b9ed2a371760--------------------------------)[![Abien Fred Agarap](../Images/8f616478044e721a31c1c1df3d8e8b62.png)](https://medium.com/@afagarap?source=post_page-----b9ed2a371760--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b9ed2a371760--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b9ed2a371760--------------------------------) [Abien Fred Agarap](https://medium.com/@afagarap?source=post_page-----b9ed2a371760--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F782adfd45f71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-soft-nearest-neighbor-loss-in-pytorch-b9ed2a371760&user=Abien+Fred+Agarap&userId=782adfd45f71&source=post_page-782adfd45f71----b9ed2a371760---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b9ed2a371760--------------------------------) ·8分钟阅读·2023年11月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb9ed2a371760&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-soft-nearest-neighbor-loss-in-pytorch-b9ed2a371760&user=Abien+Fred+Agarap&userId=782adfd45f71&source=-----b9ed2a371760---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb9ed2a371760&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-soft-nearest-neighbor-loss-in-pytorch-b9ed2a371760&source=-----b9ed2a371760---------------------bookmark_footer-----------)

> 在本文中，我们讨论了如何实现软最近邻损失，我们也在[这里](https://medium.com/towards-data-science/improving-k-means-clustering-with-disentanglement-caf59a8c57bd)谈到过这个话题。

表示学习是通过深度神经网络学习给定数据集中最显著特征的任务。通常这是在监督学习范式中隐式完成的任务，并且是深度学习成功的关键因素 ([Krizhevsky et al., 2012](https://dl.acm.org/doi/abs/10.1145/3065386)；[He et al., 2016](https://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html)；[Simonyan et al., 2014](https://arxiv.org/abs/1409.1556))。换句话说，表示学习自动化了特征提取过程。通过这个过程，我们可以将学到的表示用于下游任务，如分类、回归和合成。

![](../Images/dffcba9ed212b05553d9160844edf567.png)

*图 1\. 来源于 SNNL (*[*Frosst et al., 2019*](http://proceedings.mlr.press/v97/frosst19a/frosst19a.pdf)*). 通过最小化软最近邻损失，类相似数据点（如其颜色所示）之间的距离被最小化，而类不同数据点之间的距离被最大化。*

我们也可以影响学习到的表示的形成，以适应特定的应用场景。在分类的情况下，表示会被调整使得同一类的数据点聚集在一起，而在生成（例如在 GAN 中）中，表示会被调整使得真实数据点与合成数据点聚集在一起。

同样，我们也享受了使用主成分分析（PCA）来编码特征以用于下游任务。然而，在 PCA 编码的表示中没有任何类或标签信息，因此在下游任务上的表现可能会进一步提升。我们可以通过学习数据集的邻域结构来改进编码的表示，即哪些特征被聚集在一起，这样的聚集会暗示这些特征属于同一类，如半监督学习文献中的聚类假设所示 ([Chapelle et al., 2009](https://ieeexplore.ieee.org/abstract/document/4787647))。

为了将邻域结构整合进表示中，已经引入了流形学习技术，如局部线性嵌入（LLE）([Roweis & Saul, 2000](https://www.science.org/doi/abs/10.1126/science.290.5500.2323)）、邻域组件分析（NCA）([Hinton et al., 2004](https://proceedings.neurips.cc/paper_files/paper/2004/file/42fe880812925e520249e808937738d2-Paper.pdf)）和t-随机邻域嵌入（t-SNE）([Maaten & Hinton, 2008](https://www.jmlr.org/papers/volume9/vandermaaten08a/vandermaaten08a.pdf?fbcl=))。

然而，上述流形学习技术各有其缺点。例如，LLE 和 NCA 编码的是线性嵌入，而非非线性嵌入。同时，t-SNE 嵌入的结构依赖于所使用的超参数。

为了避免这种缺陷，我们可以使用改进的NCA算法，即*软最近邻损失*或SNNL（[Salakhutdinov & Hinton, 2007](http://proceedings.mlr.press/v2/salakhutdinov07a/salakhutdinov07a.pdf); [Frosst et al., 2019](https://proceedings.mlr.press/v97/frosst19a.html)）。SNNL通过引入非线性改进了NCA算法，它是在神经网络的每一隐藏层上计算的，而不仅仅是最后的编码层。此损失函数用于优化数据集中点的*纠缠*。

在这种情况下，*纠缠*定义为类相似的数据点彼此之间的接近程度，相较于类不同的数据点。低纠缠意味着类相似的数据点彼此之间要比类不同的数据点更接近（见图1）。拥有这样的数据点集合将使下游任务更容易完成，且性能更佳。Frosst等人（2019）通过引入温度因子*T*扩展了SNNL目标。因此，我们得到以下最终损失函数，

![](../Images/a50f465990d5f1e33763426cb103a67c.png)

图2\. 软最近邻损失函数。图由作者提供。

其中*d*是原始输入特征或神经网络隐藏层表示上的距离度量，*T*是与隐藏层中数据点之间的距离直接成正比的温度因子。对于此实现，我们使用余弦距离作为我们的距离度量，以获得更稳定的计算。

![](../Images/b86cead71297c76297474ae763bb464a.png)

图3\. 余弦距离公式。图由作者提供。

本文的目的是帮助读者理解和实现软最近邻损失，因此我们将详细分析损失函数以更好地理解它。

## 距离度量

我们应该首先计算的是数据点之间的距离，这些距离可以是原始输入特征或网络的隐藏层表示。

![](../Images/e85ff680e27cec97a8863df628054780.png)

图4\. 计算SNNL的第一步是计算输入数据点的距离度量。图由作者提供。

对于我们的实现，我们使用余弦距离度量（图3）以获得更稳定的计算。暂时，我们忽略上图中标记的子集*ij*和*ik*，只专注于计算输入数据点之间的余弦距离。我们通过以下PyTorch代码实现：

```py
normalized_a = torch.nn.functional.normalize(features, dim=1, p=2)
normalized_b = torch.nn.functional.normalize(features, dim=1, p=2)
normalized_b = torch.conj(normalized_b).T
product = torch.matmul(normalized_a, normalized_b)
distance_matrix = torch.sub(torch.tensor(1.0), product)
```

在上面的代码片段中，我们首先在第1和第2行使用欧几里得范数对输入特征进行归一化。然后在第3行，我们获取归一化输入特征第二组的共轭转置。我们计算共轭转置以[考虑复数向量](https://math.stackexchange.com/questions/273527/cosine-similarity-between-complex-vectors)。在第4和第5行，我们计算输入特征的余弦相似度和距离。

具体来说，考虑以下特征集，

```py
tensor([[ 1.0999, -0.9438,  0.7996, -0.4247],
        [ 1.2150, -0.2953,  0.0417, -1.2913],
        [ 1.3218,  0.4214, -0.1541,  0.0961],
        [-0.7253,  1.1685, -0.1070,  1.3683]])
```

使用我们上面定义的距离度量，我们得到以下距离矩阵，

```py
tensor([[ 0.0000e+00,  2.8502e-01,  6.2687e-01,  1.7732e+00],
        [ 2.8502e-01,  0.0000e+00,  4.6293e-01,  1.8581e+00],
        [ 6.2687e-01,  4.6293e-01, -1.1921e-07,  1.1171e+00],
        [ 1.7732e+00,  1.8581e+00,  1.1171e+00, -1.1921e-07]])
```

## 采样概率

现在我们可以计算表示选择每个特征的概率的矩阵，给定其与所有其他特征的成对距离。这仅仅是选择*i*点的概率，基于*i*与*j*或*k*点之间的距离。

![](../Images/f8b096316279538b34805a0ceec0be1b.png)

图5。第二步是计算基于距离的采样概率。图由作者提供。

我们可以通过以下代码计算这一点：

```py
pairwise_distance_matrix = torch.exp(
    -(distance_matrix / temperature)
) - torch.eye(features.shape[0]).to(model.device)
```

代码首先计算距离矩阵的负指数除以温度因子，将值缩放到正值。温度因子决定如何控制给定点对之间距离的重要性，例如，在低温下，损失由小距离主导，而广泛分隔表示之间的实际距离变得不那么重要。

在减去`torch.eye(features.shape[0])`（即对角矩阵）之前，张量如下，

```py
tensor([[1.0000, 0.7520, 0.5343, 0.1698],
        [0.7520, 1.0000, 0.6294, 0.1560],
        [0.5343, 0.6294, 1.0000, 0.3272],
        [0.1698, 0.1560, 0.3272, 1.0000]])
```

我们从距离矩阵中减去一个对角矩阵，以去除所有自相似项（即每个点到自身的距离或相似度）。

接下来，我们可以通过以下代码计算每对数据点的采样概率：

```py
pick_probability = pairwise_distance_matrix / (
    torch.sum(pairwise_distance_matrix, 1).view(-1, 1)
    + stability_epsilon
)
```

## 掩码采样概率

到目前为止，我们计算的采样概率不包含任何标签信息。我们通过用数据集标签掩盖采样概率来整合标签信息。

![](../Images/82e4c0d5d90bba3e2035e5668cff0b57.png)

图6。我们利用标签信息来隔离属于同一类别的点的概率。图由作者提供。

首先，我们必须从标签向量中推导出一个成对矩阵：

```py
masking_matrix = torch.squeeze(
    torch.eq(labels, labels.unsqueeze(1)).float()
)
```

我们应用掩码矩阵，利用标签信息来隔离属于同一类别的点的概率：

```py
masked_pick_probability = pick_probability * masking_matrix
```

接下来，我们通过计算每行的掩码采样概率的总和来计算特定特征的总采样概率，

```py
summed_masked_pick_probability = torch.sum(masked_pick_probability, dim=1)
```

最后，我们可以计算采样概率总和的对数，为计算便利性添加额外的计算稳定变量，并求平均作为网络的最近邻损失，

```py
snnl = torch.mean(
    -torch.log(summed_masked_pick_probability + stability_epsilon
)
```

我们现在可以将这些组件组合在一起，在前向传递函数中计算整个深度神经网络的软最近邻损失，

```py
def forward(
    self,
    model: torch.nn.Module,
    features: torch.Tensor,
    labels: torch.Tensor,
    outputs: torch.Tensor,
    epoch: int,
) -> Tuple:
    if self.use_annealing:
        self.temperature = 1.0 / ((1.0 + epoch) ** 0.55)

    primary_loss = self.primary_criterion(
        outputs, features if self.unsupervised else labels
    )

    activations = self.compute_activations(model=model, features=features)

    layers_snnl = []
    for key, value in activations.items():
        value = value[:, : self.code_units]
        distance_matrix = self.pairwise_cosine_distance(features=value)
        pairwise_distance_matrix = self.normalize_distance_matrix(
            features=value, distance_matrix=distance_matrix
        )
        pick_probability = self.compute_sampling_probability(
            pairwise_distance_matrix
        )
        summed_masked_pick_probability = self.mask_sampling_probability(
            labels, pick_probability
        )
        snnl = torch.mean(
            -torch.log(self.stability_epsilon + summed_masked_pick_probability)
        )
        layers_snnl.append(snnl)

    snn_loss = torch.stack(layers_snnl).sum()

    train_loss = torch.add(primary_loss, torch.mul(self.factor, snn_loss))

    return train_loss, primary_loss, snn_loss
```

## 可视化解耦表示

我们使用软最近邻损失训练了一个自编码器，并可视化了其学习到的解缠表示。该自编码器包含 (x-500–500–2000-d-2000–500–500-x) 单元，并在 MNIST、Fashion-MNIST 和 EMNIST-Balanced 数据集的小型标注子集上进行训练。这是为了模拟标注样本的稀缺性，因为自编码器通常是无监督模型。

![](../Images/87e9b4cbadd1d80a4990a3e4a11f456f.png)

图 7. 3D 可视化比较了三个数据集的原始表示和解缠潜在表示。为了实现这种可视化，表示使用 t-SNE 编码，困惑度 = 50，学习率 = 10，优化 5000 次迭代。图由作者提供。

我们仅可视化了任意选择的 10 个簇，以便更简单、更清晰地展示 EMNIST-Balanced 数据集。从上图可以看出，潜在代码表示变得更适合聚类，通过集群分散度和正确的集群分配（由集群颜色指示）体现了良好的定义簇。

## 结束语

在本文中，我们详细分析了软最近邻损失函数，并讨论了如何在 PyTorch 中实现它。

软最近邻损失首次由 [Salakhutdinov & Hinton (2007)](http://proceedings.mlr.press/v2/salakhutdinov07a.html) 引入，用于计算自编码器潜在代码（瓶颈）表示上的损失，然后将该表示用于下游的 kNN 分类任务。

[Frosst, Papernot, & Hinton (2019)](https://proceedings.mlr.press/v97/frosst19a.html) 通过引入温度因子并计算神经网络所有层的损失，扩展了软最近邻损失。

最后，我们采用了退火温度因子来进一步改善网络的学习解缠表示，并加速解缠过程 ([Agarap & Azcarraga, 2020](https://arxiv.org/abs/2006.04535))。

完整的代码实现可在 [GitLab](https://gitlab.com/afagarap/pt-snnl) 上获得。

## 参考文献

+   Agarap, Abien Fred, 和 Arnulfo P. Azcarraga. “通过解缠内部表示来改善 k-means 聚类性能。” *2020 国际神经网络联合会议 (IJCNN)*. IEEE, 2020.

+   Chapelle, Olivier, Bernhard Scholkopf 和 Alexander Zien. “半监督学习 (chapelle, o. 等, 编；2006)[书评]。” *IEEE 神经网络交易* 20.3 (2009): 542–542.

+   Frosst, Nicholas, Nicolas Papernot 和 Geoffrey Hinton. “分析和改进软最近邻损失的表示。” *国际机器学习会议*. PMLR, 2019.

+   Goldberger, Jacob 等. “邻域组件分析。” *神经信息处理系统进展*. 2005.

+   He, Kaiming, 等. “用于图像识别的深度残差学习。” *IEEE 计算机视觉与模式识别会议论文集*。2016年。

+   Hinton, G., 等. “邻域组件分析。” *NIPS 会议论文集*。2004年。

+   Krizhevsky, Alex, Ilya Sutskever, 和 Geoffrey E. Hinton. “使用深度卷积神经网络进行 ImageNet 分类。” *神经信息处理系统进展* 25 (2012)。

+   Roweis, Sam T., 和 Lawrence K. Saul. “通过局部线性嵌入进行非线性维度约简。” *科学* 290.5500 (2000): 2323–2326。

+   Salakhutdinov, Ruslan, 和 Geoff Hinton. “通过保留类别邻域结构来学习非线性嵌入。” *人工智能与统计学*。2007年。

+   Simonyan, Karen, 和 Andrew Zisserman. “用于大规模图像识别的非常深的卷积网络。” *arXiv 预印本 arXiv:1409.1556* (2014)。

+   Van der Maaten, Laurens, 和 Geoffrey Hinton. “使用 t-SNE 进行数据可视化。” *机器学习研究杂志* 9.11 (2008)。
