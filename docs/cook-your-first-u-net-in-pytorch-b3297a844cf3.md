# 在 PyTorch 中制作你的第一个 U-Net

> 原文：[https://towardsdatascience.com/cook-your-first-u-net-in-pytorch-b3297a844cf3?source=collection_archive---------0-----------------------#2023-05-12](https://towardsdatascience.com/cook-your-first-u-net-in-pytorch-b3297a844cf3?source=collection_archive---------0-----------------------#2023-05-12)

## 一份神奇的配方，助力你的图像分割项目

[](https://mostafawael.medium.com/?source=post_page-----b3297a844cf3--------------------------------)[![Mostafa Wael](../Images/bf0a052c6446eb3d133e67453ae38143.png)](https://mostafawael.medium.com/?source=post_page-----b3297a844cf3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b3297a844cf3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b3297a844cf3--------------------------------) [Mostafa Wael](https://mostafawael.medium.com/?source=post_page-----b3297a844cf3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc10e1129fb32&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcook-your-first-u-net-in-pytorch-b3297a844cf3&user=Mostafa+Wael&userId=c10e1129fb32&source=post_page-c10e1129fb32----b3297a844cf3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b3297a844cf3--------------------------------) · 6分钟阅读 · 2023年5月12日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb3297a844cf3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcook-your-first-u-net-in-pytorch-b3297a844cf3&source=-----b3297a844cf3---------------------bookmark_footer-----------)![](../Images/c1de747dfac6df8728f9837b2c6bed9d.png)

图片由 [Stefan C. Asafti](https://unsplash.com/@stefanasafti?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，发布在 [Unsplash](https://unsplash.com/photos/x5jilo3ck3o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上。

U-Net 是一种用于图像分析中语义分割任务的深度学习架构。它由 Olaf Ronneberger、Philipp Fischer 和 Thomas Brox 在一篇名为 “[U-Net: Convolutional Networks for Biomedical Image Segmentation](https://papers.labml.ai/paper/2e48c3ffdc8311eba3db37f65e372566)” 的论文中提出。

它对生物医学图像分割任务特别有效，因为它可以处理任意大小的图像，并生成具有清晰物体边界的平滑高质量分割掩模。它后来被广泛采用于许多其他图像分割任务，例如卫星和航空图像分析，以及自然图像分割。

在本教程中，我们将更深入地了解 U-Net 及其工作原理，并使用 PyTorch 制作我们自己的实现配方。所以，让我们开始吧！

# 它是如何工作的？

U-Net 架构包括两部分：编码器和解码器。

![](../Images/302f2b53c15c476ff36507d93dc9f5e3.png)

[U-Net：用于生物医学图像分割的卷积网络](https://paperswithcode.com/paper/u-net-convolutional-networks-for-biomedical)

## 编码器（收缩路径）

编码器是一系列**卷积**和**池化**层，逐步…
