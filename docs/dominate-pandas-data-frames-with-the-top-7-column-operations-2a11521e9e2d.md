# 使用 Pandas 数据框更有效地进行 7 种顶级列操作

> 原文：[`towardsdatascience.com/dominate-pandas-data-frames-with-the-top-7-column-operations-2a11521e9e2d`](https://towardsdatascience.com/dominate-pandas-data-frames-with-the-top-7-column-operations-2a11521e9e2d)

## 完整指南，介绍以不同方式执行 7 种顶级 Pandas 列操作

[](https://federicotrotta.medium.com/?source=post_page-----2a11521e9e2d--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----2a11521e9e2d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a11521e9e2d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a11521e9e2d--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----2a11521e9e2d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a11521e9e2d--------------------------------) ·阅读时间 15 分钟·2023 年 7 月 6 日

--

![](img/4a4f32645b84448222d2511ae309f680.png)

图片由 [Alan](https://pixabay.com/it/users/ad_images-6663717/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2987596) 提供，来源于 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2987596)

在数据分析方面，[Pandas](https://pandas.pydata.org/) 是最常用的 Python 库，用于操控和准备数据以进行进一步的分析和机器学习。

现实是 Pandas 是一个非常灵活的库，甚至可以用来转换文件格式。

然而，即使我们几乎每天使用一些 Pandas 功能，我们仍然花费大量时间 Googling 如何在 Pandas 中做某些事情。

我知道，我抓住你了！

但说实话：一些功能很难记住，也许是因为我们可以用不同的方法达到相同的目标。所以，每天 Google 同样的内容也没有什么可羞愧的。

然而，节省时间始终是个好主意。为此，在本文中，我们将探讨操控 Pandas 列的 7 个顶级功能。这样你就不需要再去 Google 查找了：只需保存这篇文章（也许通过收藏它）并在需要时返回查看即可。

这是你会在这里找到的内容：

```py
**Table of contents:** 
How to create a new Pandas column
How to add a new column to a Pandas data frame
How to rename a column in Pandas
How to drop a Pandas column
How to find unique values in a Pandas column
How to transform a Pandas column into a list
How to sort a Pandas data frame for a column
```

# 如何创建一个新的 Pandas 列

首先，让我们记住 Pandas 列也被称为 [Pandas 系列](https://pandas.pydata.org/docs/reference/api/pandas.Series.html)。这意味着 Pandas 数据框是有序的 Pandas 系列集合。

有几种方法可以创建新的 Pandas 列。我们来看看它们吧。

## 创建一个 Pandas 列作为 Pandas 系列

创建一个被认为“独立存在”的 Pandas 列的正确方法是通过 Pandas 系列方法，如下所示：

```py
# Create a Panad series
series = pd.Series([6, 12, 18, 24])

# Print Pandas series
print(series)

>>>

  0     6
  1    12
  2    18
  3    24
  dtype: int64
```

我说“正确的方法”是因为，正如我们所说，Pandas 列是 Pandas 系列。因此，如果我们只需要单列，我们应该使用这种方法，如果我们想要“**正式**正确”。

## 创建一个 Pandas 列作为 Pandas 数据框

然而，现实是我们不会经常需要一个单独的列。

所以，创建 Pandas 列的另一种方法是通过创建一个只有一列的新 Pandas 数据框：这样，我们可以在第二步中用其他列来丰富它。

我们可以这样做：

```py
import pandas as pd

# Create a Pandas column as a Pandas data frame
df = pd.DataFrame({'A': [1, 2, 3, 4]})

# Print Pandas data frame
print(df)

>>>

     A
  0  1
  1  2
  2  3
  3  4
```

所以，这里与之前的例子不同的是，这次 Pandas 列也有名字。在这种情况下，我们将其命名为“A”。

```py
**NOTE:**

If we take a look more closely to what we've done here, we can see that
we can create a Pandas data frame as a dictionary.

In fact, "A" is the key and it's separated by a list of values
by a colon. Then, both the keys and the values are inside curly braces.
```

## 从 NumPy 数组开始创建 Pandas 列作为 Pandas 数据框

Pandas 的超级能力之一是它可以“接受” NumPy 数组作为输入值。换句话说，我们可以从 NumPy 数组创建一个数据框。

对于单列的情况，我们可以创建一个一维数组并将其转换为数据框：这将得到一个只有一列的数据框。

我们可以这样做：

```py
import numpy as np
import pandas as pd

# Create a NumPy array
values = np.array([5, 10, 15, 20])

# Transform array into Pandas data frame
df = pd.DataFrame(values)

# Print data frame
print(df)

>>>

    0
0   5
1  10
2  15
3  20
```

# 如何向 Pandas 数据框添加新列

将新列添加到 Pandas 数据框的可能性与创建新列的过程有某种程度上的关联。

我的意思是，我们首先需要创建一个 Pandas 数据框，然后是一个单独的 Pandas 列，最后需要将该列添加到数据框中。

在这种情况下，我们也有多种可能的实现方法。让我们全部查看一下。

## 向 Pandas 数据框添加新列：标准方法

向 Pandas 数据框添加新列的标准方法是先创建数据框，然后创建一个单独的列，最后将其添加到数据框中。

我们将在接下来的所有示例中使用这种方法。所以，我们可以这样做：

```py
import pandas as pd

# Create a DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 4]})

# Add a new column by using a list of values
df['B'] = [20, 30, 40, 50]

# Print data frame
print(df)

>>>

   A   B
0  1  20
1  2  30
2  3  40
3  4  50
```

那么，让我们一步一步分析我们所做的：

1.  我们使用 `pd.DataFrame()` 方法创建了一个 Pandas 数据框。

1.  我们用 `df['B']` 创建了一个新列，这意味着我们将这个新列命名为“B”。

1.  我们已经将值分配给新创建的列，使用了一个数字列表。

那么，创建新列的另一种方法是什么？如果我们已经有一个数据框，可以使用**数字列表**。

## 向 Pandas 数据框添加新列：应用函数

将新列添加到现有数据框的标准方法使我们能够创建一个新列并将其添加到现有数据框中，所有这些都在一行代码中完成。

例如，假设我们想创建两个新列作为现有列的组合。我们可以通过将函数应用于现有列来做到这一点，如下所示：

```py
import pandas as pd

# Create a DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 4]})

# Create a column doubling the values of column A
df['B'] = df['A'] * 2

# Apply lambda function to column A to create column C
df['C'] = df['A'].apply(lambda x: x ** 2)

# Print data frame
print(df)

>>>

   A  B   C
0  1  2   1
1  2  4   4
2  3  6   9
3  4  8  16
```

所以，这里是我们所做的：

1.  我们已经创建了一个 Pandas 列（“A”）作为一个数据框。

1.  我们通过将列“A”的值翻倍来创建了列“B”。

1.  我们通过将 lambda 函数应用于列“A”创建了列“C”。具体来说，在这种情况下，我们是在对列“A”的值进行平方运算。

所有这些列都一起存储在一个唯一的数据框中。

## 向 Pandas 数据框添加新列：使用 Pandas 系列或单个 Pandas 列

当然，我们可以向 Pandas 数据框中添加列，即使这些列是 Pandas 系列或 Pandas 数据框。

下面是我们可以做到的：

```py
import pandas as pd

# Create a DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 4]})

# Create a new column using pd.Series()
values = pd.Series([5, 10, 15, 20]) # Create series
df['B'] = values # Add series to data frame as a column

# Print data frame
print(df)

>>>

   A   B
0  1   5
1  2  10
2  3  15
3  4  20
```

所以，在上面的例子中，我们创建了一个 Pandas 系列，然后通过给它命名将其添加到现有的数据框中。

在作为 Pandas 数据框创建的 Pandas 列的情况下，我们有：

```py
import pandas as pd

# Create a DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 4], 'B': [5, 6, 7, 8]})

# Create a Pandas column as a data frame
df['C'] = pd.DataFrame({'C': [9, 10, 11, 12]})

# Print data frame
print(df)

>>>

   A  B   C
0  1  5   9
1  2  6  10
2  3  7  11
3  4  8  12
```

就这样。

```py
**NOTE**:

of course, the same methodology can be applied if we create a column
as a NumPy array. We won't show the method here as "the game" should
now be clear.
```

# 如何重命名 Pandas 列

重命名 Pandas 列（或多个列）是我们需要执行的另一个典型的日常任务，但我们常常记不住。

同样，在这种情况下，我们有不同的方法来做到这一点。让我们看看它们。

## 如何重命名 Pandas 列：`rename()`方法

我们可以使用`rename()`方法来重命名 Pandas 列，如下所示：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})

# Renaming a single column
df = df.rename(columns={'A': 'NewA'})

# Print data frame
print(df)

>>>
     NewA  B
0     1    4
1     2    5
2     3    6 
```

所以，这就像我们在使用字典。在`rename()`方法中，实际上，我们需要传递参数`columns`，并在大括号中指定实际名称和新名称，用冒号分隔它们。就像我们在字典中做的那样。

当然，我们可以使用这种方法重命名多个列，如下所示：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})

# Rename multiple columns
df = df.rename(columns={'A': 'NewA', 'B': 'NewB'})

# Print data frame
print(df)

>>>

    NewA  NewB
0     1     4
1     2     5
2     3     6
```

再次，这就像我们在使用字典一样。

## 如何重命名 Pandas 列：`column`属性

要重命名一个 Pandas 列（或多个列，如我们将看到的），我们可以使用`columns`属性，如下所示：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})

# Renaming all columns
df.columns = ['NewA', 'NewB']

# Print data frame
print(df)

>>>

    NewA  NewB
0     1     4
1     2     5
2     3     6
```

所以，在这种情况下，`columns`属性让我们可以使用字符串列表来重命名列。

## 如何重命名 Pandas 列：`set_axis()`方法

要重命名一个（或多个）Pandas 列，我们可以使用`set_axis()`方法，如下所示：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6]})

# Renaming all columns
df.set_axis(['NewA', 'NewB'], axis=1, inplace=True)

# Print data frame
print(df)

>>>

     NewA  NewB
0     1     4
1     2     5
2     3     6
```

所以，即使在这种情况下，我们也使用字符串列表来重命名列，但这里我们还需要传递参数`axis=1`和`inplace=True`，因为`set_axis()`方法是从零开始设置轴，因此它会重新创建它们。这使得这个方法可以重命名列。

## 如何重命名 Pandas 列：使用 lambda 函数

当我们处理字符串（如 Pandas 列名称的情况）时，我们可以使用 lambda 函数来修改文本中的字符。

例如，我们可能希望（或需要）通过简单地将字母转为小写来重命名列。我们可以这样做：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'COLUMN_1': [1, 2, 3], 'COLUMN_2': [4, 5, 6]})

