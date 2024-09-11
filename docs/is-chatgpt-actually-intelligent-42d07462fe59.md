# ChatGPT 真的聪明吗？

> 原文：[https://towardsdatascience.com/is-chatgpt-actually-intelligent-42d07462fe59?source=collection_archive---------15-----------------------#2023-07-21](https://towardsdatascience.com/is-chatgpt-actually-intelligent-42d07462fe59?source=collection_archive---------15-----------------------#2023-07-21)

## 也许不是…

[](https://huonglanchu.medium.com/?source=post_page-----42d07462fe59--------------------------------)[![Lan Chu](../Images/813b24f60d6cfe2c9273e064d850c7fe.png)](https://huonglanchu.medium.com/?source=post_page-----42d07462fe59--------------------------------)[](https://towardsdatascience.com/?source=post_page-----42d07462fe59--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----42d07462fe59--------------------------------) [Lan Chu](https://huonglanchu.medium.com/?source=post_page-----42d07462fe59--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916743f0e10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-chatgpt-actually-intelligent-42d07462fe59&user=Lan+Chu&userId=3916743f0e10&source=post_page-3916743f0e10----42d07462fe59---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----42d07462fe59--------------------------------) · 12分钟阅读 · 2023年7月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F42d07462fe59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-chatgpt-actually-intelligent-42d07462fe59&user=Lan+Chu&userId=3916743f0e10&source=-----42d07462fe59---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F42d07462fe59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-chatgpt-actually-intelligent-42d07462fe59&source=-----42d07462fe59---------------------bookmark_footer-----------)

如果你在过去几个月内使用过任何社交媒体平台，我相信你一定听说过 ChatGPT、Google Bard、Microsoft Bing 以及各种新的语言模型。所有这些新模型，有些人可能会争辩说，比你和我写得更好，而且他们的英语肯定比我好**🥲**。每隔几年，总会有一些疯狂的发明，让你彻底重新考虑什么是可能的。在这篇文章中，我们将讨论这种让全世界为之震撼的发明——是的，你猜对了——ChatGPT。

![](../Images/e9402ce0a1ff589cf90aaf22d6fcd633.png)

图片由 Bing 图像生成器生成。人类智能的艺术表现。

随着我们越来越依赖人工智能来为我们做事、做决策，问人工智能是否真正智能，是否在语言理解上与我们相似，或是否根本不同，是很自然的。

为了搞清楚这一切，我们将首先了解生成预训练变换器（GPT）和ChatGPT是如何工作的，然后讨论人工智能具备智能的意义。

# 理解GPT模型

GPT模型最早由OpenAI在其论文[《通过生成预训练提高语言理解》](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf)中提出，该模型使用无监督预训练，随后在各种语言任务上进行有监督的微调。
