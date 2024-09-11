# 理解深度学习优化器：动量、AdaGrad、RMSProp与Adam

> 原文：[https://towardsdatascience.com/understanding-deep-learning-optimizers-momentum-adagrad-rmsprop-adam-e311e377e9c2?source=collection_archive---------0-----------------------#2023-12-30](https://towardsdatascience.com/understanding-deep-learning-optimizers-momentum-adagrad-rmsprop-adam-e311e377e9c2?source=collection_archive---------0-----------------------#2023-12-30)

## 了解神经网络中的加速训练技术的直观感受

[](https://medium.com/@slavahead?source=post_page-----e311e377e9c2--------------------------------)[![Vyacheslav Efimov](../Images/db4b02e75d257063e8e9d3f1f75d9d6d.png)](https://medium.com/@slavahead?source=post_page-----e311e377e9c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e311e377e9c2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e311e377e9c2--------------------------------) [Vyacheslav Efimov](https://medium.com/@slavahead?source=post_page-----e311e377e9c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc8a0ca9d85d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-deep-learning-optimizers-momentum-adagrad-rmsprop-adam-e311e377e9c2&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=post_page-c8a0ca9d85d8----e311e377e9c2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e311e377e9c2--------------------------------) ·8 min read·Dec 30, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe311e377e9c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-deep-learning-optimizers-momentum-adagrad-rmsprop-adam-e311e377e9c2&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=-----e311e377e9c2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe311e377e9c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-deep-learning-optimizers-momentum-adagrad-rmsprop-adam-e311e377e9c2&source=-----e311e377e9c2---------------------bookmark_footer-----------)![](../Images/665f9f91827ad58ea605c3ef0704e2e8.png)

# 介绍

**深度学习**在人工智能领域迈出了巨大的一步。目前，神经网络在非表格数据（如图像、视频、音频等）上优于其他类型的算法。深度学习模型通常具有较强的复杂性，并且拥有数百万甚至数十亿个可训练的参数。因此，在现代时代，使用加速技术来减少训练时间是至关重要的。

在训练过程中最常见的算法之一是**反向传播**，它包括根据给定的损失函数调整神经网络的权重。反向传播通常通过**梯度下降**进行，该方法试图一步一步地将损失函数收敛到局部最小值。

事实证明，简单的梯度下降通常不是训练深度网络的首选，因为其收敛速度较慢。这激励了研究人员开发加速梯度下降的优化算法。

> 在阅读本文之前，强烈建议你了解**指数移动平均**的概念，它在优化算法中被使用。如果不了解，可以参考下面的文章。

[](/intuitive-explanation-of-exponential-moving-average-2eb9693ea4dc?source=post_page-----e311e377e9c2--------------------------------) [## 指数移动平均的直观解释

### 理解梯度下降中使用的基本算法的逻辑

[towardsdatascience.com](/intuitive-explanation-of-exponential-moving-average-2eb9693ea4dc?source=post_page-----e311e377e9c2--------------------------------)

# 梯度下降

梯度下降是最简单的优化算法，它计算损失函数相对于模型权重的梯度，并使用以下公式更新它们。

![](../Images/d32e845ebbb9169b66dfac2195651c04.png)

梯度下降方程。w 是权重向量，dw 是 w 的梯度，α 是学习率，t 是迭代次数。

为了理解为什么梯度下降收敛缓慢，让我们看下面的**峡谷**示例，其中一个包含两个变量的函数应该被最小化。

![](../Images/45fe2806f8f0dbaf9ec84762d1c2f2cf.png)

在峡谷区域中使用梯度下降的优化问题示例。起始点用蓝色表示，局部最小值用黑色表示。

> **峡谷**是一个在一个维度上表面比另一个维度更陡峭的区域。

从图像中可以看出，起始点和局部最小值具有不同的横坐标，并且几乎相等的纵坐标。使用梯度下降寻找局部最小值可能会导致损失函数沿垂直轴慢慢振荡。这些跳跃发生是因为梯度下降不存储任何关于先前梯度的历史，使得每次迭代的梯度步长更加不确定。这个例子可以推广到更高维度。

因此，使用较大的学习率是有风险的，因为这可能导致不收敛。

# 动量

基于上述示例，期望在水平方向上使损失函数执行较大的步长，在垂直方向上执行较小的步长。这样，收敛速度会更快。这正是动量所实现的效果。

动量在每次迭代时使用一对方程：

![](../Images/1deb4e4c81fb9ccb3870e1987c5e3597.png)

动量法公式

第一个公式使用指数移动平均来处理梯度值*dw*。基本上，这是为了存储有关一组先前梯度值的趋势信息。第二个公式在当前迭代中使用计算出的移动平均值执行正常的梯度下降更新。α是算法的学习率。

动量法特别适用于上述情况。假设我们在每次迭代中计算了梯度，如上图所示。我们不仅简单地使用这些梯度来更新权重，还取几个过去的值，并在平均方向上进行更新。

![](../Images/027d92c3a7351463a50ea5572741f48d.png)

> Sebastian Ruder 在他的 [论文](https://arxiv.org/pdf/1609.04747.pdf) 中简洁地描述了动量法的效果：“动量项对于梯度方向一致的维度增加，对于梯度方向改变的维度减少更新。因此，我们获得了更快的收敛速度和减少的振荡。”

因此，动量法执行的更新可能如下图所示。

![](../Images/fa9de8bee2c37be9dba9294007916497.png)

动量法优化

在实践中，动量法通常比梯度下降法收敛更快。使用动量法时，使用较大学习率的风险也较小，从而加速了训练过程。

> 在动量法中，建议选择接近0.9的β值。

# AdaGrad（自适应梯度算法）

AdaGrad 是另一种优化器，其动机是根据计算的梯度值调整学习率。在训练过程中，可能会出现权重向量的一个组件具有非常大的梯度值，而另一个组件具有极小的梯度值的情况。**这种情况尤其发生在不常见的模型参数对预测的影响较小时**。值得注意的是，对于频繁出现的参数，这类问题通常不会发生，因为模型在更新它们时会使用大量的预测信号。由于在梯度计算中考虑了大量信号的信息，梯度通常是适当的，并且代表了朝向局部最小值的正确方向。然而，对于稀有参数，这种情况并非如此，可能导致极大的不稳定梯度。相同的问题也可能出现在稀疏数据中，其中某些特征的信息过少。

AdaGrad 通过**为每个权重组件独立调整学习率**来解决上述问题。如果与某个权重向量组件相关的梯度较大，则相应的学习率会较小。相反，对于较小的梯度，学习率会较大。通过这种方式，Adagrad 处理了梯度消失和爆炸的问题。

在内部，Adagrad 累积所有先前迭代的梯度的元素平方 *dw²*。在权重更新期间，AdaGrad 不使用正常的学习率 α，而是通过将 α 除以累积梯度的平方根 *√vₜ* 来缩放它。此外，还向分母中添加了一个小的正数 ε，以防止潜在的除零错误。

![](../Images/8e0f659992ad29fb50e113b0daf3571d.png)

AdaGrad 方程

AdaGrad 的最大优点是不再需要手动调整学习率，因为它在训练过程中会自行适应。然而，AdaGrad 也有负面的一面：学习率随着迭代次数的增加不断衰减（学习率总是被一个正的累积数除）。因此，该算法在最后几次迭代中趋于慢收敛，因为学习率变得非常低。

![](../Images/b0fc1e109cdcd4288a12b6abe3d8f08e.png)

使用 AdaGrad 进行优化

# RMSProp（均方根传播）

RMSProp 被详细阐述为对 AdaGrad 的改进，解决了学习率衰减的问题。与 AdaGrad 类似，RMSProp 使用一对方程，其权重更新完全相同。

![](../Images/bdd2b8950a31350fc6aeb1330e1aaf2d.png)

RMSProp 方程

然而，与其为 vₜ 存储平方梯度的累积和 *dw²*，不如计算平方梯度 *dw²* 的指数移动平均。实验表明，由于指数移动平均，RMSProp 通常比 AdaGrad 收敛更快，因为它更重视最近的梯度值，而不是通过从第一次迭代开始简单地累积所有梯度来平均分配重要性。此外，与 AdaGrad 相比，RMSProp 的学习率并不总是随着迭代次数的增加而衰减，这使得它在特定情况下能够更好地适应。

![](../Images/9d0dd019424624f624dd0940c1a21e0a.png)

使用 RMSProp 进行优化

> 在 RMSProp 中，建议选择接近 1 的 β。

**为什么不简单地使用平方梯度 v**ₜ **而不是指数移动平均？**

已知指数移动平均将更高的权重分配给最近的梯度值。这是 RMSProp 快速适应的原因之一。但如果我们只考虑每次迭代的最后一个平方梯度（vₜ *= dw²*），而不是使用移动平均，是否会更好？事实证明，更新方程将变成以下形式：

![](../Images/921431c4faa6b96309222b853c12f650.png)

使用平方梯度代替指数移动平均时 RMSProp 方程的转换

如我们所见，结果公式与梯度下降中使用的公式非常相似。然而，我们现在使用的是梯度的符号，而不是正常的梯度值来进行更新：

+   如果 *dw > 0*，则权重 *w* 由 α 减少。

+   如果 *dw < 0*，则权重 *w* 由 α 增加。

总而言之，如果 vₜ *= dw²*，那么模型权重只能通过 ±α 来改变。尽管这种方法在某些情况下有效，但它的灵活性较差，算法对 α 的选择变得极其敏感，并且忽略了梯度的绝对大小，这可能导致方法收敛速度极其缓慢。该算法的一个积极方面是仅需一个位来存储梯度的符号，这在有严格内存要求的分布式计算中非常方便。

# Adam（自适应矩估计）

目前，Adam 是深度学习中最著名的优化算法。从高层次来看，Adam 结合了动量和 RMSProp 算法。为了实现这一点，它分别跟踪计算梯度和平方梯度的指数移动平均值。

![](../Images/36f3227cbddd28917346c2d951b40e27.png)

Adam 方程

此外，可以对移动平均进行偏差修正，以更精确地估算前几次迭代中的梯度趋势。实验表明，Adam 对几乎任何类型的神经网络架构适应良好，兼具动量和 RMSProp 的优点。

![](../Images/820a86fef66786198d018b7f3bd3b4de.png)

Adam优化

> 根据 [Adam 论文](https://arxiv.org/pdf/1412.6980.pdf)，超参数的良好默认值为 β₁ = 0.9，β₂ = 0.999，ε = 1e-8。

# 结论

我们已经研究了神经网络中的不同优化算法。Adam 被认为是动量（Momentum）和 RMSProp 的结合体，它在适应大规模数据集和深层网络方面表现最为出色。此外，它实现简单且内存需求少，使其在大多数情况下成为首选。

# 资源

+   [梯度下降优化算法概述](https://arxiv.org/pdf/1609.04747.pdf)

+   [Adam：一种随机优化方法](https://arxiv.org/pdf/1412.6980.pdf)

*除非另有说明，否则所有图片均由作者提供*
