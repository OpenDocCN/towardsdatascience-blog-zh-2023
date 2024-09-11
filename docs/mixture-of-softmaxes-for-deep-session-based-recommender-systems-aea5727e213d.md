# 深度会话推荐系统中的 Softmax 混合

> 原文：[`towardsdatascience.com/mixture-of-softmaxes-for-deep-session-based-recommender-systems-aea5727e213d?source=collection_archive---------15-----------------------#2023-07-18`](https://towardsdatascience.com/mixture-of-softmaxes-for-deep-session-based-recommender-systems-aea5727e213d?source=collection_archive---------15-----------------------#2023-07-18)

## 现代的基于会话的深度推荐系统在某种程度上也会受到 softmax 瓶颈的限制，类似于它们的语言模型同类。

[](https://biarnes-adrien.medium.com/?source=post_page-----aea5727e213d--------------------------------)![Adrien Biarnes](https://biarnes-adrien.medium.com/?source=post_page-----aea5727e213d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aea5727e213d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aea5727e213d--------------------------------) [Adrien Biarnes](https://biarnes-adrien.medium.com/?source=post_page-----aea5727e213d--------------------------------)

·

[阅读更多](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F151fca431deb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixture-of-softmaxes-for-deep-session-based-recommender-systems-aea5727e213d&user=Adrien+Biarnes&userId=151fca431deb&source=post_page-151fca431deb----aea5727e213d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aea5727e213d--------------------------------) ·15 分钟阅读·2023 年 7 月 18 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faea5727e213d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixture-of-softmaxes-for-deep-session-based-recommender-systems-aea5727e213d&source=-----aea5727e213d---------------------bookmark_footer-----------)![](img/1e8ad6bdeba47c5c5c75fc5e30fcb86e.png)

图片由 [Preethi Viswanathan](https://unsplash.com/@sallybrad2016?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## TL;DR:

传统的 softmax 在充分建模像自然语言这种高度依赖上下文的任务时存在局限性。这种表达能力的限制称为 Softmax 瓶颈，可以通过矩阵分解和研究由此产生的矩阵秩来描述。

杨等人提出的[Softmax 混合方法](https://arxiv.org/pdf/1711.03953.pdf) [1] 通过增加网络的表现能力来克服瓶颈，而不会导致参数数量激增。

在这项工作中，我研究了这种限制是否也适用于深度会话推荐系统，以及 MoS 技术是否有益。我实现了该技术并将其应用于 Gru4Rec 架构，使用了[Movielens-25m 数据集](https://grouplens.org/datasets/movielens/25m/) [3]。结果表明，这取决于……

## 引言

在之前的一个职位上，当我在一个大规模的基于序列的深度推荐系统上工作时，产品团队指出了一个已识别的用例，此时推荐质量非常低。那个……
