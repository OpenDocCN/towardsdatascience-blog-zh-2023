# 分析 Power BI 和 DAX 查询中汇总数据时的性能

> 原文：[`towardsdatascience.com/analyze-performance-when-aggregating-data-in-power-bi-and-dax-queries-fc00027950a3?source=collection_archive---------9-----------------------#2023-05-01`](https://towardsdatascience.com/analyze-performance-when-aggregating-data-in-power-bi-and-dax-queries-fc00027950a3?source=collection_archive---------9-----------------------#2023-05-01)

## *我们在 Power BI 中经常汇总数据。有时我们想手动查询数据模型或在我们的度量值中需要中间表。让我们来看看如何做到这一点。*

[](https://medium.com/@salvatorecagliari?source=post_page-----fc00027950a3--------------------------------)![萨尔瓦托雷·卡利亚里](https://medium.com/@salvatorecagliari?source=post_page-----fc00027950a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc00027950a3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc00027950a3--------------------------------) [萨尔瓦托雷·卡利亚里](https://medium.com/@salvatorecagliari?source=post_page-----fc00027950a3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-performance-when-aggregating-data-in-power-bi-and-dax-queries-fc00027950a3&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----fc00027950a3---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc00027950a3--------------------------------) ·8 分钟阅读·2023 年 5 月 1 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffc00027950a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-performance-when-aggregating-data-in-power-bi-and-dax-queries-fc00027950a3&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----fc00027950a3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffc00027950a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalyze-performance-when-aggregating-data-in-power-bi-and-dax-queries-fc00027950a3&source=-----fc00027950a3---------------------bookmark_footer-----------)![](img/e798626b974acc167cbccbdc87b18161.png)

照片由[艾萨克·史密斯](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral)拍摄，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

# 介绍

你是否曾经问过自己：

Power BI 可视化背后发生了什么？

或

我该如何编写查询，以在 Power BI 可视化中获得所示结果？

好的，你可以使用性能分析器捕获查询，并将查询复制到文本编辑器中，或者更好的是，使用 DAX Studio。

但你是否理解查询中发生了什么？

当你查看 DAX 的函数文档，无论是在 [Microsoft DAX 函数参考](https://learn.microsoft.com/en-us/dax/dax-function-reference) 还是 [DAX.Guide](https://dax.guide/)，你会发现至少有五个函数可以在查询中生成表格：

+   [SELECTCOLUMNS()](https://dax.guide/selectcolumns/) 和 [ADDCOLUMNS()](https://dax.guide/addcolumns/)

+   [SUMMARIZE()](https://dax.guide/summarize/)

+   [SUMMARIZECOLUMNS()](https://dax.guide/summarizecolumns/)

+   [CALCULATETABLE()](https://dax.guide/calculatetable/)

在这篇文章中，我将以基础查询设定场景。然后我将使用不同的函数从头重建查询……
