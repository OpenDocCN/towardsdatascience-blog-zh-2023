# 揭开 DAX 中 KEEPFILTERS 的秘密

> 原文：[`towardsdatascience.com/uncovering-the-secrets-of-kepfilters-in-dax-6d268e3565d0?source=collection_archive---------13-----------------------#2023-07-13`](https://towardsdatascience.com/uncovering-the-secrets-of-kepfilters-in-dax-6d268e3565d0?source=collection_archive---------13-----------------------#2023-07-13)

## *DAX 中的 KEEPFILTERS() 函数被低估了。因此，我决定深入研究这个函数，并向你们提供一些有趣的细节和一个令人惊讶的效果。*

[](https://medium.com/@salvatorecagliari?source=post_page-----6d268e3565d0--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----6d268e3565d0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6d268e3565d0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d268e3565d0--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----6d268e3565d0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-the-secrets-of-kepfilters-in-dax-6d268e3565d0&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----6d268e3565d0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d268e3565d0--------------------------------) · 8 分钟阅读 · 2023 年 7 月 13 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6d268e3565d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funcovering-the-secrets-of-kepfilters-in-dax-6d268e3565d0&source=-----6d268e3565d0---------------------bookmark_footer-----------)![](img/d3f91fccfe4690bda82bbb16d3e42861.png)

照片由 [Ian Tuck](https://unsplash.com/@iantuck?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

当我们在 DAX 中使用 CALCULATE() 函数时，我们通常会添加一个简单的过滤器，如下所示：

Product[Color] = “Green”

该过滤器用值“Green”替换了列 [Color] 上的任何现有过滤器。

但有时候，我们需要额外的步骤，并保留表或列上的现有过滤器，以进行一些有趣的计算。

有时候，我们会从我们的测量中得到错误的结果，并且无法理解为何会发生这种情况。

这些是[KEEPFILTERS()](https://dax.guide/keepfilters/)函数可以帮助我们的情况。

# 源查询

首先，让我们定义我们想要操作的查询。

我想获取按颜色分类的在线销售列表：

```py
DEFINE
  MEASURE 'All Measures'[Online Sales] = SUMX('Online Sales', [UnitPrice]*[SalesQuantity])

 EVALUATE…
```
