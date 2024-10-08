# 通过这些技巧和窍门使你的表格数据在 CLI 中脱颖而出

> 原文：[`towardsdatascience.com/make-your-tabular-data-stand-out-via-cli-with-these-tips-and-tricks-a21f276b7ba9`](https://towardsdatascience.com/make-your-tabular-data-stand-out-via-cli-with-these-tips-and-tricks-a21f276b7ba9)

## 提高可读性：在 CLI 中展示数据集的技巧

[](https://federicotrotta.medium.com/?source=post_page-----a21f276b7ba9--------------------------------)![Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----a21f276b7ba9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a21f276b7ba9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a21f276b7ba9--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----a21f276b7ba9--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a21f276b7ba9--------------------------------) ·6 分钟阅读·2023 年 4 月 12 日

--

![](img/cffe7e7f8f14546db47248ae34a16223.png)

图片来源于 [Dorothe](https://pixabay.com/it/users/darkmoon_art-1664300/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3224768) 在 [Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3224768)

几天前，我想帮助我的父亲解决一个问题。他需要尽快聚合、筛选和展示一些数据。事实上，他每次都打印数据（大约 10 页！！）并手动搜索数据！我看到了他的困难，决定立即帮助他。

对于像我这样能够分析数据的人来说，这并不难：数据已经是 Excel 格式，所以 Jupyter Notebook 和 Pandas 是完美的选择。

问题是我不为我的父亲工作。而且，我们住在不同的城市，每隔几周才见一次面。所以，我需要给他一个可以使用的工具，其特点如下：

+   简单使用。

+   如果他输入错误，不要让电脑爆炸。

这就是我决定创建一个可以通过 CLI 管理的小程序的原因。我想创建的是简单的：用户通过命令行输入，然后程序在终端中显示与 Pandas 相同的所有数据。

在这篇文章中，我将展示如何通过 CLI（命令行界面，即终端）展示表格数据。如果你不知道的话，终端就是我们要使用的工具。我们将创建一个简单的项目，让你立刻上手 Python，并讨论我使用的库。

# 创建表格数据

首先，让我们创建一些表格数据以便练习：

```py
import pandas as pd

# Create data

data = {"fruit":["banana", "apple", "pear", "orange"],
      "color":["yellow", "red", "green", "orange"],
      "weight(kg)":[0.3, 0.1, 0.1, 0.2]

}

# Transform data to data frame
df = pd.DataFrame(data)
```

这些是我们的数据：

![](img/04d2689935cec48a1be534f1fbfe8178.png)

我们创建的表格数据。图片由作者提供。

我们创建了一些包含水果信息的表格数据，特别是：水果名称、颜色和重量（以千克为单位）。

现在，为了使其“更真实”，我们可以将其保存到 Excel 文件中，如下所示：

```py
# Save data frame to xlsx file
df.to_excel("fruit.xlsx")
```

```py
**NOTE:** This methodology of saving files that Pandas gives us is very useful.
For example, we can use it to convert CSV files into XLSX; We did it
in this article here.
```

# 问题及其解决方案

现在，我们将创建一个简单的过滤器，如果我们稍微使用一下 Excel，就不需要 Python。我面临的问题更复杂，但这里我们故意简单地创建它：我们的目标不是展示这种方法是否比其他方法更好。这里我们展示的是如何通过 CLI 显示表格数据，一个简单的示例就能完成任务。

所以，假设这是我们的问题：我们希望用户输入一个水果的名称，我们的程序返回所选水果的所有特征。我们还希望过滤器在某种程度上是“智能”的，以便如果用户输入“pea”，它将显示与“pear”相关的特征。

为此，在 Pandas 中我们可以使用方法`str.contains()`。让我们在 Jupyter Notebook 中尝试一下：

```py
import pandas as pd

# Import data
df = pd.read_excel("fruit.xlsx")

# Filter for pear
data_frame = df[df["fruit"].str.contains("pea")]

# Show filtered data
data_frame.head()
```

我们得到：

![](img/e28f8981602d703d7ee22f6cd64f99a5.png)

过滤后的数据。图片由作者提供。

仔细阅读：我们故意将“pea”写成了一个错别字，以确保 Pandas 无论如何都会返回数据。它确实如预期般返回了数据。

现在，我们必须面对另一个问题：用户通过 CLI 的干预。据我所知，在这些情况下，我们可以使用两种不同的方法：我们可以使用内置函数`input`，或者可以使用库`argparse`。

如果你错过了，我写了一篇关于如何在数据科学中使用`argparse`的文章。可以在这里查看：

[](https://medium.com/mlearning-ai/how-argparse-can-be-useful-in-your-data-science-projects-ecf02bef3b07?source=post_page-----a21f276b7ba9--------------------------------) [## Argparse 如何在数据科学项目中发挥作用]

### 我知道，这可能很难承认，但……它真的可以节省你的时间！

[medium.com](https://medium.com/mlearning-ai/how-argparse-can-be-useful-in-your-data-science-projects-ecf02bef3b07?source=post_page-----a21f276b7ba9--------------------------------)

现在，在这种情况下，我决定使用内置函数`input`，因为我认为它更容易使用，在像这样的简单情况下是一个很好的选择。实际上，如果我们只需要通过 CLI 传递一个字符串作为参数，这是完美的选择（你可以在[这里](https://docs.python.org/3/library/functions.html#input)阅读文档）。

我们可以这样使用`input`函数：

```py
# User input
fruit = input("filter the data for the kind of fruit: ")
```

现在，让我们看看这如何工作以及如何返回数据。我们可以使用的代码如下：

```py
import pandas as pd

# User input
fruit = input("filter the data for the kind of fruit: ")

# Import data
df = pd.read_excel("fruit.xlsx")

# Filter for user input
data_frame = df[df["fruit"].str.contains(fruit)]

# Print results
print(data_frame)
```

```py
**NOTE:**
look at the difference of how we've pasted the arguments in the method
str.contains(). Aboved we've passed "pea" with quotes because we were
searching directly for a string.
In this case, insetead, we have passed "fruit" without quotes because
we have used "fruit" as a variable to invoke the input() function so it
has to be passed as is (with no quotes).
```

现在，让我们将其保存为`fruit.py`，移动到`fruit.xlsx`所在的文件夹，并通过终端运行它：

![](img/02f78a8ed9254fc9f09b5f20e2968c42.png)

我们通过 CLI 的代码。GIF 由作者提供。

好吧，正如我们所看到的，一切都正常工作。但有一点：我们可以改进可视化吗？如果我们想要更好地展示数据，就像在 Pandas 中一样，会怎样呢？

嗯，我找到的解决方案是使用库 `tabulate` （[这是文档](https://pypi.org/project/tabulate/)）。

所以，让我们将 `tabulate` 添加到我们的代码中，看看会发生什么：

```py
import pandas as pd
from tabulate import tabulate

# User input
fruit = input("filter the data for the kind of fruit: ")

# Import data
df = pd.read_excel("fruit.xlsx")

# Filter for user input
data_frame = df[df["fruit"].str.contains(fruit)]

# Print results
print(tabulate(data_frame, headers='keys', tablefmt='psql'))
```

然后我们得到了：

![](img/62976a8a02b7d5738cd18e193ecf31f2.png)

我们的 CLI 代码。图片由作者提供。

如我们所见，数据以“表格方式”显示，这样更清晰。此外，正如我们所见，代码可以正确处理拼写错误，如果我们搜索“pea”就像之前在 Jupyter 中那样。

# 结论

我希望这对你在通过 CLI 显示表格数据时有所帮助。如果你有其他建议，请在评论中告诉我：我总是乐于改进和学习新知识。

+   *订阅* [*我的新闻通讯*](https://federicotrotta.substack.com/?r=1ep1nf&utm_campaign=pub-share-checklist) *以获取更多关于 Python 和数据科学的信息。*

+   *喜欢这篇文章吗？通过* [***我的推荐链接***](https://federicotrotta.medium.com/membership)*加入 Medium：以 5$/月（无需额外费用）解锁 Medium 上的所有内容。*

+   *在这里找到/联系我：* [*https://bio.link/federicotrotta*](https://bio.link/federicotrotta)

+   *觉得有用吗？* ***支持我一下*** [***Ko-fi***](https://ko-fi.com/federicotrotta)***。***
