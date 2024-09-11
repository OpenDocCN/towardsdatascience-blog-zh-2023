# 基于嵌入的推荐系统简介

> 原文：[`towardsdatascience.com/introduction-to-embedding-based-recommender-systems-956faceb1919?source=collection_archive---------1-----------------------#2023-01-25`](https://towardsdatascience.com/introduction-to-embedding-based-recommender-systems-956faceb1919?source=collection_archive---------1-----------------------#2023-01-25)

## [推荐系统](https://medium.com/tag/recommendation-system)

## 学习如何在 TensorFlow 中构建一个简单的矩阵分解推荐系统

[](https://dr-robert-kuebler.medium.com/?source=post_page-----956faceb1919--------------------------------)![Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----956faceb1919--------------------------------)[](https://towardsdatascience.com/?source=post_page-----956faceb1919--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----956faceb1919--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----956faceb1919--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-embedding-based-recommender-systems-956faceb1919&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----956faceb1919---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----956faceb1919--------------------------------) ·13 分钟阅读·2023 年 1 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F956faceb1919&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-embedding-based-recommender-systems-956faceb1919&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=-----956faceb1919---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F956faceb1919&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-embedding-based-recommender-systems-956faceb1919&source=-----956faceb1919---------------------bookmark_footer-----------)![](img/ff12d44a9188531303b168fa10bb28cf.png)

照片由 [Johannes Plenio](https://unsplash.com/es/@jplenio?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

它们无处不在：这些有时很棒、有时很差、有时甚至搞笑的**推荐**出现在像亚马逊、Netflix 或 Spotify 这样的大型网站上，告诉你接下来要买什么、观看什么或听什么。虽然**推荐系统**对我们用户很方便——我们可以得到尝试新事物的灵感——但**公司尤其受益**于此。

为了了解其程度，我们来看一下 Dietmar Jannach 和 Michael Jugovac [1] 的论文 [**测量推荐系统的商业价值**](https://arxiv.org/abs/1908.08328) 中的一些数据。从他们的论文中：

+   **Netflix：** “75% 的观看内容来自某种推荐” ([这个甚至来自 Medium!](https://netflixtechblog.com/netflix-recommendations-beyond-the-5-stars-part-1-55838468f429))

+   **YouTube：** “60% 的主页点击来自推荐”

+   **亚马逊：** “大约 35% 的销售额来自交叉销售（即推荐）”，其中 *their* 指的是亚马逊。

在这篇论文 [1] 中，你可以找到更多关于通过使用推荐系统所带来的点击率、用户参与度和销售额的有趣数据。
