# 将平面表格转换为 Power Query 中的良好数据模型

> 原文：[https://towardsdatascience.com/converting-a-flat-table-to-a-good-data-model-in-power-query-46208215f17a?source=collection_archive---------6-----------------------#2023-12-22](https://towardsdatascience.com/converting-a-flat-table-to-a-good-data-model-in-power-query-46208215f17a?source=collection_archive---------6-----------------------#2023-12-22)

## *当将一个宽的 Excel 表格加载到 Power BI 中时，我们会得到一个次优的数据模型。我们该如何创建一个良好的数据模型？什么才算是“良好的”数据模型？让我们深入探讨一下。*

[](https://medium.com/@salvatorecagliari?source=post_page-----46208215f17a--------------------------------)[![Salvatore Cagliari](../Images/a24b0cefab6e707cfee06cde9e857559.png)](https://medium.com/@salvatorecagliari?source=post_page-----46208215f17a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----46208215f17a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----46208215f17a--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----46208215f17a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-a-flat-table-to-a-good-data-model-in-power-query-46208215f17a&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----46208215f17a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----46208215f17a--------------------------------) ·12 min read·2023年12月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F46208215f17a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-a-flat-table-to-a-good-data-model-in-power-query-46208215f17a&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----46208215f17a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F46208215f17a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconverting-a-flat-table-to-a-good-data-model-in-power-query-46208215f17a&source=-----46208215f17a---------------------bookmark_footer-----------)![](../Images/d7991f273e6117d3d4ccf54cabef8548.png)

[Kaleidico](https://unsplash.com/@kaleidico?utm_source=medium&utm_medium=referral) 的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

在办公室的另一普通一天：一位客户打电话给我，请我修复他 Power BI 报告中的一个问题。我查看了数据，发现有一张宽表，包含 30、40 或更多列。

我问了一个自然的问题：“这个表格的来源是什么？”

答案是：“Excel，其他还有什么？”

“当然，”我想。

我接下来的问题是：“我们能从中构建一个好的数据模型吗？”

我的客户问：“为什么？”

于是我们到了这里。

理想情况下，我会将源文件导入关系型数据库，并将数据转换为一个漂亮的数据模型。

不幸的是，我的客户愿意为一些乍一看对他们没有益处的东西付费，这种情况很少见。

但我为什么要拥有一个好的数据模型？一个平坦的表格不是也很好吗？
