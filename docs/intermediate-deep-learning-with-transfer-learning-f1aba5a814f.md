# 中级深度学习与迁移学习

> 原文：[`towardsdatascience.com/intermediate-deep-learning-with-transfer-learning-f1aba5a814f?source=collection_archive---------2-----------------------#2023-02-22`](https://towardsdatascience.com/intermediate-deep-learning-with-transfer-learning-f1aba5a814f?source=collection_archive---------2-----------------------#2023-02-22)

## 计算机视觉和自然语言处理中的深度学习模型微调实用指南

[](https://medium.com/@iamleonie?source=post_page-----f1aba5a814f--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----f1aba5a814f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f1aba5a814f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1aba5a814f--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----f1aba5a814f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintermediate-deep-learning-with-transfer-learning-f1aba5a814f&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----f1aba5a814f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1aba5a814f--------------------------------) ·11 分钟阅读·2023 年 2 月 22 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff1aba5a814f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintermediate-deep-learning-with-transfer-learning-f1aba5a814f&source=-----f1aba5a814f---------------------bookmark_footer-----------)![](img/77c0a9277a5199f8f3fbdf7450e3992a.png)

作者提供的图片

入门深度学习很简单。你可以在几行代码内设置和训练一个神经网络。然而，当你从初学者水平提升到中级水平时，情况可能会变得复杂。你会遇到许多新术语，如“EfficientNet”或“DeBERTa”。这些术语都是什么意思？它们与神经网络有什么关系？

初学者教程没有告诉你，实际上只有少数人会从头训练神经网络。这是因为神经网络在足够大的数据集上训练需要大量时间和计算资源。因此，通常会将预训练模型作为起点。这种做法被称为迁移学习 [2]。

> 初学者教程没有告诉你，实际上只有少数人会从头训练神经网络。

## 先决条件

本文将指导你从初学者水平提升到中级水平，涉及深度学习领域。因此，它假设你已经对深度学习有一些基本了解……
