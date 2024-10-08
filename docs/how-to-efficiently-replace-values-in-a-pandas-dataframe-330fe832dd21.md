# 如何高效地替换 Pandas DataFrame 中的值

> 原文：[`towardsdatascience.com/how-to-efficiently-replace-values-in-a-pandas-dataframe-330fe832dd21`](https://towardsdatascience.com/how-to-efficiently-replace-values-in-a-pandas-dataframe-330fe832dd21)

## PYTHON

## 对 Pandas 替换方法的详细讲解，以及如何通过几个简单的示例使用它

[](https://byrondolon.medium.com/?source=post_page-----330fe832dd21--------------------------------)![Byron Dolon](https://byrondolon.medium.com/?source=post_page-----330fe832dd21--------------------------------)[](https://towardsdatascience.com/?source=post_page-----330fe832dd21--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----330fe832dd21--------------------------------) [Byron Dolon](https://byrondolon.medium.com/?source=post_page-----330fe832dd21--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----330fe832dd21--------------------------------) ·阅读时间 8 分钟·2023 年 7 月 12 日

--

![](img/53c719fb6e620fc207e6e6e861e91571.png)

图片由我的才华横溢的姐姐[ohmintyartz](https://www.instagram.com/ohmintyartz/)提供许可使用

Pandas 库提供了多种内置方法，供你用来处理和清洗数据，以便为分析和机器学习做好准备。

在处理不同类型的数据时，你经常会发现需要根据条件删除整行或更新字符串值的一部分作为数据清洗的一部分。你也可能想从现有列中创建新列，作为特征工程过程的一部分。

Pandas 将让你使用其本地转换方法对对象和字符串数据类型执行各种操作。在这篇文章中，让我们特别看看如何在 DataFrame 的列中替换整个值和/或子字符串。

随意在笔记本中跟随本篇示例操作！你可以从 Kaggle 下载[数据集](https://www.kaggle.com/datasets/drahulsingh/top-largest-universities/versions/1?resource=download)，该数据集在 Open Data Commons Public Domain Dedication and License (PDDL) v1.0 下免费使用。然后导入并运行以下代码，我们可以开始了！

```py
import pandas as pd
df_raw = pd.read_csv("Top-Largest-Universities.csv")
```

![](img/fef95aa81d4bae2014d6111cc7f1d3ed.png)

图片由作者提供

## 使用 Pandas 中的“replace”来编辑 DataFrame Series（列）中的子字符串值

假设我们想要专门查看“Continent”列中的值。我们可以使用 Pandas 中的`value_counts`方法，它实际上是对指定列进行分组，然后返回每个列值在 DataFrame 中唯一值的计数。这对于查看列中每个唯一值在 DataFrame 中出现的次数非常有用。

```py
df.value_counts("Continent")
```

![](img/5eab7bb4cc77fbe3ebb22e556a5d151d.png)

图片由作者提供

现在，假设我们想要去掉“North” America，这样我们可以将其简化为“America”。我们可能希望这样做，以便创建一个新的列作为特征工程过程的一部分，或者如果我们想将北美和南美视为一个区域。

我们可以通过使用 replace 方法从字符串“North America”中删除“North”来实现这一点。我们可以这样尝试：

```py
df[“Continent”].replace(“North”)
```

![](img/c239e9c4720583e098931b70cb4a0d61.png)

作者提供的图片

这不起作用，因为如果你仅传递一个字符串值来替换，Pandas 方法只会替换在 Series 中找到的完全匹配的值。要对子字符串进行简单匹配，我们可以这样做：

```py
df[“Continent”].replace(to_replace=”North”, value=””, regex=True)
```

在这个片段中，我们做了几件不同的事情：

+   `to_replace` — 为了清楚起见，我们将“North”传递给这个参数，以便将来清楚这个方法的作用（我们知道我们要替换哪个值）；

+   `value` — 我们在这里传递一个空字符串“”以指定我们希望该方法将“North”替换为空（实际上是从原始字符串中删除子字符串）；

+   `regex` — 我们将其设置为“True”，这让 Pandas 方法知道我们传递给`to_replace`的字符串应该被视为正则表达式。

代码现在返回一个新的编辑后的 Pandas Series：

![](img/ec7c38ecb2ff8b7850bc8571ed3c85be.png)

作者提供的图片

## 在 Pandas DataFrame Series 中替换多个子字符串

我们现在可以进一步查看在“Continent”列中同时替换“North”和“South”子字符串。为此，你可以将一个字符串列表传递给`to_replace`参数，而不仅仅是一个字符串：

```py
df["Continent"].replace(to_replace=["North", "South"], value="", regex=True)
```

![](img/2de0ee838a503d2d3273284cf8c366f4.png)

作者提供的图片

根据输出，代码成功地从列中删除了“North”和“South”这些子字符串。然而，如果我们更仔细地查看用`value_counts`修改后的 Series，你会看到：

![](img/202409d76368694abf1f52703f670254.png)

作者提供的图片

这在正确的方向上迈出了一步，但还不完全符合我们的初衷，因为现在我们有了“ America”作为替换值。这是因为 Pandas 方法匹配的是“North”或“South”的精确字符串，但原始数据实际在子字符串后面包含一个空格。

## 使用正则表达式和 Pandas 替换方法

为了解决这个问题，我们可以显式地将一个符合我们初始需求的正则表达式作为参数传递给`to_replace`：

```py
df[“Continent”].replace(to_replace=r"(North|South) ", value="", regex=True)
```

在这里，我们将正则表达式`r"(North|South) "`传递给`to_replace`参数，这使我们能够从字符串值中提取出“North”或“South”后面跟一个空格的单词。

然后，就像之前一样，我们用空字符串替换了字符串的那部分，有效地从原始字符串中删除了子字符串。你可以看到下面对原始 Series 的更改：

![](img/5090e7fd61c1be739afb6551556cc1a7.png)

作者提供的图片

要检查汇总结果，如果我们再次将`value_counts`附加到上述内容中，我们会得到：

![](img/753c6cab3458ea40795da3126abcd12a.png)

作者提供的图片

现在，我们已经实现了最初的目标，即通过使用 Pandas 中的 replace 方法将“North America”和“South America”合并为一个值“America”！

我们知道这也已经生效，因为原始数据集中“North America”有 31 个值，“South America”有 6 个值，这从我们第一次对 continent 列运行`value_counts`中可以看到。当你为分析实现这种代码时，实施某种数据验证或测试以确保处理按预期进行也是很好的做法。

## 在一次函数调用中替换 Pandas DataFrame 中多个列的值

你还可以在 DataFrame 上调用`replace`值，而不是特定的系列。这在你想编辑存在于多个列中的子字符串并将它们替换为相同的值时非常方便。

例如，我们可以考虑将 DataFrame 中的所有标点符号替换为更容易进行分析的东西。如果所有的标点符号都被简单地替换为下划线，那么处理字符串数据可能会更容易，而不是处理逗号、括号和破折号。

为此，我们可以运行以下代码：

```py
edited_df = (
    df.replace(
        to_replace={
            "Distance / In-Person": ["/", "-"], 
            "Institution": [", ", " "], 
            "Location": [", ", " "]
        }, 
        value="_", 
        regex=True)
)
```

之前，我们传递给`to_replace`的是一个列表或字符串（以及正则表达式字符串）。这一次，我们利用了`to_replace`方法也接受字典作为有效参数的特性。

在这种情况下，我们想用下划线替换初始 DataFrame 中几个列的不同值。为此，我们传递一个字典，其中键是 DataFrame 中列的名称，值是我们要替换的子字符串。

你会注意到，对于“Distance / In-Person”列，我们将一个列表作为该键的值。这是因为我们仍然可以指定多个值进行匹配。我们还为“Institution”和“Location”列传递列表，以便进一步清理数据，我们可以删除空格并用下划线替换它们。

当使用这种方式与 replace 方法时，我们还必须明确地在`value`参数中传递一个字符串，否则将会引发错误。在这种情况下，由于我们想将所有标点符号更改为下划线，因此我们只需将`value="_"`。然后，和之前一样，为了匹配可能出现在目标值中的子字符串而不是精确字符串，我们设置`regex=True`。

我们将其分配给一个新的清理后的 DataFrame `edited_df`，其样子如下：

![](img/16e1dcd3d600b9a8a846073df84bc51f.png)

作者提供的图片

我们现在已经成功地在一次函数调用中编辑了整个 DataFrame，并用下划线替换了目标列中的标点符号和空格。

- 如果你想将对原始 DataFrame 的更改应用到数据框而不是像我们所做的那样赋值给新变量，`replace`方法也接受一个可选参数`inplace`，你可以将其设置为“True”（因为默认情况下设置为“False”）。

- 这些只是一些示例来帮助你入门。使用 Pandas 中的 replace 方法可以处理 DataFrame 中的各种数据类型，还有很多其他功能可以探索。

- 使用此方法时还需考虑一些额外的事项：

+   - 建议进行检查，以确保`replace`方法按预期工作。如果你的原始数据随着时间的推移而变化，你最初编写的匹配子字符串的代码可能不再有效，你可能需要更新你编写的正则表达式，以确保输出符合预期；

+   - 对于更复杂的操作，例如匹配许多不同的子字符串或列，你可能想单独定义字典，而不是直接将其传递到`to_replace`参数中。这将提供更好的可读性，之后你也可以简单地传递一个变量，例如`values_to_replace`，该变量包含你定义的字典，包含列和要匹配的子字符串。

- 我希望你发现这篇文章中的示例有用！祝你在数据探险中好运。

- 如果你喜欢我的内容，考虑通过下面的推荐链接**注册成为 Medium 会员**。每月仅需 $5，你将无限访问 Medium 上的所有内容。通过我的链接注册让我赚取一小笔佣金。如果你已经注册关注我，非常感谢你的支持！

- [## 通过我的推荐链接加入 Medium - Byron Dolon](https://byrondolon.medium.com/membership?source=post_page-----330fe832dd21--------------------------------)

### - 作为 Medium 会员，你的会员费的一部分将用于你阅读的作家，并且你可以完全访问每一个故事…

- [byrondolon.medium.com](https://byrondolon.medium.com/membership?source=post_page-----330fe832dd21--------------------------------)

> - *更多内容:* - 理解 Pandas 中数据的 5（加半行）行代码
> 
> - 在 GitHub 上学习 Pandas 的前 4 个仓库
> 
> - 在 Pandas DataFrame 中检查子字符串
> 
> - 使用 Pandas 中的 .loc 进行条件选择和赋值
> 
> - 通过 Pandas 从网站获取表格的 2 种简单方法
