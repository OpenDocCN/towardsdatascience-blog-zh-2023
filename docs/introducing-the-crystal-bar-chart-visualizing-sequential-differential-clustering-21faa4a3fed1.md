# 介绍 Crystal Bar Chart：可视化序列差分聚类

> 原文：[`towardsdatascience.com/introducing-the-crystal-bar-chart-visualizing-sequential-differential-clustering-21faa4a3fed1`](https://towardsdatascience.com/introducing-the-crystal-bar-chart-visualizing-sequential-differential-clustering-21faa4a3fed1)

## 了解 Crystal Bar Charts 并使用 Python 创建你自己的图表

[](https://medium.com/@nickgerend?source=post_page-----21faa4a3fed1--------------------------------)![Nick Gerend](https://medium.com/@nickgerend?source=post_page-----21faa4a3fed1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----21faa4a3fed1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----21faa4a3fed1--------------------------------) [Nick Gerend](https://medium.com/@nickgerend?source=post_page-----21faa4a3fed1--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----21faa4a3fed1--------------------------------) ·阅读时间 12 分钟·2023 年 12 月 29 日

--

![](img/5de7f839aa55031ad3e4a9eec799d12c.png)

Crystal Bar Chart 作者**Nick Gerend**

# 介绍

有很多方法可以更好地理解一系列数据。无论是温度、考试成绩、食品价格、步数还是 UFO 目击事件，总能通过数据分析和可视化的视角学到关于这些事物的本质。

在接下来的内容中，我将描述一种新的数据可视化技术，它使用简单的基于阈值的聚类。我希望你会发现这是一种与传统可视化方法（如直方图和箱线图）相配的全新方法，从而对感兴趣的特征获得新的视角。

# 序列差分聚类

## 数值上

怎样才能脱颖而出？以单一数值属性为例，我们假设简单的阈值是将有序值进行比较并分组的标准。以以下序列为例：

[ 0 , 1 , 1 , 2 , 3 , 5 , 8 , 13 , 21 , 34 , 55 , 89 , 144 ]

这当然是斐波那契数列的前 13 个数字。如果我们从第 3 个位置开始，计算每个值与前一个值之间的差，我们会得到相同的序列，只是向前移动了 2 步：

[ _ , _ , 0 , 1 , 1 , 2 , 3 , 5 , 8 , 13 , 21 , 34 , 55 ]

现在，让我们设定一个阈值为 5，并假设在从左到右遍历序列时，任何大于 5 的数值都标志着一个新簇的开始。这将导致前 8 项（值为 0-13）被分到一起，而每个后续项由于所有剩余的差值都超过 5，因而每项都是其所在组的唯一项。

对于斐波那契数列，这种方法会产生可预测的结果，但在探索性数据分析中，可能会出现各种模式。我将描述如何使用一种新的数据可视化方法探索这些模式，首先从下面几个熟悉的图表类型开始比较。

## 使用直方图

让我们尝试将聚类策略应用于直方图，通过确定基于相同阈值的箱子数量并审查分离：

```py
# pandas histogram
import pandas as pd
import numpy as np
data = {'value' : [0,1,1,2,3,5,8,13,21,34,55,89,144]}
df = pd.DataFrame(data=data)
data_range = df['value'].max() - df['value'].min()
num_bins = np.ceil(data_range/5).astype(int)
print(num_bins) # 29
df['value'].hist(bins=num_bins, color='w', edgecolor='black', 
  linewidth=1.2, grid=False, figsize=(7,1.5))
```

![](img/06a81e6b5ef2c72ae4d181eb05a1700e.png)

以 29 个箱子的形式呈现的斐波那契数列（0 到 144）的直方图，使用 Matplotlib 渲染

## 使用蜜蜂散点图

现在，让我们用蜜蜂散点图可视化每个单独的值，每个圆的直径等于阈值，以寻找间隙：

```py
# vizmath (modified) beeswarm chart
from vizmath.beeswarm import swarm
import pandas as pd
from math import pi
data = {
    'id' : [str(i) for i in range(1, 14)],
    'value' : [0,1,1,2,3,5,8,13,21,34,55,89,144]
}
df = pd.DataFrame(data=data)
bs = swarm(df, 'id', 'value', None, size_override=pi*(5/2)**2)
bs.beeswarm_plot(color=False)
```

![](img/03f47b57c7d0edffe1d21f4f268d1d60.png)

斐波那契数列（0 到 144）的蜜蜂散点图，直径为 5，使用 Matplotlib 渲染

# 水晶条形图

## 布局

为了介绍水晶条形图，让我们继续之前的例子，使用交替的灰色阴影来说明不同的集群（稍后我们将详细介绍）：

```py
# vizmath crystal bar chart
import pandas as pd
from vizmath.crystal_bar_chart import crystals
data = {
    'id' : [str(i) for i in range(1, 14)],
    'value' : [0,1,1,2,3,5,8,13,21,34,55,89,144]
}
df = pd.DataFrame(data=data)
cbc = crystals(df, 'id', 'value', 5, width_override=5, rotation=90)
cbc.cbc_plot(legend=False, alternate_color=True, color=False)
```

![](img/35ebc1436cf6da4b641635a9a53ecb32.png)

斐波那契数列（0 到 144）的水晶条形图，使用 Matplotlib 渲染

为了有趣，我们添加一个任意大小属性：

```py
# vizmath crystal bar chart with added width property
import pandas as pd
from vizmath.crystal_bar_chart import crystals
data = {
    'id' : [str(i) for i in range(1, 14)],
    'value' : [0,1,1,2,3,5,8,13,21,34,55,89,144],
    'size' : [5,13,8,7,6,8,13,5,11,4,9,12,6] # new size property
}
df = pd.DataFrame(data=data)
cbc = crystals(df, 'id', 'value', 5, width_field='size', rotation=90)
cbc.cbc_plot(legend=False, alternate_color=True, color=False)
```

![](img/44f0f040635ea574c3cedb87f79adc76.png)

斐波那契数列（0 到 144）的水晶条形图，带有任意属性‘大小’，使用 Matplotlib 渲染

现在让我们移动值，以观察图表如何适应新的原点：

```py
# vizmath crystal bar chart with adjusted origin
import pandas as pd
from vizmath.crystal_bar_chart import crystals
data = {
    'id' : [str(i) for i in range(1, 14)],
    'value' : [0,1,1,2,3,5,8,13,21,34,55,89,144]
}
df = pd.DataFrame(data=data)
cbc = crystals(df, 'id', 'value', 5, width_override=5,
    rotation=90, offset=21) # new offset
cbc.cbc_plot(legend=False, alternate_color=True, color=False)
```

![](img/f86cfcf1eb94e51277662ffd06b12717.png)

斐波那契数列（0 到 144）的水晶条形图，调整偏移 = 21，使用 Matplotlib 渲染

## 灵感

在 2020 年 10 月，我和我的妻子在查看一个数据可视化挑战时，数据提供的是一个包含一些重复项的单一特征。

我最初的想法是制作一些看起来有三维效果的、类似水晶的东西，最终得到的是一种简单的水晶条形图版本。

![](img/6f78957a070ff157368aca0337ba7055.png)

早期的水晶条形图，由 Nick Gerend 于 2020 年 10 月制作，使用 Tableau 渲染

在 2022 年，我再次拿起了这个想法，并使用基于阈值的聚类（顺序差分聚类策略）对一组值进行分层，给定阈值，并使每个后续水晶堆叠在其偏移（移除一次）邻居旁边的中心轴周围形成一个集群的水晶（代表一个集群中的值）。

摩天大楼¹似乎是一个很好的数据集开始，并成为我对新算法的第一次测试：

![](img/1965e53a04f6303556e701294973203c.png)

“天际线”由 Nick Gerend（2022 年 4 月 10 日）

接下来，我将描述水晶条形图的关键特性。

## 水晶条形图算法

绘制 Crystal Bar Chart 是将晶体顶部面的接触点与原点对齐，以便侧面正确绘制，勾勒出每个值对应的完整晶体：

![](img/743f5440d37f421b00f598844dbeb95b.png)

Crystal Bar Chart 由 Nick Gerend 构建

1.  从***一组值***开始，并根据偏好（升序或降序）对其进行排序，例如：0.2, 1.5, 7.4, 9.4

1.  使用期望的***偏移量***（例如 -1.7）调整值，以调整原点的位置

1.  设置一个***阈值***（例如 3.5），并根据之前描述的***顺序差异聚类***对数据进行分组

1.  遍历每个组（外层循环）及组内的每个值（内层循环），将初始值的晶体沿***中心轴***定向，并根据项目的范围更新下一个晶体的起始位置，该范围垂直于中心轴

1.  对于每个后续值，交替左右放置晶体，以类似方式考虑晶体顶部面占据的范围，以调整下一个交替晶体的位置（不同的***排序和垂直轴上的放置方法***正在审查以备未来更新）

1.  计算晶体每个面的点（左、右、顶部）：

    ***顶部面尺寸***：由顺序差异聚类阈值（沿中心轴的高度）和根据大小属性的范围（垂直于中心轴的宽度）勾勒

    ***左右面尺寸***：根据视图中的侧面多边形绘制，确定为晶体顶部面和原点之间接触点的斜率（默认为 0,0）

1.  当遇到新集群时，重置起始位置，并对集群中每个晶体重复面位置计算

1.  如果需要，可以反转偏移量以将值重新映射到原始范围

## Python 实现

我已经通过我的 [vizmath](https://pypi.org/project/vizmath/) 包在 PyPI 上提供了 Crystal Bar Chart 算法的 Python 实现。让我们使用上面的初始示例详细分析几个选项，并解释输入和输出：

```py
import pandas as pd
from vizmath.crystal_bar_chart import crystals # pip install vizmath==0.0.14

# using the example data from above:
data = {
    'id' : [str(i) for i in range(1, 14)],
    'value' : [0,1,1,2,3,5,8,13,21,34,55,89,144],
    'size' : [5,13,8,7,6,8,13,5,11,4,9,12,6]
}
df = pd.DataFrame(data=data)

# create a crystals object
#   > df: DataFrame with 1 numerical column of data and an
#     optional size column 'width_field'
#   > id_field: required identifier (can be dummy values)
#   > height_field: required value column
#   > height_range: sequential differential clustering threshold
#   > width_field = optional size column
#   > bottom_up: False = descending, True = ascending
#   > width_override: value constant to set the size value
#     (overrides the width_field values)
#   > offset: value to adjust the origin by
#   > reset_origin: False = keeps offset, True: resets origin with offset
#   > rotation: overall rotation around the center in degrees
cbc = crystals(df, 'id', 'value', 5, width_field='size', bottom_up = True, 
    width_override = None, offset=21, reset_origin=True, rotation=90)

#plot the Crystal Bar Chart
cbc.cbc_plot(legend=False, alternate_color=True, color=False)
```

![](img/46102c97134969bba408389b2c9742fe.png)

Crystal Bar Chart 以反向顺序和偏移量及重置原点，使用 Matplotlib 渲染

以下是 Crystal Bar Chart 算法的输出：

+   id - 项目标识符

+   group - 由顺序差异聚类生成的项目集群：1 到 N

+   side - 晶体面的标识符：{0,1,2}

+   value - 项目的值（沿中心轴的晶体顶部的质心位置）

+   height - 顺序差异聚类阈值（晶体顶部面的高度，平行于中心轴）

+   width - 次要值 ≥ 0（晶体顶部面的宽度，垂直于中心轴）

+   x, y - 布局中点的笛卡尔二维坐标

+   path - 描述一个有序整数集的路径，该路径围绕一个多边形，同时与水晶条形图中的每个(x, y)点对应，对于每个水晶 id 和面：1 到 N。

```py
# Cyrtsal Bar Chart DataFrame
cbc.o_crystal_bar_chart.df[['id', 'group', 'side',
    'value', 'height', 'width', 'x', 'y', 'path']].head()
```

![](img/d8f0de714b8953789535c5dcf81c41cc.png)

水晶条形图数据框

在未来的版本中，我将尝试结合不同的选项，将水晶沿垂直于中央轴的轴线放置，以便进行集群和价值比较。

## Tableau Public 实现

在本节中，我将介绍如何在 Tableau Public (v 2023.3.0)中实现我的水晶条形图可视化以及一些有趣的交互功能。

为了配合水晶主题，我们从维基百科上获取一个关于钻石的数据集²。数据包含钻石的名称、未切割和已切割的重量、来源等信息。为了示例 purposes，我将数据限制为大于 200 克拉且未切割和已切割值只有一个切割记录的钻石。

```py
import pandas as pd

# https://en.wikipedia.org/wiki/List_of_diamonds (as of 12/25/2023)
# filtered to enries with uncut and cut values with only 1 cut, >200 carats
diamonds = {
    'Name' : [
        '4 February Stone', 'Centenary Diamond', 'Cross of Asia',
        'DeBeers Diamond', 'Earth Star Diamond', 'Golden Jubilee Diamond',
        'Graff Lesedi La Rona', 'Great Mogul Diamond', 'Gruosi Diamond',
        'Incomparable Diamond', 'Jubilee Diamond', 'Koh-i-Noor',
        'Lesotho Brown', 'Lesotho Promise', 'Millennium Star',
        'Premier Rose Diamond', 'Regent Diamond', 'Taylor-Burton Diamond',
        'Tiffany Yellow Diamond'],
    'Uncut' : [
        404.2, 599, 280, 440, 248.9, 755.5, 1111, 780, 300.12, 
        890, 650.8, 793, 601, 603, 777, 353.9, 410, 241, 280],
    'Cut' : [
        163.41, 273.85, 79.12, 234.5, 111.59, 545.67, 302.37, 280, 115.34,
        407.48, 245.3, 105.6, 71.73, 75, 203.04, 137, 140.64, 68, 128.54],
    'Color' : [
        'white', 'colorless', 'yellow', '-', 'brown', 'yellow-brown',
        'colourless', '-', 'black', 'brownish-yellow', 'colorless',
        'colorless', 'pale brown', 'colorless', 'colorless',
        'colorless', 'white with pale blue', 'colorless', 'yellow'],
    'Origin' : [
        'Angola', 'South Africa', 'South Africa', 'South Africa',
        'South Africa', 'South Africa', 'Botswana', 'India', 'India',
        'Democratic Republic of Congo', 'South Africa', 'India', 'Lesotho',
        'Lesotho', 'Democratic Republic of Congo', 'South Africa', 'India',
        'South Africa', 'South Africa']
    }

df = pd.DataFrame(data=diamonds)
```

接下来，我们将使用 vizmath 创建一个水晶条形图，并将绘图信息和原始数据输出到 csv 文件中：

```py
from vizmath.crystal_bar_chart import crystals
cbc = crystals(df, 'Name', 'Uncut', 100, width_field='Cut') # calculate
cbc.o_crystal_bar_chart.dataframe_rescale(0, 5000, -2500, 2500) #rescale
cbc.to_csv('crystal_bar_chart') # crystal bar chart output
cbc.df.to_csv('data.csv') # original data
```

使用***Text file***选项将***crystal_bar_char.csv***文件导入 Tableau。然后使用***Connections***旁边的***Add***链接将***data.csv***添加到***Files***列表中。在右侧的阶段，双击***crystal_bar_char.csv***标签并将***data.csv***文件从***Files***拖到该阶段。选择***Inner Join***，在***Data Source***下拉菜单中选择***[Id]***字段，***data.csv***下的***[Name]***字段。

数据集准备好后，导航到***Sheet 1***，并创建我们将用于绘制图表的计算列：

> ***[chart]****:* MAKEPOINT([Y],[X])
> 
> ***[chart_top]****:* 如果[Side] = 0，则 MAKEPOINT([Y],[X])，否则为 null end。

首先，将***[chart]***拖到***Marks***下的***Detail***中以生成第一个地图层，并通过右击地图区域并选择***Background Layer***s 调整这些选项。

+   取消所有***Background Map Layers***（Base、Land Cover 等）。

+   现在右击地图区域，选择***Map Options***并取消所有选项。

关闭***Background Layers***并继续以下步骤：

+   将***[Group]***、***[Id]***和***[Side]***拖到***Marks***下的***Detail***中。

+   将现在的***SUM(Group)***和***SUM(Side)***右击每一个并选择相应选项转换为***Dimension***和***Discrete***。

+   右击***[Group]***，选择***Sort***，并选择***Descending***。

+   在***Marks***下拉菜单中选择***Polygon***（此时如果看起来有些奇怪也不用担心）。

+   将***[Path]***拖到***Marks***下的***Path***，右击现在的***SUM(Path)***并选择***Dimension***。

+   将***[Group]***拖到***Color***，并重复该过程，将其转换为***Dimension***和***Discrete***。

+   在***Color***下选择“Edit Colors…”，并配置为交替的灰度方案，0 和 2 组使用浅色，1 和 3 组使用深色。

+   点击确定，将不透明度调整为 95%，并在***Color***下选择黑色边框。

+   将***[chart_top]***拖入地图区域，弹出窗口将出现：***Add a Marks Layer -*** 将该药丸拖入其中以创建新的地图层。

+   重复上述步骤，只不过现在将***[Origin]***用于***Color***，并根据需要调整颜色。

+   在***Color***下，选择黑色边框并将不透明度设置为 70%，然后右击图表右下角的空值药丸，选择***Hide Indicator***以隐藏空值标签。

现在图表部分已就位，应类似于以下内容：

![](img/24e0228439c531acbe49d4c5fb28bf03.png)

让我们添加另一个图表以用于比较和交互。

+   使用底部面板上的第一个加号创建一个新的工作表，命名为***Sheet 2***。

+   将***[Group]***拖到***Columns***并转换为***Dimension***和***Discrete***。

+   将***[Name]***拖到***Columns***，将***[Uncut]***拖到***Rows***。

+   右键点击***[Uncut]***，在***Measure***下选择***Maximum***。

+   将***[Origin]***拖到***Color***，将***[Cut]***拖到***Size***。

+   右键点击***[Cut]***，在***Measure***下选择***Maximum***。

+   在顶部的***Format***菜单下，选择***Shading…***，并在***Column Banding*** > ***Header***下选择深灰色，***Level***勾选设置为第一个刻度，其他选项设置为***None***。

+   右键点击顶部的**Group / Name**，选择**Hide Field Labels for Columns***，并根据需要调整图例名称和颜色。

***Sheet 2***准备好了，选择顶部菜单中的***Entire View***后应类似于以下内容：

![](img/37fa232ae8d0c4a18615e1cfac0190e7.png)

最后，将两个工作表整合到仪表板中。创建仪表板并添加工作表后，在***Dashboard***顶部菜单中的***Actions***下设置一个操作。点击***Add Action***下拉菜单，选择***Highlight***。在***Source Sheets***中选择***Sheet 2***，在***Target Sheets***中选择***Sheet 1***。在***Targeted Highlighting***中选择***Selected Fields***，并选择***[Group]***和***[Name]***字段。最后，在右侧的***Run action on***菜单中选择***Hover***选项，这样整个仪表板将在***Sheet 2***上的每个条形和组上悬停时突出显示！

调整颜色并将所有内容以有序的方式排列后，以下是我们在 Tableau Public 中的新仪表板：

![](img/f9b51913372a1a0b89d3290fc1589b1a.png)

## 结论

在本文中，我概述了一种我称之为晶体条形图的新型数据可视化工具。这个工具对于将信息压缩到一个小空间非常有用，通过沿中央轴重叠的形状来表示一维数据，这些数据按顺序差异聚类分组，还可以选择沿垂直轴表示第二个数值属性。

晶体条形图的节省空间功能是对条形图和类似可视化工具的有效替代，它不需要较大的占用空间，并且与各种其他工具配合良好，用于在学术和专业工作中检查数据系列。

通过多种选项调整晶体状数据表示的特征，我希望你能发现这种可视化技术适用于不同的数据探索任务，并且是一种有趣的新方式来发现洞察！

**参考文献**

[1] 维基百科（CC BY-SA），“[最高建筑物列表](https://en.wikipedia.org/wiki/List_of_tallest_buildings)”（截至 2022 年 4 月 10 日）

[2] 维基百科（CC BY-SA），“[钻石列表](https://en.wikipedia.org/wiki/List_of_diamonds)”（截至 2023 年 12 月 25 日）

**相关文章**

+   [介绍多弦图：可视化复杂的集合关系](https://medium.com/towards-data-science/introducing-the-multi-chord-diagram-visualizing-complex-set-relationships-c6fe6cc1cb8b)

+   [径向树形图：将树形图扩展到圆形映射](https://medium.com/towards-data-science/radial-treemaps-extending-treemaps-to-circular-mappings-7b47785191da)
