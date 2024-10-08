# 讲述故事的图表：将数据可视化转变为更有意义的东西

> 原文：[`towardsdatascience.com/charts-that-tell-a-story-turn-a-plotly-visualization-into-something-more-a723e427d5aa`](https://towardsdatascience.com/charts-that-tell-a-story-turn-a-plotly-visualization-into-something-more-a723e427d5aa)

[](https://medium.com/@kurt.klingensmith?source=post_page-----a723e427d5aa--------------------------------)![Kurt Klingensmith](https://medium.com/@kurt.klingensmith?source=post_page-----a723e427d5aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a723e427d5aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a723e427d5aa--------------------------------) [Kurt Klingensmith](https://medium.com/@kurt.klingensmith?source=post_page-----a723e427d5aa--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a723e427d5aa--------------------------------) ·阅读时间 9 分钟·2023 年 3 月 29 日

--

![](img/2d64ae634a4dc6519ffaf708147a18f7.png)

照片由 [Erik Mclean](https://unsplash.com/@introspectivedsgn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/bOJC9SwFfQk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄。

数据可视化传达了数据框和表格无法表达的想法。然而，通过数据可视化有效讲述故事需要一个具有美感、可解释的图表，这样图表才能提供必要的上下文，自成一体。

幸运的是，Python 包含了许多数据可视化库，例如 [Plotly Express](https://plotly.com/python/plotly-express/)，可以通过一行代码快速创建图表 [1]。虽然这些图表很有用，但它们在正式出版物中很少能单独存在，或者在没有提供更多上下文的情况下经受得住审查；一个讲述数据故事的专业、独立图表需要额外的工作。本文介绍了将数据可视化提升到下一个水平的步骤。

## 代码：

本教程的代码可以在 [**GitHub 页面链接**](https://github.com/kurtklingensmith/Visualizations) 上获取。请随意下载代码，并在 Jupyter notebook 中跟随操作——点击“代码”和“下载 ZIP”即可获取 ipynb 文件。

# 1\. 数据准备和初步可视化

使用的库有：

```py
# Data Handling
import pandas as pd

# Data visualization Libraries
import seaborn as sns
import plotly.express as px
import plotly.io as pio
```

回到一个老牌的数据集，[Seaborn 库](https://seaborn.pydata.org) 的汽车数据集（标记为‘mpg’）提供了能源危机时期的车辆燃料消耗的优质数据 [2]。通过使用 groupby 函数，这些数据将提供 1970 年至 1982 年间每加仑平均行驶里程（MPG）。以下是加载和准备数据框的代码：

```py
# Load in data:
mpg = sns.load_dataset('mpg')

# Get dataset showing average MPG per year by using groupby:
mpg = mpg.groupby(['model_year'])['mpg'].mean().to_frame().reset_index()

# Rename columns:
mpg = mpg.rename(columns={'model_year': 'Year',
                          'mpg': 'Average MPG'})
mpg.head()
```

数据框的头部应该如下所示：

![](img/b2ceecdcbc0021090490a730dc198cfd.png)

作者截图。

一行代码在 plotly express 中生成以下柱状图：

```py
px.bar(mpg, x='Year', y='Average MPG')
```

![](img/fa78a65a40c47f0ef7aaecae1899b5c1.png)

作者截图。

仅需几行代码即可准备和可视化数据。但这样的可视化是否值得在杂志或正式商业报告中发表？可能不够。

# 2\. 下一层级：标签、格式、颜色

首先，让我们来看一下图表的颜色方案。运行以下代码将更新颜色值和模板：

```py
# Generate base plot:
plot = px.bar(mpg, x='Year', y='Average MPG', color='Average MPG',
              color_continuous_scale=px.colors.diverging.RdYlGn)

# Remove colorbar:
plot.update_coloraxes(showscale=False)

# Update plotly style:
plot.update_layout(template='plotly_white')

plot.show()
```

更新后的图表现在如下所示：

![](img/d5297f43b9abd8713a05848cd7f5bec2.png)

作者截图。

上述代码的第一个更改是将“平均 MPG”列添加到颜色中。虽然从技术上讲是多余的（“平均 MPG” 已在 y 值或柱状高度中捕获），但将颜色指定为“平均 MPG”允许应用颜色刻度以帮助快速解释。在这种情况下，Plotly 的发散颜色刻度“RdYlGn”与数据很好地映射：较低效的年份为红色，而较高效的年份为绿色，中间则是橙色和黄色。在[这里](https://plotly.com/python/builtin-colorscales/)发现其他 Plotly 颜色方案[3]。

由于颜色和 y 值相同，Plotly 通常包含的颜色条是多余的；上述代码将其去除。最后，代码将默认的 Plotly 主题更改为“plotly_white”。有关 Plotly 主题的更多信息，请[点击这里](https://plotly.com/python/templates/) [4]。

**注意：** 如果考虑无色盲友好的方案，请阅读这篇《Towards Data Science》文章，了解如何调整颜色方案[5]。在这篇文章中，保留了红黄绿颜色方案，既作为概念验证，也因为 MPG 信息仍通过 y 轴（柱状高度）值传达。

# 3\. 标签

接下来的步骤标记轴：

```py
# Label axes:
plot.update_xaxes(title='Model Year',
                  dtick=1)
plot.update_yaxes(title='Average Miles Per Gallon (MPG)')
```

Plotly 通常会将轴标题默认为指定轴的数据列标题，但[**update_xaxes()** 和 **update_yaxes()**](https://plotly.com/python/axes/)允许进一步自定义[6]。请注意，MPG 被拼写为“每加仑英里数”以避免用缩写混淆观众。此外，对于 x 轴，“dtick” 值被设置为 1；这确保了 x 轴上的每个柱状图下方都有一个型号年份值。

以下代码更新标题：

```py
# Update plot layout:
plot.update_layout(
    title=dict(
        text='<b>Average Miles Per Gallon of Cars Over Time</b>\
              <br><i><sup>A Visualization of Improvements in \
              Fuel Efficiency During the Energy Crisis Era</sup></i>',
        x=0.085,
        y=0.95,
        font=dict(
            family='Helvetica',
            size=25,
            color='#272b4f'
        )))
```

函数 [**update_layout**()](https://plotly.com/python/reference/layout/) 允许添加标题[7]。请注意，文本包含 HTML 风格代码：<b> 和 </b> 用于加粗文本；<i> 和 </i> 用于斜体；<br> 代表分页符；最后，<sup> 代表图表副标题的上标。标题中的 \ 使文本块在下一行代码上继续，从而创建更整洁的代码块。

字体也可以调整，字体的颜色也是如此。可以使用 Google 的 [**颜色选择器**](https://g.co/kgs/KHVrPZ) [8] 轻松找到其他颜色。

最后，我们使用 [**add_annotation**()](https://plotly.com/python/text-and-annotations/) 函数在图表上注释数据源[9]：

```py
# Add annotation on data source:
plot.add_annotation(x=0,
                    y=-0.15,
                    showarrow=False,
                    text="<i>Fuel mileage data courtesy of \
                          Python Seaborn Library</i>",
                    textangle=0,
                    xanchor='left',
                    xref="paper",
                    yref="paper",
                    font_color='#a6aeba')
```

所有这些最终产生了以下结果：

![](img/1c94f5a63974d98a8b5eb05e4e78b4e4.png)

作者截屏。

注意图表有标题和副标题，引入了数据的背景，同时轴提供了明确的标签，条形的高度和颜色迅速显示了每年的平均 MPG。此外，底部有数据源参考。这可能是一个适合出版的图表的良好停止点，但为了展示可视化的进一步可能性，下一部分将介绍更多选项。

# 4\. 添加注释

为了为图表增加更多背景，以下代码创建了一条表示 1970 年至 1982 年平均 MPG 的水平线以及一个描述该线的注释框：

```py
# Add average MPG across era:
plot.add_hline(y=mpg['Average MPG'].mean())

# Add explanation of line:
plot.add_annotation(x=.05,
                    y=0.67,
                    text="Average MPG, 1970 through 1982",
                    textangle=0,
                    xanchor='left',
                    xref="paper",
                    yref="paper",
                    font_color='black',
                    bordercolor='black',
                    borderpad=5,
                    showarrow=True,
                    arrowhead=2,
                    bgcolor='white',
                    arrowside='end'
                    )
```

添加到图表中后，结果如下：

![](img/293e6bbeb1beb33b46ba81e9d296f96e.png)

作者截屏。

注意 `add_annotation()` 函数允许创建边框（bordercolor=‘black’）和箭头。`add_annotation()` 函数中的 x 和 y 值定位标注框，可能需要一些试验和错误以将框放置在所需的位置。

这条线和标注框进一步强调了 1980 年、1981 年和 1982 年 MPG 的显著提升。此外，它还显示了 1975 年至 1979 年期间，平均 MPG 年年稳步提升。如果需要突出显示这段改进时期，可以使用以下代码完成：

```py
# Add highlight box:
plot.add_vrect(x0="74.5",
               x1="79.5",
               fillcolor="lightgray",
               opacity=0.3,
               line_width=0)

# Add explanation of line:
plot.add_annotation(x=.45,
                    y=0.9,
                    text="Period of Consistent Improvement
                         <br>until Breakthrough in 1980's",
                    textangle=0,
                    xanchor='left',
                    xref="paper",
                    yref="paper",
                    font_color='black',
                    showarrow=False,
                    )
```

函数 [**add_vrect()**](https://plotly.com/python/horizontal-vertical-shapes/) 创建一个矩形框，用于突出显示图表的特定部分[10]；在这种情况下，它突出显示了 1970 年代末期的一段持续改进的时期。`add_vrect()` 代码的位置很重要；将其放在 `add_hline()` 函数之后意味着它会在水平线的上方，而不是下面，如下所示：

![](img/29b09c0b41b22b79f53ac6f46a219212.png)

作者截屏。

可能看起来没有其他需要添加的内容，但还有一个可能性：解释燃油经济性的提升。**假设该图表是某项研究的一部分，该研究发现发动机尺寸的减小直接促成了每加仑英里数的提升**。幸运的是，Seaborn MPG 数据包括发动机排量数据。经过以下的一些数据准备以及一些新的注释，最终的图表准备好了：

```py
# Data prep:
displacement = sns.load_dataset('mpg')
seventies = round(
    displacement[displacement['model_year'] < 80]['displacement'].mean(), 2)
eighties = round(
    displacement[displacement['model_year'] >= 80]['displacement'].mean(), 2)

# Create text string:
explanation = "<b>Why the Improvement in MPG?</b> <br>\
               In the 70's, average engine size was {} <br>\
               cubic inches versus {} from 1980 to 1982.<br>\
               Larger engines are usually less efficient.".format(seventies, eighties)

# Add explanation for trends:
plot.add_annotation(x=.615,
                    y=0.02,
                    text=explanation,
                    textangle=0,
                    xanchor='left',
                    xref="paper",
                    yref="paper",
                    font_color='black',
                    bordercolor='black',
                    borderpad=5,
                    bgcolor='white',
                    showarrow=False
                    )
```

数据准备部分计算了 1970 年代（70 到 79 年）的平均发动机排量以及 1980 年代的三年（80、81 和 82 年）。这些数据随后被传入文本字符串中，用于 `add_annotation()` 函数中的文本值。最终图表如下所示：

![](img/c0f396f3f8410dec5b6cec4a7166c491.png)

作者截屏。

这个图表现在可以独立存在，并讲述能源危机时代的年均燃油里程的故事。一些最后的考虑事项：

+   少即是多：当图表已经达到能够传达必要信息的能力时，就停止添加内容。

+   一个独立的信息图可能需要更多的项目，而在口头报告中或作为书面报告的一部分展示的图表，则可以用更少的项目来满足要求。

+   颜色方案、字体选择和尺寸影响可读性和无障碍性。

+   如果客户无法理解图表的含义，那不是客户的错。

# 5\. 结论

Python 的各种可视化库，包括 Plotly Express，提供了一种快速生成高度可定制图表的方法，这些图表可以从基础可视化到完全自定义的独立产品。通过理解如何最好地传达故事来构建可视化，能够产生最有效的传递效果。自己动手尝试代码，并随时 [**在 GitHub 页面下载完整笔记本**](https://github.com/kurtklingensmith/Visualizations)。

## 在一个代码块中生成最终图表：

```py
# Load in Libraries:

# Data Handling
import pandas as pd

# Data visualization Libraries
import seaborn as sns
import plotly.express as px

# Load in data:
mpg = sns.load_dataset('mpg')
mpg.head()

# Get dataset showing average MPG per year by using groupby:
mpg = mpg.groupby(['model_year'])['mpg'].mean().to_frame().reset_index()

# Rename columns:
mpg = mpg.rename(columns={'model_year': 'Year',
                          'mpg': 'Average MPG'})

# Generate base plot:
plot = px.bar(mpg, x='Year', y='Average MPG', color='Average MPG',
              color_continuous_scale=px.colors.diverging.RdYlGn)

# Remove colorbar:
plot.update_coloraxes(showscale=False)

# Update plotly style:
plot.update_layout(template='plotly_white')

# Label axes:
plot.update_xaxes(title='Model Year',
                  dtick=1)
plot.update_yaxes(title='Average Miles Per Gallon (MPG)')

# Add labels and source:

# Update plot layout:
plot.update_layout(
    title=dict(
        text='<b>Average Miles Per Gallon of Cars Over Time</b>\
              <br><i><sup>A Visualization of Improvements in \
              Fuel Efficiency During the Energy Crisis Era</sup></i>',
        x=0.085,
        y=0.95,
        font=dict(
            family='Helvetica',
            size=25,
            color='#272b4f'
        )))

# Add annotation on data source:
plot.add_annotation(x=0,
                    y=-0.15,
                    showarrow=False,
                    text="<i>Fuel mileage data courtesy of \
                          Python Seaborn Library</i>",
                    textangle=0,
                    xanchor='left',
                    xref="paper",
                    yref="paper",
                    font_color='#a6aeba')

# Add highlight box:
plot.add_vrect(x0="74.5",
               x1="79.5",
               fillcolor="lightgray",
               opacity=0.3,
               line_width=0)

# Add explanation of line:
plot.add_annotation(x=.45,
                    y=0.9,
                    text="Period of Consistent Improvement\
                         <br>until Breakthrough in 1980's",
                    textangle=0,
                    xanchor='left',
                    xref="paper",
                    yref="paper",
                    font_color='black',
                    showarrow=False,
                    )

# Add average MPG across era

# Create Line:
plot.add_hline(y=mpg['Average MPG'].mean())

# Add explanation of line:
plot.add_annotation(x=.05,
                    y=0.67,
                    text="Average MPG, 1970 through 1982",
                    textangle=0,
                    xanchor='left',
                    xref="paper",
                    yref="paper",
                    font_color='black',
                    bordercolor='black',
                    borderpad=5,
                    showarrow=True,
                    arrowhead=2,
                    bgcolor='white',
                    arrowside='end'
                    )

# Add a box to explain the trends

# Data prep:
displacement = sns.load_dataset('mpg')
seventies = round(
    displacement[displacement['model_year'] < 80]['displacement'].mean(), 2)
eighties = round(
    displacement[displacement['model_year'] >= 80]['displacement'].mean(), 2)

# Create text string:
explanation = "<b>Why the Improvement in MPG?</b> <br>\
               In the 70's, average engine size was {} <br>\
               cubic inches versus {} from 1980 to 1982.<br>\
               Larger engines are usually less efficient.".format(seventies, eighties)

# Add explanation for trends:
plot.add_annotation(x=.615,
                    y=0.02,
                    text=explanation,
                    textangle=0,
                    xanchor='left',
                    xref="paper",
                    yref="paper",
                    font_color='black',
                    bordercolor='black',
                    borderpad=5,
                    bgcolor='white',
                    showarrow=False
                    )

plot.show()
```

## 参考文献：

[1] Plotly, [Python 中的 Plotly Express](https://plotly.com/python/plotly-express/) (2023)。

[2] Seaborn, [Seaborn：统计数据可视化](https://seaborn.pydata.org) (2023)。

[3] Plotly, [Python 中的内置连续颜色比例尺](https://plotly.com/python/builtin-colorscales/) (2023)。

[4] Plotly, [Python 中的主题和模板](https://plotly.com/python/templates/) (2023)。

[5] CR Ferreira, 创建色盲友好型数据可视化的两个简单步骤 (2020)，数据科学前沿。

[6] Plotly, [Python 中的坐标轴](https://plotly.com/python/axes/) (2023)。

[7] Plotly, [Python 中的布局](https://plotly.com/python/reference/layout/) (2023)。

[8] Google, [Google 颜色选择器](https://g.co/kgs/KHVrPZ) (2023)。

[9] Plotly, [Python 中的文本和注释](https://plotly.com/python/text-and-annotations/) (2023)

[10] Plotly, [Python 中的水平和垂直线条及矩形](https://plotly.com/python/horizontal-vertical-shapes/) (2023)。
