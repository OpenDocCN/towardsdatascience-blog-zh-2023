# 使用 Matplotlib 改进您的极坐标条形图

> 原文：[`towardsdatascience.com/enhance-your-polar-bar-charts-with-matplotlib-c08e332ec01c`](https://towardsdatascience.com/enhance-your-polar-bar-charts-with-matplotlib-c08e332ec01c)

## 使用几个简单的调整来改进您的极坐标条形图数据可视化

[](https://andymcdonaldgeo.medium.com/?source=post_page-----c08e332ec01c--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----c08e332ec01c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c08e332ec01c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c08e332ec01c--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----c08e332ec01c--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c08e332ec01c--------------------------------) ·8 分钟阅读·2023 年 4 月 24 日

--

![](img/117d3fa97589d9986c02fffa1aa742da.png)

使用 matplotlib 创建的极坐标条形图，并应用了 mplcyberpunk 主题。图片由作者提供。

极坐标条形图是传统条形图的一个很好的替代方案。这些图表使用极坐标网格来表示条形，而不是将条形水平或垂直排列在笛卡尔坐标系中。

以这种方式绘制数据可以帮助可视化周期性数据，并提供更引人注目且更具美感的数据可视化效果。

极坐标条形图可以很容易地在 [matplotlib](https://matplotlib.org/) 中创建；然而，默认的图表效果欠佳，需要进行一些调整才能使其更具视觉吸引力。

在这个简短的教程中，我们将展示如何通过更改样式，甚至为数据应用赛博朋克主题来改进基本图表，以获得更引人注目的效果。

# 导入库并设置数据

第一步是导入我们所需的库。在本教程中，我们将使用 [numpy](https://numpy.org/) 来进行一些计算，并使用 [matplotlib](https://matplotlib.org/) 来显示我们的图表。

```py
import numpy as np
import matplotlib.pyplot as plt
```

一旦导入了库，我们可以创建一些数据进行操作。

在这个例子中，我们将创建代表调查数据的虚拟数据，其中人们选择他们最喜欢的岩石类型。

为了创建这个虚拟数据集，我们首先需要生成十个 1 到 10 之间的随机整数。这将代表十种不同的岩石名称，我们将接下来创建这些名称并将其分配给变量 `rock_names`。

```py
rankings = np.random.randint(1, 10, 10)

rock_names = [
    'Granite',
    'Basalt',
    'Gneiss',
    'Schist',
    'Sandstone',
    'Limestone',
    'Shale',
    'Conglomerate',
    'Dolomite',
    'Marble'
]
```

# 创建一个基本的 Matplotlib 条形图

使用 matplotlib 创建简单图表非常容易。

让我们创建一个简单的条形图，看看我们的图表会是什么样的。

要创建条形图，我们只需两行代码。

```py
plt.bar(x=rock_names, height=rankings)
plt.show()
```

当代码运行时，我们将得到如下图表。

![](img/87085a8b5416f533c8631263b3d71831.png)

使用 matplotlib 创建的基本条形图。图像由作者提供。

这是使用 matplotlib 生成的标准图表样式。它们非常沉闷、缺乏灵感且常常令人困惑。通常，我们需要通过几行代码来改进这些图表，才能得到可以在信息图表或可视化仪表板中使用的内容。

在上一篇文章中，我介绍了如何通过 7 个简单步骤将传统的条形图提升到一个新水平。你可以在下面查看。

[## 7 Steps to Help You Make Your Matplotlib Bar Charts Beautiful](https://towardsdatascience.com/7-steps-to-help-you-make-your-matplotlib-bar-charts-beautiful-f87419cb14cb?source=post_page-----c08e332ec01c--------------------------------)

### 通过一些简单的调整轻松提高你的 Matplotlib 数据可视化的质量

[点击这里了解更多信息](https://towardsdatascience.com/7-steps-to-help-you-make-your-matplotlib-bar-charts-beautiful-f87419cb14cb?source=post_page-----c08e332ec01c--------------------------------)

# 使用 Matplotlib 创建极坐标条形图

现在让我们来看看一个极坐标条形图，它本质上与上面的图表相同，但我们不是在传统的笛卡尔坐标系上绘制，而是在极坐标投影上绘制。

如下所示，我们需要几行代码来创建我们的基本图形。

```py
fig, ax = plt.subplots(subplot_kw={'projection': 'polar'}, figsize=(10,10))

indexes = list(range(0, len(rock_names)))
width = 2*np.pi / len(rock_names)
angles = [element * width for element in indexes]

label_loc = np.linspace(start=0, stop=2 * np.pi, num=len(rock_names))

bars = ax.bar(x = angles, height=rankings, width=width, 
       edgecolor='black', zorder=2, alpha=0.8)

plt.show()
```

首先，我们需要通过调用`plt.subplots`来创建我们的图形对象。在这里我们可以指定我们希望将数据绘制在极坐标投影上。

然后我们基于原始列表创建四个新变量：

+   `indexes`：创建一个从 0 到我们列表中项目总数的数字列表。在我们的情况下，这将生成一个从 0 到 9 的列表。

+   `width`：根据通过将圆周长除以`rock_names`中的岩石类型数量来计算每个条形图的宽度，创建一个列表

+   `angles`：创建一个包含每种岩石类型角度的列表

+   `label_loc`：创建一个 0 到 2 * pi 之间均匀分布的值列表，用于显示岩石类型标签

一旦这些变量创建好，我们可以调用`ax.bar`并开始将一些变量传递给所需的参数。

为了让网格显示在条形图下面，我们可以将`zorder`值设置为 2。

我们还将把条形图的`edgecolour`设置为黑色，并将透明度（alpha）设置为 0.8。

当我们运行代码时，会返回以下图表，现在我们有了一个基本的极坐标条形图。

![](img/76ebb210d93a5f534a371d7ad2d61076.png)

一个基本的 matplotlib 极坐标条形图。图像由作者提供。

在上面的图表中，我们可以看到我们不知道每个条形图的具体含义——这可以通过添加来自`label_loc`列表的标签来解决。

我们还将看到从中心延伸出的辐条与条形图不匹配，我们可以轻松解决这个问题。

## 向极坐标条形图添加标签

首先，让我们为图表添加一些标签。

为了确保我们的标签易于阅读而不需要扭曲脖子，我们可以确保标签的位置恰当。

为此，我们需要遍历`angles`、`rankings`和`rock_names`列表中的元素。

然后我们需要将角度测量从弧度转换为度。

然后我们需要在循环中检查当前角度。如果条形的角度小于π，则从旋转角度中减去 90 度。否则，如果条形在圆的下半部分，则将 90 度添加到旋转角度。

请注意，在之前的图片中，0 度位于图表的右侧。

接下来，我们需要使用`ax.text`添加标签。为了确保标签位于图表外部，我们可以将`y`参数设置为 11，因为我们外环的最大值是 10。

```py
fig, ax = plt.subplots(subplot_kw={'projection': 'polar'}, figsize=(10,10))

indexes = list(range(0, len(rock_names)))
width = 2*np.pi / len(rock_names)
angles = [element * width for element in indexes]

label_loc = np.linspace(start=0, stop=2 * np.pi, num=len(rock_names))

bars = ax.bar(x = angles, height=rankings, width=width, 
       edgecolor='black', zorder=2, alpha=0.8)

for angle, height, rock_name in zip(angles, rankings, rock_names):
    rotation_angle = np.degrees(angle)
    if angle < np.pi:
        rotation_angle -= 90
    elif angle == np.pi:
        rotation_angle -= 90
    else:
        rotation_angle += 90
    ax.text(angle, 11, rock_name, 
            ha='center', va='center', 
            rotation=rotation_angle, rotation_mode='anchor', fontsize=16)

plt.show()
```

当代码运行时，我们现在得到如下图表，标签整齐地位于图表周围，易于读取。

![](img/bad1b898082d4e5d51273acc7b8caf2b.png)

设置标签与条形平行后的极坐标条形图。图片由作者提供。

# 格式化径向条形图网格

在之前的图表中，我们有网格显示在背景中。然而，辐条与条形的边缘不对齐。

有几种方法可以解决这个问题。在这个例子中，我添加了一个位于数据条形下方的第二个径向条形图，称为`bars_bg`。它设置为灰色，且高度参数设置为 10，这样填充到图表边缘。

```py
bars_bg = ax.bar(x = angles, height=10, width=width, color='lightgrey',
       edgecolor='white', zorder=1, alpha=0.8)
```

要隐藏网格线，我们可以使用`ax.set_ticks`并传入一个空列表。

我们也可能希望保留极坐标网格，以便清晰地看到各个部分。这是可选的，但有助于可读性。

```py
ax.set_xticks([])

ax.grid(alpha=0.8, color='white', lw=3)
```

我们创建极坐标条形图的完整代码现在如下所示：

```py
fig, ax = plt.subplots(subplot_kw={'projection': 'polar'}, figsize=(10,10))

indexes = list(range(0, len(rock_names)))
width = 2*np.pi / len(rock_names)
angles = [element * width for element in indexes]

label_loc = np.linspace(start=0, stop=2 * np.pi, num=len(rock_names))

bars_bg = ax.bar(x = angles, height=10, width=width, color='lightgrey',
       edgecolor='white', zorder=1, alpha=0.8)

bars = ax.bar(x = angles, height=rankings, width=width, 
       edgecolor='white', zorder=1, alpha=0.8)

for angle, height, rock_name in zip(angles, rankings, rock_names):
    rotation_angle = np.degrees(angle)
    if angle < np.pi:
        rotation_angle -= 90
    elif angle == np.pi:
        rotation_angle -= 90
    else:
        rotation_angle += 90
    ax.text(angle, 11, rock_name, 
            ha='center', va='center', 
            rotation=rotation_angle, rotation_mode='anchor', fontsize=16)

ax.set_xticks([])

ax.grid(alpha=0.8, color='white', lw=3)

plt.ylim(0, 10)
plt.show()
```

这会生成如下图表。

![](img/a2cee878070330fdfb2a1c83d7711909.png)

调整样式和修正网格线后的极坐标条形图。图片由作者提供。

我们可以调整图表直到满意，但这相比我们开始时的图表有了显著改善。

# 赛博朋克风格的径向条形图

如果我们想改变图表的样式，可以使用多种可用于 matplotlib 的主题库之一。

应用这些样式通常需要添加几行代码——通常是一个`import`语句和一个包含所选样式的`with`语句。

这是赛博朋克风格化图表的完整代码。请注意，为了使条形稍微减少“直观冲击”，对`alpha`值进行了些许调整。

```py
import mplcyberpunk

with plt.style.context('cyberpunk'):
    fig, ax = plt.subplots(subplot_kw={'projection': 'polar'}, figsize=(10,10))

    angles = np.linspace(0, 2*np.pi, len(rock_names), endpoint=False)

    indexes = list(range(0, len(rock_names)))
    width = 2*np.pi / len(rock_names)
    angles = [element * width for element in indexes]

    label_loc = np.linspace(start=0, stop=2 * np.pi, num=len(rock_names))
    bars_bg = ax.bar(x = angles, height=10, width=width, color='lightgrey',
           edgecolor='white', zorder=1, alpha=0.05)

    bars = ax.bar(x = angles, height=rankings, width=width, 
           edgecolor='white', zorder=2, alpha=0.6)

    for angle, height, rock_name in zip(angles, rankings, rock_names):
        rotation_angle = np.degrees(angle)
        if angle < np.pi:
            rotation_angle -= 90
        elif angle == np.pi:
            rotation_angle -= 90
        else:
            rotation_angle += 90
        ax.text(angle, 11, rock_name, 
                ha='center', va='center', 
                rotation=rotation_angle, rotation_mode='anchor', fontsize=16)

    ax.set_xticks([])

    ax.grid(alpha=0.1, color='white', lw=3)

    plt.ylim(0, 10)

    plt.show()
```

运行上述代码后，我们得到如下图形，非常适合用于信息图表和吸引人们的注意。

![](img/370ed69d496af8df21048af065ee0fb7.png)

应用赛博朋克主题后的极坐标条形图。图片由作者提供。

要了解更多其他样式库，请查看我下面关于四种关键库的文章，它们将提升你的 matplotlib 图表水平。

[](/upgrade-your-data-visualisations-4-python-libraries-to-enhance-your-matplotlib-charts-74361bc3b92e?source=post_page-----c08e332ec01c--------------------------------) ## 升级您的数据可视化：4 个 Python 库提升您的 Matplotlib 图表

### 使用这些易于使用的 Matplotlib 样式库提升您的数据可视化

towardsdatascience.com

# 摘要

极坐标条形图通过在极坐标网格上表示数据，为传统条形图提供了一个很好的替代方案。这在查看周期性数据和创建视觉上吸引人的数据可视化时非常有用。

由于 matplotlib 默认生成的图表通常较为单调和乏味，我们可以通过少量的调整和几行代码，使其看起来显著更好。

matplotlib 提供的众多主题也允许我们通过几行额外的代码将基础图表转变为完全不同的样式。

*感谢阅读。在您离开之前，您一定要订阅我的内容，将我的文章送到您的收件箱。* [***您可以在这里操作！***](https://andymcdonaldgeo.medium.com/subscribe)*另外，您也可以* [***注册我的通讯***](https://fabulous-founder-2965.ck.page/2ca286e572) *，以便免费将额外的内容直接发送到您的收件箱。*

*其次，您可以通过注册会员来获得完整的 Medium 体验，并支持包括我在内的数千位其他作者。这仅需每月 $5，您就可以完全访问所有精彩的 Medium 文章，并有机会通过写作赚取收入。*

*如果您通过* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***注册，*** *您将直接支持我，并且不会增加您的费用。如果您这样做，非常感谢您的支持。*
