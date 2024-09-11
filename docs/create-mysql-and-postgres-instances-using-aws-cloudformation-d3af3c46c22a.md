# 使用 AWS CloudFormation 创建 MySQL 和 Postgres 实例

> 原文：[`towardsdatascience.com/create-mysql-and-postgres-instances-using-aws-cloudformation-d3af3c46c22a?source=collection_archive---------14-----------------------#2023-03-20`](https://towardsdatascience.com/create-mysql-and-postgres-instances-using-aws-cloudformation-d3af3c46c22a?source=collection_archive---------14-----------------------#2023-03-20)

## 数据库从业者的基础设施即代码

[](https://mshakhomirov.medium.com/?source=post_page-----d3af3c46c22a--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----d3af3c46c22a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d3af3c46c22a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3af3c46c22a--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----d3af3c46c22a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-mysql-and-postgres-instances-using-aws-cloudformation-d3af3c46c22a&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----d3af3c46c22a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3af3c46c22a--------------------------------) ·7 分钟阅读·2023 年 3 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd3af3c46c22a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-mysql-and-postgres-instances-using-aws-cloudformation-d3af3c46c22a&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----d3af3c46c22a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd3af3c46c22a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-mysql-and-postgres-instances-using-aws-cloudformation-d3af3c46c22a&source=-----d3af3c46c22a---------------------bookmark_footer-----------)![](img/21d6c29d19d4d9e6c31365437e7443f2.png)

图片来源：[Yunqing Leo](https://unsplash.com/@leoo0oo?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

利用这两个简单的 **CloudFormation 模板**，我们将学习如何通过一个 CLI 命令部署 Postgres 和 MySQL Aurora 数据库实例。我已经优化了模板文件，将属性数量减少到最低，以便更容易理解。

我知道“基础设施即代码”对不熟悉该概念的中级用户来说可能有点困难，但相信我，这确实是正确的方向。职业发展上，它将带来许多好处。

## 这个教程适合谁？

这个教程是针对希望进入数据工程领域并学习一些高级内容（例如**基础设施即代码**）的初级和中级数据及软件工程师的。

我希望这个教程对所有需要使用云数据库来存储应用数据的软件工程师都有帮助。

## **前提条件**
