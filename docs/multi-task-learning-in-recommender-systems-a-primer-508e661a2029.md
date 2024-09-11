# 推荐系统中的多任务学习：入门指南

> 原文：[`towardsdatascience.com/multi-task-learning-in-recommender-systems-a-primer-508e661a2029?source=collection_archive---------17-----------------------#2023-07-25`](https://towardsdatascience.com/multi-task-learning-in-recommender-systems-a-primer-508e661a2029?source=collection_archive---------17-----------------------#2023-07-25)

## 尝试完成所有任务的算法背后的科学与工程

[](https://medium.com/@samuel.flender?source=post_page-----508e661a2029--------------------------------)![Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----508e661a2029--------------------------------)[](https://towardsdatascience.com/?source=post_page-----508e661a2029--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----508e661a2029--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----508e661a2029--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-task-learning-in-recommender-systems-a-primer-508e661a2029&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----508e661a2029---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----508e661a2029--------------------------------) · 8 分钟阅读·2023 年 7 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F508e661a2029&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-task-learning-in-recommender-systems-a-primer-508e661a2029&user=Samuel+Flender&userId=ce56d9dcd568&source=-----508e661a2029---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F508e661a2029&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-task-learning-in-recommender-systems-a-primer-508e661a2029&source=-----508e661a2029---------------------bookmark_footer-----------)![](img/14dcc1089b980b3a39af30f387ebe796.png)

图片来源：[Mike Kononov](https://unsplash.com/@mikofilm?utm_source=medium&utm_medium=referral) 在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

尽管多任务学习在计算机视觉和自然语言处理领域已被广泛应用，但在现代推荐系统中的使用仍相对较新，因此尚未得到充分理解。

在这篇文章中，我们将深入探讨多任务推荐系统中的一些最重要的设计考虑因素和最新的研究突破。我们将涵盖

+   我们首先为什么需要多任务推荐系统，

+   正向和负向迁移：多任务学习者中的关键挑战，

+   硬参数共享和专家建模，以及

+   辅助学习：添加新任务以提高主要任务的效果的思想。

让我们开始吧。

## 为什么需要多任务推荐系统？

多任务推荐系统的主要优势在于它们能够同时解决多个业务目标。例如，在视频推荐系统中，我们可能希望优化点击率，但也希望优化观看时间、点赞、分享、评论或其他形式的用户互动。在这种情况下，…
