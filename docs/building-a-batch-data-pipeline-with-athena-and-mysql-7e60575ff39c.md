# 使用 Athena 和 MySQL 构建批量数据管道

> 原文：[`towardsdatascience.com/building-a-batch-data-pipeline-with-athena-and-mysql-7e60575ff39c?source=collection_archive---------6-----------------------#2023-10-20`](https://towardsdatascience.com/building-a-batch-data-pipeline-with-athena-and-mysql-7e60575ff39c?source=collection_archive---------6-----------------------#2023-10-20)

## 初学者的端到端教程

[](https://mshakhomirov.medium.com/?source=post_page-----7e60575ff39c--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----7e60575ff39c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7e60575ff39c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7e60575ff39c--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----7e60575ff39c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-batch-data-pipeline-with-athena-and-mysql-7e60575ff39c&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----7e60575ff39c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7e60575ff39c--------------------------------) ·16 分钟阅读·2023 年 10 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7e60575ff39c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-batch-data-pipeline-with-athena-and-mysql-7e60575ff39c&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----7e60575ff39c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7e60575ff39c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-batch-data-pipeline-with-athena-and-mysql-7e60575ff39c&source=-----7e60575ff39c---------------------bookmark_footer-----------)![](img/368293b91e4bc0283007a555789b6479.png)

照片由 [Redd F](https://unsplash.com/@raddfilms?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这个故事中，我将讲述一种最受欢迎的数据转换任务运行方式——批量数据处理。当我们需要分块处理数据时，这种数据管道设计模式变得非常有用，使其在需要调度的 ETL 任务中非常高效。我将演示如何通过使用 MySQL 和 Athena 构建数据转换管道来实现这一点。我们将使用基础设施即代码在云中部署它。

想象一下你刚刚加入一家公司担任数据工程师。他们的数据栈现代、事件驱动、成本效益高、灵活，并且可以轻松扩展以满足不断增长的数据资源。数据平台中的外部数据源和数据管道由数据工程团队管理，使用具有 CI/CD GitHub 集成的灵活环境设置。

作为数据工程师，你需要创建一个业务智能仪表盘，展示公司的收入流地理分布，如下所示。原始支付数据存储在服务器数据库（MySQL）中。你想建立一个批处理管道，每天从数据库中提取数据，然后使用 AWS S3 存储数据文件，并用 Athena 处理这些数据。
