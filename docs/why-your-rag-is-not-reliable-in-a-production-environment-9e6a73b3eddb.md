# 为什么你的 RAG 在生产环境中不可靠

> 原文：[`towardsdatascience.com/why-your-rag-is-not-reliable-in-a-production-environment-9e6a73b3eddb?source=collection_archive---------0-----------------------#2023-10-12`](https://towardsdatascience.com/why-your-rag-is-not-reliable-in-a-production-environment-9e6a73b3eddb?source=collection_archive---------0-----------------------#2023-10-12)

## 以及你应该如何正确调整它

[](https://ahmedbesbes.medium.com/?source=post_page-----9e6a73b3eddb--------------------------------)![Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----9e6a73b3eddb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9e6a73b3eddb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9e6a73b3eddb--------------------------------) [Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----9e6a73b3eddb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fadc8ea174c69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-your-rag-is-not-reliable-in-a-production-environment-9e6a73b3eddb&user=Ahmed+Besbes&userId=adc8ea174c69&source=post_page-adc8ea174c69----9e6a73b3eddb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9e6a73b3eddb--------------------------------) ·7 分钟阅读·2023 年 10 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9e6a73b3eddb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-your-rag-is-not-reliable-in-a-production-environment-9e6a73b3eddb&user=Ahmed+Besbes&userId=adc8ea174c69&source=-----9e6a73b3eddb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9e6a73b3eddb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-your-rag-is-not-reliable-in-a-production-environment-9e6a73b3eddb&source=-----9e6a73b3eddb---------------------bookmark_footer-----------)![](img/96beae64de9f7f71263e41a7767a45fe.png)

图片由 [Ivan Jermakov](https://unsplash.com/@ivanjermakov?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着大规模语言模型（LLMs）的兴起，检索增强生成（RAG）[框架](https://arxiv.org/abs/2005.11401)也逐渐受到关注，因为它使得在数据上构建问答系统成为可能。

我们都见过那些聊天机器人与 PDF 或电子邮件对话的演示。

尽管这些系统确实令人印象深刻，但如果不进行调整和实验，它们在生产环境中可能不够可靠。

> ***在这篇文章中，我探讨了 RAG 框架背后的问题，并介绍了一些提高其性能的技巧。这些技巧包括利用文档元数据到微调超参数。***

这些发现基于我作为一名机器学习工程师的经验，我仍在学习这项技术并在制药行业构建 RAG。

不再多说，让我们来看看 🔍

> 如果你对提高构建机器学习系统生产力的实用技巧感兴趣，可以订阅我的 [通讯](https://thetechbuffet.substack.com/)。
> 
> 我每周发送编程和系统设计的见解，以帮助你更快地推出 AI 产品。

# RAG 简介 ⚙️

让我们首先掌握基础知识。
