# 如何将多个 CSV 文件加载到 Pandas DataFrame 中

> 原文：[`towardsdatascience.com/load-multiple-csv-pandas-9c0c88c5adff`](https://towardsdatascience.com/load-multiple-csv-pandas-9c0c88c5adff)

## 导入并连接多个 CSV 文件到一个 pandas DataFrame 中

[](https://gmyrianthous.medium.com/?source=post_page-----9c0c88c5adff--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9c0c88c5adff--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c0c88c5adff--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c0c88c5adff--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----9c0c88c5adff--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c0c88c5adff--------------------------------) ·5 分钟阅读·2023 年 1 月 31 日

--

![](img/649a65c5a7ba242fecd95611a28a9576.png)

照片由 [Daniel K Cheung](https://unsplash.com/@danielkcheung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/cPF2nlWcMY4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

CSV（逗号分隔值）是一种常用的文件格式，用于存储和交换数据。事实上，这种类型的源通常用于相对较小的数据量。

`pandas` 是一个常用的 Python 包，它允许开发人员处理和转换数据，作为分析和数据科学任务的一部分。然而，在执行任何任务之前，pandas 需要将所有数据加载到内存中。这意味着该包只能用于相对较小的数据量——这取决于主机机器的能力，但对于一台普通的机器，我们只能在内存中加载几 GB 的数据。

因此，将 CSV 文件加载到内存中，然后用 pandas 处理数据，是一个非常常见的任务，因为这些文件通常包含可以加载到内存中的数据。

[**订阅数据管道**](https://thedatapipeline.substack.com/welcome)**，这是一个专注于数据工程的新闻通讯**

在这篇文章中，我们将演示如何将多个 CSV 文件加载到一个 pandas DataFrame 中。此外，我们还将展示如何在每条记录中识别源文件，以便我们可以确定哪个数据点属于某个数据文件。

现在假设我们在三个分开的 CSV 文件中收集了数据点，即 `data_1.csv`、`data_2.csv` 和 `data_3.csv`：

```py
colA,colB,colC
'A',1412,True
'B',1252,False
'C',1536,True
'D',1508,False
```

```py
colA,colB,colC
'E',1115,False
'F',6416,True
'G',6241,True
```

```py
colA,colB,colC
'H',1267,False
'I',1252,False
'J',2753,False
'K',7346,True
```

## 在一个 pandas DataFrame 中连接多个 CSV 文件

现在，我们有三个分开的 CSV 文件中的一些虚拟数据，我们可以继续将它们导入到一个 DataFrame 中。

我们可以选择的第一个选项是使用`pandas.read_csv()`函数读取每个单独的 CSV 文件，并使用`pandas.concatenate()`函数将所有加载的文件合并为一个单一的 DataFrame。

```py
import pandas as pd 

data_files = ['data_1.csv', 'data_2.csv', 'data_3.csv']
df = pd.concat((pd.read_csv(filename) for filename in data_files))
```

现在新构建的 DataFrame 包含了三个输入 CSV 文件中发现的所有数据点：

```py
>>> df
  colA  colB   colC
0  'A'  1412   True
1  'B'  1252  False
2  'C'  1536   True
3  'D'  1508  False
0  'E'  1115  False
1  'F'  6416   True
2  'G'  6241   True
0  'H'  1267  False
1  'I'  1252  False
2  'J'  2753  False
3  'K'  7346   True
```

注意，当从多个文件加载数据时，我们的 DataFrame 的索引会被重置。如果你希望为新创建的 DataFrame 创建一个新的索引，你只需在合并文件时忽略索引即可：

```py
import pandas as pd 

data_files = ['data_1.csv', 'data_2.csv', 'data_3.csv']
df = pd.concat(
  (pd.read_csv(filename) for filename in data_files), 
  ignore_index=True
)

>>> df
   colA  colB   colC
0   'A'  1412   True
1   'B'  1252  False
2   'C'  1536   True
3   'D'  1508  False
4   'E'  1115  False
5   'F'  6416   True
6   'G'  6241   True
7   'H'  1267  False
8   'I'  1252  False
9   'J'  2753  False
10  'K'  7346   True
```

## 避免明确指定文件名

现在假设我们有数百个不同的 CSV 文件，我们希望将它们合并为一个单一的 DataFrame。与其浪费时间和代码行数来明确写出所有单独的文件名，不如使用通配符。

我们可以利用`glob`模块，该模块是标准库的一部分，提供支持 Unix 样式路径名模式扩展的功能。例如，为了创建一个包含当前目录下所有以`.csv`结尾的文件的列表，我们可以使用以下代码片段：

```py
>>> import glob
>>>
>>> data_files = glob.glob('*.csv')
>>> data_files
['data_2.csv', 'data_3.csv', 'data_1.csv']
```

下面分享了一个完整的代码，该代码读取当前目录下的所有 CSV 文件，并将它们合并为一个单一的 pandas DataFrame：

```py
import glob

import pandas as pd 

data_files = glob.glob('*.csv')
df = pd.concat(
  (pd.read_csv(filename) for filename in data_files), 
  ignore_index=True
)
```

另一种方法——可能更符合 Python 风格——是利用`map`内置函数，它允许我们在不显式调用`for`循环的情况下，对 Iterable（如 Python 列表）运行方法或函数：

```py
import glob

import pandas as pd

df = pd.concat(map(pd.read_csv, glob.glob('*.csv')))
```

## 识别来自不同文件的记录

在某些其他用例中，知道给定记录的原始来源文件可能是我们需要跟踪的信息。

```py
import glob
import pandas as pd

df = pd.concat(
  [
    pd.read_csv(filename).assign(source=filename)
    for filename in glob.glob('*.csv')
  ], 
  ignore_index=True
)
```

现在我们期望我们的 DataFrame 将包含一个额外的列，用于指定每条记录添加到 DataFrame 的对应文件名：

```py
>>> df
   colA  colB   colC      source
0   'E'  1115  False  data_2.csv
1   'F'  6416   True  data_2.csv
2   'G'  6241   True  data_2.csv
3   'H'  1267  False  data_3.csv
4   'I'  1252  False  data_3.csv
5   'J'  2753  False  data_3.csv
6   'K'  7346   True  data_3.csv
7   'A'  1412   True  data_1.csv
8   'B'  1252  False  data_1.csv
9   'C'  1536   True  data_1.csv
10  'D'  1508  False  data_1.csv
```

## 最后想法

在本文中，我们展示了如何加载多个 CSV 文件并将它们合并到一个 pandas DataFrame 中。此外，我们展示了如何在实际执行导入时，不需要明确指定要加载的文件名。最后，我们讨论了如何在加载的 DataFrame 中创建一个新列，以便识别每条记录的来源文件。

现在你已经将数据加载到 pandas 中，你可以利用该包提供的丰富 API，进行分析、转换以及你可能需要的任何处理。如果你打算将 pandas DataFrame 写回 CSV 文件，请确保遵循下面的指南👇。

## 如何将 Pandas DataFrame 写入 CSV 文件

### 利用在将 pandas DataFrame 写入 CSV 文件时提供的所有选项

towardsdatascience.com

[**订阅 Data Pipeline**](https://thedatapipeline.substack.com/welcome)**，一份专注于数据工程的通讯**

👇 **你可能还会喜欢的相关文章** 👇

[](/etl-vs-elt-68e221d78719?source=post_page-----9c0c88c5adff--------------------------------) ## ETL 与 ELT：有什么区别？

### 数据工程中 ETL 与 ELT 的比较

towardsdatascience.com [](/cte-sql-945e4b461de3?source=post_page-----9c0c88c5adff--------------------------------) ## SQL 中的 CTE 是什么

### 理解 SQL 中的**公用表表达式**（CTE）

towardsdatascience.com [](/dbt-55b35c974533?source=post_page-----9c0c88c5adff--------------------------------) ## 什么是 dbt（数据构建工具）

### 介绍正在主宰数据领域的 dbt

towardsdatascience.com
