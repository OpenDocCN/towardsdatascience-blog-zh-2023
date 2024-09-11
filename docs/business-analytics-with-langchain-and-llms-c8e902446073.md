# 使用 LangChain 和 LLMs 的商业分析

> 原文：[`towardsdatascience.com/business-analytics-with-langchain-and-llms-c8e902446073?source=collection_archive---------5-----------------------#2023-12-19`](https://towardsdatascience.com/business-analytics-with-langchain-and-llms-c8e902446073?source=collection_archive---------5-----------------------#2023-12-19)

## 生成式 AI

## 使用自然语言查询 SQL 数据库的逐步教程

[](https://tamimi-naser.medium.com/?source=post_page-----c8e902446073--------------------------------)![Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----c8e902446073--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c8e902446073--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c8e902446073--------------------------------) [Naser Tamimi](https://tamimi-naser.medium.com/?source=post_page-----c8e902446073--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff94e8b97c7aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbusiness-analytics-with-langchain-and-llms-c8e902446073&user=Naser+Tamimi&userId=f94e8b97c7aa&source=post_page-f94e8b97c7aa----c8e902446073---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c8e902446073--------------------------------) · 7 分钟阅读 · 2023 年 12 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc8e902446073&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbusiness-analytics-with-langchain-and-llms-c8e902446073&user=Naser+Tamimi&userId=f94e8b97c7aa&source=-----c8e902446073---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc8e902446073&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbusiness-analytics-with-langchain-and-llms-c8e902446073&source=-----c8e902446073---------------------bookmark_footer-----------)![](img/5540ac3aed1e97360f2bf8286e9d3663.png)

图片由作者提供（通过 Midjourney 生成）

许多企业在其数据库中存储了大量的专有数据。如果有一个能够理解自然语言并查询这些数据库的虚拟助手，这将为这些企业带来巨大的机会。想想客服聊天机器人，它们就是一个常见的例子。这些助手可以接受客户的请求，向数据库询问信息，并提供客户所需的内容。

这种代理的好处不仅限于外部客户互动。许多企业主或公司中的人员，甚至是技术公司中的人，可能不懂 SQL 或类似的语言，但他们仍然需要向数据库请求信息。这就是像 LangChain 这样的框架发挥作用的地方。这些框架使得创建这些有用的代理/应用程序变得容易。这些代理可以与人类交流，同时也可以与数据库、API 等进行对话。

# LLM 支持的应用程序

LangChain 是一个开源框架，用于构建利用大型语言模型（LLMs）的互动应用程序。它是一个帮助 LLMs 与其他信息源连接并让它们与周围世界交流的工具。这样的框架中的一个重要概念是……
