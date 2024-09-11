# 更有效地使用 Pandas 数据框的 7 大列操作

> 原文：[`towardsdatascience.com/dominate-pandas-data-frames-with-the-top-7-column-operations-2a11521e9e2d?source=collection_archive---------8-----------------------#2023-07-06`](https://towardsdatascience.com/dominate-pandas-data-frames-with-the-top-7-column-operations-2a11521e9e2d?source=collection_archive---------8-----------------------#2023-07-06)

## 完整指南：以不同方式执行 7 大 Pandas 列操作

[](https://federicotrotta.medium.com/?source=post_page-----2a11521e9e2d--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----2a11521e9e2d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a11521e9e2d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a11521e9e2d--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----2a11521e9e2d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdominate-pandas-data-frames-with-the-top-7-column-operations-2a11521e9e2d&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----2a11521e9e2d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a11521e9e2d--------------------------------) ·15 分钟阅读·2023 年 7 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2a11521e9e2d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdominate-pandas-data-frames-with-the-top-7-column-operations-2a11521e9e2d&user=Federico+Trotta&userId=654cd4bbe899&source=-----2a11521e9e2d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2a11521e9e2d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdominate-pandas-data-frames-with-the-top-7-column-operations-2a11521e9e2d&source=-----2a11521e9e2d---------------------bookmark_footer-----------)![](img/4a4f32645b84448222d2511ae309f680.png)

图片来源：[Alan](https://pixabay.com/it/users/ad_images-6663717/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2987596) 在 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2987596)

在数据分析方面，[Pandas](https://pandas.pydata.org/) 是最常用的 Python 库，用于操作和准备数据以进行进一步分析和机器学习。

实际上，Pandas 是一个非常灵活的库，甚至可以用于转换文件格式。

然而，即使我们每天几乎不使用某些 Pandas 功能，我们仍然花费大量时间在 Google 上搜索如何在 Pandas 中做某事。

我知道，我抓到你了！

但让我们诚实一点：有些功能很难记住，可能是因为我们可以用不同的方法达到相同的目标。因此，每天在 Google 上搜索相同的东西也不必感到羞愧。

然而，节省时间总是个好主意。因此，在本文中，我们将探讨操作 Pandas 列的 7 个顶级功能。这样你就不再需要去 Google 搜索它们了：只需保存本文（也许，通过书签）并在需要时返回查看即可。

这就是你在这里会找到的内容：

```py
**Table of contents:** 
How to create a new Pandas column
How to add a new column to a Pandas…
```
