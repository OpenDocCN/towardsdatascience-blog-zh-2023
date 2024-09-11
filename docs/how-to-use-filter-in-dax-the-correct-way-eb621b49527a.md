# 如何正确使用 DAX 中的 FILTER 函数

> 原文：[https://towardsdatascience.com/how-to-use-filter-in-dax-the-correct-way-eb621b49527a?source=collection_archive---------2-----------------------#2023-02-13](https://towardsdatascience.com/how-to-use-filter-in-dax-the-correct-way-eb621b49527a?source=collection_archive---------2-----------------------#2023-02-13)

## *DAX 中的 FILTER() 函数可能很难掌握。你可能会遇到一些陷阱，导致你的 DAX 代码性能不佳。这里有一些使用 FILTER() 的示例，以及一些不该做的事情。*

[](https://medium.com/@salvatorecagliari?source=post_page-----eb621b49527a--------------------------------)[![Salvatore Cagliari](../Images/a24b0cefab6e707cfee06cde9e857559.png)](https://medium.com/@salvatorecagliari?source=post_page-----eb621b49527a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb621b49527a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eb621b49527a--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----eb621b49527a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-filter-in-dax-the-correct-way-eb621b49527a&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----eb621b49527a---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----eb621b49527a--------------------------------) ·11分钟阅读·2023年2月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb621b49527a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-filter-in-dax-the-correct-way-eb621b49527a&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----eb621b49527a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb621b49527a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-filter-in-dax-the-correct-way-eb621b49527a&source=-----eb621b49527a---------------------bookmark_footer-----------)![](../Images/e1e8439b16701f24cc9fe52f0c54ec2a.png)

图片由 [Andrea De Santis](https://unsplash.com/@santesson89?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

一年多以前，我写了一篇关于 [FILTER()](https://dax.guide/filter/) 函数的文章：

[](/discover-the-power-of-filter-in-dax-4bfeac3dd786?source=post_page-----eb621b49527a--------------------------------) [## 发现 FILTER() 在 DAX 中的强大功能

### DAX 中的FILTER()函数功能强大，但也有一些复杂性。让我们深入探讨这些细节，以构建一个良好的...

[前往数据科学网站](https://towardsdatascience.com/discover-the-power-of-filter-in-dax-4bfeac3dd786?source=post_page-----eb621b49527a--------------------------------)

我在那篇文章中解释了一些关于这个强大函数的细节。

我没有做的是解释和展示FILTER()函数的一些与性能相关的细节。

如果你对这个函数不熟悉或不确定如何使用它，可以跳到那篇文章以获得基本理解。

我的旧文章和这篇文章之间有一些重复，但学习基础知识总是没有错的。

此外，为了展示如何使用FILTER()函数，我将展示每种变体对性能和效率的影响。
