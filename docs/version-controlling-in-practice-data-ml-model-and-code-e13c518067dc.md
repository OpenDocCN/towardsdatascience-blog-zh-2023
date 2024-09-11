# 实践中的版本控制：数据、机器学习模型与代码

> 原文：[https://towardsdatascience.com/version-controlling-in-practice-data-ml-model-and-code-e13c518067dc?source=collection_archive---------4-----------------------#2023-12-02](https://towardsdatascience.com/version-controlling-in-practice-data-ml-model-and-code-e13c518067dc?source=collection_archive---------4-----------------------#2023-12-02)

## MLOps版本控制的逐步指南

[](https://medium.com/@chimso1994?source=post_page-----e13c518067dc--------------------------------)[![Chayma Zatout](../Images/341c45f53ddf73dc0851d547cc7cb55a.png)](https://medium.com/@chimso1994?source=post_page-----e13c518067dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e13c518067dc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e13c518067dc--------------------------------) [Chayma Zatout](https://medium.com/@chimso1994?source=post_page-----e13c518067dc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7da1c34b82e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fversion-controlling-in-practice-data-ml-model-and-code-e13c518067dc&user=Chayma+Zatout&userId=f7da1c34b82e&source=post_page-f7da1c34b82e----e13c518067dc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e13c518067dc--------------------------------) ·13分钟阅读·2023年12月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe13c518067dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fversion-controlling-in-practice-data-ml-model-and-code-e13c518067dc&user=Chayma+Zatout&userId=f7da1c34b82e&source=-----e13c518067dc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe13c518067dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fversion-controlling-in-practice-data-ml-model-and-code-e13c518067dc&source=-----e13c518067dc---------------------bookmark_footer-----------)![](../Images/37c9fd4dbfd42fec21a997f19490f28a.png)

照片由 [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

版本控制是一项关键的实践！没有它，你的项目可能会变得混乱，使得回滚到任何所需的点变得困难。你可能会失去重要的模型配置、权重、来自广泛训练时期的实验结果，甚至整个项目本身。代码出现问题时，你可能还会与团队成员产生分歧和冲突，从而阻碍有效的协作。在本文中，我们通过一个实际的例子来探讨版本控制的重要性，该例子使用了一些该领域中最常见的工具。本文的完整代码库可以在[关联的代码库](https://github.com/Chim-SO/hand-written-digits-classification)中获取。

**目录：**

· [1\. 介绍](#7df7)

· [2\. 工具](#51d8)

· [3\. 设置你的项目](#4d35)

∘ [3.1\. 项目文件夹](#65ee)

∘ [3.2\. 项目环境](#fed7)

· [4\. 代码版本控制](#dfb9)

· [5\. 数据版本控制](#c5f4)

· [6\. 模型版本控制](#caf0)

· [结论](#a958)

# 1\. 介绍
