# 使用 Python 构建美丽条形图的 5 个步骤

> 原文：[`towardsdatascience.com/5-steps-to-build-beautiful-bar-charts-with-python-3691d434117a`](https://towardsdatascience.com/5-steps-to-build-beautiful-bar-charts-with-python-3691d434117a)

## 如何利用 Matplotlib 的全部功能讲述更具吸引力的故事

[](https://guillaume-weingertner.medium.com/?source=post_page-----3691d434117a--------------------------------)![Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----3691d434117a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3691d434117a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3691d434117a--------------------------------) [Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----3691d434117a--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3691d434117a--------------------------------) ·7 分钟阅读·2023 年 1 月 23 日

--

![](img/a5aac1b4be76c70f2fbd259728b8c68b.png)

2008 年美国国内航班的平均航空延误 — 作者提供的图像

# 动机

当支持这些故事的图表**清晰**、**自解释**且**视觉上令人愉悦**时，讲述一个引人入胜的故事变得容易得多。

在许多情况下，内容和形式同样重要。

数据再好但表现不佳将不会引起应有的关注，而表现出色的数据即使质量较差也容易被忽视。

Matplotlib 使得使用现成函数绘制数据变得快捷而简单，但微调步骤则需要更多的努力。

我花了不少时间研究使用 Matplotlib 构建引人注目的图表的最佳实践，以便你不必花费时间。

在本文中，我重点介绍了**条形图**，并解释了如何将我在各处找到的知识片段拼接在一起，从而达到如下效果…

![](img/ac695c7158e02d061dca9dd61eb1be34.png)

2008 年美国国内航班的平均航空延误 — 作者提供的图像

… 到那个：

![](img/a5aac1b4be76c70f2fbd259728b8c68b.png)

2008 年美国国内航班的平均航空延误 — 作者提供的图像

# #0 数据

为了说明方法论，我使用了一个关于 2008 年美国国内航班延误的公开数据集：

> 2008 年，“数据展览 2009：航空公司准时数据”， [`doi.org/10.7910/DVN/HG7NV7`](https://doi.org/10.7910/DVN/HG7NV7)，哈佛数据集，V1
> 
> 公有领域 [CC0 1.0](http://creativecommons.org/publicdomain/zero/1.0)

在导入了读取数据和构建图表所需的包后，我简单地按月份对数据进行了分组，并计算了平均延迟，使用了以下代码：

```py
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib as mpl
from matplotlib.ticker import MaxNLocator

df = pd.read_csv('DelayedFlights.csv')

df = df[['Month', 'ArrDelay']] # Let's only keep the columns useful to us
df = df[~df['ArrDelay'].isnull()] # Get rid of cancelled and diverted flights

# Group by Month and get the mean
delay_by_month = df.groupby(['Month']).mean()['ArrDelay'].reset_index() 
```

本文中用于构建不同版本条形图的数据集如下：

![](img/d6b1eafc16e8ab4721fdc6c137d2f0f9.png)

本文中使用的数据集 —— 图片由作者提供

# #1 基础图表

坦白说，使用两行代码，你已经可以构建一个条形图并从中获得一些基本的见解。

虽然这个图表既不最美观，也不最实用，因为缺少关键信息，但你已经可以看出，12 月份旅行很可能会导致航班延误。

```py
# Create the figure and axes objects, specify the size and the dots per inches 
fig, ax = plt.subplots(figsize=(13.33,7.5), dpi = 96)

# Plot bars
bar1 = ax.bar(delay_by_month['Month'], delay_by_month['ArrDelay'], width=0.6)
```

![](img/ac695c7158e02d061dca9dd61eb1be34.png)

Matplotlib 最基本的条形图 —— 图片由作者提供

# #2 基本要素

让我们在图表中添加一些重要元素，以使其更易于观众阅读。

+   **网格**

    为了提高可读性，图表的网格是必不可少的。它们的透明度设置为 0.5，以免过多干扰数据点。

+   **X 轴和 Y 轴重新格式化**

    我主动添加了比实际需要更多的参数，以获得对微调可能性的更全面的视角。例如，x 轴不需要 major_formatter 和 major_locator 对象，因为我们只是设置标签，但如果读者的 x 轴包含其他数字，那么这些可能会派上用场。

+   **条形标签**

    在每个条形上方添加条形标签，使得对接近数据点之间的比较更容易，并提供关于实际值的更多细节。

```py
# Create the grid 
ax.grid(which="major", axis='x', color='#DAD8D7', alpha=0.5, zorder=1)
ax.grid(which="major", axis='y', color='#DAD8D7', alpha=0.5, zorder=1)

# Reformat x-axis label and tick labels
ax.set_xlabel('', fontsize=12, labelpad=10) # No need for an axis label
ax.xaxis.set_label_position("bottom")
ax.xaxis.set_major_formatter(lambda s, i : f'{s:,.0f}')
ax.xaxis.set_major_locator(MaxNLocator(integer=True))
ax.xaxis.set_tick_params(pad=2, labelbottom=True, bottom=True, labelsize=12, labelrotation=0)
labels = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
ax.set_xticks(delay_by_month['Month'], labels) # Map integers numbers from the series to labels list

# Reformat y-axis
ax.set_ylabel('Delay (minutes)', fontsize=12, labelpad=10)
ax.yaxis.set_label_position("left")
ax.yaxis.set_major_formatter(lambda s, i : f'{s:,.0f}')
ax.yaxis.set_major_locator(MaxNLocator(integer=True))
ax.yaxis.set_tick_params(pad=2, labeltop=False, labelbottom=True, bottom=False, labelsize=12)

# Add label on top of each bar
ax.bar_label(bar1, labels=[f'{e:,.1f}' for e in delay_by_month['ArrDelay']], padding=3, color='black', fontsize=8) 
```

![](img/924f8adcc67ffc3c209f677fd5ce9f98.png)

在我们的图表中添加一些基本功能 —— 图片由作者提供

# #3 专业外观

在我们的图表中添加更多功能会使其看起来更加专业。这些功能可以叠加在任何图表上（不仅仅是条形图），并且与我们在本文中使用的数据无关。

感谢下面的代码片段，这些调整将几乎不需要任何额外的努力来实现。作者的建议：保存并随意重用。

读者可以调整这些参数来创建自己的视觉身份。

+   **坐标轴**

    坐标轴构成了图表周围可见的框架。它们被移除，除了右侧的坐标轴被设置得稍微粗一点。

+   **顶部的红线和矩形**

    在标题上方添加了一条红线和一个矩形，以很好地将图表与上方的文本隔离开来。

+   **标题和副标题**

    一个没有标题来介绍它的图表算什么呢？

    副标题可以用来进一步解释内容，甚至提出初步结论。

+   **来源** 在所有生产的图表中必不可少。

+   **边距调整**

    调整图表区域周围的边距，以确保充分利用所有可用空间。

+   **白色背景**

    设置白色背景（默认透明）在通过电子邮件、Teams 或其他工具发送图表时会非常有用，因为透明背景可能会造成问题。

```py
# Remove the spines
ax.spines[['top','left','bottom']].set_visible(False)

# Make the left spine thicker
ax.spines['right'].set_linewidth(1.1)

# Add in red line and rectangle on top
ax.plot([0.12, .9], [.98, .98], transform=fig.transFigure, clip_on=False, color='#E3120B', linewidth=.6)
ax.add_patch(plt.Rectangle((0.12,.98), 0.04, -0.02, facecolor='#E3120B', transform=fig.transFigure, clip_on=False, linewidth = 0))

# Add in title and subtitle
ax.text(x=0.12, y=.93, s="Average Airlines Delay per Month in 2008", transform=fig.transFigure, ha='left', fontsize=14, weight='bold', alpha=.8)
ax.text(x=0.12, y=.90, s="Difference in minutes between scheduled and actual arrival time averaged over each month", transform=fig.transFigure, ha='left', fontsize=12, alpha=.8)

# Set source text
ax.text(x=0.1, y=0.12, s="Source: Kaggle - Airlines Delay - https://www.kaggle.com/datasets/giovamata/airlinedelaycauses", transform=fig.transFigure, ha='left', fontsize=10, alpha=.7)

# Adjust the margins around the plot area
plt.subplots_adjust(left=None, bottom=0.2, right=None, top=0.85, wspace=None, hspace=None)

# Set a white background
fig.patch.set_facecolor('white')
```

![](img/ad040c67c94d53bcb93cd66782a93264.png)

我们的视觉身份应用到图表上，使其更整洁 —— 图片由作者提供

# #4 色彩渐变

我们在上一节中留下的图表干净整洁，准备好被纳入演示中。虽然对条形颜色进行渐变以更好地可视化变化并非必需，但会增加一个吸引人的特性。

这个用例的在线文档可能不是最好的，但实际上用 Matplotlib 的*LinearSegmentedColormap*和*Normalize*函数实现起来并不太难。

```py
# Colours - Choose the extreme colours of the colour map
colours = ["#2196f3", "#bbdefb"]

# Colormap - Build the colour maps
cmap = mpl.colors.LinearSegmentedColormap.from_list("colour_map", colours, N=256)
norm = mpl.colors.Normalize(delay_by_month['ArrDelay'].min(), delay_by_month['ArrDelay'].max()) # linearly normalizes data into the [0.0, 1.0] interval

# Plot bars
bar1 = ax.bar(delay_by_month['Month'], delay_by_month['ArrDelay'], color=cmap(norm(delay_by_month['ArrDelay'])), width=0.6, zorder=2)
```

![](img/3d34ed9be4d2f9e36c72a49e2c288b15.png)

仍然是相同的条形图，增加了额外的颜色特性——图片由作者提供

# #5 最后修饰

为了实现文章开头介绍的最终结果，剩下的工作就是实现这几个额外的组件：

+   **平均数据线**

    在图表上显示平均数据线是帮助观众快速了解情况的一种有效方式。

+   **第二种颜色刻度**

    通过第二种颜色刻度，我们突出了高于平均水平（或任何阈值）的数据，以便在短时间内更容易理解可视化内容。

+   **图例**

    当我们添加第二种颜色刻度时，我们在图表上引入了图例的需求。

```py
# Find the average data point and split the series in 2
average = delay_by_month['ArrDelay'].mean()
below_average = delay_by_month[delay_by_month['ArrDelay']<average]
above_average = delay_by_month[delay_by_month['ArrDelay']>=average]
```

```py
# Colours - Choose the extreme colours of the colour map
colors_high = ["#ff5a5f", "#c81d25"] # Extreme colours of the high scale
colors_low = ["#2196f3","#bbdefb"] # Extreme colours of the low scale

# Colormap - Build the colour maps
cmap_low = mpl.colors.LinearSegmentedColormap.from_list("low_map", colors_low, N=256)
cmap_high = mpl.colors.LinearSegmentedColormap.from_list("high_map", colors_high, N=256)
norm_low = mpl.colors.Normalize(below_average['ArrDelay'].min(), average) # linearly normalizes data into the [0.0, 1.0] interval
norm_high = mpl.colors.Normalize(average, above_average['ArrDelay'].max())

# Plot bars and average (horizontal) line
bar1 = ax.bar(below_average['Month'], below_average['ArrDelay'], color=cmap_low(norm_low(below_average['ArrDelay'])), width=0.6, label='Below Average', zorder=2)
bar2 = ax.bar(above_average['Month'], above_average['ArrDelay'], color=cmap_high(norm_high(above_average['ArrDelay'])), width=0.6, label='Above Average', zorder=2)
plt.axhline(y=average, color = 'grey', linewidth=3)

# Determine the y-limits of the plot
ymin, ymax = ax.get_ylim()
# Calculate a suitable y position for the text label
y_pos = average/ymax + 0.03
# Annotate the average line
ax.text(0.88, y_pos, f'Average = {average:.1f}', ha='right', va='center', transform=ax.transAxes, size=8, zorder=3)

# Add legend
ax.legend(loc="best", ncol=2, bbox_to_anchor=[1, 1.07], borderaxespad=0, frameon=False, fontsize=8)
```

![](img/a5aac1b4be76c70f2fbd259728b8c68b.png)

最终产品：图表易于阅读——图片由作者提供

# #6 最终思考

本文的目的是分享这里和那里的知识，以使用 Matplotlib 构建更具吸引力的条形图。我尽量使其尽可能实用，包含可重用的代码片段。

我相信还有其他调整是我没想到的。如果你有任何改进的建议，欢迎评论，让这篇文章对大家更有用！

本文仅关注于**条形图**，敬请期待更多内容！

*感谢你阅读到文章的最后！*

如果你有任何问题或意见，欢迎在下方留言，或者通过* [*LinkedIn*](https://www.linkedin.com/in/guillaume-weingertner-a4a27972/) *与我联系！*
