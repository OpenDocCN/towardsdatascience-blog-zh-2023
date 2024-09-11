# 创建你自己的 ChatGPT 无需编程——逐步指南

> 原文：[`towardsdatascience.com/creating-your-own-chatgpt-without-coding-a-step-by-step-guide-a4523e6a46e1?source=collection_archive---------0-----------------------#2023-11-12`](https://towardsdatascience.com/creating-your-own-chatgpt-without-coding-a-step-by-step-guide-a4523e6a46e1?source=collection_archive---------0-----------------------#2023-11-12)

[](https://medium.com/@AhmedF?source=post_page-----a4523e6a46e1--------------------------------)![Ahmed Fessi](https://medium.com/@AhmedF?source=post_page-----a4523e6a46e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4523e6a46e1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4523e6a46e1--------------------------------) [Ahmed Fessi](https://medium.com/@AhmedF?source=post_page-----a4523e6a46e1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F37fb6215fe2c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-your-own-chatgpt-without-coding-a-step-by-step-guide-a4523e6a46e1&user=Ahmed+Fessi&userId=37fb6215fe2c&source=post_page-37fb6215fe2c----a4523e6a46e1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4523e6a46e1--------------------------------) ·9 分钟阅读·2023 年 11 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa4523e6a46e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-your-own-chatgpt-without-coding-a-step-by-step-guide-a4523e6a46e1&user=Ahmed+Fessi&userId=37fb6215fe2c&source=-----a4523e6a46e1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa4523e6a46e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-your-own-chatgpt-without-coding-a-step-by-step-guide-a4523e6a46e1&source=-----a4523e6a46e1---------------------bookmark_footer-----------)

几乎在 ChatGPT 发布一年后，OpenAI 继续以其平台的新功能和能力让我们惊喜。

OpenAI 的最新发布说明确实提供了一个新的酷炫功能，可以创建你自己定制版本的 ChatGPT，称为 [GPTs](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)。

GPTs 是进一步提高生产力的一种方式。如果你已经是 ChatGPT 的用户，你可能已经识别出了一长串可以帮助你日常活动的用例：它可以优化你的推文，帮助头脑风暴文章创意，生成销售推介的变体，审查你的代码或帮助你学习世界语。GPTs 旨在帮助你制作一个“专业化”的 ChatGPT，针对你的需求进行个性化，并在某一特定领域表现出色，而不是泛泛而谈。

这开启了新的可能性。在本文中，我们将逐步创建一个新的定制 GPT，并讨论一些这个新功能的限制。

## 先决条件

先决条件很简单，你需要：

+   一个 ChatGPT Plus 许可或企业账户（这意味着，基本上，你不能使用免费账户创建 GPTs）。

+   以及一个新 GPT 的想法（考虑你的主要用例，或者请 ChatGPT 帮助你进行头脑风暴）。以下是 OpenAI 分享的一些用例示例：
