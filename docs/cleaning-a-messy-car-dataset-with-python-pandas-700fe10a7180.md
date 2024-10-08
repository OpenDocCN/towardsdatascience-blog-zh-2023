# 使用 Python Pandas 清理混乱的汽车数据集

> 原文：[`towardsdatascience.com/cleaning-a-messy-car-dataset-with-python-pandas-700fe10a7180`](https://towardsdatascience.com/cleaning-a-messy-car-dataset-with-python-pandas-700fe10a7180)

## 无论你是进行探索性数据分析还是构建复杂的机器学习系统，你都需要确保数据已被清理。

[](https://sonery.medium.com/?source=post_page-----700fe10a7180--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----700fe10a7180--------------------------------)[](https://towardsdatascience.com/?source=post_page-----700fe10a7180--------------------------------)![数据科学之路](https://towardsdatascience.com/?source=post_page-----700fe10a7180--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----700fe10a7180--------------------------------)

·发表于[数据科学之路](https://towardsdatascience.com/?source=post_page-----700fe10a7180--------------------------------) ·阅读时间 7 分钟·2023 年 10 月 17 日

--

![](img/4d84b2673b18c91810618f7530f6b299.png)

（图像由作者使用 Midjourney 创建）

网络是一个高度有价值的数据来源。例如，用于创建大型语言模型的大量训练数据来自网络。

然而，它通常不在最适合的格式中。网络数据主要是非结构化的（即以自由文本的形式存在）。即使它有预定义的结构，网络数据在用于分析目的之前也需要大量的清理和预处理。

在这篇文章中，我们将使用 pandas 库清理一个包含汽车价格及其他属性的混乱数据集。

如果你想跟随并自行执行代码，可以从我的[数据集](https://github.com/SonerYldrm/datasets)仓库下载数据集。它叫做“mock_car_dataset”。我们将在这个混乱的数据集上执行的一些操作如下：

+   字符串操作

+   处理数据类型

+   基于字符串的过滤

+   替换值

+   使用其他列更新列值

+   格式化数值数据

+   分析数据以检测问题

我用模拟数据创建了这个数据集。然而，它就像是你从网络抓取的汽车数据集。我知道这一点，因为我以前做过。

数据集采用 CSV 格式。让我们从这个文件创建一个 pandas DataFrame 开始。

```py
import pandas as pd

# create DataFrame
cars = pd.read_csv("mock_car_dataset.csv")

# check the shape
cars.shape 
# output
(20, 6)
```

数据集包含 20 行和 6 列，这意味着我们有 20 辆车的 6 个属性的数据。虽然这是一个小数据集，但我们将执行的操作可以很容易地应用于更大的数据集（即数十万行）。

让我们看看这些属性是什么（`cars.head()`将显示以下内容）：

![](img/0d518112902c44fce10b8885606016d4.png)

汽车 DataFrame 的前 5 行（图像由作者提供）

下一步是检查数据类型。我们可以使用 `dtypes` 方法来完成：

```py
cars.dtypes

# output
price        object
year          int64
make         object
model        object
cylinders    object
odometer      int64
dtype: object
```

这些列中的一些确实应该转换为数字数据类型，但在当前格式下无法做到。例如，不能直接将“$11,250”转换为整数值 11250。同样，“6 cylinders”值需要一些调整才能转换为 6。

Pandas 是一个非常多功能的字符串处理工具。看似复杂的操作可以在一行代码中完成。

例如，要将价格值转换为整数数据类型，我们首先需要从价格值中删除“$”和“,”，然后应用 `astype` 函数来更改数据类型。假设所有价格值的第一个字符是“$”，我们可以如下进行操作：

```py
cars["price"] = cars["price"].str[1:].str.replace(",", "").astype("int")
```

`str[1:]` 选择从第二个字符开始的所有字符，这意味着删除“$”。然后我们用空字符串替换“,”，最后更改数据类型。但首先，让我们确保所有价格值的第一个字符是“$”。

```py
# select only the first characters in the price column and check unique values
cars.price.str[0].unique()

# output
array(['$'], dtype=object)
```

现在我们可以使用以下代码行来修复价格列：

```py
cars["price"] = cars["price"].str[1:].str.replace(",", "").astype("int")
```

下一个要处理的是 cylinders 列。让我们看看值的分布：

```py
cars["cylinders"].value_counts()

# output
6 cylinders    9
8 cylinders    5
4 cylinders    4
other          2
Name: cylinders, dtype: int64
```

`value_counts` 函数显示列中的唯一值及其出现次数。我们需要的是从值中删除“ cylinders”，并用我们选择的值替换“other”。我倾向于用最频繁的值替换“other”，即 6。

最后，我们可以将该列的数据类型更改为整数。以下代码行完成了所有这些操作：

```py
cars["cylinders"] = cars["cylinders"].str.replace(" cylinders", "").replace("other", 8)

# check the value distribution again
cars["cylinders"].value_counts()

# output
6    9
8    5
4    4
6    2
Name: cylinders, dtype: int64
```

在上面的代码片段的第一行中，你可能会注意到我们首先使用了 `str.replace`，然后使用了 `replace`。

+   `str.replace` 用于替换字符串中的一系列字符

+   `replace` 用于列中的整个值

让我们看看 DataFrame 现在的样子：

![](img/da88afd88c25f0c16157469af21f68c1.png)

车辆 DataFrame 的前 5 行 (图片由作者提供)

`make` 列包含大写和小写字母，这可能导致相同的值被视为不同。这里有一个快速测试来确认这一点：

```py
cars[cars["make"].str.contains("onda")]["make"].value_counts()

# output
honda    1
Honda    1
Name: make, dtype: int64
```

“Honda”和“honda”是相同的品牌，但它们不是相同的字符串。我们可以通过将所有字符转换为小写或大写来快速解决这个问题。

```py
cars["make"] = cars["make"].str.lower()

# check the first 5 values in the make column
cars["make"][:5]

# output
0      nissan
1    chrysler
2     porsche
3       honda
4     lincoln
Name: make, dtype: object
```

## 更具体一点

当我检查“make”列中的唯一值时，我看到像 2007 和 2014 这样的值，它们不代表任何品牌（至少我所知）。这一定是因为在自由形式中错误的数据输入，这是从网络抓取数据时的常见问题。

让我们检查这些意外品牌的年份、制造商和型号值。

```py
cars[cars["make"].isin(["2007", "2014"])]
```

`isin` 方法允许基于多个值过滤行。此行代码的输出是：

![](img/57284056ccb1f7c85e3334416a5770fa.png)

(图片由作者提供)

问题似乎是年份值被输入了两次，这导致品牌（即`make`）值被切换到模型旁边。解决这个问题有多种不同的替代方案。

我想到的一个方法是拆分`model`值，并将拆分后的第一部分作为`make`列中值的替代，如下所示：

![](img/3da39fed45390e6da76a69bfd4290742.png)

(image by author)

以下代码行拆分了`make`值为 2007 的行的`model`列。

```py
cars.loc[cars["make"].isin(["2007"]), "model"].str.split(" ", expand=True)
```

![](img/15ef0dd5e6360867c7c144489372db39.png)

(image by author)

看起来有点奇怪。原因是一些值在开头包含空格字符，这会导致拆分后有 3 个项目。对于那些开头没有空格字符的值，最后一个项目变成了 None。我们可以通过使用`strip`方法来解决这个问题，该方法可以去除前后的空白字符。

```py
cars.loc[cars["make"].isin(["2007"]), "model"].str.strip().str.split(" ", expand=True)
```

![](img/4e6b383853425bf6fe963da3edf999b4.png)

(image by author)

现在它是准确的，看来也好多了。下一步是将这段代码应用于所有`make`列中有年份值的行。

要找到需要替换的`make`值（即`make`列中的年份），你可以执行以下代码行：

```py
pd.Series(cars["make"].unique()).sort_values()
```

它找到`make`列中的唯一值并对其进行排序，这将年份值放在开头。输出的前 5 行如下：

![](img/d899ef962db77a6abd3f55ed0dc59e98.png)

(image by author)

我们现在可以创建一个包含 2007 和 2014 的列表，然后用它来过滤需要替换的`make`列中的行。以下代码片段执行了我们需要的任务：

```py
to_replace = ["2007", "2014"]

cars.loc[cars["make"].isin(to_replace), "make"] = \
cars.loc[cars["make"].isin(to_replace), "model"].str.strip().str.split(" ", expand=True)[0]
```

让我们确认一下`make`列中是否有任何年份值。

```py
pd.Series(cars["make"].unique()).sort_values()
```

![](img/3d5d8d9935e2a96ca3a85d9483841e16.png)

(image by author)

`make`列中的所有年份值都已消失。

# 结语

最后但同样重要的是，DataFrame 中有许多重复的行。我们可以使用`drop_duplicates`函数来消除它们。默认情况下，它会删除所有列值相同的行。如果你想基于一列或一组列中的值来确定重复项，你可以使用`subset`参数。

数据清洗是任何数据工作流程中的重要部分。无论你是在进行探索性数据分析还是构建复杂的机器学习系统，你都需要确保数据是干净的。否则，你所产生的结果可能是不可靠的。

感谢阅读。如有任何反馈，请告诉我。
