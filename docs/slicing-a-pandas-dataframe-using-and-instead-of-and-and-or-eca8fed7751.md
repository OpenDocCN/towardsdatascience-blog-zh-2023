# 使用“&” 和 “|” 切片 Pandas Dataframe，而不是 “and” 和 “or”

> 原文：[`towardsdatascience.com/slicing-a-pandas-dataframe-using-and-instead-of-and-and-or-eca8fed7751?source=collection_archive---------19-----------------------#2023-04-14`](https://towardsdatascience.com/slicing-a-pandas-dataframe-using-and-instead-of-and-and-or-eca8fed7751?source=collection_archive---------19-----------------------#2023-04-14)

## 3-分钟 Pandas

## 当你看到 ValueError: The truth value of a Series is ambiguous. Use a.empty, a.bool(), a.item(), a.any() 或 a.all()

[](https://jianan-lin.medium.com/?source=post_page-----eca8fed7751--------------------------------)![Yufeng](https://jianan-lin.medium.com/?source=post_page-----eca8fed7751--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eca8fed7751--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eca8fed7751--------------------------------) [Yufeng](https://jianan-lin.medium.com/?source=post_page-----eca8fed7751--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9cda0369fb2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fslicing-a-pandas-dataframe-using-and-instead-of-and-and-or-eca8fed7751&user=Yufeng&userId=9cda0369fb2&source=post_page-9cda0369fb2----eca8fed7751---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eca8fed7751--------------------------------) ·6 分钟阅读·2023 年 4 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feca8fed7751&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fslicing-a-pandas-dataframe-using-and-instead-of-and-and-or-eca8fed7751&user=Yufeng&userId=9cda0369fb2&source=-----eca8fed7751---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feca8fed7751&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fslicing-a-pandas-dataframe-using-and-instead-of-and-and-or-eca8fed7751&source=-----eca8fed7751---------------------bookmark_footer-----------)![](img/500c0747fef4427deeade943cf346e84.png)

照片由 [TJ Arnold](https://unsplash.com/@missinformed?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据筛选/切片是处理数据时的日常任务。

数据切片的基本思想是选择满足特定条件的行。例如，选择第二列值小于 3 的行，选择第三列值在预定义列表中的行，选择第五列值以‘ABC’开头的行，等等（[有关如何详细切片，请参见此帖子](https://medium.com/towards-data-science/extract-rows-columns-from-a-dataframe-in-python-r-678e5b6743d6)）。

如果你在 Python 库`pandas`中进行数据切片并通过`and`或`or`运算符组合多个条件，你一定遇到过这个问题。

```py
ValueError: The truth value of a Series is ambiguous. Use a.empty, a.bool(), a.item(), a.any() or a.all()
```

这是一个示例。让我们在 pandas 中创建一个数据框，

```py
import pandas as pd
import numpy as np

data = {'name': ['John', 'Emily', 'David', 'Samantha', 'Michael', 'Amy', 'Mark', 'Jessica', 'Lucas', 'Maria'],
        'age': [25, 32, 19, 41, 28, 36, 24, 27, 30, 39],
        'city': ['New York', 'Paris', 'London'…
```
