# 《神经网络简要介绍：一个分类问题》

> 原文：[https://towardsdatascience.com/a-brief-introduction-to-neural-networks-a-classification-problem-43e68c770081?source=collection_archive---------4-----------------------#2023-01-24](https://towardsdatascience.com/a-brief-introduction-to-neural-networks-a-classification-problem-43e68c770081?source=collection_archive---------4-----------------------#2023-01-24)

## 《Python中神经网络的实用入门指南》

[](https://medium.com/@chimso1994?source=post_page-----43e68c770081--------------------------------)[![Chayma Zatout](../Images/341c45f53ddf73dc0851d547cc7cb55a.png)](https://medium.com/@chimso1994?source=post_page-----43e68c770081--------------------------------)[](https://towardsdatascience.com/?source=post_page-----43e68c770081--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----43e68c770081--------------------------------) [Chayma Zatout](https://medium.com/@chimso1994?source=post_page-----43e68c770081--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7da1c34b82e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-introduction-to-neural-networks-a-classification-problem-43e68c770081&user=Chayma+Zatout&userId=f7da1c34b82e&source=post_page-f7da1c34b82e----43e68c770081---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----43e68c770081--------------------------------) ·18分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F43e68c770081&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-introduction-to-neural-networks-a-classification-problem-43e68c770081&user=Chayma+Zatout&userId=f7da1c34b82e&source=-----43e68c770081---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F43e68c770081&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-introduction-to-neural-networks-a-classification-problem-43e68c770081&source=-----43e68c770081---------------------bookmark_footer-----------)![](../Images/c8d201223863da346293e2b716990b27.png)

图片由 [W T](https://unsplash.com/ja/@goodfunlover?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在一个[先前的教程](https://medium.com/towards-data-science/a-brief-introduction-to-neural-networks-a-regression-problem-c58c26e18008#6dba)中，我介绍了神经网络的基础知识，并提供了一个使用神经网络进行回归问题的简单示例。我简要概述了使用神经网络的总体过程。在本教程中，我们将深入学习如何使用神经网络进行分类任务。我们将遵循与之前相同的一般流程。然而，如果你需要更多关于神经网络的背景信息，我建议你查看之前的教程，在其中我也简要讨论了神经元和多层网络的概念。

**目录**

· [1\. 介绍](#cc0d)

· [2\. 问题理解](#69f1)

· [3\. 数据准备和预处理](#cc9e)

∘ [3.1\. 数据描述](#1162)

∘ [3.2\. 数据转换](#ebb1)

· [4\. 模型构思](#e1d7)

∘ [4.1\. 单一单元输出](#1a1c)

∘ [4.2\. 一热输出](#288a)

∘ [4.3\. 卷积神经网络](#692e)

· [5\. 训练](#0fb6)

∘ [5.1\. 单一单元输出](#c474)

∘ [5.2\. 一热输出](#17aa)

∘ [5.3\. 卷积单元](#56e6)

· [6\. 验证](#2608)

∘ [6.1 预测](#d7f1)

∘ [6.2\. 学习曲线](#39df)

∘ [6.3\. 测试集上的评价](#4c07)

∘ [6.4\. 评价指标](#2f91)

∘ [6.5\. 显示一些数据](#5a16)

· [7\. 结论](#5151)
