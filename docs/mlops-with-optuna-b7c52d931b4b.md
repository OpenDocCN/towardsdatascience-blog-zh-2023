# Optuna 的 MLOps

> 原文：[`towardsdatascience.com/mlops-with-optuna-b7c52d931b4b?source=collection_archive---------10-----------------------#2023-03-01`](https://towardsdatascience.com/mlops-with-optuna-b7c52d931b4b?source=collection_archive---------10-----------------------#2023-03-01)

## 不要浪费时间，使用 Optuna

[](https://zjwarnes.medium.com/?source=post_page-----b7c52d931b4b--------------------------------)![Zachary Warnes](https://zjwarnes.medium.com/?source=post_page-----b7c52d931b4b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b7c52d931b4b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7c52d931b4b--------------------------------) [Zachary Warnes](https://zjwarnes.medium.com/?source=post_page-----b7c52d931b4b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa8a13d1fdc28&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmlops-with-optuna-b7c52d931b4b&user=Zachary+Warnes&userId=a8a13d1fdc28&source=post_page-a8a13d1fdc28----b7c52d931b4b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7c52d931b4b--------------------------------) ·7 min 阅读·2023 年 3 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb7c52d931b4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmlops-with-optuna-b7c52d931b4b&user=Zachary+Warnes&userId=a8a13d1fdc28&source=-----b7c52d931b4b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb7c52d931b4b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmlops-with-optuna-b7c52d931b4b&source=-----b7c52d931b4b---------------------bookmark_footer-----------)![](img/52d8b8b6c3b6e3792658e18a40051b48.png)

由 DALLE-2 生成，超参数优化（作者照片）

对于任何熟悉超参数调优艰巨过程的人来说，Optuna 是一个救星。

使用不同超参数优化技术调优各种模型的能力令人惊叹。

> 如果你还在通过网格搜索调优模型，你需要改变方法——你正在丧失性能

本文包含可立即使用的代码，你可以立刻实现。如果你想亲自实验，可以快进到文章末尾。不要忘记在整篇文章中加载这些函数。本文将讨论超参数调优的高级内容，然后深入探讨 Optuna。

+   它将概述如何使用自定义参数网格和度量创建新的研究。

+   它将展示如何保存和加载研究，并收集最佳试验和模型。

+   最终，它将展示如何分叉研究以在更新的搜索空间中继续搜索。

# 超参数调优

在学习机器学习时，你将面临超参数调优，它决定了你优化模型的结构。最常见的方法是网格搜索…
