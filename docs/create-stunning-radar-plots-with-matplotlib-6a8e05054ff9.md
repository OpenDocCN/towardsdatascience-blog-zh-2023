# 使用 Matplotlib 创建惊艳的雷达图

> 原文：[`towardsdatascience.com/create-stunning-radar-plots-with-matplotlib-6a8e05054ff9`](https://towardsdatascience.com/create-stunning-radar-plots-with-matplotlib-6a8e05054ff9)

## 使用 Python 最受欢迎的数据可视化库可视化地质数据

[](https://andymcdonaldgeo.medium.com/?source=post_page-----6a8e05054ff9--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----6a8e05054ff9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6a8e05054ff9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a8e05054ff9--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----6a8e05054ff9--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a8e05054ff9--------------------------------) ·阅读时间 7 分钟·2023 年 4 月 19 日

--

![](img/0dec40fb2793b0b25ae483d0ba80f368.png)

带有赛博朋克主题的 Matplotlib 雷达图。图片由作者提供。

雷达图（也称为蜘蛛图或雷达图表）是一种流行的数据可视化工具，它允许我们通过在二维图上同时显示多个变量来比较数据集。

每个变量由一个从图中心延伸到边缘的射线表示，该变量的大小由射线的长度表示。然后在每个变量之间绘制线条，以形成类似网状的形状。

在地球科学和岩石物理学中，我们可以使用雷达图来比较不同钻井间岩石类型的变化，或展示岩石样本间矿物学的差异。

在本教程中，我将演示如何使用一些合成的岩石类型数据来创建雷达图。这些数据代表了在一个钻井中遇到的平均岩石类型。

让我们开始吧！

# 导入库和数据

要开始，我们需要导入两个库：[numpy](https://numpy.org/) 和 [matplotlib](https://matplotlib.org/stable/index.html)。

```py
import matplotlib.pyplot as plt
import numpy as np
```

接下来，我们可以创建一些虚拟数据。

通常，这些数据可能已经包含在 pandas 数据框中。如果是这样，建议在继续之前将所需的列转换为列表。

```py
lithologies = ['Shale', 'Sandstone', 
               'Limestone', 'Dolomite', 
               'Anhydrite', 'Coal', 'Tuff']

well1 = [47, 35, 10, 1, 0, 2, 5]

well2 = [7, 10, 51, 22, 10, 0, 0]
```

在使用数据之前，我们需要确保在雷达图上绘制的区域形成一个封闭的多边形。

为了做到这一点，我们需要将列表中的所有元素取出，然后将第一个元素附加到列表的末尾。

```py
lithologies = [*lithologies, lithologies[0]]
well1 = [*well1, well1[0]]
well2 = [*well2, well2[0]]
```

如果我们调用 `well2`，我们将得到以下值列表，我们会发现第一个数字现在也在列表末尾。

```py
[7, 10, 51, 22, 10, 0, 0, 7]
```

# 使用 Matplotlib 构建雷达图

为了开始构建雷达图，我们首先需要对`lithologies`列表中的标签位置进行排序。这些标签将均匀分布在极坐标图的边缘。

```py
label_loc = np.linspace(start=0, stop=2 * np.pi, num=len(lithologies))
```

接下来，我们可以开始创建雷达图。

首先，我们需要定义图形，这将通过使用`plt.subplots`来创建。在`subplot_kw`参数中，我们需要指定我们想要一个极坐标图。

接下来，我们将向图中添加两个坐标轴，并传入`label_loc`变量以及`well1`和`well2`列表。

然后，我们将使用`plt.thetagrids()`创建极坐标网格，在这里，我们将传入标签位置和岩性标签。

```py
fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

ax.plot(label_loc, well1, lw=2)
ax.plot(label_loc, well2, lw=2)

lines, labels = plt.thetagrids(np.degrees(label_loc), labels=lithologies)

plt.show()
```

当我们运行上述代码时，我们将得到以下雷达图。

![](img/d592979df781d1c6d4b4ae502e301cb8.png)

使用 matplotlib 创建的基本雷达图。图片由作者提供。

我们得到的图对于基本的雷达图来说很好。然而，我们可以通过一些调整来改进它。

# 向 Matplotlib 雷达图添加填充

我们可以通过给两个区域添加透明填充来改进雷达图。

这是通过使用`ax.fill()`完成的，传入`label_loc`、包含值的相关井列表，并通过`alpha`参数将不透明度设置为 0.3。

```py
fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

ax.plot(label_loc, well1, lw=2)
ax.plot(label_loc, well2, lw=2)

ax.fill(label_loc, well1, alpha=0.3)
ax.fill(label_loc, well2, alpha=0.3)

lines, labels = plt.thetagrids(np.degrees(label_loc), labels=lithologies)

plt.show()
```

当我们运行上述代码时，我们现在得到的雷达图填充了区域。

![](img/27261a95e5e6b1ffd3fdeda3ee4dbf37.png)

使用 matplotlib 创建的基本雷达图，显示了区域填充。图片由作者提供。

# 调整 Matplotlib 雷达图上的标签填充

你会注意到，之前图中的标签看起来很小，并且与线条重叠。

我们可以通过设置`tick_params`并更改填充和标签大小来修复这个问题。

```py
fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

ax.plot(label_loc, well1, lw=2)
ax.plot(label_loc, well2, lw=2)

ax.fill(label_loc, well1, alpha=0.3)
ax.fill(label_loc, well2, alpha=0.3)

lines, labels = plt.thetagrids(np.degrees(label_loc), labels=lithologies)

ax.tick_params(axis='both', which='major', pad=30, labelsize=15)

plt.show()
```

当我们运行代码时，我们得到的图标签更加清晰。

![](img/f6864882205ed4057d70afa7db42038e.png)

使用 matplotlib 创建的基本雷达图，显示了区域填充，并在调整标签填充和大小后。图片由作者提供。

# 使用赛博朋克风格美化雷达图

之前的图形成了一个适用于报告或演示的良好雷达图。然而，如果我们想要美化它，可以使用众多 matplotlib 主题库中的一种。

如果你想查看可用的库，那么我建议查看我之前的文章，其中介绍了 4 个流行的主题。

[](/upgrade-your-data-visualisations-4-python-libraries-to-enhance-your-matplotlib-charts-74361bc3b92e?source=post_page-----6a8e05054ff9--------------------------------) ## 提升你的数据可视化：4 个 Python 库来增强你的 Matplotlib 图表

### 使用这些易于使用的 Matplotlib 样式库来提升你的数据可视化

towardsdatascience.com

网络朋克主题是我在 matplotlib 中创建时尚图表的最爱之一。然而，这完全是出于美学考虑，你应始终考虑观众并确保最大可读性。

我们可以通过导入库并添加 `with` 语句来使用网络朋克主题。

```py
import mplcyberpunk

with plt.style.context('cyberpunk'):
    fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

    ax.plot(label_loc, well1, lw=2)
    ax.plot(label_loc, well2, lw=2)

    ax.fill(label_loc, well1, alpha=0.3)
    ax.fill(label_loc, well2, alpha=0.3)

    lines, labels = plt.thetagrids(np.degrees(label_loc), labels=lithologies)

    ax.tick_params(axis='both', which='major', pad=30, labelsize=15)

    plt.show()
```

我们现在可以看到，通过添加两行代码，图表已经发生了显著变化。然而，网格线对于我的喜好来说有点模糊。

![](img/0dd93f5f60f9a36aeabb7086c15ee2fd.png)

应用网络朋克主题后的 Matplotlib 雷达图。图像由作者提供。

我们可以通过添加一些额外的代码来使线条变亮。

更改网格线可以使用 `ax.grid()`，并将 `color` 和 `alpha` 参数设置为白色和 0.3。

为了给雷达图添加外环，我们需要设置一个包含 4 个值的自定义元组：红色、绿色和蓝色的颜色值以及一个 alpha 值。

然后我们可以将其传递到 `ax.spines['polar'].set_color()` 调用中。

```py
with plt.style.context('cyberpunk'):
    fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

    ax.plot(label_loc, well1, lw=2)
    ax.plot(label_loc, well2, lw=2)

    ax.fill(label_loc, well1, alpha=0.3)
    ax.fill(label_loc, well2, alpha=0.3)

    lines, labels = plt.thetagrids(np.degrees(label_loc), labels=lithologies)

    ax.tick_params(axis='both', which='major', pad=30, labelsize=15)

    ax.spines['polar'].set_linewidth(3)

    edge_color = (1, 1, 1, 0.2) 
    ax.spines['polar'].set_color(edge_color) 

    ax.grid(color='white', alpha=0.3)

    plt.show()
```

当我们运行代码时，现在可以看到带有径向网格线的雷达图。我们可以调整颜色和透明度，以达到想要的效果。

然而，请记住我们不希望线条过于明亮，以免分散读者对主要数据的关注。

![](img/5dc3e32c6e44b008f6a0a3e2a415792d.png)

带有网络朋克主题和可见网格线的 Matplotlib 雷达图。图像由作者提供。

为了完成样式设置，我们可以设置网格线的范围，从而使其从我们想要的数字开始并结束。这将使外环与其他环更加协调。

我们可以通过设置 `ax.set_ylim()` 并传入我们的最小值和最大值来实现这一点。

```py
with plt.style.context('cyberpunk'):
    fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

    ax.plot(label_loc, well1, lw=2)
    ax.plot(label_loc, well2, lw=2)

    ax.fill(label_loc, well1, alpha=0.3)
    ax.fill(label_loc, well2, alpha=0.3)

    lines, labels = plt.thetagrids(np.degrees(label_loc), labels=lithologies)

    ax.tick_params(axis='both', which='major', pad=30, labelsize=15)

    ax.spines['polar'].set_linewidth(3)

    edge_color = (1, 1, 1, 0.2) 
    ax.spines['polar'].set_color(edge_color) 

    ax.grid(color='white', alpha=0.3)

    ax.set_ylim(0, 60)

    plt.show()
```

我们现在有了一个看起来更好的雷达图。

![](img/7a50e30978d538313f98981ff318475a.png)

带有网络朋克主题并修正环范围的 Matplotlib 雷达图。图像由作者提供。

# 给雷达图添加图例

为了完成我们的图表，我们可以通过添加图例来帮助读者更好地理解图表。这将使读者了解每个区域的含义。

我们可以简单地在 `ax.plot()` 调用中添加一些标签，然后调用 `ax.legend`，然而，这只会向我们的图例中添加线条。

如果我们想要添加表示阴影区域的小矩形，我们必须首先创建一些补丁。

这可以通过从 `matplotlib.patches` 中导入 `Patch` 来完成。

创建的补丁随后被传递到 `ax.legend` 调用中。

```py
from matplotlib.patches import Patch

with plt.style.context('cyberpunk'):
    fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

    ax.plot(label_loc, well1, lw=2)
    ax.plot(label_loc, well2, lw=2)

    ax.fill(label_loc, well1, alpha=0.3)
    ax.fill(label_loc, well2, alpha=0.3)

    lines, labels = plt.thetagrids(np.degrees(label_loc), labels=lithologies)

    ax.tick_params(axis='both', which='major', pad=30, labelsize=15)

    ax.spines['polar'].set_linewidth(3)

    edge_color = (1, 1, 1, 0.2) 
    ax.spines['polar'].set_color(edge_color) 

    ax.grid(color='white', alpha=0.3)

    ax.set_ylim(0, 60)

    # Create custom legend handles
    well1_legend = Patch(facecolor='C0', alpha=0.5, label='Well 1')
    well2_legend = Patch(facecolor='C1', alpha=0.5, label='Well 2')

    # Add a legend with custom position and handles
    ax.legend(handles=[well1_legend, well2_legend],
              bbox_to_anchor=(1.3, 0.2), fontsize=20, 
              frameon=True)

    plt.show()
```

当我们运行代码时，现在可以看到完成的图表。

![](img/3ee56cd435914fa8faf8ab2c0ca72768.png)

带有网络朋克主题的 Matplotlib 雷达图。图像由作者提供。

# 总结

雷达图提供了良好的数据可视化，并且可以使用 matplotlib 轻松创建。通过 Cyberpunk 主题库的额外样式，我们可以将雷达图提升到一个新的水平，使其更具时尚感。

为什么不在下一个数据可视化项目中尝试一下雷达图呢？

*感谢阅读。在离开之前，你一定要订阅我的内容，并将我的文章直接发送到你的收件箱。* [***你可以在这里做到这一点！***](https://andymcdonaldgeo.medium.com/subscribe)*或者，你可以* [***注册我的新闻通讯***](https://fabulous-founder-2965.ck.page/2ca286e572) *以获取额外的内容，直接免费送到你的收件箱。*

*其次，你可以通过注册会员获得完整的 Medium 体验，并支持成千上万的其他作者和我。每月只需 $5，你就可以完全访问所有精彩的 Medium 文章，还有机会通过写作赚取收入。*

*如果你使用* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***注册，*** *你将直接通过一部分费用支持我，而且不会额外增加你的支出。如果你这样做了，非常感谢你的支持。*
