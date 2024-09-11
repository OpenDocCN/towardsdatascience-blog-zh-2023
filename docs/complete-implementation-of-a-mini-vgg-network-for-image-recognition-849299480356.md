# 完整实现一个用于图像识别的迷你 VGG 网络

> 原文：[https://towardsdatascience.com/complete-implementation-of-a-mini-vgg-network-for-image-recognition-849299480356?source=collection_archive---------11-----------------------#2023-02-27](https://towardsdatascience.com/complete-implementation-of-a-mini-vgg-network-for-image-recognition-849299480356?source=collection_archive---------11-----------------------#2023-02-27)

![](../Images/4e4dcec20a06722d2ced502b9d033988.png)

照片由 [Guillaume de Germain](https://unsplash.com/@guillaumedegermain?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 一个深度卷积神经网络，用于更高效的图像识别

[](https://rashida00.medium.com/?source=post_page-----849299480356--------------------------------)[![Rashida Nasrin Sucky](../Images/42bd057e8eca255907c43c29a498f2ca.png)](https://rashida00.medium.com/?source=post_page-----849299480356--------------------------------)[](https://towardsdatascience.com/?source=post_page-----849299480356--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----849299480356--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----849299480356--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomplete-implementation-of-a-mini-vgg-network-for-image-recognition-849299480356&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----849299480356---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----849299480356--------------------------------) ·7 min read·2023年2月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F849299480356&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomplete-implementation-of-a-mini-vgg-network-for-image-recognition-849299480356&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=-----849299480356---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F849299480356&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcomplete-implementation-of-a-mini-vgg-network-for-image-recognition-849299480356&source=-----849299480356---------------------bookmark_footer-----------)

VGG 网络是最流行的图像识别技术之一的基础。学习它是值得的，因为它开辟了许多途径。要理解 VGGNet，你需要了解卷积神经网络（CNN）。如果你对 CNN 架构不熟悉，请先阅读这个教程：

[](/convolutional-neural-network-good-understanding-of-the-layers-and-an-image-classification-example-a280bc02c13e?source=post_page-----849299480356--------------------------------) [## 卷积神经网络：对层的良好理解及图像分类示例

### 信息量丰富

[towardsdatascience.com](/convolutional-neural-network-good-understanding-of-the-layers-and-an-image-classification-example-a280bc02c13e?source=post_page-----849299480356--------------------------------)

在本文中，我们将仅关注 VGGNet 的实现部分。因此我们会比较快地进行讨论。

## 关于 VGG 网络

VGGNet 是一种卷积神经网络（CNN），能够更成功地提取特征。在 VGGNet 中，我们堆叠多个卷积层。VGGNet 可以是浅层的也可以是深层的。在浅层 VGGNet 中，通常仅添加两组四个卷积层，正如我们接下来所见。而在深层 VGGNet 中，可以添加超过四个卷积层。两个常用的深层 VGGNet 是 VGG16，它总共使用 16 层，等等。
