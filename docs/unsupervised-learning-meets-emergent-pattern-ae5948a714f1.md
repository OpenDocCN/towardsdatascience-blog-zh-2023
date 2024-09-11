# 无监督学习与涌现模式

> 原文：[https://towardsdatascience.com/unsupervised-learning-meets-emergent-pattern-ae5948a714f1?source=collection_archive---------16-----------------------#2023-04-26](https://towardsdatascience.com/unsupervised-learning-meets-emergent-pattern-ae5948a714f1?source=collection_archive---------16-----------------------#2023-04-26)

## 无监督学习如何帮助我们检测相变和涌现现象？

[](https://medium.com/@rodrigodamottacc?source=post_page-----ae5948a714f1--------------------------------)[![Rodrigo da Motta C. Carvalho](../Images/ed77f9c38a0a40b3d3ec2c119a350bbb.png)](https://medium.com/@rodrigodamottacc?source=post_page-----ae5948a714f1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae5948a714f1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ae5948a714f1--------------------------------) [Rodrigo da Motta C. Carvalho](https://medium.com/@rodrigodamottacc?source=post_page-----ae5948a714f1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd17b17427c47&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-meets-emergent-pattern-ae5948a714f1&user=Rodrigo+da+Motta+C.+Carvalho&userId=d17b17427c47&source=post_page-d17b17427c47----ae5948a714f1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae5948a714f1--------------------------------) · 6分钟阅读 · 2023年4月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fae5948a714f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-meets-emergent-pattern-ae5948a714f1&user=Rodrigo+da+Motta+C.+Carvalho&userId=d17b17427c47&source=-----ae5948a714f1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fae5948a714f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-meets-emergent-pattern-ae5948a714f1&source=-----ae5948a714f1---------------------bookmark_footer-----------)![](../Images/4ae945f423d5f995203c5532012d3518.png)

*William L. Stefanov* 摄影，发表于 [*NASA-JSC*](https://earthobservatory.nasa.gov/images/48076/cities-at-night-northern-china) *。（城市增长可以被理解为二阶相变和一种涌现现象）。*

本文灵感来源于 *Lei Wang [1]* 的论文 *Discovering Phase Transitions with Unsupervised Learning*。

## 介绍

自然科学中最有趣的现象之一是相变或临界现象，通常可以在物质状态的过渡中看到，如固体、液体和气体。然而，这一现象超越了常规，连接了大脑、铁磁材料片和种群动态等方面[2,3]。然而，这些相变的涌现模式并不总是容易检测。因此，可以使用无监督学习技术来帮助完成这一任务。在本文中，我将使用主成分分析（PCA）作为玩具模型进行相变识别，这是一种用于临界现象分析的最简单模型之一，也就是2D伊辛模型。它由威尔赫尔姆·伦茨于1920年引入，以简化方式描述铁磁材料。

2D伊辛模型相变的动态涌现现象。GIF由作者提供。

## 相变与涌现现象

当系统经历相变时，会展示由微观结构组件相互作用产生的各种复杂宏观结构模式，从而诞生出涌现的模式。兰道在1930年代对这些现象进行了最简单的描述，他引入了**φ**这一阶参数概念，它可以是标量、向量甚至张量。阶参数提供有关系统的相位及其之间过渡的信息。相变的阶次可以定义为：

## *一阶相变*

*在一阶相变中，阶参数* ***φ*** *会在相变过程中出现不连续跳跃。*

## *二阶相变*

*在二阶相变中，阶参数* ***φ*** *在相变过程中不会出现不连续跳跃；然而，它的导数会出现不连续跳跃。*

![](../Images/90f809b3467f5e45503f8feddd1deb22.png)

第一阶段和第二阶段相变中阶参数的区别

阶数。图像由作者提供。

## 最简单的相变模型之一

2D伊辛模型是描述铁磁材料的最简单模型之一，由一个自旋网格组成，自旋由离散变量描述，取两个值***σᵢ = -1, 1***，即自旋 *“上”* 和自旋 *“下”。* 这些自旋之间的相互作用由称为哈密顿量的函数给出，由系统的温度 ***T*** 控制，但我不会详细讨论。这个系统以在***临界******温度******Tc*** 处经历***二阶相变***而著名，具有非常简单的描述。

![](../Images/2cbcbb04ac2893f8f6144bed723f01c4.png)

2D伊辛模型的示意图，其中每个网格节点为自旋，蓝色表示

按“下”按钮和“上”按钮的红色。图像由作者提供。

所以，对于***T < Tc***，温度是亚临界的，此时自旋几乎全部对齐。对于***T > Tc***，在超临界温度下，自旋几乎都是随机分布的。临界点的模型，即相变，展示了非常有趣的时空模式，称为突现模式，这些模式具有多种影响并在不同领域中进行研究。

![](../Images/904a509b195e05f551601515dfb781ce.png)

2D Ising模型的模拟展示了不同温度下的空间模式。突现现象出现在临界温度下。图像由作者提供。

2D Ising模型的秩序参数是已知的，并由Onsager在1944年解析求解。解是简单的**φ = ∑ᵢ σᵢ**（即晶格中每个自旋值的总和），并且会通过无监督学习捕获，除了系统状态（即包含所有自旋值的矩阵）之外没有任何输入[1]。对于2D Ising模型，可以执行PCA，甚至可能导致秩序参数的识别。

## 未知的相变和无监督学习

相变或秩序参数并不总是容易找到，因此某些无监督学习和降维技术可以帮助找到相变和临界点。这些技术的一个优点是它们不假设临界点的存在或局部性。**主成分分析（PCA）**是一种简单且常用的降维技术，广泛应用于各种物理系统中来帮助解决这一问题。

PCA是一种通过主成分进行降维的技术，主成分是相互正交的方向，在这些方向上，数据方差随着接近第一个成分而单调增加。PCA通过简单的线性变换找到主成分，即***Y = XW***。正交变换作用在列向量上***W = (w₁,w₂,…,wₙ)***，其中***wₗ***表示配置空间中主成分的权重。它们由特征向量和特征值问题确定[1]。

![](../Images/a7cae657f5df90c8a5dba2ffb2cce077.png)

其中***XᵗX***等同于协方差矩阵。我不会详细解释，但这里确实发生了很有趣的事情。

通过仅保留第一个主成分，PCA是一种有效的降维方法，可以捕捉原始数据中的大部分线性变化，如下所示。可以看到，第一个成分表示数据扩展更广的轴。

![](../Images/f35d96026f988707f95c420b4d27f5c6.png)

这是一个二维数据的PCA组件的示例。图像由作者提供。

当应用于状态配置，即每个系统状态所在的多维空间时，PCA 会发现数据中最显著的线性变化。因此，该方法将对形成每个系统状态的基向量进行线性组合，以使方差单调增加。因此，在许多情况下，如果存在相变，则可以观察到它们。

## 寻找相变

对于二维伊辛模型，PCA 可以执行，甚至可以导致序参量的识别。

可以在不同温度下生成未关联的自旋配置，温度范围从 ***T/J = 1.6*** 到 ***2.9***，假设在此范围内存在临界温度，并将每次模拟整合成一个 ***M*x*N*** 矩阵，其中 ***M=900*** 是配置的总数，***N*** 是晶格上的自旋数量。因此，矩阵的第一行的所有元素，*(****S₁ⱼ****)*，将是模拟 1 在温度 ***T = 1.6*** 下的晶格自旋 ***σᵢ = -1,1***，而矩阵的第二行，(***S₂ⱼ***)，将是模拟 *2* 在温度 ***T = 1.62*** 下的晶格自旋 ***σᵢ = -1,1***。

![](../Images/7971ce6d1b252e72ccbf1052dcf258ef.png)

前两个 PCA 主成分，每个点都是模型的模拟

在二维伊辛模型在某一温度下的表现。可以看到超临界和亚临界状态的明显簇，而相变则分布在多个主成分上。图像由作者提供。

仅使用两个主成分，温度不同的簇的出现是显而易见的，超临界 ***T > Tc*** 和亚临界 ***T < Tc*** 情况下的簇特征明显，而接近相变的配置则分散。此时，其他监督算法可以分离这些簇。需要指出的是，使用其他主成分不会有显著贡献，因为模拟数据中的绝大多数方差都集中在前两个主成分中。

![](../Images/4399d469da0dab368a1f1ed2dd82eb4e.png)

PCA 组件的累计解释方差。第一个主成分包含几乎所有的方差。Y 轴为对数刻度。图像由作者提供。

现在可以看到，PCA的第一个主成分确实捕捉到了二维伊辛模型的序参量！因此，可以使用更先进的无监督学习方法来发现更复杂数据和模型的相变和涌现现象。

![](../Images/4da921b1bb7d588194dceae28d00dac1.png)

在二阶相变中，第一个主成分与序参量的比较。图像由作者提供。

## 结论

使用无监督学习和降维方法可以在不假设相变的位置和存在的情况下，甚至不需要系统状态数据之外的输入信息，检测模型的关键行为。这在没有解析解、具有复杂相变和涌现现象的更复杂模型中证明是非常有用的。

**本文的笔记本可在** [**此处**](https://github.com/Rodrigo-Motta/Unsupervised-Learning-meets-emergent-patters)**获取**。

## **参考文献**

[1] Lei Wang. (2016). [通过无监督学习发现相变](https://journals.aps.org/prb/pdf/10.1103/PhysRevB.94.195105). *物理评论B, 94(19),195105*. [https://doi.org/10.1103/PhysRevB.94.195105](https://doi.org/10.1103/PhysRevB.94.195105)

[2] Krkošek, M., & Drake, J. M. (2014). [关于鲑鱼种群动态中相变信号](https://pubmed.ncbi.nlm.nih.gov/24759855/). *生物科学会议录*, *281*(1784), 20133221\. [https://doi.org/10.1098/rspb.2013.3221](https://doi.org/10.1098/rspb.2013.3221)

[3] Steyn-Ross, M. L., Steyn-Ross, D. A., & Sleigh, J. W. (2004). [将全身麻醉建模为皮层中的一阶相变](https://pubmed.ncbi.nlm.nih.gov/15142753/). *生物物理学与分子生物学进展*, *85*(2–3), 369–385\. https://doi.org/10.1016/j.pbiomolbio.2004.02.001
