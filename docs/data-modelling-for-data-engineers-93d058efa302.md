# 数据建模与数据工程师

> 原文：[`towardsdatascience.com/data-modelling-for-data-engineers-93d058efa302?source=collection_archive---------0-----------------------#2023-12-03`](https://towardsdatascience.com/data-modelling-for-data-engineers-93d058efa302?source=collection_archive---------0-----------------------#2023-12-03)

## 初学者的终极指南

[](https://mshakhomirov.medium.com/?source=post_page-----93d058efa302--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----93d058efa302--------------------------------)[](https://towardsdatascience.com/?source=post_page-----93d058efa302--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----93d058efa302--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----93d058efa302--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modelling-for-data-engineers-93d058efa302&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----93d058efa302---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----93d058efa302--------------------------------) ·9 分钟阅读·2023 年 12 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F93d058efa302&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modelling-for-data-engineers-93d058efa302&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----93d058efa302---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F93d058efa302&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modelling-for-data-engineers-93d058efa302&source=-----93d058efa302---------------------bookmark_footer-----------)![](img/38909e6454cd7c166ad2897c1828b42d.png)

图片由[Sebastian Svenson](https://unsplash.com/@sebastiansvenson?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据建模是数据工程中的一个重要部分。在这个故事中，我将讨论不同的数据模型、SQL 在数据转换中的作用以及数据增强过程。SQL 是一个强大的工具，可以帮助操作数据。通过数据转换管道，我们可以转换和增强加载到数据平台中的数据。我们将讨论各种数据操作方法、调度和增量表更新。为了使这一过程高效，我们首先需要了解数据建模的一些基本知识。

## 什么是数据建模？

**数据模型**旨在组织数据元素，并标准化数据元素之间的关系。

**数据模型**确保数据的质量、语义配置以及命名规范的一致性。它有助于从***概念上***设计数据库，并在数据元素之间创建逻辑连接，即主键和外键、表等。

良好且全面的**数据模型设计**对于需要最可靠且经济高效的数据转换的数据平台至关重要。它保证数据处理不会出现延迟和不必要的步骤。
