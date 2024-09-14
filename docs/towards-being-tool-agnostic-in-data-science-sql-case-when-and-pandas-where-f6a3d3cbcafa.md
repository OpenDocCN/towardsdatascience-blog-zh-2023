# 实现数据科学中的工具无关性：SQL 中的 CASE WHEN 与 Pandas 中的 WHERE

> 原文：[`towardsdatascience.com/towards-being-tool-agnostic-in-data-science-sql-case-when-and-pandas-where-f6a3d3cbcafa`](https://towardsdatascience.com/towards-being-tool-agnostic-in-data-science-sql-case-when-and-pandas-where-f6a3d3cbcafa)

## 通过示例进行解释

[](https://sonery.medium.com/?source=post_page-----f6a3d3cbcafa--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----f6a3d3cbcafa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f6a3d3cbcafa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6a3d3cbcafa--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----f6a3d3cbcafa--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6a3d3cbcafa--------------------------------)·阅读时间 6 分钟·2023 年 7 月 17 日

--

![](img/886913bc04bb14136595417bda17365c.png)

图片由[Monika Simeonova](https://unsplash.com/@monnysim?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来自[Unsplash](https://unsplash.com/photos/yUdawywuYm0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你的客户数据在一个 SQL 数据库中。你被分配了一个任务，涉及从某些表中检索数据，进行一些数据清洗和处理，并将结果写入另一个表中。

不幸的是，你不知道如何用 SQL 执行这些操作。别担心！你擅长使用 Pandas 进行数据清洗和处理。因此，你想出了一个解决方案，即：

+   从 SQL 表中检索所有数据

+   将数据下载为 CSV 文件

+   将 CSV 文件读取到 Pandas DataFrames 中

+   执行所需的数据清洗和处理操作

+   将结果写入另一个 CSV 文件

+   将 CSV 文件中的数据上传到 SQL 表中

计划不错吧？

如果你真的执行了这个计划，我相信你的经理会和你谈一谈，这可能是愉快的，也可能是不愉快的，这取决于你经理的个性。无论如何，我不认为在谈话后你还会执行这个很棒的计划。

我知道在数据科学中通常有很多不同的方法来完成一个任务。你应该始终追求最有效的方法，因为你通常会处理非常大的数据集。让事情变得比必要的更复杂会浪费你额外的时间和金钱。

“我擅长 Pandas，所以我会用 Pandas 做所有事情”并不是一种理想的态度。如果你的任务涉及从 SQL 表中读取数据并将结果写入 SQL 表，通常最好的方法是使用 SQL 完成中间的步骤。

SQL 不仅仅是一种查询语言。它也可以作为一个高效的数据分析和处理工具。

我记得编写 SQL 作业来进行非常复杂的数据预处理操作，它们运行得很好。

数据科学仍然是一个不断发展的领域。新的工具和概念不断出现。你不应依赖单一工具，应该始终保持学习新工具的开放态度。

## Pandas 与 SQL

Pandas 和 SQL 在能力上有很多相似之处。两者都非常擅长数据分析和处理。

在本文中，我们将通过示例学习如何使用 SQL `CASE WHEN`语句和 Pandas `where`函数来完成相同的操作。作为额外内容，我们还将了解 NumPy `where`函数，它比 Pandas `where`更灵活。

我在一个 SQL 表和一个名为`product_inventory`的 Pandas DataFrame 中存储了以下数据。

![](img/757a12c4e1d2c24775a6c1f504781519.png)

数据的前 5 行（作者提供的图片）

你可以从我的[数据集](https://github.com/SonerYldrm/datasets)库中下载这个数据集。它的名字是`product_inventory.csv`。

## 第一个任务

我们将首先创建一个名为`has_enough_stock`的新列，如果库存量大于 500 则取值 1，否则取值 0。

SQL 版本：

```py
SELECT
 product_id,
 product_description,
 price,
 stock_qty,
 next_shipment,
 CASE
  WHEN stock_qty > 500 THEN 1
  ELSE 0
 END AS has_enough_stock
FROM product_inventory
```

以下部分根据指定条件创建`has_enough_stock`列：

```py
CASE
  WHEN stock_qty > 500 THEN 1
  ELSE 0
END AS has_enough_stock
```

Pandas 版本包括`where`函数，该函数接受一个条件并在条件为假时替换值。

为了在我们的任务中使用它，我们首先需要创建一个全为 0 的`has_enough_stock`列。然后，我们将库存量大于 500 的 0 更改为 1。

因为`where`函数允许在条件为假时更改值，所以我们将条件定义为`stock_qty <= 500`（即库存量小于或等于 500）。该条件为假时的行即库存量大于 500 的行。

```py
# create the has_enough_stock column with all 0s
product_inventory.loc[:, "has_enough_stock"] = 0

# change values to 1 where stock quantity is more than 500
product_inventory.loc[:, "has_enough_stock"] = product_inventory.where(product_inventory["stock_qty"] <= 500, 1)

# display the first 5 rows
product_inventory.head()
```

![](img/b1a04f964c78ca7506860275829640c1.png)

`product_inventory` DataFrame 的前 5 行（作者提供的图片）

## NumPy where 函数

使用 Pandas `where`函数时，我们需要首先创建一个全为 0 的`has_enough_stock`列。这是因为它只允许更新不符合给定条件的值。

NumPy `where`函数允许你更新符合和不符合条件的值，因此无需先初始化列。

![](img/dc6761d832f4f6f443e74e774ade5fab.png)

NumPy where 函数（作者提供的图片）

以下是如何使用 NumPy `where`函数创建`has_enough_stock`列的方法：

```py
import numpy as np

# create the column
product_inventory.loc[:, "has_enough_stock"] = np.where(product_inventory["stock_qty"] > 500, 1, 0)
```

第一个参数是条件，第二个是符合条件的行所使用的值，第三个是对于不符合条件的行的值。

## 如果我们需要多于 2 个类别怎么办？

我们做的示例涉及创建一个只有 2 个值（即类别）的列。我们还将做一个需要创建多个类别的示例。

*你可以成为* [*Medium 会员*](https://sonery.medium.com/membership) *以解锁对我写作内容的完全访问权限，并获得 Medium 的其他内容。如果你已经是会员，别忘了* [*订阅*](https://sonery.medium.com/subscribe) *以便在我发布新文章时收到电子邮件。*

我们将根据以下标准创建一个名为`stock_situation`的新列：

+   库存数量 > 500，“足够”

+   库存数量 ≤ 500 且库存数量 > 200，“紧急”

+   库存数量 ≤ 200，“尽快订购”

SQL 版本非常相似。我们只需添加一行`WHEN condition THEN value`。

```py
SELECT
 product_id,
 product_description,
 price,
 stock_qty,
 next_shipment,
 CASE
  WHEN stock_qty > 500 THEN 'enough'
  WHEN stock_qty <= 500 AND stock_qty > 200 THEN 'critical'
  ELSE 'order asap'
 END AS stock_situation
FROM product_inventory
```

在 Pandas DataFrame 中创建此列时，我们需要一个不同的函数，即 NumPy 的`select`函数。

它可以接受一组条件，并允许为每个条件单独分配一个值。

```py
# define conditions
conditions = [
    product_inventory["stock_qty"] > 500,
    (product_inventory["stock_qty"] <= 500) & (product_inventory["stock_qty"] > 200),
    product_inventory["stock_qty"] <= 200
]

# define values associated with the conditions
values = ["enough", "critical", "order asap"]

# create the column
product_inventory.loc[:, "stock_situation"] = np.select(conditions, values)

# display the first 5 rows
product_inventory.head()
```

![](img/ea9d3a4c9371da246c00d093b61ff675.png)

(图片由作者提供)

条件和值是使用 Python 列表创建的。然后，我们将它们作为参数传递给`select`函数。

## 结语

我们做的这些例子并不复杂，但它们支持我的观点，即在处理大数据集时不要拘泥于特定工具。

花几个小时学习一个新工具，从长远来看可以节省你更多时间。它还能避免做不必要的计算，从而节省额外的费用。

感谢阅读。如果你有任何反馈，请告知我。
