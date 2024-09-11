# 机器学习实验的艺术

> 原文：[`towardsdatascience.com/how-to-run-machine-learning-experiments-that-really-matter-2ba0a88bc579?source=collection_archive---------15-----------------------#2023-01-17`](https://towardsdatascience.com/how-to-run-machine-learning-experiments-that-really-matter-2ba0a88bc579?source=collection_archive---------15-----------------------#2023-01-17)

## 5 种简单策略，帮助你充分利用机器学习实验

[](https://medium.com/@samuel.flender?source=post_page-----2ba0a88bc579--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----2ba0a88bc579--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2ba0a88bc579--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2ba0a88bc579--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----2ba0a88bc579--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-run-machine-learning-experiments-that-really-matter-2ba0a88bc579&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----2ba0a88bc579---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2ba0a88bc579--------------------------------) ·7 分钟阅读·2023 年 1 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2ba0a88bc579&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-run-machine-learning-experiments-that-really-matter-2ba0a88bc579&user=Samuel+Flender&userId=ce56d9dcd568&source=-----2ba0a88bc579---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2ba0a88bc579&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-run-machine-learning-experiments-that-really-matter-2ba0a88bc579&source=-----2ba0a88bc579---------------------bookmark_footer-----------)![](img/c90986e2d4e211752d520f635a80b495.png)

照片由 [Oudom Pravat](https://unsplash.com/@opravat?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

实验是机器学习行业的核心。我们之所以取得进展，是因为我们进行实验。

然而，并不是所有实验都同样有意义。有些实验带来的商业影响比其他实验更大。然而，专注于影响的实验选择、执行和迭代的艺术通常没有涵盖在标准的机器学习课程中。

这会造成很多困惑。新的机器学习从业者可能会产生这样的印象：你应该简单地把所有东西都扔到问题上，然后“看看哪个有效”。事情并不是这样运作的。

明确来说，我不是在讨论离线和在线测试及其变体的统计数据，例如 A/B 测试。我在讲的是实验完成前后发生的事情。我们如何选择实验内容？如果结果是负面的，我们该怎么办？我们如何以最有效的方式进行迭代？

更广泛地说，你如何从机器学习实验中获得最大收益？这里有 5 个简单的策略你可以采纳。

# 1 — 知道何时进行实验

作为机器学习从业者，你脑海中总会有无数问题。比如，我们如果去掉这个特性会怎么样……
