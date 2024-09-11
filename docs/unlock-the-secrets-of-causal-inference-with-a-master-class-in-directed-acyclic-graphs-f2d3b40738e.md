# 通过有向无环图的精英课程揭开因果推断的秘密

> 原文：[https://towardsdatascience.com/unlock-the-secrets-of-causal-inference-with-a-master-class-in-directed-acyclic-graphs-f2d3b40738e?source=collection_archive---------7-----------------------#2023-04-06](https://towardsdatascience.com/unlock-the-secrets-of-causal-inference-with-a-master-class-in-directed-acyclic-graphs-f2d3b40738e?source=collection_archive---------7-----------------------#2023-04-06)

## 从基础到更高级方面的有向无环图逐步解释

[](https://grahamharrison-86487.medium.com/?source=post_page-----f2d3b40738e--------------------------------)[![Graham Harrison](../Images/c6bfe00c6e0cfcdf3bd042c7fdc03554.png)](https://grahamharrison-86487.medium.com/?source=post_page-----f2d3b40738e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2d3b40738e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f2d3b40738e--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----f2d3b40738e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd1c93739f33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-secrets-of-causal-inference-with-a-master-class-in-directed-acyclic-graphs-f2d3b40738e&user=Graham+Harrison&userId=bd1c93739f33&source=post_page-bd1c93739f33----f2d3b40738e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2d3b40738e--------------------------------) · 36 分钟阅读 · 2023年4月6日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff2d3b40738e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlock-the-secrets-of-causal-inference-with-a-master-class-in-directed-acyclic-graphs-f2d3b40738e&source=-----f2d3b40738e---------------------bookmark_footer-----------)![](../Images/a3c8c1147fdd45191a6e1fc6574c9a09.png)

由 [Caleb Jones](https://unsplash.com/es/@gcalebjones?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/s/photos/paths?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 目标

在花了大量时间研究因果推断后，我开始意识到自己对有向无环图（DAGs）的掌握不够，这阻碍了我将理解提升到能够用来解决现实世界问题的水平。

本文的目的是记录我的学习历程，并分享你需要了解的所有关于DAG（有向无环图）的知识，以便将你对因果推断的理解提升到一个新的水平。

# 背景

我想从提出因果推断的定义开始——

> **因果推断是推理过程，应用从变量之间的因果关系中得出的结论，同时考虑潜在的混杂因素和偏见。**

这确实是个长篇大论，但它确实概括了关键点——

1.  这是因果关系的研究。

1.  关键是得出可以应用的结论……
