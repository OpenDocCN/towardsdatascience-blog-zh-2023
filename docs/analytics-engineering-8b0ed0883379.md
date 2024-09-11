# 数据分析工程简介

> 原文：[`towardsdatascience.com/analytics-engineering-8b0ed0883379?source=collection_archive---------1-----------------------#2023-10-22`](https://towardsdatascience.com/analytics-engineering-8b0ed0883379?source=collection_archive---------1-----------------------#2023-10-22)

## 谁是分析工程师，他们应该做些什么

[](https://gmyrianthous.medium.com/?source=post_page-----8b0ed0883379--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----8b0ed0883379--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b0ed0883379--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b0ed0883379--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----8b0ed0883379--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalytics-engineering-8b0ed0883379&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----8b0ed0883379---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b0ed0883379--------------------------------) ·6 min read·2023 年 10 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8b0ed0883379&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalytics-engineering-8b0ed0883379&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----8b0ed0883379---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8b0ed0883379&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalytics-engineering-8b0ed0883379&source=-----8b0ed0883379---------------------bookmark_footer-----------)![](img/94ed22629f1f62ce0015550806f265cf.png)

图像生成 via [DALL-E2](https://labs.openai.com/s/Ibq51s6cLfdgFn68sk94aHLF)

传统上，数据团队由数据工程师和数据分析师组成。

数据工程师负责建立支持数据操作的基础设施。这包括数据库的配置和实施 ETL 过程，以将数据从外部来源导入目标系统（可能是另一个数据库）。此外，数据工程师通常负责确保数据的完整性、新鲜度和安全性，以便分析师可以查询数据。数据工程师的典型技能包括 Python（或 Java）、SQL、协调（使用如 Apache Airflow 的工具）和数据建模。

另一方面，数据分析师应使用 Excel 或 SQL 构建仪表板和报告，以向内部用户和部门提供业务见解。

![](img/dc94913ce71ac6ef8b9ca0cb68e2e7f6.png)

传统的数据团队构建方式

# 从 ETL 到 ELT 的过渡

为了处理数据并获得有价值的见解，我们首先需要提取数据，对吗？🤯
