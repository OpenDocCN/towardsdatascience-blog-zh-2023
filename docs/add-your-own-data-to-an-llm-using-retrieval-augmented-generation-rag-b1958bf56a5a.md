# 检索增强生成（RAG）

> 原文：[https://towardsdatascience.com/add-your-own-data-to-an-llm-using-retrieval-augmented-generation-rag-b1958bf56a5a?source=collection_archive---------1-----------------------#2023-09-29](https://towardsdatascience.com/add-your-own-data-to-an-llm-using-retrieval-augmented-generation-rag-b1958bf56a5a?source=collection_archive---------1-----------------------#2023-09-29)

## 学习如何使用名为检索增强生成的基于提示的技术将自己的专有数据添加到预训练的语言模型

[](https://medium.com/@bea_684?source=post_page-----b1958bf56a5a--------------------------------)[![Beatriz Stollnitz](../Images/63a2a7daeca6d93e26b3ac0556c42aa1.png)](https://medium.com/@bea_684?source=post_page-----b1958bf56a5a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1958bf56a5a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b1958bf56a5a--------------------------------) [Beatriz Stollnitz](https://medium.com/@bea_684?source=post_page-----b1958bf56a5a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1c8863892480&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadd-your-own-data-to-an-llm-using-retrieval-augmented-generation-rag-b1958bf56a5a&user=Beatriz+Stollnitz&userId=1c8863892480&source=post_page-1c8863892480----b1958bf56a5a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1958bf56a5a--------------------------------) ·21 分钟阅读·2023 年 9 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb1958bf56a5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadd-your-own-data-to-an-llm-using-retrieval-augmented-generation-rag-b1958bf56a5a&user=Beatriz+Stollnitz&userId=1c8863892480&source=-----b1958bf56a5a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb1958bf56a5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadd-your-own-data-to-an-llm-using-retrieval-augmented-generation-rag-b1958bf56a5a&source=-----b1958bf56a5a---------------------bookmark_footer-----------)![](../Images/cd0c3b0f21d72762c8a25dcf3ac3c039.png)

照片由[Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**简介**

大型语言模型（LLMs）对世界了解很多，但并不是全部。由于训练这些模型需要很长时间，它们最后一次训练的数据可能已经相当陈旧。尽管LLMs了解互联网上的通用事实，但它们不了解你的专有数据，而这些数据通常是你在基于AI的应用中所需要的。因此，最近在学术界和工业界，扩展LLMs以包含新数据成为了一个重要的关注点也就不足为奇了。

在大型语言模型的新时代之前，我们通常通过简单地微调模型来扩展新数据。但现在我们的模型更大，训练的数据也更多，微调只适用于少数情况。当我们想让LLM以不同的风格或语气进行交流时，微调表现尤其出色。一个很好的微调例子是OpenAI将其较老的完成风格GPT-3.5模型适配为较新的对话风格GPT-3.5-turbo（ChatGPT）模型。如果给完成风格模型输入“你能告诉我关于...
