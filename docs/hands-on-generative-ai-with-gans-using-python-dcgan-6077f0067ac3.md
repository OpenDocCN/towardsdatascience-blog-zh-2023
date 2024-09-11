# 使用 Python 的 DCGAN 进行动手实践生成式 AI

> 原文：[`towardsdatascience.com/hands-on-generative-ai-with-gans-using-python-dcgan-6077f0067ac3?source=collection_archive---------11-----------------------#2023-04-04`](https://towardsdatascience.com/hands-on-generative-ai-with-gans-using-python-dcgan-6077f0067ac3?source=collection_archive---------11-----------------------#2023-04-04)

![](img/a663051d751ad0481e8cc825db37cbd7.png)

图片由[Vinicius "amnx" Amano](https://unsplash.com/@viniciusamano?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 使用 PyTorch 中的卷积层改进合成图像生成

[](https://medium.com/@marcellopoliti?source=post_page-----6077f0067ac3--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----6077f0067ac3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6077f0067ac3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6077f0067ac3--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----6077f0067ac3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-dcgan-6077f0067ac3&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----6077f0067ac3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6077f0067ac3--------------------------------) · 5 分钟阅读 · 2023 年 4 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6077f0067ac3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-dcgan-6077f0067ac3&user=Marcello+Politi&userId=7390355d40fe&source=-----6077f0067ac3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6077f0067ac3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-generative-ai-with-gans-using-python-dcgan-6077f0067ac3&source=-----6077f0067ac3---------------------bookmark_footer-----------)

## 介绍

在我的[上一篇文章](https://medium.com/towards-data-science/hands-on-generative-ai-with-gans-using-python-image-generation-9a62e591c7c6)中，我们已经看到如何**使用 GAN 生成 MNIST 数据集类型的图像**。我们取得了良好的结果，并成功达到了我们的目的。然而，这两个网络 G（生成器）和 D（判别器）主要由全连接层组成。在这一点上，你应该知道，通常在处理图像时，我们使用 CNN（卷积神经网络），因为它们使用卷积层。那么，让我们看看如何通过使用这些类型的层来**改进我们的 GAN**。使用**卷积层的 GAN 称为 DCGAN**。

## 什么是转置反卷积？

通常，当我们处理 CNN 时，我们习惯于使用卷积层。然而，在这种情况下，我们还需要“逆”操作，即转置反卷积，有时也称为反卷积。

**此操作允许我们对特征空间进行上采样**。例如，如果我们有一个由 5x5 网格表示的图像，我们可以“放大”这个网格，使其变为 28x28。

原则上你做的事情非常简单，你**在初始特征图的元素中放入零以放大它，然后应用正常的卷积**……
