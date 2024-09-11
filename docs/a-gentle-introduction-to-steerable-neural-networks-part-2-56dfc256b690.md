# 《可操控神经网络简介（第2部分）》

> 原文：[https://towardsdatascience.com/a-gentle-introduction-to-steerable-neural-networks-part-2-56dfc256b690?source=collection_archive---------9-----------------------#2023-11-21](https://towardsdatascience.com/a-gentle-introduction-to-steerable-neural-networks-part-2-56dfc256b690?source=collection_archive---------9-----------------------#2023-11-21)

## 如何构建可操控滤波器和可操控CNN

[](https://medium.com/@mat.cip43?source=post_page-----56dfc256b690--------------------------------)[![Matteo Ciprian](../Images/61dab88069d99263e941a0e549473bdf.png)](https://medium.com/@mat.cip43?source=post_page-----56dfc256b690--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56dfc256b690--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----56dfc256b690--------------------------------) [Matteo Ciprian](https://medium.com/@mat.cip43?source=post_page-----56dfc256b690--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F975b976da56a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-steerable-neural-networks-part-2-56dfc256b690&user=Matteo+Ciprian&userId=975b976da56a&source=post_page-975b976da56a----56dfc256b690---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----56dfc256b690--------------------------------) · 10分钟阅读 · 2023年11月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F56dfc256b690&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-steerable-neural-networks-part-2-56dfc256b690&user=Matteo+Ciprian&userId=975b976da56a&source=-----56dfc256b690---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F56dfc256b690&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-steerable-neural-networks-part-2-56dfc256b690&source=-----56dfc256b690---------------------bookmark_footer-----------)

# 1) 介绍

本文是**《可操控神经网络简介》**教程的第二部分，也是最后一部分。它接续了第一部分的内容（[在这里](https://medium.com/@mat.cip43/a-gentle-introduction-to-steerable-neural-networks-part-1-32323d95b03f)）。

第一篇文章提供了Steerable神经网络（S-CNNs）的简明概述，解释了它们的目的和应用。它还深入探讨了基础形式主义和关键概念，包括等变性和Steerable滤波器的定义。尽管下一段落将对形式主义进行快速回顾，但我们建议您阅读第一篇文章以全面了解。

> *在本教程的最后部分，我们希望提供一个关于如何构建* ***Steerable Filter*** *的指南，并在最后，介绍如何组合一个Steerable神经网络。*

## 快速回顾术语：

![](../Images/0d083f156172448f1c8b6b6eb607045a.png)

图3A：按照形式主义表示的神经网络。

+   **S**：输入域空间。对象存在的空间（通常为ℝ³或ℝ²）。

+   ***f***ₙ****:*** 一个映射/函数，*f*ₙ:* S → ℝ ͨ ʿ*ⁿ* ʾ（Fₙ）*，*描述了NN的第n个特征映射。请注意，f⁰是描述输入（输入层）的函数，而对于n>0，fₙ描述了第n个特征映射。

+   **F***ₙ***= ℝ ͨ ʿ*ⁿ* ʾ**，它是描述*f*ₙ*的值域。

+   **Φ***ₙ***: F***ₙ****→ F*** *ₙ₊₁****,* 第*n* 个NN的滤波器可由核函数*k****ⁿ***: S →* ℝ ͨ ʿ*ⁿ* ʾ ˟ ͨ ʿ*ⁿ⁺ ¹* ʾ *来描述。卷积的定义如上第二个方程式所示。

+   **G**：变换的组（单一元素*g*）。

综合考虑所有这些概念，我们已经能够定义如下卷积：

![](../Images/a6e4eece3326f205c8b544d4ec5627b4.png)![](../Images/1fc9a17c9ca2c61643ca20e9da05e064.png)

# 2) Steerable CNN 滤波器的设计

![](../Images/b7c3c0dab47626529d9cbdd3d2a6c5ce.png)

图3A：等变CNN滤波器的视觉示例。给定对S的变换g及由Π₀(g)给定的输入信号f的旋转，f₁由Π₁(g)旋转。

## 2.1 问题的形式化

我们可以声明，如果对于每个*g在G,0*，当输入函数f₀变换为Π₀(g)时，那么第n层的输出函数将变换为Πn(g)，则n层的CNN对于一组变换G是等变的。

使这个陈述成立的一个充分条件是每个连续的层对其直接输入的变换具有等变性（见图3A）。网络的等变性是通过归纳来实现的。根据第二篇文章中给出的定义，如果滤波器Φ满足以下条件，则Φ是等变的：

![](../Images/d5c3b29e939626aaa18c1f6770b5e3f7.png)

方程0：等变性的定义

现在可以宣称steerable神经网络理论的**主要结果**。

> *设*k*连接层*f*ₙ和*f*的核心函数，使得*f*ₙ₊₁ = k* f ₙ*.*
> 
> *卷积*k* f ₙ*对于变换g是等变的，当且仅当：*

![](../Images/702fd59bbe89895caaf8f03ed705cb3a.png)

> *或更简单*

![](../Images/82a6e1a92a10f1b410b49216cc6989f4.png)

Eq.1: 关于变换g的核等变性的必要且充分条件。

在更广泛的文献中[2,3]，遵循此约束的核被称为g-可导核。由于核约束以线性方式操作，它生成的解构成了标准CNN中通常使用的无约束核的向量空间中的一个线性子空间。*经过更仔细的审查，这一定义与最后一篇文章第2段中介绍的可导滤波器的概念非常契合* [*这里*](https://medium.com/@mat.cip43/a-gentle-introduction-to-steerable-neural-networks-part-2-a17b4139aa5f)。在实际操作中，为了获得此工作，我们需要一个此核子空间的基，记作{k_1, …k_D}，它符合方程（1）。这个基的大小，记作D，可以计算为D = cʿⁿ ʾ ˟ cʿⁿ⁺¹ʾ。核*k(x)*随后通过这个基的线性组合得出，网络在过程中学习权重：

![](../Images/e8b0ba730141ddb8e89c464338a011ac.png)

Eq.2: 方程（1）的线性使得解等于以下线性组合。

在训练场景中，我们的方法涉及将输入层和输出层的大小设置为特定的值，即cʿⁿ ʾ和cʿⁿ⁺¹。然后，根据我们寻求等变的变换，解决方程并确定一个核基。随后，在训练过程中，我们学习与这些核相关的权重。

## 2.2 解方程

方程（1）中呈现的约束的解远非简单。它依赖于三个主要元素：

+   空间S，无论是S= ℝ³还是S= ℝ²。

+   群体 *G*。

+   层的输入输出维度：cʿ*ⁿ* ʾ 和 cʿ*ⁿ⁺ ¹* ʾ*。

更具体地说，我们可以说群G的选择定义了网络的类型。具体来说，我们主要对以下类型的网络感兴趣：

+   SO 网络：对特殊正交群（SO）中的旋转具有等变性。

+   SE 网络：对特殊欧几里得群（SE）中的旋转和平移具有等变性。

+   E 网络：对欧几里得群（E）中的旋转、平移和反射具有等变性。

如果我们在2D输入域中操作，我们有SO(2)，SE(2)和E(2)网络 [[4]](https://arxiv.org/abs/1911.08251)。相反，对于3D输入域，我们使用SO(3)，SE(3)和E(3)网络[[1](http://link)]，并且这可以扩展到任何E(n)空间 [[6]](https://openreview.net/pdf?id=WE4qe9xlnQw)。

将这项工作扩展到其他空间和对称性是一个持续的研究领域，感兴趣的读者可以调查被称为Hilbert空间和Green函数的数学研究领域，这里不在本文讨论范围之内。

> 然而，可以看到在 SE(n) 网络的情况下，方程式1的一般解是S=ℝ*ⁿ*中的一个谐波基函数。在上面的图像中（图3B），可以看到ℝ²左侧的谐波函数和ℝ³中的谐波函数。

![](../Images/643be215a340fa4459acfae5cf9f293d.png)

图 3B：二维（左）和三维（右）中的谐波函数基。这些基分别构成 SE(2) 和 SE(3) 网络中可操控等变滤波器的基。

考虑一个更具滤波器设计场景的情况，在下图 Fig 3C 中，我们可以看到如何为输入层 *f ₀*: ℝ²->ℝ³ 和输出层 *f₁*: ℝ²->ℝ² 构建一个 SO2 可操控等变核。

核是一个函数 *k*: ℝ²->ℝ³ˣ²。矩阵的每个单独元素是通过对在位置*(x₁,x₂)*采样的D基的线性加权组合得到的函数。我们可以查看上面的示例位置 *x=(1,2)*。

接下来，我们将展示一些这个方程的简单解，考虑 S=ℝ² 和 G 作为旋转变换的群体，包含 SO2 网络。

![](../Images/3ae0f08c22bf149cea91fecb975c2883.png)

图 3C：使用 6 个谐波函数的基构建的 3x2 可操控核的可视化表示。

## 2.3 实际解决方案

## - Case1A: SO2 网络，k: S=ℝ² → ℝ

假设实际情况下输入为灰度图像，我们想要构建一个可操控滤波器来处理它。首先，我们必须决定输出层的维度（特征数量）。为了简便起见，假设维度为 1。

在这个设置中，我们有一个输入函数 *f*: ℝ²-> ℝ 和一个类似的输出函数 *f₁*: ℝ²-> ℝ。因此，核函数是 *k*: ℝ² -> ℝ。我们希望我们的 CNN 层对一个变换群体 G 是等变的，G 代表了角度 theta 在 [0,2π) 范围内的旋转（SO 网络）。对于这个问题，核函数的基需要使用方程式1。由于 f 和 f¹ 都是标量，Pi_out = 1 和 Pi_in = 1。结果是 *k[****g_****θ(x)] = k[x]*，如方程式3中所写。

如果 *x = (x₁, x₂)* 在 ℝ² 中，g(theta) 与 2D 欧拉矩阵对齐。

![](../Images/23227fb2d9498574821a1a0809d776c2.png)

方程式3：在 k: S=ℝ² → ℝ 的情况下重写方程式（1）

很容易看出，这可以通过在(*x₁, x₂*)中的每个各向同性函数来解决。具体来说，这可以通过一维的各向同性（旋转不变）核来解决。（即 *k(x₁, x₂)* = *x₁*² + *x₂*²）

## Case 2: SO2 滤波器，k: ℝ² → ℝ²

现在考虑一个更复杂的情况。输入函数为*f:* ℝ² → ℝ²，输出层为函数*f ₁:* ℝ² → ℝ²。因此，内核可以写为函数*k: S=* ℝ² *→* ℝ² ˣ ²; 换句话说，对于ℝ²中的每个位置x，我们有一个二维矩阵2x2（见下方方程）。我们想要构建S02滤波器，因此需要考虑的变换群再次是G={g(*θ*)} ={r(*θ*)}，*θ* ∈ *[0,2Π[*. 由于ℝ²是*f*和*f ₁*的值域，Π_out=Π_θ* 和 *Π_in=Π_θ*，其中Π_θ是ℝ²中的欧拉矩阵。考虑到所有这些条件，我们可以以下述方式重写Eq.1：

![](../Images/9c16aa56e4b18daebd5ae7451eff30cc.png)

Eq.(4): 考虑到的内核函数*k: S=* ℝ² *→* ℝ² ˣ ²

![](../Images/234b4cbcbdd561e26738e4c75aac7564.png)

Eq.(5): 为SO2内核k重写Eq.(1): S=ℝ² → ℝ²。

欲更全面地理解该方程的解及更多见解，请参考论文[4]中的附录部分。

## 2.4 网络非线性

到目前为止，我们仅考虑了相对于卷积操作的等变性，而没有考虑由函数σ*(f(x)):* **ℝ=**ℝ ͨ→ℝ ͨ’给出的非线性部分。论文[1]的第4.3节和论文[4]的第2.6节对此进行了广泛讨论。

给定函数f(x)，等变性条件可以总结如下：

![](../Images/af2dadd3f7be62964669f540ae651f03.png)

Eq.(5): 激活函数的等变性条件。

如相关的YouTube讲座中所提到的[这里](https://www.youtube.com/watch?v=b8K6adf_zY0)，可以通过利用所谓的基于范数的激活函数，如**σ*(u) =* σ*(||u||)***，来创建满足该标准的激活函数。其动机在于标量范数是透明不变的，因此对其应用任何非线性函数将产生不变的输出。为了证明这一点，当我们将此公式应用于上述条件时，会得到以下方程：

![](../Images/16f45464bf676098d1c8a548920ba2a2.png)

Eq.(6): 将Eq.(5)重写为基于范数的函数。

如果‘g’属于E变换群，则范数保持不变。因此，当Π’(g)等于单位矩阵时，该方程在普遍情况下是有效的。这意味着特别设计的激活函数始终具有旋转不变性。例如，*Norm-ReLUs*，其定义为*η(|f(x)|) = ReLU(|f(x)| − b)*

研究论文和讲座中提出了额外的非线性激活函数，如非门控激活函数。我们建议读者查阅这些来源以获取进一步的解释。

# **3) 设计一个可操控的CNN**

![](../Images/4c4ab19a773efc0fc05e8dda9e506ba6.png)

图3D: 可操控CNN的架构，如[[3]](https://arxiv.org/abs/1711.07289)中所述。注意第2层中使用的可操控滤波器与G卷积的结合。

在上一节中，我们掌握了构建单个可引导滤波器的基础知识。在本节中，我们将深入探讨如何将这些滤波器有效地整合以建立一个功能全面的可引导神经网络。

在上图中，我们可以看到一篇论文中的示例[[3]](https://arxiv.org/abs/1711.07289)。我们特别关注第2层，其中使用了可引导滤波器。

在这里，每个水平表示都是一个可引导滤波器——由加权谐波函数组成——它产生一个不同的输出，表示为单个 fⁿ。观察其结构，很明显虽然谐波函数在各个滤波器中保持一致，但它们的方向在每个滤波器之间有所变化。这是 G-卷积技术的一个典型特征，这是一种复杂的方法，有助于构建对变换不变的网络（你可以在[这里](https://chat.openai.com/c/64d8f147-a86f-497f-be18-2e09cb13da96#)找到更多关于该技术的信息）。该网络利用最大池化的强大功能，将来自可引导滤波器阵列中的最强响应传递到下一层。这种选择性传输的原则确保了最强的特征在网络中传递和增强。这种方法与其他工作中实现的方法类似，例如参考文献[5]成功构建了一个尺度不变的可引导网络。这种可引导 CNN 的架构受益于这种技术，因为它自然地结合了尺度和旋转不变性，从而增强了网络以更抽象但更强大的方式识别模式和特征的能力。无论如何，从图片中可以看出，最终结果是一个对旋转不变的网络。

![](../Images/206e02a4aa42a18b1aba73b7faab5f60.png)

图 3E：在旋转图像上应用2D可引导滤波器的视觉示例（原始图像可以在[这里](https://github.com/QUVA-Lab/e2cnn)找到）

关于可引导神经网络设计的优秀逐步解释可以在此[链接](https://github.com/QUVA-Lab/e2cnn/blob/master/examples/introduction.ipynb)中找到，该链接包含在Github库*“e2cn”*（*[link](https://github.com/QUVA-Lab/e2cnn)）。在该库中，可以找到设计 SE2 可引导网络的 PyTorch 代码。关于 SE3 网络的有用代码可以在此[链接](https://github.com/mariogeiger/se3cnn)中找到，而关于3D 等变网络的快速课程已在[这里](http://www.ipam.ucla.edu/abstract/?tid=16346&pcode=MLPWS1)发布。

## 文献：

[1] “3D Steerable CNNs: Learning Rotationally Equivariant Features in Volumetric Data”，Weilier et al.，([link](https://arxiv.org/abs/1807.02547));

[2] “Steerable CNNs”，Cohen et al. [( link](https://arxiv.org/abs/1612.08498));

[3] “学习旋转等变CNN的可调节滤波器”，Weilier等人 ([link](https://arxiv.org/abs/1711.07289))

[4] “通用E(2)-等变可调节CNN”，Weilier等人 ([link](https://arxiv.org/abs/1911.08251))

[5] “适用于局部尺度不变卷积神经网络的尺度可调节滤波器”，Ghosh等人 ([link](https://www.researchgate.net/publication/334480982_Scale_Steerable_Filters_for_the_Locally_Scale-Invariant_Convolutional_Neural_Network?enrichId=rgreq-7fc8b3654779eb94d36221a6e5fab2ff-XXX&enrichSource=Y292ZXJQYWdlOzMzNDQ4MDk4MjtBUzo3ODEyNTQ4ODI1MDg4MDBAMTU2MzI3NzA4NzY1NA%25253D%25253D&el=1_x_3&_esc=publicationCoverPdf))

[6] “构建E(n)-等变可调节CNN的程序。” Cesa等人 ([link](https://openreview.net/pdf?id=WE4qe9xlnQw))

# ✍️ 📄. 关于作者：

***1️⃣ Matteo Ciprian***，机器学习工程师/研究员

+   硕士学位，电信工程，帕多瓦大学。当前在传感器融合、信号处理和应用人工智能领域工作。参与与人工智能在电子健康和可穿戴技术中的应用相关的项目（学术研究和企业领域）。专注于开发异常检测算法，以及推进深度学习和传感器融合技术。

    对哲学充满热情。YouTube 内容创作者。

    **🔗 链接：** 💼 [Linkedin](https://www.linkedin.com/in/matteo-ciprian-ba30ab122/)

    📹 [Youtube](https://www.youtube.com/channel/UCF--7G3kkCmEsdPLm8wyPow)

    👨‍💻 [Instagram](https://www.instagram.com/cip_mat/)

2️⃣ ***Robert Schoonmaker***，信号处理/机器学习研究员

+   博士学位，计算凝聚态物理，杜伦大学。专注于应用机器学习和非线性统计学，目前正在研究GPU计算方法在合成孔径雷达及类似系统中的应用。经验包括开发用于传感器融合和定位技术的对称机器学习方法。

    **🔗 链接：** 💼 [Linkedin](https://www.linkedin.com/in/robert-schoonmaker-951221b/)
