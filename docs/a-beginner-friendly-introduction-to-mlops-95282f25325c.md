# 初学者友好的 MLOps 介绍

> 原文：[https://towardsdatascience.com/a-beginner-friendly-introduction-to-mlops-95282f25325c?source=collection_archive---------2-----------------------#2023-01-04](https://towardsdatascience.com/a-beginner-friendly-introduction-to-mlops-95282f25325c?source=collection_archive---------2-----------------------#2023-01-04)

## MLOps 的第一步

[](https://medium.com/@chimso1994?source=post_page-----95282f25325c--------------------------------)[![Chayma Zatout](../Images/341c45f53ddf73dc0851d547cc7cb55a.png)](https://medium.com/@chimso1994?source=post_page-----95282f25325c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----95282f25325c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----95282f25325c--------------------------------) [Chayma Zatout](https://medium.com/@chimso1994?source=post_page-----95282f25325c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7da1c34b82e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginner-friendly-introduction-to-mlops-95282f25325c&user=Chayma+Zatout&userId=f7da1c34b82e&source=post_page-f7da1c34b82e----95282f25325c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----95282f25325c--------------------------------) ·9分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F95282f25325c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginner-friendly-introduction-to-mlops-95282f25325c&user=Chayma+Zatout&userId=f7da1c34b82e&source=-----95282f25325c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F95282f25325c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginner-friendly-introduction-to-mlops-95282f25325c&source=-----95282f25325c---------------------bookmark_footer-----------)![](../Images/d1a25fc18fe181535406d3367f668517.png)

图片由 [Sarah Dorweiler](https://unsplash.com/@sarahdorweiler?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我对 MLOps 一直很感兴趣。我最初是从机器学习工程师那里了解到它的，当时我还是一名博士生，因此并不知道它的存在。然而，我的好奇心被激发了，我开始学习它。回想起来，我很后悔没有早点了解它，以优化我的机器学习工作流程。

在这篇文章中，我将尝试提供一个适合初学者的 MLOps 介绍，并以简单的方式解释关键概念。作为一个起初也觉得理解困难的人，我理解对这个主题的简单介绍的需求。我的希望是，在阅读完这篇文章后，初学者能更容易阅读更高级的 MLOps 文档。

**目录：**

· [1. MLOps 动机](#b624)

· [2. 定义](#1c5a)

· [3. MLOps 生命周期](#4504)

· [4. MLOps 工作流程](#fc3c)

∘ [4.1. 商业问题](#9dcc)

∘ [4.2. 数据工程](#73c3)

∘ [4.3. ML 模型工程](#aabc)

∘ [4.4. 代码工程](#b08f)

· [5. 结论](#e4b0)

**我的 MLOps 教程：**

+   [*教程 1：MLOps 的关键起点：探索*](/a-key-start-to-mlops-exploring-its-essential-components-27646238372d)…
