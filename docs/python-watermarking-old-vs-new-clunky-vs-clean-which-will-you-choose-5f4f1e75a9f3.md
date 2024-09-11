# Python 水印处理：旧版 vs. 新版，笨重 vs. 精简——你会选择哪一种？

> 原文：[https://towardsdatascience.com/python-watermarking-old-vs-new-clunky-vs-clean-which-will-you-choose-5f4f1e75a9f3?source=collection_archive---------8-----------------------#2023-03-28](https://towardsdatascience.com/python-watermarking-old-vs-new-clunky-vs-clean-which-will-you-choose-5f4f1e75a9f3?source=collection_archive---------8-----------------------#2023-03-28)

![](../Images/7b429df4182e762658cdb26e413f5f4d.png)

图片由 [Siegfried Frech](https://pixabay.com/users/stilles_wasser-19985110/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7863868) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7863868)

## Python 水印处理变得简单：OpenCV、PIL 和 filestools 的全面比较

[](https://christophertao.medium.com/?source=post_page-----5f4f1e75a9f3--------------------------------)[![Christopher Tao](../Images/bea1e3c81cc62eb28bdba9275d6b326f.png)](https://christophertao.medium.com/?source=post_page-----5f4f1e75a9f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5f4f1e75a9f3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5f4f1e75a9f3--------------------------------) [Christopher Tao](https://christophertao.medium.com/?source=post_page-----5f4f1e75a9f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8176fabf308&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-watermarking-old-vs-new-clunky-vs-clean-which-will-you-choose-5f4f1e75a9f3&user=Christopher+Tao&userId=b8176fabf308&source=post_page-b8176fabf308----5f4f1e75a9f3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5f4f1e75a9f3--------------------------------) ·8分钟阅读·2023年3月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5f4f1e75a9f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-watermarking-old-vs-new-clunky-vs-clean-which-will-you-choose-5f4f1e75a9f3&user=Christopher+Tao&userId=b8176fabf308&source=-----5f4f1e75a9f3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5f4f1e75a9f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-watermarking-old-vs-new-clunky-vs-clean-which-will-you-choose-5f4f1e75a9f3&source=-----5f4f1e75a9f3---------------------bookmark_footer-----------)

为图像添加水印是摄影师、艺术家以及任何希望保护其视觉内容免受未经授权使用的人的重要任务。在 Python 世界中，有许多库可以让你给图像添加水印。在这篇文章中，我们将比较三种流行的 Python 水印方法：**OpenCV**、**PIL**（Python Imaging Library）和**filestools**。最后一个方法只需要一行代码！

在这篇文章中，我将演示如何使用我从澳大利亚维多利亚州菲利普岛拍摄的照片来进行图像水印。原始照片在这里。请随意下载以便使用。

![](../Images/eb3c9f8a925a5b880f69a9a38eb16b8b.png)

作者拍摄的照片

# 1\. OpenCV — 小任务的大工具

![](../Images/907a3fa4f5f07147a6edf904a232c5ae.png)

图像来自于[Lukas](https://pixabay.com/users/computerizer-4588466/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2301646)，[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2301646)

OpenCV 是一个全面的计算机视觉库，提供了广泛的图像…
