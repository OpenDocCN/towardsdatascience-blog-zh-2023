# **关于对应分析的简要介绍**

> 原文：[`towardsdatascience.com/brief-introduction-to-correspondence-analysis-a88297ebba2a?source=collection_archive---------9-----------------------#2023-01-16`](https://towardsdatascience.com/brief-introduction-to-correspondence-analysis-a88297ebba2a?source=collection_archive---------9-----------------------#2023-01-16)

## 了解在 R 中进行多重对应分析的基本步骤

[](https://gustavorsantos.medium.com/?source=post_page-----a88297ebba2a--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----a88297ebba2a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a88297ebba2a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a88297ebba2a--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----a88297ebba2a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbrief-introduction-to-correspondence-analysis-a88297ebba2a&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----a88297ebba2a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a88297ebba2a--------------------------------) · 7 min read · 2023 年 1 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa88297ebba2a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbrief-introduction-to-correspondence-analysis-a88297ebba2a&user=Gustavo+Santos&userId=4429d99b1245&source=-----a88297ebba2a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa88297ebba2a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbrief-introduction-to-correspondence-analysis-a88297ebba2a&source=-----a88297ebba2a---------------------bookmark_footer-----------)![](img/59dc59e603fb47dc094c0959e1b008a4.png)

[John Barkiple](https://unsplash.com/@barkiple?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片，来源于 [Unsplash](https://unsplash.com/photos/l090uFWoPaI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

数据集由数字和/或文本组成。因此，我们应该预期并非所有变量都仅仅是数字，数字确实有许多技术可以分析、测试和处理。

当我们处理数值变量时，有相关性、PCA、缩放、标准化等工具。而如果我们处理文本，更具体地说，是分类数据，我们应该寻找其他技术来应用于数据分析。

这些工具之一是对应分析 [CA]。

> 对应分析是一种统计技术，可以显示**基于给定的列联表数据，两个变量之间类别的关系**。

从定义中可以看出，它是一种统计工具。概念上，它类似于主成分分析 [PCA]，但应用于分类数据，因为它使我们能够以二维图形展示数据集，显示哪些类别与哪些类别对应（或相关）。
