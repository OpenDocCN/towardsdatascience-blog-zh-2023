# 大型图像模型中的最新CNN核

> 原文：[https://towardsdatascience.com/latest-in-cnn-kernels-for-large-image-models-b48b27e75638?source=collection_archive---------3-----------------------#2023-08-04](https://towardsdatascience.com/latest-in-cnn-kernels-for-large-image-models-b48b27e75638?source=collection_archive---------3-----------------------#2023-08-04)

## 对可变形卷积网络中的最新卷积核结构、DCNv2、DCNv3进行的高级概述

[](https://medium.com/@1996hwm?source=post_page-----b48b27e75638--------------------------------)[![Wanming Huang](../Images/05051c8e4fe9c6ffc1d28a9578bf6537.png)](https://medium.com/@1996hwm?source=post_page-----b48b27e75638--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b48b27e75638--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b48b27e75638--------------------------------) [Wanming Huang](https://medium.com/@1996hwm?source=post_page-----b48b27e75638--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cad9f97731c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flatest-in-cnn-kernels-for-large-image-models-b48b27e75638&user=Wanming+Huang&userId=1cad9f97731c&source=post_page-1cad9f97731c----b48b27e75638---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b48b27e75638--------------------------------) ·7分钟阅读·2023年8月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb48b27e75638&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flatest-in-cnn-kernels-for-large-image-models-b48b27e75638&user=Wanming+Huang&userId=1cad9f97731c&source=-----b48b27e75638---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb48b27e75638&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flatest-in-cnn-kernels-for-large-image-models-b48b27e75638&source=-----b48b27e75638---------------------bookmark_footer-----------)![](../Images/53825b3549061898e455c5ff37ab15ed.png)

澳大利亚拜伦湾灯塔 | 作者拍摄

随着OpenAI的ChatGPT的显著成功引发了大型语言模型的热潮，许多人预测下一次突破将发生在大型图像模型领域。在这个领域，视觉模型可以像我们目前对ChatGPT的提示一样被提示去分析甚至生成图像和视频。

最新的大型图像模型深度学习方法已分为两个主要方向：基于卷积神经网络（CNN）的方法和基于变换器的方法。本文将重点介绍 CNN 方面，并提供改进的 CNN 内核结构的高级概述。

# 目录

1.  [DCN](#00f3)

1.  [DCNv2](#1709)

1.  [DCNv3](#22a3)

# 1\. 可变形卷积网络（DCN）

传统上，CNN 内核在每一层中应用于固定位置，导致所有激活单元具有相同的感受野。

如下图所示，为了在输入特征图 ***x*** 上执行卷积，计算每个输出位置 ***p****0* 的值是通过内核权重 ***w*** 和在 ***x*** 上的滑动窗口之间的逐元素乘法和求和来完成的。滑动窗口由网格 ***R*** 定义，这也是 ***p****0**** 的感受野。*** 在 *y* 的同一层中的所有位置，***R*** 的大小保持不变。

![](../Images/cbb617318368ccf22a3c93a1182e37b6.png)

常规 3x3 内核的卷积操作。

每个输出值的计算如下：

![](../Images/4060a93bc9894d787e43933e6adbef99.png)

[论文](https://arxiv.org/pdf/1703.06211v3.pdf)中的常规卷积操作函数。

其中 ***p****n* 枚举滑动窗口（网格 ***R***）中的位置。

RoI（兴趣区域）池化操作也在每一层中固定大小的 bin 上操作。对于 (*i, j*)-th bin 包含 *nij* 像素，其池化结果计算如下：

![](../Images/7eb8c29a407505e0889f7429f7b574bb.png)

[论文](https://arxiv.org/pdf/1703.06211v3.pdf)中的常规平均 RoI 池化函数。

每一层中的 bin 的形状和大小再次保持不变。

![](../Images/6bc09b7c45eef4cf0b3a7ead1634614e.png)

3x3 bin 的常规平均 RoI 池化操作。

因此，对于编码语义的高层，如具有不同尺度的对象，这两个操作特别具有挑战性。

[DCN](https://arxiv.org/pdf/1703.06211v3.pdf) 提出了可变形卷积和可变形池化，这些方法对建模这些几何结构更加灵活。两者都在 2D 空间域上操作，即操作在通道维度上保持不变。

## **可变形卷积**

![](../Images/ac617c6bd395fa1da6bc84592033e74a.png)

3x3 内核的可变形卷积操作。

给定输入特征图 **x**，对于输出特征图 **y** 中的每个位置 ***p****0*，DCN 在枚举常规网格 ***R*** 中的每个位置 ***p****n* 时添加 2D 偏移量 △***p****n*。

![](../Images/252f21b12aa8ce2e3daa7fbb8152e2ae.png)

[论文](https://arxiv.org/pdf/1703.06211v3.pdf)中的可变形卷积函数。

这些偏移量从前面的特征图中学习得到，通过在特征图上应用额外的卷积层获得。由于这些偏移量通常是小数，它们通过双线性插值来实现。

## **可变形 RoI 池化**

类似于卷积操作，池化偏移量 △***p****ij* 被添加到原始 bin 位置。

![](../Images/959d5a7c4599ec6b37fe78d6f2b673a9.png)

可变形 RoI 池化函数来自 [论文](https://arxiv.org/pdf/1703.06211v3.pdf)。

如下图所示，这些偏移量通过原始池化结果后的全连接（FC）层进行学习。

![](../Images/0069fe84c6c266e72c62cba93337ff30.png)

可变形平均 RoI 池化操作，使用 3x3 bin。

## **可变形位置敏感（PS）RoI 池化**

在将可变形操作应用于 PS RoI 池化 ([Dai et al., n.d.](https://arxiv.org/pdf/1605.06409v2.pdf)) 时，如下图所示，偏移量应用于每个分数图而不是输入特征图。这些偏移量通过卷积层而不是全连接层进行学习。

*位置敏感 RoI 池化* ([Dai et al., n.d.](https://arxiv.org/pdf/1605.06409v2.pdf))*：传统的 RoI 池化丢失了每个区域所代表的对象部分的信息。PS RoI 池化通过将输入特征图转换为每个对象类别的 k² 分数图来保留这些信息，其中每个分数图表示一个特定的空间部分。因此，对于 C 个对象类别，总共有 k² (C+1) 个分数图。*

![](../Images/2ce91cae1a55879f64a3a8c6b7f74cce.png)

3x3 可变形 PS RoI 池化的示意图 | 来源于 [论文](https://arxiv.org/pdf/1703.06211v3.pdf)。

# 2\. DCNv2

尽管 DCN 允许更灵活地建模感受野，但它假设每个感受野中的像素对响应的贡献是相等的，这种情况通常不成立。为了更好地理解贡献行为，作者使用三种方法来可视化空间支持：

1.  有效的感受野：节点响应关于每个图像像素的强度扰动的梯度

1.  有效的采样/ bin 位置：网络节点相对于采样/ bin 位置的梯度

1.  错误界限显著区域：逐步遮蔽图像的部分，以找到与整个图像产生相同响应的最小图像区域

为了将可学习的特征幅度分配到感受野中的位置，[DCNv2](https://arxiv.org/pdf/1811.11168.pdf) 引入了调制可变形模块：

![](../Images/27af2f6ab5d795a12b0e24917a48cab3.png)

DCNv2 卷积函数来自 [论文](https://arxiv.org/pdf/1811.11168.pdf)，符号已修订以匹配 DCN 论文中的符号。

对于位置 ***p****0*，偏移量 △***pn*** 及其幅度 △***m****n* 通过应用于相同输入特征图的单独卷积层进行学习。

DCNv2 通过为每个 (i,j)-th bin 添加一个可学习的幅度 △***m****ij* 来修订可变形 RoI 池化。

![](../Images/3717e68140640eabaf86a43988b688a3.png)

DCNv2 池化函数来自 [论文](https://arxiv.org/pdf/1811.11168.pdf)，符号已修订以匹配 DCN 论文中的符号。

DCNv2 还扩展了变形卷积层的使用，以替代 ResNet-50 中 conv3 到 conv5 阶段的常规卷积层。

# **3\. DCNv3**

为了减少参数大小和内存复杂度， [DCNv3](https://arxiv.org/pdf/2211.05778.pdf) 对卷积核结构进行了以下调整。

1.  灵感来自于[深度可分离卷积（Chollet，2017）](https://arxiv.org/pdf/1610.02357.pdf)

*深度可分离卷积将传统卷积解耦为：1\. 深度卷积：每个输入特征通道与一个滤波器单独进行卷积；2\. 点卷积：在通道上应用 1x1 卷积。*

作者提出将特征幅度 *m* 设为深度卷积部分，将投影权重 *w* 在网格中的位置共享作为点卷积部分。

2\. 灵感来自于[组卷积](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)（Krizhevsky，Sutskever 和 Hinton，2012）

*组卷积：将输入通道和输出通道拆分为多个组，并对每个组分别应用卷积。*

[DCNv3](https://arxiv.org/pdf/2211.05778.pdf)（Wang 等，2023）提出将卷积拆分为 G 组，每组具有单独的偏移量 △***p****gn* 和特征幅度 △***m****gn*。

因此，DCNv3 被表述为：

![](../Images/cb0312cbf8c926d9e223e8254ec7fda7.png)

DCNv3 卷积功能来自于[论文，](https://arxiv.org/pdf/2211.05778.pdf)符号修订以匹配 DCN 论文中的符号。

其中 *G* 是卷积组的总数，***w****g* 与位置无关，△***m****gn* 通过 softmax 函数进行标准化，使得网格 ***R*** 上的和为 1。

# 性能

迄今为止，基于 DCNv3 的 InternImage 在检测和分割等多个下游任务中表现优越，如下表所示，以及 papers with code 上的排行榜。有关更详细的比较，请参考原论文。

![](../Images/e0b2852403fca3c51778ab822abfde1a.png)

COCO val2017上的目标检测和实例分割性能。FLOPs 是用 1280×800 输入测量的。AP’ 和 AP’ 分别表示框 AP 和掩码 AP。“MS”代表多尺度训练。来源于[论文](https://arxiv.org/pdf/2211.05778.pdf)。

![](../Images/7bdd9ad06e7904179df50b42de38d53f.png)

来自[paperswithcode.com](https://paperswithcode.com/task/object-detection)的目标检测排行榜截图。

![](../Images/a582193e0ed5faab979a487ba6e3a102.png)

来自[paperswithcode.com](https://paperswithcode.com/task/semantic-segmentation)的语义分割排行榜截图。

# 摘要

在这篇文章中，我们回顾了常规卷积网络的核结构及其最新改进，包括可变形卷积网络（DCN）及两个更新版本：DCNv2 和 DCNv3。我们讨论了传统结构的局限性，并突出了在前一版本基础上的创新进展。欲深入了解这些模型，请参阅参考文献中的论文。

# 致谢

特别感谢[Kenneth Leung](https://medium.com/u/dcd08e36f2d0?source=post_page-----b48b27e75638--------------------------------)，他激发了我创作这篇文章的灵感，并分享了惊人的想法。对Kenneth、[Melissa Han](https://medium.com/u/2c1cee38ff69?source=post_page-----b48b27e75638--------------------------------)和Annie Liao致以深深的谢意，他们对改进这篇文章做出了贡献。你们的深刻建议和建设性反馈显著提升了内容的质量和深度。

# 参考文献

Dai, J., Qi, H., Xiong, Y., Li, Y., Zhang, G., Hu, H. 和 Wei, Y. (n.d.). *可变形卷积网络*。[在线] 可在：[https://arxiv.org/pdf/1703.06211v3.pdf.](https://arxiv.org/pdf/1703.06211v3.pdf.)

‌Zhu, X., Hu, H., Lin, S. 和 Dai, J. (n.d.). *Deformable ConvNets v2: 更可变形，效果更佳*。[在线] 可在：[https://arxiv.org/pdf/1811.11168.pdf.](https://arxiv.org/pdf/1811.11168.pdf.)

‌Wang, W., Dai, J., Chen, Z., Huang, Z., Li, Z., Zhu, X., Hu, X., Lu, T., Lu, L., Li, H., Wang, X. 和 Qiao, Y. (n.d.). *InternImage: 利用可变形卷积探索大规模视觉基础模型*。[在线] 可在：[https://arxiv.org/pdf/2211.05778.pdf](https://arxiv.org/pdf/2211.05778.pdf) [访问日期 2023年7月31日]。

Chollet, F. (n.d.). *Xception: 深度学习中的深度可分离卷积*。[在线] 可在：[https://arxiv.org/pdf/1610.02357.pdf.](https://arxiv.org/pdf/1610.02357.pdf.)

‌Krizhevsky, A., Sutskever, I. 和 Hinton, G.E. (2012). 使用深度卷积神经网络的ImageNet分类。*ACM通讯*, 60(6), 页84–90\. doi:https://doi.org/10.1145/3065386.

Dai, J., Li, Y., He, K. 和 Sun, J. (n.d.). *R-FCN: 基于区域的全卷积网络进行物体检测*。[在线] 可在：[https://arxiv.org/pdf/1605.06409v2.pdf.](https://arxiv.org/pdf/1605.06409v2.pdf.)

‌

‌
