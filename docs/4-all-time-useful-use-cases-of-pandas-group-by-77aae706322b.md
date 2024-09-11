# Pandas 分组操作的四个终极实用技巧

> 原文：[`towardsdatascience.com/4-all-time-useful-use-cases-of-pandas-group-by-77aae706322b?source=collection_archive---------11-----------------------#2023-03-24`](https://towardsdatascience.com/4-all-time-useful-use-cases-of-pandas-group-by-77aae706322b?source=collection_archive---------11-----------------------#2023-03-24)

## 数据科学

## 一个解决所有 Pandas 分组操作数据聚合问题的一站式方案

[](https://medium.com/@17.rsuraj?source=post_page-----77aae706322b--------------------------------)![Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----77aae706322b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----77aae706322b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----77aae706322b--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----77aae706322b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-all-time-useful-use-cases-of-pandas-group-by-77aae706322b&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----77aae706322b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----77aae706322b--------------------------------) ·8 min read·Mar 24, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F77aae706322b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-all-time-useful-use-cases-of-pandas-group-by-77aae706322b&user=Suraj+Gurav&userId=1fdda183cca2&source=-----77aae706322b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F77aae706322b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-all-time-useful-use-cases-of-pandas-group-by-77aae706322b&source=-----77aae706322b---------------------bookmark_footer-----------)![](img/6cb476af3f438709882dc9b91ed40ea2.png)

图片由 [Hands off my tags! Michael Gaida](https://pixabay.com/users/652234-652234/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=1422182) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=1422182)

在 Python 中，pandas 是一个常用的数据分析库。凭借其众多内置函数和方法，它使数据分析变得更快更简便。

数据分析的一个重要方面是数据聚合，它帮助你根据一个变量对数据进行分组，并对其余的数值数据进行聚合，以获得汇总统计数据。最终，你可以使用这些汇总统计数据来回答业务问题。

这就是 pandas 函数 — `**groupby()**` — 的作用，它根据分类或非数值列中的值对数据进行分组，帮助你通过这些新形成的组来分析数据。

在我以前的一篇文章 — **Python 中你应该知道的 5 个 Pandas 分组技巧** — 中，你可以学习什么是 `groupby` 以及如何使用它。

在这篇文章中，我将解释 4 个非常有用且常被搜索的 pandas `groupby` 技巧，并提供示例，这些技巧是你有效进行数据分析必知的。

我还附上了这篇文章末尾的完整 Jupyter-Notebook，其中包含所有示例。
