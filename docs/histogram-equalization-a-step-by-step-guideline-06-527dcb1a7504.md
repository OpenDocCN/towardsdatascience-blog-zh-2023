# 直方图均衡化：逐步指南 (CV- 06)

> 原文：[https://towardsdatascience.com/histogram-equalization-a-step-by-step-guideline-06-527dcb1a7504?source=collection_archive---------2-----------------------#2023-07-27](https://towardsdatascience.com/histogram-equalization-a-step-by-step-guideline-06-527dcb1a7504?source=collection_archive---------2-----------------------#2023-07-27)

## 图像的直方图均衡化详解

[![Md. Zubair](../Images/1b983a23226ce7561796fa5b28c00d65.png)](https://zubairhossain.medium.com/?source=post_page-----527dcb1a7504--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----527dcb1a7504--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----527dcb1a7504--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fdaeaeeea52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhistogram-equalization-a-step-by-step-guideline-06-527dcb1a7504&user=Md.+Zubair&userId=2fdaeaeeea52&source=post_page-2fdaeaeeea52----527dcb1a7504---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----527dcb1a7504--------------------------------) ·5分钟阅读·2023年7月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F527dcb1a7504&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhistogram-equalization-a-step-by-step-guideline-06-527dcb1a7504&user=Md.+Zubair&userId=2fdaeaeeea52&source=-----527dcb1a7504---------------------clap_footer-----------)

--

![](../Images/36c2593b53eee8c7ba0611e2ee487993.png)

原始图像由 [Dan Fador](https://pixabay.com/users/danfador-55851/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=190055) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=190055)（左上图是原图，左下图是该图的灰度版本，右侧的图像是直方图均衡化的结果）

## 动机

直方图是通过条形图进行频率分布的可视化过程。在计算机视觉中，图像直方图是通过条形图表示强度值的频率。通过图像直方图均衡化，我们可以轻松调整图像强度值的频率分布。通常，这个过程有助于增加图像的对比度和亮度。这个过程简单易行。本文将讨论直方图均衡化的完整过程，并提供代码示例。

## 目录

1.  `[**图像直方图**](#bc52)`

1.  `[**直方图均衡化的完整过程**](#38d7)`

1.  `[**逐步、动手实现**](#04de)`

## 图像直方图

***图像直方图*** 是通过条形图表示图像强度值的频率。在图 -1 中，我展示了一张示例图像及其在二维空间中的强度值。

值的范围从 0 到 7。让我们计算这些值的频率。

一个 ***图像直方图*** 是一个简单的表示 *频率* 的图示，反映了…
