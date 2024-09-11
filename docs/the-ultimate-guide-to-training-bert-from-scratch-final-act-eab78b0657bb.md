# 从零开始训练 BERT 的终极指南：最终篇

> 原文：[https://towardsdatascience.com/the-ultimate-guide-to-training-bert-from-scratch-final-act-eab78b0657bb?source=collection_archive---------5-----------------------#2023-12-18](https://towardsdatascience.com/the-ultimate-guide-to-training-bert-from-scratch-final-act-eab78b0657bb?source=collection_archive---------5-----------------------#2023-12-18)

## 最终前沿：构建和训练你的 BERT 模型

[](https://dpoulopoulos.medium.com/?source=post_page-----eab78b0657bb--------------------------------)[![Dimitris Poulopoulos](../Images/ce535a1679779f5a2ec8b024e6691e50.png)](https://dpoulopoulos.medium.com/?source=post_page-----eab78b0657bb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eab78b0657bb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eab78b0657bb--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----eab78b0657bb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-final-act-eab78b0657bb&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----eab78b0657bb---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eab78b0657bb--------------------------------) · 7分钟阅读 · 2023年12月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feab78b0657bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-final-act-eab78b0657bb&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----eab78b0657bb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feab78b0657bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-final-act-eab78b0657bb&source=-----eab78b0657bb---------------------bookmark_footer-----------)![](../Images/548c617f9d3d11635cc30ccc45e1d106.png)

图片来源：[Rob Laughter](https://unsplash.com/@roblaughter?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 本博客文章结束了我们关于从零开始训练 BERT 的系列。如果需要背景信息和完整理解，请参考系列的 [第一部分](/the-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f)、[第二部分](/the-ultimate-guide-to-training-bert-from-scratch-the-tokenizer-ddf30f124822) 和 [第三部分](/the-ultimate-guide-to-training-bert-from-scratch-prepare-the-dataset-beaae6febfd5)。

当 BERT 在 2018 年登上舞台时，它在自然语言处理（NLP）领域引发了巨大的波澜。许多人将这一时刻视为 NLP 自身的 ImageNet 时刻，类似于 2012 年深度神经网络对计算机视觉和更广泛的机器学习领域带来的变革。

五年过去了，这一预言依然成立。基于 Transformer 的大型语言模型（LLMs）不仅仅是炫目的新玩具，它们正在重塑整个领域。这些模型正在改变我们的工作方式，并彻底革新我们获取信息的方式，它们是无数新兴初创公司背后的核心技术，这些公司旨在挖掘其尚未开发的潜力。

这就是我决定写这一系列博客文章的原因，深入探讨 BERT 的世界以及如何从零开始训练自己的模型。关键不仅仅是完成任务——毕竟，你可以轻松地在 Hugging Face Hub 上找到预训练的 BERT 模型。真正的魔力在于理解这个突破性模型的内部工作原理并应用……
