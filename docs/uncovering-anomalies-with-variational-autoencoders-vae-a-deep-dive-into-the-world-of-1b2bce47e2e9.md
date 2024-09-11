# 使用变分自编码器（VAE）发现异常：深入探讨无监督学习的世界

> 原文：[`towardsdatascience.com/uncovering-anomalies-with-variational-autoencoders-vae-a-deep-dive-into-the-world-of-1b2bce47e2e9?source=collection_archive---------9-----------------------#2023-01-17`](https://towardsdatascience.com/uncovering-anomalies-with-variational-autoencoders-vae-a-deep-dive-into-the-world-of-1b2bce47e2e9?source=collection_archive---------9-----------------------#2023-01-17)

## 使用变分自编码器（VAE）检测各种数据类型异常的一个示例用例

[](https://medium.com/@will.badr?source=post_page-----1b2bce47e2e9--------------------------------)![Will Badr](https://medium.com/@will.badr?source=post_page-----1b2bce47e2e9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1b2bce47e2e9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b2bce47e2e9--------------------------------) [Will Badr](https://medium.com/@will.badr?source=post_page-----1b2bce47e2e9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F551ba3f6b67d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-anomalies-with-variational-autoencoders-vae-a-deep-dive-into-the-world-of-1b2bce47e2e9&user=Will+Badr&userId=551ba3f6b67d&source=post_page-551ba3f6b67d----1b2bce47e2e9---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b2bce47e2e9--------------------------------) · 9 分钟阅读 · 2023 年 1 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1b2bce47e2e9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-anomalies-with-variational-autoencoders-vae-a-deep-dive-into-the-world-of-1b2bce47e2e9&user=Will+Badr&userId=551ba3f6b67d&source=-----1b2bce47e2e9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1b2bce47e2e9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-anomalies-with-variational-autoencoders-vae-a-deep-dive-into-the-world-of-1b2bce47e2e9&source=-----1b2bce47e2e9---------------------bookmark_footer-----------)![](img/d0d9e1cf8a587f22677aba93c805486f.png)

照片由[JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在早期的[帖子](https://medium.com/p/3e5c6f017726)中，我解释了什么是自编码器，它们的用途以及如何利用它们来训练异常检测模型。作为提醒，自编码器是一种常用于降维和学习特征的神经网络。它们也常用于异常检测，因为它们可以学习重建正常数据，但可能在重建异常或离群数据时遇到困难。

自编码器网络由两个组件组成：编码器和解码器。编码器将输入数据映射到一个低维的潜在空间，而解码器将潜在表示映射回原始输入空间。在训练过程中，自编码器被训练成尽可能准确地重建输入数据。

要使用自编码器进行异常检测，首先需要在*正常、非异常*的数据集上训练自编码器。训练完成后，自编码器可以用于重建新的数据样本。如果一个新的数据样本与自编码器训练的正常数据有显著差异……
