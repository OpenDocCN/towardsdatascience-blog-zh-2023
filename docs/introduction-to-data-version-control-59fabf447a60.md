# 数据版本控制简介

> 原文：[https://towardsdatascience.com/introduction-to-data-version-control-59fabf447a60?source=collection_archive---------5-----------------------#2023-08-18](https://towardsdatascience.com/introduction-to-data-version-control-59fabf447a60?source=collection_archive---------5-----------------------#2023-08-18)

## PYTHON | DATA | PROGRAMMING

## 使用 Hangar 在 Python 中实现自定义 DVC 的逐步指南

[](https://david-farrugia.medium.com/?source=post_page-----59fabf447a60--------------------------------)[![David Farrugia](../Images/082ed61e24c7c26a4ae1c77343a87824.png)](https://david-farrugia.medium.com/?source=post_page-----59fabf447a60--------------------------------)[](https://towardsdatascience.com/?source=post_page-----59fabf447a60--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----59fabf447a60--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----59fabf447a60--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916826092a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-data-version-control-59fabf447a60&user=David+Farrugia&userId=3916826092a6&source=post_page-3916826092a6----59fabf447a60---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----59fabf447a60--------------------------------) ·7 min read·2023年8月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F59fabf447a60&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-data-version-control-59fabf447a60&user=David+Farrugia&userId=3916826092a6&source=-----59fabf447a60---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F59fabf447a60&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-data-version-control-59fabf447a60&source=-----59fabf447a60---------------------bookmark_footer-----------)![](../Images/a8220431c99c3ef95c58fd1005524447.png)

图片由 [Florian Olivo](https://unsplash.com/@florianolv?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 什么是数据版本控制（DVC）？

任何生产级系统都需要某种版本控制。

> **一个当前真相的单一来源。**

任何持续更新的资源，尤其是多个用户同时更新的资源，都需要某种审计跟踪以记录所有变更。

在软件工程中，解决这个问题的方案是 [Git](https://git-scm.com/)。

如果你曾编写过代码，那么你可能对 Git 的美妙感到熟悉。

Git 允许我们提交更改，从一个源创建不同的分支，并将我们的分支合并回原始分支，仅举几例。

DVC 完全是相同的范式，但针对数据集。看，实时数据系统不断吸收更新的数据点，同时不同用户对相同的数据集进行不同的实验。

这导致了相同数据集的多个版本，这绝对不是唯一的真实来源。

此外，在机器学习环境中，我们还会有多个版本……
