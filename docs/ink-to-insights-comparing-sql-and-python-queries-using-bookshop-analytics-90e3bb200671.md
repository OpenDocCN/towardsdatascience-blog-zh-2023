# 《从墨水到洞察：使用书店分析比较 SQL 和 Python 查询》

> 原文：[`towardsdatascience.com/ink-to-insights-comparing-sql-and-python-queries-using-bookshop-analytics-90e3bb200671?source=collection_archive---------3-----------------------#2023-09-01`](https://towardsdatascience.com/ink-to-insights-comparing-sql-and-python-queries-using-bookshop-analytics-90e3bb200671?source=collection_archive---------3-----------------------#2023-09-01)

## 哪种方法更适合你的探索性数据分析？

[](https://medium.com/@john_lenehan?source=post_page-----90e3bb200671--------------------------------)![John Lenehan](https://medium.com/@john_lenehan?source=post_page-----90e3bb200671--------------------------------)[](https://towardsdatascience.com/?source=post_page-----90e3bb200671--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----90e3bb200671--------------------------------) [John Lenehan](https://medium.com/@john_lenehan?source=post_page-----90e3bb200671--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2eb00da71bb6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fink-to-insights-comparing-sql-and-python-queries-using-bookshop-analytics-90e3bb200671&user=John+Lenehan&userId=2eb00da71bb6&source=post_page-2eb00da71bb6----90e3bb200671---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----90e3bb200671--------------------------------) ·9 分钟阅读·2023 年 9 月 1 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F90e3bb200671&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fink-to-insights-comparing-sql-and-python-queries-using-bookshop-analytics-90e3bb200671&source=-----90e3bb200671---------------------bookmark_footer-----------)![](img/c7141249ee90ee12403abfe5fb98a1d2.png)

图片来源：[艾曼·优素福](https://unsplash.com/@ayman_yusuf97?utm_source=medium&utm_medium=referral)拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

SQL 是任何数据科学家工具箱中的基础——快速从数据源中提取数据进行分析的能力是处理大量数据的任何人的基本技能。在这篇文章中，我想展示一些我在 SQL 中通常使用的基本查询示例，贯穿 EDA 过程。我将这些查询与 Python 中产生相同输出的类似脚本进行比较，以此对比两种方法。

对于这次分析，我将使用一些来自虚构书店连锁（Total Fiction Bookstore）去年的高评分图书的合成数据。有关此项目的 github 文件夹的链接可以在[这里](https://github.com/jlenehan/Bookshop_EDA)找到，我在这里深入探讨了运行分析的细节。

![](img/d202ae38130a05412bd27ed575c3a2cb.png)

[Eugenio Mazzone](https://unsplash.com/@eugi1492?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

另外值得一提的是——虽然我在本文中主要关注 SQL 查询，但值得注意的是，这些查询可以通过 pandaSQL 库与 Python 无缝集成（就像我为这个项目所做的那样）。这可以在 Jupyter notebook 中详细看到…
