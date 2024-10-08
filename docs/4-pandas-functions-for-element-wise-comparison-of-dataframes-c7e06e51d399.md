# 4 个 Pandas 函数用于 DataFrame 的逐元素比较

> 原文：[`towardsdatascience.com/4-pandas-functions-for-element-wise-comparison-of-dataframes-c7e06e51d399`](https://towardsdatascience.com/4-pandas-functions-for-element-wise-comparison-of-dataframes-c7e06e51d399)

## 通过示例进行解释。

[](https://sonery.medium.com/?source=post_page-----c7e06e51d399--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----c7e06e51d399--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c7e06e51d399--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7e06e51d399--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----c7e06e51d399--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7e06e51d399--------------------------------) ·4 分钟阅读·2023 年 6 月 10 日

--

![](img/79a6153d096edfc0b646b5adb84002bf.png)

图片由 [NordWood Themes](https://unsplash.com/@nordwood?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/E9tFH39iRPE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

Pandas DataFrame 是具有标签的行和列的二维数据结构。

![](img/e7756b0352e79032b25187b5be0e23cb.png)

具有 3 行 3 列的 DataFrame（图片由作者提供）

我们有时需要对两个 DataFrame 进行逐元素比较。例如：

+   使用另一个 DataFrame 中的值来更新 DataFrame 中的值。

+   比较值并选择较大或较小的值。

在本文中，我们将学习四种不同的 Pandas 函数，这些函数可以用于此类任务。我们还将通过示例更好地理解它们之间的差异和相似之处。

*如果你想了解更多关于 Pandas 的内容，请访问我的课程* [*500 道练习掌握 Python Pandas*](https://www.udemy.com/course/500-exercises-to-master-python-pandas/learn/lecture/37842166#overview)*。*

让我们首先创建两个 DataFrame 以用于示例。

```py
import numpy as np
import pandas as pd

# create DataFrames with random integers
df1 = pd.DataFrame(np.random.randint(0, 10, size=(4, 4)), columns=list("ABCD"))
df2 = pd.DataFrame(np.random.randint(0, 10, size=(4, 4)), columns=list("ABCD"))

# add a couple of missing values
df1.iloc[2, 3] = np.nan
df1.iloc[1, 2] = np.nan
```

![](img/f142324f2e4a8073908c2acaeedf547a.png)

（图片由作者提供）

## 1\. **结合**

`combine` 函数根据给定的函数进行逐元素比较。

例如，我们可以在每个位置选择两个值中的最大值。当我们做例子时会更清楚。

```py
combined_df = df1.combine(df2, np.maximum)
```

![](img/9953c9a1ccdef4469b6be83a2176a0ee.png)

（图片由作者提供）

查看第一行第一列的值。合并的 DataFrame 取 5 和 2 中较大的值。

如果其中一个值是 `NaN`（即缺失值），那么合并后的 DataFrame 在这个位置也会是 `NaN`，因为 Pandas 无法比较带有缺失值的值。

我们可以通过使用`fill_value`参数来选择一个常量值以处理缺失值。在比较之前，缺失值会被这个值填充。

```py
combined_df = df1.combine(df2, np.maximum, fill_value=0)
```

![](img/97958df9142d176939f62c5f79fa45d5.png)

(图片由作者提供)

`df1`中有两个`NaN`值，它们被填充为 0，然后与`df2`中相同位置的值进行比较。

## 2\. combine_first

`combine_first`函数用另一个数据框相同位置的值更新`NaN`值。

```py
combined_df = df1.combine_first(df2)
```

![](img/6fbad36a2da560af5f4b39a6f6bcd0a1.png)

(图片由作者提供)

如上图所示，`combined_df`的值与`df1`相同，除了`NaN`值，这些值被`df2`中的值填充。

需要注意的是，`combine_first`函数不会更新`df1`和`df2`中的值。它仅返回第一个数据框的更新版本。

## 3\. update

`update`函数使用另一个数据框中相同位置的值来更新数据框中的缺失值。

这听起来和`combine_first`函数的作用一样。然而，有一个重要的区别。

`update`函数不返回任何内容，而是就地更新。因此，原始数据框会被修改（或更新）。用一个例子来看会更清楚。

我们有两个数据框，如下所示：

![](img/434db78de9c9dcb341c387a4ba4278ee.png)

(图片由作者提供)

我们来对`df1`使用`update`函数。

```py
df1.update(df2)
```

这行代码不返回任何内容，但它更新了`df1`。更新后的版本是：

![](img/d24111d3fd18b88082a513c9b57cb487.png)

(图片由作者提供)

`df1`不再包含缺失值，这些值已经使用`df2`中的值进行更新。

## 4\. compare

`compare`函数比较相同位置的值，并返回一个数据框，将它们并排显示。

```py
comparison = df1.compare(df2)
```

![](img/10f254be2e1191ee5f5b29b8dbca30f7.png)

(图片由作者提供)

如果某个特定位置的值相同，比较会显示为`NaN`（例如第二行，第一列）。我们可以通过使用`keep_equal`参数来更改这一行为。

```py
comparison = df1.compare(df2, keep_equal=True)
```

![](img/eb488dc39349a4b4f56becd55164f69c.png)

(图片由作者提供)

## 结论

我们学习了四种不同的 Pandas 函数，用于对两个数据框中的值进行逐元素比较。它们各有不同的目的。有些用于更新值，而有些只是进行比较。

在某些情况下，这些函数中的某一个可能是最合适的使用方式。因此，最好了解所有这些函数。

*你可以成为* [*Medium 会员*](https://sonery.medium.com/membership) *以解锁我所有的写作内容，以及 Medium 上的其他内容。如果你已经是会员，请不要忘记* [*订阅*](https://sonery.medium.com/subscribe) *，这样我发布新文章时你就会收到邮件。*

感谢阅读。如果你有任何反馈，请告诉我。
