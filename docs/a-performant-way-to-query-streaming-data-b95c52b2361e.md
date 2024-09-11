# 高效查询流数据的方法

> 原文：[`towardsdatascience.com/a-performant-way-to-query-streaming-data-b95c52b2361e?source=collection_archive---------5-----------------------#2023-05-22`](https://towardsdatascience.com/a-performant-way-to-query-streaming-data-b95c52b2361e?source=collection_archive---------5-----------------------#2023-05-22)

## 使用 ksqlDB 改进实时流处理项目

[](https://harrisonfhoffman.medium.com/?source=post_page-----b95c52b2361e--------------------------------)![哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----b95c52b2361e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b95c52b2361e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b95c52b2361e--------------------------------) [哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----b95c52b2361e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F38889d0801d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-performant-way-to-query-streaming-data-b95c52b2361e&user=Harrison+Hoffman&userId=38889d0801d0&source=post_page-38889d0801d0----b95c52b2361e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b95c52b2361e--------------------------------) ·13 分钟阅读·2023 年 5 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb95c52b2361e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-performant-way-to-query-streaming-data-b95c52b2361e&user=Harrison+Hoffman&userId=38889d0801d0&source=-----b95c52b2361e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb95c52b2361e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-performant-way-to-query-streaming-data-b95c52b2361e&source=-----b95c52b2361e---------------------bookmark_footer-----------)![](img/7ad66dc44368d76b3dccaa094133115e.png)

北卡罗来纳州。图片来自作者。

在上一篇文章中，我们探讨了一个实时流处理项目的组成部分，该项目使用 FastAPI、Kafka、QuestDB 和 Docker 来消费和处理智能手机传感器数据。该项目是实现一种架构的初步尝试，该架构可以将智能手机上的流数据通过 Kafka 日志传输，并进入时间序列数据库，以便可以方便地查询和处理数据。最终产品是一个仪表盘，它轮询数据库并实时显示传感器读数：

[](https://youtube.com/shorts/zRUVvz5vsl8?feature=share&source=post_page-----b95c52b2361e--------------------------------) [## 使用 FastAPI、Kafka、QuestDB 和 Docker 进行智能手机数据流处理

### 该视频演示了使用 FastAPI、Kafka、QuestDB 等工具可视化实时智能手机传感器数据……

youtube.com](https://youtube.com/shorts/zRUVvz5vsl8?feature=share&source=post_page-----b95c52b2361e--------------------------------)

对该项目的一项批评是，由于从 Kafka 写入数据库并查询数据库以显示最新的传感器读数，引入了不必要的延迟。当我们的终极目标是近实时分析数据时，写入和读取数据库变得低效。

这是[ksqlDB](https://docs.ksqldb.io/en/latest/)创建的目的之一。与其将数据写入数据库并查询分析，不如直接处理数据……
