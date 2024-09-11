# 使用Gumbel Softmax的离散分布变分自编码器（VAE）

> 原文：[https://towardsdatascience.com/variational-autoencoder-vae-with-discrete-distribution-using-gumbel-softmax-b3f749b3417e?source=collection_archive---------11-----------------------#2023-08-09](https://towardsdatascience.com/variational-autoencoder-vae-with-discrete-distribution-using-gumbel-softmax-b3f749b3417e?source=collection_archive---------11-----------------------#2023-08-09)

## 理论和PyTorch实现

[](https://medium.com/@alexml0123?source=post_page-----b3f749b3417e--------------------------------)[![Alexey Kravets](../Images/3b31f9b3c73c6c7ca709f845e6f70023.png)](https://medium.com/@alexml0123?source=post_page-----b3f749b3417e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b3f749b3417e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b3f749b3417e--------------------------------) [Alexey Kravets](https://medium.com/@alexml0123?source=post_page-----b3f749b3417e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcf3e4a05b535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariational-autoencoder-vae-with-discrete-distribution-using-gumbel-softmax-b3f749b3417e&user=Alexey+Kravets&userId=cf3e4a05b535&source=post_page-cf3e4a05b535----b3f749b3417e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b3f749b3417e--------------------------------) ·17分钟阅读·2023年8月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb3f749b3417e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariational-autoencoder-vae-with-discrete-distribution-using-gumbel-softmax-b3f749b3417e&user=Alexey+Kravets&userId=cf3e4a05b535&source=-----b3f749b3417e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb3f749b3417e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariational-autoencoder-vae-with-discrete-distribution-using-gumbel-softmax-b3f749b3417e&source=-----b3f749b3417e---------------------bookmark_footer-----------)![](../Images/596f70c179f4ea59a15aa4591018200b.png)

[https://unsplash.com/photos/sbVu5zitZt0](https://unsplash.com/photos/sbVu5zitZt0)

由于这篇文章将会很详尽，我将为读者提供一个索引，以便更好地导航：

1.  介绍

1.  变分自编码器（VAEs）简要介绍

1.  Kullback–Leibler (KL) 散度

1.  VAE损失

1.  重参数化技巧

1.  从类别分布中采样 & Gumbel-Max技巧

1.  实现

## 介绍

生成模型如今变得非常流行，因为它们能够通过学习和捕捉训练数据的潜在概率分布来生成具有固有变异性的全新样本。

我们可以识别出两类主要的生成模型，即生成对抗网络（GANs）、变分自编码器（VAEs）和扩散模型。在本文中，我们将深入探讨变分自编码器，特别关注具有分类潜在空间的变分自编码器。

## **变分自编码器（VAEs）简要介绍**
