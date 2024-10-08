# DETR（用于目标检测的变换器）

> 原文：[`towardsdatascience.com/detr-transformers-for-object-detection-a8b3327b737a`](https://towardsdatascience.com/detr-transformers-for-object-detection-a8b3327b737a)

## 对论文“基于变换器的端到端检测”的深入剖析和清晰解释

[](https://medium.com/@francoisporcher?source=post_page-----a8b3327b737a--------------------------------)![François Porcher](https://medium.com/@francoisporcher?source=post_page-----a8b3327b737a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a8b3327b737a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a8b3327b737a--------------------------------) [François Porcher](https://medium.com/@francoisporcher?source=post_page-----a8b3327b737a--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a8b3327b737a--------------------------------) ·阅读时长 8 分钟·2023 年 10 月 7 日

--

![](img/dae11e25d74f1483e61af5a29e11a35f.png)

[Aditya Vyas](https://unsplash.com/@aditya1702?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

注：本文深入探讨了计算机视觉的复杂世界，特别是变换器和注意力机制。建议了解论文[“Attention is All You Need.”](https://arxiv.org/abs/1706.03762)中的关键概念。

# 历史快照

**DETR**，即**DE**tection **TR**ansformer，开创了一种新的目标检测方式，由**Nicolas Carion 及其团队于 2020 年在 Facebook AI Research 提出**。

尽管目前尚未达到 SOTA（最先进技术）水平，DETR 对目标检测任务的创新重新定义显著影响了随后的模型，例如 CO-DETR，它是[LVIS](https://www.lvisdataset.org)上**目标检测**和**实例分割**的当前最先进技术。

与传统的多对一问题场景不同，其中每个真实值对应多个锚点候选，DETR 通过将目标检测视为**集合预测问题**，实现了预测与真实值之间的一对一对应，从而消除了对某些后处理技术的需求。

# 目标检测概述

目标检测是计算机视觉的一个领域，专注于识别和定位图像或视频帧中的对象。除了仅仅对对象进行分类外，它还提供一个边界框，指示对象在图像中的位置，从而使系统能够理解各种识别对象的空间上下文和位置。

![](img/882459fab2d3469f3648a695ae2aab34.png)

Yolo5 视频分割，[来源](https://commons.wikimedia.org/wiki/File:Yolo5.gif)

物体检测本身非常有用，例如在自动驾驶中，但它也是**实例分割**的前置任务，在实例分割中我们尝试寻找物体的更精确轮廓，同时能够区分不同实例（与语义分割不同）。

# 揭开非极大值抑制的神秘面纱（并摆脱它）

![](img/1b9c9b633277fbe1263194dea79f1675.png)

非极大值抑制，作者提供的图像

**非极大值抑制 (NMS)** 长期以来一直是物体检测算法中的基石，在后处理过程中发挥了不可或缺的作用，以精细化预测输出。在传统的物体检测框架中，模型提出了大量围绕潜在物体区域的边界框，其中一些不可避免地显示出显著的重叠（如上图所示）。

NMS 通过**保留具有最大预测对象性分数的边界框**，同时抑制那些表现出高重叠度的相邻框来解决这一问题，重叠度通过交并比 (IoU) 量化。具体来说，给定预设的 IoU 阈值，NMS 迭代选择具有最高置信度分数的边界框，并取消那些 IoU 超过该阈值的边界框，**确保每个对象只有一个高置信度的预测**。

尽管 DETR（DEtection TRansformer）无处不在，但它大胆地避开了传统的 NMS，通过**将物体检测重新定义为集合预测问题**来重新发明物体检测。

通过利用变换器，DETR 直接预测固定大小的边界框，从而消除了传统 NMS 的必要性，显著**简化了物体检测管道，同时保持甚至提升了模型性能**。

# 深入探讨 DETR 架构

在高层次图像中，DETR 是

+   **图像编码器**（实际上是一个双重图像编码器，因为首先是一个**CNN 骨干网络**，然后是一个**变换器** **编码器**，以增加更多的表达能力）

+   **变换器解码器** 从图像编码中生成边界框。

![](img/b34f2bb86d4d6e488a9661e3a9571e11.png)

DETR 架构，图片来自 [文章](https://arxiv.org/abs/2005.12872)

让我们详细了解每个部分：

1.  **骨干网络：**

我们从一个具有 3 个颜色通道的初始图像开始：

这张图像被输入到一个骨干网络中，该骨干网络是一个卷积神经网络

我们使用的典型值是 *C = 2048* 和 *H = W = H0 = W0 = 32*

**2\. 变换器编码器：**

变换器编码器在理论上并非强制要求，但它为骨干网络增加了更多表达能力，消融研究表明性能有所提升。

首先，1x1 卷积将高层激活图 *f* 的通道维度从 *C* 减少到较小的维度 *d*。

卷积之后

但正如你所知，Transformers 将输入向量序列映射到输出向量序列，所以我们需要压缩空间维度：

在压缩空间维度后

现在我们准备将其输入到 Transformer 编码器中。

重要的是要注意**Transformer 编码器仅使用自注意力机制。**

就是这样**图像编码部分**！

![](img/46dc94deac1f578f41c40d103aaca1e9.png)

解码器的更多细节，图像来自[文章](https://arxiv.org/abs/2005.12872)

**3\. Transformer 解码器：**

这一部分是最难理解的部分，耐心点，如果你理解了这部分，你就理解了文章的大部分内容。

解码器使用自注意力和交叉注意力机制的组合。它接收*N*个对象查询，每个查询将被转换为一个输出框和类别。

框预测是什么样的？

它实际上由两个组件组成。

+   一个边界框具有一些坐标（x1，y1，x2，y2）来识别边界框。

+   一类（例如海鸥，但也可以为空）

重要的是要注意*N*是固定的。这意味着 DETR 始终预测**N**个边界框。但其中一些可能为空。我们只需确保*N*足够大，以覆盖图像中的足够对象。

然后交叉注意力机制可以关注编码部分（骨干网络 + Transformer 编码器）生成的图像特征。

如果你对机制不确定，这个方案应该能澄清问题：

![](img/71798b7319d8ae8879d3f90251249028.png)

详细的注意力机制，图像来自[文章](https://arxiv.org/abs/2005.12872)

在原始 Transformer 架构中，我们生成一个 token，然后使用自注意力和交叉注意力的组合来生成下一个 token 并重复。但在这里，我们不需要这种递归的公式，我们可以一次性生成所有输出，从而利用并行性。

# 主要创新：二分匹配损失

如前所述，DETR 产生**N**个输出（框 + 类）。但每个输出只对应一个真实框。如果你记得清楚，这就是关键点，我们不想应用任何后处理来过滤重叠的框。

![](img/96b2fc4578be6806117b213a87aae949.png)

二分匹配，由作者提供的图像

我们基本上想将每个预测与最近的真实框关联。因此，我们实际上是在寻找预测集与真实框集之间的一个双射，来最小化总损失。

那么我们如何定义这个损失呢？

## 1\. 匹配损失（成对损失）

我们首先需要定义一个匹配损失，它对应于一个预测框和一个真实框之间的损失：

这个损失需要考虑两个组件：

+   **分类损失**（预测框内的类别是否与真实类别相同）

+   **边界框损失**（边界框是否接近真实框）

![](img/3624d28403e0a284aa6a0767562ff29d.png)

匹配损失

更准确地说，对于边界框组件，有两个子组件：

+   交并比损失（IOU）

+   L1 损失（坐标之间的绝对差异）

![](img/360a54f203774c2e160b37c8428202b6.png)

## 2\. 双射的总损失

计算总损失时，我们只是对*N*个实例求和：

![](img/b55f7d61d8df711130b3ea3aa7eec395.png)

所以基本上我们的问题是找到最小化总损失的双射：

![](img/c411a6a9b81d88fc882850c11e59d6af.png)

问题的重新表述

# 性能洞察

![](img/f0a9bdef234d7ce79dfd7e5147b34a42.png)

DETR 与 Faster RCNN 的性能对比

+   **DETR：** 这指的是原始模型，使用了一个用于目标检测的 transformer 和**ResNet-50 作为骨干网络。**

+   **DETR-R101：** 这是 DETR 的一个变体，采用了**ResNet-101 骨干网络而不是 ResNet-50**。这里，“R101”指的是“ResNet-101”。

+   **DETR-DC5：** 这个版本的 DETR 使用了修改过的**ResNet-50 骨干网络中的扩张 C5 阶段**，由于特征分辨率的提高，提高了模型在小物体上的表现。

+   **DETR-DC5-R101：** 这个变体结合了**这两种修改**。它使用 ResNet-101 骨干网络，并包括扩张 C5 阶段，受益于更深的网络和更高的特征分辨率。

DETR 在大型物体上显著超越了基线，这很可能是由于 transformer 允许的非局部计算。然而，值得注意的是，DETR 在小物体上的表现较差。

# 为什么在这种情况下 Attention 机制如此强大？

![](img/6148040e2feb2ba4e44ddee1901f4c47.png)

对重叠实例的 Attention，图像来自文章

非常有趣的是，我们可以观察到在重叠实例的情况下，Attention 机制能够正确地分离出各个实例，如上图所示。

![](img/54f9274e18939352912de99147a2587b.png)

极端点的 Attention 机制

同样有趣的是，注意力机制集中在物体的极端点上以生成边界框，这正是我们所期望的。

# 总结

DETR 不仅仅是一个模型；它是一个范式转变，将目标检测从一个一对多的问题转变为一个集合预测问题，充分利用了 Transformer 架构的进展。

自其诞生以来，已经出现了许多改进，例如 DETR++和 CO-DETR，现在在[LVIS](https://www.lvisdataset.org)数据集上引领了实例分割和目标检测的最前沿。

感谢阅读！在你离开之前：

+   查看我在 Github 上的[AI 教程合集](https://github.com/FrancoisPorcher/awesome-ai-tutorials)

[](https://github.com/FrancoisPorcher/awesome-ai-tutorials?source=post_page-----a8b3327b737a--------------------------------) [## GitHub — FrancoisPorcher/awesome-ai-tutorials: 最好的 AI 教程合集让你成为…]

### 最好的 AI 教程合集，让你成为数据科学的专家！— GitHub …

[github.com](https://github.com/FrancoisPorcher/awesome-ai-tutorials?source=post_page-----a8b3327b737a--------------------------------)

*你应该将我的文章直接发送到你的邮箱。* [***在这里订阅。***](https://medium.com/@francoisporcher/subscribe)

*如果你想访问 Medium 上的优质文章，你只需每月支付 $5 会员费。如果你通过* [***我的链接***](https://medium.com/@francoisporcher/membership)*注册，你将用你的一部分费用支持我，而无需额外支付。*

> 如果你觉得这篇文章有见解且有帮助，请考虑关注我并点赞，以便获得更多深入的内容！你的支持帮助我继续制作对我们集体理解有帮助的内容。

# 参考文献

+   “End-to-End Object Detection with Transformers” 由 Nicolas Carion、Francisco Massa、Gabriel Synnaeve、Nicolas Usunier、Alexander Kirillov 和 Sergey Zagoruyko 编写。你可以在 [arXiv](https://arxiv.org/abs/2005.12872) 上阅读全文。

+   Zong, Z., Song, G., & Liu, Y.（出版年份）。DET 网上协作混合分配训练。 [`arxiv.org/pdf/2211.12860.pdf`](https://arxiv.org/pdf/2211.12860.pdf)。

+   [COCO 数据集](https://cocodataset.org/#home)
