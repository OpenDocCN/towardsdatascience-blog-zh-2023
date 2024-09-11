# **苹果硅芯片（M1 & M2）上的终极 Python 和 Tensorflow 设置指南**

> 原文：[`towardsdatascience.com/the-ultimate-python-and-tensorflow-set-up-guide-for-apple-silicon-macs-m1-m2-e9ef304a2c06?source=collection_archive---------3-----------------------#2023-02-25`](https://towardsdatascience.com/the-ultimate-python-and-tensorflow-set-up-guide-for-apple-silicon-macs-m1-m2-e9ef304a2c06?source=collection_archive---------3-----------------------#2023-02-25)

## 分步指南

## 在 ARM Macs 上安装 TensorFlow 和 Python 从未如此简单

[](https://polmarin.medium.com/?source=post_page-----e9ef304a2c06--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----e9ef304a2c06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9ef304a2c06--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9ef304a2c06--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----e9ef304a2c06--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-python-and-tensorflow-set-up-guide-for-apple-silicon-macs-m1-m2-e9ef304a2c06&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----e9ef304a2c06---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9ef304a2c06--------------------------------) ·6 分钟阅读·2023 年 2 月 25 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe9ef304a2c06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-python-and-tensorflow-set-up-guide-for-apple-silicon-macs-m1-m2-e9ef304a2c06&source=-----e9ef304a2c06---------------------bookmark_footer-----------)![](img/2cd7fe5600bcbb2c4b7f9f2473504fe0.png)

照片由 [Ales Nesetril](https://unsplash.com/de/@alesnesetril?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一年前我换了工作，发现自己有一台全新的 M1 MacBook Pro。我不得不安装数据科学家所需的一切，这真是一场痛苦的经历。

更糟糕的是，结果发现我没有正确设置，一片混乱。

但有位同事分享了一套步骤，跟随这些步骤成功设置之后，我觉得可以和大家分享一下。因此，我对其进行了总结和扩展，添加了额外的信息和步骤以确保完整性。由于我在网上没有找到类似的内容，或者至少在搜索时没有找到，所以我决定公开，以便每个人都能受益。

在这个教程中，你将找到一个逐步指导，教你如何在 M1 和 M2 Mac 上成功安装 Python 和 Tensorflow，而不必经历自己设置的痛苦。

工作流程相对简单明了：

1.  我们首先在 Mac 上安装任何开发者所需的基本要求（如果你不是使用新款 Mac，你可能已经具备了这些要求）。
