# 开源 LLM 的历史：更好的基础模型（第二部分）

> 原文：[`towardsdatascience.com/the-history-of-open-source-llms-better-base-models-part-two-6ca51ae74ebe?source=collection_archive---------7-----------------------#2023-11-18`](https://towardsdatascience.com/the-history-of-open-source-llms-better-base-models-part-two-6ca51ae74ebe?source=collection_archive---------7-----------------------#2023-11-18)

## LLaMA、MPT、Falcon 和 LLaMA-2 如何将开源 LLM 推向舞台……

[](https://wolfecameron.medium.com/?source=post_page-----6ca51ae74ebe--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6ca51ae74ebe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ca51ae74ebe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ca51ae74ebe--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6ca51ae74ebe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-history-of-open-source-llms-better-base-models-part-two-6ca51ae74ebe&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----6ca51ae74ebe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ca51ae74ebe--------------------------------) ·16 分钟阅读·2023 年 11 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6ca51ae74ebe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-history-of-open-source-llms-better-base-models-part-two-6ca51ae74ebe&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----6ca51ae74ebe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6ca51ae74ebe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-history-of-open-source-llms-better-base-models-part-two-6ca51ae74ebe&source=-----6ca51ae74ebe---------------------bookmark_footer-----------)![](img/1cad7310b9d455e5c1b8cf3ad38f5a03.png)

(照片由 [Iñaki del Olmo](https://unsplash.com/@inakihxz?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/assorted-title-of-books-piled-in-the-shelves-NIJuEQw0RKg?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash))

开源大语言模型（LLMs）研究极具价值，因为它旨在使这一强大而有影响力的技术实现民主化。虽然开源 LLMs 现在已经被广泛使用和研究，但这个研究领域最初经历了一些难以克服的困难。即，开源 LLMs 最初表现不佳，并受到严重批评。在本概述中，我们将研究一项改变这种叙事的研究线，该研究通过向所有人提供高性能的预训练 LLMs 来改变局面。由于预训练语言模型成本高昂，我们在这里研究的模型特别具有影响力。在这些高性能基础模型被创建和发布后，许多人可以以边际附加成本使用这些模型进行研究。

> “考虑到训练方法看似简单，LLMs 的能力是令人惊叹的。” *— 引自[14]*

**当前系列。** 本概述是关于开源大语言模型（LLMs）历史的三部分系列中的第二部分。系列的[第一部分](https://medium.com/towards-data-science/the-history-of-open-source-llms-early-days-part-one-d782bcd8f7e8)概述了创建开源 LLMs 的初步尝试。在这里，我们将研究最流行的开源基础模型（即语言模型）……
