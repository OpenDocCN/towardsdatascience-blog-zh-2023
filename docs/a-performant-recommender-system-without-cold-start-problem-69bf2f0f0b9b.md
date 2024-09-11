# 一个高效的推荐系统，无需冷启动问题

> 原文：[`towardsdatascience.com/a-performant-recommender-system-without-cold-start-problem-69bf2f0f0b9b?source=collection_archive---------11-----------------------#2023-01-31`](https://towardsdatascience.com/a-performant-recommender-system-without-cold-start-problem-69bf2f0f0b9b?source=collection_archive---------11-----------------------#2023-01-31)

## [推荐系统](https://medium.com/tag/recommendation-system)

## 当协作和基于内容的推荐系统融合时

[](https://dr-robert-kuebler.medium.com/?source=post_page-----69bf2f0f0b9b--------------------------------)![Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----69bf2f0f0b9b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----69bf2f0f0b9b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----69bf2f0f0b9b--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----69bf2f0f0b9b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-performant-recommender-system-without-cold-start-problem-69bf2f0f0b9b&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----69bf2f0f0b9b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----69bf2f0f0b9b--------------------------------) ·11 分钟阅读·2023 年 1 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F69bf2f0f0b9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-performant-recommender-system-without-cold-start-problem-69bf2f0f0b9b&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=-----69bf2f0f0b9b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F69bf2f0f0b9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-performant-recommender-system-without-cold-start-problem-69bf2f0f0b9b&source=-----69bf2f0f0b9b---------------------bookmark_footer-----------)![](img/c0e674a93dd06ef0ac51647be57bbd22.png)

照片由 [Ivan Aleksic](https://unsplash.com/@ivalex?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

也许最著名的推荐系统就是所谓的**矩阵分解**。在这个**协同**推荐系统中，用户和项目通过**嵌入**来表示，嵌入不过是一个数字向量。直观地说，用户和项目嵌入的点积应该产生用户对该项目的评分。

如果你还不熟悉这些概念，我建议（😉）在继续之前阅读我的另一篇文章，因为我在那里解释了许多概念和代码片段。

[## 嵌入式推荐系统介绍](https://towardsdatascience.com/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----69bf2f0f0b9b--------------------------------)

### 学习在 TensorFlow 中构建一个简单的推荐系统

[towardsdatascience.com](https://towardsdatascience.com/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----69bf2f0f0b9b--------------------------------)

# 冷启动问题

纯协同推荐系统如矩阵分解的优势在于，即使没有太多关于用户和你想推荐的电影/文章/项目的数据，你通常也可以立即构建它们。你只需要知道谁评价了什么，以及如何评价；例如，用户*B* 给电影*Y* 评分为…
