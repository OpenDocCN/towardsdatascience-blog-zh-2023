# 探索 Numexpr：Pandas 背后的强大引擎

> 原文：[`towardsdatascience.com/exploring-numexpr-a-powerful-engine-behind-pandas-cdb94965ca3a?source=collection_archive---------9-----------------------#2023-09-22`](https://towardsdatascience.com/exploring-numexpr-a-powerful-engine-behind-pandas-cdb94965ca3a?source=collection_archive---------9-----------------------#2023-09-22)

## [快速计算](https://qtalen.medium.com/list/fast-computing-2a37a7e82be5)

## 使用 Python 的 Numexpr 和 Pandas 的 eval/query 函数来提升数据分析性能

[](https://qtalen.medium.com/?source=post_page-----cdb94965ca3a--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----cdb94965ca3a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cdb94965ca3a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cdb94965ca3a--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----cdb94965ca3a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-numexpr-a-powerful-engine-behind-pandas-cdb94965ca3a&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----cdb94965ca3a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cdb94965ca3a--------------------------------) · 10 分钟阅读 · 2023 年 9 月 22 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcdb94965ca3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-numexpr-a-powerful-engine-behind-pandas-cdb94965ca3a&user=Peng+Qian&userId=8e2fe735546d&source=-----cdb94965ca3a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcdb94965ca3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-numexpr-a-powerful-engine-behind-pandas-cdb94965ca3a&source=-----cdb94965ca3a---------------------bookmark_footer-----------)![](img/fa720ac983cd13291d2bf6c1b49b674d.png)

使用 Numexpr 帮我找到最宜居的城市。照片来源：作者制作，[Canva](https://www.canva.com/)

本文将向你介绍 Python 库 [Numexpr](https://numexpr.readthedocs.io/en/latest/intro.html?ref=dataleadsfuture.com#)，这是一个提升 `Numpy Arrays` 计算性能的工具。Pandas 的 `eval` 和 `query` 方法也基于这个库。

本文还包括一个动手的天气数据分析项目。

通过阅读这篇文章，你将理解 Numexpr 的原理以及如何使用这一强大工具来加快现实中的计算。

# 介绍

## 回顾 Numpy 数组

在上一篇讨论 `Numpy Arrays` 的文章中，我用一个库的例子解释了为什么 Numpy 的缓存局部性如此高效：

[## Python 列表与 NumPy 数组：深入探讨内存布局与性能优势](https://towardsdatascience.com/python-lists-vs-numpy-arrays-a-deep-dive-into-memory-layout-and-performance-benefits-a74ce774bc1e?source=post_page-----cdb94965ca3a--------------------------------)

### 探索分配差异和效率提升

[towardsdatascience.com](https://towardsdatascience.com/python-lists-vs-numpy-arrays-a-deep-dive-into-memory-layout-and-performance-benefits-a74ce774bc1e?source=post_page-----cdb94965ca3a--------------------------------)

每次你去图书馆寻找资料时，你会取出几本与内容相关的书籍，并将它们放在桌子旁边。
