# 多变量分析 — 超越一次一个变量

> 原文：[`towardsdatascience.com/multivariate-analysis-going-beyond-one-variable-at-a-time-5d341bd4daca`](https://towardsdatascience.com/multivariate-analysis-going-beyond-one-variable-at-a-time-5d341bd4daca)

## Python 中的多变量分析与可视化

[](https://medium.com/@fmnobar?source=post_page-----5d341bd4daca--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----5d341bd4daca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d341bd4daca--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d341bd4daca--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----5d341bd4daca--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d341bd4daca--------------------------------) ·阅读时间 9 分钟·2023 年 1 月 12 日

--

![](img/5c2d26281810d576715f7708bbbd9c84.png)

一只反思数据的猫头鹰，来自 [DALL.E 2](https://openai.com/dall-e-2/)

现在，企业和公司收集尽可能多的信息已成为一种常见做法，即使在收集时这些数据的使用场景尚不明确——希望是将来能够理解和利用这些数据。一旦这些数据集可用，数据驱动的个人将深入数据中，寻找其中隐藏的模式和关系。发现这些隐藏模式的工具之一就是多变量分析。

*多变量*分析涉及分析多个变量（即多变量数据）之间的关系，并理解它们如何相互影响。这是一个重要的工具，帮助我们更好地理解复杂的数据集，从而做出基于数据的明智决策。如果你仅对一次分析一个变量的影响感兴趣，可以通过*单变量*分析来实现，我在这篇文章中做了介绍。

[](/univariate-analysis-intro-and-implementation-b9d1e07e5c16?source=post_page-----5d341bd4daca--------------------------------) ## 单变量分析 — 介绍与实现

### 使用 seaborn 进行的单变量分析：统计数据可视化

towardsdatascience.com

现在我们已经熟悉了*多变量*数据，我们可以将*单变量*数据定义为*多变量*数据的一种特殊情况，其中数据只包含一个变量。类似地，*双变量*数据包含两个变量，依此类推。

在这篇文章中，我们将讨论数值和分类变量的双变量/多变量分析。因此，让我们先快速复习一下这两种变量的区别，然后再进入分析部分。

+   **数值变量：** 代表一个可测量的量，可以是连续变量或离散变量。连续变量可以取某个范围内的任何值（例如身高、体重等），而离散数值变量只能取范围内的特定值（例如孩子的数量、停车场的汽车数量等）。

+   **分类变量：** 代表一个组（或类别），可以取有限数量的值，例如汽车品牌、狗的品种等。

现在我们已经了解了这两种变量的区别，我们可以进入实际的分析部分。

我已经将这篇文章组织成一系列问答的格式，这是我个人认为的有效学习方法。我还在文末提供了一个链接，指向我用来创建这个练习的笔记本。阅读完这篇文章后，随时下载并练习！

让我们开始吧！

*（除非另有说明，所有图像均由作者提供。）*

[](https://medium.com/@fmnobar/membership?source=post_page-----5d341bd4daca--------------------------------) [## 通过我的推荐链接加入 Medium - Farzad Mahmoodinobar

### 阅读 Farzad（以及 Medium 上其他作者）的每一个故事。您的会员费将直接支持 Farzad 和其他人……

medium.com](https://medium.com/@fmnobar/membership?source=post_page-----5d341bd4daca--------------------------------)

# 数据集

为了练习多变量分析，我们将使用来自 [UCI 机器学习库](https://archive-beta.ics.uci.edu/dataset/10/automobile)（CC BY 4.0）的数据集，该数据集包括汽车价格及与每个汽车价格相关的一组汽车属性。为了简化过程，我已经清理和筛选了数据，可以从[这个链接](https://gist.github.com/fmnobar/c9b4029e08e97978a9a53f4eb034b16f)下载。

让我们先导入今天将要使用的库，然后将数据集读入数据框，并查看数据框的前 5 行，以熟悉数据。

```py
# Import libraries
import numpy as np
import pandas as pd
import seaborn as sns
from scipy import stats
import matplotlib.pyplot as plt
%matplotlib inline

# Show all columns/rows of the dataframe
pd.set_option("display.max_columns", None)
pd.set_option("display.max_rows", None)

# Read the data
df = pd.read_csv('auto-cleaned.csv')

# Return top 5 rows of the dataframe
df.head()
```

结果：

![](img/ff72bc71a0bbe3a9d65b49c2b4290ce2.png)

在这篇文章中使用的列是自解释的，因此目前无需担心理解所有列。

让我们继续分析吧！

# 数值双变量分析

让我们从一个包含两个变量的双变量数据集开始。双变量分析的目标是理解两个变量之间的关系。有多种统计技术可以用来分析双变量数据，而散点图是其中最常见的一种。让我们看看散点图是如何工作的。

**问题 1：**

价格与发动机大小之间的关系是什么？直观上，我们可能会预期发动机较大的汽车价格较高（其他条件相同），但让我们看看数据是否支持这一点。创建一个以价格为 x 轴、发动机大小为 y 轴的散点图。

**回答:**

```py
# Create the scatterplot
sns.regplot(data = df, x = 'price', y = 'engine-size', fit_reg = False)
plt.show()
```

结果:

![](img/660a62d731e543b8703b30f522a0dc39.png)

价格与发动机大小的散点图

正如我们所见，数据中价格与发动机大小之间似乎存在正相关关系。需要注意的是，这并不意味着存在因果关系（无论这种关系是否正确），只是展示了两者之间的正相关性。让我们添加相关值，以便为参考提供量化的测量。

**问题 2:**

返回价格与其他变量之间的相关性，并按降序排列。

**回答:**

```py
# Create the overall correlation
corr = np.round(df.corr(numeric_only = True), 2)

# Return correlation only with price
price_corr = corr['price'].sort_values(ascending = False)
price_corr
```

结果:

![](img/a84356105fa742d5c85ea429810850ef.png)

结果确认了我们在散点图中观察到的价格与发动机大小之间的正相关性。让我们尝试更深入地了解数据中的变异情况。

# 异质性与分层

数据中的异质性指的是数据集中的变异。例如，我们的数据集包含了不同的车身风格，如轿车、掀背车、旅行车、敞篷车等。我们是否期望这些车身风格中价格与发动机大小之间的相关性是相似的？例如，客户对敞篷车大发动机的增量支付意愿可能会高于对主要用于家庭的旅行车的支付意愿。让我们检验这个假设，看看不同车身风格之间是否存在这样的变异，通过对数据进行分层分析。

**问题 3:**

数据集包括具有不同车身风格的汽车价格，如“body-style”列所示。数据集中每个类别有多少行？

**回答:**

```py
# Apply value_counts to the df['class'] column
df['body-style'].value_counts()
```

结果:

![](img/300c5a8fd56b16618a7e74d00a6de2ab.png)

根据结果，共有五类。

**问题 4:**

创建每种车身风格的价格与发动机大小的散点图，以展示车身风格之间是否存在视觉上的差异。

**回答:**

```py
sns.FacetGrid(data = df, col = 'body-style').map(plt.scatter, 'price', 'engine-size').add_legend()
plt.show()
```

结果:

![](img/beea590425e09c769485ac14478f41d0.png)

价格与发动机大小的散点图，按车身风格分类

这确实很有趣！这些分布与我们在问题 1 中观察到的总体分布相差甚远，并展示了这五种车身风格之间的视觉差异。所有五种车身风格都如预期般展示了价格与发动机大小之间的正相关性，但坡度似乎在敞篷车上最高（尽管数据点较少），而在旅行车上较低。让我们查看相关数字来量化这些差异。

**问题 5:**

对于每种车身风格，价格与发动机大小之间的相关性是什么？

**回答:**

```py
bodies = df['body-style'].unique()

for body in bodies:
    print(body)
    print(df.loc[df['body-style'] == body, ['price', 'engine-size']].corr())
    print()
```

![](img/3f1a00e2d972d72192dcf473cc4ea224.png)

结果确认了我们的视觉检查——所有车身类型的价格与发动机尺寸的相关性都是正的，其中敞篷车的相关性最高，旅行车的相关性最低，这正如我们直观上所预期的。接下来，我们将查看分类双变量分析。

# **分类双变量分析**

在本节中，我们将创建一个类似的双变量分析，但用于分类变量。在统计学中，这种分析通常通过“列联表”（也称为交叉表或 crosstab）来可视化，它显示了两个（对于双变量）或更多（对于多变量）分类变量的观察频率或计数。让我们看一个例子，以便更好地理解列联表。

**问题 6：**

创建一个显示汽车车身类型和气缸数量的列联表。你在结果中看到模式了吗？

**答案：**

```py
crosstab = pd.crosstab(df['body-style'], df['num-of-cylinders'])
crosstab
```

结果：

![](img/6dae922f62246e9d1e6c33a8ab11b73e.png)

如果你对汽车有所了解，最常见的气缸数量是 4、6 和 8，这也是我们在表中看到的频率最多的地方。我们还可以看到，我们的数据集中大多数汽车是四缸的，车身类型为轿车和掀背车，其次是旅行车。你是否注意到我们在进行心理计算，以计算每种气缸数量和车身类型组合的百分比？列联表可以被规范化来解决这个问题。有三种方法可以规范化这样的表格：

1.  每行的条目总和为 1

1.  每列的条目总和为 1

1.  整个表格的条目总和为 1

让我们在下一个问题中尝试其中一个。

**问题 7：**

创建一个类似于前一个问题的交叉表，规范化方式是每行的条目总和等于 1，四舍五入到两位小数。

**答案：**

我将在这里演示两种不同的方法以供学习。第一种方法使用 Pandas 的 crosstab，第二种方法使用 groupby。

```py
# Approach 1

# Create the crosstab (similar to previous question)
crosstab = pd.crosstab(df['body-style'], df['num-of-cylinders'])

# Normalize the crosstab by row
crosstab_normalized = crosstab.apply(lambda x: x/x.sum(), axis = 1)

# Round the results to two decimal places
round(crosstab_normalized, 2)
```

结果：

![](img/b3ceb7dda346a6b22c1e9c0d179e6ace.png)

```py
# Approach 2

# Group by and count occurences using size method
grouped_table = df.groupby(['body-style', 'num-of-cylinders']).size()

# Pivot the results using unstack and apply the row normalization
grouped_table_normalized = grouped_table.unstack().fillna(0).apply(lambda x: x/x.sum(), axis = 1)

# Round the results to two decimal places
round(grouped_table_normalized, 2)
```

结果：

![](img/1bdb1938ff9376b3110f3b4a0d887a68.png)

# 混合数值和分类数据的双变量分析

我们经常需要分析混合了数值和分类变量的数据，所以让我们看看如何在我们已经知道如何分别处理每种数据类型的情况下来完成这项工作。

**问题 8：**

创建一系列箱线图，展示不同车身类型（x 轴的分类变量）下价格（y 轴的数值变量）的分布。

**答案：**

```py
# Set the figure size
plt.figure(figsize = (10, 5))

# Create the boxplots
sns.boxplot(x = df['body-style'], y = df['price'])
plt.show()
```

结果：

![](img/8e8004bcf2da4937cfd068207274526b.png)

按车身类型分层的汽车价格箱线图

我个人认为这个可视化非常有用。例如，我们可以看到，相比硬顶车或敞篷车，掀背车的价格范围相对较小。与其他车身风格相比，敞篷车的起始价格较高，并且根据车辆的各种特征，价格范围似乎也很广泛。

如果我们只关注轿车，并查看价格范围如何随气缸数量变化？让我们创建箱线图。

```py
# Set the figure size
plt.figure(figsize = (10, 5))

# Create the boxplots
sns.boxplot(x = df[df['body-style'] == 'sedan']['num-of-cylinders'], y = df[df['body-style'] == 'sedan']['price'])
plt.show()
```

结果：

![](img/275566bb329736e1b6c50549e68c496e.png)

按气缸数量分组的轿车价格箱线图

正如预期的那样，气缸数量增加时，价格也随之上升。

# 包含练习题的笔记本

下面是包含问题和答案的笔记本，供参考和练习。

# 结论

在这篇文章中，我们介绍了多变量分析作为发现数据中隐藏模式的工具，然后讲解了如何实现对数值变量、分类变量以及两者混合的分析。我们利用了散点图和箱线图等可视化工具来展示变量之间的关系，并在一些情况下量化了这些相关性。

# 感谢阅读！

如果你觉得这篇文章有帮助，请 [关注我在 Medium 上](https://medium.com/@fmnobar) 并订阅以接收我的最新文章！
