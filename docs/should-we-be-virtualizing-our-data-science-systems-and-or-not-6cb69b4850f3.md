# 我们是否应该虚拟化我们的数据科学系统 — 或者不虚拟化？

> 原文：[https://towardsdatascience.com/should-we-be-virtualizing-our-data-science-systems-and-or-not-6cb69b4850f3?source=collection_archive---------5-----------------------#2023-09-12](https://towardsdatascience.com/should-we-be-virtualizing-our-data-science-systems-and-or-not-6cb69b4850f3?source=collection_archive---------5-----------------------#2023-09-12)

![](../Images/5f30971c4f4c248f248ced63c885dacc.png)

作者当前的家庭实验室设置

## 在虚拟化数据科学流程的优缺点上可能很难进行导航，但某些性能和能力的趋势是无法忽视的

[](https://medium.com/@willkeefe?source=post_page-----6cb69b4850f3--------------------------------)[![Will Keefe](../Images/e26b5d394bd268f2b7e333d4b45711eb.png)](https://medium.com/@willkeefe?source=post_page-----6cb69b4850f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6cb69b4850f3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6cb69b4850f3--------------------------------) [Will Keefe](https://medium.com/@willkeefe?source=post_page-----6cb69b4850f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F87087a3fef0b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-we-be-virtualizing-our-data-science-systems-and-or-not-6cb69b4850f3&user=Will+Keefe&userId=87087a3fef0b&source=post_page-87087a3fef0b----6cb69b4850f3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6cb69b4850f3--------------------------------) ·12 min read·2023年9月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6cb69b4850f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-we-be-virtualizing-our-data-science-systems-and-or-not-6cb69b4850f3&user=Will+Keefe&userId=87087a3fef0b&source=-----6cb69b4850f3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6cb69b4850f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-we-be-virtualizing-our-data-science-systems-and-or-not-6cb69b4850f3&source=-----6cb69b4850f3---------------------bookmark_footer-----------)

随着“巨大数据”的利用在各行各业中的问题解决中变得越来越重要，无论是家用实验室数据仓库还是数据湖规模的数据仓库，都需要比以往更多的并行计算能力来提取、转换、加载和分析数据。在创建自己的家用实验室时，决定是通过虚拟机还是直接在硬件上创建并行设置让我感到困惑，我很难找到性能比较。在本文中，我们将探讨每种设置的一些优缺点，并对每种方法论（无论是虚拟的还是本地的）进行并排性能和基准测试。

# 介绍

许多并行计算集群包括多个节点，即指定用于处理分布在集群中的任务的计算机。管理这些节点可能是一个主要的难题，这也是为什么[数据工程如此有利可图](https://www.thinkful.com/blog/data-analyst-vs-data-engineer/#:~:text=Data%20Analysis%20or%20Data%20Engineering,hand%2C%20is%20%24112%2C288%20a%20year.)，相比于它们的分析同行。通常，公司会管理整个集群队伍，这使得几乎不可能对每个节点给予单独关注，而是“高……
