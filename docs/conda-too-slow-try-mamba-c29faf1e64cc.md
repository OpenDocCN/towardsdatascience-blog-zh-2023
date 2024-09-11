# Conda太慢？试试Mamba！

> 原文：[https://towardsdatascience.com/conda-too-slow-try-mamba-c29faf1e64cc?source=collection_archive---------6-----------------------#2023-11-22](https://towardsdatascience.com/conda-too-slow-try-mamba-c29faf1e64cc?source=collection_archive---------6-----------------------#2023-11-22)

## 流行的包管理器对比

[](https://medium.com/@caroline.arnold_63207?source=post_page-----c29faf1e64cc--------------------------------)[![Caroline Arnold](../Images/2560e106ba9deda7889c7d253792d814.png)](https://medium.com/@caroline.arnold_63207?source=post_page-----c29faf1e64cc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c29faf1e64cc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c29faf1e64cc--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----c29faf1e64cc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9367198e7a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconda-too-slow-try-mamba-c29faf1e64cc&user=Caroline+Arnold&userId=9367198e7a3c&source=post_page-9367198e7a3c----c29faf1e64cc---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c29faf1e64cc--------------------------------) ·5分钟阅读·2023年11月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc29faf1e64cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconda-too-slow-try-mamba-c29faf1e64cc&user=Caroline+Arnold&userId=9367198e7a3c&source=-----c29faf1e64cc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc29faf1e64cc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconda-too-slow-try-mamba-c29faf1e64cc&source=-----c29faf1e64cc---------------------bookmark_footer-----------)![](../Images/740951dc23be607fefeb3ee291fcc7a4.png)

复古的包管理交付。照片由[Charlie M](https://unsplash.com/@chollz?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

迟早，每位数据科学家和机器学习工程师都会遇到包管理器和环境。环境包含运行项目代码所需的库。开发者可以在同一台机器上创建多个环境，从而可以为不同的项目维护不同的环境。软件不会系统性安装，而是包含在一个环境内。

包管理器用于分发软件库。流行的包管理器包括conda、pip和mamba。

> 绝对值得查看 mamba，因为与 conda 相比，我通过 mamba 安装一个大型环境的速度快了 10 倍！

在这篇文章中，我将向你展示如何获得这种加速。我将讨论：

+   如何设置环境

+   conda 和 mamba 包管理器及环境管理器

+   它们在速度方面的比较

+   libmamba：mamba 在 conda 中的加速效果？

## 软件环境

维护软件环境文件可以确保代码的可复现性，并且可以在不同的平台上运行。机器学习项目应始终包括所需软件包的列表，等等……
