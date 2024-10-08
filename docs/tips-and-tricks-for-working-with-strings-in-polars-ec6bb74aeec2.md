# 在 Polars 中处理字符串的技巧与窍门

> 原文：[`towardsdatascience.com/tips-and-tricks-for-working-with-strings-in-polars-ec6bb74aeec2`](https://towardsdatascience.com/tips-and-tricks-for-working-with-strings-in-polars-ec6bb74aeec2)

## 从排序列名到拆分列

[](https://weimenglee.medium.com/?source=post_page-----ec6bb74aeec2--------------------------------)![魏梦龙](https://weimenglee.medium.com/?source=post_page-----ec6bb74aeec2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec6bb74aeec2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec6bb74aeec2--------------------------------) [魏梦龙](https://weimenglee.medium.com/?source=post_page-----ec6bb74aeec2--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec6bb74aeec2--------------------------------) ·阅读时长 9 分钟·2023 年 1 月 17 日

--

![](img/dcdf237f50b5028ea8a4c18ac4d7898f.png)

图片来源 [Raphael Schaller](https://unsplash.com/@raphaelphotoch?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我过去关于 Polars 的文章中（[`medium.com/search?q=wei-meng+lee+polars`](https://medium.com/search?q=wei-meng+lee+polars)），我深入探讨了如何开始使用 Polars，它的惰性计算模式如何帮助优化查询和提高处理大数据集的效率，以及如何利用它进行各种任务，如数据清理、数据分析和数据可视化。

我没有深入探讨的一个领域是字符串处理，这在处理数据框时是一个非常常见的话题。在本文中，我将介绍一些你可以在 Polars 中进行字符串处理时使用的技巧和方法。它们包括：

+   排序 DataFrame 列

+   计算字符串长度

+   根据标题选择列

+   使用正则表达式过滤行

+   拆分字符串列

+   替换字符串值

# Polars 中的所有标题必须是字符串类型

在我们深入各种技巧和窍门之前，重要的是要记住，在 Polars 中，所有列标题都是字符串类型。请考虑以下示例：

```py
import polars as pl
import numpy as npdf = pl.DataFrame(np.random.randint(0, 100, size=(10, 4)),  
                  columns=list('CDAB'))
df
```

![](img/9a3525a54b857eec56756b52deab2280.png)

上面的示例展示了一个具有四列的 Polars DataFrame。请注意，与 Pandas 不同，在 Pandas 中列标题可以是数字类型，而在 Polars 中，所有列标题必须是字符串类型。以下是**不允许**的：

```py
df = pl.DataFrame(np.random.randint(0, 100, size=(10, 4)), 
                  columns=[1,2,3,4])  # error
```

如果你真的想使用数字作为列标题，你需要将它们转换为字符串：

```py
df = pl.DataFrame(np.random.randint(0, 100, size=(10, 4)), 
                  columns=list('1234'))
```

# 列排序

要排序 Polars DataFrame 中的列，首先使用 Python 的 `sorted()` 函数对列名进行排序，然后使用 `select()` 函数重新排列列的顺序：

```py
df.select(
    sorted(df.columns)
)
```

`select()` 函数返回一个新的 DataFrame，列的顺序已重新排列：

![](img/f142bdb117c912cf02e1a4ccf5661c76.png)

> 本文中的所有图像均由作者创建

`sorted()` 函数的默认排序顺序是字母顺序。要以相反的顺序进行排序，将 `reverse` 参数设置为 `True`：

```py
df.select(
    sorted(df.columns, reverse=True)
)
```

![](img/0b272c5380135528a2f181b0d886c548.png)

你可能会想使用方括号表示法进行排序（如 Pandas 中所示）：

```py
df[sorted(df.columns, reverse=True)]  # not recommended in Polars
```

但是，如果你打算将结果与其他 Polars 函数一起使用，这种方法不推荐，因为这种方法不能与延迟计算一起使用。

> 如果你只是想大致了解数据的样子，可以使用方括号表示法。

# 字符串长度计数

有时你需要计算特定列中字符串的长度。为此，我有一个名为 **names.csv** 的自制 CSV 文件：

```py
name,age
Kristopher Ruch,23
Lakiesha Halton,45
Yun Frei,23
Sharyn Llanos,76
Lorretta Herren,21
Merrilee Akana,67
Boyd Gilman,89
Heidy Smalling,11
Leta Batalla,45
Siu Wayne,67
Sammie Gurule,23
Jayne Whetzel,11
Byron Doggett,67
Luke Alcazar,90
Petra Doutt,12
Tula Parkhurst,67
Davina Hess,26
Enda Cornelius,78
Merlyn Cora,89
Jeanett Hardeman,34
```

首先，将其加载到 Polars DataFrame 中：

```py
import polars as pl
q = (
    pl.scan_csv('names.csv')    
)
q.collect()
```

![](img/c716d58c888417f5687d97500b4d724d.png)

你可以使用 `lengths()` 函数获取每个名称的长度，然后将其存储在名为 **length_of_name** 的新列中：

```py
import polars as pl
q = (
    pl.scan_csv('names.csv')
    .select(
    [
        'name',
        'age',
        pl.col('name').str.lengths().alias('length_of_name'),        
    ])
)
q.collect()
```

![](img/15be3991c81149d0577cc911ba088c97.png)

# 基于标题选择列

在我之前关于 Polars 的文章中 (`towardsdatascience.com/getting-started-with-the-polars-dataframe-library-6f9e1c014c5c`)，我展示了如何使用 `select()` 函数从数据框中选择列。让我们通过 Titanic 数据集详细了解一下。

> **数据来源**：本节的数据来源于 [`www.kaggle.com/datasets/tedllh/titanic-train`](https://www.kaggle.com/datasets/tedllh/titanic-train)。
> 
> **许可 —** 数据库内容许可（DbCL）v1.0 [`opendatacommons.org/licenses/dbcl/1-0/`](https://opendatacommons.org/licenses/dbcl/1-0/)

加载 Titanic 数据集时，你会看到它有 12 列：

```py
import polars as pl
q = (
    pl.scan_csv('Titanic_train.csv')
)
q.collect()
```

![](img/32d935a6d0af970fe576ce373532a740.png)

如果你只想检索 **Name** 和 **Age** 列，将它们放入列表中并传递给 `select()` 函数：

```py
import polars as pl
q = (
    pl.scan_csv('Titanic_train.csv')
    .select(
        ['Name','Age']
    )
)
q.collect()
```

![](img/d6bfd61eee91c4ff3e242d35224b23a6.png)

如果你想要 *所有* 列，除了 **PassengerId** 列，可以使用 `pl.exclude()` 函数：

```py
import polars as pl
q = (
    pl.scan_csv('Titanic_train.csv')
    .select(
        pl.exclude('PassengerId')
    )
)
q.collect()
```

![](img/dd93527f024bd4ccf087b05b51f1aaca.png)

`exclude()` 函数支持正则表达式。以下示例检索所有列，除了那些名称以“S”开头的列：

```py
import polars as pl
q = (
    pl.scan_csv('Titanic_train.csv')
    .select(
        pl.exclude("^S.*$")   # exclude all columns that starts with S
    )
)
q.collect()
```

![](img/5f0f112b0db98e101402314d29eea22b.png)

如果你想检索特定的列，可以使用 `pl.col()` 函数。以下示例检索所有以“S”开头的列：

```py
import polars as pl
q = (
    pl.scan_csv('Titanic_train.csv')
    .select(
        pl.col('^S.*$')  # get all columns that starts with S
    )
)
q.collect()
```

![](img/6538399c8809b03aafb983293fe5de5e.png)

# 使用正则表达式筛选行

除了 `pl.col()` 函数，`contains()` 函数也支持正则表达式。以下代码片段检索所有名称以“William”结尾的行：

```py
import polars as pl
q = (
    pl.scan_csv('Titanic_train.csv')
    .filter(
        pl.col('Name').str.contains('William$')       
    )
    .select(
        [
            'Name'
        ]
    )
)
q.collect().to_pandas()
```

![](img/f0f6b7df958dc7f76f9483b47f37c3e3.png)

你也可以尝试以下表达式：

+   `[Ww]illiam`

+   `(?i)illiam`

+   `^William`

你能弄清楚它们的作用吗？

这是一个挑战：你如何找到所有**不**以“William”结尾的名字？

好吧，你可以使用正则表达式来做到这一点，但最简单的方法是使用`is_not()`函数来否定`contains()`函数中指定的条件：

```py
import polars as pl
q = (
    pl.scan_csv('Titanic_train.csv')
    .filter(
        pl.col('Name').str.contains('William$').is_not()        
    )
    .select(
        [
            'Name'
        ]
    )
)
q.collect().to_pandas()
```

![](img/e8102b7eb570b6fd9586612f29d303f5.png)

> 请注意，Polars 不支持正则表达式中的前瞻和回顾。

# 字符串列拆分

另一个关于字符串的流行活动是列拆分。现在让我们基于空格使用`split()`函数拆分**name**列（在**names.csv**文件中）：

```py
q = (
    pl.scan_csv('names.csv')
    .select(
    [
        'name',
        pl.col('name').str.split(' ').alias('splitname'),
        'age',
    ])
)

q.collect()
```

结果是一个名为**split_name**的新列，如下所示：

![](img/dd04a214ddf9889ddb5bfb517ff03d26.png)

请注意，名字现在被拆分成了字符串列表。接下来你需要做的是将字符串列表转换为多个列，表示名字和姓氏。你可以使用`with_column()`和`pl.struct()`函数来完成：

```py
q = (
    pl.scan_csv('names.csv')
    .select(
    [
        'name',
        pl.col('name').str.split(' ').alias('split_name'),
        'age',
    ])
    .with_column(
        pl.struct(
            [
                pl.col('split_name').arr.get(i).alias(
                    'first_name' if i==0 else 'last_name') 
                for i in range(2)
            ]
        ).alias('split_name')
    )
)
q.collect()
```

`with_column()`函数返回一个更新了列的新 DataFrame。在这种情况下，我将使用`pl.struct()`函数更新`split_name`列，该函数遍历**split_name**列中的名字列表。将结构视为一组列。更新后的数据框现在如下所示：

![](img/6ef53174a109b045452f9731ce436b55.png)

值得注意的是，**split_name**列现在是`struct`类型。最后一步是使用`unnest()`函数将`struct`列拆分为单独的列：

```py
q = (
    pl.scan_csv('names.csv')
    .select(
    [
        'name',
        pl.col('name').str.split(' ').alias('split_name'),
        'age',
    ])
    .with_column(
        pl.struct(
            [
                pl.col('split_name').arr.get(i).alias(
                    'first_name' if i==0 else 'last_name') 
                for i in range(2)
            ]
        ).alias('split_name')
    )
    .unnest('split_name')
)
q.collect()
```

最终结果如下：

![](img/264ef3b593565818bd3597919d6b341d.png)

现在让我们用 Titanic 数据集尝试另一个例子。特别是，我想关注**Name**列：

```py
q = (
    pl.scan_csv('Titanic_train.csv')
)
q.collect()['Name'].to_pandas().unique()
```

这是**Name**列中唯一名字的快照：

```py
'Braund, Mr. Owen Harris',
'Cumings, Mrs. John Bradley (Florence Briggs Thayer)',
'Heikkinen, Miss. Laina',
'Futrelle, Mrs. Jacques Heath (Lily May Peel)',
'Allen, Mr. William Henry', 'Moran, Mr. James',
'McCarthy, Mr. Timothy J', 'Palsson, Master. Gosta Leonard',
'Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)',
'Nasser, Mrs. Nicholas (Adele Achem)',
...
```

名字以以下格式保存：

`姓氏，头衔。名字`

理想情况下，你可以在`split()`函数中使用正则表达式。你可以使用以下正则表达式将**Name**列拆分为姓氏、头衔和名字：

`‘([\’A-Za-z ()”//.-]+), ([A-Za-z]+). ([A-Za-z ()”//.-]*)’`

> 这个正则表达式在我之前的文章中讨论过：

[](/feature-engineering-using-regular-expression-regex-in-pandas-dataframe-c965380316da?source=post_page-----ec6bb74aeec2--------------------------------) ## 使用正则表达式（RegEx）进行特征工程（Pandas DataFrame）

### 探索如何使用正则表达式轻松操作你的字符串列

towardsdatascience.com

不幸的是，在写作时，Polars 中的`split()`函数不支持正则表达式。

所以策略是多次执行拆分：

![](img/ba52331a15b5a7cb0353577658da7d49.png)

这是第一次拆分：

```py
q = (
    pl.scan_csv('Titanic_train.csv')
    .select(
    [
        pl.col('Name').str.split(r', ').alias('split_name'),
    ])
    .with_column(
        pl.struct(
            [
                pl.col('split_name').arr.get(i).alias(
                    'Last Name' if i==0 else 'First Name') 
                for i in range(2)
            ]
        ).alias('split_name')
    ).unnest('split_name')
)
q.collect()
```

结果现在显示**姓氏**被提取出来，接着是**名字**，其中包含标题和名字：

![](img/4c6e537834cb5d50f0e9eab2f1042664.png)

这是第二次分割：

```py
q = (
    pl.scan_csv('Titanic_train.csv')
    .select(
    [
        pl.col('Name').str.split(r', ').alias('split_name'),
    ])
    .with_column(
        pl.struct(
            [
                pl.col('split_name').arr.get(i).alias(
                    'Last Name' if i==0 else 'First Name') 
                for i in range(2)
            ]
        ).alias('split_name')
    ).unnest('split_name')
    #---Second split---
    .select(
        [
            pl.exclude('First Name'), # get all columns except first name
            pl.col('First Name').str.split(r'. ').alias('split_name'),
        ]
    )
    .with_column(
        pl.struct(
            [
                pl.col('split_name').arr.get(i).alias(
                    'Title' if i==0 else 'First Name') 
                for i in range(2)
            ]
        ).alias('split_name')
    ).unnest('split_name')    
)
q.collect()
```

现在从名字中提取标题：

![](img/58832fd35ba06cd7b570fb51a13c923b.png)

# 替换字符串值

替换 DataFrame 中的字符串值是你常常进行的另一个任务。在 Titanic 数据集中，乘客名称中有很多重复的标题。例如，对于女性乘客，使用的一些标题有 Ms、Miss、Mlle、Mlle 和 Mme。你通常会想将标题数量减少到一个更可管理的范围。

要在列中替换字符串，请使用 `replace()` 函数：

```py
q = (
    pl.scan_csv('Titanic_train.csv')        
    .select(
    [
        pl.col('Name').str.replace('Mlle.','Miss.'),
    ])
)
q.collect()
```

上面的代码片段将 **Name** 列中所有的 “Mlle.” 替换为 “Miss.”。如果你想将多个标题替换为 “Miss.”，你可以使用正则表达式，如下所示：

```py
q = (
    pl.scan_csv('Titanic_train.csv')        
    .select(
    [
        pl.col('Name').str.replace('Mlle.|Ms.|Mme.','Miss.'),
    ])
)
q.collect()
```

上面的代码片段将 “Mlle.”、 “Ms.” 和 “Mme.” 替换为 “Miss.”。

## 如果你喜欢阅读我的文章并且它对你的职业/学习有所帮助，请考虑注册成为 Medium 会员。每月 5 美元，它让你可以无限制地访问 Medium 上的所有文章（包括我的）。如果你使用以下链接注册，我将赚取少量佣金（对你没有额外费用）。你的支持意味着我将能花更多时间撰写像这样的文章。

[](https://weimenglee.medium.com/membership?source=post_page-----ec6bb74aeec2--------------------------------) [## 通过我的推荐链接加入 Medium - Wei-Meng Lee]

### 阅读 Wei-Meng Lee 的每一个故事（以及 Medium 上其他成千上万的作者）。你的会员费直接支持…

weimenglee.medium.com](https://weimenglee.medium.com/membership?source=post_page-----ec6bb74aeec2--------------------------------)

# 总结

在 Polars 中处理字符串类似于 Pandas。此外，如果你熟悉正则表达式，它一定会让你的工作变得更轻松。我在这篇文章中使用了相当多的函数，所以这里有一个方便的指南，供你在下一次处理 Polars 中的字符串时参考：

+   `select()` — 从 DataFrame 中选择你需要的列

+   `with_column()` — 返回一个更新了列的新 DataFrame

+   `unnest()` — 将一个结构体列分解为单独的列

+   `str.lengths()` — 返回字符串的长度

+   `str.contains()` — 检查字符串是否包含指定的字符串

+   `str.split()` — 基于指定的字符串分割字符串

+   `str.replace()` — 用另一个字符串替换字符串

+   `pl.exclude()` — 排除特定列

+   `pl.col()` — 包括特定的列

+   `pl.struct()` — 包含一组列的列
