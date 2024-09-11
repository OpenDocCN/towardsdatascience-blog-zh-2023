# Power BI 中的字段参数动态筛选

> 原文：[https://towardsdatascience.com/dynamic-filtering-with-field-parameters-in-power-bi-520a472127d5?source=collection_archive---------7-----------------------#2023-04-12](https://towardsdatascience.com/dynamic-filtering-with-field-parameters-in-power-bi-520a472127d5?source=collection_archive---------7-----------------------#2023-04-12)

## 字段参数真是太棒了！了解如何使用这个功能进行数据建模，并为用户提供完全灵活的数据展示方式！

[](https://datamozart.medium.com/?source=post_page-----520a472127d5--------------------------------)[![Nikola Ilic](../Images/9fab894b9696c0dfd80c5173188b720b.png)](https://datamozart.medium.com/?source=post_page-----520a472127d5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----520a472127d5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----520a472127d5--------------------------------) [Nikola Ilic](https://datamozart.medium.com/?source=post_page-----520a472127d5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F64005b7daa38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-filtering-with-field-parameters-in-power-bi-520a472127d5&user=Nikola+Ilic&userId=64005b7daa38&source=post_page-64005b7daa38----520a472127d5---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----520a472127d5--------------------------------) ·5 分钟阅读·2023年4月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F520a472127d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-filtering-with-field-parameters-in-power-bi-520a472127d5&user=Nikola+Ilic&userId=64005b7daa38&source=-----520a472127d5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F520a472127d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-filtering-with-field-parameters-in-power-bi-520a472127d5&source=-----520a472127d5---------------------bookmark_footer-----------)![](../Images/cb7756fcbed7245635b7612d01bfe4ae.png)

作者提供的图片

如果你经常关注我的文章，你可能已经注意到我非常喜欢字段参数。这个功能在2022年5月推出，它显著减少了处理一些非常常见的业务场景时的复杂性和开发工作量。

我已经写过如何利用[字段参数为你的 Power BI 报告增添更多生气](https://data-mozart.com/bring-life-to-field-parameters-in-power-bi/)。然而，这不仅仅是关于数据可视化，因为这一功能也可以以非常优雅的方式解决一些数据建模挑战。

在我展示一个我最近实施的极其实用的字段参数用例之前，让我们首先解释什么是字段参数，以及当你开始使用这一功能时，背后发生了什么。

简而言之，字段参数使你能够执行两个操作：

***1\. 动态更改用于切片和切块数据的属性*** — 意思是，动态切换不同的列