# Renaming columns using a lambda function
df = df.rename(columns=lambda x: x.lower())  # Lowercase column names

# Print data frame
print(df)

>>>

      column_1  column_2
0         1         4
1         2         5
2         3         6
```

就这样。

# 如何删除 Pandas 列

删除 Pandas 列（或多个列）是我们经常需要执行的另一个任务。也许是因为它的值不重要，也许是因为它的值都是`NULL`或其他原因。

要执行这个任务，我们有两种方法。让我们看看它们。

## 如何删除 Pandas 列：使用 drop()方法

删除 Pandas 列（或多个列）的典型方法是使用`drop()`方法。

在这里，唯一需要记住的是决定是否要删除某些列并创建一个新的数据框，或者是否要删除这些列并替换当前的数据框。

让我展示一下区别：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6], 'C': [7, 8, 9]})

# Drop one column and substitute the current data frame
df = df.drop('A', axis=1)

# Print updated data frame
print(df)

>>>

   B  C
0  4  7
1  5  8
2  6  9
```

所以，我们使用 `drop()` 方法删除了“ A”列，指定了要删除的列的名称和轴（`axis=1` 在 Pandas 中表示垂直方向，必须指定）。

在这种情况下，我们决定替换数据框 `df`。因此，在过程结束时，数据框 `df` 中没有“ A”列。

