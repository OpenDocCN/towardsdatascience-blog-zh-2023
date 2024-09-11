# 关于在 DAX Measures 中使用中间结果

> 原文：[`towardsdatascience.com/on-using-intermediary-results-in-dax-measures-9971efa72ae?source=collection_archive---------14-----------------------#2023-03-13`](https://towardsdatascience.com/on-using-intermediary-results-in-dax-measures-9971efa72ae?source=collection_archive---------14-----------------------#2023-03-13)

## *我们在 DAX 中经常使用表变量。但当我们需要计算中间结果并在 DAX Measure 中稍后重用时，会怎样呢？这个挑战听起来简单，但其实并不容易。*

[](https://medium.com/@salvatorecagliari?source=post_page-----9971efa72ae--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----9971efa72ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9971efa72ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9971efa72ae--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----9971efa72ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39cccb39e92a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-using-intermediary-results-in-dax-measures-9971efa72ae&user=Salvatore+Cagliari&userId=39cccb39e92a&source=post_page-39cccb39e92a----9971efa72ae---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9971efa72ae--------------------------------) ·8 min read·Mar 13, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9971efa72ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-using-intermediary-results-in-dax-measures-9971efa72ae&user=Salvatore+Cagliari&userId=39cccb39e92a&source=-----9971efa72ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9971efa72ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-using-intermediary-results-in-dax-measures-9971efa72ae&source=-----9971efa72ae---------------------bookmark_footer-----------)![](img/56e23375995cfedbed0a540dd53def90.png)

图片由 [Mika Baumeister](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

我们在 DAX 中一直在使用中间表变量。

例如，看看下面的 Measure：

```py
[SalesYTD] =
  VAR YTDDates = DATESYTD('Date'[Date])

RETURN
  CALCULATE(
      [Sum Online Sales]
      ,YTDDates
      )
```

在这种情况下，我们利用[DATESYTD()](https://dax.guide/datesytd/)函数生成一个基于实际筛选上下文的年初至今表，该表包含从实际筛选上下文中的 1 月 1 日到实际日期的所有日期（当然是基于当前的筛选上下文）。

但有时，我们需要做更多的工作。

例如，我们需要根据当前的筛选上下文查询一个表，并对结果进行进一步计算。

在这种情况下，我们必须生成一个中间表，并将结果赋值给度量值内部的一个变量，以进行所需的计算。
