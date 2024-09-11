# 将正确性带回机器学习

> 原文：[`towardsdatascience.com/bring-correctness-back-to-machine-learning-a56a96262f17?source=collection_archive---------7-----------------------#2023-10-13`](https://towardsdatascience.com/bring-correctness-back-to-machine-learning-a56a96262f17?source=collection_archive---------7-----------------------#2023-10-13)

## 我们是否在基于错误的假设建立我们的领域？

[](https://medium.com/@mattiadigangi?source=post_page-----a56a96262f17--------------------------------)![Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----a56a96262f17--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a56a96262f17--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a56a96262f17--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----a56a96262f17--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a5b9f193a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbring-correctness-back-to-machine-learning-a56a96262f17&user=Mattia+Di+Gangi&userId=8a5b9f193a3c&source=post_page-8a5b9f193a3c----a56a96262f17---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a56a96262f17--------------------------------) · 9 分钟阅读·2023 年 10 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa56a96262f17&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbring-correctness-back-to-machine-learning-a56a96262f17&user=Mattia+Di+Gangi&userId=8a5b9f193a3c&source=-----a56a96262f17---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa56a96262f17&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbring-correctness-back-to-machine-learning-a56a96262f17&source=-----a56a96262f17---------------------bookmark_footer-----------)![](img/f4abe69e4718d076a4a8eb6168a88bd6.png)

图片由[Andrea De Santis](https://unsplash.com/@santesson89?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍：什么是正确性？

研究论文仍然是机器学习领域传达新发现的主要方式。然而，论文结果无法重复出现的情况相当频繁，而且原因不明。

在这里，我想讨论一下研究论文作为沟通工具的优缺点。我将阐述科学和科学生产在发展集体人类知识中的角色。

最近一篇论文发现了研究出版过程中的一个漏洞。我发现这篇论文非常有说服力，我将向你展示主要观点，目的是提高对**代码正确性**在传播机器学习知识中的作用的关注。

这个问题不仅限于机器学习领域，但在这个领域中，许多研究人员缺乏强大的工程技能——并且常常试图逃避工程工作——这导致了可靠、正确、可用的工作软件的问题。

你是否从事机器学习工作，并阅读研究论文以寻找新点子？那么，这篇文章将帮助你以原则性的方式对所读内容进行更为批判性的分析。