相反，如果我们想创建另一个数据框，假设我们称之为 `df_2`，我们必须这样做：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6], 'C': [7, 8, 9],
      'D': [10, 11, 12]})

# Drop one column and substitute the current data frame
df_2 = df.drop(['A', 'D'], axis=1)

# Print new data frame
print(df_2)

>>>

   B  C
0  4  7
1  5  8
2  6  9
```

所以，在这种情况下，我们删除了两列，并创建了一个新的数据框，仅包含“B”和“C”列。

如果我们认为未来可能需要原始数据框 `df` 以进行进一步分析，这可能会很有用。

## 如何删除 Pandas 列：使用列索引

在 Pandas 中，列可以通过索引单独提取。这意味着我们可以像这样使用索引删除它们：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3], 'B': [4, 5, 6], 'C': [7, 8, 9],
      'D': [10, 11, 12]})

# Drop one column and append to a new data frame
df_2 = df.drop(df.columns[[0, 1]], axis=1)

# Print new data frame
print(df_2)

>>>

   C    D
0  7   10
1  8   11
2  9   12
```

所以，在这种情况下，我们创建了一个新的数据框，仅包含“ C”和“ D”列，并通过使用它们的索引删除了“ A”和“ B”列。

记住在 Python 中我们从 0 开始计数（所以，第一列的索引是 0，列为“ A”），我们必须说明，如果我们有数十列，这种方法可能不是最优的，原因很简单：我们应该通过计数找到要删除的那一列（或那些列），这容易出错。

