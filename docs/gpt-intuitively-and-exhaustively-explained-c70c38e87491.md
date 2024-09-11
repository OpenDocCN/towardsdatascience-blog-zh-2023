# GPT — 直观且详尽的解释

> 原文：[https://towardsdatascience.com/gpt-intuitively-and-exhaustively-explained-c70c38e87491?source=collection_archive---------0-----------------------#2023-12-01](https://towardsdatascience.com/gpt-intuitively-and-exhaustively-explained-c70c38e87491?source=collection_archive---------0-----------------------#2023-12-01)

## 自然语言处理 | 机器学习 | 聊天 GPT

## 探索 OpenAI 的生成预训练变换器的架构。

[](https://medium.com/@danielwarfield1?source=post_page-----c70c38e87491--------------------------------)[![Daniel Warfield](../Images/c1c8b4dd514f6813e08e401401324bca.png)](https://medium.com/@danielwarfield1?source=post_page-----c70c38e87491--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c70c38e87491--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c70c38e87491--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----c70c38e87491--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-intuitively-and-exhaustively-explained-c70c38e87491&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----c70c38e87491---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c70c38e87491--------------------------------) ·16 min 阅读·2023年12月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc70c38e87491&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-intuitively-and-exhaustively-explained-c70c38e87491&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----c70c38e87491---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc70c38e87491&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-intuitively-and-exhaustively-explained-c70c38e87491&source=-----c70c38e87491---------------------bookmark_footer-----------)![](../Images/4411a465ee908df05347a8808740ba13.png)

“Mixture Expert” 由作者使用 MidJourney 制作。所有图像均由作者提供，除非另有说明。

在本文中，我们将探索 OpenAI GPT 模型的演变。我们将简要介绍变换器，描述导致第一个 GPT 模型的变换器变体，然后我们将回顾 GPT1、GPT2、GPT3 和 GPT4，以建立对前沿技术的完整概念理解。

**这对谁有用？** 任何对自然语言处理（NLP）或前沿人工智能进展感兴趣的人。

**这篇文章的难度如何？** 这篇文章应该对所有经验水平的人都适用。

**前提条件：** 我将在本文中简要介绍变换器，但你可以参考我专门的文章获取更多信息。

[](/transformers-intuitively-and-exhaustively-explained-58a5c5df8dbb?source=post_page-----c70c38e87491--------------------------------) [## 变换器 — 直观而全面的解释

### 探索现代机器学习的潮流：逐步拆解变换器

[towardsdatascience.com](/transformers-intuitively-and-exhaustively-explained-58a5c5df8dbb?source=post_page-----c70c38e87491--------------------------------)

# 《变换器简要介绍》
