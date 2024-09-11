# Polars 字符串操作的技巧和窍门

> 原文：[`towardsdatascience.com/tips-and-tricks-for-working-with-strings-in-polars-ec6bb74aeec2?source=collection_archive---------5-----------------------#2023-01-17`](https://towardsdatascience.com/tips-and-tricks-for-working-with-strings-in-polars-ec6bb74aeec2?source=collection_archive---------5-----------------------#2023-01-17)

## 从排序列名到拆分列

[](https://weimenglee.medium.com/?source=post_page-----ec6bb74aeec2--------------------------------)![Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----ec6bb74aeec2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec6bb74aeec2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec6bb74aeec2--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----ec6bb74aeec2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftips-and-tricks-for-working-with-strings-in-polars-ec6bb74aeec2&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----ec6bb74aeec2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec6bb74aeec2--------------------------------) ·9 分钟阅读·2023 年 1 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fec6bb74aeec2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftips-and-tricks-for-working-with-strings-in-polars-ec6bb74aeec2&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----ec6bb74aeec2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fec6bb74aeec2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftips-and-tricks-for-working-with-strings-in-polars-ec6bb74aeec2&source=-----ec6bb74aeec2---------------------bookmark_footer-----------)![](img/dcdf237f50b5028ea8a4c18ac4d7898f.png)

由 [Raphael Schaller](https://unsplash.com/@raphaelphotoch?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我以前关于 Polars 的文章中 ([`medium.com/search?q=wei-meng+lee+polars`](https://medium.com/search?q=wei-meng+lee+polars))，我深入探讨了如何开始使用 Polars，如何利用其惰性求值模式来优化查询并提高处理大型数据集的效率，以及如何使用它执行各种任务，如数据清理、数据分析和数据可视化。

我没有深入探讨的一个领域是字符串处理，这在处理数据帧时是一个非常常见的话题。在本文中，我将介绍一些在 Polars 中进行字符串处理时可以使用的技巧和方法。它们包括：

+   排序 DataFrame 列

+   计算字符串的长度

+   基于标题选择列

+   使用正则表达式过滤行

+   拆分字符串列

+   替换字符串值

# Polars 中的所有标题必须是字符串类型

在我们深入探讨各种技巧和窍门之前，重要的是要记住在 Polars 中所有的列标题都是字符串类型。考虑以下示例：

```py
import polars as pl
import…
```
