# 提示工程指南

> 原文：[`towardsdatascience.com/prompt-engineering-guide-for-data-analysts-54f480ba4d98?source=collection_archive---------0-----------------------#2023-05-07`](https://towardsdatascience.com/prompt-engineering-guide-for-data-analysts-54f480ba4d98?source=collection_archive---------0-----------------------#2023-05-07)

![](img/7eb4d92c8f54e60146f7dd4ba68cb20b.png)

图片由 [Emiliano Vittoriosi](https://unsplash.com/@emilianovittoriosi?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 原则、技巧和应用，以利用 LLM 中提示的力量作为数据分析师

[](https://tanuwidjajaolivia.medium.com/?source=post_page-----54f480ba4d98--------------------------------)![Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----54f480ba4d98--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54f480ba4d98--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----54f480ba4d98--------------------------------) [Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----54f480ba4d98--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff43d6dd597&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-guide-for-data-analysts-54f480ba4d98&user=Olivia+Tanuwidjaja&userId=f43d6dd597&source=post_page-f43d6dd597----54f480ba4d98---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----54f480ba4d98--------------------------------) ·7 分钟阅读·2023 年 5 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F54f480ba4d98&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-guide-for-data-analysts-54f480ba4d98&user=Olivia+Tanuwidjaja&userId=f43d6dd597&source=-----54f480ba4d98---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F54f480ba4d98&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-guide-for-data-analysts-54f480ba4d98&source=-----54f480ba4d98---------------------bookmark_footer-----------)

[大型语言模型（LLM）](https://www.youtube.com/watch?v=iR2O2GPbB0E) 正在兴起，得益于 [OpenAI 的 ChatGPT](https://openai.com/blog/chatgpt) 的广泛普及，迅速席卷了互联网。作为数据领域的从业者，我寻求在工作中最佳利用这项技术，尤其是在数据分析师工作中的洞察性和实用性方面。

LLMs 可以通过“提示”技术解决任务，这些技术中**问题以文本提示的形式呈现给模型**。获得“**正确的提示**”对于确保模型为分配的任务提供高质量和准确的结果至关重要。

在本文中，我将分享提示的原则、构建提示的技术，以及数据分析师在这一“提示时代”中可以发挥的角色。

# 什么是提示工程？

引用[Ben Lorica 来自 Gradient Flow](https://gradientflow.com/the-future-of-prompt-engineering-getting-the-most-out-of-llms/)，**“提示工程是设计有效输入提示以从基础模型中引出所需输出的艺术”**。这是一个迭代的过程，旨在开发可以有效利用现有生成式 AI 模型能力以实现特定目标的提示。
