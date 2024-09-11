# 聚类算法介绍

> 原文：[`towardsdatascience.com/introduction-to-clustering-algorithms-76da35b5670a?source=collection_archive---------4-----------------------#2023-11-12`](https://towardsdatascience.com/introduction-to-clustering-algorithms-76da35b5670a?source=collection_archive---------4-----------------------#2023-11-12)

## 这是一个关于 10 种常用的层次聚类、划分聚类和基于密度的聚类算法的全面指南

[](https://kevindbabitz.medium.com/?source=post_page-----76da35b5670a--------------------------------)![Kevin Babitz](https://kevindbabitz.medium.com/?source=post_page-----76da35b5670a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----76da35b5670a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----76da35b5670a--------------------------------) [Kevin Babitz](https://kevindbabitz.medium.com/?source=post_page-----76da35b5670a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84c159909fbb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-clustering-algorithms-76da35b5670a&user=Kevin+Babitz&userId=84c159909fbb&source=post_page-84c159909fbb----76da35b5670a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----76da35b5670a--------------------------------) ·9 分钟阅读·2023 年 11 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F76da35b5670a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-clustering-algorithms-76da35b5670a&user=Kevin+Babitz&userId=84c159909fbb&source=-----76da35b5670a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F76da35b5670a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-clustering-algorithms-76da35b5670a&source=-----76da35b5670a---------------------bookmark_footer-----------)![](img/66a16934e96383d4113c300de64f6477.png)

照片由[Rod Long](https://unsplash.com/@rodlong?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)提供，发布在[Unsplash](https://unsplash.com/photos/penguin-walking-on-gray-sand-during-daytime-BSz0jJ172vM?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

# 介绍

聚类算法在数据分析中扮演着重要角色。这些无监督学习的探索性数据分析工具通过根据共享特征将数据点分类到不同组中，为知识发现提供系统。这使得可以识别出在原始数据中难以看到的关系和趋势。它们通过系统地增加对复杂和错综复杂数据集的理解，促进更为明智的决策。

本文将介绍三种聚类算法的基础知识：层次、分区和基于密度的聚类模型。我们将从定义每个类别开始。接下来，我们将深入探讨 10 种不同的聚类算法，提供定义、原始或有趣的研究论文链接、算法的优势，以及每种算法的 Python 代码片段。

## **目录**

层次聚类算法

+   Birch

+   Cure

+   ROCK
