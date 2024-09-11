# 大模型与大数据：Spark 和 LLMs 的和谐

> 原文：[`towardsdatascience.com/large-models-meet-big-data-spark-and-llms-in-harmony-5e2976b69b62?source=collection_archive---------6-----------------------#2023-12-05`](https://towardsdatascience.com/large-models-meet-big-data-spark-and-llms-in-harmony-5e2976b69b62?source=collection_archive---------6-----------------------#2023-12-05)

## 数据工程与生成性 AI

## *逐步指南：如何使用 Apache Spark 和大语言模型*

[](https://tamimi-naser.medium.com/?source=post_page-----5e2976b69b62--------------------------------)![Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----5e2976b69b62--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5e2976b69b62--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e2976b69b62--------------------------------) [Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----5e2976b69b62--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff94e8b97c7aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flarge-models-meet-big-data-spark-and-llms-in-harmony-5e2976b69b62&user=Naser+Tamimi&userId=f94e8b97c7aa&source=post_page-f94e8b97c7aa----5e2976b69b62---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5e2976b69b62--------------------------------) ·6 min read·2023 年 12 月 5 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5e2976b69b62&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flarge-models-meet-big-data-spark-and-llms-in-harmony-5e2976b69b62&source=-----5e2976b69b62---------------------bookmark_footer-----------)![](img/4b2f8379a79a83aae121976d9e91f2c7.png)

图片由 Midjourney 生成。

生成性人工智能，包括大型语言模型（LLMs），正在革新人类生活的各个方面。在过去的五年中，生成性人工智能已经从一个研究项目发展成为许多人实际应用的技术。作为一个对生成性人工智能感兴趣的数据工程师，我一直在问自己，这项技术对我的工作和数据工程应用带来了什么？生成性人工智能和大型语言模型在工程师中有一些常见的应用，例如代码生成、文档辅助等。但在这里，我正在评估生成性人工智能和大型语言模型在数据工程中的一些更专业的用途。如果你对这个话题感兴趣，请阅读这篇文章，并关注我在[Medium](https://tamimi-naser.medium.com/)和[Linkedin](https://www.linkedin.com/in/nasertamimi/)上的更多文章，了解其他应用案例。

# 大型语言模型：变革的强大工具

数据工程师喜欢结构化和抽象化的数据并不是什么新鲜事。但世界上充满了需要数据工程师关注的非结构化和无序的数据。对非结构化数据的转换总是复杂的，有时用传统工具甚至是不可能的。历史上，其中之一……
