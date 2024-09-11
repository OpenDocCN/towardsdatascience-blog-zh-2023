# 《Python 中的数学优化介绍》

> 原文：[`towardsdatascience.com/introduction-to-mathematical-optimisation-in-python-b88e6556f9e?source=collection_archive---------2-----------------------#2023-12-02`](https://towardsdatascience.com/introduction-to-mathematical-optimisation-in-python-b88e6556f9e?source=collection_archive---------2-----------------------#2023-12-02)

## 数据科学基础

## 初学者的 Python 离散优化实用指南

[](https://zluvsand.medium.com/?source=post_page-----b88e6556f9e--------------------------------)![Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----b88e6556f9e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b88e6556f9e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b88e6556f9e--------------------------------) [Zolzaya Luvsandorj](https://zluvsand.medium.com/?source=post_page-----b88e6556f9e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5bca2b935223&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mathematical-optimisation-in-python-b88e6556f9e&user=Zolzaya+Luvsandorj&userId=5bca2b935223&source=post_page-5bca2b935223----b88e6556f9e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b88e6556f9e--------------------------------) · 10 分钟阅读 · 2023 年 12 月 2 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb88e6556f9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mathematical-optimisation-in-python-b88e6556f9e&user=Zolzaya+Luvsandorj&userId=5bca2b935223&source=-----b88e6556f9e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb88e6556f9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mathematical-optimisation-in-python-b88e6556f9e&source=-----b88e6556f9e---------------------bookmark_footer-----------)

数据科学家使用数据和各种技术来解决广泛的现实问题。数学优化是一种强大的技术，可以应用于许多领域的广泛问题，是数据科学家工具包中的重要投资。在这篇实用的入门文章中，我们将熟悉 Python 中的三个流行优化库：Google 的[OR-Tools](https://developers.google.com/optimization)、IBM 的[DOcplex](https://www.ibm.com/docs/en/icos/12.9.0?topic=docplex-python-modeling-api)和 COIN-OR Foundation 的[PuLP](https://coin-or.github.io/pulp/)

![](img/8110e850abb6f32c794b985530fffe78.png)

图片由[Akhilesh Sharma](https://unsplash.com/@fotonium?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 📍 概述

数学优化旨在寻找*最佳选择*以解决*定量问题*，在*预定义的范围*内。它有三个组成部分：

+   **目标函数：** 告诉我们一个解决方案的好坏，并允许我们比较解决方案。最优解是根据用例最大化或最小化目标函数的解。

    ▶ ️在某些情况下，可能存在多个目标函数。这增加了确定最优解的复杂性。

    ▶ ️在某些情况下，可能没有目标函数。这类优化问题被称为可行性问题。

+   **决策变量：** 代表我们想要找出的一个或多个值，...
