# GPT-4 与 ChatGPT：对训练、性能、能力和局限性的探索

> 原文：[`towardsdatascience.com/gpt-4-vs-chatgpt-an-exploration-of-training-performance-capabilities-and-limitations-35c990c133c5?source=collection_archive---------0-----------------------#2023-03-17`](https://towardsdatascience.com/gpt-4-vs-chatgpt-an-exploration-of-training-performance-capabilities-and-limitations-35c990c133c5?source=collection_archive---------0-----------------------#2023-03-17)

## GPT-4 是一种改进，但要保持合理的期待。

[](https://medium.com/@mary.newhauser?source=post_page-----35c990c133c5--------------------------------)![玛丽·纽豪瑟](https://medium.com/@mary.newhauser?source=post_page-----35c990c133c5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----35c990c133c5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----35c990c133c5--------------------------------) [玛丽·纽豪瑟](https://medium.com/@mary.newhauser?source=post_page-----35c990c133c5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b27bdb820b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-4-vs-chatgpt-an-exploration-of-training-performance-capabilities-and-limitations-35c990c133c5&user=Mary+Newhauser&userId=6b27bdb820b9&source=post_page-6b27bdb820b9----35c990c133c5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----35c990c133c5--------------------------------) ·7 分钟阅读·2023 年 3 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F35c990c133c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-4-vs-chatgpt-an-exploration-of-training-performance-capabilities-and-limitations-35c990c133c5&user=Mary+Newhauser&userId=6b27bdb820b9&source=-----35c990c133c5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F35c990c133c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-4-vs-chatgpt-an-exploration-of-training-performance-capabilities-and-limitations-35c990c133c5&source=-----35c990c133c5---------------------bookmark_footer-----------)![](img/8ebc0c71e1eaa32026c30fe54eb8b0d9.png)

图片由作者创建。

OpenAI 在 2022 年末发布了 [ChatGPT](https://openai.com/blog/chatgpt)，震惊了整个世界。这个新的生成语言模型预计将彻底改变包括媒体、教育、法律和技术在内的整个行业。简而言之，ChatGPT 威胁到几乎所有领域的现状。甚至在我们还没有时间真正设想一个后 ChatGPT 的世界时，OpenAI 就推出了 [GPT-4](https://openai.com/research/gpt-4)。

最近几个月，突破性的超大语言模型发布速度令人惊叹。如果你仍然不明白 ChatGPT 与 GPT-3 的不同，更不用说与 GPT-4 的差异，我也不怪你。

在本文中，我们将涵盖 ChatGPT 和 GPT-4 之间的主要相似性和差异，包括它们的训练方法、性能与能力以及局限性。

# ChatGPT 与 GPT-4：训练方法的相似性与差异

ChatGPT 和 GPT-4 都建立在前人的基础上，在前几代 GPT 模型的基础上进行改进，改进了模型架构，采用了更复杂的训练方法，并增加了训练参数的数量。
