# 如何将 Pandas DataFrame 写入 CSV 文件

> 原文：[`towardsdatascience.com/pandas-to-csv-aab4ac27c455`](https://towardsdatascience.com/pandas-to-csv-aab4ac27c455)

## 利用将 pandas DataFrames 写入 CSV 文件时提供的所有选项

[](https://gmyrianthous.medium.com/?source=post_page-----aab4ac27c455--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----aab4ac27c455--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aab4ac27c455--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aab4ac27c455--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----aab4ac27c455--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aab4ac27c455--------------------------------) ·阅读时间 5 分钟·2023 年 1 月 13 日

--

![](img/881b8b4f1e7bf014f76b61f5f78e7900.png)

图片由 [Hello I’m Nik](https://unsplash.com/fr/@helloimnik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/l4ADb9OVqTY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在使用 pandas 时，一个非常常见的任务是将 DataFrame 导出到 CSV 文件。

在本文中，我们将演示如何将 pandas DataFrame 写入 CSV 文件。此外，我们还将展示如何使用 pandas API 的各种选项来处理缺失值、日期时间格式、压缩，以及处理 CSV 文件中 DataFrame 时可能需要的其他功能。

现在让我们创建一个示例 DataFrame，我们将在本教程中使用并引用它，以演示将 pandas DataFrame 导出为 CSV 文件的几种不同选项。

```py
from datetime import datetime

import pandas as pd

df = pd.DataFrame(
  [
    (1, 'A', True, datetime(2018, 12, 27), 158, None),
    (2, 'B', False, datetime(2019, 11, 15), 10, 76.3),
    (3, 'A', True, None, 210, 78.1),
    (4, 'F', None, datetime(2022, 10, 5), 679, 54.1),
    (5, 'F', False, datetime(2021, 10, 10), 765, 21.1),
    (6, 'F', True, datetime(2021, 7, 23), 132, 4.5),
    (7, 'C', False, datetime(2017, 4, 5), 45, 5.6),
    (8, 'B', False, datetime(2016, 3, 4), 67, 6.7),
    (9, 'A', True, datetime(2019, 4, 5), 786, 78.1),
    (10, 'C', False, datetime(2023, 1, 1), 78, 89.2),
  ],
  columns=['colA', 'colB', 'colC', 'colD', 'colE', 'colF']
)

>>> df
   colA colB   colC       colD  colE  colF
0     1    A   True 2018-12-27   158   NaN
1     2    B  False 2019-11-15    10  76.3
2     3    A   True        NaT   210  78.1
3     4    F   None 2022-10-05   679  54.1
4     5    F  False 2021-10-10   765  21.1
5     6    F   True 2021-07-23   132   4.5
6     7    C  False 2017-04-05    45   5.6
7     8    B  False 2016-03-04    67   6.7
8     9    A   True 2019-04-05   786  78.1
9    10    C  False 2023-01-01    78  89.2

>>> df.dtypes
colA             int64
colB            object
colC            object
colD    datetime64[ns]
colE             int64
colF           float64
dtype: object
```

## 将 pandas DataFrame 导出为 CSV 文件

要将 pandas DataFrame 导出为 `csv` 文件，你只需调用 `pandas.DataFrame.to_csv()` 函数并提供输出文件名：

```py
df.to_csv('output_df.csv')
```

**输出**

```py
,colA,colB,colC,colD,colE,colF
0,1,A,True,2018-12-27,158,
1,2,B,False,2019-11-15,10,76.3
2,3,A,True,,210,78.1
3,4,F,,2022-10-05,679,54.1
4,5,F,False,2021-10-10,765,21.1
5,6,F,True,2021-07-23,132,4.5
6,7,C,False,2017-04-05,45,5.6
7,8,B,False,2016-03-04,67,6.7
8,9,A,True,2019-04-05,786,78.1
9,10,C,False,2023-01-01,78,89.2
```

## 更改默认分隔符

如果你想更改默认分隔符（ `,` ），则需要提供 `sep` 参数。以下示例将 pandas DataFrame 持久化到一个 CSV 文件中，其中列值将使用制表符分隔：

```py
df.to_csv('output_df.csv', sep='\t')
```

**输出**

```py
 colA colB colC colD colE colF
0 1 A True 2018-12-27 158 
1 2 B False 2019-11-15 10 76.3
2 3 A True  210 78.1
3 4 F  2022-10-05 679 54.1
4 5 F False 2021-10-10 765 21.1
5 6 F True 2021-07-23 132 4.5
6 7 C False 2017-04-05 45 5.6
7 8 B False 2016-03-04 67 6.7
8 9 A True 2019-04-05 786 78.1
9 10 C False 2023-01-01 78 89.2
```

## 排除索引

在前面的示例中，你可能已经注意到，除了列名和数值外，索引也会被保留。如果你想去掉它，只需提供 `index=False` 参数：

```py
df.to_csv('output_df.csv', index=False)
```

**输出**

```py
colA,colB,colC,colD,colE,colF
1,A,True,2018-12-27,158,
2,B,False,2019-11-15,10,76.3
3,A,True,,210,78.1
4,F,,2022-10-05,679,54.1
5,F,False,2021-10-10,765,21.1
6,F,True,2021-07-23,132,4.5
7,C,False,2017-04-05,45,5.6
8,B,False,2016-03-04,67,6.7
9,A,True,2019-04-05,786,78.1
10,C,False,2023-01-01,78,89.2
```

## 移除标题

同样，你可以通过传递 `header=False` 来去掉标题（即列名）。

```py
df.to_csv('output_df.csv', index=False, header=False)
```

**输出**

```py
1,A,True,2018-12-27,158,
2,B,False,2019-11-15,10,76.3
3,A,True,,210,78.1
4,F,,2022-10-05,679,54.1
5,F,False,2021-10-10,765,21.1
6,F,True,2021-07-23,132,4.5
7,C,False,2017-04-05,45,5.6
8,B,False,2016-03-04,67,6.7
9,A,True,2019-04-05,786,78.1
10,C,False,2023-01-01,78,89.2
```

## 只写出一部分列

有时，你可能只想导出 DataFrame 对象中的一部分列。你可以通过在 `columns` 参数中指定所需的列来选择要持久化的列：

```py
df.to_csv('output_df.csv', index=False, header=False)
```

**输出**

```py
colA,colD,colF
1,2018-12-27,
2,2019-11-15,76.3
3,,78.1
4,2022-10-05,54.1
5,2021-10-10,21.1
6,2021-07-23,4.5
7,2017-04-05,5.6
8,2016-03-04,6.7
9,2019-04-05,78.1
10,2023-01-01,89.2
```

## 指定日期时间对象的格式

在我们的示例 DataFrame 中，列 `colD` 的类型为 `datetime`，默认情况下，它将以 `YYYY-MM-DD` 格式导出。你可以在 `date_format` 参数中指定所需的日期格式。

```py
df.to_csv('output_df.csv', index=False, date_format='%d/%m/%Y')
```

**输出**

```py
colA,colB,colC,colD,colE,colF
1,A,True,27/12/2018,158,
2,B,False,15/11/2019,10,76.3
3,A,True,,210,78.1
4,F,,05/10/2022,679,54.1
5,F,False,10/10/2021,765,21.1
6,F,True,23/07/2021,132,4.5
7,C,False,05/04/2017,45,5.6
8,B,False,04/03/2016,67,6.7
9,A,True,05/04/2019,786,78.1
10,C,False,01/01/2023,78,89.2
```

如果你有兴趣了解更多关于如何处理 Python 中日期时间对象格式的内容，你可以阅读我最近在 Medium 上的一篇文章。

[](/datetime-format-pandas-541c661d41c2?source=post_page-----aab4ac27c455--------------------------------) ## 如何更改 Pandas 中的日期时间格式

### 在 Python 和 pandas 中处理日期时间

towardsdatascience.com

## 更改 NaN 值写入 CSV 文件的方式

默认情况下，`to_csv()` 函数会将缺失值导出为空字符。`na_rep` 参数可以用来指定 `NaN` 值的所需输出格式：

```py
df.to_csv('output_df.csv', index=False, na_rep='N/A')
```

**输出**

```py
colA,colB,colC,colD,colE,colF
1,A,True,2018-12-27,158,N/A
2,B,False,2019-11-15,10,76.3
3,A,True,N/A,210,78.1
4,F,N/A,2022-10-05,679,54.1
5,F,False,2021-10-10,765,21.1
6,F,True,2021-07-23,132,4.5
7,C,False,2017-04-05,45,5.6
8,B,False,2016-03-04,67,6.7
9,A,True,2019-04-05,786,78.1
10,C,False,2023-01-01,78,89.2
```

## 其他选项

对于大型 DataFrame，你可能需要将输出压缩为 `zip` 格式。为此，你可以在 `compression` 参数中指定所需的压缩策略。

```py
df.to_csv('output_df.csv', index=False, compression='gzip')
```

其他一些压缩策略包括 `'zip'`、`'gzip'`、`'bz2'`、`'zstd'`、`'tar'`。

最后，要更改文件格式编码，你可以利用 `encoding` 参数：

```py
df.to_csv('output_df.csv', index=False, encoding='utf-8')
```

## 最后的思考

总结来说，本文展示了将 pandas DataFrame 写入 CSV 文件时的各种选项。从更改默认分隔符、排除索引、移除标题、仅写入部分列、指定日期时间对象的格式，到更改 NaN 值写入 CSV 文件的方式。

pandas API 提供了多种选项来处理将 DataFrame 持久化到 CSV 文件中的不同场景。这使得它成为一个强大的数据处理和分析工具。根据本文提供的知识，你应该能够轻松而准确地将你的 DataFrame 导出到 CSV 文件中。

[**成为会员**](https://gmyrianthous.medium.com/membership) **，阅读 Medium 上的每个故事。你的会员费用直接支持我和你阅读的其他作者。你还将获得对 Medium 上每个故事的全面访问权限。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----aab4ac27c455--------------------------------) [## 使用我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 会员，你的部分会员费用会分配给你阅读的作者，并且你可以全面访问每个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----aab4ac27c455--------------------------------)

**你可能也感兴趣的相关文章**

[](/diagrams-as-code-python-d9cbaa959ed5?source=post_page-----aab4ac27c455--------------------------------) ## Python 中的代码图示

### 创建 Python 云系统架构图

towardsdatascience.com [](/args-kwargs-python-d9c71b220970?source=post_page-----aab4ac27c455--------------------------------) ## *args 和 **kwargs 在 Python 中的使用

### 讨论位置参数和关键字参数之间的区别以及如何在 Python 中使用 *args 和 **kwargs

towardsdatascience.com
