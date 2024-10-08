# 使用图表讲故事

> 原文：[`towardsdatascience.com/storytelling-with-charts-23dd41096721`](https://towardsdatascience.com/storytelling-with-charts-23dd41096721)

## 第一部分：显示单一定量变量

[](https://medium.com/@dar.wtz?source=post_page-----23dd41096721--------------------------------)![Darío Weitz](https://medium.com/@dar.wtz?source=post_page-----23dd41096721--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23dd41096721--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----23dd41096721--------------------------------) [Darío Weitz](https://medium.com/@dar.wtz?source=post_page-----23dd41096721--------------------------------)

· 发表在[数据科学前沿](https://towardsdatascience.com/?source=post_page-----23dd41096721--------------------------------) ·8 分钟阅读·2023 年 2 月 10 日

--

![](img/87afa223cff95ec78d84158a9c6912e6.png)

图片由[Derek Story](https://unsplash.com/@derekstory?utm_source=medium&utm_medium=referral)拍摄，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

每个数据集都包含大量细节。此外，许多数据集只是充满了没有任何分类的数字列表。

例如：

· 阿根廷中部地区过去 20 年 1 月份的平均降雨量。

· 信息系统工程学生的智商测试结果。

· 根据 2022 年普查数据，阿根廷 24 个省份的人口。

· 根据一天中的星期几和小时计算的阿根廷车祸致死人数。

以上是典型的包含相对较少**单一定量变量**的数据集示例。请记住，定量数据表示数量。同时，记住根据值是测量还是计数，定量变量可以是连续的或离散的。

为什么数据分析师应该对这样一组数字感兴趣？首先，尽管许多科学、商业或管理问题涉及数字变量之间的比较、关系、组成或趋势，但**对数据集中每个变量的可视化**作为基本的探索性数据分析，对于理解这些变量的变化模式非常重要。其次，如上例所示，**探索性图表**可以帮助理解生成数据集中存储数字的过程。

如[我之前所述](https://medium.com/analytics-vidhya/scatter-plots-why-how-3de6e1e32645)：“在探索性图表中可以发现数据集的三个重要特征：1）**异常值**，数据集中与其他数据非常不同且不符合相同模式的数据。这些异常值可能代表了有价值的信息用于分析。首先，必须验证这些异常值的存在是否由于数据测量错误；2）**间隙**，一个不包含数据的区间。数据间隙的可视化合理化了对其存在原因的深入分析；3）**簇**，孤立的数据点组，也可能需要对其在图表中存在的原因进行特定分析。当然，间隙和簇可能代表数据收集方法中的错误。”

本文将展示三种简单的（**基于 Python**）探索性图表，以便可视化单一定量变量的分布。

# **点图**

点图，也称为点 chart，是最简单的可视化图之一，由在二维 x 轴和 y 轴方案中绘制的数据值点（小圆圈）组成。一个轴显示数据值分组的类别或范围，而另一个轴显示每个不同组的数据点数量。每个小圆圈代表一个值。根据分析师的喜好，点可以垂直或水平堆叠。

![](img/7efc99e55ef5b6e5e92cbdb235bd6279.png)

图 1：作者使用 Matplotlib 制作的点图。

它们适用于小到中等规模的数据集（10–45 个值），并且非常有助于突出异常值、间隙、簇和偏斜。

点图有两种类型：1）威尔金森点图；2）克利夫兰点图。前者表示连续数据值的分布，而后者是条形图的替代方案。本文专门讨论威尔金森点图。

Matplotlib 没有专门绘制点图的方法。Plotly 使用*px.scatter*来绘制克利夫兰点图。我发现了一段非常有趣的 Python 代码，来源于**帕特里克·菲茨杰拉德**（1）在*stackoverflow*（2）上：

```py
# Create random data
rng = np.random.default_rng(1) # random number generator
data = rng.integers(10, 30, size=45)
values, counts = np.unique(data, return_counts=True)

# Set formatting parameters based on data
data_range = max(values)-min(values)
width = data_range/2 if data_range<30 else 15
height = max(counts)/3 if data_range<50 else max(counts)/4
marker_size = 10 if data_range<50 else np.ceil(30/(data_range//10))

# Create dot plot with appropriate format
fig, ax = plt.subplots(figsize=(width, height))
for value, count in zip(values, counts):
    ax.plot([value]*count, list(range(count)), marker='o', color='green',
            ms=marker_size, linestyle='')
for spine in ['top', 'right', 'left']:
    ax.spines[spine].set_visible(False)
ax.yaxis.set_visible(False)
ax.set_ylim(-1, max(counts))
ax.set_xticks(range(min(values), max(values)+1))
ax.tick_params(axis='x', length=0, pad=10)
ax.set_title('Dot Plot of Random Integer Values')
plt.show()
```

图 2 是通过代码创建的。很容易注意到在 21 和 24 之间存在一个间隙，该间隙被样本中最高浓度的值所包围。聪明的数据分析师应该探究这种特殊值分布的原因。

![](img/e0265f4407125ea522f221d72cbfe9a1.png)

图 2：作者使用 Matplotlib 制作的点图。

**茎叶图**

这种**表格图**在 1900 年左右非常流行，并在**约翰·W·图基**在普林斯顿大学的讲座中被重新发现（3）。这种图表之所以得名，是因为每个值被分为叶子和茎。

我们如何手动绘制茎叶图？我们首先确定数据的范围。然后，根据数据集的大小，我们将范围划分为固定长度的区间。接下来，我们绘制一条垂直线，将数字的前几位（千位、百位或十位）**除了最后一位** 按升序排列在垂直线的左侧。这就是茎。我们再次遍历数据集，将下一个显著的（最后）数字写在垂直线的右侧。这就是叶子。在最左侧，从底部到顶部，我们累计我们要绘制的值的数量。

例如，给定以下列表：[16, 25, 47, 56, 23, 45, 19, 55, 44, 27]，对应的茎叶图是（区间长度等于 10）：

![](img/6af5312c159354d46f2960c4aa446cba.png)

图 3：作者用 Stemgraphic 制作的茎叶图。

在图的底部，我们标出了值 16 和 19，然后是 23，再上一行放置 25 和 27。接着是 44，再上一行是 45 和 47，最后在两个单独的叶子中是 55 和 56。

Matplotlib 和 Plotly 都没有茎叶图。专门为此目的设计了一个名为**Stemgraphic**的 Python 模块。它支持任何大小的数据，并可以生成准备打印的图表（4）。

首先，你必须安装它：

*pip3 install -U stemgraphic*

*Stemgraphic* 需要 docopt、Matplotlib 和 pandas。可选地，安装 Scipy 会提供次级图表（5）。

假设我们有来自两个信息科学工程课程的 IQ 数据。让我们使用*stemgraphic*创建一个茎叶图来分析这些数据的分布。

```py
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import Stemgraphic

data_IQ1 = [111, 85, 83, 98, 107, 101, 100, 94, 101, 86,105, 122, 104, 106, 90, 123, 102, 107, 93, 109]
data_IQ2 = [146, 81, 91, 88, 98, 121, 83, 105, 97, 116, 93, 105, 108, 87, 104, 120, 109, 108, 107, 75, 93, 108, 104]

data_IQ = data_IQ1 + data_IQ2
df_IQ = pd.DataFrame(data_IQ, columns = ["IQ"])

stemgraphic.stem_graphic(df_IQ['IQ'])

print('')
print(df_IQ.describe())
print("")

print(df_IQ.agg({

        "IQ": ["count", "min", "max", "mean", "median", "skew"],

                })
)
```

![](img/5eff02152b6783253aab77d7982898db.png)

图 4：作者用 Stemgraphic 制作的茎叶图。

![](img/8dcf59cd838ffb617e08f3bd5514c433.png)

图 5：作者制作。

我们可以清晰地观察到两个离群值，一个在下端（75），另一个在排名顶部（146）。图形还表明中位数在 104 和 105 之间。数据呈适度偏斜。最左边的列包含累计计数。**茎叶图的主要优点是我们可以通过观察图表轻松重建源数据。**

# **直方图**

也叫柱状图，它们是数据集分布的图形表示。它们是二维图，具有两个坐标轴：水平轴被划分为**箱子**（数值范围的区间）；垂直轴是频率轴，其值来源于每个箱子的计数。每个箱子的频率通过垂直矩形条的面积显示。

直方图提供了连续定量变量分布的视觉总结。你可以推断数据集的位置、分布、对称性和偏斜程度。你还可以注意到是否存在集群、间隙和离群值。

你可以在我之前的 Medium 文章中找到更多理论概念和几个示例，关于直方图的内容：1) [直方图，为什么以及如何。讲故事、技巧和扩展](https://medium.com/@dar.wtz/histograms-why-how-431a5cfbfcd5)；2) 使用 Plotly Express 的直方图，主题和模板。

我在与蒙特卡洛模拟相关的几篇文章中使用了 Matplotlib 来绘制直方图。

例如，在 [蒙特卡洛模拟。第二部分](https://medium.com/p/8db846f3d8ed#09a4-6a25f853d2de) 中，我使用了以下代码来显示图 6：

```py
fig, ax = plt.subplots(figsize=(8, 6))
ax.hist(list_of_costs,  histtype ='bar', bins=20, color = 'c',
        edgecolor='k', alpha=0.65, density = True)  #density=True show probability

ax.axvline(media, color='g', linestyle='dashed', linewidth=3)
ax.axvline(Sum_most_like, color='r', linestyle='dashed', linewidth=3)
ax.text(48,0.185, 'Mean - - -', color = 'green'  )
ax.text(48,0.155, 'Most Like - - -', color = 'red'  )
ax.set_title('Frequency Chart')
ax.set_ylabel('Probability')
ax.set_xlabel('Total Cost (MM*1000 U$S)')
ax.grid(axis = 'y')
```

![](img/c6becef1ba81e5c3f4ceb5804fd463f6.png)

图 6：作者用 Matplotlib 制作的直方图。

在 蒙特卡洛模拟。第三部分 中，我使用了以下代码来显示图 7 中所示的直方图：

```py
fig, ax = plt.subplots(figsize=(8, 6))
ax.hist(list_cost_TTR,  histtype ='bar', bins=20, color = 'c',
         edgecolor='k', alpha=0.65, density = False)  # density=False show counts

ax.axvline(media, color='g', linestyle='dashed', linewidth=3)
ax.axvline(median, color='r', linestyle='dashed', linewidth=3)

ax.set_title('Frequency Chart')
ax.set_ylabel('Counts')
ax.set_xlabel('U$S')
ax.grid(axis = 'y')
```

![](img/963831c39d7e29f35e4ab8e24e75e098.png)

图 7：作者用 Matplotlib 制作的直方图。

# **结论**

约翰·图基曾表示，探索性数据分析就像是数字侦探工作。他试图解释说，数据中的统计结构是建立模型或理论之前的基础，这些模型或理论可以解释生成存储在数据集中的数字的过程。

每个数据分析师都必须具备某些可视化工具，用于分析和展示数据。特别是，对于单一的定量变量。

在本文中，我描述了三种简单的基于 Python 的探索性图表，用于可视化单一定量变量的分布：点图；茎叶图；直方图。

在三个图形之间选择的主要标准是数据集的大小。建议点图不应显示超过 50 个值。还建议茎叶图不应显示超过 300 个数值。相反，直方图在所表示的数值增加时会得到改善。在前面提到的文章中（与蒙特卡洛模拟相关），这些直方图总结了来自 5000 次复制（5000 个数值）的模拟运行获得的信息。

**参考文献**

(1) [`stackoverflow.com/users/14148248/patrick-fitzgerald`](https://stackoverflow.com/users/14148248/patrick-fitzgerald)

(2) [`stackoverflow.com/questions/49703938/how-to-create-a-dot-plot-in-matplotlib-not-a-scatter-plot`](https://stackoverflow.com/questions/49703938/how-to-create-a-dot-plot-in-matplotlib-not-a-scatter-plot)

(3) Tukey, John. “Exploratory Data Analysis”, Addison-Wesley, 1977

(4) [`stemgraphic.org/doc/modules.html`](http://stemgraphic.org/doc/modules.html)

(5) [`pypi.org/project/stemgraphic/`](https://pypi.org/project/stemgraphic/)
