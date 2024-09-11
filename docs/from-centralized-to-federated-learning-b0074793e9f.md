# 从集中式学习到联邦学习

> 原文：[https://towardsdatascience.com/from-centralized-to-federated-learning-b0074793e9f?source=collection_archive---------9-----------------------#2023-03-16](https://towardsdatascience.com/from-centralized-to-federated-learning-b0074793e9f?source=collection_archive---------9-----------------------#2023-03-16)

## 关于在CIFAR基准数据集上进行联邦学习的数据集分布技术总结

[](https://medium.com/@neged.ng?source=post_page-----b0074793e9f--------------------------------)[![Gergely D. Németh](../Images/156b43b2d7be061e9795fb31d21f75a9.png)](https://medium.com/@neged.ng?source=post_page-----b0074793e9f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b0074793e9f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b0074793e9f--------------------------------) [Gergely D. Németh](https://medium.com/@neged.ng?source=post_page-----b0074793e9f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F71eefc6da84b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-centralized-to-federated-learning-b0074793e9f&user=Gergely+D.+N%C3%A9meth&userId=71eefc6da84b&source=post_page-71eefc6da84b----b0074793e9f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0074793e9f--------------------------------) ·9 min read·Mar 16, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb0074793e9f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-centralized-to-federated-learning-b0074793e9f&user=Gergely+D.+N%C3%A9meth&userId=71eefc6da84b&source=-----b0074793e9f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb0074793e9f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-centralized-to-federated-learning-b0074793e9f&source=-----b0074793e9f---------------------bookmark_footer-----------)

[**联邦学习** (FL)](https://federated.withgoogle.com/) 是一种在分布式环境下训练机器学习（ML）模型的方法 [1]。其理念是客户端（例如医院）希望在不共享其私密和敏感数据的情况下合作。每个客户端在FL中保留其私有数据，并在这些数据上训练ML模型。然后，中央服务器收集并聚合模型参数，从而基于所有数据分布的信息构建一个全球模型。理想情况下，这作为**设计上的隐私保护**。

已经进行了大量研究以理解FL的效率、隐私和公平性。在这里，我们将重点关注用于评估*水平FL方法*的基准数据集，其中客户端共享相同的任务和数据类型，但他们拥有各自的数据样本。

如果你想了解更多关于联邦学习及我所做的工作，请访问我们的[研究实验室网站](https://ellisalicante.org/federated-learning)!

![](../Images/71456a6c58d0c80ceca1a5b605996eab.png)

照片由[JJ Ying](https://unsplash.com/@jjying?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/photos/8bghKxNU1j0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

文献中有三种类型的数据集：

1.  **真实的FL场景**：FL是一种需要的方法的应用场景。它具有自然分布和敏感数据。然而，考虑到FL的性质，如果你想将数据保存在本地，你不会将数据集在线发布以进行基准测试。因此，很难找到这种类型的数据集。[OpenMined背后的PySyft试图组织一个FL社区](https://github.com/OpenMined/PySyft)，由大学和研究实验室主办数据，以更现实的场景进行数据托管。此外，最近隐私意识有所提高的应用场景也存在。因此，可能会有公开的数据，而FL的需求依然存在。一个应用场景是智能电表[2]。

1.  **FL基准数据集**：这些数据集设计用于作为FL基准。分布是现实的，但数据的敏感性存在疑问，因为它们是从公开来源构建的。例如，从Reddit帖子中创建FL数据集，使用用户作为客户端，并将其分发给一个用户作为一个分区。[LEAF项目](https://leaf.cmu.edu/)提出了更多类似的数据集[3]。

1.  **分发标准数据集**：有一些众所周知的数据集，如CIFAR和ImageNet，作为图像的例子，在许多机器学习工作中用作基准。在这里，FL科学家根据他们的研究问题定义一个分布。如果主题在标准机器学习场景中研究得很透彻，并且希望将其FL算法与集中式SOTA进行比较，那么使用这种方法是有意义的。然而，这种人工分布并没有揭示分布偏斜的所有问题。例如，客户端收集的图像可能来自不同的相机或不同的光照条件。

由于最后一种类别不是设计上分布的，过去的研究将其分为几种方式。在接下来的内容中，我将总结在联邦场景中用于CIFAR数据集的分发技术。

## CIFAR数据集

[CIFAR-10 和 CIFAR-100 数据集](https://www.cs.toronto.edu/~kriz/cifar.html) 包含 32x32 的彩色图像，并标记为互斥的类别 [4]。CIFAR-10 有 10 个类别，每个类别 6000 张图像，而 CIFAR-100 有 100 个类别，每个类别 600 张图像。它们在许多图像分类任务中使用，可以访问到基于这些数据集评估的数十个模型，甚至可以通过 [PapersWithCode 的排行榜](https://paperswithcode.com/sota/image-classification-on-cifar-100) 浏览它们。

# 联邦学习中的数据划分

## **均匀分布**

这被认为是 **同分布且独立**（IID）数据。数据点随机分配给客户端。

## 单类（n-）客户端

分配给特定客户端的数据点来自相同的类别或类别。这可以被认为是极端非独立同分布（non-IID）设置。这种分布的例子见 [1,5–8]。首先将这种设置命名为联邦学习 [1] 的工作使用了 200 个单类数据集，并给每个客户端两个数据集，使其成为 2 类客户端。[5–7] 使用 2 类客户端。

[9] 基于 CIFAR-100 中的层级类：客户端的数据点来自每个超级类中的一个子类。这样，在超级类的分类任务中，客户端拥有来自每个（超级）类的样本，但由于数据点来自不同的子类，因此模拟了分布偏斜。例如，一个客户端访问狮子的图像，而另一个客户端访问老虎的图像，超级类任务是将两者归类为大型食肉动物。

## 主导类别客户端

[5] 也使用了均匀和 2 类客户端的混合，这意味着一半的数据点来自 2 个主要类别，其余的数据点均匀地从其他类别中选择。[10] 使用 80%-20% 的划分，80% 来自单一主导类别，其余的从其他类别中均匀选择。

## Dirichlet 分布

要了解 [Dirichlet 分布](https://en.wikipedia.org/wiki/Dirichlet_distribution#)，我参考了 [这篇博客文章](https://blog.bogatron.net/blog/2014/02/02/visualizing-dirichlet-distributions/)。假设有人想要制作一个骰子，θ=(1/6,1/6,1/6,1/6,1/6,1/6) 表示每个数字 1–6 的概率。然而，实际上没有什么是完美的，所以每个骰子会有些偏斜。例如，4 出现的可能性稍高，而 3 的可能性稍低。Dirichlet 分布用参数向量 **α=(**α₁,α₂,..,α₆**)** 描述这种多样性。较大的 αᵢ 增强了该数字的权重，而 αᵢ 值的整体总和较大则确保了更相似的抽样概率（骰子）。回到骰子例子，为了让骰子公平，每个 αᵢ 应该相等，且 α 值越大，骰子的制造越好。由于这是 [beta 分布](https://en.wikipedia.org/wiki/Beta_distribution) 的多变量推广，让我们展示一些 beta 分布的例子（Dirichlet 分布与两个骰子）：

![](../Images/2de43c9da07b6edee8de41b34c90c01e.png)

不同的 beta 分布（2 变量的 Dirichlet 分布） — 图由作者提供

我重现了 [11] 中的可视化，使用相同的 α 值为每个 αᵢ。这被称为 **对称 Dirichlet 分布**。我们可以看到，随着 α 值的减少，更有可能出现不平衡的骰子。下面的图显示了不同 α 值的 Dirichlet 分布。这里每一行代表一个类别，每一列代表一个客户端，圆圈的面积与概率成正比。

![](../Images/9d7b720d17e1f23b3a3cc237b23b6215.png)

**类别分布**：使用不同的 Dirichlet 分布 α 值对 10 个类别的 20 个客户端进行采样 — 图由作者提供

**类别分布**：每个客户端的样本是独立抽取的，类别分布遵循 Dirichlet 方法。[11, 16] 使用这种 Dirichlet 分布版本。

![](../Images/5abdd77d6e8846582cf563fe4c4491b2.png)

**类别分布**：按类别（10）和客户端（20）归一化的样本总和 — 图由作者提供

每个客户端有一个预定数量的样本，但类别是随机选择的，因此最终的类别表示会不平衡。在客户端中，α→∞ 是先验（均匀）分布，而 α→0 意味着单类别客户端。

![](../Images/ea91e00df9dc8a2edbf286338e39aca6.png)

**客户端分布**：使用不同的 Dirichlet 分布 α 值对 10 个类别的 20 个客户端进行采样 — 图由作者提供

**客户端分布**：如果我们知道一个类别中的样本总数和客户端的数量，我们可以按类别将样本分配给客户端。这将导致客户端拥有不同数量的样本（在 FL 中非常典型），而全局类别分布则是平衡的。[12] 使用了这种 Dirichlet 分布的变体。

![](../Images/2e435888deb289b0978a04c2f25891d6.png)

**客户端分布**：按类别（10）和客户端（20）归一化的样本总和 — 图由作者提供

尽管像 [11–16] 这样的工作相互引用使用 Dirichlet 分布，但它们使用了两种不同的方法。此外，不同的实验使用了不同的 α 值，这可能导致非常不同的性能。[11,12] 使用 α=0.1，[13-15] 使用 α=0.5，[16] 概述了不同的 α 值。这些设计选择丧失了使用相同基准数据集来评估算法的原始原则。

**非对称 Dirichlet 分布**：可以使用不同的 αᵢ 值来模拟更具资源的客户端。例如，下面的图是使用 1/*i* 为第 *i* 个客户端生成的。据我所知，这在文献中没有表示，而是使用 Zipf 分布 [17]。

![](../Images/48c47aee44f15bc19ac9f6e15b330f56.png)

**非对称 Dirichlet 分布**，αᵢ=1/*i* — 图由作者提供

## Zipf 分布

[17] 使用了Zipf分布和Dirichlet分布的组合。它使用Zipf分布来确定每个客户端的样本数量，然后使用Dirichlet分布选择类别分布。

![](../Images/a2a495150ba551ab0bc5e9bec50c6d30.png)

Zipf分布中第*k*名的概率，其中是黎曼ζ函数

在Zipf（zeta）分布中，某个项目的频率与其在频率表中的排名成反比。[Zipf定律](https://en.wikipedia.org/wiki/Zipf%27s_law)可以在许多现实世界的数据集中观察到，例如语言语料库中的词频[18]。

![](../Images/5d486c9eef7e59b824ce03d2041af8b2.png)

使用Zipf分布抽样的项目 — 作者根据[numpy Zipf文档](https://numpy.org/doc/stable/reference/random/generated/numpy.random.zipf.html)绘制的图

# 结论

基准测试联邦学习方法是一项具有挑战性的任务。理想情况下，人们会使用预定义的真实联邦数据集。然而，如果某种场景必须在没有良好的现有数据集来覆盖的情况下进行模拟，可以使用数据分布技术。为可重复性和设计选择的动机提供适当的文档是重要的。在这里，我总结了用于FL算法评估的最常见方法。访问[这个Colab笔记本](https://colab.research.google.com/drive/1Bj14Quxo8afOTdAxP3s7aFdTK7RP9bGC?usp=sharing)获取用于此故事的代码！

# 参考文献

[1] McMahan, B., Moore, E., Ramage, D., Hampson, S., & y Arcas, B. A. (2017年4月). 从去中心化数据中高效学习深度网络. 载于*人工智能与统计学*（第1273–1282页）。PMLR。

[2] Savi, M., & Olivadese, F. (2021). 边缘端的短期能源消耗预测：一种联邦学习方法. *IEEE Access*, *9*, 95949–95969。

[3] Caldas, S., Duddu, S. M. K., Wu, P., Li, T., Konečný, J., McMahan, H. B., … & Talwalkar, A. (2019). Leaf: 联邦设置的基准. *联邦学习数据隐私与保密研讨会*

[4] Krizhevsky, A. (2009). 从微小图像中学习多层特征. *硕士论文, 特隆赫大学*。

[5] Liu, W., Chen, L., Chen, Y., & Zhang, W. (2020). 通过动量梯度下降加速联邦学习. *IEEE并行与分布式系统汇刊*, *31*(8), 1754–1766。

[6] Zhang, L., Luo, Y., Bai, Y., Du, B., & Duan, L. Y. (2021). 通过统一特征学习和优化目标对齐进行非独立同分布数据的联邦学习. 载于*IEEE/CVF国际计算机视觉会议论文集*（第4420–4428页）。

[7] Zhang, J., Guo, S., Ma, X., Wang, H., Xu, W., & Wu, F. (2021). 个性化联邦学习的参数化知识转移. *神经信息处理系统进展*, *34*, 10092–10104。

[8] Zhao, Y., Li, M., Lai, L., Suda, N., Civin, D., & Chandra, V. (2018). 带有非独立同分布数据的联邦学习. *arXiv预印本arXiv:1806.00582*。

[9] Li, D., & Wang, J. (2019)。Fedmd：通过模型蒸馏的异质联邦学习。*arXiv预印本 arXiv:1910.03581*。

[10] Wang, H., Kaplan, Z., Niu, D., & Li, B. (2020年7月)。使用强化学习优化非独立同分布数据上的联邦学习。载于*IEEE INFOCOM 2020-IEEE计算机通信会议*（第1698–1707页）。IEEE。

[11] Lin, T., Kong, L., Stich, S. U., & Jaggi, M. (2020)。用于联邦学习中强健模型融合的集成蒸馏。*神经信息处理系统进展*，*33*，2351–2363。

[12] Luo, M., Chen, F., Hu, D., Zhang, Y., Liang, J., & Feng, J. (2021)。不惧异质性：用于非独立同分布数据的联邦学习分类器校准。*神经信息处理系统进展*，*34*，5972–5984。

[13] Yurochkin, M., Agarwal, M., Ghosh, S., Greenewald, K., Hoang, N., & Khazaeni, Y. (2019年5月)。贝叶斯非参数化联邦学习的神经网络。载于*国际机器学习会议*（第7252–7261页）。PMLR。

[14] Wang, H., Yurochkin, M., Sun, Y., Papailiopoulos, D., & Khazaeni, Y. (2020) 使用匹配平均的联邦学习。载于*国际学习表征会议*。

[15] Li, Q., He, B., & Song, D. (2021)。模型对比的联邦学习。载于*IEEE/CVF计算机视觉与模式识别会议论文集*（第10713–10722页）。

[16] Hsu, T. M. H., Qi, H., & Brown, M. (2019)。测量非相同数据分布对联邦视觉分类的影响。*arXiv预印本 arXiv:1909.06335*。

[17] Wadu, M. M., Samarakoon, S., & Bennis, M. (2021)。在联邦学习中在频道不确定性下的联合客户端调度与资源分配。*IEEE通信交易*，*69*(9)，5962–5974。

[18] Fagan, Stephen; Gençay, Ramazan (2010)，“文本计量经济学导论”，载于Ullah, Aman; Giles, David E. A.（编），*实证经济学与金融手册*，CRC Press，第133–153页
