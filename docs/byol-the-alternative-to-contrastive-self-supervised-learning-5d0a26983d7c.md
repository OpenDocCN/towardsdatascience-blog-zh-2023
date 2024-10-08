# BYOL —对比自监督学习的替代方法

> 原文：[`towardsdatascience.com/byol-the-alternative-to-contrastive-self-supervised-learning-5d0a26983d7c`](https://towardsdatascience.com/byol-the-alternative-to-contrastive-self-supervised-learning-5d0a26983d7c)

## [🚀Sascha 的论文俱乐部](https://towardsdatascience.com/tagged/saschas-paper-club)

## Bootstrap Your Own Latent: A New Approach to Self-Supervised Learning 由 J. Grill 等人

[](https://medium.com/@SaschaKirch?source=post_page-----5d0a26983d7c--------------------------------)![Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----5d0a26983d7c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d0a26983d7c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d0a26983d7c--------------------------------) [Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----5d0a26983d7c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d0a26983d7c--------------------------------) ·10 分钟阅读·2023 年 9 月 7 日

--

在今天的论文分析中，我们将深入探讨关于 BYOL (**B**ootstrap **Y**our **O**wn **L**atent) 的论文。它提供了一个对比自监督学习技术的替代方案，能够去除对大量负样本和庞大批量大小的需求。此外，它在理解今天最先进的基础模型（如 [DINO](https://arxiv.org/abs/2104.14294) 系列，包括 [DINOv2](https://arxiv.org/abs/2304.07193)）的道路上具有里程碑式的意义。

尽管对比自监督学习框架仍然有些直观，但 BYOL 起初可能会让人感到困惑和不安。因此，这是一个很好的论文来一起分析。让我们深入了解它，揭示其核心思想吧！

![](img/63946f35ab17c2db935b29bf3105ec08.png)

图片由 [Sascha Kirch](https://medium.com/@SaschaKirch) 创作，来源于 [出版物](https://arxiv.org/abs/2006.07733)

> **论文：** [Bootstrap your own latent: A new approach to self-supervised Learning](https://arxiv.org/abs/2006.07733) 由 Jean-Bastien Grill 等人，2020 年 6 月 13 日
> 
> **资源：** [GitHub](https://github.com/deepmind/deepmind-research/tree/master/byol)
> 
> **类别：** 相似性学习、表征学习、计算机视觉、基础模型
> 
> [**其他讲解**](https://medium.com/@SaschaKirch/list/paper-walkthroughs-by-sascha-kirch-89c7847da8e2)**:**
> 
> [CLIP] — [GLIP] — [Segment Anything] — [[Depth Anything](https://medium.com/towards-data-science/depth-anything-a-foundation-model-for-monocular-depth-estimation-8a7920b5c9cc?sk=fc6197edd68e6137c3396c83e50f65cb)] — [DINO] — [DDPM]

# 大纲

1.  背景与概述

1.  声称的贡献

1.  方法

1.  实验

1.  结论

1.  进一步阅读与资源

# 背景与概述

BYOL 属于通过相似性学习进行的自监督表示学习。自监督意味着没有提供明确的真实标签，但可以从未标记的数据中构建监督信号。表示学习意味着模型学习将输入编码到一个维度较低且语义丰富的表示空间中。最后，在相似性学习中，相似的特征在潜在表示空间中被映射得相互接近，而不相似的特征则被映射得更远。这些表示在许多深度学习任务中至关重要，这些任务利用这些表示来生成新数据、执行分类、分割或单目深度估计等。

许多成功的方法，如 CLIP、GLIP、[MoCo](https://arxiv.org/abs/1911.05722)或[SimCLR](https://arxiv.org/abs/2002.05709)使用了对比学习的方法。在对比学习中，最大化匹配数据对的得分，同时最小化不匹配数据的得分。这一过程严重依赖于训练期间提供的批量大小和负样本数量。这种依赖性使得数据收集和训练变得更加具有挑战性。

BYOL 的目标是：

1.  摆脱对对比学习所需的负样本和大批量大小的需求。

1.  减少对特定领域数据增强的依赖，使其适用于其他领域，如语言或图像。

在论文中提到的许多参考文献中，BYOL 突出了它与[平均教师](https://arxiv.org/abs/1703.01780)、[动量编码器](https://arxiv.org/abs/1911.05722)和[引导潜在预测 (PBL)](https://arxiv.org/abs/2004.14646)的相似性。

# 声称的贡献（根据作者的说法）

1.  BYOL（Bootstrap your own latent）的介绍，这是一种自监督表示学习方法，不需要负对（如对比学习中的）。

1.  BYOL 表示法被证明优于当时的最新技术水平（论文发布时）。

1.  与对比学习法相比，BYOL 显示出对批量大小和使用的图像增强方法更具弹性。

![Sascha Kirch](img/3edf0b4a499cde306202656453c7fe0a.png)

[Sascha Kirch](https://medium.com/@SaschaKirch?source=post_page-----5d0a26983d7c--------------------------------)

## Sascha Kirch 的论文解析

[查看列表](https://medium.com/@SaschaKirch/list/paper-walkthroughs-by-sascha-kirch-89c7847da8e2?source=post_page-----5d0a26983d7c--------------------------------)7 个故事！“DDPM — 去噪扩散概率模型” 论文插图，由 Sascha Kirch 绘制![“Depth Anything” 论文插图，由 Sascha Kirch 绘制](img/bd8cd71a02e42cf64d0afd39f41f48e0.png)![](img/8708d91a4a1902cef889ced95d46fc39.png)

# 方法

既然我们已经了解了 BYOL 声称要解决的问题，让我们尝试理解如何实现这一目标。首先，让我们观察图 1 中呈现的架构。

![](img/153409ea2efbcbf75ad2378f1063bab6.png)

图 1：框架架构。 [图像来源](https://arxiv.org/abs/2006.07733) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释。

BYOL 由两个网络组成：在线网络和目标网络。在线网络由三个子模块组成，即编码器、投影器和预测器。目标网络由两个子模块组成，即编码器和投影器。两个网络的编码器和预测器具有完全相同的架构，它们仅在模型权重上有所不同。在线网络在训练过程中进行优化，而目标网络则通过在线网络和自身的指数移动平均来更新其权重。

**编码器** —— 编码器由一个 ResNet 卷积神经网络组成。它将输入图像转换为潜在表示。

**投影器** —— 通过多层感知器网络（MLP）将潜在空间从 4096 维空间投影到 256 维空间。我猜测投影器对框架的工作并不关键，但 256 只是表示学习领域中常用的方便输出维度。

**预测器** —— 旨在从在线网络的投影潜在空间中预测目标网络的投影潜在空间。避免表示崩溃至关重要。

在训练过程中，对输入图像应用两个不同且随机选择的增强来构建该图像的两个不同视图。一个视图被输入到在线模型中，另一个视图被输入到目标模型中。这些增强包括但不限于：调整大小、翻转、裁剪、颜色扭曲、灰度转换、高斯模糊和饱和度。训练目标是最小化两个网络输出之间的平方 L2 距离。训练后，**最终只保留在线网络的编码器作为最终模型！**

就这样。简单，对吧？😜 好吧，看完论文后我的表情更像这样：😵 虽然将框架的处理过程分解为其关键组件相对直接，但获得直觉确实花费了我不少时间。

在我们尝试理解为什么 BYOL 实际有效之前，让我们首先简化呈现的方程并揭示它们的奥秘。

## 数学揭秘

在大致了解了 BYOL 的架构及其训练方式后，让我们仔细看看这些方程。我不得不说，论文中呈现的数学部分比实际需要的要复杂得多。在某些情况下，它的展示方式过于复杂，而在其他情况下，它在清晰度上滞后，留下了解释的空间，造成混淆。

我会专注于那些我认为理解发生了什么的方程。我们从精确倒序分析这些方程开始，为什么不呢？😜

首先，让我们谈谈训练期间模型参数的更新。回顾一下，我们有两个模型：在线模型和目标模型。在线模型通过使用 LARS 优化器优化损失函数来更新。

![](img/b3fdffacdfd667decdc3fe64eaba9260.png)

方程 1：在线网络的权重更新。[来源](https://arxiv.org/abs/2006.07733) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

上面的方程简单地说：“通过调用优化器函数对当前参数、这些参数相对于损失函数的梯度以及学习率 eta 进行更新，来更新模型的参数 theta”。

另一方面，目标模型不是通过优化来更新，而是通过从在线模型中复制权重，并对复制的更新权重和目标网络的当前权重应用指数移动平均：

![](img/9325b82871ba916524d9e0601e02b0ab.png)

方程 2：目标网络的权重更新。[来源](https://arxiv.org/abs/2006.07733) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

上面的方程简单地说：“通过计算当前权重 xi 和在线模型的更新权重的指数移动平均来更新模型的参数 xi”。Tau 遵循余弦调度，以减少整个训练过程中在线模型的贡献。

现在让我们看看用于更新在线模型的损失函数。它被定义为两个其他损失函数的和。这些损失函数具有相同的方程，稍后我们将看到，但在网络的两个不同输入上计算。回忆一下图 1\.，从图像 x 生成了两个不同的视角（即 v 和 v'），通过应用不同的增强。一个视角输入到在线模型中，另一个输入到目标模型中。在训练期间，计算损失之前执行两个前向传递，其中网络的输入会交换。输入到在线模型的图像会输入到目标模型中，反之亦然。

![](img/045f6f999186a4c829428be4c00eca0e.png)

方程 3：BYOL 的损失函数。 [来源](https://arxiv.org/abs/2006.07733) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

个体前向传递的损失是在线模型和目标模型的 L2 归一化输出的平方 L2 距离。让我们分解论文中的相应方程：

![](img/2e456b27a6689941529b2fb798f4c768.png)

方程 4：个体损失函数。 [来源](https://arxiv.org/abs/2006.07733) + [Sascha Kirch](https://medium.com/@SaschaKirch) 的注释

> 注：论文中提到这是均方误差，实际上并不正确。L2 距离没有除以其元素数量。我猜他们将其与计算所有批次的均值混淆了。

## BYOL 的直觉

现在，我们已经理解了框架和方程的核心信息，让我们尝试获得一些直觉。我会呈现作者的观点，然后我会尝试加入一些我自己的直觉，虽然知道这可能不准确 🤡。

***BYOL 如何学习其表示？*** — 模型被鼓励生成其两个输入的相同潜在表示，这两个输入代表了同一对象/场景的不同视角。无论图像是模糊的、黑白的还是翻转的，猫仍然是猫。事实上，我认为强大的数据增强在这里至关重要。它基本上告诉模型：“看，这些是同一事物的不同变体，所以忽略这些变体，当提取对象/场景的表示时，将它们视为相等！”。

***为什么表示没有崩溃？*** — 回顾之前我们提到，BYOL 属于相似性学习的范畴。网络不会最简单地将所有内容映射到潜在空间的同一点以实现最高相似度吗？实际上，这是相似性学习中的一个主要困难，称为“崩溃解决方案”。对比学习方法通过为每个匹配提供许多负样本来解决这个问题，以将相似特征映射到潜在空间中的彼此接近，同时将不相似的特征映射到更远的位置。BYOL 通过在在线网络和目标网络之间引入不对称性及其预测子模块，并通过基于指数移动平均的更新规则来解决这个问题，以确保在整个训练过程中预测器的近似最优。

[](https://medium.com/@SaschaKirch/subscribe?source=post_page-----5d0a26983d7c--------------------------------) [## 获取 Sascha Kirch 发布的新邮件通知 🚀

### 获取 Sascha Kirch 发布的新邮件通知 🚀 想要了解更多深度学习知识或保持最新动态…

medium.com](https://medium.com/@SaschaKirch/subscribe?source=post_page-----5d0a26983d7c--------------------------------)

# 实验

BYOL 的作者展示了实验和消融研究，以证明他们方法的有效性。

## *批量大小的消融研究*

从对比表示学习方法（例如，CLIP 和 GLIP）我们知道，在训练过程中，批量大小有很大的依赖性。例如，CLIP 是在批量大小为 32,768 的情况下训练的，这在考虑到它是一个多模态语言-图像模型时显得非常疯狂。

作者声称，由于 BYOL 不需要负样本，它对较低批量大小的敏感度较低，他们通过图 2 所示的实验进行了验证。

![](img/4bc8059a6403de2b0bca72cd8eaf9d4b.png)

图 2：批量大小的影响。[图片来源](https://arxiv.org/abs/2006.07733) + [Sascha Kirch](https://medium.com/@SaschaKirch)的注释。

可惜，这对于我的私人笔记本电脑来说可能仍然太大了 😅

## 图像增强的鲁棒性消融研究

[SimCLR](https://arxiv.org/abs/2002.05709)论文表明，对比视觉方法对图像增强的选择非常敏感，特别是那些影响颜色直方图的增强。虽然相同图像的裁剪部分共享类似的颜色直方图，但负样本对的裁剪部分则不然。模型在训练过程中可能会走捷径，专注于颜色直方图的差异，而不是语义特征。

作者们声称，BYOL 对图像增强选择的鲁棒性更强，因为在线和目标网络的更新方式。虽然这一假设得到了实验的支持，但仍然存在较强的依赖性，因此性能有所下降。

![](img/29ea63b28df3e850c03a04e04176a409.png)

图 3：对图像增强的鲁棒性。[图片来源](https://arxiv.org/abs/2006.07733) + [Sascha Kirch](https://medium.com/@SaschaKirch)的注释。

## 在 ImageNet 上的线性评估

在表示学习领域，一个重要的特征是模型将语义丰富的特征投射到潜在空间的能力，以便对相似特征进行聚类，并将不同的特征分开。一个常见的测试是冻结模型（对于 BYOL，只冻结在线模型的编码器），并在表示的基础上训练线性分类器。

BYOL 的线性评估已在 ImageNet 上进行，并与许多其他模型进行了比较，超越了当时的最新技术。

你会在许多论文中发现 ResNet-50 编码器与其他 ResNet 变体的区别。只是 ResNet-50 已经成为评估性能的标准网络。

![](img/16b12891474954baee3a1cb16206edaf.png)

表 1：在 ImageNet 上的线性评估。[来源](https://arxiv.org/abs/2006.07733)

## 半监督微调用于分类

表示学习中的另一个典型实验设置是模型在特定下游任务和数据集上进行微调时的表现。

表 2 展示了在分类任务上对 BYOL 进行微调时使用 1%或 10%的整个 ImageNet 训练集的指标。

![](img/770661dfb170a1d00aa5b21ca752decc.png)

表 2：在 ImageNet 上的半监督训练。[来源](https://arxiv.org/abs/2006.07733)

## 迁移到其他视觉任务

作者们还展示了他们在语义分割任务和单目深度估计任务（计算机视觉的两个重要领域）中迁移学习 BYOL 的实验。

与之前的方法相比，差异微乎其微，但我想这里的关键信息是，“我们有一种不同的方法，效果同样出色。”

![](img/a569a55cb7fd46bed3e371110e8cf386.png)

表 3：迁移到其他视觉任务。[来源](https://arxiv.org/abs/2006.07733)

# 结论

BYOL 提出了一种自监督表示学习的替代方法。通过实现两个进行相似性学习的网络，BYOL 可以在没有对比学习方法所需的负样本的情况下进行训练。为了避免崩溃解，目标网络通过 EMA 从在线网络中更新，并在在线网络之上构建了一个额外的预测子模块。

# 进一步阅读与资源

如果你已经读到这里：恭喜🎉并感谢😉！既然你似乎对这个话题很感兴趣，这里有一些进一步的资源：

以下是基于 BYOL 的论文列表：

1.  [DINO: 自监督视觉变换器中的新兴特性](https://arxiv.org/abs/2104.14294)

1.  [DINOv2: 无监督学习强健的视觉特征](https://arxiv.org/abs/2304.07193)

这里是我关于对比学习方法的两篇文章：CLIP 和 GLIP，用于自监督表示学习：

[](/the-clip-foundation-model-7770858b487d?source=post_page-----5d0a26983d7c--------------------------------) ## CLIP 基础模型

### 论文总结——从自然语言监督中学习可迁移的视觉模型

towardsdatascience.com [](/glip-introducing-language-image-pre-training-to-object-detection-5ddb601873aa?source=post_page-----5d0a26983d7c--------------------------------) ## GLIP: 将语言-图像预训练引入目标检测

### 论文总结：基础语言-图像预训练

towardsdatascience.com
