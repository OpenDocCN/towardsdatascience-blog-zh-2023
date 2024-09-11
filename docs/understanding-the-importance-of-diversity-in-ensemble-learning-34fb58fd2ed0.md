# 理解集成学习中多样性的重要性

> 原文：[`towardsdatascience.com/understanding-the-importance-of-diversity-in-ensemble-learning-34fb58fd2ed0?source=collection_archive---------9-----------------------#2023-01-02`](https://towardsdatascience.com/understanding-the-importance-of-diversity-in-ensemble-learning-34fb58fd2ed0?source=collection_archive---------9-----------------------#2023-01-02)

## 提升集成学习性能的多样性作用

[](https://donatoriccio.medium.com/?source=post_page-----34fb58fd2ed0--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----34fb58fd2ed0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----34fb58fd2ed0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----34fb58fd2ed0--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----34fb58fd2ed0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe384fc71d292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-importance-of-diversity-in-ensemble-learning-34fb58fd2ed0&user=Donato+Riccio&userId=e384fc71d292&source=post_page-e384fc71d292----34fb58fd2ed0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----34fb58fd2ed0--------------------------------) ·9 分钟阅读·2023 年 1 月 2 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F34fb58fd2ed0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-importance-of-diversity-in-ensemble-learning-34fb58fd2ed0&user=Donato+Riccio&userId=e384fc71d292&source=-----34fb58fd2ed0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F34fb58fd2ed0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-the-importance-of-diversity-in-ensemble-learning-34fb58fd2ed0&source=-----34fb58fd2ed0---------------------bookmark_footer-----------)![](img/dab0c86b9beea259e2d15c4ae808d51d.png)

照片由 [Mulyadi](https://unsplash.com/@mullyadii?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

集成学习是一种机器学习技术，通过结合多个模型来实现更好的结果。近年来，随着计算速度的提升，集成学习成为解决机器学习领域复杂问题的最有效技术之一。这种方法被用于大多数获胜的机器学习竞赛解决方案中，奖金高达 10 万美元。

我们如何选择最佳的集成成员，将它们结合成一个更强大的模型？在本文中，我们将探讨如何选择要集成的模型以及如何对它们进行汇总。

# **什么是多样性？**

集成学习基于将多个模型结合的概念，这些模型称为*弱学习器*。这个名字来源于这样的想法：单个集成成员不需要非常准确。只要它们比随机模型稍微好一点，结合它们就是有益的。多样性是集成学习中的一个重要概念，指的是集成中的各个模型的预测**应尽可能不同**。这是因为不同的模型可能会产生不同类型的错误。通过...
