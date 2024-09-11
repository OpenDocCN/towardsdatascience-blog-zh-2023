# 使用 Pinecone 进行 CRUD

> 原文：[https://towardsdatascience.com/crud-with-pinecone-ee6b6f8b54e8?source=collection_archive---------5-----------------------#2023-05-08](https://towardsdatascience.com/crud-with-pinecone-ee6b6f8b54e8?source=collection_archive---------5-----------------------#2023-05-08)

## 向量数据库

## 从传统数据库的角度入门向量数据库的简单指南

[](https://manfyegoh.medium.com/?source=post_page-----ee6b6f8b54e8--------------------------------)[![Manfye Goh](../Images/9ce9f554c8b025b26b204c29b59c80b7.png)](https://manfyegoh.medium.com/?source=post_page-----ee6b6f8b54e8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee6b6f8b54e8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ee6b6f8b54e8--------------------------------) [Manfye Goh](https://manfyegoh.medium.com/?source=post_page-----ee6b6f8b54e8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F79c0fed0a4d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcrud-with-pinecone-ee6b6f8b54e8&user=Manfye+Goh&userId=79c0fed0a4d2&source=post_page-79c0fed0a4d2----ee6b6f8b54e8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee6b6f8b54e8--------------------------------) ·7分钟阅读·2023年5月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee6b6f8b54e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcrud-with-pinecone-ee6b6f8b54e8&user=Manfye+Goh&userId=79c0fed0a4d2&source=-----ee6b6f8b54e8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee6b6f8b54e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcrud-with-pinecone-ee6b6f8b54e8&source=-----ee6b6f8b54e8---------------------bookmark_footer-----------)![](../Images/d72e74c9bf31f2efc4bedf9eb6ee2b42.png)

图片由 Brett Sayles 提供，来自 Pexels

机器学习应用和人工智能的迅速发展推动了对专业数据存储解决方案的需求。

向量数据库由于能够高效地执行相似性搜索并支持复杂的数据结构，已经成为处理大规模、高维数据的热门选择。最近，[Pinecone](https://www.pinecone.io/) 作为一个可扩展且高效的向量数据库解决方案，在开发者和数据科学家中越来越受欢迎。

我发现很多人教授如何使用 Pinecone，但没有人从传统数据库的角度描述它，比如如何与传统 SQL 数据库进行比较。

在本文中，我们将从传统数据库的角度提供对CRUD（创建、读取、更新和删除）操作的清晰理解。我们将**深入探讨**向量数据库与传统数据库之间的差异，探索如何利用向量数据库来优化现代应用中的数据管理。

> 本文的代码[可在此处找到](https://github.com/manfye/medium-publication/blob/main/Pinecone%20Medium%20Article.ipynb)
