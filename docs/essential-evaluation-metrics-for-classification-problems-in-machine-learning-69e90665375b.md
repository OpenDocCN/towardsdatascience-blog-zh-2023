# 机器学习中分类问题的关键评估指标

> 原文：[https://towardsdatascience.com/essential-evaluation-metrics-for-classification-problems-in-machine-learning-69e90665375b?source=collection_archive---------6-----------------------#2023-03-10](https://towardsdatascience.com/essential-evaluation-metrics-for-classification-problems-in-machine-learning-69e90665375b?source=collection_archive---------6-----------------------#2023-03-10)

## 理解和评估分类模型性能的全面指南

[](https://aaron-zhu.medium.com/?source=post_page-----69e90665375b--------------------------------)[![Aaron Zhu](../Images/42e9690c4b4aad63f396b171a74e29f7.png)](https://aaron-zhu.medium.com/?source=post_page-----69e90665375b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----69e90665375b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----69e90665375b--------------------------------) [Aaron Zhu](https://aaron-zhu.medium.com/?source=post_page-----69e90665375b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffbd30d6294e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fessential-evaluation-metrics-for-classification-problems-in-machine-learning-69e90665375b&user=Aaron+Zhu&userId=fbd30d6294e5&source=post_page-fbd30d6294e5----69e90665375b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----69e90665375b--------------------------------) ·10分钟阅读·2023年3月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F69e90665375b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fessential-evaluation-metrics-for-classification-problems-in-machine-learning-69e90665375b&user=Aaron+Zhu&userId=fbd30d6294e5&source=-----69e90665375b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F69e90665375b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fessential-evaluation-metrics-for-classification-problems-in-machine-learning-69e90665375b&source=-----69e90665375b---------------------bookmark_footer-----------)![](../Images/f1067a61f9cd203144c29c260ca6388d.png)

图片来源于 [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

对于评估机器学习模型性能时使用的术语感到困惑吗？当你遇到混淆矩阵、精确度、召回率、特异性和敏感性这些术语时是否感到迷茫？好吧，不用再担心，因为在这篇博客文章中，我们将深入探讨这些评估指标，帮助你理解这些术语。

## 什么是混淆矩阵？

让我们从混淆矩阵开始。它是一个用于评估分类模型性能的表格。它包含四个值：真正例（**TP**）、真负例（**TN**）、假正例（**FP**）和假负例（**FN**）。真正例是指模型正确预测了正类，真负例是指模型正确预测了负类，假正例是指模型预测了正类但实际是负类，假负例是指模型预测了负类但实际是正类。
