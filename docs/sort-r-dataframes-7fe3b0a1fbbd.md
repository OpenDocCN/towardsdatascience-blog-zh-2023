# 如何按多个列对 R 中的数据框进行排序

> 原文：[`towardsdatascience.com/sort-r-dataframes-7fe3b0a1fbbd?source=collection_archive---------9-----------------------#2023-04-27`](https://towardsdatascience.com/sort-r-dataframes-7fe3b0a1fbbd?source=collection_archive---------9-----------------------#2023-04-27)

## 在 R 编程语言中对数据框进行排序

[](https://gmyrianthous.medium.com/?source=post_page-----7fe3b0a1fbbd--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----7fe3b0a1fbbd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7fe3b0a1fbbd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7fe3b0a1fbbd--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----7fe3b0a1fbbd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsort-r-dataframes-7fe3b0a1fbbd&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----7fe3b0a1fbbd---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7fe3b0a1fbbd--------------------------------) ·4 分钟阅读·2023 年 4 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7fe3b0a1fbbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsort-r-dataframes-7fe3b0a1fbbd&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----7fe3b0a1fbbd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7fe3b0a1fbbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsort-r-dataframes-7fe3b0a1fbbd&source=-----7fe3b0a1fbbd---------------------bookmark_footer-----------)![](img/4ee5e1a89e83d4f4e809ddc061df614a.png)

照片由[Andre Taissin](https://unsplash.com/fr/@andretaissin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，刊登于[Unsplash](https://unsplash.com/photos/hOwcob_3dpc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

R 是一种广泛用于统计学家和机器学习科学家的编程语言。数据框是强大的 R 构造，有助于以有效且强大的方式进行数据处理、操作和分析。

在今天的简短教程中，我们将演示如何对 R 数据框进行排序，可以按降序或升序排列。

首先，让我们在 R 中创建一个数据框，以便在整个教程中引用，演示一些关于排序的有用概念。

```py
df <- data.frame(
  id = c(1:8),
  value = c(123, 86, 234, 235, 212, 121, 123, 899),
  colC = c("A", "C", "B", "B", "A", "B", "C", "C"),
  colD = c(TRUE, TRUE, FALSE, FALSE, FALSE, FALSE, TRUE, FALSE)
)

df
  id value colC  colD
1  1   123    A  TRUE
2  2    86    C  TRUE
3  3   234    B FALSE
4  4   235    B FALSE
5  5   212    A FALSE
6  6   121    B FALSE
7  7   123    C  TRUE
8  8   899    C FALSE
```

# 对 R 数据框按单列进行排序（升序或降序）

现在假设我们想按某一列对数据框进行排序。为此，我们可以利用 `[order()](https://stat.ethz.ch/R-manual/R-devel/library/base/html/order.html)` 函数，并对数据进行切片……
