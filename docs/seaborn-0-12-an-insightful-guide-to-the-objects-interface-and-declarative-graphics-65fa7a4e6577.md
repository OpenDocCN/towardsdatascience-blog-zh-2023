# Seaborn 0.12：对象接口和声明式图形的深度指南

> 原文：[`towardsdatascience.com/seaborn-0-12-an-insightful-guide-to-the-objects-interface-and-declarative-graphics-65fa7a4e6577`](https://towardsdatascience.com/seaborn-0-12-an-insightful-guide-to-the-objects-interface-and-declarative-graphics-65fa7a4e6577)

## [PYTHON 工具箱](https://medium.com/@qtalen/list/python-toolbox-4289824c6407)

## 利用 Python 的流行库简化数据可视化之旅

[](https://qtalen.medium.com/?source=post_page-----65fa7a4e6577--------------------------------)![Peng Qian](https://qtalen.medium.com/?source=post_page-----65fa7a4e6577--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65fa7a4e6577--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----65fa7a4e6577--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----65fa7a4e6577--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----65fa7a4e6577--------------------------------) ·14 分钟阅读·2023 年 8 月 24 日

--

![](img/d930c9fdcb0a9730a8836e89e069c8c6.png)

图片来源：作者创作，[Canva](https://www.canva.com/)

本文旨在介绍 Seaborn 0.12 中的对象接口功能，包括声明式图形语法的概念，并通过一个实用的可视化项目展示对象接口的用法。

在本文结束时，你将清楚了解 Seaborn 对象接口 API 的优点和局限性。你将能够更轻松地使用 Seaborn 进行数据分析项目。

# 介绍

想象一下，你正在使用 Python 创建数据可视化图表。

你必须逐步指示计算机：选择数据集、创建图形、设置颜色、添加标签、调整大小等等……

然后你会发现你的代码越来越长，越来越复杂，而你只想快速可视化你的数据。

这就像去超市时，你不得不指定每个物品的地点、颜色、大小和形状，而不是直接告诉店员你需要什么。

这不仅费时，还可能让人感到疲惫。

然而，Seaborn 0.12 的新功能——对象接口——以及其声明式图形语法的使用，就像有一个理解你的店员。你只需告诉它你需要做什么，它会为你找到一切。

你不再需要逐步指示它。你只需告诉它你想要什么样的结果。

在这篇文章中，我将指导你如何使用对象接口，这一新功能使得你的数据可视化过程更加轻松、灵活且愉悦。让我们开始吧！

# Seaborn API：过去与现在

在深入了解对象接口 API 之前，让我们系统地查看 Seaborn 早期版本和 0.12 版本之间的区别。

## 原始 API

许多读者在学习 Python 数据可视化时可能会被 Matplotlib 复杂的 API 文档吓到。

Seaborn 通过封装和简化 Matplotlib 的 API 来简化这一过程，使学习曲线变得更平缓。

Seaborn 不仅提供了对 Matplotlib 的高级封装，还将所有图表分类为关系图、分布图和类别图。

![](img/b0613dd60f527fe38d6b1ecb4e48af2f.png)

Seaborn 原始 API 设计概述。图片作者提供

你应该通过这个图表全面理解 Seaborn 的 API，并知道何时使用哪种图表。

例如，一个表示数据分布的`histplot`将归入分布图类别。

相比之下，一个按类别表示数据特征的`violinplot`将被归类为类别图。

除了垂直分类，Seaborn 还进行水平分类：`Figure-level` 和 `axes-level`。

根据[官方网站](https://seaborn.pydata.org/tutorial/function_overview.html?ref=dataleadsfuture.com#figure-level-vs-axes-level-functions)，`axes-level` 图表绘制在 `matplotlib.pyplot.axes` 上，只能绘制一个图形。

相比之下，`Figure-level` 图表使用 Matplotlib 的 `FacetGrid` 在一个图形中绘制多个图表，便于轻松比较相似的数据维度。

然而，即使 Seaborn 的 API 通过封装 Matplotlib 显著简化了图表绘制，创建特定的图表仍然需要复杂的配置。

例如，如果我使用 Seaborn 内置的 `penguins` 数据集绘制 `histplot`，代码如下：

```py
sns.histplot(penguins, x="flipper_length_mm", hue="species");
```

![](img/99fa47414e9ad030e4014d41520544de.png)

原来的绘制 histplot 的方式。图片作者提供

当我使用相同的数据集绘制`kdeplot`时，代码如下：

```py
sns.kdeplot(penguins, x="flipper_length_mm", fill=True, hue="species");
```

![](img/ce9c2bfcf427ed4228dcd1a4e867be4b.png)

原来的绘制 kdeplot 的方式。图片作者提供

除了图表 API，其余的配置都是相同的。

这就像告诉厨师我想用羊排和洋葱做一锅羊肉汤，并且指定烹饪步骤。当我想用这些食材做烤羊排时，我必须重新告诉厨师所有的食材和烹饪步骤。

这样不仅效率低下，还需要更多的灵活性。

这就是为什么 Seaborn 在 0.12 版本中引入了对象接口 API。这种声明性图形语法显著改善了图表创建过程。

## 对象接口 API

在开始使用对象接口 API 之前，让我们先从高层次了解一下它，以便更好地理解绘图过程。

与按分类组织绘图 API 的原始 Seaborn API 不同，对象接口 API 按绘图流程收集 API。

对象接口 API 将绘图分为多个阶段，如数据绑定、布局、展示、定制等。

![](img/e9a6a8234d10e055f5e1ba1d39eea83a.png)

Seaborn 对象接口 API 设计概述。图像由作者提供

数据绑定和展示阶段是必要的，而其他阶段是可选的。

另外，由于各个阶段是独立的，每个阶段都可以被重用。继续之前的 hist 和 kde 图的例子：

要使用对象接口绘图，我们首先需要绑定数据：

```py
p = so.Plot(penguins, x="flipper_length_mm", color="species")
```

从这行代码中，我们可以看到对象接口使用`so.Plot`类进行数据绑定。

另外，相较于使用难以理解的`hue`参数的原始 API，它使用`color`参数将`species`维度直接绑定到图表颜色，使配置更直观。

最后，这行代码返回一个可以重用以绘制图表的`p`实例。

接下来，让我们绘制一个`histplot`：

```py
p.add(so.Bars(), so.Hist())
```

![](img/4f9f6ead980a48f039a81dec19b1a9fc.png)

使用对象接口 API 绘制 histplot。图像由作者提供

这行代码表明绘图阶段不需要重新绑定数据。我们只需告诉`add`方法绘制什么：`so.Bars()`，以及如何计算：`so.Hist()`。

`add`方法还返回`Plot`实例的副本，因此`add`方法中的任何调整不会影响原始数据绑定。`p`实例仍然可以重用。

因此，我们继续调用`p.add()`方法来绘制一个`kdeplot`：

```py
p.add(so.Area(), so.KDE())
```

![](img/14137c191e25355e9ee14696fbf11866.png)

使用对象接口 API 绘制 kdeplot。图像由作者提供

由于`KDE`是一种统计方法，这里在`stat`参数上调用了`so.KDE()`。而且由于`kdeplot`本身是一个区域图，因此使用`so.Area()`进行绘制。

我们重用了绑定到数据的`p`实例，所以不需要告诉厨师如何烹饪每道菜，而是直接说我们想要什么。这样不是更加简洁和灵活吗？

# 用示例解构对象接口

接下来，看看如何使用原始 Seaborn API 和对象接口 API 编写一些常见的图表。

在开始之前，我们需要导入必要的库：

```py
%matplotlib inline

import matplotlib.pyplot as plt
import seaborn as sns
import seaborn.objects as so

import pandas as pd

sns.set()
penguins = sns.load_dataset('penguins')
```

## 条形图

在原始 API 中，绘制条形图的代码如下：

```py
sns.barplot(penguins, x="island", y="body_mass_g", hue="species");
```

![](img/d16e4abd660cfb2cccefee6a81f28990.png)

原始的条形图绘制方式。图像由作者提供

在对象接口中，绘制条形图的代码如下：

```py
(
    so.Plot(penguins, x="island", y="body_mass_g", color="species")
    .add(so.Bar(), so.Dodge())
)
```

![](img/3ce53f3575c57d6aa684c9f9b1bce1b5.png)

使用对象接口绘制条形图。图像由作者提供

## 散点图

在原始 API 中，绘制散点图的代码如下：

```py
sns.relplot(penguins, x="bill_length_mm", y="bill_depth_mm", hue="species");
```

![](img/9e7217563e8f3b81a81ee5e5a3a5067c.png)

在原始方式中，我们使用 relplot 绘制散点图。图像由作者提供

在对象接口中，绘制散点图的代码如下：

```py
(
    so.Plot(penguins, x="bill_length_mm", y="bill_depth_mm", color="species")
    .add(so.Dots())
)
```

![](img/5c6083c06f7af66107f1c15b0141c04b.png)

使用对象接口时，我们使用 so.Dots 绘制散点图。图像由作者提供

你可能会觉得在比较了两个 API 的绘图后，对象接口似乎也没有特别之处。

不用担心。让我们看看对象接口的高级用法。

## 高级用法

假设我们使用 Seaborn 的 `tips` 数据集。

```py
tips = sns.load_dataset("tips")
```

我想使用条形图来查看不同日期的平均小费，并在图表上标记这些值。

我想要的图表如下所示：

![](img/fc50e7959f39167f59840dbcc27ef8cc.png)

一个带有文本的条形图以显示这些值。图像由作者提供

在我们开始绘图之前，需要处理 `tips` 数据集，以计算每一天的平均值。

```py
day_mean = tips[['day', 'tip']].groupby('day').mean().round(2).reset_index()
```

然后，我们可以使用对象接口绘制：

```py
(
    day_mean
    .pipe(so.Plot, y="day", x="tip", text="tip")
    .add(so.Bar(width=.5))
    .add(so.Text(color='w', halign="right"))
)
```

我们在这里使用了两个技巧：

首先，我们在 `dataframe` 上调用 `pipe` 方法，以启用链式代码调用。

其次，我们可以重用 `so.Plot` 的实例，只需绑定数据一次即可绘制多个图形。

那么，让我们看看使用原始 API 编写的代码是怎样的：

```py
ax = sns.barplot(day_mean, x="tip", y="day")

for p in ax.patches:
    width = p.get_width()
    ax.text(width,
            p.get_y() + p.get_height()/2,
            '{:1.2f}'.format(width),
            ha="right", va="center")
plt.show()
```

如你所见，原始代码复杂得多：

首先，绘制一个水平条形图。

然后使用迭代在每个条形上绘制相应的值。

相比之下，对象接口是不是显得更简单和灵活？

# 将对象接口应用于实际数据

接下来，为了帮助大家加深记忆并系统掌握对象接口的用法，我计划带领大家在实际的数据可视化项目中进行练习。

在这个项目中，我计划直观地探索纽约市共享自行车系统的数据，以了解该市共享自行车的使用情况，并帮助企业更好地运营。

## 数据来源

我们将在这个项目中使用 Citibikenyc 的 Citi Bike Sharing 数据集。

你可以在这里找到数据集：[`citibikenyc.com/system-data`](https://citibikenyc.com/system-data)

这个数据集的许可证可以在 [这里](https://ride.citibikenyc.com/data-sharing-policy) 找到。

为了方便后续编码过程，我清理并合并了数据集中的数据，最终合成了一个数据集。

## 数据预处理

在我们开始之前，我们应当了解数据集中包含的字段，这可以通过执行以下代码来实现：

```py
citibike = pd.read_csv("../data/CitiBike-2021-combined.csv", index_col="ID")
citibike.info()
```

```py
Data columns (total 15 columns):
 #   Column                   Non-Null Count   Dtype         
---  ------                   --------------   -----         
 0   Trip Duration            735502 non-null  int64         
 1   Start Time               735502 non-null  datetime64[ns]
 2   Stop Time                735502 non-null  datetime64[ns]
 3   Start Station ID         735502 non-null  int64         
 4   Start Station Name       735502 non-null  object        
 5   Start Station Latitude   735502 non-null  float64       
 6   Start Station Longitude  735502 non-null  float64       
 7   End Station ID           735502 non-null  int64         
 8   End Station Name         735502 non-null  object        
 9   End Station Latitude     735502 non-null  float64       
 10  End Station Longitude    735502 non-null  float64       
 11  Bike ID                  735502 non-null  int64         
 12  User Type                735502 non-null  object        
 13  Birth Year               735502 non-null  int64         
 14  Gender                   735502 non-null  object           
dtypes: datetime64ns, float64(4), int64(8), object(6)
memory usage: 117.8+ MB
```

这个数据集包含 15 个字段，由于我们的目标是了解城市共享自行车的使用情况，所以所有 15 个字段对我们都很有帮助。

此外，为了便于分析每年不同月份以及每周工作日和非工作日的共享自行车使用情况，我需要为数据集生成两个字段：`Start Month` 和 `Day Of Week`：

```py
citibike['Start Time'] = pd.to_datetime(citibike['Start Time'])
citibike['Stop Time'] = pd.to_datetime(citibike['Stop Time'])

citibike['Day Of Week'] = citibike['Start Time'].dt.day_of_week
citibike['Start Month'] = citibike['Start Time'].dt.month
day_dict = {0: 'Mon', 1: 'Tue', 2: 'Wen', 3: 'Thu', 4: 'Fri', 5: 'Sat', 6: 'Sun'}
citibike['Day Of Week'] = citibike['Day Of Week'].replace(day_dict)
```

为了方便展示，我将 `Gender` 字段转换为文本性别，将 `Birth Year` 转换为 `Decade`，并将 `Trip Duration` 从秒转换为分钟：

```py
citibike['Gender'] = citibike['Gender'].replace({0: 'Unknown', 1: 'Male', 2: 'Female'})
citibike['Decade'] = (citibike['Birth Year'] // 10 * 10).astype(str) + 's'
citibike['Duration_Min'] = citibike['Trip Duration'] // 60
```

最后，由于原始数据集很大，我们只需找出数据的分布，因此我将对数据集进行抽样，以便更快更轻松地绘图：

```py
citibike_sample = citibike.sample(n=10000, random_state=1701)
```

## 可视化分析

**记住，数据可视化的目的不仅仅是展示数据，而是挖掘数据背后的故事。**

在这个项目中，我期望了解用户在什么情况下会使用共享自行车，以便优化自行车分布或进行相应的推广。

首先，我想了解在不同季节，人们更倾向于使用共享自行车。

由于我想查看按月的数据总量，我直接使用原始数据集进行绘图。

为了加快绘图速度，我在`dataframe`中汇总数据，然后使用`pipe`方法调用管道。

```py
(
    citibike.groupby('Start Month').size().reset_index(name="Count")
    .pipe(so.Plot, x="Start Month", y="Count")
    .add(so.Line(marker='o', edgecolor='w'))
    .add(so.Text(valign='bottom'), text='Count')
)
```

![](img/f233f521a535ff1456aaf691993dfb66.png)

按月查看共享自行车使用情况。图片来源：作者

图表显示，每年的三月和十月自行车的使用次数更多。这表明人们在气候温和的时候更愿意骑自行车。

接下来，我想了解人们在一周中的哪些天使用共享自行车更多。

由于我们这里只需要查看比例，我使用了采样数据集，并在`so.Hist()`中设置了一个`proportion`。

```py
(
    so.Plot(citibike_sample, x="Day Of Week", color="Gender")
    .scale(x=so.Nominal(order=['Mon', 'Tue', 'Wen', 'Thu', 'Fri', 'Sat', 'Sun']))
    .add(so.Bar(), so.Hist(stat="proportion"), so.Dodge())
)
```

![](img/1d10a475bc814621af62ba0ad91bc3bc.png)

一周中的哪些天人们使用共享自行车更多？图片来源：作者

男性和女性在工作日使用共享自行车的频率更高，这可能是为了通勤。

但我们也发现‘未知’性别的用户在周末使用共享自行车的频率更高。

为什么会这样？我们可以继续探讨。

接下来，我想了解在不同性别情况下骑行时长的比例。

在这里，我将为每个性别分别绘制直方图，并使用`facet`进行布局。

为了消除异常数据带来的干扰，我只参考了平均骑行时间一个标准差内的数据。

```py
mean = citibike_sample["Duration_Min"].mean()
std = citibike_sample["Duration_Min"].std()
citibike_filterd = citibike_sample.query("(Duration_Min > @mean - @std) and (Duration_Min < @mean + @std)")

(
    so.Plot(citibike_filterd, x="Duration_Min")
    .facet(col="Gender")
    .layout(size=(6,3))
    .add(so.Bars(), so.Hist(stat="proportion"))
)
```

![](img/3e30b6da446ee41ca4a1230e1d5eca22.png)

分别为每个性别绘制直方图以显示骑行时长的比例。图片来源：作者

图表显示，男性和女性的骑行时长符合我们的认知。

尽管如此，‘未知’性别用户的骑行时长似乎分布更均匀，这表明骑行更随意且缺乏目的。

第四步，我想了解按会员类别划分的骑行时长比例：

```py
(
    so.Plot(citibike_filterd, x="Duration_Min")
    .facet(col="Gender", row="User Type")
    .share(y=False)
    .add(so.Bars(), so.Hist(stat="proportion"))
)
```

![](img/e83102fd04064ce8d6d8c968746ba08a.png)

按会员类别划分的骑行时长比例。图片来源：作者

从图表中可以看出，对于会员用户，不论性别，骑行时长的分布更有目的性，倾向于短时间骑行以快速到达目的地。

对于普通用户，‘未知’性别的用户骑行时长更随意，骑行时间也较长。

看来这些用户是为了暂时骑上自行车欣赏风景？

因此，在第五步中，我想查看站点之间自行车使用时间的分布，以验证我的猜想。

由于图表中显示如此多的站点是不可能的，我首先按`Start Station ID`和`End Station ID`的计数对采样数据进行汇总。

```py
start_end_station = citibike_sample
                    .groupby(["Start Station ID", "End Station ID"])
                    .size().reset_index(name="Count")
```

同样，为了避免过多的数据点干扰我们的分析，我只取了前 20% 的数据进行绘图。

```py
p8 = start_end_station["Count"].quantile(.8)
start_end_filtered = start_end_station[start_end_station["Count"] >= p8]
```

然后使用散点图绘制数据，并用点的大小表示计数大小。

```py
(
    so.Plot(start_end_filtered, x="Start Station ID", y="End Station ID",
            pointsize="Count", color="Count")
    .add(so.Dots())
)
```

![](img/04ee29ec4ff0ed06003f30b9decb71b4.png)

车站之间的骑行分布。图片来源：作者

图表显示，骑行数量主要分布在 ID 值为 3180 和 3220 的车站之间。

与表格数据相比，这个区域主要集中在办公室工作人员。

在 3260 到 3280 的车站 ID 之间还有大量数据分布。

通过比较表格数据，我们可以看到这个区域有许多公园和旅游景点。

这证实了我们的猜测：除了平日倾向于骑共享单车的办公室工作人员外，许多游客愿意在周末骑共享单车出门游览风景。

因此，对于这个城市的共享单车运营部门，运营策略不仅要在工作日打折吸引会员多骑行。

他们还可以利用新的用户注册礼物或在周末推广更多应用内的景点，鼓励游客或临时用户成为会员用户。

# 发展空间：对象接口的当前局限性

在展示 Seaborn 对象接口如何帮助我们在实际项目中快速进行数据分析后，我想基于我的经验讨论一些对象接口需要改进的地方。

首先，绘图性能需要提升。

如上面项目所示，当我使用原始数据集绘图时，速度缓慢，Seaborn 没有利用 `Numpy` 或 `Python Arrow` 的计算能力。

其次，需要更多的文档。

许多 API 的具体用法我找不到介绍，只能慢慢摸索。

而且 API 设计目前给我的感觉还不够成熟。

例如，我认为 `so.Stat` 和 `so.Move` 应该放在数据映射阶段，但目前通过 add 方法放在了展示阶段，这需要修订。

最后，图表的选择需要更丰富。

我最初计划在城市共享单车项目中使用饼图和地图图表，但我找不到它们。

尽管我可以自己编写扩展，但那是另一回事。

同样，当我想更复杂地布局图表时，我需要使用 Matplotlib 的 `subplots` API 并将其与 `on` 方法集成，这仍然需要完全封装。

尽管存在这些不足，我对 Seaborn 的未来充满信心。

我认为团队选择声明式图形语法使得 Seaborn 更易用且更灵活。

我希望 Seaborn 社区在不久的将来变得更加活跃。

# 结论

在这篇文章中，我介绍了 Seaborn 0.12 中的对象接口功能。

通过引入声明式图形语法的好处，我让你理解为什么 Seaborn 团队选择以这种方式发展。

此外，为了迎合那些需要进一步熟悉 Seaborn 的读者，我介绍了原始 Seaborn 与对象接口版本在 API 设计哲学上的异同。

通过实际项目分析城市共享单车的使用情况，你亲眼见证了对象接口 API 的使用方式及我的期望。

永远记住，数据可视化的目标不仅仅是展示数据，而是揭示数据背后的故事。

希望你觉得这篇文章对你有帮助。如果你有任何问题或新想法，欢迎评论和参与讨论。我很乐意回答你的问题。

# 数据集使用权限：

1.  文章中使用的“企鹅”和“提示”数据集是包含在 seaborn 源代码中的示例数据集。Seaborn 是一个遵循[BSD 3-Clause “New”或“Revised”许可证](https://github.com/mwaskom/seaborn/blob/master/LICENSE.md)的开源软件，允许用于商业目的。

1.  你可以从这里下载 Citi Bike 旅行历史数据：[`citibikenyc.com/system-data`](https://citibikenyc.com/system-data)，并且许可证[`ride.citibikenyc.com/data-sharing-policy`](https://ride.citibikenyc.com/data-sharing-policy)允许我在文章中使用这些数据。

除了提高代码执行速度和性能外，使用各种工具提高工作效率也是一种性能提升：

![Peng Qian](img/fa6bd24b4781f623be8ea40c4e6bdb78.png)

[Peng Qian](https://qtalen.medium.com/?source=post_page-----65fa7a4e6577--------------------------------)

## Python 工具箱

[查看列表](https://qtalen.medium.com/list/python-toolbox-4289824c6407?source=post_page-----65fa7a4e6577--------------------------------)6 个故事！Seaborn 0.12: 透视对象接口和声明性图形的指南![用 Aiomultiprocess 提升你的 Python Asyncio 性能：全面指南](img/9c366de04067cd0ec1b30d9ce223011b.png)![用 Tenacity 征服 Python 中的重试：深入教程](img/e636d5546f5826d60865c6a95f976fa8.png)

感谢阅读我的故事。

你可以[**订阅**](https://www.dataleadsfuture.com/#/portal)以获取我最新的数据科学故事。

如果你有任何问题，可以在[LinkedIn](https://www.linkedin.com/in/qtalen/)或[Twitter(X)](https://twitter.com/qtalen)上找到我。

本文最初发布在[数据引领未来](https://www.dataleadsfuture.com/seaborn-0-12-an-insightful-guide-to-the-objects-interface-and-declarative-graphics/#/portal)。
