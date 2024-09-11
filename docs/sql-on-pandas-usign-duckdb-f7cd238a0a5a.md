# SQL 在 Pandas 中 — 我新的 10 倍速度最爱。

> 原文：[https://towardsdatascience.com/sql-on-pandas-usign-duckdb-f7cd238a0a5a?source=collection_archive---------0-----------------------#2023-03-23](https://towardsdatascience.com/sql-on-pandas-usign-duckdb-f7cd238a0a5a?source=collection_archive---------0-----------------------#2023-03-23)

## 融合两全其美的解决方案

[](https://thuwarakesh.medium.com/?source=post_page-----f7cd238a0a5a--------------------------------)[![Thuwarakesh Murallie](../Images/44f1a14a899426592bbd8c7f73ce169d.png)](https://thuwarakesh.medium.com/?source=post_page-----f7cd238a0a5a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f7cd238a0a5a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f7cd238a0a5a--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f7cd238a0a5a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-on-pandas-usign-duckdb-f7cd238a0a5a&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----f7cd238a0a5a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7cd238a0a5a--------------------------------) ·5 分钟阅读·2023年3月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff7cd238a0a5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-on-pandas-usign-duckdb-f7cd238a0a5a&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----f7cd238a0a5a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff7cd238a0a5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-on-pandas-usign-duckdb-f7cd238a0a5a&source=-----f7cd238a0a5a---------------------bookmark_footer-----------)![](../Images/c3fe425569b86a13bbb8b061d74d222e.png)

图片由 [Akira Hojo](https://unsplash.com/@joephotography?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

我热衷于寻找加速分析任务的方法。

我过去写过几篇关于测量内存使用、运行时间以及加速数据密集型任务的文章。由于我们主要每天使用 Pandas，我不断研究如何提高性能。

[](/fuzzy-string-matching-in-pandas-2c185a24617f?source=post_page-----f7cd238a0a5a--------------------------------) [## 如何在 Pandas Dataframe 中进行模糊字符串匹配

### 匹配没有完美匹配的文本

[towardsdatascience.com](/fuzzy-string-matching-in-pandas-2c185a24617f?source=post_page-----f7cd238a0a5a--------------------------------)

这个令人印象深刻的 Python 库功能丰富且可靠。但速度不是它的超级能力之一。通过一些优化，我们可以提升速度。但这些技巧也有它们的问题。

[](/3-important-sql-optimization-technique-d6da3e9c8442?source=post_page-----f7cd238a0a5a--------------------------------) [## 3 种 SQL 优化技术，可以立即提升查询速度

### 在完全更换数据模型之前可以尝试的一些简单技巧

[towardsdatascience.com](/3-important-sql-optimization-technique-d6da3e9c8442?source=post_page-----f7cd238a0a5a--------------------------------)

新的替代品，如 Polars，也在不断获得关注。但我仍然像许多其他数据专业人士一样更喜欢 Pandas。

这也是我们使用数据库而不是内存中的数据框的原因之一。数据库的设计是…
