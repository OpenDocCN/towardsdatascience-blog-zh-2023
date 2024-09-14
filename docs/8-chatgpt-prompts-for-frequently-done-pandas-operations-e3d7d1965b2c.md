# 8 个 ChatGPT 提示，用于频繁执行的 Pandas 操作

> 原文：[`towardsdatascience.com/8-chatgpt-prompts-for-frequently-done-pandas-operations-e3d7d1965b2c`](https://towardsdatascience.com/8-chatgpt-prompts-for-frequently-done-pandas-operations-e3d7d1965b2c)

## 使用 Pandas 快速完成任务

[](https://sonery.medium.com/?source=post_page-----e3d7d1965b2c--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----e3d7d1965b2c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e3d7d1965b2c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e3d7d1965b2c--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----e3d7d1965b2c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e3d7d1965b2c--------------------------------) ·7 分钟阅读·2023 年 5 月 26 日

--

![](img/c2e8e682e64722afd27b4fea46bcaa24.png)

由 [Karsten Winegeart](https://unsplash.com/@karsten116?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/YzxeHEzBZ6I?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

我们都听说过 ChatGPT。它不仅在科技行业引起关注，也在更广泛的媒体上成为头条新闻。我经常收到朋友问我是否听说过它。是的，我当然听说过 —— 我每天都在使用它。

尽管有一些关于其在简单任务上表现和可靠性的批评，ChatGPT 和其他大型语言模型（LLMs）在各种任务中表现出色。对我来说，它们大大提高了生产力。

最近，我决定利用 ChatGPT 来进行我定期执行的数据清理和分析的 Pandas 操作。在这篇文章中，我将分享并指导你如何使用 ChatGPT 完成 Pandas 操作的 8 个提示示例。

*如果你想了解更多关于 Pandas 的信息，请访问我的课程* [*500 Exercises to Master Python Pandas*](https://www.udemy.com/course/500-exercises-to-master-python-pandas/learn/lecture/37842166#overview)*。*

**第一个提示以定义其角色：**

*提示：你是一个 Python 导师，教我 Pandas 库。我会问你如何使用 Pandas 完成特定任务，并希望你向我解释。同时也展示一下代码和你的解释。*

在开始提问之前，我已经根据列名和数据类型给 DataFrame 设定了结构。

**第二个提示：**

*提示：首先让我告诉你我拥有的 DataFrame 的信息。然后，我将开始提问。列及其数据类型如下所示，以 Python 字典的形式呈现，键显示列名，值显示数据类型。*

*{‘store’: dtype(‘O’), ‘product_group’: dtype(‘O’), ‘product_code’: dtype(‘int64’), ‘stock_qty’: dtype(‘int64’), ‘cost’: dtype(‘float64’), ‘price’: dtype(‘float64’), ‘last_week_sales’: dtype(‘int64’), ‘last_month_sales’: dtype(‘int64’)}*

我将使用一个我准备的包含虚拟数据的样本数据集。你可以从我的[数据集](https://github.com/SonerYldrm/datasets)仓库下载。它是名为“sales_data_with_stores.csv”的那个。

这是我为你准备的 DataFrame，方便你参考：

![](img/0733c160600599104d25e18c2c61efc7.png)

DataFrame 的前 5 行（图片由作者提供）

我们现在可以开始处理 Pandas 任务了。

## **示例 1**

*提示：告诉我如何计算每个商店上周的平均销售额。*

这是一个简单的一行代码，但如果你刚开始学习 Pandas，它仍然可能需要一些时间来弄清楚。以下是 ChatGPT 的答案：

![](img/59a8c41893d9f96a3746f2b76b72e8cc.png)

(图片由作者提供)

我将复制并粘贴代码片段，以防你想自己测试。

```py
# Import pandas
import pandas as pd

# Assuming 'df' is your DataFrame
average_sales = df.groupby('store')['last_week_sales'].mean()
```

ChatGPT 还提供了详细的解释和代码，说明了任务是如何完成的以及输出的类型。

## **示例 2**

*提示：告诉我如何找到上个月每个商店的总利润。*

对于这个任务，首先需要计算利润。让我们看看它的回应：

![](img/1f030edaa0c72a6862468502fd2792f2.png)

(图片由作者提供)

```py
# Calculate profit per product
df['last_month_profit'] = (df['price'] - df['cost']) * df['last_month_sales']

# Calculate total profit per store
total_profit = df.groupby('store')['last_month_profit'].sum()
```

## 示例 3

在处理真实世界的数据时，我们常常会遇到缺失值的挑战。我想看看 ChatGPT 如何处理一个不那么简单的任务。

*提示：库存数量列中有一些缺失值。我想用该商店的平均库存数量来替换这些缺失值。告诉我如何完成这个任务。*

![](img/5d4b2e71dabef0f409234e2b3fff1795.png)

(图片由作者提供)

```py
# Calculate the average stock quantity per store
average_stock_qty_per_store = df.groupby('store')['stock_qty'].transform('mean')

# Replace missing values in 'stock_qty' with the average stock quantity of that store
df['stock_qty'].fillna(average_stock_qty_per_store, inplace=True)
```

解决这个任务还有其他方法，但使用 transform 函数是高效的方法之一。至少，这就是我会做到的方式。

我最近写了一篇[文章](https://medium.com/@sonery/5-python-pandas-functions-you-should-not-forget-about-91a0acc8e718)，提到了 transform 函数的重要性。以下是文章中的一个简短片段，解释了我为何会使用 transform 函数来完成这个任务：

这听起来类似于`apply`函数，但不同于`apply`函数的是，`transform`可以返回一个与输入具有相同大小的 Series，这使得它在执行向量化操作和广播时非常有用，尤其是在与 groupby 对象一起使用时。

## **示例 4**

*提示：告诉我如何找到上个月每个商店销售额最高的前 3 个产品。*

![](img/c885d75b38ed27ee5632858f27587bd3.png)

(图片由作者提供)

```py
# First, sort the DataFrame by 'store' and 'last_month_sales'
df_sorted = df.sort_values(['store', 'last_month_sales'], ascending=[True, False])

# Then, group by 'store' and use the apply method to select the top 3 rows for each group
top_3_products = df_sorted.groupby('store').head(3)
```

它首先对整个 DataFrame 进行排序。我们还可以在每个组内进行排序。让我们看看 ChatGPT 是否知道如何做到这一点。

## **示例 5**

*提示：是否有另一种方法可以在不先对 DataFrame 进行排序的情况下完成这个任务？*

![](img/88e97c4e53d16c513c7e952d6df866f9.png)

(图片由作者提供)

```py
def top_3_products(group):
    return group.nlargest(3, 'last_month_sales')

top_3_df = df.groupby('store').apply(top_3_products)
```

需要注意的是，首先对整个 DataFrame 进行排序可能是更高效的方法。我只是想看看 ChatGPT 是否提供了不同的解决方案。

## **示例 6**

*提示：我想查看每家商店上周的表现与上个月的表现比较。*

这是一个简单的任务，但提示没有直接告诉 ChatGPT 怎么做。让我们看看它的表现如何。

![](img/7a8ce1a79293b040c0a0260039ebe987.png)

(图片由作者提供)

```py
# Calculate total sales for each store in the last week and last month
total_sales_last_week = df.groupby('store')['last_week_sales'].sum()
total_sales_last_month = df.groupby('store')['last_month_sales'].sum()

# Create a new DataFrame to store this information
performance_df = pd.DataFrame({
    'LastWeekSales': total_sales_last_week,
    'LastMonthSales': total_sales_last_month,
})

# Calculate the change in sales from last month to last week
performance_df['SalesChange'] = performance_df['LastWeekSales'] - (performance_df['LastMonthSales'] / 4)
```

作为对上周表现与上个月相比的回应，它计算了上周销售额与上个月销售额的四分之一之间的差异，这很有意义。

它创建的 DataFrame 看起来是这样的：

![](img/a42aaa52a4d7ce600d46d53a37064e39.png)

## 示例 7

*提示：创建一个名为 `critical_stock` 的新列，如果 `stock_qty` 小于 500，则值为 1，否则为 0。*

ChatGPT 为这个任务提供了 3 个选项。这是我比较喜欢的两个：

![](img/9ddd6f4c1b4027af78fa5c385fd55b3d.png)

(图片由作者提供)

```py
# first option
df['critical_stock'] = df['stock_qty'].apply(lambda x: 1 if x < 500 else 0)

# second option
import numpy as np

df['critical_stock'] = np.where(df['stock_qty'] < 500, 1, 0)
```

## 示例 8

*提示：创建一个 DataFrame 的摘要，包括大小、缺失值、数据类型和列统计信息。*

用于总结 DataFrame 的函数和方法相当简单，但 ChatGPT 做得很好，将它们整理得非常整洁。

我只是复制粘贴了它生成的代码：

```py
# Import necessary libraries
import pandas as pd

# DataFrame size
print("DataFrame Size:")
print(df.shape)
print("\n")

# Missing values
print("Missing Values:")
print(df.isnull().sum())
print("\n")

# Data types
print("Data Types:")
print(df.dtypes)
print("\n")

# Column statistics
print("Column Statistics:")
print(df.describe(include='all'))
```

# 最后的话

我们讨论的示例简要展示了 ChatGPT 如何彻底改变我们在数据分析中执行常见任务的方式，特别是在 Pandas 上。它不仅提高了我们完成任务的效率，还丰富了我们对 Pandas 复杂操作的理解。

我将探索更多利用 ChatGPT 的方法来完成更复杂的任务。需要注意的是，随着任务复杂性的增加，你编写提示的方式变得尤为重要。这就是为什么有一个新的学科正在出现，叫做提示工程。

*你可以成为* [*Medium 会员*](https://sonery.medium.com/membership) *来解锁对我写作的完整访问权限，以及 Medium 其余的内容。如果你已经是会员，别忘了* [*订阅*](https://sonery.medium.com/subscribe) *以便每当我发布新文章时能收到电子邮件。*

感谢阅读。如果你有任何反馈，请告诉我。
