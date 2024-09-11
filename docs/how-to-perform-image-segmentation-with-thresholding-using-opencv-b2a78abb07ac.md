# 如何使用 OpenCV 进行图像分割与阈值处理

> 原文：[https://towardsdatascience.com/how-to-perform-image-segmentation-with-thresholding-using-opencv-b2a78abb07ac?source=collection_archive---------6-----------------------#2023-01-12](https://towardsdatascience.com/how-to-perform-image-segmentation-with-thresholding-using-opencv-b2a78abb07ac?source=collection_archive---------6-----------------------#2023-01-12)

![](../Images/d02ca3281b7d0e272bd0b812d2f46411.png)

照片由 [Lysander Yuen](https://unsplash.com/@lysanderyuen?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 简单的、Otsu 的和自适应阈值处理的实现及示例

[](https://rashida00.medium.com/?source=post_page-----b2a78abb07ac--------------------------------)[![Rashida Nasrin Sucky](../Images/42bd057e8eca255907c43c29a498f2ca.png)](https://rashida00.medium.com/?source=post_page-----b2a78abb07ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b2a78abb07ac--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b2a78abb07ac--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----b2a78abb07ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-image-segmentation-with-thresholding-using-opencv-b2a78abb07ac&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----b2a78abb07ac---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2a78abb07ac--------------------------------) ·6分钟阅读·2023年1月12日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb2a78abb07ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-image-segmentation-with-thresholding-using-opencv-b2a78abb07ac&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=-----b2a78abb07ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb2a78abb07ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-image-segmentation-with-thresholding-using-opencv-b2a78abb07ac&source=-----b2a78abb07ac---------------------bookmark_footer-----------)

最常用的分割技术之一是阈值处理。它在计算机视觉中广泛应用，非常有用来将我们感兴趣的物体与背景分开。本文将重点探讨 OpenCV 中的阈值处理技术。

阈值处理有三种不同的类型。我们将了解它们的高层次工作原理，并主要关注如何在 OpenCV 中使用它们，并通过示例进行说明。

## 什么是阈值处理

阈值处理是图像二值化的过程。如果你像我最初那样对灰度图像和二值图像感到困惑，灰度图像中可能有一系列的色调。但在二值图像中，像素值要么是0，要么是255。

## 简单阈值处理

这是一种最基本的阈值处理方法，在许多实际应用中比较难以使用。但我们仍然需要从学习这个方法开始，以了解阈值处理的实际工作原理。我们将看到简单阈值处理如何能够很好地发挥作用。

**简单阈值处理在非常受控的光照条件下可以很好地发挥作用**…
