# 从基本门到深度神经网络：权威感知机教程

> 原文：[`towardsdatascience.com/the-definitive-perceptron-guide-fd384eb93382?source=collection_archive---------2-----------------------#2023-04-28`](https://towardsdatascience.com/the-definitive-perceptron-guide-fd384eb93382?source=collection_archive---------2-----------------------#2023-04-28)

## 达到人工智能精通

## 数学、二分类、逻辑门等

[](https://jvision.medium.com/?source=post_page-----fd384eb93382--------------------------------)![Joseph Robinson, Ph.D.](https://jvision.medium.com/?source=post_page-----fd384eb93382--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fd384eb93382--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd384eb93382--------------------------------) [Joseph Robinson, Ph.D.](https://jvision.medium.com/?source=post_page-----fd384eb93382--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8049fa781539&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-definitive-perceptron-guide-fd384eb93382&user=Joseph+Robinson%2C+Ph.D.&userId=8049fa781539&source=post_page-8049fa781539----fd384eb93382---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd384eb93382--------------------------------) · 21 分钟阅读 · 2023 年 4 月 28 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffd384eb93382&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-definitive-perceptron-guide-fd384eb93382&user=Joseph+Robinson%2C+Ph.D.&userId=8049fa781539&source=-----fd384eb93382---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffd384eb93382&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-definitive-perceptron-guide-fd384eb93382&source=-----fd384eb93382---------------------bookmark_footer-----------)

## TL;DR

感知机的世界令人着迷，因为这些模型是现代人工智能的基础。在这篇博客文章中，我们将提供感知机故事的简明版本，从其神经网络起源到其演变为多层感知机及其以后的发展。我们将探讨支持这一模型的基本数学，使其能够作为二分类器、模拟计算机晶体管、乘法器和逻辑门来工作。此外，我们还将研究感知机模型如何为更高级的分类器奠定基础，包括逻辑回归、SVM 和深度学习。我们将提供示例代码片段和插图以增强我们的理解。此外，我们将通过实际用例来理解何时以及如何使用感知机模型。

本指南是任何对数据科学感兴趣的人的宝贵资源，无论他们的专业水平如何。我们将探索感知机模型，这一模型自人工智能早期以来一直存在，并且至今仍然具有相关性。我们将深入了解其历史、工作原理以及与其他模型的比较。此外，我们将构建模型和逻辑门，并提供对未来可能发展的见解。无论你是自学成才的数据科学家，还是人工智能爱好者…
