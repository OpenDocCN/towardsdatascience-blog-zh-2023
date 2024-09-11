# 探索股票切割问题的贪婪解决方案

> 原文：[https://towardsdatascience.com/how-bad-is-being-greedy-cdec75ce52a6?source=collection_archive---------5-----------------------#2023-08-03](https://towardsdatascience.com/how-bad-is-being-greedy-cdec75ce52a6?source=collection_archive---------5-----------------------#2023-08-03)

## 对股票切割解决方案进行快速估算

[](https://medium.com/@jarom.hulet?source=post_page-----cdec75ce52a6--------------------------------)[![Jarom Hulet](../Images/0fdeb1a2df90cccdd8f2f4b84d5e54eb.png)](https://medium.com/@jarom.hulet?source=post_page-----cdec75ce52a6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cdec75ce52a6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cdec75ce52a6--------------------------------) [Jarom Hulet](https://medium.com/@jarom.hulet?source=post_page-----cdec75ce52a6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88982a88b4e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-bad-is-being-greedy-cdec75ce52a6&user=Jarom+Hulet&userId=88982a88b4e5&source=post_page-88982a88b4e5----cdec75ce52a6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cdec75ce52a6--------------------------------) ·7分钟阅读·2023年8月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcdec75ce52a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-bad-is-being-greedy-cdec75ce52a6&user=Jarom+Hulet&userId=88982a88b4e5&source=-----cdec75ce52a6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcdec75ce52a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-bad-is-being-greedy-cdec75ce52a6&source=-----cdec75ce52a6---------------------bookmark_footer-----------)

内容

1.  股票切割问题的动机

1.  NP-Hard 问题的快速概述

1.  将股票切割问题编码为 Python

1.  贪婪算法

1.  与低维空间中的穷举搜索的比较

1.  与高维空间中的随机搜索的比较

1.  结论

**股票切割问题的动机**

我是一个数据科学家。虽然我的数据科学技能在工作中非常重要（根据定义），但我发现数据科学概念也有助于解决工作之外的许多问题！

我的数据科学技能在我的DIY/创客爱好中非常实用。一个常见的挑战是知道如何规划材料的切割。我有一份需要从多块相同尺寸的材料中进行切割的清单。如何规划这些切割以尽可能减少浪费？这个挑战被称为“库存切割问题”。事实证明，这实际上是一个非常难以解决的问题，实际上是NP-Hard！

在这篇文章中，我将探讨一种解决这个问题的“捷径”（文字游戏），并分析这种方法与传统方法的比较。
