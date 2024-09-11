# 计算机视觉的数据策展指南

> 原文：[`towardsdatascience.com/data-curation-guide-for-computer-vision-acc525f4cd7?source=collection_archive---------10-----------------------#2023-03-09`](https://towardsdatascience.com/data-curation-guide-for-computer-vision-acc525f4cd7?source=collection_archive---------10-----------------------#2023-03-09)

## **计算机视觉中的数据策展缺乏标准化，导致许多从业者不确定如何正确进行。我们总结了一些最常见的方法。**

[](https://medium.com/@isusmelj?source=post_page-----acc525f4cd7--------------------------------)![伊戈尔·苏斯梅尔](https://medium.com/@isusmelj?source=post_page-----acc525f4cd7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----acc525f4cd7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----acc525f4cd7--------------------------------) [伊戈尔·苏斯梅尔](https://medium.com/@isusmelj?source=post_page-----acc525f4cd7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa20329809ac8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-curation-guide-for-computer-vision-acc525f4cd7&user=Igor+Susmelj&userId=a20329809ac8&source=post_page-a20329809ac8----acc525f4cd7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----acc525f4cd7--------------------------------) ·10 分钟阅读·2023 年 3 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Facc525f4cd7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-curation-guide-for-computer-vision-acc525f4cd7&user=Igor+Susmelj&userId=a20329809ac8&source=-----acc525f4cd7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Facc525f4cd7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-curation-guide-for-computer-vision-acc525f4cd7&source=-----acc525f4cd7---------------------bookmark_footer-----------)![](img/f27f307fd92fa83752779cc56ea32a2f.png)

从数据策展开始？瓦伦丁·安东努奇的照片：[`www.pexels.com/de-de/foto/person-die-kompass-halt-691637/`](https://www.pexels.com/de-de/foto/person-die-kompass-halt-691637/)

本文是针对计算机视觉的数据策展编写的。然而，多个概念也适用于其他数据领域，如自然语言处理（NLP）、音频或表格数据。

# 什么是数据策展？

数据策划是一个在行业中广泛使用的术语，特别是在[数据驱动的人工智能](https://datacentricai.org/)领域。理解数据策划在机器学习背景下的组成部分非常重要。我们理解数据策划的以下组成部分：

+   **数据清洗和标准化** — 移除“损坏”样本或尝试纠正它们的过程

+   **数据选择** — 根据特定任务的重要性对数据进行排序的过程

## 机器学习中的数据清洗

理解**结构化数据的数据清洗**的最简单方法是考虑表格数据。假设你在银行做一个项目，想要根据客户的来源分析他们的支出。你的数据在一个 CSV 文件中，你发现…
