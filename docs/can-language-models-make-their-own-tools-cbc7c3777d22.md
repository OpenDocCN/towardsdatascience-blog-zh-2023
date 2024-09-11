# 语言模型能否自制工具？

> 原文：[`towardsdatascience.com/can-language-models-make-their-own-tools-cbc7c3777d22?source=collection_archive---------1-----------------------#2023-09-17`](https://towardsdatascience.com/can-language-models-make-their-own-tools-cbc7c3777d22?source=collection_archive---------1-----------------------#2023-09-17)

## LaTM、CREATOR 和其他用于 LLM 工具的闭环框架……

[](https://wolfecameron.medium.com/?source=post_page-----cbc7c3777d22--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----cbc7c3777d22--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cbc7c3777d22--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cbc7c3777d22--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----cbc7c3777d22--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-language-models-make-their-own-tools-cbc7c3777d22&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----cbc7c3777d22---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cbc7c3777d22--------------------------------) ·16 分钟阅读·2023 年 9 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcbc7c3777d22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-language-models-make-their-own-tools-cbc7c3777d22&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----cbc7c3777d22---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcbc7c3777d22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-language-models-make-their-own-tools-cbc7c3777d22&source=-----cbc7c3777d22---------------------bookmark_footer-----------)![](img/dcbddb8cfa90114c42e1cb68195ad874.png)

（图片由 [Todd Quackenbush](https://unsplash.com/@toddquackenbush?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/IClZBVw5W5A?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

在最近的综述中，我们探讨了将外部工具增强大型语言模型（LLMs）的实用性。这些模型可以被教导以多种方式利用工具。然而，我们应该认识到，现有的工具跟随 LLMs 仅利用了有限的一组潜在工具[3]，*而我们希望用 LLMs 解决的问题范围几乎是无穷无尽的！* 鉴于此，这种范式显然有限——我们总能找到需要尚不存在的工具的场景。在本综述中，我们将探讨近期的研究，这些研究旨在通过赋予 LLMs 制造自己工具的能力来解决这个问题。这种方法与人类生活形成了有趣的类比，因为制造工具的能力导致了重大的技术进步。现在，我们将探讨类似技术对 LLMs 进化的影响。

> “根据人类进化里程碑的经验教训，一个关键的转折点是人类获得了制造工具的能力，以应对新出现的挑战。我们开始初步探索将这一进化概念应用于 LLM 领域。” *— 摘自[1]*
