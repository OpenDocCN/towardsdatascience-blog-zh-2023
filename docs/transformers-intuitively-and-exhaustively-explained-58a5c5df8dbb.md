# 变换器 — 直观而全面的解释

> 原文：[`towardsdatascience.com/transformers-intuitively-and-exhaustively-explained-58a5c5df8dbb?source=collection_archive---------0-----------------------#2023-09-20`](https://towardsdatascience.com/transformers-intuitively-and-exhaustively-explained-58a5c5df8dbb?source=collection_archive---------0-----------------------#2023-09-20)

## 探索现代机器学习的浪潮：一步一步拆解变换器

[](https://medium.com/@danielwarfield1?source=post_page-----58a5c5df8dbb--------------------------------)![Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----58a5c5df8dbb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----58a5c5df8dbb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----58a5c5df8dbb--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----58a5c5df8dbb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-intuitively-and-exhaustively-explained-58a5c5df8dbb&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----58a5c5df8dbb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----58a5c5df8dbb--------------------------------) ·15 分钟阅读·2023 年 9 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F58a5c5df8dbb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-intuitively-and-exhaustively-explained-58a5c5df8dbb&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----58a5c5df8dbb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F58a5c5df8dbb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftransformers-intuitively-and-exhaustively-explained-58a5c5df8dbb&source=-----58a5c5df8dbb---------------------bookmark_footer-----------)![](img/edb20757aa401a62d13a932e35ee4b95.png)

图片由作者使用 MidJourney 制作。除非另有说明，否则所有图片均由作者提供。

在这篇文章中，你将了解变换器架构，这几乎是所有前沿大型语言模型的核心架构。我们将从一些相关的自然语言处理概念的简要时间轴开始，然后逐步深入变换器，揭示它的工作原理。

**这对谁有用？** 任何对自然语言处理（NLP）感兴趣的人。

**这篇文章有多高级？** 这不是一篇复杂的文章，但概念较多，因此对经验较少的数据科学家来说可能会感到有些望而生畏。

**前提条件：** 对标准神经网络有良好的工作理解。对嵌入、编码器和解码器有一些初步经验也可能会有所帮助。

# 从 NLP 到 Transformer 的简要时间线

以下部分包含在深入了解 Transformer 之前需要掌握的有用概念和技术。如果你感到自信，可以随意跳过。

## 词向量嵌入
