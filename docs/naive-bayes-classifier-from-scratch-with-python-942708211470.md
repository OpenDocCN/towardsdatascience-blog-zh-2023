# 从零开始的朴素贝叶斯分类器，使用 Python

> 原文：[`towardsdatascience.com/naive-bayes-classifier-from-scratch-with-python-942708211470?source=collection_archive---------7-----------------------#2023-01-04`](https://towardsdatascience.com/naive-bayes-classifier-from-scratch-with-python-942708211470?source=collection_archive---------7-----------------------#2023-01-04)

## 从理论到实践，运用贝叶斯定理

[](https://piero-paialunga.medium.com/?source=post_page-----942708211470--------------------------------)![Piero Paialunga](https://piero-paialunga.medium.com/?source=post_page-----942708211470--------------------------------)[](https://towardsdatascience.com/?source=post_page-----942708211470--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----942708211470--------------------------------) [Piero Paialunga](https://piero-paialunga.medium.com/?source=post_page-----942708211470--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F254e653181d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-classifier-from-scratch-with-python-942708211470&user=Piero+Paialunga&userId=254e653181d2&source=post_page-254e653181d2----942708211470---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----942708211470--------------------------------) ·10 分钟阅读·2023 年 1 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F942708211470&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-classifier-from-scratch-with-python-942708211470&user=Piero+Paialunga&userId=254e653181d2&source=-----942708211470---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F942708211470&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnaive-bayes-classifier-from-scratch-with-python-942708211470&source=-----942708211470---------------------bookmark_footer-----------)![](img/0861ef7d4e87fc5bedc5552305cb25bc.png)

图片由 [Joel Abraham](https://unsplash.com/@joe_27?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/8RRYJg26Wr4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

数学和物理充满了定理、方程、原则、公设和推论。当我开始学习物理时，我记得有一段时间，我学习的所有课程都有相同的结构：

A. 定义**基本假设**

B. 使用数学构建下一个“**墙壁砖块**”

C. **一个接一个地叠放砖块**，直到所有部分拼接成一个优雅、美丽的世界模型。

让我们以我第一次学习的物理课程为例：**微积分**。

1\. 你从**集合**和**数字**的基本假设开始。你开始定义自然数、整数、实数和复数。

2\. 从那里你开始定义**函数**，它不过是一个从空间 A（假设为 N 维实空间）到空间 B（假设为 1 维实空间）的**映射**。

3\. 然后你开始**研究**这些函数。于是你开始分析它们的极小值、极大值和鞍点。你偶然间（*哎呀！*）了解到**“导数”**的概念。

4\. 然后你了解如何**积分**一个函数，这正好是**导数**的反操作。

5\. 然后你将这些东西与**微分方程**结合起来。
