# 使用 Python 制作美观（且实用）的意大利面图

> 原文：[`towardsdatascience.com/make-beautiful-and-useful-spaghetti-plots-with-python-ec4269d7e8c9`](https://towardsdatascience.com/make-beautiful-and-useful-spaghetti-plots-with-python-ec4269d7e8c9)

## 堆叠折线图现在非常热门！

[](https://medium.com/@lee_vaughan?source=post_page-----ec4269d7e8c9--------------------------------)![李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----ec4269d7e8c9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec4269d7e8c9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec4269d7e8c9--------------------------------) [李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----ec4269d7e8c9--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec4269d7e8c9--------------------------------) ·9 分钟阅读·2023 年 8 月 16 日

--

![](img/fedaf4e55bedea5aab332d21b73cc8b7.png)

照片由 [Hunter Harritt](https://unsplash.com/@hharritt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/photos/Ype9sdOPdYc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

最近出现了很多关于气候变化的文章，其中许多使用了独特的*堆叠折线图*来总结几十年的数据。这里是一个来自 [*气候再分析器*](https://climatereanalyzer.org/clim/sst_daily/) 的示例，展示了过去一年半海温远高于平均水平[1]：

![](img/ab6c58584ac93c1b200f233ca2eefaed.png)

全球（60S-60N）海面温度（1981–2023）[1]

这里是来自 Dr. Zachary Labe 的 [网站](https://zacklabe.com/antarctic-sea-ice-extentconcentration/) 的类似图表，展示了过去 40 多年南极海冰的范围[2]：

![](img/d15623fc1525214fdbbbf8d9a3bd1368.png)

南极海冰范围（1978–2023）[2]

这些图表已经成为*信息图*的热门选择，如 [这篇文章](https://www.theguardian.com/world/2023/jul/29/something-weird-is-going-on-search-for-answers-as-antarctic-sea-ice-stays-at-historic-lows) 中所示，但这种受欢迎程度有些令人惊讶[3]。由于很难跟踪这些密集、缠绕的显示中的单条线，它们通常被回避并被贬低为“意大利面”图。

但要成功使用意大利面图有一个秘诀。你必须*强调*一两条线，并将它们置于所有其他线条的*减弱*背景中。这种策略让你能将选定的线条放置在整体背景下。它们代表*正常*结果还是*异常值*？结果是*非常好*还是*非常差*？通过将它们叠加在背景趋势上，故事可以自然而然地展开。

在这个*快速成功的数据科学*项目中，我们将使用 Plotly Express 绘图库生成前 Antarctic Sea Ice 图表的副本。通过这个代码示例，你应该能够为自己的数据集生成类似的图表。

# 国家雪冰数据中心

对于数据，我们将使用由[*国家雪冰数据中心*](https://nsidc.org/home)编制的全面公共数据集，该中心是科罗拉多大学博尔德分校环境科学合作研究所（CIRES）的一个部分[4]。该数据集利用卫星图像追踪和监测极地海冰的变化，例如南极洲周围的“光晕”。

![](img/ddae3e9de58928b91470652d170fe1bc.png)

2023 年 8 月 10 日海冰范围基于卫星图像[4]

数据以每月和每天的增量提供。为了获得尽可能高的分辨率，我们将查看每日数据。为了方便起见，我已将 CSV 文件下载到这个[ gist](https://gist.github.com/rlvaugh/219460e980f44fa72e3259118b0ce512)。此外，用户指南可以在[这里](https://nsidc.org/sites/default/files/g02135-v003-userguide_1_1.pdf#page=30)找到。

# 安装库

*Plotly Express* 是 *Plotly* 绘图库的高级版本，可以制作美丽且高度互动的可视化。你可以通过 conda 或 pip 安装它。

这是 conda 安装：

`conda install -c plotly plotly_express`

这是 pip 版本：

`pip install plotly`

我们还需要 pandas 数据分析包。可以通过以下方式安装：

`conda install pandas`

或：

`pip install pandas`

你可能还需要`nbformat`用于 Plotly Express。可以通过以下方式安装：

`conda install -c conda-forge nbformat`

或：

`pip install nbformat`

# 代码

以下代码是在 Jupyter Lab 中编写的，并按*单元*呈现。

## 导入库

这里是导入。我们使用别名以便于输入：

```py
import pandas as pd
import plotly.graph_objects as go
import plotly.io as plt_io
import plotly.express as px
```

通常，导入 Plotly Express 就足够了。然而，包括 Plotly 的`graph_objects`模块则提供了更多的自定义选项（想想 matplotlib 与 seaborn 的区别）。`plotly.io`模块将让我们导入 Plotly 的现成设计模板，从而节省工作量。

## 加载和准备数据

以下注释的代码使用 pandas 库从 Gist 加载数据并为绘图做准备。这部分包括为一年中的每一天创建一个新的 DataFrame 列（1 月 1 日= 1，12 月 31 日= 365（非闰年），366（闰年））。我们将使用这个`Day of Year`列作为折线图中的 x 轴。

```py
# Read sea ice extent file:
URL = 'https://bit.ly/3OtPnnh'
df = pd.read_csv(URL, skiprows=[1])
df.columns = df.columns.str.strip()  # Strip any leading white spaces.
df.drop(columns=['Missing', 'Source Data'], inplace=True)

# Combine date columns into a single datetime column:
df['Date'] = pd.to_datetime(df[['Year', 'Month', 'Day']])

# Extract the day of the year from the 'Date' column:
df['Day of Year'] = df['Date'].dt.dayofyear

# Move Date column to the far left:
column_to_move = df.pop("Date")
df.insert(0, "Date", column_to_move)

df.head(3)
```

![](img/df5bfae9f114327632d1345e4111dd5d.png)

## 绘制堆叠折线图

以下注释的代码绘制了海冰范围的堆叠折线图。每年的线条首先以浅灰色绘制。然后将 2022 年和 2023 年的线条分别以黑色和红色绘制，并加粗线条。目的是展示冰层在过去两年中退缩的程度。

```py
# Plot each year's extent data in a stacked line chart:
fig = px.line(df, 
              x='Day of Year', 
              y='Extent', 
              line_group='Year', 
              color='Year',
              labels={'x': 'Month', 'y': 'Extent'},
              title='Antarctic Sea Ice Extent January to December (1978-2023)',
              template='plotly_white')

# Customize layout; tickvals represent starting 'day of year' of each month:
fig.update_layout(width=800,
                  height=650,
                  legend={'orientation': 'h'},
                  xaxis_title='',
                  yaxis_title='Sea Ice Extent (million sq km)',
                  xaxis={'tickmode':'array',
                         'tickvals': [1, 32, 60, 91, 121, 152, 
                                      182, 213, 244, 274, 305, 336], 
                         'ticktext': ['Jan', 'Feb', 'Mar', 'Apr', 
                                      'May', 'Jun', 'Jul', 'Aug', 
                                      'Sep', 'Oct', 'Nov', 'Dec']})

# Draw border around the plot:
fig.update_xaxes(showline=True, linewidth=1, linecolor='black', mirror=True)
fig.update_yaxes(showline=True, linewidth=1, linecolor='black', mirror=True)

# Update trace styles to make all lines light gray:
fig.update_traces(line={'color': "lightgray", 'width': 0.75})

# Highlight selected years:
fig.update_traces(patch={'line': {'color': 'black', 'width': 2}},
                  selector={'legendgroup': '2022'})
fig.update_traces(patch={'line': {'color': 'red', 'width': 2}},
                  selector={'legendgroup': '2023'})

# Add annotation:
fig.add_annotation(dict(font=dict(color='darkgray',size=15),
                        x=85,
                        y=16,
                        showarrow=False,
                        text='All years 1978-2023----',
                        textangle=0,
                        xanchor='left'))

fig.show()
```

![](img/67aae9fcb963efa705bf151079e77672.png)

使用 Plotly Express 构建的南极海冰面积图（图像由作者提供）

哇，真是个美丽的图表！使用 Plotly Express，图例是“动态”的。这意味着你可以点击一个年份，它会从图表中消失。双击一个年份，所有其他线条都会消失。真是解开意大利面图的简便方法！

如果你想使用*暗色主题*，只需将调用`px.line()`方法时的`template`参数替换为`plotly_dark`。你还需要在`fig.update_xaxes()`和`fig.update_yaxes()`方法中将边框的线条颜色更改为`white`，并在第一次调用`fig.update_traces()`时为 2022 年更改线条颜色。结果如下：

![](img/c32a4d9c5c0a28905a7e631d9a3b03b2.png)

使用“plotly_dark”主题的南极海冰面积图（图像由作者提供）

要将这些图表转化为持久的静态图像，只需点击 Plotly 工具栏上的“相机”图标。这将把图表保存为*PNG* 文件。

## 使用填充颜色

显示大多数线条为*浅色*的另一种替代方法是使用*实心填充颜色*来表示*最大*和*最小*的范围。以下是 Zack Labe 的一个吸引人的例子 [2]：

![](img/b1ae7ba453f893b7a08d6aa0a83c00cb.png)

南极海冰异常（1979–2022） [2]

让我们用之前的图表尝试这种方法。第一步是计算按年份中的天（`Day of Year`）分组的`Extent`列的统计数据。对于每个按天分组的结果，pandas 的`agg()`（聚合）方法将帮助我们找到最小值和最大值。我们会将结果保存在一个名为`bounds`的新 DataFrame 中。

```py
# Calculate minimum and maximum bounds of "Extent" for each day of the year:
bounds = df.groupby('Day of Year')['Extent'].agg(['min', 'max']).reset_index()
bounds.rename(columns={'min': 'Min Extent', 'max': 'Max Extent'}, inplace=True)
bounds.head(3)
```

![](img/f0ea650ee9069b3a6caf2bc76c1043a7.png)

如果你查看原始 CSV 数据文件，你会发现数据是每隔一天收集一次。多年来，每一天都会被采样，但不是每个*特定*的年份。

![](img/9119a3d3ac5650f96adac1c15e1c9d98.png)

CSV 数据文件的开头（图像由作者提供）

因为*相邻*天的范围值可能来自*不同*的年份，所以我们可能会得到“颤动”的曲线。这似乎在最小范围值上是一个更大的问题：

![](img/f25721b971294e30ddbce8b88f5389ac.png)

不规则采样导致了锯齿状的最小范围曲线（图像由作者提供）

## 平滑最小范围曲线

为了平滑这条锯齿状的曲线，我们只需要对数据进行 2 天的[*移动平均*](https://en.wikipedia.org/wiki/Moving_average)。这涉及到在列上调用 pandas 的 `rolling()` 方法，传入`2`，然后调用 `mean()` 方法。由于第一行之前没有数据，它将被分配一个 `NaN` 值，所以我们将其从 DataFrame 中删除。

```py
# Smooth the "Min Extent" using a 2-day simple moving average (SMA):
bounds['Min SMA2'] = bounds['Min Extent'].rolling(2).mean()
bounds = bounds.iloc[1:]  # Remove first row with NaN for SMA2.
bounds.head(3)
```

![](img/767988fee4d972ca85da6e667de58568.png)

## 突出显示最近两年

为了在新的图表中轻松突出显示 2022 年和 2023 年的海冰范围，我们将过滤原始 DataFrame（`df`），以创建两个新的 DataFrame。

```py
# Filter data for plotting specific years:
df_2022 = df[(df['Year'] >= 2022) & (df['Year'] < 2023)].copy().reset_index()
df_2023 = df[df['Year'] >= 2023].copy().reset_index()
```

## 绘制填充图表

以下注释代码生成了填充线图。由于 Plotly 的`plotly_dark`模板并不是真正的黑色，前几步创建了一个*自定义*模板，其中所有元素都是黑色的。这样的控制使用*完整的 Plotly*会更容易，而不是使用更高层次的 Plotly Express 包。

接下来，我们将使用 Plotly 的`go.Scatter()`方法，并传递填充曲线下方区域的参数。对于上面的`Max Extent`曲线，我们将使用`tonexty`参数填充曲线下方的区域为深灰色。然后，对于平滑的`Min SMA2`曲线，我们将使用`tozeroy`参数用黑色填充其下方，覆盖之前的深灰色。

```py
# Load the dark template:
plt_io.templates["custom_dark"] = plt_io.templates["plotly_dark"]

# Customize the template using all black background colors:
plt_io.templates["custom_dark"]['layout']['paper_bgcolor'] = '#000000'
plt_io.templates["custom_dark"]['layout']['plot_bgcolor'] = '#000000'
# Customize gridline colors:
plt_io.templates['custom_dark']['layout']['yaxis']['gridcolor'] = '#000000'
plt_io.templates['custom_dark']['layout']['xaxis']['gridcolor'] = '#000000'

# Create a figure:
fig = go.Figure()

# Add filled area traces for max and min extents:
fig.add_trace(go.Scatter(x=bounds['Day of Year'], y=bounds['Max Extent'], 
                         fill='tonexty', fillcolor='darkgray', 
                         line=dict(color='lightgrey', width=0.75)))

fig.add_trace(go.Scatter(x=bounds['Day of Year'], y=bounds['Min SMA2'], 
                         fill='tozeroy', fillcolor='black', 
                         line=dict(color='lightgrey', width=0.75)))

# Add traces for 2022 and 2023
fig.add_trace(go.Scatter(x=df_2022['Day of Year'], y=df_2022['Extent'], 
                         mode='lines', 
                         marker=dict(color='white', size=4), 
                         name='2022'))

fig.add_trace(go.Scatter(x=df_2023['Day of Year'], y=df_2023['Extent'], 
                         mode='lines', 
                         marker=dict(color='red', size=4), 
                         name='2023'))

# Update layout
fig.update_layout(
    width=800,
    height=650,
    template='custom_dark',
    title=dict(text='Antarctic Sea Ice Extent (1978-2023)', 
               font=dict(size=30)),
    showlegend=False,
    xaxis_title='Month',
    yaxis_title='Sea Ice Extent (million sq km)',
    xaxis=dict(tickmode='array',
               tickvals=[1, 32, 60, 91, 121, 152, 182, 213, 244, 274, 305, 336],
               ticktext=['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 
                         'Sep', 'Oct', 'Nov', 'Dec']))

# Update x and y axes properties:
fig.update_xaxes(showgrid=False, 
                 ticks="outside", 
                 tickson="boundaries", 
                 ticklen=5)
fig.update_yaxes(showgrid=False, 
                 ticks="outside", 
                 tickson="boundaries", 
                 ticklen=20)

# Add annotations for 2022 and 2023:
fig.add_annotation(dict(font=dict(color='white', size=15),
                        x=368, y=5.0,
                        showarrow=False,
                        text="2022",
                        textangle=0,
                        xanchor='left'))
fig.add_annotation(dict(font=dict(color='red', size=15),
                        x=220, y=15,
                        showarrow=False,
                        text="2023",
                        textangle=0,
                        xanchor='left'))

fig.show()
```

![](img/814e38240749b5a6bd7b8508fa258bb3.png)

使用自定义暗色主题绘制的填充线图（作者提供的图片）

注意灰色填充是如何被“拉下”以连接到最终的 2023 数据点。这展示了 2023 年对海冰记录的巨大影响。

## 绘制标准差与平均值

许多已发布的海冰图表包含了 1981–2010 年 20 年期间的平均值和 2 倍标准差。使用 pandas 的`agg()`方法，轻松可以回到这些年份的原始 DataFrame，并重新生成`bounds` DataFrame 以包括平均值和 2 倍标准差的列。结果如下：

![](img/d7c7c4f6eec83dbaff6fbb112671fd95.png)

显示 1981–2010 年平均值和 2 倍标准差的填充线图（作者提供的图片）

对于高斯分布，两个标准差涵盖了 95%以上的样本。这真正强调了过去两年的极端性质。

## 总结

在这篇文章中，我们重现了气候数据可视化中流行的堆叠线图技术。Plotly 库不仅使这变得简单，而且生成了可以保存为静态图像的交互式数字图表。

尽管意大利面图常因其复杂性而被贬低，但如果使用得当，它们可以讲述一个有力的故事。在这种情况下，我们使用了*强调*技术来突出显示几条线，同时削弱其他所有线条。

关于理解意大利面图的其他策略，请访问[数据讲述网站](https://www.storytellingwithdata.com/blog/2013/03/avoiding-spaghetti-graph)。要使用 matplotlib 库制作类似的图表，请查看[IceVarFigs/README.md at master · zmlabe/IceVarFigs (github.com)](https://github.com/zmlabe/IceVarFigs/blob/master/README.md)

## 引用

1.  Birkel, S.D.，“每日海表温度，”气候再分析器 (https://ClimateReanalyzer.org)，缅因大学气候变化研究所，美国。访问日期：2023 年 8 月 13 日。（气候再分析器内容在[创意共享署名 4.0 国际许可证](http://creativecommons.org/licenses/by/4.0/)下授权。）

1.  Labe, Zachary, 2023 年，“南极海冰，”气候可视化，（[`zacklabe.com/`](https://zacklabe.com/)），普林斯顿大学和 NOAA GFDL。访问日期：2023 年 8 月 13 日。 （内容受[创意共享署名 4.0 国际许可协议](http://creativecommons.org/licenses/by/4.0/)许可）。

1.  Readfern, Graham, 2023 年 7 月 28 日，“‘一些奇怪的事情正在发生’：在南极海冰保持历史低位时寻求答案，” ([`theguardian.com`](https://theguardian.com))。

1.  Fetterer, F., K. Knowles, W. N. Meier, M. Savoie, 和 A. K. Windnagel. 海冰指数，第 3 版。2017 年，由国家雪冰数据中心分发。[`doi.org/10.7265/N5K072F8`](https://nsidc.org/data/g02135/versions/3)。访问日期：2023 年 8 月 9 日。

## 谢谢！

感谢阅读，请关注我，以便未来获取更多*快速成功的数据科学*项目。
