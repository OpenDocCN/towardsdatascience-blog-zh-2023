# 使用梯度下降的多类逻辑回归

> 原文：[https://towardsdatascience.com/logistic-regression-a3a23e169eec?source=collection_archive---------17-----------------------#2023-03-27](https://towardsdatascience.com/logistic-regression-a3a23e169eec?source=collection_archive---------17-----------------------#2023-03-27)

## 多类逻辑回归的理论与Python实现介绍

[](https://cookieblues.medium.com/?source=post_page-----a3a23e169eec--------------------------------)[![Stefan Hrouda-Rasmussen](../Images/701ac16a5753432d493002208151d89f.png)](https://cookieblues.medium.com/?source=post_page-----a3a23e169eec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a3a23e169eec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a3a23e169eec--------------------------------) [Stefan Hrouda-Rasmussen](https://cookieblues.medium.com/?source=post_page-----a3a23e169eec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8f5e0a37037&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-a3a23e169eec&user=Stefan+Hrouda-Rasmussen&userId=b8f5e0a37037&source=post_page-b8f5e0a37037----a3a23e169eec---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a3a23e169eec--------------------------------) ·6分钟阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa3a23e169eec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-a3a23e169eec&user=Stefan+Hrouda-Rasmussen&userId=b8f5e0a37037&source=-----a3a23e169eec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa3a23e169eec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-a3a23e169eec&source=-----a3a23e169eec---------------------bookmark_footer-----------)![](../Images/119f2a6d5cc440dcfe1bdf8df9510593.png)

逻辑回归的决策边界。图片作者提供。

## 目录

本文是我将要发布的一系列文章的一部分。下方可以看到该系列的概览。

## 1\. 机器学习简介

+   [(a) 什么是机器学习？](/what-is-machine-learning-91040db474f9)

+   [(b) 机器学习中的模型选择](/model-selection-in-machine-learning-813fe2e63ec6)

+   [(c) 维度诅咒](/the-curse-of-dimensionality-5673118fe6d2)

+   [(d) 什么是贝叶斯推断？](/what-is-bayesian-inference-4eda9f9e20a6)

## 2\. 回归

+   [(a) 线性回归实际是如何工作的](/how-linear-regression-actually-works-theory-and-implementation-8d8dcae3222c)

+   [(b) 如何通过基函数和正则化改进线性回归](/how-to-improve-your-linear-regression-with-basis-functions-and-regularization-8a6fcebdc11c)

## 3\. 分类

+   [(a) 分类器概述](/overview-of-classifiers-d0a0d3eecfd1)

+   [(b) 二次判别分析 (QDA)](/quadratic-discriminant-analysis-ae55d8a8148a)

+   [(c) 线性判别分析 (LDA)](/linear-discriminant-analysis-1894bbf04359)

+   [(d) (高斯) 朴素贝叶斯](/gaussian-naive-bayes-4d2895d139a)

+   **(e) 使用梯度下降的多类逻辑回归**

## 设置和目标
