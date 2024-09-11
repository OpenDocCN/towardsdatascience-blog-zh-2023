# 你应该为你的大数据项目选择哪种数据格式？

> 原文：[`towardsdatascience.com/which-data-format-to-use-for-your-big-data-project-837a48d3661d?source=collection_archive---------8-----------------------#2023-10-26`](https://towardsdatascience.com/which-data-format-to-use-for-your-big-data-project-837a48d3661d?source=collection_archive---------8-----------------------#2023-10-26)

## Pickle、Parquet、CSV、Feather、HDF5、ORC、JSON：你应该使用哪一种，为什么？

[](https://armandsauzay.medium.com/?source=post_page-----837a48d3661d--------------------------------)![Armand Sauzay](https://armandsauzay.medium.com/?source=post_page-----837a48d3661d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----837a48d3661d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----837a48d3661d--------------------------------) [Armand Sauzay](https://armandsauzay.medium.com/?source=post_page-----837a48d3661d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6ce70a13b962&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhich-data-format-to-use-for-your-big-data-project-837a48d3661d&user=Armand+Sauzay&userId=6ce70a13b962&source=post_page-6ce70a13b962----837a48d3661d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----837a48d3661d--------------------------------) · 6 分钟阅读 · 2023 年 10 月 26 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F837a48d3661d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhich-data-format-to-use-for-your-big-data-project-837a48d3661d&user=Armand+Sauzay&userId=6ce70a13b962&source=-----837a48d3661d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F837a48d3661d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhich-data-format-to-use-for-your-big-data-project-837a48d3661d&source=-----837a48d3661d---------------------bookmark_footer-----------)![](img/f6052f807142ff0736003534edc8f2dc.png)

图片由 Maarten van den Heuvel 提供 — [Unsplash](https://unsplash.com/photos/assorted-color-book-lot-8EzNkvLQosk)

在数据科学项目中选择合适的数据格式至关重要，它影响从数据读写速度到内存消耗和互操作性等各个方面。本文探讨了 Python 中七种流行的序列化/反序列化格式，重点关注它们的速度和内存使用情况。

通过分析，我们还将探讨如何在 Python 中使用分析工具（使用内置的 `cProfile` 模块），以及如何获取文件系统中特定文件的内存使用统计信息（使用 `os` Python 模块）。

当然，每个项目都有其特定性，不仅仅是速度和内存使用。但我们将总结一些趋势，希望能够帮助你明晰在特定项目中可以选择哪种格式。

## 理解序列化与反序列化

> 序列化是将对象（例如 Python 中的 pandas DataFrame）保存到一种可以存储到文件中的格式的过程，以便后续检索。反序列化是相反的过程。

数据框是一个 Python 对象，不能直接持久化。它需要被转换为文件...
