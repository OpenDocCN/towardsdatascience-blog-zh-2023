# 如何在 5 个简单示例中将 SQL 查询重写和优化为 Pandas

> 原文：[`towardsdatascience.com/how-to-rewrite-and-optimize-your-sql-queries-to-pandas-in-5-simple-examples-6983cae8426e`](https://towardsdatascience.com/how-to-rewrite-and-optimize-your-sql-queries-to-pandas-in-5-simple-examples-6983cae8426e)

## 编程

## 从 SQL 过渡到 Pandas 以改善你的数据分析工作流程

[](https://byrondolon.medium.com/?source=post_page-----6983cae8426e--------------------------------)![Byron Dolon](https://byrondolon.medium.com/?source=post_page-----6983cae8426e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6983cae8426e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6983cae8426e--------------------------------) [Byron Dolon](https://byrondolon.medium.com/?source=post_page-----6983cae8426e--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6983cae8426e--------------------------------) ·9 分钟阅读·2023 年 6 月 1 日

--

![](img/802af72a42f8761f0a4ee55bcb3d5954.png)

作者提供的图片

数据分析师、工程师和科学家都熟悉 SQL。该查询语言仍广泛用于处理任何类型的关系数据库。

然而，现在越来越多的情况，尤其是对于数据分析师，技术要求在上升，人们预计至少要了解一种编程语言的基础知识。在处理数据时，Python 和 Pandas 特别是常见的工作要求之一。

尽管 Pandas 对于熟悉 SQL 的数据人员来说可能是新的，但在 SQL 中选择、过滤和聚合数据的概念可以很容易地转移到 Pandas 中。在这篇文章中，让我们看看一些常见的 SQL 查询以及如何在 Pandas 中编写和优化它们。

随意在你自己的笔记本或 IDE 中跟随操作。你可以从 Kaggle [这里](https://www.kaggle.com/datasets/deepanshuverma0154/sales-dataset-of-ecommerce-electronic-products?resource=download) 下载数据集，该数据集在 CC0 1.0 Universal (CC0 1.0) 公共领域献身许可证下免费使用。

只需导入并运行以下代码，开始吧！

```py
import pandas as pd
from functools import reduce

df = pd.read_csv("/Users/byrondolon/Desktop/Updated_sales.csv")
df.columns = [i.replace(" ", "_") for i in df.columns]py
```

# SQL 的简单示例及其 Pandas 等效方法

## 查询整个表

我们可以直接开始，看看经典的 SELECT ALL from 一个表。

这是 SQL:

```py
SELECT * FROM df
```

这是 Pandas 代码

```py
df
```

![](img/b7ed16647269553597becebeb6b3a5a5.png)

Pandas 代码输出 — 作者提供的图片

你需要做的就是调用 Pandas 中的 DataFrame 来返回整个表及其所有列。

你可能还希望在编写更复杂的查询之前快速检查表中的一个小子集。在 SQL 中，你可以使用 `LIMIT 10` 或类似的方式仅获取选定数量的行。在 Pandas 中，你可以调用 `df.head(10)` 或 `df.tail(10)` 来获取表的前 10 行或最后 10 行。

## 查询没有空值的表

要在最初的选择查询中添加条件，除了限制行数之外，你还可以在 SQL 的 WHERE 子句中放入过滤表的条件。例如，如果你想要所有 `Order_ID` 列中没有任何空值的行，SQL 查询将如下所示：

```py
SELECT * FROM df WHERE Order_ID IS NOT NULL
```

在 Pandas 中，你有两个选项：

```py
# Option 1
df.dropna(subset="Order_ID")

# Option 2
df.loc[df["Order_ID"].notna()]
```

![](img/8e24f10217698d40c468436f73d038a3.png)

Pandas 代码输出 — 作者提供的图片

现在，我们得到的表没有 `Order_ID` 列中的任何空值（你可以与上面的第一个输出进行比较）。这两种方法都会返回没有空值的表，但它们的工作方式略有不同。

你可以使用 Pandas 中的 `dropna` 方法返回没有任何空行的 DataFrame，指定 `subset` 参数中要从中删除空值的列。

另外，`loc` 方法允许你传递一个掩码或布尔标签来过滤 DataFrame。在这里，我们传递 `df["Order_ID"].notna()`，如果你单独调用它，将返回一个 True 和 False 值的系列，这些值可以映射到原始 DataFrame 行，以确定 `Order_ID` 是否为空。当我们将它传递给 `loc` 方法时，它会返回 `df["Order_ID"].notna()` 评估为 True 的 DataFrame（即所有 `Order_ID` 列不是空的行）。

## 查询表中的特定列

接下来，我们将选择表中的几个特定列，而不是选择所有列。在 SQL 中，你可以在查询的 SELECT 部分中写入列名，如下所示：

```py
SELECT Order_ID, Product, Quantity_Ordered FROM df
```

在 Pandas 中，我们将代码写成这样：

```py
df[["Order_ID", "Product", "Quantity_Ordered"]]
```

![](img/102a928216a9a5b874dda7b8030104cc.png)

Pandas 代码输出 — 作者提供的图片

要选择特定的列子集，你可以将列名列表传递给 Pandas 中的 DataFrame。你也可以像这样单独定义列表以便清晰：

```py
target_cols = ["Order_ID", "Product", "Quantity_Ordered"]
df[target_cols]
```

分配一个目标列列表，然后将其传递到 DataFrame 中，可以使在需要对代码进行更改时处理表变得更加容易。例如，你可以让一个函数返回你需要的列作为列表，或者根据用户需要的输出来追加和删除列表中的列。

## SQL 和 Pandas 中的 GROUP BY

现在我们可以开始聚合数据。在 SQL 中，我们通过将要分组的列传递给 SELECT 和 GROUP BY 子句来实现这一点，然后在 SELECT 子句中将该列添加到像 COUNT 这样的聚合度量中。例如，这样做将允许我们对原始表中每个 `Product` 的所有单独的 `Order_ID` 行进行分组，并计算它们的数量。查询可能如下所示：

```py
SELECT 
    Product, 
    COUNT(Order_ID)
FROM df
WHERE Order_ID IS NOT NULL
GROUP BY Product
```

在 Pandas 中，它会像这样显示：

```py
df[df["Order_ID"].notna()].groupby(["Product"])["Order_ID"].count()
```

![](img/241f0b75bb7b93365d2e4f48e1e62239.png)

Pandas 代码输出 — 图片由作者提供

输出是一个 Pandas Series，其中表格按产品进行分组，并显示每个产品的`Order_ID`计数。除了我们之前在 Pandas 中包含过滤条件的查询外，我们现在要做三件事：

1.  添加`groupby`并传递你想要对 DataFrame 进行分组的列（或列列表）；

1.  在原始分组的 DataFrame 上用方括号传递列名；

1.  调用`count`（或任何其他聚合）方法对 DataFrame 中的目标列进行聚合。

为了更好地阅读，我们可以将条件赋值给一个变量（这在后面会派上用场），并格式化查询，使其更易读。

```py
condition = df["Order_ID"].notna()
grouped_df = (
    df.loc[condition]
    .groupby("Product")
    ["Order_ID"]  # select column to count
    .count()
)
grouped_df
```

# 一个完整的 SQL 查询在 Pandas 中翻译并高效编写

现在我们已经具备了完整 SQL 查询的大部分组件，让我们看看一个更复杂的查询在 Pandas 中的样子。

```py
SELECT 
    Product,
    COUNT(Order_ID)
FROM df
WHERE Order_ID IS NOT NULL
AND Purchase_Address LIKE "%Los Angeles%"
AND Quantity_Ordered == 1
GROUP BY Product
ORDER BY COUNT(Order_ID) DESC
```

在这里，我们在之前的查询中添加了一些内容，包括多个过滤条件以及 ORDER BY，以便查询返回的表按照我们正在聚合的度量进行排序。由于查询中有更多组件，让我们逐步看看如何在 Pandas 中实现这个查询。

首先，我们不直接在调用`loc`方法时传递多个条件，而是定义一个条件列表，并将其赋值给变量`FILTER_CONDITIONS`。

```py
FILTER_CONDITIONS = [
    df["Order_ID"].notna(),
    df["Purchase_Address"].str.contains("Los Angeles"),
    df["Quantity_Ordered"] == "1",
]
```

如之前所述，传递给`loc`的条件应该是一个 Pandas 掩码，其结果为真或假。可以将多个条件传递给`loc`，但语法应如下所示：

```py
df.loc[condition_1 & condition_2 & condition_3]
```

然而，像这样直接传递条件列表是不可行的：

```py
df.loc[FILTER_CONDITIONS]  
# doesn't work -> you can't just pass a list into loc
```

如果你尝试上述操作会出现错误，因为每个条件应通过`&`操作符（用于“且”条件）或`|`操作符（用于“或”条件）分隔。相反，我们可以编写一些快速代码来以正确的格式返回条件。我们将使用`functools.reduce`方法将条件组合在一起。

如果你想在笔记本中查看它的样子，并查看使用`reduce`函数组合字符串的效果，可以尝试这个：

```py
reduce(lambda x, y: f"{x} & {y}", ["condition_1", "condition_2", "condition_3"])
```

这将输出如下字符串：

```py
>>> 'condition_1 & condition_2 & condition_3'
```

回到我们实际的 Pandas 条件，我们可以改写为（不使用字符串格式化，仅使用我们在`FILTER_CONDITIONS`变量中定义的条件列表）。

```py
reduce(lambda x, y: x & y, FILTER_CONDITIONS)
```

`reduce`的作用是将一个函数累积地应用于一个可迭代对象中的元素，或者在我们的例子中，对`FILTER_CONDITIONS`列表中的每一项运行`lambda`函数，并用`&`操作符将它们组合起来。这会一直运行，直到没有条件剩下，或者在这种情况下，对于所有三个条件，它将有效地返回：

```py
df["Order_ID"].notna() & df["Purchase_Address"].str.contains("Los Angeles") & df["Quantity_Ordered"] == "1"
```

最后，让我们将条件列表添加到 Pandas 中以创建最终的分组查询：

```py
final_df = (
    df
    .loc[reduce(lambda x, y: x & y, FILTER_CONDITIONS)]
    .groupby("Product")
    .size()
    .sort_values(ascending=False)
)
```

你会注意到与之前的查询有两个额外的区别：

1.  与其指定要计数的特定列，我们可以简单地调用 `size` 方法，它会返回 DataFrame 中的行数（就像之前每个 `Order_ID` 值都是唯一的，并且意味着我们计数时的每一行一样）；

1.  在 Pandas 中有几种不同的方法来进行排序，其中一种方法是简单地调用 `sort_values` 并传递 `ascending=False` 以按降序排序。

如果你想使用之前的语法来聚合数据，它将如下所示：

```py
final_df = (
    df
    .loc[reduce(lambda x, y: x & y, FILTER_CONDITIONS)]
    .groupby("Product")
    ["Order_ID"].count()
    .sort_values(ascending=False)
)
```

![](img/60a5fc51f777d90503c1a3fcde57f3a7.png)

Pandas 代码输出 — 图片由作者提供

两种方法的输出将与之前相同，即一个 Series，其中包含你分组的列和每个产品的计数。

如果你想要输出 DataFrame，可以在系列上调用 `reset_index` 方法，以获取你分组的列和你聚合的列的原始列名（在这种情况下，我们按“Product”分组，并计数“Order_ID”）。

```py
final_df.reset_index()
```

![](img/2424825fd9300c03675c23d2ab7faad7.png)

Pandas 代码输出 — 图片由作者提供

就这样！所有完整 SQL 查询的组件，但最终用 Pandas 编写。为了进一步优化处理数据的过程，我们可以做以下一些事情：

+   将不同的列列表（用于 SELECT 或 GROUP BY）放入它们自己的变量或函数中（这样你或用户可以随着时间的推移修改它们）；

+   将合并列列表的逻辑移到自己的函数中，以便最终用户不会对 `reduce` 逻辑感到困惑；

+   在调用 `reset_index` 之后，我们可以重命名输出列（如果我们在多个列上聚合，则重命名多个列），以便更清晰，例如命名为“Count_Order_ID”。

感谢你花时间阅读这篇文章！如果你还没有，请点击关注按钮，以便及时了解我更多的技术文章。此外，如果你喜欢我的内容，我会很高兴如果你**通过下面的推荐链接注册 Medium**。这样我可以获得你月订阅的一部分，而且你将可以访问一些只有 Medium 会员才能使用的独家功能。

[](https://byrondolon.medium.com/membership?source=post_page-----6983cae8426e--------------------------------) [## 通过我的推荐链接加入 Medium — Byron Dolon

### 作为 Medium 会员，你的会员费的一部分将分配给你阅读的作者，你可以访问每一个故事…

byrondolon.medium.com](https://byrondolon.medium.com/membership?source=post_page-----6983cae8426e--------------------------------)

> **更多来自我：** *-* [*5 个实用技巧助你成为数据分析师*](https://byrondolon.medium.com/5-practical-tips-for-aspiring-data-analysts-9917006d4dae?sk=019edbddaca4d313665caafe4b747d26) *-* [*掌握电子商务数据分析*](https://python.plainenglish.io/mastering-analysis-for-e-commerce-with-pandas-e4219a87b10c?sk=9aa8fd1024b89e89e4b0904c8d00d242) *-* *在 Pandas DataFrame 中检查子字符串* *-* [*学习 Python 的 7 个最佳 GitHub 仓库*](https://medium.com/towards-data-science/top-7-repositories-on-github-to-learn-python-44a3a7accb44) *-* *5（又半）行代码助你理解 Pandas 数据*
