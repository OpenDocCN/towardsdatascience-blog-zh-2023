# 使用 Go 构建 REST API：时间序列的数据分析

> 原文：[https://towardsdatascience.com/build-a-rest-api-with-go-data-analytics-for-time-series-8d3f0747665c?source=collection_archive---------16-----------------------#2023-11-07](https://towardsdatascience.com/build-a-rest-api-with-go-data-analytics-for-time-series-8d3f0747665c?source=collection_archive---------16-----------------------#2023-11-07)

## 一个使用 Go、Gin 和 Gorm 的 CRUD 操作和统计分析的逐步示例。

[](https://medium.com/@davide.burba?source=post_page-----8d3f0747665c--------------------------------)[![Davide Burba](../Images/a1ca3cf59c2b933021fa0d978e1af522.png)](https://medium.com/@davide.burba?source=post_page-----8d3f0747665c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8d3f0747665c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8d3f0747665c--------------------------------) [Davide Burba](https://medium.com/@davide.burba?source=post_page-----8d3f0747665c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9f58aaaeaed7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-rest-api-with-go-data-analytics-for-time-series-8d3f0747665c&user=Davide+Burba&userId=9f58aaaeaed7&source=post_page-9f58aaaeaed7----8d3f0747665c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d3f0747665c--------------------------------) ·10分钟阅读·2023年11月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8d3f0747665c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-rest-api-with-go-data-analytics-for-time-series-8d3f0747665c&user=Davide+Burba&userId=9f58aaaeaed7&source=-----8d3f0747665c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8d3f0747665c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-rest-api-with-go-data-analytics-for-time-series-8d3f0747665c&source=-----8d3f0747665c---------------------bookmark_footer-----------)![](../Images/a6e61301baab7f2a0bed1c777ea8da26.png)

“Learning Go”，由[Giulia Roggia](https://www.instagram.com/giulia_roggia__/)。经许可使用。

+   [介绍](#23c9)

+   [模型与数据库](#4f5c)

+   [统计](#810b)

+   [处理程序](#1043)

+   [使用方法](#380a)

# 介绍

本文展示了一个使用 Go 构建的 REST API 示例，该 API 执行 CRUD（创建、读取、更新、删除）操作，并计算时间序列数据的统计信息。

*本文使用的完整代码可在* [*此处*](https://github.com/davide-burba/code-collection/)*找到*。

## *为什么选择 Go？*

[Go](https://go.dev/) 是构建 REST API 的常见选择，有几个原因。尽管它是一种高效的编译语言，其语法简单易读。这使得它容易实现并发处理。它提供了功能丰富的标准库，并拥有一个庞大的库和工具生态系统。

在这个示例中，我们使用了两个流行的 Go 库：

+   [Gin](https://gin-gonic.com/)：一个提供工具用于创建 Web 应用程序的 Web 框架。

+   [Gorm](https://gorm.io/)：一个全功能的 ORM（对象关系映射）用来与数据库交互。

## 文件夹结构
