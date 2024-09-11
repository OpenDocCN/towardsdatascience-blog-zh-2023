# Python 中生存分析的完整介绍

> 原文：[`towardsdatascience.com/the-complete-introduction-to-survival-analysis-in-python-7523e17737e6?source=collection_archive---------0-----------------------#2023-05-09`](https://towardsdatascience.com/the-complete-introduction-to-survival-analysis-in-python-7523e17737e6?source=collection_archive---------0-----------------------#2023-05-09)

## 理解生存分析，其在行业中的应用以及如何在 Python 中应用它

[](https://medium.com/@marcopeixeiro?source=post_page-----7523e17737e6--------------------------------)![Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----7523e17737e6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7523e17737e6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7523e17737e6--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----7523e17737e6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complete-introduction-to-survival-analysis-in-python-7523e17737e6&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----7523e17737e6---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7523e17737e6--------------------------------) ·13 分钟阅读·2023 年 5 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7523e17737e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complete-introduction-to-survival-analysis-in-python-7523e17737e6&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=-----7523e17737e6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7523e17737e6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complete-introduction-to-survival-analysis-in-python-7523e17737e6&source=-----7523e17737e6---------------------bookmark_footer-----------)![](img/2159a9f569567d8f42ea9e0b7a718133.png)

照片由[Ricky Kharawala](https://unsplash.com/@sweetmangostudios?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上拍摄。

生存分析是一个迷人的领域，但在数据科学从业者中很少被使用或提到，至少在我的经验中是如此。虽然它主要用于医疗保健行业，但生存分析也可以在各种领域中使用。

本文的目标是让您了解生存分析及其在您所在行业中的可能应用。

在这篇文章中，我们首先定义生存分析，并激发其在不同产业中的应用。然后，我们将探讨如何实现不同的生存分析算法，并学习如何评估它们。

让我们开始吧！

# 什么是生存分析？

生存分析是统计学的一个分支，它测量事件发生的预期时间持续。这个名字来源于临床研究，因为我们确实关心的是患者的生

生存分析根据应用领域的不同可能有多种名称。在工程领域，我们谈论的是*可靠性分析*和*持续时间*…
