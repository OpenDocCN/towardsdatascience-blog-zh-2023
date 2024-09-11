# 生成式问答与 GPT 3.5 及长期记忆

> 原文：[`towardsdatascience.com/generative-question-answering-with-long-term-memory-c280e237b144?source=collection_archive---------0-----------------------#2023-02-17`](https://towardsdatascience.com/generative-question-answering-with-long-term-memory-c280e237b144?source=collection_archive---------0-----------------------#2023-02-17)

## 探索检索增强机器学习的新世界

[](https://jamescalam.medium.com/?source=post_page-----c280e237b144--------------------------------)![詹姆斯·布里格斯](https://jamescalam.medium.com/?source=post_page-----c280e237b144--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c280e237b144--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c280e237b144--------------------------------) [詹姆斯·布里格斯](https://jamescalam.medium.com/?source=post_page-----c280e237b144--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9d77a4ca1d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerative-question-answering-with-long-term-memory-c280e237b144&user=James+Briggs&userId=b9d77a4ca1d1&source=post_page-b9d77a4ca1d1----c280e237b144---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c280e237b144--------------------------------) ·6 分钟阅读·2023 年 2 月 17 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc280e237b144&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerative-question-answering-with-long-term-memory-c280e237b144&source=-----c280e237b144---------------------bookmark_footer-----------)![](img/c5e51b39e15d83cb4ab0c9ed3580abef.png)

图片由 [布雷特·卡瓦诺](https://unsplash.com/@bretkavanaugh?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。 *最初发表于* [*pinecone.io*](https://www.pinecone.io/learn/openai-gen-qa/)，*作者在该网站工作*。*

生成式 AI 在 2022 年引发了几个*“哇”*的时刻。从生成艺术工具如 OpenAI 的 DALL-E 2、Midjourney 和 Stable Diffusion，到下一代**大**型**语**言**模**型，如 OpenAI 的 GPT-3.5 系列模型、BLOOM，以及像 LaMDA 和 ChatGPT 这样的聊天机器人。

生成式 AI 正经历着兴趣和创新的繁荣，这一点并不令人惊讶[1]。然而，这只是生成式 AI 广泛应用的**第一年**：这是一个新兴领域的早期阶段，预计将颠覆我们与机器的互动方式。

最引人深思的应用之一属于**生成式问题回答（GQA）**。通过使用 GQA，我们可以打造类似人类的机器信息检索（IR）交互。

我们每天都在使用 IR 系统。Google 搜索索引了网络并检索与搜索词相关的信息。Netflix 利用你在平台上的行为和历史来推荐新的电视剧和电影，Amazon 也用类似的方法推荐产品[2]。

这些 IR 应用正在改变世界。然而，它们可能只是未来几个月和几年 IR 与 GQA 结合后的微弱回声。
