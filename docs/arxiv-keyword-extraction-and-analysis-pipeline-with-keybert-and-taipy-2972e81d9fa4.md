# arXiv 关键词提取与分析管道，使用 KeyBERT 和 Taipy

> 原文：[`towardsdatascience.com/arxiv-keyword-extraction-and-analysis-pipeline-with-keybert-and-taipy-2972e81d9fa4?source=collection_archive---------11-----------------------#2023-04-18`](https://towardsdatascience.com/arxiv-keyword-extraction-and-analysis-pipeline-with-keybert-and-taipy-2972e81d9fa4?source=collection_archive---------11-----------------------#2023-04-18)

## 构建一个关键词分析的 Python 应用程序，包括前端用户界面和后端管道

[](https://kennethleungty.medium.com/?source=post_page-----2972e81d9fa4--------------------------------)![Kenneth Leung](https://kennethleungty.medium.com/?source=post_page-----2972e81d9fa4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2972e81d9fa4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2972e81d9fa4--------------------------------) [Kenneth Leung](https://kennethleungty.medium.com/?source=post_page-----2972e81d9fa4--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdcd08e36f2d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Farxiv-keyword-extraction-and-analysis-pipeline-with-keybert-and-taipy-2972e81d9fa4&user=Kenneth+Leung&userId=dcd08e36f2d0&source=post_page-dcd08e36f2d0----2972e81d9fa4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2972e81d9fa4--------------------------------) ·12 分钟阅读·2023 年 4 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2972e81d9fa4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Farxiv-keyword-extraction-and-analysis-pipeline-with-keybert-and-taipy-2972e81d9fa4&user=Kenneth+Leung&userId=dcd08e36f2d0&source=-----2972e81d9fa4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2972e81d9fa4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Farxiv-keyword-extraction-and-analysis-pipeline-with-keybert-and-taipy-2972e81d9fa4&source=-----2972e81d9fa4---------------------bookmark_footer-----------)![](img/177bb91daf175985fe9b1a3705329593.png)

照片由 [Marylou Fortier](https://unsplash.com/@rylouma?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/photos/heNLI144X7Y?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随着社交媒体、客户评价和在线平台等来源的文本数据数量呈指数级增长，我们必须能够理解这些非结构化数据。

关键词提取和分析是强大的自然语言处理（NLP）技术，使我们能够实现这一目标。

**关键词提取**涉及自动识别和提取给定文本中最相关的词汇，而**关键词分析**则是对这些关键词进行分析，以获取潜在模式的洞见。

在这个逐步指南中，我们将探讨如何利用**KeyBERT**和**Taipy**这两个强大的工具，构建一个关键词提取和分析管道及网页应用，基于 arXiv 摘要。

## 目录

> ***(1)*** *背景****(2)*** *工具概述****(3)*** *逐步指南****(4)*** *总结*

这是本文的[GitHub 仓库](https://github.com/kennethleungty/Keyword-Analysis-with-KeyBERT-and-Taipy)。

# (1) 背景

鉴于人工智能（AI）和机器学习研究的快速进展，跟踪众多论文...
