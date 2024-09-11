# 开源大型语言模型的历史：模仿与对齐（第三部分）

> 原文：[`towardsdatascience.com/the-history-of-open-source-llms-imitation-and-alignment-part-three-603d709c7aa5?source=collection_archive---------12-----------------------#2023-11-28`](https://towardsdatascience.com/the-history-of-open-source-llms-imitation-and-alignment-part-three-603d709c7aa5?source=collection_archive---------12-----------------------#2023-11-28)

## 开源大型语言模型需要对齐才能真正卓越…

[](https://wolfecameron.medium.com/?source=post_page-----603d709c7aa5--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----603d709c7aa5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----603d709c7aa5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----603d709c7aa5--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----603d709c7aa5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-history-of-open-source-llms-imitation-and-alignment-part-three-603d709c7aa5&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----603d709c7aa5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----603d709c7aa5--------------------------------) · 20 分钟阅读 · 2023 年 11 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F603d709c7aa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-history-of-open-source-llms-imitation-and-alignment-part-three-603d709c7aa5&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----603d709c7aa5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F603d709c7aa5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-history-of-open-source-llms-imitation-and-alignment-part-three-603d709c7aa5&source=-----603d709c7aa5---------------------bookmark_footer-----------)![](img/09529330477dcea0f682d4764a7fe0fc.png)

（照片由 [Joanna Kosinska](https://unsplash.com/@joannakosinska?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，出处：[Unsplash](https://unsplash.com/photos/brown-paper-and-black-pen-B6yDtYs2IgY?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)）

大多数关于开源大语言模型（LLMs）的先前研究都集中于创建预训练的基础模型。然而，这些模型并没有经过任何微调，因此由于缺乏对齐，它们无法与顶级闭源 LLMs（例如，ChatGPT 或 Claude）的质量相匹配。付费模型通过 SFT 和 RLHF 等技术进行广泛对齐，这大大提高了它们的可用性。相比之下，开源模型通常只用较小的公共数据集进行有限的微调。然而，在本概述中，我们将探讨最近的研究，旨在通过更广泛的微调和对齐来提高开源 LLMs 的质量。

![](img/1a77e11ba9dd423a60dcfd943e2a772e.png)

（来自 [1, 2, 12]）

本概述是我关于开源 LLMs 历史系列的第三部分（也是最后一部分）。在第一部分中，我们探讨了创建开源语言模型的初步尝试。尽管这些最初的预训练 LLMs 表现不佳，但它们很快被更好的开源基础模型所取代，我们在[第二部分](https://medium.com/towards-data-science/the-history-of-open-source-llms-better-base-models-part-two-6ca51ae74ebe)中进行了介绍…
