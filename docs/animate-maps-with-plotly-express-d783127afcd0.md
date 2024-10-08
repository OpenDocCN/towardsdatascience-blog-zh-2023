# 使用 Plotly Express 动画地图

> 原文：[`towardsdatascience.com/animate-maps-with-plotly-express-d783127afcd0`](https://towardsdatascience.com/animate-maps-with-plotly-express-d783127afcd0)

## 振兴你的信息图！

[](https://medium.com/@lee_vaughan?source=post_page-----d783127afcd0--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----d783127afcd0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d783127afcd0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d783127afcd0--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----d783127afcd0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d783127afcd0--------------------------------) ·9 分钟阅读·2023 年 7 月 11 日

--

![](img/c173db5b859806e3f3e30abb0f52171a.png)

按年上色的选定州（所有图片均为作者提供，除非另有说明）

动态地图是引起注意和传达信息的绝佳工具。无论你是准备演示文稿的商人、准备信息图的记者，还是准备课堂教学的老师，动画都会提高观众的参与度、注意力和记忆力。即使你不打算展示实时动画，这个功能对于在不同时间框架中准备静态展示仍然很有用。

在这个 *快速成功的数据科学* 项目中，我们将使用 Python、pandas 和 Plotly Express 来可视化美国的演变。具体来说，我们将使用 [choropleth maps](https://en.wikipedia.org/wiki/Choropleth_map) 来按天、按年以及在更大的时间范围内动画展示美国各州加入联邦的过程。

# 代码

以下代码是在 Jupyter Lab 中编写的，并以 *单元格* 形式呈现。

## 安装和导入库

Plotly Express 是 Plotly 图形库的高级版本，需要 Plotly 作为依赖。你可以通过 conda 或 pip 安装它。

这是 conda 安装命令：

`conda install -c plotly plotly_express`

这是 pip 版本：

`pip install plotly`

要安装 pandas 数据分析包，可以使用以下命令之一：

`conda install pandas`

或者：

`pip install pandas`

我们还需要 NumPy（数值 Python）库。你不需要直接安装它，因为它已包含在 pandas 中。

这是导入部分。我们使用了别名以方便输入：

```py
import numpy as np
import pandas as pd
import plotly.express as px
```

## 加载和准备数据

为方便起见，我创建了所需数据的 CSV 文件，并将其存储在这个[Gist](https://gist.github.com/rlvaugh/13b17ae878cb1ef10146d6ebf42d4858)中。该文件包括州名、官方缩写、加入联邦的日期以及加入顺序。文件按日期升序排序。我们将其加载为 pandas DataFrame，因为 Plotly Express 与 pandas 兼容性很好。

除了*天*，我们还希望使用*年份*对数据进行动画处理。这将使我们更容易识别在同一年加入联邦的州。为此，我们将创建一个新的 DataFrame 列，命名为`Year`，并将其转换为整数类型。

```py
# Load the statehood data and prepare a "Year" column:
df = pd.read_csv('https://bit.ly/44qtTyk')
df['Year'] = df['date_entered'].str[-4:].astype('int')
df.head(3)
```

![](img/9f1bfc3c0aff78df4959c2fbbe9ebe0b.png)

DataFrame 的头部（所有图片由作者提供，除非另有说明）

## 按加入日期着色的静态 Choropleth 地图

在 Plotly Express 中创建 choropleth 地图非常简单。只需调用内置的`[choropleth()](https://plotly.com/python/choropleth-maps/)`方法。

第一个参数是我们想要动画的 DataFrame。之后，我们只需引用 DataFrame 中的*列*。

Plotly Express 还附带一些内置的地理空间数据集。要选择适用于美国各州的数据集，我们将使用`scope`、`locationmode`和`locations`参数。`abbr`参数与`locations`参数一起使用，表示州的官方[缩写](https://en.wikipedia.org/wiki/List_of_U.S._state_and_territory_abbreviations)，例如纽约的 NY 和德州的 TX。

颜色方面，我选择了`Portland`颜色尺度。如果你想尝试其他选项，请访问 Plotly 的[文档](https://plotly.com/python/builtin-colorscales/)查看可用的选项。

> 专业提示：要反转 Plotly 颜色条，请在其名称末尾附加“_r”。

```py
# Plot the DataFrame as a static map:
fig = px.choropleth(df, 
                    locations='abbr', 
                    locationmode='USA-states', 
                    color='Year', 
                    color_continuous_scale=px.colors.diverging.Portland, 
                    scope='usa',
                    hover_data=['state', 'order', 'Year'],
                    title='States by Date of Entry into Union')
fig.update_layout(width=750, height=500)
```

![](img/17334114ca1b87fae242baa06f37b5d2.png)

具有活动悬停窗口的科罗拉多州静态地图。

生成的地图目前是静态的（非动画），但它具有许多交互功能。你可以将光标悬停在一个州上，查看其元数据的弹出窗口。右上角的工具栏允许执行截屏、缩放、平移、框选和重置为“主页”屏幕等操作。

尽管具有所有这些功能，但地图看起来有些乏味。让我们通过添加动画来为它增添一些活力。

## 按加入年份进行动画

你可以通过一个额外的参数`animation_frame`为之前的地图添加动画。在这里，我们使用`data_entered`列进行动画，该列捕捉了州加入联邦的日期。

为确保颜色条包含数据的最小值和最大值，我们需要为`range_color`参数包含一个参数。否则，比例将默认显示当前活跃状态的值。这次，我选择了`Earth`颜色条，因为它具有古老的“历史”外观。

```py
# Plot the Dataframe as an animation showing each state in order of admission:
fig = px.choropleth(df, 
                    locations='abbr', 
                    locationmode='USA-states', 
                    color='Year', 
                    color_continuous_scale=px.colors.diverging.Earth, 
                    scope='usa',
                    hover_data=['state', 'order'],
                    animation_frame='date_entered',
                    range_color= (min(df.Year), max(df.Year)),
                    title='The Date Each State Entered the Union')

fig.update_layout(width=750, height=500)
```

![](img/07665ae503ba9f5ee723bb89d0a71530.png)

根据州成立日期进行动画的地图

要运行动画，可以按播放按钮或使用滑块。一次只会显示一个州。有趣的是，州的进入并不是按东到西的逻辑顺序，而是经常在南北和东西之间“跳跃”。

## 按进入年份动画

要查看在给定年份进入联盟的所有州，只需将 `animation_frame` 参数更改为 `Year` 列。

```py
# Animate the states grouped by year of admission:
fig = px.choropleth(df, 
                    locations='abbr', 
                    locationmode='USA-states', 
                    color='Year', 
                    color_continuous_scale=px.colors.diverging.Earth, 
                    scope='usa',
                    hover_data=['state', 'order'],
                    animation_frame='Year',
                    range_color= (min(df.Year), max(df.Year)))
fig.update_layout(width=750, height=500)
```

![](img/a525ea8a0c3e2597fb78113a56d303aa.png)

按年份动画

## 动画化的大时间跨度的州

尽管按州的入驻日期或年份进行动画化很有趣，但很难获得整体视图。在以下示例中，我们将州按四个重要时间跨度进行汇总：

1.  十三个原始殖民地进入联盟时

1.  扩展至内战前的后续扩展

1.  开拓荒野

1.  20 世纪

为了在这些时间跨度上对州进行分组，我们将使用 NumPy 的 `select()` 方法在数据框中创建新列。这需要三个步骤：

1.  使用日期范围创建新列的条件列表，

1.  为每个条件创建新列的值列表，

1.  通过调用 `np.select()` 方法并传递前两个步骤中的列表来创建新列。

我们将新列命名为 `Timespan`。要用于动画，请将 `animation_frame` 参数更改为 `Timespan`。

```py
# Animate the states grouped by large timeframes:
conditions = [(df['Year'] >= 1787) & (df['Year'] <= 1790),
              (df['Year'] >= 1791) & (df['Year'] <= 1860),
              (df['Year'] >= 1861) & (df['Year'] <= 1900),
              (df['Year'] >= 1901) & (df['Year'] <= 1959)]

values = ['Original 13 Colonies', 'Antebellum Expansion', 
          'Westward Expansion', 'Twentieth Century']

df['Timespan'] = np.select(conditions, values)

# Animate the states grouped by year of admission:
fig = px.choropleth(df, 
                    locations='abbr', 
                    locationmode="USA-states", 
                    color='Year', 
                    color_continuous_scale=px.colors.diverging.Earth, 
                    scope="usa",
                    hover_data=['state', 'order'],
                    animation_frame='Timespan',
                    range_color= (min(df.Year), max(df.Year)))
fig.update_layout(width=750, height=500)

# Set the animation speed (in millseconds):
fig.layout.updatemenus[0].buttons[0].args[1]["frame"]["duration"] = 2000
fig.show()
```

![](img/a205676ff0eb65fc7bf310d9d60ce145.png)

按时间跨度动画

请注意，我们在最后一行更改了动画的*速度*（以毫秒为单位）。如果你觉得之前的动画太快，可以使用这段代码。

## 制作静态信息图

如果你想将结果作为*静态图像*保存，例如，用于制作手册，该怎么办？

对于像这样的任务，我喜欢通过复制和编辑主数据框，然后循环遍历这些副本来绘制地图，这样可以更好地控制图形标题和颜色条范围。

为了确保颜色条包括每个时间段的*终端成员*，我们将提供四个列表，指定我们希望看到标注的日期。

```py
# Make static maps of the states grouped over large timeframes:

# Create new DataFrames for designated time spans
original_13 = df[(df['Year'] >= 1787) & (df['Year'] <= 1790)].copy()
antebellum_expansion = df[(df['Year'] >= 1791) & (df['Year'] <= 1860)].copy()
western_expansion = df[(df['Year'] >= 1861) & (df['Year'] <= 1900)].copy()
twentieth_century = df[(df['Year'] >= 1901) & (df['Year'] <= 1959)].copy()

# Make a list of DataFrames to loop through
time_frames = [original_13, antebellum_expansion, 
               western_expansion, twentieth_century]

# Make a list of the dates you want to see in the color bar
colorbar_dates = [[1787, 1788, 1789, 1790],
                  [1791, 1800, 1810, 1820, 1830, 1840, 1850, 1859],
                  [1861, 1870, 1880, 1890, 1896],
                  [1907, 1920, 1930, 1940, 1950, 1959]]

# Make a list of the figure titles for each DataFrame
figure_titles = ["Original 13 Colonies (1787-1790)", 
                 "Antebellum Expansion (1791-1860)", 
                 "Western Expansion (1861-1900)", 
                 "Twentieth Century (1901-1959)"]

# Loop through and plot the DataFrames
for i, data_frame in enumerate(time_frames):
    fig = px.choropleth(data_frame, 
                        locations='abbr', 
                        locationmode="USA-states", 
                        color='Year', 
                        color_continuous_scale=px.colors.diverging.Earth, 
                        scope="usa",
                        hover_data=['state', 'order'],
                        range_color=(min(data_frame['Year']), 
                                     max(data_frame['Year'])),
                        title=figure_titles[i])
    fig.update_layout(width=750, height=500)
    fig.update_layout(coloraxis_colorbar=dict(tickvals=colorbar_dates[i],
                                               ticks='outside',
                                               thickness=15))
    fig.show()
```

![](img/41b1cc196bc4075753bf439e2048e8cf.png)

原始 13 个殖民地的静态地图，根据进入年份进行着色

![](img/15b6c3e65274cc5c7b2c6035f4d2389c.png)

进入联盟前的扩展时期州的静态地图，根据进入年份进行着色

![](img/4a9b10a6eb77d2feae03f2c869360f54.png)

向西扩展时期州的静态地图，根据进入年份进行着色

![](img/fd90c4772b4cd1571a49dcea576b1733.png)

20 世纪各时期的静态地图，根据进入年份进行着色

## 带有持久性的静态地图

我们仍然没有得到（真正的）整体视图。如果能够看到在某一特定日期联盟中的*所有*州，那就太好了。

以下单元格中的代码“逐渐构建”地图。*之前*时间段添加的州仍然在地图上*可见*，这样最后的地图会显示*所有*的联盟州。

为了实现这一目标，我们需要重新过滤主数据框，以确保保留的最小值*始终*为 1787 年。我们还需要一个更适合这种新可视化的`dates`列表。

```py
 # Make static maps of the states grouped over large timeframes, with persistence:

# Create new DataFrames for designated time spans
original_13 = df[(df['Year'] >= 1787) & (df['Year'] <= 1790)].copy()
antebellum_expansion = df[(df['Year'] >= 1787) & (df['Year'] <= 1860)].copy()
western_expansion = df[(df['Year'] >= 1787) & (df['Year'] <= 1900)].copy()
twentieth_century = df[(df['Year'] >= 1787) & (df['Year'] <= 1959)].copy()

# Make a list of DataFrames to loop through
time_frames = [original_13, antebellum_expansion, 
               western_expansion, twentieth_century]

# Make a list of the dates you want to see in the color bar:
colorbar_dates = [1787, 1790, 1800, 1820, 1840, 1859, 
                  1880, 1896, 1910, 1920, 1940, 1959]

# Make a list of the figure titles for each DataFrame
figure_titles = ["States through 1790", 
                 "States through 1860", 
                 "States through 1900", 
                 "States through 1959"]

# Loop through and plot the DataFrames
for i, data_frame in enumerate(time_frames):
    fig = px.choropleth(data_frame, 
                        locations='abbr', 
                        locationmode="USA-states", 
                        color='Year', 
                        color_continuous_scale=px.colors.diverging.Earth, 
                        scope="usa",
                        hover_data=['state', 'order'],
                        range_color=(min(data_frame['Year']), 
                                     max(data_frame['Year'])),
                        title=figure_titles[i])
    fig.update_layout(width=750, height=500)
    fig.update_layout(coloraxis_colorbar=dict(tickvals=colorbar_dates,
                                               ticks='outside',
                                               thickness=15))
    fig.show()
```

![](img/e66ba484946edbf6490ecf74ee57b84d.png)

显示 1787-1860 年间加入联邦的州的持久地图示例

为了简洁起见，我仅展示了前一个单元生成的四张地图中的一张。

# 摘要

只需一个参数，Plotly Express 就可以将美丽的*静态*地图转换为美丽的*动态*地图。除了富有吸引力外，这些动画地图还使得发现数据集中的异常值变得容易。例如，尽管我是一名美国人且对历史相当了解，但我还是很惊讶地发现：

+   特拉华州是第一个批准宪法的州，因此也是联邦中的第一个州（谁知道呢？）。

+   缅因州，尽管是新英格兰的一部分，却是相对较晚加入的州。

+   俄克拉荷马州直到 20 世纪才成为一个州。

+   亚利桑那州和新墨西哥州直到 1912 年才成为州，尽管它们是 1848 年墨西哥割让的一部分。

其他值得探索的数据集包括按美国内战期间脱离联邦顺序排列的州，按人口排列的州，或按选举团票数排列的州。

如果你想使用 Plotly Express 为其他国家构建动画染色地图，这里有一个为[加拿大省份](https://mahshadn.medium.com/animated-choropleth-map-with-discrete-colors-using-python-and-plotly-styling-5e208e5b6bf8)构建的示例。省份多边形使用[Geojson](https://en.wikipedia.org/wiki/GeoJSON)文件构建。

# 谢谢！

感谢阅读，请关注我以获取未来更多的*快速成功数据科学*项目。