# 如何在 Pandas 列中查找唯一值

在 Pandas 列中查找唯一值是我们可能需要每天执行的另一项任务，因为重复值必须以特定方式处理。

在这种情况下，我们有几种方法可以做到这一点：一种是显示一列中的重复项，另一种是删除它们。

让我们看看这两种方法。

## 如何在 Pandas 列中查找唯一值：使用 value_counts() 方法查找重复项

如果我们想查看 Pandas 列是否有重复值，并且我们还想知道它们有多少个，可以像这样使用 `value_counts()`：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 1, 3], 'B': [4, 5, 6, 7, 8,],
    'C': [7, 8, 9, 10, 11]})

# Find unique values in a Pandas column
unique_values = df['A'].value_counts()

# Print unique values
print(unique_values)

>>>

1    2
3    2
2    1
Name: A, dtype: int64
```

所以，这里的结果告诉我们：

1.  列的名称是“ A”，所有类型都是“ int64”。

1.  我们有两个 1。

1.  我们有两个 3。

1.  我们有一个 2。

所以，它向我们展示了这些值，并告诉我们它们在我们关注的列中出现了多少次。

## 如何在 Pandas 列中查找唯一值：使用 drop_duplicates() 方法删除重复项

如果我们想删除 Pandas 列中的重复值（因为我们知道其中有重复项），可以像这样使用 `drop_duplicates()` 方法：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 1, 3], 'B': [4, 5, 6, 7, 8,],
       'C': [7, 8, 9, 10, 11]})

# Drop the duplicate values in a Pandas column
unique_values = df['A'].drop_duplicates()

# Print unique values
print(unique_values)

>>>

0    1
1    2
2    3
```

所以，我们从“ A”列中删除了重复项，创建了一个名为 `unique_values` 的新 Pandas 列。

## 如何在 Pandas 列中查找唯一值：研究数据框

在这一点上，你可能会问：“ *那么，如果我有一个包含数十列的大数据框，我如何知道哪些列确实有重复项*？”

好问题！我们可以做的是首先研究整个数据框。

例如，我们可能想查看是否有任何列有重复项。我们可以这样做：

```py
import pandas as pd

# Creating a DataFrame with duplicates
df = pd.DataFrame({'A': [1, 2, 2, 3, 4, 4], 'B': [5, 6, 6, 7, 8, 8]})

# Check if there are duplicates in the DataFrame
has_duplicates = df.duplicated().any()

# Print the result
print(has_duplicates)

>>>

True
```

所以，这段代码会返回“True”如果存在重复列，返回“False”如果不存在。

那如果我们想知道哪些列实际上有重复项呢？我们可以像这样做：

```py
import pandas as pd

# Creating a DataFrame with duplicates
df = pd.DataFrame({'A': [1, 2, 2, 3, 4, 4], 'B': [5, 6, 6, 7, 8, 8]})

# Find duplicate rows
duplicate_rows = df.duplicated()

# Print the duplicate rows
print(df[duplicate_rows])

>>>

   A  B
2  2  6
5  4  8
```

所以，上面的代码显示了：

+   包含重复项的列。

+   重复项的值。

现在我们可以进一步调查 `value_counts()` 方法或使用 `drop_duplicates()` 方法删除重复项。

# 如何将 Pandas 列转换为列表

