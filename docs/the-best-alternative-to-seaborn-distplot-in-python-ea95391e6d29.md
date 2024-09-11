# 1 Python 中 Seaborn Distplot 的最佳替代方案

> 原文：[`towardsdatascience.com/the-best-alternative-to-seaborn-distplot-in-python-ea95391e6d29?source=collection_archive---------5-----------------------#2023-06-14`](https://towardsdatascience.com/the-best-alternative-to-seaborn-distplot-in-python-ea95391e6d29?source=collection_archive---------5-----------------------#2023-06-14)

## 数据科学

## Seaborn Distplot 已被弃用——让我们探索它的替代方案

[](https://medium.com/@17.rsuraj?source=post_page-----ea95391e6d29--------------------------------)![Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----ea95391e6d29--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea95391e6d29--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea95391e6d29--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----ea95391e6d29--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-alternative-to-seaborn-distplot-in-python-ea95391e6d29&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----ea95391e6d29---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea95391e6d29--------------------------------) ·8 分钟阅读·2023 年 6 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea95391e6d29&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-alternative-to-seaborn-distplot-in-python-ea95391e6d29&user=Suraj+Gurav&userId=1fdda183cca2&source=-----ea95391e6d29---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea95391e6d29&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-alternative-to-seaborn-distplot-in-python-ea95391e6d29&source=-----ea95391e6d29---------------------bookmark_footer-----------)![](img/350943ef7e0e1078140b5804fe7bb1e6.png)

图片由[Bon Vivant](https://unsplash.com/fr/@bonvivant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/FcS257Cw9es?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Seaborn 是 Python 中一个著名的数据可视化库。

因为它是建立在 matplotlib 之上，并且与 pandas 数据结构完美兼容，所以在处理 Python 中的数据时非常方便，它将数据转化为有洞察力的可视化效果。它有助于关注所需的信息，并更快地把握结果。

然而，每个库都会随着时间的推移而不断演变，Seaborn 也是如此。

当我在项目中使用 Seaborn 创建分布图时，我遇到了如下的函数废弃警告。

所以，我开始寻找替代方案，并今天分享我的发现。

在这篇快速阅读中，你将了解为什么 Seaborn 废弃了令人惊叹的函数`distplot()`，它的当前最佳替代方案，以及如何使用它创建与`distplot()`相同的图形。

这里是内容的预览 —

***·*** ***Seaborn 中的 Distplot*** ***·*** ***为什么 Seaborn 的 Distplot 被废弃？*** ***·*** ***Seaborn Distplot() 的替代方案有哪些？*** ***∘*** ***Seaborn 中的 displot()*** ***·*** ***seaborn 中 displot() 的使用案例*** ***∘*** ***双变量分布*** ***∘*** ***子集的图表***…
