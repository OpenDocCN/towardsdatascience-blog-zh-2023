# ONNX：可互操作深度学习模型的标准

> 原文：[https://towardsdatascience.com/onnx-the-standard-for-interoperable-deep-learning-models-a47dfbdf9a09?source=collection_archive---------7-----------------------#2023-01-24](https://towardsdatascience.com/onnx-the-standard-for-interoperable-deep-learning-models-a47dfbdf9a09?source=collection_archive---------7-----------------------#2023-01-24)

![](../Images/889af96423dbf877991bb0f406706a90.png)

图片由 [Jonny Caspari](https://unsplash.com/@jonnyuiux?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 了解使用ONNX标准在不同框架和硬件平台上部署模型的好处

[](https://medium.com/@marcellopoliti?source=post_page-----a47dfbdf9a09--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----a47dfbdf9a09--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a47dfbdf9a09--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a47dfbdf9a09--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----a47dfbdf9a09--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fonnx-the-standard-for-interoperable-deep-learning-models-a47dfbdf9a09&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----a47dfbdf9a09---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a47dfbdf9a09--------------------------------) ·5分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa47dfbdf9a09&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fonnx-the-standard-for-interoperable-deep-learning-models-a47dfbdf9a09&user=Marcello+Politi&userId=7390355d40fe&source=-----a47dfbdf9a09---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa47dfbdf9a09&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fonnx-the-standard-for-interoperable-deep-learning-models-a47dfbdf9a09&source=-----a47dfbdf9a09---------------------bookmark_footer-----------)

我第一次听说ONNX是在我在INRIA实习期间。当时我正在使用Julia语言开发神经网络剪枝算法。那时还没有很多可以使用的预训练模型，因此利用ONNX导入其他语言和框架开发的模型可能是一种解决方案。

在这篇文章中，我想介绍 ONNX 并通过实际示例来解释它的巨大潜力。

## 什么是 ONNX？

ONNX，或称为开放神经网络交换，是一个用于表示深度学习模型的开源标准。它由 Facebook 和微软开发，旨在使研究人员和工程师能够更轻松地在不同的深度学习框架和硬件平台之间移动模型。

ONNX 的主要优势之一是它允许模型轻松地 **从一个框架（如 PyTorch）导出，并导入到另一个框架（如 TensorFlow）**。这对那些想尝试不同框架以训练和部署其模型的研究人员，或对需要...
