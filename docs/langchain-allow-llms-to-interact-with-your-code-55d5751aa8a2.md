# 🦜🔗LangChain：允许 LLMs 与你的代码互动

> 原文：[https://towardsdatascience.com/langchain-allow-llms-to-interact-with-your-code-55d5751aa8a2?source=collection_archive---------11-----------------------#2023-07-05](https://towardsdatascience.com/langchain-allow-llms-to-interact-with-your-code-55d5751aa8a2?source=collection_archive---------11-----------------------#2023-07-05)

![](../Images/ff27b1999a99a1cc7a7add035a1901f0.png)

图片由[David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习如何为你的 LLM 实现自定义函数，使用工具

[](https://medium.com/@marcellopoliti?source=post_page-----55d5751aa8a2--------------------------------)[![Marcello Politi](../Images/484e44571bd2e75acfe5fef3146ab3c2.png)](https://medium.com/@marcellopoliti?source=post_page-----55d5751aa8a2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----55d5751aa8a2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----55d5751aa8a2--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----55d5751aa8a2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7390355d40fe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-allow-llms-to-interact-with-your-code-55d5751aa8a2&user=Marcello+Politi&userId=7390355d40fe&source=post_page-7390355d40fe----55d5751aa8a2---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----55d5751aa8a2--------------------------------) ·5分钟阅读·2023年7月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F55d5751aa8a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-allow-llms-to-interact-with-your-code-55d5751aa8a2&user=Marcello+Politi&userId=7390355d40fe&source=-----55d5751aa8a2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F55d5751aa8a2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flangchain-allow-llms-to-interact-with-your-code-55d5751aa8a2&source=-----55d5751aa8a2---------------------bookmark_footer-----------)

## 介绍

生成模型正在引起每个人的关注。现在，许多 AI 应用程序不再需要领域内的机器学习专家，而只需了解如何实现 API 调用。

最近，例如，我参加了一个黑客马拉松，我需要实现一个自定义命名实体识别，但我直接使用了一个LLM，并利用了它的少量样本学习能力来获得我想要的结果，这对赢得黑客马拉松来说已经足够了！（如果你想的话，可以[在这里](https://www.brianknows.org/)查看这个项目）。

因此，对于许多现实世界的应用程序，重点更多地转向如何与这些LLM互动和使用它们，而不是创建模型。**LangChain** 是一个允许你做到这一点的库，我最近写了几篇关于它的文章。

## LangChain 工具

**工具是LLM可以用来增强其能力的实用程序**。工具可以在链或代理中实例化。

例如，LLM 可能会在回应之前进行维基百科搜索，以确保回应是最新的。

当然，代理可以使用多个工具，因此通常的做法是定义一个工具列表。
