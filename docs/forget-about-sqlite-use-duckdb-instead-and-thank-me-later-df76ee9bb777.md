# 忘掉SQLite，使用DuckDB——然后感谢我

> 原文：[https://towardsdatascience.com/forget-about-sqlite-use-duckdb-instead-and-thank-me-later-df76ee9bb777?source=collection_archive---------0-----------------------#2023-03-16](https://towardsdatascience.com/forget-about-sqlite-use-duckdb-instead-and-thank-me-later-df76ee9bb777?source=collection_archive---------0-----------------------#2023-03-16)

## DuckDB及其Python集成介绍

[](https://polmarin.medium.com/?source=post_page-----df76ee9bb777--------------------------------)[![Pol Marin](../Images/a4f69a96717d453db9791f27b8f85e86.png)](https://polmarin.medium.com/?source=post_page-----df76ee9bb777--------------------------------)[](https://towardsdatascience.com/?source=post_page-----df76ee9bb777--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----df76ee9bb777--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----df76ee9bb777--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforget-about-sqlite-use-duckdb-instead-and-thank-me-later-df76ee9bb777&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----df76ee9bb777---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----df76ee9bb777--------------------------------) ·8分钟阅读·2023年3月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdf76ee9bb777&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforget-about-sqlite-use-duckdb-instead-and-thank-me-later-df76ee9bb777&user=Pol+Marin&userId=1fa43cc443e7&source=-----df76ee9bb777---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdf76ee9bb777&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforget-about-sqlite-use-duckdb-instead-and-thank-me-later-df76ee9bb777&source=-----df76ee9bb777---------------------bookmark_footer-----------)![](../Images/c7283a6d2ba1b4298e72e882280c3f4b.png)

照片由[Krzysztof Niewolny](https://unsplash.com/@epan5?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们程序员在处理本地环境中的嵌入式数据库时，倾向于默认使用SQLite。虽然这在大多数情况下运行良好，但就像用自行车去旅行100公里：可能不是最佳选择。

介绍DuckDB。

我第一次了解 DuckDB 是在 2022 年 9 月的西班牙 PyCon 大会上的格拉纳达。现在，经过 6 个月的使用，我已经离不开它。我希望通过为我的编程同仁和数据相关专业人士提供这个出色的分析数据库系统的介绍来为社区做出贡献。

在这篇文章中，我将介绍以下主要内容：

+   DuckDB 介绍：它是什么，为什么应该使用它，以及什么时候使用。

+   DuckDB 如何集成到 Python 中。

准备好！

> 如果你看不到完整的故事，可以考虑使用我的推荐链接以无限访问所有 Medium 的故事：[https://medium.com/@polmarin/membership](https://medium.com/@polmarin/membership)

## 什么是 DuckDB？

如果你查看 DuckDB 的官网[1]，你会在主页上看到的第一件事是：*DuckDB 是一个内嵌的 SQL OLAP 数据库管理系统。*
