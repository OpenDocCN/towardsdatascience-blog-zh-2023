# 什么是学习排序：学习排序方法的初学者指南

> 原文：[`towardsdatascience.com/what-is-learning-to-rank-a-beginners-guide-to-learning-to-rank-methods-23bbb99ef38c?source=collection_archive---------1-----------------------#2023-01-17`](https://towardsdatascience.com/what-is-learning-to-rank-a-beginners-guide-to-learning-to-rank-methods-23bbb99ef38c?source=collection_archive---------1-----------------------#2023-01-17)

## 机器学习中的 LTR 问题处理指南

[](https://ransakaravihara.medium.com/?source=post_page-----23bbb99ef38c--------------------------------)![Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----23bbb99ef38c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23bbb99ef38c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----23bbb99ef38c--------------------------------) [Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----23bbb99ef38c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F61b4d96de932&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-learning-to-rank-a-beginners-guide-to-learning-to-rank-methods-23bbb99ef38c&user=Ransaka+Ravihara&userId=61b4d96de932&source=post_page-61b4d96de932----23bbb99ef38c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23bbb99ef38c--------------------------------) · 7 分钟阅读 · 2023 年 1 月 17 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F23bbb99ef38c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-learning-to-rank-a-beginners-guide-to-learning-to-rank-methods-23bbb99ef38c&user=Ransaka+Ravihara&userId=61b4d96de932&source=-----23bbb99ef38c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23bbb99ef38c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-learning-to-rank-a-beginners-guide-to-learning-to-rank-methods-23bbb99ef38c&source=-----23bbb99ef38c---------------------bookmark_footer-----------)![](img/456cdb5d1bd66cb4e9b4db863ebc6e2e.png)

由 Possessed Photography 提供的照片，来源于 Unsplash

## 介绍

本文将讨论学习排序究竟是什么。在深入了解其内部工作原理之前，让我们快速回顾一下理解所需的基本概念。

首先，让我们探讨一下学习排序（Learning to Rank, LTR）的核心直觉。在机器学习中，学习排序（LTR）属于监督式机器学习，需要用历史数据集来训练模型。但当我开始学习学习排序概念时，我的第一个困惑是如何区分传统机器学习与 LTR。因为如果我们在构建分类或回归模型，我们的因变量和自变量都很简单，并且更有意义。如果我们需要预测给定客户的贷款违约情况，我们必须将特定的特征向量输入到模型学习的函数 *f(x)* 中。它将返回一个单一的值或客户违约的概率。但是在学习排序模型中，这就很不同而且令人困惑。

让我们举一个简单的例子。

用户 *A* 访问一个网站并输入查询 *q*。在这种情况下，我们的系统返回了一些文档，换句话说…
