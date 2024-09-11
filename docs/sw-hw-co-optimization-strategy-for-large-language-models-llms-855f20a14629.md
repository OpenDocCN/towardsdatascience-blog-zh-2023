# 大型语言模型（LLMs）的 SW/HW 协同优化策略

> 原文：[`towardsdatascience.com/sw-hw-co-optimization-strategy-for-large-language-models-llms-855f20a14629?source=collection_archive---------3-----------------------#2023-12-16`](https://towardsdatascience.com/sw-hw-co-optimization-strategy-for-large-language-models-llms-855f20a14629?source=collection_archive---------3-----------------------#2023-12-16)

## 如何最大化系统的性能以更快地运行 LLMs？— 最佳实践

[](https://medium.com/@LizLiAI?source=post_page-----855f20a14629--------------------------------)![Liz Li](https://medium.com/@LizLiAI?source=post_page-----855f20a14629--------------------------------)[](https://towardsdatascience.com/?source=post_page-----855f20a14629--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----855f20a14629--------------------------------) [Liz Li](https://medium.com/@LizLiAI?source=post_page-----855f20a14629--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdc3f793d765a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsw-hw-co-optimization-strategy-for-large-language-models-llms-855f20a14629&user=Liz+Li&userId=dc3f793d765a&source=post_page-dc3f793d765a----855f20a14629---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----855f20a14629--------------------------------) · 5 分钟阅读 · 2023 年 12 月 16 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F855f20a14629&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsw-hw-co-optimization-strategy-for-large-language-models-llms-855f20a14629&source=-----855f20a14629---------------------bookmark_footer-----------)

领先的大型语言模型（LLMs）如 ChatGPT、Llama 等正在革新科技行业并影响每个人的生活。然而，它们的成本构成了一个重大障碍。使用 OpenAI API 的应用在持续运行中会产生大量开支（每 1,000 个提示令牌$0.03，每 1,000 个采样令牌$0.06）。

为了降低成本，公司倾向于托管自己的 LLMs，费用根据模型大小差异很大（较大的 LLMs，参数为 100–200B，相较于参数为 7–15B 的小型模型，成本可能高出~10 倍）。这一趋势催生了 AI 芯片竞赛，因为主要科技公司旨在开发自己的 AI 芯片，从而减少对昂贵硬件的依赖。

![](img/c20161743022e1acac02f5a2f4dd573f.png)

模型大小的趋势。来源：AWS reInvent

如何充分挖掘每一份计算能力来运行 LLMs？在这篇文章中，我将对 LLM 优化策略进行全面分析，涵盖模型、软件和硬件。它遵循我在上一篇文章中写的[AI SW/HW 协同设计方法](https://example.org/how-to-co-design-software-hardware-architecture-for-ai-ml-in-a-new-era-b296f2842fe2)，并对 LLM 特定的成本和性能优化进行了更深入的讨论。
