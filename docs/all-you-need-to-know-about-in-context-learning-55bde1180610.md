# 关于上下文学习你需要知道的一切

> 原文：[`towardsdatascience.com/all-you-need-to-know-about-in-context-learning-55bde1180610?source=collection_archive---------7-----------------------#2023-07-25`](https://towardsdatascience.com/all-you-need-to-know-about-in-context-learning-55bde1180610?source=collection_archive---------7-----------------------#2023-07-25)

## | 上下文学习 | 大型语言模型 | LLMs

## 什么是上下文学习，它是如何工作的，以及是什么让大型语言模型如此强大

[](https://salvatore-raieli.medium.com/?source=post_page-----55bde1180610--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----55bde1180610--------------------------------)[](https://towardsdatascience.com/?source=post_page-----55bde1180610--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----55bde1180610--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----55bde1180610--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1a08d9452cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-you-need-to-know-about-in-context-learning-55bde1180610&user=Salvatore+Raieli&userId=f1a08d9452cd&source=post_page-f1a08d9452cd----55bde1180610---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----55bde1180610--------------------------------) · 19 分钟阅读 · 2023 年 7 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F55bde1180610&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-you-need-to-know-about-in-context-learning-55bde1180610&user=Salvatore+Raieli&userId=f1a08d9452cd&source=-----55bde1180610---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F55bde1180610&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fall-you-need-to-know-about-in-context-learning-55bde1180610&source=-----55bde1180610---------------------bookmark_footer-----------)![](img/7f1b46371aebe6ca8e4684f7f9be78fa.png)

图片来源：[🇸🇮 Janko Ferlič](https://unsplash.com/ko/@itfeelslikefilm?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> “对我来说，上下文是关键——从中产生对一切的理解。” — Kenneth Noland

上下文学习（ICL）是模型技能中最令人惊讶的之一。在 GPT-3 中观察到这一点引起了作者的注意。**ICL 究竟是什么？更重要的是，它是如何产生的？**

本文分为不同的部分，我们将回答这些问题：

+   什么是上下文学习（ICL）？为什么这很有趣？为什么它有用？

+   ICL 的神秘：它是如何工作的？是训练数据？是提示？还是架构？

+   ICL 的未来是什么？剩下的挑战有哪些？

查看文章末尾的参考文献列表，我还提供了一些深入探讨主题的建议。

# 什么是上下文学习（ICL）？
