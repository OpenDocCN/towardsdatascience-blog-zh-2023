# 使用笔记本风格的工作区简化 dbt 模型开发

> 原文：[`towardsdatascience.com/streamline-dbt-model-development-with-notebook-style-workspace-eb156fe6e81?source=collection_archive---------5-----------------------#2023-06-05`](https://towardsdatascience.com/streamline-dbt-model-development-with-notebook-style-workspace-eb156fe6e81?source=collection_archive---------5-----------------------#2023-06-05)

## 互动式构建和编排数据模型

[](https://khuyentran1476.medium.com/?source=post_page-----eb156fe6e81--------------------------------)![Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----eb156fe6e81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb156fe6e81--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb156fe6e81--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----eb156fe6e81--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamline-dbt-model-development-with-notebook-style-workspace-eb156fe6e81&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----eb156fe6e81---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb156fe6e81--------------------------------) · 7 分钟阅读 · 2023 年 6 月 5 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb156fe6e81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamline-dbt-model-development-with-notebook-style-workspace-eb156fe6e81&user=Khuyen+Tran&userId=84a02493194a&source=-----eb156fe6e81---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb156fe6e81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamline-dbt-model-development-with-notebook-style-workspace-eb156fe6e81&source=-----eb156fe6e81---------------------bookmark_footer-----------)![](img/fd11a3304be743c99f498095736e978a.png)

作者提供的图片

*最初发表于* [*https://mathdatasimplified.com*](https://mathdatasimplified.com/2023/06/05/dbt-mage-interactively-build-and-orchestrate-data-models/) *于 2023 年 6 月 5 日。*

# 动机

dbt（数据构建工具）是一个强大的数据仓库内数据转换工具。

然而，它确实有一些限制，包括以下几点：

1.  **缺乏输出预览：** 使用 dbt core，无法在构建模型之前预览其输出，这可能会阻碍验证和迭代数据转换过程的能力。

1.  **特征工程的限制：**由于 SQL 是 dbt 的主要语言，在进行高级特征工程任务时存在一定的限制。要执行超出 SQL 能力范围的复杂特征工程，可能需要额外的工具或语言，如 Python。

1.  **部分 ETL 解决方案：**虽然 dbt 在数据转换方面表现出色，但它并未提供全面的端到端解决方案，如数据加载、数据提取和编排。

为了缓解这些挑战，dbt Cloud 提供了诸如开发数据模型、预览输出以及通过用户友好的基于网页的 UI 安排 dbt 作业等功能……
