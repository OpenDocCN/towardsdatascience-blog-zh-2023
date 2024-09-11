# 如何使用 DAX Studio 从 Power BI 获取性能数据

> 原文：[https://towardsdatascience.com/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9?source=collection_archive---------6-----------------------#2023-01-25](https://towardsdatascience.com/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9?source=collection_archive---------6-----------------------#2023-01-25)

## *有时候我们会遇到报告运行缓慢的情况，我们需要弄清楚原因。我将展示如何收集性能数据以及这些指标的含义。*

[](https://medium.com/@salvatorecagliari?source=post_page-----b7f11b9dd9f9--------------------------------)[![Salvatore Cagliari](../Images/a24b0cefab6e707cfee06cde9e857559.png)](https://medium.com/@salvatorecagliari?source=post_page-----b7f11b9dd9f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b7f11b9dd9f9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b7f11b9dd9f9--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----b7f11b9dd9f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----b7f11b9dd9f9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7f11b9dd9f9--------------------------------) · 9分钟阅读 · 2023年1月25日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb7f11b9dd9f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----b7f11b9dd9f9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb7f11b9dd9f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9&source=-----b7f11b9dd9f9---------------------bookmark_footer-----------)![](../Images/be3df699528c306993e55c7a3e5b64cb.png)

图片由 [Aleks Dorohovich](https://unsplash.com/@doctype?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

说清楚一点：今天我不会讨论如何优化 DAX 代码。

之后会有更多的文章，集中于常见错误及其避免方法。

但是，在我们能够理解性能指标之前，我们需要理解 Power BI 中表格模型的架构。

相同的架构适用于SQL Server分析服务中的表格模型。

任何表格模型都有两个引擎：

+   存储引擎

+   公式引擎

这两者在表格模型中具有不同的属性，并完成不同的任务。

让我们深入探讨一下。

# 存储引擎

存储引擎是DAX查询与表格模型中存储的数据之间的接口。

该引擎接收任何给定的DAX查询，并将查询发送到Vertipaq存储引擎，该引擎在数据模型中存储数据。
