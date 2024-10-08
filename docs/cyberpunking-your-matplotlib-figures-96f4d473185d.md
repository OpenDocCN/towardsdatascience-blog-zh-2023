# 赛博朋克风格的 Matplotlib 图表

> 原文：[`towardsdatascience.com/cyberpunking-your-matplotlib-figures-96f4d473185d`](https://towardsdatascience.com/cyberpunking-your-matplotlib-figures-96f4d473185d)

## 用几行代码让你的 Matplotlib 图表从乏味变得有趣

[](https://andymcdonaldgeo.medium.com/?source=post_page-----96f4d473185d--------------------------------) ![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----96f4d473185d--------------------------------) [](https://towardsdatascience.com/?source=post_page-----96f4d473185d--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----96f4d473185d--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----96f4d473185d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----96f4d473185d--------------------------------) ·阅读时间 8 分钟·2023 年 5 月 21 日

--

![](img/fea8b724924c77103e370ccfe95d5961.png)

应用 mplcyberpunk 主题的包含多个子图的 matplotlib 图表。图片由作者提供。

当我们创建包含数据的信息图或海报时，我们希望吸引读者的注意，并使其在视觉上令人愉悦，同时讲述一个令人信服的故事。

在 Python 中，我们有许多绘图库可以用来创建图表——其中一个是著名的 matplotlib 库。然而，默认情况下，matplotlib 生成的图表往往被视为乏味的，要将其变得视觉上吸引人可能需要耗费时间。

这就是 matplotlib 主题库发挥作用的地方。我最喜欢的库之一是 [**CyberPunk 主题**](https://github.com/dhaitz/mplcyberpunk)

[](https://github.com/dhaitz/mplcyberpunk?source=post_page-----96f4d473185d--------------------------------) [## GitHub - dhaitz/mplcyberpunk: "赛博朋克风格" 的 matplotlib 图表

### 一个基于 matplotlib 的 Python 包，通过 3 行额外代码创建“赛博朋克”风格的图表。之后…

[github.com](https://github.com/dhaitz/mplcyberpunk?source=post_page-----96f4d473185d--------------------------------)

赛博朋克已成为一种广受欢迎的科幻子类型，其特点是描绘反乌托邦社会、高度先进的技术和反文化主题。这些设置通常以未来主义的外观呈现，突出霓虹灯和鲜艳大胆的色彩。

![](img/10163f4f60fb1a64672b34cf0421ac0e.png)

图片由作者使用 Midjourney 基本计划生成。（一个繁忙的反乌托邦赛博朋克主题的未来城市街道，特色是有许多街头小贩的地方。商店和区域被明亮的霓虹灯包围。雨天和阴暗的氛围。摄影真实。 — ar 3:2）

[**CyberPunk 主题**](https://github.com/dhaitz/mplcyberpunk)将那些霓虹和鲜艳的颜色带到了 matplotlib 中，正如你将在这篇文章中看到的，我们可以仅用两行 Python 代码应用这种样式。

![](img/0830976d812ec1e327123d432abb29c3.png)

CyberPunk 主题 Matplotlib 图形示例。图片由 dhaitz 提供，来自 CyberPunk Readme。

然而，你需要意识到，使用这种风格生成的图表在创建出版物图形时可能不会被视为专业，并且可能不适合有色觉问题的读者。

因此，在实施这种样式之前必须考虑你的受众。

# 导入库和创建数据

在我们开始创建可视化之前，我们需要导入一些库。这些库是[matplotlib](https://matplotlib.org/)、[pandas](https://pandas.pydata.org/)和[numpy](https://numpy.org/)，它们将用于创建、存储数据和创建可视化。

```py
import matplotlib.pyplot as plt
import pandas as pd
import mplcyberpunk
import numpy as np
```

一旦导入了库，我们可以使用以下代码创建虚拟数据。

```py
# Create a list of categories
categories = ['A', 'B', 'C', 'D']

# Create a DataFrame with 4 columns and 4 rows
data = {
    'Category': categories,
    'Value1': np.random.randint(0, 20, 4),
    'Value2': np.random.randint(0, 20, 4),
    'Value3': np.random.randint(0, 20, 4),
}

df = pd.DataFrame(data)
```

这段代码生成了一个包含 4 列的 pandas 数据框：其中一列是分类列，其他列是数字列，由随机整数构成。

# Cyberpunk 配色的条形图

我们将创建的第一个图表和主题是流行的条形图。

要创建这个图表，我们可以使用 pandas 的`.plot()`方法，并传入我们的`x`值，即我们的分类变量。我们还需要将`stacked`设置为`False`，将`kind`参数设置为`bar`。

```py
df.plot(x='Category', kind='bar', stacked=False, alpha=0.8, figsize=(10,10))
```

当我们运行上述代码时，我们将返回以下基本 matplotlib 图形。

![](img/8d21ae4f5b22640d17fcd602b30b4d6f.png)

使用 df.plot 生成的基本 matplotlib 条形图。图片由作者提供。

这很好；然而，它缺乏视觉吸引力，可以做得更好。

我们可以通过在`df.plot()`调用之前添加一行代码来轻松更改，这将使我们能够将 CyberPunk 主题应用于图表。

```py
with plt.style.context('cyberpunk'):
    df.plot(x='Category', kind='bar', stacked=False, alpha=0.8, 
            figsize=(10,10))
```

我们得到的是如下条形图。

![](img/80c30e862dc740ef7cc3418fe67a541f.png)

应用 mplcyberpunk 主题后的 matplotlib 条形图。图片由作者提供。

我们可以看到，图表背景颜色已切换到暗色主题，条形图呈现出“霓虹”般的外观。

在使用此主题创建条形图时，我们可能会发现条形图的颜色过于鲜艳，这就是我们必须应用 alpha 值的原因。

# 折线图

我们也可以对折线图进行同样的操作。

首先，我们调用`df.plot()`，但将`kind`从`bar`更改为`line`。使用折线图，我们还可以添加标记，这有助于识别数据的位置。如果我们在类别之间的值变化很小，这很有用。

```py
df.plot(x='Category', kind='line',
        lw=3, marker='.', ms=20,
        figsize=(10,10))
```

![](img/a8de47ed6aa78d5e5d3c609929f214e1.png)

使用 df.plot 生成的 matplotlib 基本折线图。图片由作者提供。

要应用 CyberPunk 主题，我们需要在代码中添加`with plt.style.context(‘cyberpunk’):`。

```py
with plt.style.context('cyberpunk'):
    df.plot(x='Category', kind='line',
            lw=3, marker='.', ms=20,
            figsize=(10,10))
```

运行时将生成以下折线图。

![](img/1193d1ec85522b55e133e543c698dfbf.png)

应用 mplcyberpunk 主题后的 matplotlib 折线图。图片由作者提供。

CyberPunk 主题库带有一些额外的功能，例如使折线发光：

```py
with plt.style.context('cyberpunk'):
    df.plot(x='Category', kind='line',
            lw=3, marker='.', ms=20,
            figsize=(10,10))
    mplcyberpunk.make_lines_glow()
```

![](img/9819c02aafd7f9ad12bc6536bb0bb465.png)

应用 mplcyberpunk 主题并为折线添加光晕效果后的 matplotlib 折线图。图片由作者提供。

并在折线下方添加了透明渐变填充：

```py
with plt.style.context('cyberpunk'):
    df.plot(x='Category', kind='line',
            lw=3, marker='.', ms=20,
            figsize=(10,10))
    mplcyberpunk.add_gradient_fill(alpha_gradientglow=0.4)
```

![](img/cced2202aed2388b7ce11562ee41a19c.png)

应用 mplcyberpunk 主题并对折线进行渐变填充后的 matplotlib 折线图。图片由作者提供。

如果你处理的是 1 或 2 条折线，添加渐变填充效果是不错的，但一旦开始将其应用于 3 条或更多的折线时，渐变可能会开始混合在一起，变得非常混乱。

# 带有 CyberPunk 样式的雷达图

雷达图（蜘蛛图或雷达图）是一种流行且广泛使用的数据可视化工具。它们允许我们通过在二维图表上同时显示多个变量来比较数据集。

生成雷达图需要更多的 matplotlib 代码，以使它们显示得更好。

```py
from matplotlib.patches import Patch

# Set up the data so that we can close the 'loop' of the area
categories = [*categories, categories[0]]
values_1 = [*data['Value1'], data['Value1'][0]]
values_2 = [*data['Value2'], data['Value2'][0]]
values_3 = [*data['Value3'], data['Value3'][0]]

# Set up the label potisions around the circle circumference
label_loc = np.linspace(start=0, stop=2 * np.pi, num=len(categories))

# Set up the figure
fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

# Add our data as separate axes
ax.plot(label_loc, values_1, lw=2)
ax.plot(label_loc, values_2, lw=2)
ax.plot(label_loc, values_3, lw=2)

# Apply a fill to our lines
ax.fill(label_loc, values_1, alpha=0.3)
ax.fill(label_loc, values_2, alpha=0.3)
ax.fill(label_loc, values_3, alpha=0.3)

# Convert the lines and labels to a polar grid
lines, labels = plt.thetagrids(np.degrees(label_loc), labels=categories)

# Setup the radial lines
ax.set_ylim(0, 20)
ax.set_yticks(np.arange(0, 20, 2))

# Create custom legend handles
values_1_legend = Patch(facecolor='C0', alpha=0.5, label='Values1')
values_2_legend = Patch(facecolor='C1', alpha=0.5, label='Values2')
values_3_legend = Patch(facecolor='C2', alpha=0.5, label='Values3')

# Add a legend with custom position and handles
ax.legend(handles=[values_1_legend, values_2_legend, values_3_legend],
          bbox_to_anchor=(1.3, 0.2), fontsize=20, 
          frameon=True)

plt.show()
```

当我们运行上述代码时，我们将得到以下的雷达图，该图使用了标准的 matplotlib 主题。

![](img/a53eb5c45362e696dc401eb526796cd0.png)

基本的 matplotlib 雷达图（蜘蛛图）。图片由作者提供。

我们应用之前相同的代码，将基本的雷达图转换为 CyberPunk 主题。

但我们还需要更改坐标轴和网格，以便能看到它们。

```py
with plt.style.context('cyberpunk'):
    fig, ax = plt.subplots(figsize=(10,10), subplot_kw=dict(polar=True))

    # Add our data as separate axes
    ax.plot(label_loc, values_1, lw=2)
    ax.plot(label_loc, values_2, lw=2)
    ax.plot(label_loc, values_3, lw=2)

    # Apply a fill to our lines
    ax.fill(label_loc, values_1, alpha=0.3)
    ax.fill(label_loc, values_2, alpha=0.3)
    ax.fill(label_loc, values_3, alpha=0.3)

    # Convert the lines and labels to a polar grid
    lines, labels = plt.thetagrids(np.degrees(label_loc), labels=categories)

    # Set up the grid and line properties
    ax.tick_params(axis='both', which='major', pad=30, labelsize=15)

    ax.spines['polar'].set_linewidth(3)

    edge_color = (1, 1, 1, 0.2) 
    ax.spines['polar'].set_color(edge_color) 

    ax.grid(color='white', alpha=0.3)

    # Setup the radial lines
    ax.set_ylim(0, 20)
    ax.set_yticks(np.arange(0, 20, 2))

    # Create custom legend handles
    values_1_legend = Patch(facecolor='C0', alpha=0.5, label='Values1')
    values_2_legend = Patch(facecolor='C1', alpha=0.5, label='Values2')
    values_3_legend = Patch(facecolor='C2', alpha=0.5, label='Values3')

    # Add a legend with custom position and handles
    ax.legend(handles=[values_1_legend, values_2_legend, values_3_legend],
              bbox_to_anchor=(1.3, 0.2), fontsize=20, 
              frameon=True)

    mplcyberpunk.add_glow_effects()

    plt.show()
```

当代码重新运行时，我们现在得到了一个 CyberPunk 风格的雷达图。

![](img/2955246d929d157ab10796e991fa20c7.png)

应用 CyberPunk 主题后的 matplotlib 雷达图。

如果你想了解更多关于创建雷达图的内容，可以查看我之前的文章：

[](/create-stunning-radar-plots-with-matplotlib-6a8e05054ff9?source=post_page-----96f4d473185d--------------------------------) ## 使用 Matplotlib 创建惊艳的雷达图

### 使用 Python 最流行的数据可视化库来可视化地质数据

towardsdatascience.com

# 使用子图和 CyberPunk 样式

在使用 matplotlib 创建图形时，我们通常希望显示多个图表。图表的类型可以根据你希望展示的内容有所不同。

这个例子展示了如何创建一个包含 4 个子图的 matplotlib 图形：2 个折线图、1 个散点图和 1 个条形图。

为了确保应用了我们的 CyberPunk 样式，我们可以添加以下代码： `with plt.style.context(‘cyberpunk’):`

如果我们想要应用 mplcyberpunk 库中的任何特殊样式，例如光晕，我们需要通过将 `ax` 对象传递给 `ax` 参数来指定要应用该样式的轴。

例如： `mplcyberpunk.make_lines_glow(ax=ax4)`

```py
with plt.style.context('cyberpunk'):
    fig = plt.figure(figsize=(15, 10))

    ax1 = plt.subplot2grid((2,3), (0,0))
    ax2 = plt.subplot2grid((2,3), (0,1))
    ax3 = plt.subplot2grid((2,3), (0,2))
    ax4 = plt.subplot2grid((2,3), (1,0), colspan=3)

    # Line Plot 1
    x_data = np.linspace(0, 20, 50)
    y_data_clean = np.sin(x_data)
    noise = np.random.normal(0, 2, y_data_clean.shape)
    y_data_noise = y_data_clean + noise

    ax1.plot(x_data, y_data_noise)
    ax1.plot(x_data, y_data_clean, lw=2)
    ax1.set_title('Plot 1', fontsize=14)

    # Scatter Plots
    ax2.scatter(x=(np.random.randint(0, 20, 100)), y=np.random.randint(0,20, 100))
    ax2.scatter(x=(np.random.randint(0, 20, 100)), y=np.random.randint(0,20, 100))
    ax2.set_title('Plot 2', fontsize=14)

    # Bar Chart
    bar_values = np.array(["A", "B", "C", "D"])
    bar_heights = ([20, 10, 5, 7])
    bar_heights_2 = ([10, 15, 2, 18])
    bar_width = 0.35

    bar_pos_1 = np.arange(len(bar_values))
    bar_pos_2 = [x + bar_width for x in bar_pos_1]

    # Make sure the bars are offset from each other
    ax3.bar(x=bar_pos_1, height=bar_heights, width=bar_width, label='Group 1')
    ax3.bar(x=bar_pos_2, height=bar_heights_2, width=bar_width, label='Group 2')
    ax3.set_xticks([r + bar_width / 2 for r in range(len(bar_heights))])
    ax3.set_xticklabels(bar_values)
    ax3.set_title('Plot 3', fontsize=14)

    # Bottom Axis
    values = np.arange(0, 10, 0.1)
    sine_wave1 = np.sin(values)
    sine_wave2 = sine_wave1 * 2
    sine_wave3 = sine_wave1 + 0.5

    ax4.plot(sine_wave1)
    ax4.plot(sine_wave2)
    ax4.plot(sine_wave3)
    ax4.set_title('Plot 4', fontsize=14)

    mplcyberpunk.make_lines_glow(ax=ax1)
    mplcyberpunk.make_lines_glow(ax=ax4)

    plt.suptitle('Interesting Inforgraphic Using CyberPunk', fontsize=25)
    plt.show()
```

当代码运行时，我们得到以下图表：

![](img/fea8b724924c77103e370ccfe95d5961.png)

应用 cyberpunk 主题后的 matplotlib 图形，具有多个子图。图片由作者提供。

# 概要

使用 CyberPunk 风格库，我们可以轻松而迅速地将无聊的 matplotlib 图形转变为具有未来感的外观，全部不需要添加大量新的代码行。

然而，正如文章开头提到的，在应用主题之前，始终重要的是考虑你的目标受众以及你希望通过数据可视化实现的目标。

祝你 CyberPunk 玩得愉快！

*感谢阅读。在你离开之前，你应该订阅我的内容，并将我的文章发送到你的邮箱。* [***你可以在这里做到这一点！***](https://andymcdonaldgeo.medium.com/subscribe)*另外，你也可以* [***注册我的新闻通讯***](https://fabulous-founder-2965.ck.page/2ca286e572) *以免费获得额外的内容直接发送到你的邮箱。*

*其次，你可以通过注册会员获得完整的 Medium 体验，并支持其他成千上万的作者和我。这只需每月 $5，你就可以全面访问所有精彩的 Medium 文章，并有机会通过写作赚取收入。*

*如果你通过* [***我的链接***](https://andymcdonaldgeo.medium.com/membership)***注册***，*你将通过你费用的一部分直接支持我，而这不会增加你的费用。如果你这样做，非常感谢你的支持。*
