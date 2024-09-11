# 《从头开始训练 BERT 的终极指南：准备数据集》

> 原文：[https://towardsdatascience.com/the-ultimate-guide-to-training-bert-from-scratch-prepare-the-dataset-beaae6febfd5?source=collection_archive---------3-----------------------#2023-09-14](https://towardsdatascience.com/the-ultimate-guide-to-training-bert-from-scratch-prepare-the-dataset-beaae6febfd5?source=collection_archive---------3-----------------------#2023-09-14)

## 数据准备：深入探讨、优化你的过程，并发现如何攻克最关键的步骤

[](https://dpoulopoulos.medium.com/?source=post_page-----beaae6febfd5--------------------------------)[![Dimitris Poulopoulos](../Images/ce535a1679779f5a2ec8b024e6691e50.png)](https://dpoulopoulos.medium.com/?source=post_page-----beaae6febfd5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----beaae6febfd5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----beaae6febfd5--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----beaae6febfd5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-prepare-the-dataset-beaae6febfd5&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----beaae6febfd5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----beaae6febfd5--------------------------------) ·13 分钟阅读·2023 年 9 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbeaae6febfd5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-prepare-the-dataset-beaae6febfd5&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----beaae6febfd5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbeaae6febfd5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ultimate-guide-to-training-bert-from-scratch-prepare-the-dataset-beaae6febfd5&source=-----beaae6febfd5---------------------bookmark_footer-----------)![](../Images/34a45dbd982191c4d64c0848ec5fbae1.png)

照片由 [Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 本故事的 [第一部分](/the-ultimate-guide-to-training-bert-from-scratch-introduction-b048682c795f)，[第二部分](/the-ultimate-guide-to-training-bert-from-scratch-the-tokenizer-ddf30f124822)，和 [第四部分](/the-ultimate-guide-to-training-bert-from-scratch-final-act-eab78b0657bb) 现已发布。

想象一下，你花了一整天来微调 BERT，却碰到性能瓶颈，让你抓耳挠腮。你深入检查代码，发现问题的根源：你在准备特征和标签时做得不好。就这样，宝贵的十小时 GPU 时间化为乌有。

说实话，*设置你的数据集不仅仅是另一个步骤——它是你整个训练流程的工程基石*。有人甚至认为，一旦你的数据集准备好，其余的基本都是模板化的：喂入模型，计算损失，进行反向传播，并更新模型权重。

![](../Images/b9ce304927937c39d5bde10ec7fd6689.png)

训练流程 — 图片作者

在这个故事中，**我们将深入探讨为 BERT 准备数据的过程，为最终目标奠定基础：从零开始训练一个 BERT 模型。**
