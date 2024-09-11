# 倾向得分建模方法综述

> 原文：[https://towardsdatascience.com/a-review-of-propensity-score-modelling-approaches-19af9ecd60d9?source=collection_archive---------0-----------------------#2023-05-17](https://towardsdatascience.com/a-review-of-propensity-score-modelling-approaches-19af9ecd60d9?source=collection_archive---------0-----------------------#2023-05-17)

![](../Images/5f94d633b70e9981d01b64580c1b2e9b.png)

图片为作者自有。

## 使用倾向得分在因果推断建模中的不同方法回顾

[](https://drmattcrooks.medium.com/?source=post_page-----19af9ecd60d9--------------------------------)[![Matt Crooks](../Images/7e6c3828d4fc4ed5a7f21429a7db1b57.png)](https://drmattcrooks.medium.com/?source=post_page-----19af9ecd60d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----19af9ecd60d9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----19af9ecd60d9--------------------------------) [Matt Crooks](https://drmattcrooks.medium.com/?source=post_page-----19af9ecd60d9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e483050af3b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-review-of-propensity-score-modelling-approaches-19af9ecd60d9&user=Matt+Crooks&userId=8e483050af3b&source=post_page-8e483050af3b----19af9ecd60d9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----19af9ecd60d9--------------------------------) ·11 min read·May 17, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F19af9ecd60d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-review-of-propensity-score-modelling-approaches-19af9ecd60d9&user=Matt+Crooks&userId=8e483050af3b&source=-----19af9ecd60d9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F19af9ecd60d9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-review-of-propensity-score-modelling-approaches-19af9ecd60d9&source=-----19af9ecd60d9---------------------bookmark_footer-----------)

在本文中，我将介绍倾向得分的概念及其用途，然后介绍三种常见的方法论。我将讨论以下倾向得分模型：

+   *带替换的倾向得分匹配* (PSM)

+   *不带替换的倾向得分匹配* (PSM w/o)

+   *倒数倾向得分加权* (IPSW)

# 引言

评估特定干预或治疗的影响的最佳方法是进行随机对照试验（RCT）。在RCT中，你会将你的群体随机分成两个队列，并仅对其中一个应用干预——这成为你的治疗组。未接受干预的队列是你的对照组。由于控制组和治疗组的随机分配，两组之间的特征应该没有结构性差异。如果治疗后，治疗组的行为有所不同（即发生转化），那么我们可以得出这是干预的结果。

然而，有许多情况下无法进行RCT，包括但不限于…
