# 数据建模入门——第2部分：维度建模基础

> 原文：[https://towardsdatascience.com/data-modeling-for-mere-mortals-part-2-dimensional-modeling-fundamentals-ae2f53622321?source=collection_archive---------2-----------------------#2023-09-01](https://towardsdatascience.com/data-modeling-for-mere-mortals-part-2-dimensional-modeling-fundamentals-ae2f53622321?source=collection_archive---------2-----------------------#2023-09-01)

## 介绍一种四步维度建模设计方法

[](https://datamozart.medium.com/?source=post_page-----ae2f53622321--------------------------------)[![Nikola Ilic](../Images/9fab894b9696c0dfd80c5173188b720b.png)](https://datamozart.medium.com/?source=post_page-----ae2f53622321--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ae2f53622321--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ae2f53622321--------------------------------) [Nikola Ilic](https://datamozart.medium.com/?source=post_page-----ae2f53622321--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F64005b7daa38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modeling-for-mere-mortals-part-2-dimensional-modeling-fundamentals-ae2f53622321&user=Nikola+Ilic&userId=64005b7daa38&source=post_page-64005b7daa38----ae2f53622321---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae2f53622321--------------------------------) ·9 min read·2023年9月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fae2f53622321&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modeling-for-mere-mortals-part-2-dimensional-modeling-fundamentals-ae2f53622321&user=Nikola+Ilic&userId=64005b7daa38&source=-----ae2f53622321---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fae2f53622321&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-modeling-for-mere-mortals-part-2-dimensional-modeling-fundamentals-ae2f53622321&source=-----ae2f53622321---------------------bookmark_footer-----------)

你知道在商业智能领域中*可能*是最好的数据模型是什么吗？想了解4步维度设计过程吗？本文将为你提供所有维度建模的基础知识。

![](../Images/bdfd954c7e43aaf7a89e8a58a5890b31.png)

[照片由**徐在Pexels**提供](https://www.pexels.com/de-de/foto/flachfokus-fotografie-von-gelben-sternlaternen-980859/)

在[上一篇文章](https://data-mozart.com/data-modeling-for-mere-mortals-part-1-what-is-data-modeling/)中，你学习了数据建模的一般原则。现在，是时候缩小我们的学习范围，掌握与商业智能场景特别相关的数据建模概念和技术了。每当我们谈论商业智能的数据建模时，维度建模绝对是首要概念。

# 历史课程 — **拉尔夫·金博尔**

在我们解释为什么维度建模被称作维度（dimensional）之前，先让我们简要回顾一下历史课程。1996年，一位名叫[Ralph Kimball](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/kimball-reader/)的男子出版了一本书《数据仓库工具包》，这本书仍然被认为是维度建模的“圣经”。在他的书中，金博尔介绍了一种全新的数据建模方法，即所谓的“自下而上”方法。其重点是识别组织内的关键业务流程，并…
