# 专业化的 LLM：ChatGPT、LaMDA、Galactica、Codex、Sparrow 等

> 原文：[`towardsdatascience.com/specialized-llms-chatgpt-lamda-galactica-codex-sparrow-and-more-ccccdd9f666f?source=collection_archive---------1-----------------------#2023-01-13`](https://towardsdatascience.com/specialized-llms-chatgpt-lamda-galactica-codex-sparrow-and-more-ccccdd9f666f?source=collection_archive---------1-----------------------#2023-01-13)

## 创建更佳领域特定 LLM 的简单技巧

[](https://wolfecameron.medium.com/?source=post_page-----ccccdd9f666f--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----ccccdd9f666f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ccccdd9f666f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ccccdd9f666f--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----ccccdd9f666f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspecialized-llms-chatgpt-lamda-galactica-codex-sparrow-and-more-ccccdd9f666f&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----ccccdd9f666f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ccccdd9f666f--------------------------------) ·30 分钟阅读·2023 年 1 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fccccdd9f666f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspecialized-llms-chatgpt-lamda-galactica-codex-sparrow-and-more-ccccdd9f666f&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----ccccdd9f666f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fccccdd9f666f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspecialized-llms-chatgpt-lamda-galactica-codex-sparrow-and-more-ccccdd9f666f&source=-----ccccdd9f666f---------------------bookmark_footer-----------)![](img/1469bf9ac8a88095d227844718d069f4.png)

(照片由 [NASA](https://unsplash.com/es/@nasa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/images/nature/space?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText))

大型语言模型（LLMs）是非常有用的任务无关基础模型。但，*我们实际能用一个通用模型完成多少工作？*这些模型擅长解决我们在深度学习文献中看到的常见自然语言基准问题。然而，实际使用 LLMs 通常需要对模型进行新的行为训练，使其与特定应用相关。在本综述中，我们将探讨为各种用例专业化和改进 LLMs 的方法。

我们可以通过使用领域特定的预训练、模型对齐和监督微调等技术来修改大语言模型（LLMs）的行为。这些方法可以用来消除 LLMs 的已知局限性（例如，生成错误/偏见信息），调整 LLM 行为以更好地满足我们的需求，甚至将专门的知识注入 LLM，使其成为某个领域的专家。

为特定应用创建专业化 LLMs 的概念在近期文献中得到了广泛探讨。尽管存在多种不同的方法，但它们有一个共同的主题：使 LLMs 在实际应用中更具可行性和实用性。尽管“实用”的定义在不同应用和人类之间高度可变…
