# 使用自编码器方法在 TensorFlow 和 Keras 中进行异常检测

> 原文：[https://towardsdatascience.com/anomaly-detection-in-tensorflow-and-keras-using-the-autoencoder-method-5600aca29c50?source=collection_archive---------4-----------------------#2023-09-23](https://towardsdatascience.com/anomaly-detection-in-tensorflow-and-keras-using-the-autoencoder-method-5600aca29c50?source=collection_archive---------4-----------------------#2023-09-23)

![](../Images/06b21f76fa65b8247eabeb45da835d72.png)

图片由 [Leiada Krozjhen](https://unsplash.com/@leiadakrozjhen?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 一种前沿的无监督方法，用于噪声去除、降维、异常检测等

[](https://rashida00.medium.com/?source=post_page-----5600aca29c50--------------------------------)[![Rashida Nasrin Sucky](../Images/42bd057e8eca255907c43c29a498f2ca.png)](https://rashida00.medium.com/?source=post_page-----5600aca29c50--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5600aca29c50--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5600aca29c50--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----5600aca29c50--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanomaly-detection-in-tensorflow-and-keras-using-the-autoencoder-method-5600aca29c50&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----5600aca29c50---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5600aca29c50--------------------------------) · 7 分钟阅读 · 2023 年 9 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5600aca29c50&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanomaly-detection-in-tensorflow-and-keras-using-the-autoencoder-method-5600aca29c50&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=-----5600aca29c50---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5600aca29c50&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanomaly-detection-in-tensorflow-and-keras-using-the-autoencoder-method-5600aca29c50&source=-----5600aca29c50---------------------bookmark_footer-----------)

迄今为止，我分享的所有关于 TensorFlow 和神经网络的教程都涉及监督学习。这一次将讲述自编码器，它是一种无监督学习技术。如果我想简单表达，自编码器**通过压缩输入数据**来**减少数据中的噪声**，并对数据进行编码和重建。通过这种方式，自编码器可以**降低数据的维度**或噪声，并专注于输入数据的真实重点。

正如你从自编码器的介绍中看到的，这里需要的不止一个过程。

1.  首先是一个用于压缩输入数据的模型，即编码器模型。

1.  然后是另一个模型，用于重建压缩后的数据，这个模型应尽可能接近输入数据，即解码器模型。

在这个过程中，它可以去除噪声、降低维度，并清理输入数据。

在本教程中，我将详细解释自编码器如何工作，并通过一个实际示例进行演示。
