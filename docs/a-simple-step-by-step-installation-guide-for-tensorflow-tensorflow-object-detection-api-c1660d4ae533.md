# TensorFlow 和 TensorFlow 目标检测 API 简单的逐步安装指南

> 原文：[`towardsdatascience.com/a-simple-step-by-step-installation-guide-for-tensorflow-tensorflow-object-detection-api-c1660d4ae533?source=collection_archive---------10-----------------------#2023-02-15`](https://towardsdatascience.com/a-simple-step-by-step-installation-guide-for-tensorflow-tensorflow-object-detection-api-c1660d4ae533?source=collection_archive---------10-----------------------#2023-02-15)

## 安装 TensorFlow 和 TensorFlow 目标检测 API 可能很繁琐且耗时。本指南将引导你快速完成整个设置过程。

[](https://guenterroehrich.medium.com/?source=post_page-----c1660d4ae533--------------------------------)![Günter Röhrich](https://guenterroehrich.medium.com/?source=post_page-----c1660d4ae533--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1660d4ae533--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1660d4ae533--------------------------------) [Günter Röhrich](https://guenterroehrich.medium.com/?source=post_page-----c1660d4ae533--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3718a9423801&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-step-by-step-installation-guide-for-tensorflow-tensorflow-object-detection-api-c1660d4ae533&user=G%C3%BCnter+R%C3%B6hrich&userId=3718a9423801&source=post_page-3718a9423801----c1660d4ae533---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1660d4ae533--------------------------------) ·8 min read·2023 年 2 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc1660d4ae533&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-step-by-step-installation-guide-for-tensorflow-tensorflow-object-detection-api-c1660d4ae533&user=G%C3%BCnter+R%C3%B6hrich&userId=3718a9423801&source=-----c1660d4ae533---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc1660d4ae533&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-step-by-step-installation-guide-for-tensorflow-tensorflow-object-detection-api-c1660d4ae533&source=-----c1660d4ae533---------------------bookmark_footer-----------)

尽管有一个[官方指南](https://tensorflow-object-detection-api-tutorial.readthedocs.io/en/latest/install.html)来安装 TensorFlow 及其目标检测 API，但你仍然可能会面临许多问题（尤其是当你希望安装带有 GPU 支持的版本时）。这篇文章不会做任何假设，而是直接指导你完成整个过程，使你迅速上手。

这些逐步的说明被分解为几个主要部分，但是在我们开始之前，我想指出一个重要的备注：虽然计算机视觉任务在一台普通的新电脑上是可行的，我强烈建议使用安装了 NVIDIA GPU（显卡）的机器。顺便提一下，AMD 显卡通常不被 TensorFlow 和 PyTorch 支持。

![](img/3174a505095f9f44189989019d823202.png)

一个相当复杂的 TensorFlow 安装——图片来自 Dall-E，提示由作者提供。

在现代深度学习框架的支持下，特别是针对 NVIDIA GPU 的最佳优化，你可以 1) 很快地训练模型，2) 很快地进行预测。如果你使用的是仅 CPU 的设置，我建议你不要（重新）训练模型，而是…
