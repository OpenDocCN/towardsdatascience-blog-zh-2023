# XGBoost：简介、逐步实现及性能比较

> 原文：[`towardsdatascience.com/xgboost-intro-step-by-step-implementation-and-performance-comparison-6018dfa212f3?source=collection_archive---------4-----------------------#2023-09-29`](https://towardsdatascience.com/xgboost-intro-step-by-step-implementation-and-performance-comparison-6018dfa212f3?source=collection_archive---------4-----------------------#2023-09-29)

## 极端梯度提升：快速且可靠的回归器和分类器

[](https://medium.com/@fmnobar?source=post_page-----6018dfa212f3--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----6018dfa212f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6018dfa212f3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6018dfa212f3--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----6018dfa212f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-intro-step-by-step-implementation-and-performance-comparison-6018dfa212f3&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----6018dfa212f3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6018dfa212f3--------------------------------) · 12 分钟阅读 · 2023 年 9 月 29 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6018dfa212f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-intro-step-by-step-implementation-and-performance-comparison-6018dfa212f3&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----6018dfa212f3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6018dfa212f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-intro-step-by-step-implementation-and-performance-comparison-6018dfa212f3&source=-----6018dfa212f3---------------------bookmark_footer-----------)![](img/fcc4ec2ab542f20d005332a0413e63d0.png)

图片由 [Sam Moghadam Khamseh](https://unsplash.com/@sammoghadamkhamseh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，拍摄于 [Unsplash](https://unsplash.com/photos/baII27W6z7k?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

XGBoost 已成为所有机器学习从业者中最受欢迎的全面回归器和/或分类器之一。如果你问数据科学家在没有其他信息的情况下会选择什么模型，几率很大他们会选择 XGBoost，因为它可以应用于各种类型的用例——它快速、可靠、多功能且非常容易实现。

今天，我们将概念性地回顾 XGBoost，详细讲解其实现过程，并将其性能与其他几种模型在分类任务中的表现进行比较。

让我们开始吧！

# 1. XGBoost — 概念概述

XGBoost 代表极端梯度提升。它是一种梯度提升决策树模型，既可用于有监督回归，也可用于分类任务。我们使用了几个术语来定义 XGBoost，因此让我们逐一探讨这些术语，以便更好地理解它们。我将提供每个概念的简要概述，并为那些对深入讨论感兴趣的人提供更多详细内容的链接。

+   **有监督学习 vs. 无监督学习：** 有监督学习…
