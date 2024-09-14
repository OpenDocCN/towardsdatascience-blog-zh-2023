# 如何在 Pandas 中更改日期时间格式

> 原文：[`towardsdatascience.com/datetime-format-pandas-541c661d41c2`](https://towardsdatascience.com/datetime-format-pandas-541c661d41c2)

## 在 Python 和 pandas 中处理日期时间

[](https://gmyrianthous.medium.com/?source=post_page-----541c661d41c2--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----541c661d41c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----541c661d41c2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----541c661d41c2--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----541c661d41c2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----541c661d41c2--------------------------------) ·4 分钟阅读·2023 年 1 月 6 日

--

![](img/985c477abc99a4c06172f1b8a1734c98.png)

图片来源：[Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/photos/LPRrEJU2GbQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Pandas 是 Python 生态系统中最常用的包之一，它提供了丰富的功能，帮助开发者在内存中分析和转换数据。

在 pandas DataFrame 中，日期时间构造的表示有时可能会变得有些复杂，特别是如果你对该库如何处理这些对象不是很熟悉的话。根据你是否希望将日期时间表示为 pandas `object` dtype（`string`）还是 `datetime`，你可能会发现自己在处理这些列时来回切换。

在这篇文章中，我们将展示如何处理 pandas 中不同的日期时间对象表示方式，以及如何更改格式或甚至数据类型。

首先，让我们创建一个示例 DataFrame，在本教程中我们将引用它以展示一些概念。

```py
import pandas as pd

df = pd.DataFrame(
  [
    (1, 'A', '12/02/1993', True),
    (2, 'C', '01/01/2000', False),
    (3, 'A', '10/05/2010', False),
    (4, 'B', '12/03/1967', True),
    (5, 'B', '19/10/2001', True),
    (6, 'D', '12/03/2002', True),
    (7, 'B', '04/12/2011', False),
    (8, 'A', '01/06/1989', True),
    (9, 'C', '30/11/1980', False),
    (10, 'C', '09/09/1976', False),
  ],
  columns=['colA', 'colB', 'colC', 'colD']
)

>>> df
   colA colB        colC   colD
0     1    A  12/02/1993   True
1     2    C  01/01/2000  False
2     3    A  10/05/2010  False
3     4    B  12/03/1967   True
4     5    B  19/10/2001   True
5     6    D  12/03/2002   True
6     7    B  04/12/2011  False
7     8    A  01/06/1989   True
8     9    C  30/11/1980  False
9    10    C  09/09/1976  False

>>> df.dtypes
colA     int64
colB    object
colC    object
colD      bool
dtype: object
```

## 将类型为对象的列转换为日期时间

在示例数据框中，我们将日期作为字符串提供，pandas 会自动将列解析为 `object` dtype。为了做到这一点，我们只需调用 `to_datetime()` 方法，并将结果赋回到原始列。此外，我们还指定日期的格式，以确保 pandas 正确解析它们：

```py
df['colC'] = pd.to_datetime(df.colC, format='%d/%m/%Y')
```

现在，如果我们打印出数据框的新 dtypes，我们应该会看到 `colC` 列的类型已经改变：

```py
>>> df.dtypes
colA             int64
colB            object
colC    datetime64[ns]
colD              bool
dtype: object
```

如果我们也打印出数据框，我们会注意到稍有不同的格式，因为 `colC` 的 dtype 已经改变：

```py
>>> df
   colA colB       colC   colD
0     1    A 1993-02-12   True
1     2    C 2000-01-01  False
2     3    A 2010-05-10  False
3     4    B 1967-03-12   True
4     5    B 2001-10-19   True
5     6    D 2002-03-12   True
6     7    B 2011-12-04  False
7     8    A 1989-06-01   True
8     9    C 1980-11-30  False
9    10    C 1976-09-09  False
```

## 更改日期时间对象的格式

现在我们已经将`colC`的数据类型转换为`datetime`，我们可以利用`strftime()`方法来修改记录的日期格式。

假设我们希望日期以`mm/dd/yyyy`格式表示。我们需要做的就是在调用`strftime()`时指定这种格式。我们不覆盖`colC`中的值，而是创建一个新的列以新格式表示：

```py
df['colE'] = df.colC.dt.strftime('%m/%d/%Y')
```

现在，新创建的列`colE`将遵循指定的格式：

```py
>>> df
   colA colB       colC   colD        colE
0     1    A 1993-02-12   True  02/12/1993
1     2    C 2000-01-01  False  01/01/2000
2     3    A 2010-05-10  False  05/10/2010
3     4    B 1967-03-12   True  03/12/1967
4     5    B 2001-10-19   True  10/19/2001
5     6    D 2002-03-12   True  03/12/2002
6     7    B 2011-12-04  False  12/04/2011
7     8    A 1989-06-01   True  06/01/1989
8     9    C 1980-11-30  False  11/30/1980
9    10    C 1976-09-09  False  09/09/1976
```

但请注意，结果列将是`object`类型，因为`strftime`生成的输出是字符串格式：

```py
>>> df.dtypes
colA             int64
colB            object
colC    datetime64[ns]
colD              bool
colE            object
dtype: object
```

## 最终想法

在 pandas 中处理日期时间对象有时可能是一项棘手的任务。这是因为这些对象可以使用各种数据类型表示，包括`object`或`datetime`。此外，日期格式也有所不同，主要受到用户位置的影响（例如，美国人对`mm/dd/yyyy`格式有强烈的偏好）。

在这篇文章中，我们演示了如何处理各种不同类型，以及如何在`object`和`datetime`对象之间来回切换。我希望本教程能帮助你更有效地处理这些数据类型！

[**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每一个故事。你的会员费用直接支持我和你阅读的其他作者。你还将获得对 Medium 上每一个故事的完全访问权限。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----541c661d41c2--------------------------------) [## 使用我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 会员，你的会员费用的一部分将用于支持你阅读的作者，你还将获得对每个故事的完全访问权限……

[gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----541c661d41c2--------------------------------)

**你可能也喜欢的相关文章**

[](/args-kwargs-python-d9c71b220970?source=post_page-----541c661d41c2--------------------------------) ## *args 和 **kwargs 在 Python 中

### 讨论位置参数和关键字参数之间的区别，以及如何在 Python 中使用*args 和 **kwargs

towardsdatascience.com [](/pycache-python-991424aabad8?source=post_page-----541c661d41c2--------------------------------) ## Python 中的 __pycache__ 是什么？

### 了解运行 Python 代码时创建的 __pycache__ 文件夹

towardsdatascience.com
