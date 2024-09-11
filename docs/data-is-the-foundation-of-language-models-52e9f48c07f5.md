# 数据是语言模型的基础

> 原文：[`towardsdatascience.com/data-is-the-foundation-of-language-models-52e9f48c07f5?source=collection_archive---------2-----------------------#2023-10-29`](https://towardsdatascience.com/data-is-the-foundation-of-language-models-52e9f48c07f5?source=collection_archive---------2-----------------------#2023-10-29)

## 高质量数据如何影响 LLM 训练流程的每一个方面…

[](https://wolfecameron.medium.com/?source=post_page-----52e9f48c07f5--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----52e9f48c07f5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----52e9f48c07f5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----52e9f48c07f5--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----52e9f48c07f5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-is-the-foundation-of-language-models-52e9f48c07f5&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----52e9f48c07f5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52e9f48c07f5--------------------------------) · 16 分钟阅读 · 2023 年 10 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F52e9f48c07f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-is-the-foundation-of-language-models-52e9f48c07f5&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----52e9f48c07f5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F52e9f48c07f5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-is-the-foundation-of-language-models-52e9f48c07f5&source=-----52e9f48c07f5---------------------bookmark_footer-----------)![](img/0cdc29d0203d7225c4541733960f9b92.png)

(图片由 [Joshua Sortino](https://unsplash.com/@sortino?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，[Unsplash](https://unsplash.com/photos/worms-eye-view-photography-of-ceiling-LqKhnDzSF-8?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash))

大型语言模型（LLMs）已经存在了一段时间，但只有最近，它们的惊人表现才引起了更广泛的 AI 社区的显著关注。考虑到这一点，我们可能会开始质疑当前 LLM 运动的起源。*是什么让最近的模型相比其前身如此令人印象深刻？* 尽管有人可能会争论各种不同的因素，但一个特别有影响力的进展是能够执行对齐。换句话说，我们找到了如何训练 LLMs，不仅仅输出最可能的下一个词，而是输出能满足人类目标的文本，无论是通过遵循指令还是检索重要信息。

> “我们假设对齐可以是一个简单的过程，其中模型学习与用户交互的风格或格式，以展示在预训练期间已经获得的知识和能力” *— 引自 [1]*

本综述将研究对齐的角色和影响，以及对齐与预训练之间的相互作用。有趣的是，这些观点在最近的 LIMA 模型[1]中得到了探讨，该模型通过简单地对预训练的 LLM 进行微调来实现对齐……
