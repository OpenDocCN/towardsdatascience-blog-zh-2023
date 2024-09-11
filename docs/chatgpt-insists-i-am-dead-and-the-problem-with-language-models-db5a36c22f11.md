# GPT 是一个不可靠的信息存储库

> 原文：[https://towardsdatascience.com/chatgpt-insists-i-am-dead-and-the-problem-with-language-models-db5a36c22f11?source=collection_archive---------0-----------------------#2023-02-21](https://towardsdatascience.com/chatgpt-insists-i-am-dead-and-the-problem-with-language-models-db5a36c22f11?source=collection_archive---------0-----------------------#2023-02-21)

## 了解大型语言模型的局限性和风险

[](https://medium.com/@nobleackerson?source=post_page-----db5a36c22f11--------------------------------)[![Noble Ackerson](../Images/e89361fc2723b2b08384ace7f081bfed.png)](https://medium.com/@nobleackerson?source=post_page-----db5a36c22f11--------------------------------)[](https://towardsdatascience.com/?source=post_page-----db5a36c22f11--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----db5a36c22f11--------------------------------) [Noble Ackerson](https://medium.com/@nobleackerson?source=post_page-----db5a36c22f11--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F68605bd278a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-insists-i-am-dead-and-the-problem-with-language-models-db5a36c22f11&user=Noble+Ackerson&userId=68605bd278a3&source=post_page-68605bd278a3----db5a36c22f11---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----db5a36c22f11--------------------------------) ·9 min read·2023年2月21日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdb5a36c22f11&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchatgpt-insists-i-am-dead-and-the-problem-with-language-models-db5a36c22f11&source=-----db5a36c22f11---------------------bookmark_footer-----------)![](../Images/877f131a65aa9cec5806a78ebd5af18a.png)

“寻找意义” 作者拍摄

大型语言模型（或生成预训练变换器，GPT）需要更可靠的信息准确性检查，才能被考虑用于搜索。

这些模型在创意应用方面表现出色，例如讲故事、艺术或音乐，并且能够为应用程序创建隐私保护的合成数据。

然而，这些模型在保持一致的事实准确性方面表现不佳，这主要是由于 [AI 幻觉](https://www.wired.com/story/ai-has-a-hallucination-problem-thats-proving-tough-to-fix/) 和 ChatGPT、Bing Chat 以及 Google Bard 的迁移学习限制。

首先，让我们定义什么是 AI 幻觉。存在一些情况，其中大型语言模型创建的信息并非基于事实证据，而可能受到其转换器架构偏见或解码错误的影响。换句话说，模型编造事实，这在对事实准确性至关重要的领域中可能会造成问题。

在一个准确可靠的信息对抗虚假信息至关重要的世界中，忽视一致的事实准确性是危险的。

搜索公司应该重新考虑通过将搜索与未经过滤的 GPT 驱动的聊天模式混合来“重新发明搜索”，以避免对公众健康、政治稳定或社会造成潜在伤害…