将 Pandas 列转换为列表是一个有用的特性，它可以让我们“隔离”所有来自 Pandas 列的值，将其放入列表中。然后，我们可以对列表进行任何需要的操作，因为列表很容易管理（如迭代等）。

我们有两种方法可以进行这个转换。

## 如何将 Pandas 列转换为列表：使用 list() 方法

`list()` 方法是一个内置的 Python 函数，用于将可迭代对象转换为列表。我们可以像这样使用它：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 1, 3], 'B': [4, 5, 6, 7, 8,],
    'C': [7, 8, 9, 10, 11]})

# Transform Pandas column into a list
column_list = list(df['B'])

# Print list
print(column_list)

>>>

[4, 5, 6, 7, 8]
```

所以，我们已经轻松地提取了我们的值并将其放入列表中。

## 如何将 Pandas 列转换为列表：使用 to_list() 方法

要实现相同的结果，我们可以使用 Pandas 的 `to_list()` 方法。但请注意：**它在 Pandas 版本 1.2.0 或更高版本中可用**。

我们可以像这样使用它：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 1, 3], 'B': [4, 5, 6, 7, 8,],
      'C': [7, 8, 9, 10, 11]})

# Transform Pandas column into a list
column_list = df['B'].to_list()

# Print list
print(column_list)

>>>

[4, 5, 6, 7, 8]
```

当然，我们得到了与之前相同的结果。

# 如何按列对 Pandas 数据框进行排序

有很多情况需要对列进行排序。排序意味着排序，因此我们可以选择按升序或降序排列数据。

我们可以通过以下方法实现这个目标。

## 如何按列对 Pandas 数据框进行排序：使用 sort_values() 方法

要按列对 Pandas 数据框进行排序，我们可以像这样使用 `sort_values()`：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [10, 2, 7, 1, 15], 'B': [4, 2, 6, 28, 8,],
    'C': [7, 1, 9, 10, 19]})

# Sort df for A in ascending order
df.sort_values('A', ascending=True, inplace=True)

# Print sorted data frame
print(df)

>>>

    A   B   C
3   1  28  10
1   2   2   1
2   7   6   9
0  10   4   7
4  15   8  19
```

所以，我们可以看到，数据框已按列“A”升序排序。实际上，如果我们检查：

+   在初始数据框中，列“A”中的数字 1 位于第 4 个位置。在列“B”中，数字 28 位于第四个位置。

+   在排序后的数据框中，列“A”中的数字 1 位于第一个位置。在列“B”中，数字 28 位于第一个位置。

因此，我们对数据框进行排序，但不会丢失列值之间的关系。

这个方法的一个非常有用的特性是它可以通过将 `NaNs` 作为第一个值来排序列。我们可以像这样做：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [10, 0, 0, 1, 15], 'B': [4, 2, 6, 28, 8,],
    'C': [7, 1, 15, 10, 19]})

# Sort NaNs in the beginning
df.sort_values('A', ascending=True, inplace=True)

# Print sorted data frame
print(df)

>>>

    A   B   C
1   0   2   1
2   0   6  15
3   1  28  10
0  10   4   7
4  15   8  19
```

我们到了这里。

## 如何按列对 Pandas 数据框进行排序：使用 sort_index() 方法

我们还可以按如下方式对数据框进行排序，以根据索引值排序：

```py
import pandas as pd

# Creating a DataFrame
df = pd.DataFrame({'A': [10, 2, 7, 1, 15], 'B': [4, 2, 6, 28, 8,],
    'C': [7, 1, 9, 10, 19]})

# Sort data frame for index
df.sort_index(inplace=True)

# Print sorted data frame
print(df)

>>>

    A   B   C
0  10   4   7
1   2   2   1
2   7   6   9
3   1  28  10
4  15   8  19
```

正如我们所见，索引是有序的（按升序排列）。

# 结论

在这篇文章中，我们已经看到了在 Pandas 列上执行的 7 种主要操作，这些操作我们几乎每天都会执行。

如果你保存这份指南，它将帮助你节省很多时间，因为我们已经以不同的方式执行了相同的任务，这样你就不需要再谷歌搜索它们了，节省了很多时间。

![](img/5079e3af9eda458328cb258c452fb935.png)

费德里科·特罗塔

你好，我是费德里科·特罗塔，我是一名自由职业技术作家。

想与我合作吗？ [联系我](https://bio.link/federicotrotta)。
