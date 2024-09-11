# 使用 DuckDB 与 Polars

> 原文：[https://towardsdatascience.com/using-duckdb-with-polars-e15a865e48a3?source=collection_archive---------13-----------------------#2023-04-14](https://towardsdatascience.com/using-duckdb-with-polars-e15a865e48a3?source=collection_archive---------13-----------------------#2023-04-14)

## 了解如何使用 SQL 查询你的 Polars DataFrames

[](https://weimenglee.medium.com/?source=post_page-----e15a865e48a3--------------------------------)[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----e15a865e48a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e15a865e48a3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e15a865e48a3--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----e15a865e48a3--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-duckdb-with-polars-e15a865e48a3&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----e15a865e48a3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e15a865e48a3--------------------------------) ·6 分钟阅读·2023年4月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe15a865e48a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-duckdb-with-polars-e15a865e48a3&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----e15a865e48a3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe15a865e48a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-duckdb-with-polars-e15a865e48a3&source=-----e15a865e48a3---------------------bookmark_footer-----------)![](../Images/cbb9711706d8c7fc20b5950a3119d9d5.png)

图片来源于 [Hans-Jurgen Mager](https://unsplash.com/@hansjurgen007?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我之前几篇关于数据分析的文章中，我讨论了两个在业界越来越受关注的重要新兴库：

+   **DuckDB** — 你可以使用 SQL 语句在内存中查询数据集。

+   **Polars** — 与历史悠久的 Pandas 库相比，更高效的 DataFrame 库。

[](https://levelup.gitconnected.com/using-duckdb-for-data-analytics-bab3e3ff032c?source=post_page-----e15a865e48a3--------------------------------) [## 使用 DuckDB 进行数据分析

### 了解如何使用 SQL 执行数据分析

[levelup.gitconnected.com](https://levelup.gitconnected.com/using-duckdb-for-data-analytics-bab3e3ff032c?source=post_page-----e15a865e48a3--------------------------------) [](/getting-started-with-the-polars-dataframe-library-6f9e1c014c5c?source=post_page-----e15a865e48a3--------------------------------) [## 开始使用 Polars 数据帧库

### 学习如何使用 Polars 数据帧库操作表格数据（并替代 Pandas）

[towardsdatascience.com](/getting-started-with-the-polars-dataframe-library-6f9e1c014c5c?source=post_page-----e15a865e48a3--------------------------------)

那么，结合这两个库的力量怎么样？

> 实际上，你可以通过 DuckDB 直接查询 Polars 数据帧，使用 SQL 语句。

那么，使用 SQL 查询 Polars 数据帧的好处是什么？尽管使用方便，但操作 Polars 数据帧仍需一些...
