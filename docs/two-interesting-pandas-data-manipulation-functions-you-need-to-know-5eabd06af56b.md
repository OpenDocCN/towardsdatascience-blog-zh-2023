# 你需要了解的两个有趣的 Pandas 数据处理函数

> 原文：[`towardsdatascience.com/two-interesting-pandas-data-manipulation-functions-you-need-to-know-5eabd06af56b?source=collection_archive---------13-----------------------#2023-08-24`](https://towardsdatascience.com/two-interesting-pandas-data-manipulation-functions-you-need-to-know-5eabd06af56b?source=collection_archive---------13-----------------------#2023-08-24)

## 数据科学

## 对于将连续的 pandas 列转换为分类列，非常有用的 pandas 函数。

[](https://medium.com/@17.rsuraj?source=post_page-----5eabd06af56b--------------------------------)![Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----5eabd06af56b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5eabd06af56b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5eabd06af56b--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----5eabd06af56b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-interesting-pandas-data-manipulation-functions-you-need-to-know-5eabd06af56b&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----5eabd06af56b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5eabd06af56b--------------------------------) · 7 分钟阅读 · 2023 年 8 月 24 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5eabd06af56b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-interesting-pandas-data-manipulation-functions-you-need-to-know-5eabd06af56b&user=Suraj+Gurav&userId=1fdda183cca2&source=-----5eabd06af56b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5eabd06af56b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-interesting-pandas-data-manipulation-functions-you-need-to-know-5eabd06af56b&source=-----5eabd06af56b---------------------bookmark_footer-----------)![](img/6bad487920cb6f6872e7975472f27f51.png)

图片由 [Brendan Church](https://unsplash.com/@bdchu614?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Python pandas 是一个强大且广泛使用的数据分析库。

它提供了 200 多个函数和方法，使数据操作和转换变得简单。然而，了解所有这些函数并在实际工作中适时使用并不是一项可行的任务。

数据处理中的一个常见任务是将一个包含连续数值的列转换为一个包含离散或分类值的列。pandas 提供了两个非常棒的内置函数，可以为你节省一些时间。

你可以将这种数据转换方式用于各种应用，比如分组数据、按离散组分析数据，或者使用直方图可视化数据。

例如，

> 最近，我计算了[赫芬达尔-赫希曼指数（HHI）](https://www.investopedia.com/terms/h/hhi.asp)来了解多个品牌的市场集中度。因此，在一个 pandas DataFrame 中，我有一列包含所有品牌的 HHI 连续值。最终，我想将这一列转换为离散列，以便将每个品牌分类为低、中等…
