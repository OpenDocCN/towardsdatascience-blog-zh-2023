# 理解独立性及其在因果推断和因果验证中的关键作用

> 原文：[`towardsdatascience.com/understanding-independence-and-why-it-is-critical-in-causal-inference-and-causal-validation-dfdd26c29739?source=collection_archive---------6-----------------------#2023-12-07`](https://towardsdatascience.com/understanding-independence-and-why-it-is-critical-in-causal-inference-and-causal-validation-dfdd26c29739?source=collection_archive---------6-----------------------#2023-12-07)

## 逐步指南，帮助理解独立性的概念及如何使用 Python 在因果验证中验证有向无环图

[](https://grahamharrison-86487.medium.com/?source=post_page-----dfdd26c29739--------------------------------)![Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----dfdd26c29739--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dfdd26c29739--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfdd26c29739--------------------------------) [Graham Harrison](https://grahamharrison-86487.medium.com/?source=post_page-----dfdd26c29739--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd1c93739f33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-independence-and-why-it-is-critical-in-causal-inference-and-causal-validation-dfdd26c29739&user=Graham+Harrison&userId=bd1c93739f33&source=post_page-bd1c93739f33----dfdd26c29739---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfdd26c29739--------------------------------) ·23 分钟阅读·2023 年 12 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdfdd26c29739&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-independence-and-why-it-is-critical-in-causal-inference-and-causal-validation-dfdd26c29739&user=Graham+Harrison&userId=bd1c93739f33&source=-----dfdd26c29739---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdfdd26c29739&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-independence-and-why-it-is-critical-in-causal-inference-and-causal-validation-dfdd26c29739&source=-----dfdd26c29739---------------------bookmark_footer-----------)![](img/2a1aaf170dbbf7c9485d0cd5023662a6.png)

图片由[Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)拍摄，来源于[Unsplash](https://unsplash.com/photos/a-blue-question-mark-on-a-pink-background-oZuBNC-6E2s?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 背景

在最近的一篇文章中，作者探讨并解释了如何使用依赖概念来验证提议的有向无环图（DAG）是否与数据集匹配，以识别图中的虚假边，即 DAG 建议但数据中不存在的因果链接。

在第二部分中，将应用相对（但同样关键）的概念，即如何利用独立性来识别缺失的边。这些是提议的有向无环图（DAG）中没有出现但实际存在于数据中的因果链接，必须将其重新添加到 DAG 中以使其完整且正确。

# 介绍

因果推断是数据科学中的一个新兴分支，涉及确定事件和结果之间的因果关系，它有潜力显著提升机器学习为组织带来的价值。
