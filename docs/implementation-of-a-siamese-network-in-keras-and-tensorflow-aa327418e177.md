# 在Keras和TensorFlow中实现Siamese网络

> 原文：[https://towardsdatascience.com/implementation-of-a-siamese-network-in-keras-and-tensorflow-aa327418e177?source=collection_archive---------9-----------------------#2023-08-14](https://towardsdatascience.com/implementation-of-a-siamese-network-in-keras-and-tensorflow-aa327418e177?source=collection_archive---------9-----------------------#2023-08-14)

![](../Images/b1a1da818760b158826b71ceb1e2665f.png)

照片由[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习物体检测背后的技术（以及更多内容），并附有示例代码。

[](https://rashida00.medium.com/?source=post_page-----aa327418e177--------------------------------)[![Rashida Nasrin Sucky](../Images/42bd057e8eca255907c43c29a498f2ca.png)](https://rashida00.medium.com/?source=post_page-----aa327418e177--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aa327418e177--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----aa327418e177--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----aa327418e177--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementation-of-a-siamese-network-in-keras-and-tensorflow-aa327418e177&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----aa327418e177---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa327418e177--------------------------------) ·7分钟阅读·2023年8月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faa327418e177&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementation-of-a-siamese-network-in-keras-and-tensorflow-aa327418e177&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=-----aa327418e177---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faa327418e177&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementation-of-a-siamese-network-in-keras-and-tensorflow-aa327418e177&source=-----aa327418e177---------------------bookmark_footer-----------)

神经网络在AI/ML领域非常强大和流行，但它们需要大量数据进行训练。对于物体检测、签名验证、语音验证和处方药识别等任务，常规神经网络技术由于对数据的过度需求会更加耗时和昂贵。在这些工作中，**Siamese网络**可能非常强大，因为它比常规神经网络需要的数据少得多。此外，不平衡的数据集也可以表现良好。

本教程将为你提供Siamese网络的高层次概述和一个完整的示例。我在这里使用了fashion-mnist数据集，但这种类似的结构适用于许多其他用例。

## 什么是Siamese网络？

**Siamese网络**包含一个或多个相同的网络，这些相同的网络具有相同的参数和权重。如果一个网络的权重更新，另一个网络的权重也会更新。它们必须完全一致。最终层通常是一个嵌入层，用于计算输出之间的距离。
