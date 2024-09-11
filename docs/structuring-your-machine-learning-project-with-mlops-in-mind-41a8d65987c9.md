# 以 MLOps 为导向的机器学习项目结构化

> 原文：[https://towardsdatascience.com/structuring-your-machine-learning-project-with-mlops-in-mind-41a8d65987c9?source=collection_archive---------2-----------------------#2023-03-16](https://towardsdatascience.com/structuring-your-machine-learning-project-with-mlops-in-mind-41a8d65987c9?source=collection_archive---------2-----------------------#2023-03-16)

## MLOps 实践：项目结构化

[](https://medium.com/@chimso1994?source=post_page-----41a8d65987c9--------------------------------)[![Chayma Zatout](../Images/341c45f53ddf73dc0851d547cc7cb55a.png)](https://medium.com/@chimso1994?source=post_page-----41a8d65987c9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----41a8d65987c9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----41a8d65987c9--------------------------------) [Chayma Zatout](https://medium.com/@chimso1994?source=post_page-----41a8d65987c9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7da1c34b82e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstructuring-your-machine-learning-project-with-mlops-in-mind-41a8d65987c9&user=Chayma+Zatout&userId=f7da1c34b82e&source=post_page-f7da1c34b82e----41a8d65987c9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----41a8d65987c9--------------------------------) ·14 min 阅读·2023年3月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F41a8d65987c9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstructuring-your-machine-learning-project-with-mlops-in-mind-41a8d65987c9&user=Chayma+Zatout&userId=f7da1c34b82e&source=-----41a8d65987c9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F41a8d65987c9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstructuring-your-machine-learning-project-with-mlops-in-mind-41a8d65987c9&source=-----41a8d65987c9---------------------bookmark_footer-----------)![](../Images/ff4a6fc8ea3f407bf08adfa0cf7d1d88.png)

图片来源 [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) 置于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你希望将你的机器学习项目提升到一个新层次，MLOps 是过程中的关键部分。本文将提供一个实用教程，讲解如何为 MLOps 结构化你的项目，以经典的手写数字分类问题为例。我们将一步步引导你创建一个基本的项目模板，你可以用它来组织自己的项目。完成这个教程后，你将对 MLOps 原则有一个扎实的理解，并能够将其应用到自己的项目中。然而，如果你是 MLOps 新手，我们建议你先从我的 [适合初学者的教程](https://medium.com/towards-data-science/a-beginner-friendly-introduction-to-mlops-95282f25325c) 开始，以便快速入门。那么让我们深入探讨，将你的 ML 项目提升到新高度吧！

**目录：**

· [1\. 介绍](#c6d8)

· [2\. MLOps](#6150)

∘ [2.1\. 业务问题](#2b4f)

∘ [2.2\. 数据工程](#aadf)

∘ [2.3\. 机器学习模型工程](#8845)

∘ [2.4\. 代码工程](#8a22)

· [3\. 项目结构](#5c87)

∘ [3.1\. Cookiecutter 数据科学](#477b)

· [4\. MLOps 项目结构](#ca36)

∘ [4.1\. 启动一个新的 MLOps 项目](#db66)

∘ [4.2\. 使用 MLOps 项目模板进行手写数字分类](#4e0e)

∘ [4.3\. 如何运行你的项目？](#85d1)

· [5\. 结论](#822e)
