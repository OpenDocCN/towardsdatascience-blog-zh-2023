# 如何正确使用Elasticsearch中的同义词功能

> 原文：[https://towardsdatascience.com/how-to-use-the-synonyms-feature-correctly-in-elasticsearch-7bdf856a94cb?source=collection_archive---------2-----------------------#2023-01-07](https://towardsdatascience.com/how-to-use-the-synonyms-feature-correctly-in-elasticsearch-7bdf856a94cb?source=collection_archive---------2-----------------------#2023-01-07)

## 学习简单但强大的同义词功能，以提高搜索质量

[](https://lynn-kwong.medium.com/?source=post_page-----7bdf856a94cb--------------------------------)[![Lynn G. Kwong](../Images/b9a05b6587db5ca41c1d8264adda5b06.png)](https://lynn-kwong.medium.com/?source=post_page-----7bdf856a94cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7bdf856a94cb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7bdf856a94cb--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----7bdf856a94cb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff649eccbbc3d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-synonyms-feature-correctly-in-elasticsearch-7bdf856a94cb&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=post_page-f649eccbbc3d----7bdf856a94cb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7bdf856a94cb--------------------------------) ·9分钟阅读·2023年1月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7bdf856a94cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-synonyms-feature-correctly-in-elasticsearch-7bdf856a94cb&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=-----7bdf856a94cb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7bdf856a94cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-the-synonyms-feature-correctly-in-elasticsearch-7bdf856a94cb&source=-----7bdf856a94cb---------------------bookmark_footer-----------)![](../Images/13d37f90a8d511dae87864d18c83d3ca.png)

图片来源：Tumisu，Pixabay

同义词用于提高搜索质量，扩展匹配的范围。例如，用户搜索“England”时，可能希望找到包含“British”或“UK”的文档，尽管这三个词完全不同。

Elasticsearch 的同义词功能非常强大，如果正确实施，它可以使你的搜索引擎更加健壮和强大。在这篇文章中，我们将介绍实现同义词功能的要点，并提供简单的代码片段。特别地，我们还会介绍如何为现有索引更新同义词，这是一个相对高级的主题。

## 准备工作

我们将使用 Docker 本地启动一个 Elasticsearch 服务器，并使用 Kibana 来管理索引和运行命令。

我们将在这篇文章中使用的 `docker-compose.yaml` 文件包含以下内容，稍后我们将添加更多功能：

```py
version: "3.9"
services:
  elasticsearch:
    image: elasticsearch:8.5.3
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
      - xpack.security.enabled=false
    volumes…
```
