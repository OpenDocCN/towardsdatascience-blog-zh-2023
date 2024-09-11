# MLOps 原则介绍

> 原文：[https://towardsdatascience.com/introduction-to-mlops-principles-c5d73a00aa76?source=collection_archive---------2-----------------------#2023-10-30](https://towardsdatascience.com/introduction-to-mlops-principles-c5d73a00aa76?source=collection_archive---------2-----------------------#2023-10-30)

## MLOps 入门指南

[](https://medium.com/@chimso1994?source=post_page-----c5d73a00aa76--------------------------------)[![Chayma Zatout](../Images/341c45f53ddf73dc0851d547cc7cb55a.png)](https://medium.com/@chimso1994?source=post_page-----c5d73a00aa76--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c5d73a00aa76--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c5d73a00aa76--------------------------------) [Chayma Zatout](https://medium.com/@chimso1994?source=post_page-----c5d73a00aa76--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7da1c34b82e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mlops-principles-c5d73a00aa76&user=Chayma+Zatout&userId=f7da1c34b82e&source=post_page-f7da1c34b82e----c5d73a00aa76---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c5d73a00aa76--------------------------------) · 9分钟阅读 · 2023年10月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc5d73a00aa76&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mlops-principles-c5d73a00aa76&user=Chayma+Zatout&userId=f7da1c34b82e&source=-----c5d73a00aa76---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc5d73a00aa76&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-mlops-principles-c5d73a00aa76&source=-----c5d73a00aa76---------------------bookmark_footer-----------)![](../Images/6759305021d22ea569323972e468cf25.png)

图片由 [Silvestri Matteo](https://unsplash.com/@silvestrimatteo?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你想将 MLOps 项目提升到一个新水平，理解其原则是过程中的**关键**部分。本文将对 MLOps 原则进行介绍，并以易于理解的方式阐明关键概念。每个原则都将有一个专门的教程，附有实际例子，未来的文章中将逐一展示。你可以在我的 [Github 个人资料](https://github.com/Chim-SO?tab=repositories)上查看所有示例。如果你是 MLOps 新手，我建议你从我的 [初学者友好教程](https://medium.com/towards-data-science/a-beginner-friendly-introduction-to-mlops-95282f25325c) 开始，快速入门。让我们深入了解吧！

**目录：**

· [1\. 引言](#12e0)

· [2\. MLOps 原则](#a1cf)

· [3\. 版本控制](#7444)

· [4\. 测试](#5ef5)

· [5\. 自动化](#c9ca)

· [6\. 监控与跟踪](#d4ab)

· [7\. 可重复性](#bb89)

· [8\. 结论](#dac8)

**我的 MLOps 教程：**

+   [*教程 1: MLOps 的关键起步: 探索其核心组件*](/a-key-start-to-mlops-exploring-its-essential-components-27646238372d)

+   [*教程 2: MLOps 工作流的初学者友好介绍*](/a-beginner-friendly-introduction-to-mlops-95282f25325c)

+   [*教程 3: MLOps 原则介绍*](https://medium.com/towards-data-science/introduction-to-mlops-principles-c5d73a00aa76)

+   [*教程 4: 以 MLOps 为中心构建你的机器学习项目*](https://medium.com/towards-data-science/structuring-your-machine-learning-project-with-mlops-in-mind-41a8d65987c9)
