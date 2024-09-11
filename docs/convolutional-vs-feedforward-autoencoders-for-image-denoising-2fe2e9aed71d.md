# 卷积自编码器与前馈自编码器在图像去噪中的比较

> 原文：[`towardsdatascience.com/convolutional-vs-feedforward-autoencoders-for-image-denoising-2fe2e9aed71d?source=collection_archive---------15-----------------------#2023-01-24`](https://towardsdatascience.com/convolutional-vs-feedforward-autoencoders-for-image-denoising-2fe2e9aed71d?source=collection_archive---------15-----------------------#2023-01-24)

## 自编码器的应用

## 使用卷积和前馈自编码器清理损坏的图像

![Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----2fe2e9aed71d--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fe2e9aed71d--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----2fe2e9aed71d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff90a3bb1d400&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-vs-feedforward-autoencoders-for-image-denoising-2fe2e9aed71d&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=post_page-f90a3bb1d400----2fe2e9aed71d---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fe2e9aed71d--------------------------------) · 9 分钟阅读 · 2023 年 1 月 24 日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2fe2e9aed71d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvolutional-vs-feedforward-autoencoders-for-image-denoising-2fe2e9aed71d&source=-----2fe2e9aed71d---------------------bookmark_footer-----------)

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
02\. Autoecnoder Latent Representation and Hidden Layers
03\. [Autoecnoder Latent Representation and Latent Vector Dimension](https://rukshanpramoditha.medium.com/how-the-dimension-of-autoencoder-latent-vector-affects-the-quality-of-latent-representation-c98c38fbe3c3)
04\. Dimensionality Reduction with Autoencoders
05\. Creating Shallow and Deep Autoencoders in Keras

**Optional
--------**
06\. Convolutional Neural Network (CNN) Architecture
07\. Coding a Convolutional Neural Network (CNN)…
```
