# 数据平台架构类型

> 原文：[https://towardsdatascience.com/data-platform-architecture-types-f255ac6e0b7?source=collection_archive---------0-----------------------#2023-02-20](https://towardsdatascience.com/data-platform-architecture-types-f255ac6e0b7?source=collection_archive---------0-----------------------#2023-02-20)

## 它在多大程度上满足了你的业务需求？选择的困境。

[](https://mshakhomirov.medium.com/?source=post_page-----f255ac6e0b7--------------------------------)[![💡Mike Shakhomirov](../Images/bc6895c7face3244d488feb97ba0f68e.png)](https://mshakhomirov.medium.com/?source=post_page-----f255ac6e0b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f255ac6e0b7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f255ac6e0b7--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----f255ac6e0b7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-platform-architecture-types-f255ac6e0b7&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----f255ac6e0b7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f255ac6e0b7--------------------------------) ·9 分钟阅读·2023年2月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff255ac6e0b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-platform-architecture-types-f255ac6e0b7&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----f255ac6e0b7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff255ac6e0b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-platform-architecture-types-f255ac6e0b7&source=-----f255ac6e0b7---------------------bookmark_footer-----------)![](../Images/381d2217a4acdb8982f5e74438c93ab7.png)

图片由 [Brooke Lark](https://unsplash.com/@brookelark?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在当前市场上，数据工具的种类繁多，容易让人迷失。互联网充斥着关于使用哪些数据工具以及如何使我们的**数据堆栈**在*今年*变得*现代化*的主观故事（通常是推测性的）。*哪些数据工具是最好的？谁是领导者？如何选择合适的工具？* 这个故事是为了那些在“数据领域”并且正在构建世界上最佳数据平台的人们。

> *那么什么是“现代数据堆栈”，它到底有多现代？*

简而言之，它是一个**工具集合**，用于处理数据。根据我们打算如何使用数据，这些工具可能包括以下内容：

- 管理的ETL/ELT数据管道服务

- 基于云的管理数据仓库/数据湖作为数据的目的地

- 一个数据转换工具

- 一个商业智能或数据可视化平台

- 机器学习和数据科学功能

*有时，它的现代化程度并不重要。*

> *的确，如果我们的BI工具具有现代化的定制OLAP立方体用于数据*…
