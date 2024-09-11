# 如何构建 LLM 应用

> 原文：[https://towardsdatascience.com/how-to-build-an-llm-application-360848c957db?source=collection_archive---------11-----------------------#2023-07-21](https://towardsdatascience.com/how-to-build-an-llm-application-360848c957db?source=collection_archive---------11-----------------------#2023-07-21)

## 使用 Langchain 和 OpenAI 构建 LLM 相关应用

[](https://johnadeojo.medium.com/?source=post_page-----360848c957db--------------------------------)[![John Adeojo](../Images/f6460fae462b055d36dce16fefcd142c.png)](https://johnadeojo.medium.com/?source=post_page-----360848c957db--------------------------------)[](https://towardsdatascience.com/?source=post_page-----360848c957db--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----360848c957db--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----360848c957db--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff933e1637e40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-llm-application-360848c957db&user=John+Adeojo&userId=f933e1637e40&source=post_page-f933e1637e40----360848c957db---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----360848c957db--------------------------------) ·5 min read·2023年7月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F360848c957db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-llm-application-360848c957db&user=John+Adeojo&userId=f933e1637e40&source=-----360848c957db---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F360848c957db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-llm-application-360848c957db&source=-----360848c957db---------------------bookmark_footer-----------)![](../Images/5f840382aba59b8fb02a918285107a91.png)

图片来源：作者：使用 Midjourney 生成

# 以 LLM 为中心的应用

人工智能的创新速度在短时间内飞快。特别是，两项创新为构建以大型语言模型（LLM）为中心的应用程序开辟了大量可能性：[函数调用](https://openai.com/blog/function-calling-and-other-api-updates)和[智能体](https://python.langchain.com/docs/modules/agents/)。

在这篇文章中，我将演示如何利用函数调用和智能体在航班数据库上进行搜索，帮助你找到便宜的航班、短途航班、长途航班，或任何符合你偏好的航班。

请注意——至少，你需要以下这些条件才能使这个方法对你有效：

+   一个 [OpenAI API密钥](https://www.howtogeek.com/885918/how-to-get-an-openai-api-key/) — 用于访问大型语言模型。

+   一个 [Amadeus API密钥](https://developers.amadeus.com/) — 用于访问航班数据。

现在，让我们**深入探讨**技术细节。

# 自主代理链

[Langchain](https://python.langchain.com/docs/get_started/introduction.html) 一直处于LLM驱动代理的前沿。它是一个简单但强大的概念。

本质上，你可以赋予一个代理**“推理”**的能力，在我们的例子中，这将是GPT-4。
