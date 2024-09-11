# 如何根据您的数据将领域特定知识添加到 LLM 中

> 原文：[`towardsdatascience.com/how-to-add-domain-specific-knowledge-to-an-llm-based-on-your-data-884a5f6a13ca?source=collection_archive---------0-----------------------#2023-07-11`](https://towardsdatascience.com/how-to-add-domain-specific-knowledge-to-an-llm-based-on-your-data-884a5f6a13ca?source=collection_archive---------0-----------------------#2023-07-11)

## 将您的 LLM 转变为领域专家

[](https://medium.com/@villatteantoine?source=post_page-----884a5f6a13ca--------------------------------)![Antoine Villatte](https://medium.com/@villatteantoine?source=post_page-----884a5f6a13ca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----884a5f6a13ca--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----884a5f6a13ca--------------------------------) [Antoine Villatte](https://medium.com/@villatteantoine?source=post_page-----884a5f6a13ca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff24fe466e328&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-add-domain-specific-knowledge-to-an-llm-based-on-your-data-884a5f6a13ca&user=Antoine+Villatte&userId=f24fe466e328&source=post_page-f24fe466e328----884a5f6a13ca---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----884a5f6a13ca--------------------------------) ·7 分钟阅读·2023 年 7 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F884a5f6a13ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-add-domain-specific-knowledge-to-an-llm-based-on-your-data-884a5f6a13ca&user=Antoine+Villatte&userId=f24fe466e328&source=-----884a5f6a13ca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F884a5f6a13ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-add-domain-specific-knowledge-to-an-llm-based-on-your-data-884a5f6a13ca&source=-----884a5f6a13ca---------------------bookmark_footer-----------)![](img/ee90d509f6eef9303c8de7b2790e1f9e.png)

图片由 [Hubi's Tavern](https://unsplash.com/fr/@hubistavern?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

最近几个月，大型语言模型（LLMs）深刻改变了我们工作和与技术互动的方式，并已被证明在各个领域中作为写作助手、代码生成器甚至创造性合作者都是有帮助的工具。它们理解上下文、生成类似人类的文本和执行各种语言相关任务的能力使它们在人工智能研究的前沿。

尽管 LLMs 在生成通用文本方面表现出色，但在面对需要精确知识和细致理解的高度专业领域时，它们往往表现不佳。用于领域特定任务时，这些模型可能会表现出局限性，甚至产生错误或虚幻的回答。这突显了将领域知识融入 LLMs 的必要性，使它们能更好地应对复杂的行业术语，展现出更细致的上下文理解，并减少生成虚假信息的风险。

在这篇文章中，我们将深入探讨将领域知识注入大型语言模型（LLMs）的几种策略和技术，通过添加片段使其在特定专业背景下表现最佳。
