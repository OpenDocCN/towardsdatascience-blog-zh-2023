# 《投机取样——直观且详尽的解释》

> 原文：[https://towardsdatascience.com/speculative-sampling-intuitively-and-exhaustively-explained-2daca347dbb9?source=collection_archive---------10-----------------------#2023-12-15](https://towardsdatascience.com/speculative-sampling-intuitively-and-exhaustively-explained-2daca347dbb9?source=collection_archive---------10-----------------------#2023-12-15)

## 机器学习 | 自然语言处理 | 数据科学

## 探索一种使语言模型速度提升3倍的插入策略

[](https://medium.com/@danielwarfield1?source=post_page-----2daca347dbb9--------------------------------)[![Daniel Warfield](../Images/c1c8b4dd514f6813e08e401401324bca.png)](https://medium.com/@danielwarfield1?source=post_page-----2daca347dbb9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2daca347dbb9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2daca347dbb9--------------------------------) [Daniel Warfield](https://medium.com/@danielwarfield1?source=post_page-----2daca347dbb9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbdc4072cbfdc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeculative-sampling-intuitively-and-exhaustively-explained-2daca347dbb9&user=Daniel+Warfield&userId=bdc4072cbfdc&source=post_page-bdc4072cbfdc----2daca347dbb9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2daca347dbb9--------------------------------) · 12分钟阅读 · 2023年12月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2daca347dbb9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeculative-sampling-intuitively-and-exhaustively-explained-2daca347dbb9&user=Daniel+Warfield&userId=bdc4072cbfdc&source=-----2daca347dbb9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2daca347dbb9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspeculative-sampling-intuitively-and-exhaustively-explained-2daca347dbb9&source=-----2daca347dbb9---------------------bookmark_footer-----------)![](../Images/c21285038373258e14b9f0b7ea4033a1.png)

“投机者”由Daniel Warfield使用MidJourney和Affinity Design 2创作。除非另有说明，所有图片均为作者所用。

在本文中，我们将讨论“投机取样”，这是一种在不影响性能的情况下加快文本生成速度和降低成本的策略。在此过程中，我们将深入探讨语言模型的一些更微妙的方面。

![](../Images/37be7902d8b7476df84c826007418377.png)

在各种文本生成任务中使用猜测性采样的实证结果。注意，在所有情况下，生成时间都显著缩短了。[来源](https://arxiv.org/pdf/2211.17192.pdf)

首先，我们将讨论一个正在拖慢现代语言模型的主要问题，然后我们将建立对如何通过猜测性采样优雅地加速它们的直观理解，接着我们将在 Python 中从头实现猜测性采样。

**这对谁有用？** 对自然语言处理（NLP）或前沿 AI 进展感兴趣的任何人。

**这篇文章有多先进？** 这篇文章中的概念对于机器学习爱好者是可理解的，同时又足够前沿，能够引起资深数据科学家的兴趣。文末的代码可能对开发者有用。
