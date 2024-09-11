# 如何使用 AWS Lambda 为数据科学设置一个简单的 ETL 管道

> 原文：[`towardsdatascience.com/how-to-setup-a-simple-etl-pipeline-with-aws-lambda-for-data-science-89e5b96c7017?source=collection_archive---------4-----------------------#2023-02-04`](https://towardsdatascience.com/how-to-setup-a-simple-etl-pipeline-with-aws-lambda-for-data-science-89e5b96c7017?source=collection_archive---------4-----------------------#2023-02-04)

## 如何使用 AWS Lambda 设置一个简单的 ETL 管道，该管道可以通过 API 端点或计划触发，并将结果写入 S3 存储桶以便后续处理

[](https://medium.com/@broepke?source=post_page-----89e5b96c7017--------------------------------)![Brian Roepke](https://medium.com/@broepke?source=post_page-----89e5b96c7017--------------------------------)[](https://towardsdatascience.com/?source=post_page-----89e5b96c7017--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----89e5b96c7017--------------------------------) [Brian Roepke](https://medium.com/@broepke?source=post_page-----89e5b96c7017--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff5a92cac16d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-setup-a-simple-etl-pipeline-with-aws-lambda-for-data-science-89e5b96c7017&user=Brian+Roepke&userId=f5a92cac16d6&source=post_page-f5a92cac16d6----89e5b96c7017---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----89e5b96c7017--------------------------------) ·10 分钟阅读·2023 年 2 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F89e5b96c7017&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-setup-a-simple-etl-pipeline-with-aws-lambda-for-data-science-89e5b96c7017&user=Brian+Roepke&userId=f5a92cac16d6&source=-----89e5b96c7017---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F89e5b96c7017&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-setup-a-simple-etl-pipeline-with-aws-lambda-for-data-science-89e5b96c7017&source=-----89e5b96c7017---------------------bookmark_footer-----------)![](img/4c63b13a67b5fb8d990da76038c2922d.png)

照片由 [Rod Long](https://unsplash.com/@rodlong?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **AWS Lambda 的 ETL 介绍**

当需要构建 ETL 管道时，有许多选项。你可以使用像 [Astronomer](https://www.dataknowsall.com/astrointro.html) 或 [Prefect](http://prefect.io) 这样的工具进行编排，但你还需要一个地方来运行计算。在这方面，你有几个选择：

+   像 AWS EC2 这样的虚拟机 (VM)

+   像 AWS ECS 或 AWS Fargate 这样的容器服务

+   类似 AWS EMR（弹性映射计算）的 Apache Spark

+   像 AWS Lambda 这样的无服务器计算

这些都有其优点。如果你在寻找设置、维护和成本上的简单性，你可以使用**AWS Lambdas**或无服务器计算来运行*简单*任务。

注意，我说的是**简单**。AWS Lambdas 并不适合计算密集型或长期运行的任务。它们适合执行小量代码，这些代码耗时几分钟而不是几小时。

# 什么是 AWS Lambda 和无服务器计算？
