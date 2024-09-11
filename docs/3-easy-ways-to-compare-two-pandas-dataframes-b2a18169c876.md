# 比较两个 Pandas 数据框的三种简单方法

> 原文：[`towardsdatascience.com/3-easy-ways-to-compare-two-pandas-dataframes-b2a18169c876?source=collection_archive---------2-----------------------#2023-08-09`](https://towardsdatascience.com/3-easy-ways-to-compare-two-pandas-dataframes-b2a18169c876?source=collection_archive---------2-----------------------#2023-08-09)

## 数据科学

## 快速了解如何找出两个 Pandas 数据框之间的共同和不共同的行。

[](https://medium.com/@17.rsuraj?source=post_page-----b2a18169c876--------------------------------)![Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----b2a18169c876--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b2a18169c876--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2a18169c876--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----b2a18169c876--------------------------------)

·

[点击查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-ways-to-compare-two-pandas-dataframes-b2a18169c876&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----b2a18169c876---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----b2a18169c876--------------------------------) ·6 分钟阅读·2023 年 8 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb2a18169c876&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-ways-to-compare-two-pandas-dataframes-b2a18169c876&user=Suraj+Gurav&userId=1fdda183cca2&source=-----b2a18169c876---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb2a18169c876&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-easy-ways-to-compare-two-pandas-dataframes-b2a18169c876&source=-----b2a18169c876---------------------bookmark_footer-----------)![](img/47b45ee1ffb03229a3cf990c1b2178f3.png)

图片由 [梅根·赫斯勒](https://unsplash.com/@meghankix?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**这是一项简单的任务——当你使用 Pandas 内置方法时。**

在 Python Pandas 中，DataFrame 是最简单的数据结构，你可以将数据以表格形式（即行 — 列）存储，并对其进行处理以获取有用的见解。

在处理实际场景时，数据分析师的一个常见任务是查看数据中发生了什么变化。你可以通过比较两组数据来实现这一点。

> 最近，我开发了一个自动化计算机视觉系统，该系统在两个不同的时间从 10 个设备收集数据，并将其存储在 2 个 pandas DataFrames 中。为了了解系统中发生了什么变化，我比较了这两个 DataFrames，这就是这个故事灵感的来源。

你可以在数据验证、数据变化检测、测试和调试中最常见地找到这种 DataFrame 比较应用。因此，了解如何快速且轻松地比较两个数据集是很重要的。

因此，在这篇文章中，我将解释比较两个 DataFrames 的三种最佳、最简单、最可靠和最快的方法。你可以快速了解…
