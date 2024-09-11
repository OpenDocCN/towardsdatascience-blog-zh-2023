# SQL 单元测试概况：2023

> 原文：[https://towardsdatascience.com/the-sql-unit-testing-landscape-2023-7a8c5f986dd3?source=collection_archive---------6-----------------------#2023-05-02](https://towardsdatascience.com/the-sql-unit-testing-landscape-2023-7a8c5f986dd3?source=collection_archive---------6-----------------------#2023-05-02)

## 提升 SQL 开发的速度和安全性

[](https://medium.com/@cisenbe?source=post_page-----7a8c5f986dd3--------------------------------)[![Chad Isenberg](../Images/56e50c1ee292ac672df4b8062e460c8e.png)](https://medium.com/@cisenbe?source=post_page-----7a8c5f986dd3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a8c5f986dd3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7a8c5f986dd3--------------------------------) [Chad Isenberg](https://medium.com/@cisenbe?source=post_page-----7a8c5f986dd3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9113837f160&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-sql-unit-testing-landscape-2023-7a8c5f986dd3&user=Chad+Isenberg&userId=b9113837f160&source=post_page-b9113837f160----7a8c5f986dd3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a8c5f986dd3--------------------------------) · 9 分钟阅读 · 2023年5月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7a8c5f986dd3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-sql-unit-testing-landscape-2023-7a8c5f986dd3&user=Chad+Isenberg&userId=b9113837f160&source=-----7a8c5f986dd3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7a8c5f986dd3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-sql-unit-testing-landscape-2023-7a8c5f986dd3&source=-----7a8c5f986dd3---------------------bookmark_footer-----------)![](../Images/7df8aad2d9bf6732036f59b3df2c4a19.png)

照片由 [Ilse Orsel](https://unsplash.com/@lgtts?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# SQL 开发的演变

尽管SQL几乎有[50年历史](https://en.wikipedia.org/wiki/SQL)（或者说*正因为*它接近50年历史），该语言的发展在采用现代实践和工具方面一直较慢。几十年来，我们的主要“IDE”是像[SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)和[DBeaver](https://dbeaver.io/)这样的管理工具，版本控制通过像*stored_proc_2022_03_15*这样的命名约定来完成，测试则是将查询运行在生产表中，然后导出到电子表格中进行手动核对。

但在过去十年里，情况迅速改善。这些数据库管理工具以及数据仓库的云控制台功能丰富，具备语法高亮、代码补全和计划可视化。我们通过[Liquibase](https://docs.liquibase.com/home.html)、[Flyway](https://documentation.red-gate.com/fd/welcome-to-flyway-184127914.html)和[dbt](https://docs.getdbt.com/docs/introduction)等工具实现了数据库版本控制。即使是测试/审计，也因为像[dbt-audit-helper](https://github.com/dbt-labs/dbt-audit-helper)这样的工具变得不那么麻烦了。

SQL [单元测试](https://en.wikipedia.org/wiki/Unit_testing)是最后的前沿之一，它仍处于起步阶段。让我们深入了解一下！

# 我们为什么要进行单元测试？

对于那些从软件工程之外进入SQL开发领域的人来说，单元测试的价值可能不会立刻显现。通常，查询…
