# 使用 Pandas 进行 Python 中的数据汇总：分析地质岩性数据

> 原文：[`towardsdatascience.com/data-aggregation-in-python-with-pandas-analysing-geological-lithology-data-94192f5631c0`](https://towardsdatascience.com/data-aggregation-in-python-with-pandas-analysing-geological-lithology-data-94192f5631c0)

## 探索挪威大陆架上 Zechstein 组的岩性变化

[](https://andymcdonaldgeo.medium.com/?source=post_page-----94192f5631c0--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----94192f5631c0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94192f5631c0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----94192f5631c0--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----94192f5631c0--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94192f5631c0--------------------------------) ·6 分钟阅读·2023 年 6 月 29 日

--

![](img/d088a32dac22c1b514824f1a9acdc8f6.png)

图片由作者使用 Midjourney（付费订阅计划）生成。

使用数据汇总技术可以帮助我们将一个庞大且几乎无法理解的数字数据集转换成易于消化且更具可读性的内容。数据汇总过程涉及将多个数据点总结为单一的度量指标，以提供数据的高层次概述。

在岩石物理学和地球科学中，我们可以应用这个过程来总结从井日志测量中解释出的地质构造的岩性组成。

在这个简短的教程中，我们将看到如何处理来自挪威大陆架的 90 多个油井的大型数据集，并提取 Zechstein 组的岩性组成。

# 导入库和加载数据

首先，我们需要导入 [**pandas**](https://pandas.pydata.org) 库，该库将用于从 CSV 加载数据文件并进行汇总。

```py
import pandas as pd
```

一旦导入了 pandas 库，我们就可以使用 `pd.read_csv()` 读取 CSV 文件。

我们将使用的数据来自[**XEEK 和 Force 2020 机器学习竞赛**](https://xeek.ai/challenges/force-well-logs)，该竞赛旨在从井日志测量中预测岩性。我们使用的数据集代表了所有可用的训练数据。有关此数据集的更多详细信息，请参见文章末尾。

由于此 CSV 文件中的数据是用分号而不是逗号分隔的，我们需要在`sep`参数中传递一个冒号。

```py
df = pd.read_csv('data/train.csv', sep=';')
```

然后我们可以运行这段代码开始加载过程。由于我们有一个大数据集（1100 万+行），这可能需要几秒钟。但一旦加载完成，我们可以通过调用`df`对象来查看数据框。这将返回数据框，并显示前五行和最后五行。

![](img/319373d0bba633c6b6147e410779575f.png)

包含挪威大陆架井日志数据的数据框。图片由作者提供。

# 使用 pandas 的.map()将数值代码转换为岩性字符串

在这个数据集中，岩性数据存储在`FORCE_2020_LITHOFACIES_LITHOLOGY`列中。然而，当我们仔细查看数据时，会发现岩性值是以数字编码的。如果你不知道键，就很难解读哪个数字代表哪个岩性。

幸运的是，对于这个数据集，我们有键，可以创建一个包含键和岩性对的字典。

```py
lithology_numbers = {30000: 'Sandstone',
                 65030: 'Sandstone/Shale',
                 65000: 'Shale',
                 80000: 'Marl',
                 74000: 'Dolomite',
                 70000: 'Limestone',
                 70032: 'Chalk',
                 88000: 'Halite',
                 86000: 'Anhydrite',
                 99000: 'Tuff',
                 90000: 'Coal',
                 93000: 'Basement'}
```

要将这应用于我们的数据集，我们可以使用 pandas 的`map()`函数，它将使用我们的字典进行查找，然后将正确的岩性标签分配给数值。

```py
df['LITH'] = df['FORCE_2020_LITHOFACIES_LITHOLOGY'].map(lithology_numbers)
```

一旦运行完成，我们可以再次查看数据框，以确保映射成功并且数据框末尾添加了新的 LITH 列。

![](img/e68e6e4d1bbeb11a678e6589f21bd8d1.png)

包含挪威大陆架井日志数据的数据框。图片由作者提供。

# 为特定地质组过滤数据框

由于我们有一个相当大的数据集，包含 11,705,511 行，因此对我们的岩性组成分析来说，关注特定的地质组会更好。

在这种情况下，我们将对子集数据进行处理，关注 Zechstein 组。

我们可以通过使用`query()`方法并传入一个简单的字符串来实现：`GROUP == "ZECHSTEIN GP."`

```py
df_zechstein = df.query('GROUP == "ZECHSTEIN GP."')
df_zechstein.WELL.unique()
```

我们可以通过调用`df_zechstein.WELL.unique()`来检查子集中有多少个井，它返回包含 8 个井的数组。

```py
array(['15/9-13', '16/1-2', '16/10-1', '16/11-1 ST3', '16/2-16', '16/2-6',
       '16/4-1', '17/11-1'], dtype=object)
```

由于我们只对岩性感兴趣，我们可以简单地提取井名和岩性列。这也将使聚合过程更加便捷。

```py
df_zechstein_liths = df_zechstein[['WELL', 'LITH']]
```

![](img/d36052cce552ce69e671c133ab538299.png)

井名及相关岩性。图片由作者提供。

# 使用链式 Pandas 函数进行数据聚合

现在我们已经将数据整理成可以处理的格式，我们可以开始聚合过程。为此，我们将把多个 pandas 方法链在一起使用。

首先，我们将使用`groupby`函数按 WELL 列对数据进行分组。这实际上为 WELL 列中的每个唯一井名创建数据框的子集。

接下来，我们将计算每个组中每种岩性类型的出现次数。`normalize=True`部分意味着它将给出比例（介于 0 和 1 之间），而不是绝对计数。例如，如果在一个井（组）中，‘砂岩’出现了 5 次，而‘页岩’出现了 15 次，该函数将返回 0.25 给‘砂岩’和 0.75 给‘页岩’，而不是 5 和 15。

最后，我们需要重新排列结果数据框，使得行索引包含井名称，列包含岩性名称。如果某口井中没有某种岩性的实例，则用零填充，因为`fill_value=0`。

```py
summary_df = df_zechstein_liths.groupby('WELL').value_counts(normalize=True).unstack(fill_value=0)
summary_df
```

我们得到的是以下数据框，展示了每口井中每种岩性的十进制比例。

![](img/de1a20c35a3742ebddf37251361fe791.png)

以十进制形式表示的 Zechstein 组岩性组成。图片由作者提供。

如果我们想以百分比的形式查看这些数据，我们可以使用以下代码改变它们的显示方式：

```py
summary_df.style.format('{:.2%}')
```

当我们运行代码时，我们得到以下数据框，它提供了一个更易读的表格，可以纳入报告中。

![](img/ee515a22ee1d934f6d526053afe2da57.png)

聚合后的 Zechstein 组的岩性组成，并将数字转换为百分比。图片由作者提供。

应用这种样式不会改变实际值。它们仍将以其十进制等效值存储。

如果我们确实想将值永久更改为百分比，可以通过将数据框乘以 100 来实现。

```py
summary_df = summary_df * 100
summary_df
```

![](img/c2e3454b59949c293d66f9ccef16bf52.png)

聚合后并转换为百分比的 Zechstein 组岩性组成。图片由作者提供。

一旦数据呈现此格式，我们可以用它创建类似于下方信息图的内容，该图展示了每口井的岩性百分比。

![](img/9c6c88b42a6a79564abfc124d56ec981.png)

展示每口井中每种岩性的百分比变化的信息图。由作者使用 matplotlib 创建。

# 摘要

在这个简短的教程中，我们已经了解了如何从 90 多口井的大量测井数据中提取和总结特定的地质组。这使我们能够以易于阅读和理解的格式来了解地质组的岩性组成，并可纳入报告或演示文稿中。

# 本教程中使用的数据集

训练数据集用作 Xeek 和 FORCE 2020（*Bormann et al., 2020*）举办的机器学习竞赛的一部分。该数据集采用创作共用署名 4.0 国际许可证。

完整的数据集可以通过以下链接访问: [`doi.org/10.5281/zenodo.4351155`](https://doi.org/10.5281/zenodo.4351155)。

*感谢阅读。在你离开之前，你绝对应该订阅我的内容，将我的文章送到你的收件箱中。* [***你可以在这里订阅！***](https://andymcdonaldgeo.medium.com/subscribe)

*其次，你可以通过注册会员获得完整的 Medium 体验，并支持成千上万的其他作者和我。它每月只需 $5，你可以完全访问所有精彩的 Medium 文章，还可以通过写作赚取收入。*

*如果你通过* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***注册***，*你将直接用你的费用的一部分支持我，这不会让你多花钱。如果你这样做，非常感谢你的支持。*
