# 正确处理推荐系统中的采样偏差

> 原文：[`towardsdatascience.com/correct-sampling-bias-for-recommender-systems-d2f6d9fdddec?source=collection_archive---------1-----------------------#2023-10-01`](https://towardsdatascience.com/correct-sampling-bias-for-recommender-systems-d2f6d9fdddec?source=collection_archive---------1-----------------------#2023-10-01)

## 什么是推荐系统中的采样偏差，如何纠正

[](https://medium.com/@vuphuongthao9611?source=post_page-----d2f6d9fdddec--------------------------------)![Thao Vu](https://medium.com/@vuphuongthao9611?source=post_page-----d2f6d9fdddec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d2f6d9fdddec--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2f6d9fdddec--------------------------------) [Thao Vu](https://medium.com/@vuphuongthao9611?source=post_page-----d2f6d9fdddec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa836aac352ca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrect-sampling-bias-for-recommender-systems-d2f6d9fdddec&user=Thao+Vu&userId=a836aac352ca&source=post_page-a836aac352ca----d2f6d9fdddec---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2f6d9fdddec--------------------------------) ·8 分钟阅读·2023 年 10 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd2f6d9fdddec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrect-sampling-bias-for-recommender-systems-d2f6d9fdddec&user=Thao+Vu&userId=a836aac352ca&source=-----d2f6d9fdddec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd2f6d9fdddec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcorrect-sampling-bias-for-recommender-systems-d2f6d9fdddec&source=-----d2f6d9fdddec---------------------bookmark_footer-----------)![](img/c5bbbc4ae0707318b5df926332f23574.png)

图片来源：[NordWood Themes](https://unsplash.com/@nordwood?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **介绍**

推荐在我们的数字生活中无处不在，从电子商务巨头到流媒体服务。然而，每个大型推荐系统背后都隐藏着一个可能显著影响其有效性的挑战——采样偏差。

在这篇文章中，我将介绍采样偏差如何在训练推荐模型时发生，以及我们如何在实践中解决这个问题。

让我们深入探讨吧！

# 双塔推荐系统

一般来说，我们可以将推荐问题表述为：给定**查询 x**（可能包含用户信息、上下文、之前点击的项目等***），找到用户可能感兴趣的项目集合***{y1,.., yk}***。

大规模推荐系统的主要挑战之一是低延迟要求。然而，用户和项目池是庞大而动态的，因此对每个候选项进行评分并贪婪地寻找最佳项是不可能的。因此，为了满足延迟要求，推荐系统通常被分解为两个主要阶段：检索和排序。
