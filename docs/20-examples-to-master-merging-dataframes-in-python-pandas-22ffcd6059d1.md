# 20 个 Python Pandas 合并 DataFrame 的示例

> 原文：[`towardsdatascience.com/20-examples-to-master-merging-dataframes-in-python-pandas-22ffcd6059d1?source=collection_archive---------3-----------------------#2023-05-30`](https://towardsdatascience.com/20-examples-to-master-merging-dataframes-in-python-pandas-22ffcd6059d1?source=collection_archive---------3-----------------------#2023-05-30)

## 一份全面的实用指南。

[](https://sonery.medium.com/?source=post_page-----22ffcd6059d1--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----22ffcd6059d1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----22ffcd6059d1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----22ffcd6059d1--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----22ffcd6059d1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F20-examples-to-master-merging-dataframes-in-python-pandas-22ffcd6059d1&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----22ffcd6059d1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----22ffcd6059d1--------------------------------) ·10 分钟阅读·2023 年 5 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F22ffcd6059d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F20-examples-to-master-merging-dataframes-in-python-pandas-22ffcd6059d1&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----22ffcd6059d1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F22ffcd6059d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F20-examples-to-master-merging-dataframes-in-python-pandas-22ffcd6059d1&source=-----22ffcd6059d1---------------------bookmark_footer-----------)

合并可以将来自不同来源的数据组合成一个统一的结构。这是处理表格数据时必不可少的操作，因为将所有数据存储在一个数据表或 DataFrame 中是不可能或不切实际的。

理解如何有效地合并 Pandas DataFrame 是任何数据科学家或分析师的关键技能。

合并意味着根据共享列或列中的值来组合 DataFrame。

![](img/b06dd333413f88c65677fc3c86734608.png)

合并 DataFrame（作者提供的图片）

在本文中，我们将通过一组全面的 20 个示例来阐明合并操作的细微差别。我们将从基本的合并函数开始，逐步深入更复杂的场景，涵盖关于使用 Pandas 合并 DataFrames 的所有细节。

*如果你想了解更多关于 Pandas 的内容，请访问我的课程* [*500 个练习掌握 Python Pandas*](https://www.udemy.com/course/500-exercises-to-master-python-pandas/learn/lecture/37842166#overview)*。*

我们将讨论的函数包括：

+   `merge`

+   `merge_asof`

+   `merge_ordered`

让我们从创建两个 DataFrames 开始，这些 DataFrames 将用于示例中。

```py
import numpy as np
import pandas as pd

names = pd.DataFrame(

    {
        "id": [1, 2, 3, 4, 10]…
```
