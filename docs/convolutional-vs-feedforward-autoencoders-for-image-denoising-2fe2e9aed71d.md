# 卷积自编码器与前馈自编码器在图像去噪中的比较

> 原文：[https://towardsdatascience.com/convolutional-vs-feedforward-autoencoders-for-image-denoising-2fe2e9aed71d?source=collection_archive---------15-----------------------#2023-01-24](https://towardsdatascience.com/convolutional-vs-feedforward-autoencoders-for-image-denoising-2fe2e9aed71d?source=collection_archive---------15-----------------------#2023-01-24)

## 自编码器的应用

## 使用卷积和前馈自编码器清理损坏的图像

[![Rukshan Pramoditha](../Images/b80426aff64ff186cb915795644590b1.png)](https://rukshanpramoditha.medium.com/?source=post_page-----2fe2e9aed71d--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2fe2e9aed71d--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----2fe2e9aed71d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff90a3bb1d400&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-vs-feedforward-autoencoders-for-image-denoising-2fe2e9aed71d&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=post_page-f90a3bb1d400----2fe2e9aed71d---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fe2e9aed71d--------------------------------) · 9分钟阅读 · 2023年1月24日

--

[![](../Images/3dcd4cfa1a5be3ca4f767f62e4438c48.png)](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2fe2e9aed71d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-vs-feedforward-autoencoders-for-image-denoising-2fe2e9aed71d&source=-----2fe2e9aed71d---------------------bookmark_footer-----------)

由[皮埃尔·巴敏](https://unsplash.com/@bamin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片，来源于[Unsplash](https://unsplash.com/photos/_EzTds6Fo44?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)（作者稍作修改）

*你想知道卷积自编码器如何在图像去噪中优于前馈自编码器吗？*

如果是‘*是*’，请继续阅读本文。

# 不同类型的自编码器

自编码器有许多实际应用。图像去噪就是其中之一。

图像去噪指的是从受损的图像中去除噪声，以获得清晰的图像。

用于图像去噪的自编码器特别被称为**去噪自编码器**。

我们已经在以下文章中介绍了自编码器的基本原理。

```py
**Prerequisites
-------------**
01\. [An Introduction to Autoencoders in Deep Learning](https://rukshanpramoditha.medium.com/an-introduction-to-autoencoders-in-deep-learning-ab5a5861f81e)
02\. [Autoecnoder Latent Representation and Hidden Layers](/how-number-of-hidden-layers-affects-the-quality-of-autoencoder-latent-representation-181215c8e7d1)
03\. [Autoecnoder Latent Representation and Latent Vector Dimension](https://rukshanpramoditha.medium.com/how-the-dimension-of-autoencoder-latent-vector-affects-the-quality-of-latent-representation-c98c38fbe3c3)
04\. [Dimensionality Reduction with Autoencoders](/how-autoencoders-outperform-pca-in-dimensionality-reduction-1ae44c68b42f)
05\. [Creating Shallow and Deep Autoencoders in Keras](/generate-mnist-digits-using-shallow-and-deep-autoencoders-in-keras-fb011dd3fec3)

**Optional
--------**
06\. [Convolutional Neural Network (CNN) Architecture](/convolutional-neural-network-cnn-architecture-explained-in-plain-english-using-simple-diagrams-e5de17eacc8f)
07\. [Coding a Convolutional Neural Network (CNN)](/coding-a-convolutional-neural-network-cnn-using-keras-sequential-api-ec5211126875)…
```
