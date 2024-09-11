# Scikit-LLM：在 scikit-learn 框架中利用 LLMs 提升 Python 的文本分析能力

> 原文：[https://towardsdatascience.com/scikit-llm-power-up-your-text-analysis-in-python-using-llm-models-within-scikit-learn-framework-e9f101ffb6d4?source=collection_archive---------3-----------------------#2023-06-06](https://towardsdatascience.com/scikit-llm-power-up-your-text-analysis-in-python-using-llm-models-within-scikit-learn-framework-e9f101ffb6d4?source=collection_archive---------3-----------------------#2023-06-06)

## 实操教程，Python 库

## 使用高级语言模型如 ChatGPT 执行文本分类任务，如情感分析、文本总结及其他文本分析任务

[](https://medium.ealizadeh.com/?source=post_page-----e9f101ffb6d4--------------------------------)[![Essi Alizadeh](../Images/be2244231732f93bcadf09682ef8ca37.png)](https://medium.ealizadeh.com/?source=post_page-----e9f101ffb6d4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e9f101ffb6d4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e9f101ffb6d4--------------------------------) [Essi Alizadeh](https://medium.ealizadeh.com/?source=post_page-----e9f101ffb6d4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F35a932e89ec1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscikit-llm-power-up-your-text-analysis-in-python-using-llm-models-within-scikit-learn-framework-e9f101ffb6d4&user=Essi+Alizadeh&userId=35a932e89ec1&source=post_page-35a932e89ec1----e9f101ffb6d4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e9f101ffb6d4--------------------------------) ·10 分钟阅读·2023年6月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe9f101ffb6d4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscikit-llm-power-up-your-text-analysis-in-python-using-llm-models-within-scikit-learn-framework-e9f101ffb6d4&user=Essi+Alizadeh&userId=35a932e89ec1&source=-----e9f101ffb6d4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe9f101ffb6d4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscikit-llm-power-up-your-text-analysis-in-python-using-llm-models-within-scikit-learn-framework-e9f101ffb6d4&source=-----e9f101ffb6d4---------------------bookmark_footer-----------)![](../Images/828eb6a079b92568a3d0c1e2344e2ed1.png)

照片由 [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

[Scikit-LLM](https://github.com/iryna-kondr/scikit-llm) 是一个 Python 包，它将像 OpenAI 的 GPT-3 这样的 **大型语言模型（LLMs）** 集成到 [scikit-learn](https://scikit-learn.org/) 框架中，用于文本分析任务。

Scikit-LLM 旨在与 scikit-learn 框架兼容。因此，如果你熟悉 scikit-learn，你会发现 scikit-llm 非常得心应手。该库提供了一系列功能，其中我们将涵盖以下内容：

+   零样本文本分类

+   多标签零样本文本分类

+   文本向量化

+   文本翻译

+   文本摘要

![](../Images/63233c936c361da65586d05a1ba72051.png)

本文涵盖的概念（图片来源：作者）

> 请注意，本文中使用的所有数据集均为独家……
