# 关于 NLP 模型的归一化快速指南

> 原文：[https://towardsdatascience.com/a-quick-guide-on-normalization-for-your-nlp-model-2dbd7d2d42a7?source=collection_archive---------5-----------------------#2023-09-14](https://towardsdatascience.com/a-quick-guide-on-normalization-for-your-nlp-model-2dbd7d2d42a7?source=collection_archive---------5-----------------------#2023-09-14)

## 通过归一化加速模型收敛并稳定训练过程

[](https://medium.com/@vuphuongthao9611?source=post_page-----2dbd7d2d42a7--------------------------------)[![Thao Vu](../Images/9d44a2f199cdc9c29da72d9dc4971561.png)](https://medium.com/@vuphuongthao9611?source=post_page-----2dbd7d2d42a7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2dbd7d2d42a7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2dbd7d2d42a7--------------------------------) [Thao Vu](https://medium.com/@vuphuongthao9611?source=post_page-----2dbd7d2d42a7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa836aac352ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-guide-on-normalization-for-your-nlp-model-2dbd7d2d42a7&user=Thao+Vu&userId=a836aac352ca&source=post_page-a836aac352ca----2dbd7d2d42a7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2dbd7d2d42a7--------------------------------) ·7 min 阅读·2023年9月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2dbd7d2d42a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-guide-on-normalization-for-your-nlp-model-2dbd7d2d42a7&user=Thao+Vu&userId=a836aac352ca&source=-----2dbd7d2d42a7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2dbd7d2d42a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-quick-guide-on-normalization-for-your-nlp-model-2dbd7d2d42a7&source=-----2dbd7d2d42a7---------------------bookmark_footer-----------)![](../Images/664add61750b752d6992bc44d3645704.png)

图片由 [Mattia Bericchia](https://unsplash.com/@mattiabericchia?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

高效地训练深度学习模型是具有挑战性的。随着 NLP 模型规模和架构复杂性的增长，问题变得更加困难。为了处理数十亿个参数，提出了更多优化方法，以实现更快的收敛和稳定的训练。其中一种最显著的技术是归一化。

在本文中，我们将学习一些归一化技术，它们的工作原理以及如何将它们用于 NLP 深度模型。

# 为什么不用 BatchNorm？

BatchNorm [2] 是一种早期提出的归一化技术，旨在解决内部协变量偏移问题。

简单来说，当层的输入数据分布发生变化时，会出现内部协变量偏移。当神经网络被迫适应不同的数据分布时，梯度更新在不同批次之间会发生剧烈变化。因此，模型调整、学习正确权重和收敛所需的时间更长。随着模型规模的增长，这个问题会变得更严重。

初步解决方案包括使用较小的学习率（以便数据的影响...
