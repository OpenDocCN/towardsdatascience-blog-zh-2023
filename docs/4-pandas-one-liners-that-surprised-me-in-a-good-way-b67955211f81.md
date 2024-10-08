# 4 个 Pandas 一行代码解决特定任务的高效方法

> 原文：[`towardsdatascience.com/4-pandas-one-liners-that-surprised-me-in-a-good-way-b67955211f81`](https://towardsdatascience.com/4-pandas-one-liners-that-surprised-me-in-a-good-way-b67955211f81)

## 以快速简单的方式完成复杂任务

[](https://sonery.medium.com/?source=post_page-----b67955211f81--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----b67955211f81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b67955211f81--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b67955211f81--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----b67955211f81--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b67955211f81--------------------------------) ·阅读时间 5 分钟·2023 年 11 月 23 日

--

![](img/5569fc0f11be8364c66846bfb0609968.png)

图片由 [Tom Bradley](https://unsplash.com/@tomrootstudio?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/brown-squirrel-on-green-grass-during-daytime-Z-ns9qAcvl4?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

第三方库是为了回应需求而创建和开发的。没有人会坐下来说：“我要创建一个工具，等待情况出现，让其他人需要它。”相反，他们会意识到问题并思考解决方案来解决它。这就是工具的创造方式。

添加新功能到现有工具也一样。新功能的添加速度和效果取决于工具的受欢迎程度和背后的团队。

确实，Pandas 拥有一个高度活跃的社区，这使得 Pandas 成为数据科学生态系统中最受欢迎的数据分析和清洗库之一。

Pandas 拥有解决非常具体问题和用例的函数。这些一定是社区中积极使用它的人提出的需求。

在本文中，我将分享 4 个可以用 Pandas 一行代码完成的操作。这些操作帮助我高效解决了特定任务，并让我感到惊喜。

## 1\. 从列表创建字典

我有一个项目列表，想查看它们的分布。更具体地说，我想查看列表中每个唯一值及其出现次数。

Python 字典是以这种格式存储数据的好方法。项目将是字典的键，而出现次数将是值。

通过 `value_counts` 和 `to_dict` 函数，这个任务可以用一行代码完成。

这是一个简单的示例来演示这个情况：

```py
import pandas as pd

grades = ["A", "A", "B", "B", "A", "C", "A", "B", "C", "A"]

pd.Series(grades).value_counts().to_dict()

# output
{'A': 5, 'B': 3, 'C': 2}
```

我们首先将列表转换为 Pandas Series，这是一维的数据结构。然后，我们应用 `value_counts` 函数以获取 Series 中唯一值及其频率。最后，我们将输出转换为字典。

## 2. 从 JSON 文件创建 DataFrame

JSON 是一种常用的文件格式，用于存储和传递数据。例如，当你从 API 请求数据时，它很可能会以 JSON 格式交付。

当我们清理、处理或分析数据时，通常更喜欢将其以表格格式（即表格状的数据结构）表示。我们可以利用 `json_normalize` 函数通过一次操作从 JSON 格式的对象创建 Pandas DataFrame。

假设数据存储在名为 `data.json` 的 JSON 文件中。我们首先按如下方式读取它：

```py
import json

with open("data.json") as f:
    data = json.load(f)

data
# output
{'data': [{'id': 101,
   'category': {'level_1': 'code design', 'level_2': 'method design'},
   'priority': 9},
  {'id': 102,
   'category': {'level_1': 'error handling', 'level_2': 'exception logging'},
   'priority': 8}]}
```

如果我们将这个变量传递给 DataFrame 构造函数，它将创建如下的 DataFrame，这绝对不是一个可用的格式：

```py
df = pd.DataFrame(data)
```

![](img/2ac39c06795fa69c508251ff2fba93f3.png)

df（图片由作者提供）

但如果我们使用 `json_normalize` 函数并提供记录路径，我们将获得一个格式良好且干净的 DataFrame：

```py
df = pd.json_normalize(data, "data")
```

![](img/f54adad98e37b03455dd5e31b8f388fc.png)

df（图片由作者提供）

## 3. Explode 函数

考虑一个你有符合特定记录的项列表的情况。你需要以每个项单独占一行的方式重新格式化它。

以下图示说明了我试图解释的内容：

![](img/347fc5df4fa22374320ceff798e524a4.png)

（图片由作者提供）

你可以考虑多种不同的方法来解决这个任务。最简单（可能是最简单）的一个方法是 `explode` 函数。让我们看看它是如何工作的。

我们有以下 DataFrame：

![](img/ae95d917871c615a345df60e29aed647.png)

df（图片由作者提供）

我们将使用 `explode` 函数，并指定要爆炸的列名：

```py
df_new = df.explode(column="data").reset_index(drop=True)
```

![](img/017148d21da9f19eea24b9e72176fb88.png)

df_new（图片由作者提供）

`reset_index` 将新的整数索引分配给结果 DataFrame。否则，爆炸之前的索引将被保留（即所有键值为 A 的行将具有索引 0）。

## 4. Combine first

`combine_first` 函数有一个特定的用途，但极大地简化了这个特定的任务。

使用 `combine_first` 函数的具体情况：

> 你想从一个 DataFrame 中提取一列。如果该列中存在缺失值，你希望用另一列中的值来替换这些缺失值。

在这方面，它的功能与 SQL 中的`COALESCE`函数相同。

让我们创建一个包含缺失值的示例 DataFrame：

```py
df = pd.DataFrame(
    {
        "A": [None, 0, 12, 5, None], 
        "B": [3, 4, 1, None, 11]
    }
)
```

![](img/f0ad46e4f43caf9569dc6a1ab205f110.png)

df（图片由作者提供）

我们需要列 A 中的数据。如果有一行缺失值（即 NaN），我们希望用同一行中列 B 的值填充它。

```py
df["A"].combine_first(df["B"])

# output
0     3.0
1     0.0
2    12.0
3     5.0
4    11.0
Name: A, dtype: float64
```

正如我们在输出中看到的，列 A 的第一行和最后一行是从列 B 中取出的。

如果我们想使用 3 列，我们可以链式调用 `combine_first` 函数。以下代码首先检查 A 列。如果有缺失值，则从 B 列获取。如果 B 列中对应的行也为 NaN，则从 C 列获取值。

```py
df["A"].combine_first(df["B"]).combine_first(df["C"])
```

我们还可以在 DataFrame 层级上使用 `combine_first` 函数。在这种情况下，所有缺失值都将从第二个 DataFrame 中相应的值（即同一行、同一列）填充。

## 最后的话

Pandas 是我用过的最万能的工具之一。从计算简单的统计数据到非常复杂的数据清理过程，Pandas 总是帮助我快速解决任务。我唯一遇到的问题是处理非常大的数据集时，这似乎是 Pandas 唯一的短板。然而，最近有一些进展使 Pandas 在处理大数据集时更高效。我相信这是对所有热爱使用这个伟大工具的人们的好消息。

感谢阅读。如果您有任何反馈，请告知我。
