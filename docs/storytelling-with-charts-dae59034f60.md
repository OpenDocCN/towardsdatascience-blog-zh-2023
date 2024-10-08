# 图表讲故事

> 原文：[`towardsdatascience.com/storytelling-with-charts-dae59034f60`](https://towardsdatascience.com/storytelling-with-charts-dae59034f60)

## 第二部分：你想展示数量吗？

[](https://medium.com/@dar.wtz?source=post_page-----dae59034f60--------------------------------)![达里奥·维茨](https://medium.com/@dar.wtz?source=post_page-----dae59034f60--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dae59034f60--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dae59034f60--------------------------------) [达里奥·维茨](https://medium.com/@dar.wtz?source=post_page-----dae59034f60--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dae59034f60--------------------------------) ·8 分钟阅读·2023 年 4 月 3 日

--

![](img/59abc501c8b14f358548a31674a610fb.png)

图片由 [Claudio Schwarz](https://unsplash.com/@purzlbaum?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据可视化的第一个规则是：“数据可视化是一种沟通工具”。它是讲述商业、商业、科学、学术和创业环境中的故事的最佳工具。

但请记住，你总是有一个观众。也许这个观众包括你的老板、同事、客户、员工、公务员，或者你自己。

在任何情况下，关键问题是：我是否清晰地传达了信息？

下一个问题是：我是否选择了正确的图表来讲述我的故事？

在 [本系列的第一篇文章](https://medium.com/towards-data-science/storytelling-with-charts-23dd41096721) 中，我们介绍了三种基于 Python 的探索性图表，允许可视化单一定量变量的分布。在本文（以及接下来的几篇文章中），我们将建议根据要传达给观众的信息的性质选择最合适的图表。

# **信息 1：展示数量**

这个信息包括展示某一组数字的大小：销售额、销售的项目数量、缴纳的税款、制造的物品、生产的项目、完成的程度、人口数据、性能数据、商业智能仪表板中的关键绩效指标（KPIs）等。

当要展示的数据只有一项（一个数值度量）时，通常会附带其他指标来添加额外的信息并增强信息的传达效果。

常用的数字图形表示图表如下：

+   数值指标

+   角度指标

+   子弹图

+   图示符号

+   柱状图

+   横向条形图

# **数值指标**

它们是定制的小部件，用于显示一个或两个数值，通常伴有标题和副标题、一些注释以及趋势指示器。它们经常用于在商业智能仪表盘中显示关键绩效指标（KPI）。

![](img/9ea5e58f9429ff8f0b614633305b8621.png)

图 1：作者使用 Plotly 制作。

[Plotly](https://plotly.com/python/)包括一个名为*Indicator*的图形，具有几种模式。对于上述图中的数字指示器，[Plotly trace](https://plotly.com/python/indicator/)是*go.Indicator()*，对应的参数有：*mode = “number+delta”* 用于可视化数值量以及该数值与参考值之间的差异；*value* 用于指示要显示的数值；*domain* 用于指示图形的位置；*title* 用于数值上方的文本。*delta* 有几个属性：*reference* 是要比较的值；*relative = False* 表示数量与参考值之间的差异必须以绝对方式计算；*position* 确定相对于数值的位置；*valueformat* 编码差异的数值格式。*mode = “delta”* 仅显示数量与参考值之间的差异。

```py
import plotly.graph_objects as go

# defining the path where the charts will be stored.  
path  =  your_path

fig = go.Figure()

fig.add_trace(go.Indicator(
    mode = "number+delta",
    value = 300,
    delta = {'reference': 380, 'relative': False,
             "valueformat": "0.0f"},
    title = {'text': "Total Revenue (MU$S)", 'font': {'size': 32}},
    domain = {'x': [0.25, 0.75], 'y': [0.5, 1.0]}
    ))

fig.add_trace(go.Indicator(
    mode = "delta",
    value = 450,
    delta = {'reference': 350, 'relative': False,
             'position' : "bottom", "valueformat": "0.0f"},
    title = {'text': "Total Quantities (units)", 'font': {'size': 32}},
    domain = {'x': [0.25, 0.75], 'y': [0.0, 0.4]}
    ))

fig.update_layout(paper_bgcolor = "lightblue")

fig.write_image(path + 'figIndic1.png')
fig.show() 
```

# **角度指示器**

最著名的角度指示器是**标准仪表图**。这是一种非常简单的图表，它提供了有关单一数值测量的准确信息，允许将显示的值与目标值进行比较，并与由不同颜色带表示的数值范围进行对比。目标值可以是基准、需要超越的前一个值，或是需要达到的目标。

标准仪表图类似于汽车的速度计。它有一个**径向数值刻度**，分为几个部分，每个部分由特定的颜色标识。它们还显示了下限和上限值，特别是目标值。当前值也用指针或指示针标示，或者用**弯曲条**在径向数值刻度上移动。最常见的配置有三个部分及其对应的颜色 [红色、黄色、绿色]。通常红色表示差、警告或低性能；黄色表示一般、警示或正常性能；绿色表示好、满意或高性能。红色还意味着数值测量低于下限，黄色表示数值测量在阈值之间，绿色则表示高于上限。

以下图示表明当前销售值（45）处于平均范围内，但比预定目标低 25%。

![](img/fd072dee898fe8b3eee164f19b50b2d5.png)

图 2：作者使用 Plotly 制作的标准仪表图。目标为 60。阈值为 25 和 50。

图 2 是用以下 Python 代码制作的。更多细节可以在我的[上一篇文章](https://medium.com/towards-data-science/indicators-with-plotly-85a543f002cd)中找到。

```py
import plotly.graph_objects as go

path  =  your_path

val   = 45
title = 'Sales (MU$S)'

fig1  = go.Figure(go.Indicator(
    domain = {'x': [0, 1], 'y': [0, 1]},

    title = {'text': title, 'font_size' : 40},
    value = val,  
    number= {'font_size' : 50},
    mode  = "gauge + number",

    gauge = { 'shape' : 'angular',
              'steps' : [
                        {'range': [0, 25],  'color': "red"},
                        {'range': [25, 50], 'color': "yellow"},
                        {'range': [50, 100],'color': "lightgreen"}
                        ],
             'bar' : {'color' : "black", 'thickness': 0.5},
             'threshold' : {'line': {'color': "orange", 'width':8},
                            'thickness': 0.8, 'value': 60},
             'axis': {'range': [None, 100], 'tickformat' : '$'},
             }
                 ))

fig1.add_annotation(x=0.5, y=0.4, text="-25%", 
                    font=dict(size = 30, color="darkred"),
                    showarrow=False)
# Add a circle
fig1.add_shape(type="circle", x0=0.4,  y0=0.3, x1=0.6, y1=0.5,
               line_color="purple")

fig1.update_layout(paper_bgcolor = "lightblue")

fig1.write_image(path + 'figIndic1.png')
fig1.show()
```

# **子弹图**

从概念上讲，它们类似于标准仪表图。它们显示关于单一定量测量的信息，将其当前值与目标值进行比较，并与由不同颜色带表示的一组数值范围进行对比。在视觉上，它们类似于标准条形图（水平或垂直），因为它们也通过矩形条的长度或高度来编码信息。主要的区别在于，子弹图包括一个中央狭窄的条，表示报告的数值测量的当前值。

![](img/fb4414fd1da572d02da64b194c332547.png)

图 3：作者使用 Plotly 制作的子弹图。目标是 450。阈值为 250 和 350。

目标值由 450 处的红色垂直线表示。类似于仪表图，有多个扇区（从三个到五个），用颜色或单一色调的不同强度来表示。扇区指示定性值，如[差，满意，良好]，[警报，警告，满意]，[差绩效，平均绩效，高绩效]。扇区也可以表示与显示的数值相关的不同数值范围。

子弹图相较于仪表图的主要优点是它们占用的屏幕空间较少，没有分散注意力的装饰物，并且最重要的是，其较小的尺寸允许你在单一图表中比较多个不同的类别。

更多细节和 Python 代码可以在我的[上一篇文章](https://medium.com/towards-data-science/indicators-with-plotly-85a543f002cd)中找到。

# **象形图**

又称：象形图，图示单位图，图片图表。

象形图使用图标显示离散的数据集。通常，图标代表与所示数据相关的主题类别；例如，人口数据使用人物图标（见下图）。每个图标可以代表一个单位或其他数量（如 10，100，1000）。数据集通过图标的列或行的接近程度进行比较。

![](img/9b7120db1524348f3472a9d33c2b6bf0.png)

由[Edward Howell](https://unsplash.com/@edwardhowellphotography?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

使用图标有时有助于**克服语言、文化和教育水平的差异**。观众通常很容易理解它们**。** 图标还可以提供数据的更具代表性和全面的视图。

在象形图中，始终避免显示大量图标，这可能会使显示的值难以计数。也要避免部分图标，这可能会使观众感到困惑并影响信息的传达。

# **柱状图**

又称：柱形图，垂直条形图

这是特别类型的垂直标准条形图。它们有两个坐标轴：水平轴显示类别，垂直轴显示数量。每个类别的数量通过垂直矩形条的高度显示。每条的高度与要显示的数值成比例。每条代表一个类别，并且它们之间留有一些空隙。

![](img/9025a411ad5c2ecc3da1f08901d7ce1c.png)

图 5：由作者使用 Matplotlib 制作的柱形图。

虽然柱形图的经典用途通常是进行比较、显示排名或指示时间趋势，但它们**对于简单展示数值也是非常有用的。**这些数值通过每个矩形的最终高度进行编码。柱形图也适用于显示负数值。

通过在条形图内或条形图上方使用注释，可以大大提高信息的清晰度。显示网格线也很方便，但仅限于水平网格线。

![](img/8a9ce3908dfe8f8421e1cba033aa125c.png)

图 6：由作者使用 Plotly 制作的柱形图。

好的实践建议将垂直轴从 0 开始，因为如果基线被修改，我们不可避免地会扭曲视觉效果。还必须避免所有 3D 效果，因为这些效果违反了适当讲述的所有规则。最后，必须避免使用圆角而不是锐角矩形，因为这会使最终数值的读取变得困难。

始终记住，最多 10% 的男性观众可能有色觉障碍。在这方面，尽量**不要在同一图表中使用绿色和红色条形。** 色盲症是一种缺乏绿色感知锥体的情况，而红盲症则是缺乏红色感知锥体的情况。

![](img/4d629d6fd3868f93eab700b83cd26444.png)

图 7：由作者使用 Plotly 制作的柱形图。

# **水平条形图**

这是特别类型的水平标准条形图。它们有两个坐标轴：水平轴显示数量，垂直轴显示类别。每个类别的数量通过垂直矩形条的长度显示。每条的长度与要显示的数值成比例。每条代表一个类别，并且它们之间留有一些空隙。

![](img/deb4497293798beb0cf0aa518427c8a9.png)

图 8：由作者使用 Matplotlib 制作的水平条形图。

通过在条形图内或条形图右侧使用注释，可以大大提高信息的清晰度。显示网格线也很方便，但仅限于垂直网格线。

在绘制多个类别时，尤其是带有非常长标签的类别时，水平条形图更为合适。

![](img/3e641c051654ad3346ee93b04a7210c5.png)

图 9：由作者使用 Plotly 制作的水平条形图。

正如之前在柱状图描述中提到的，你必须将数值轴从 0 开始，避免所有的 3D 效果，并且避免使用圆角边缘。同时，还要考虑红绿色盲的问题。

数据可视化是将某种信息转化为视觉上下文的过程。它是向多样化的受众讲述故事的强大工具。为此，提供了大量的图表、图形和示意图。完成任务的关键步骤是选择合适的图表来讲述你的故事，这取决于你想传达的信息。

在这篇文章中，我们介绍了六种不同的图表 [数值指示器、角度指示器、子弹图、图标图、柱状图和水平条形图]，这些图表旨在展示一个或几个数值，通常还附带注释和趋势指示器。我们介绍了优点，一些良好的实践，警告以及几个 Python 代码。

明智地选择，但始终问自己：**我是否在清晰地沟通？**
