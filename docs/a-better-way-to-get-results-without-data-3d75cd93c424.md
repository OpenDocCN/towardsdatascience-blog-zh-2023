# 更好的方法来获取没有数据的结果

> 原文：[`towardsdatascience.com/a-better-way-to-get-results-without-data-3d75cd93c424?source=collection_archive---------14-----------------------#2023-02-27`](https://towardsdatascience.com/a-better-way-to-get-results-without-data-3d75cd93c424?source=collection_archive---------14-----------------------#2023-02-27)

## *想象一下有一个数据集，并且希望看到零而不是空白单元格。听起来很简单？其实这个情况有一些陷阱。让我们来探讨一下。*

[](https://medium.com/@salvatorecagliari?source=post_page-----3d75cd93c424--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----3d75cd93c424--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d75cd93c424--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d75cd93c424--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----3d75cd93c424--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-better-way-to-get-results-without-data-3d75cd93c424&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----3d75cd93c424---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d75cd93c424--------------------------------) · 5 分钟阅读 · 2023 年 2 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3d75cd93c424&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-better-way-to-get-results-without-data-3d75cd93c424&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----3d75cd93c424---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3d75cd93c424&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-better-way-to-get-results-without-data-3d75cd93c424&source=-----3d75cd93c424---------------------bookmark_footer-----------)![](img/d040e5a4f041e47f5477c2568f858ba9.png)

图片由 [Ramón Salinero](https://unsplash.com/@donramxn?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

几周前，我已经写了一篇关于这个话题的文章：

[](/how-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf?source=post_page-----3d75cd93c424--------------------------------) ## 如何在 Power BI 中显示没有数据时的结果

### 这个标题似乎有些违背直觉。为什么在没有数据的情况下还要寻求结果？让我们看看这个请求并且……

[towardsdatascience.com

在那篇文章中，我使用了 Power Query 来生成额外的数据行，以填充数据表并获得所需的结果。

现在我找到了解决这个挑战的更简单的方法。

首先，我将解释这个挑战。如果你知道我上面链接的文章，接下来的部分将描述我如何找到新的方法。然后我会展示新的解决方案。

# 挑战

想象一个报告，其中有一个表格显示多个任务、相关成本以及每个任务的状态：
