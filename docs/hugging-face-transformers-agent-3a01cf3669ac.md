# 🤗Hugging Face Transformers 代理

> 原文：[`towardsdatascience.com/hugging-face-transformers-agent-3a01cf3669ac?source=collection_archive---------0-----------------------#2023-05-14`](https://towardsdatascience.com/hugging-face-transformers-agent-3a01cf3669ac?source=collection_archive---------0-----------------------#2023-05-14)

## 与 🦜🔗LangChain 代理的比较

[](https://sophiamyang.medium.com/?source=post_page-----3a01cf3669ac--------------------------------)![Sophia Yang, Ph.D.](https://sophiamyang.medium.com/?source=post_page-----3a01cf3669ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3a01cf3669ac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3a01cf3669ac--------------------------------) [Sophia Yang, Ph.D.](https://sophiamyang.medium.com/?source=post_page-----3a01cf3669ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae9cae9cbcd2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhugging-face-transformers-agent-3a01cf3669ac&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=post_page-ae9cae9cbcd2----3a01cf3669ac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3a01cf3669ac--------------------------------) ·5 min 阅读·2023 年 5 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3a01cf3669ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhugging-face-transformers-agent-3a01cf3669ac&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=-----3a01cf3669ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3a01cf3669ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhugging-face-transformers-agent-3a01cf3669ac&source=-----3a01cf3669ac---------------------bookmark_footer-----------)

就在两天前，🤗Hugging Face 发布了 Transformers 代理——一个利用自然语言从精心挑选的工具中选择工具并完成各种任务的代理。听起来熟悉吗？是的，因为它很像 🦜🔗LangChain 工具和代理。在这篇博客文章中，我将深入探讨 Transformers 代理是什么以及它与 🦜🔗LangChain 代理的比较。

# 尝试代码：

你可以在[这个 colab](https://colab.research.google.com/drive/1c7MHD-T1forUPGcC_jlwsIptOzpG3hSj)（由 Hugging Face 提供）中尝试代码。

# **什么是 Transformers 代理？**

> 简而言之，它提供了一个基于 transformers 的自然语言 API：我们定义了一组精心挑选的工具，并设计了一个代理来解释自然语言并使用这些工具。

我可以想象 HuggingFace 的工程师们会
