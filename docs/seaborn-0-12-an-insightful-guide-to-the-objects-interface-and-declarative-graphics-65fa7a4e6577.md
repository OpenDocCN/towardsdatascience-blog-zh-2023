# Seaborn 0.12: 一份关于对象接口和声明性图形的深刻指南

> 原文：[https://towardsdatascience.com/seaborn-0-12-an-insightful-guide-to-the-objects-interface-and-declarative-graphics-65fa7a4e6577?source=collection_archive---------14-----------------------#2023-08-24](https://towardsdatascience.com/seaborn-0-12-an-insightful-guide-to-the-objects-interface-and-declarative-graphics-65fa7a4e6577?source=collection_archive---------14-----------------------#2023-08-24)

## [PYTHON TOOLBOX](https://medium.com/@qtalen/list/python-toolbox-4289824c6407)

## 简化你使用 Python 流行库进行数据可视化的旅程

[](https://qtalen.medium.com/?source=post_page-----65fa7a4e6577--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----65fa7a4e6577--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65fa7a4e6577--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----65fa7a4e6577--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----65fa7a4e6577--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseaborn-0-12-an-insightful-guide-to-the-objects-interface-and-declarative-graphics-65fa7a4e6577&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----65fa7a4e6577---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----65fa7a4e6577--------------------------------) ·14 分钟阅读·2023年8月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F65fa7a4e6577&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseaborn-0-12-an-insightful-guide-to-the-objects-interface-and-declarative-graphics-65fa7a4e6577&user=Peng+Qian&userId=8e2fe735546d&source=-----65fa7a4e6577---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F65fa7a4e6577&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fseaborn-0-12-an-insightful-guide-to-the-objects-interface-and-declarative-graphics-65fa7a4e6577&source=-----65fa7a4e6577---------------------bookmark_footer-----------)![](../Images/d930c9fdcb0a9730a8836e89e069c8c6.png)

图片来源：作者制作，[Canva](https://www.canva.com/)

本文旨在介绍 Seaborn 0.12 中的对象接口特性，包括声明性图形语法的概念，以及一个实际的可视化项目来展示对象接口的使用。

阅读完这篇文章后，你将清楚了解Seaborn的对象接口API的优缺点。你也将能够更轻松地使用Seaborn进行数据分析项目。

# 介绍

想象一下你正在使用Python创建数据可视化图表。

你必须指导计算机每一步：选择数据集、创建图形、设置颜色、添加标签、调整大小等……

然后你发现你的代码变得越来越长和复杂，而你只是想快速可视化你的数据。

这就像是去杂货店，你需要指定每个物品的位置、颜色、大小和形状，而不是只告诉店员你需要什么。
