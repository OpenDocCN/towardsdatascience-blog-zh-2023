# 使用 Pydeck 告别平面地图

> 原文：[`towardsdatascience.com/say-goodbye-to-flat-maps-with-pydeck-5ce440177bcd`](https://towardsdatascience.com/say-goodbye-to-flat-maps-with-pydeck-5ce440177bcd)

## 提升你的映射技能，掌握 3D 可视化

[](https://medium.com/@lee_vaughan?source=post_page-----5ce440177bcd--------------------------------)![Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----5ce440177bcd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ce440177bcd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ce440177bcd--------------------------------) [Lee Vaughan](https://medium.com/@lee_vaughan?source=post_page-----5ce440177bcd--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ce440177bcd--------------------------------) ·9 分钟阅读·2023 年 7 月 18 日

--

![](img/6a954c5dab844b92ab97e64cc81a7215.png)

图片来源：Google-Deepmind，Unsplash

*3D 挤出地图* 是一种数据可视化类型，其中 3D 条形或列基于其地理坐标在地图上定位。每个条形的高度表示与特定位置相关的*数值*，如人口或温度。这里是一个展示夏威夷群岛城市人口密度的例子：

![](img/7acc43ad9c0b782ab41528f83d97a031.png)

夏威夷的人口密度（人/平方公里）（所有其余图片均由作者提供）

这类地图以“倾斜”的视角呈现，以便条形的高度显而易见。通过将地图提供的地理信息与条形所代表的垂直维度相结合，3D 挤出地图能够在有趣的空间背景下传达信息和模式。*相对关系* 通常比*绝对值*更重要。

在这个*快速成功的数据科学*项目中，我们将使用 Python 和 pydeck 库来轻松创建美国和澳大利亚的人口分布 3D 挤出地图。完成这个简短的教程后，你将能够轻松创建你自己的地理空间数据集的惊人可视化。

# 人口数据集

在这个项目中，我们将绘制美国和澳大利亚的人口数据。对于美国，我们将使用免费的*基础* *美国城市数据库*，网址为 [*simplemaps.com*](https://simplemaps.com/data/us-cities) [1]。

该数据集包含截至 2023 年 1 月 31 日的 30,844 个城镇和城市的信息。它在*知识共享署名 4.0*许可证下提供，可进行再分发和商业使用。为了方便起见，我已经下载了数据并将其存储在一个[代码片段](https://gist.github.com/rlvaugh/a8c2fd019a33e347be2542320a942210)中。

对于澳大利亚，我们将使用 2020 年的 [Kaggle 数据集](https://www.kaggle.com/datasets/maryamalizadeh/worldcities-australia)，该数据集源于 *simplemaps.com* 的 [*World Cities Database*](https://simplemaps.com/data/world-cities)[2]。它包含了澳大利亚的大多数人口的 1,035 个主要城市。它以 [MIT 许可证](https://opensource.org/license/mit/) 和 *Creative Commons Attribution 4.0* 许可证免费发布。为方便起见，该数据集还存储在 [Gist](https://gist.github.com/rlvaugh/f5d08b0ef8fae288e4bbcfcf5af9bf89) 中。

# pydeck 库

[*pydeck*](https://pypi.org/project/pydeck/) 图形库是一组 Python 绑定，优化用于 Jupyter Notebook 环境，用于使用 [*deck.gl*](https://deck.gl/) 进行空间可视化。后者是一个 WebGL（GPU）驱动的框架，使用分层方法视觉上探索大型数据集。

pydeck 库使你可以在 Python 中访问完整的 deck.gl 图层目录。你可以创建美丽的 deck.gl 地图，而无需使用大量 JavaScript，并可以将这些地图嵌入 Jupyter notebook 或将其导出为独立的 HTML 文件。该库默认使用 [Carto](https://carto.com/)，但也可以与其他基础地图提供商（如 Mapbox）良好配合使用。

pydeck 主题地图旨在*交互式*使用。像 Plotly Express 地图一样，你可以平移和缩放地图。将光标悬停在柱状图上，也会弹出一个悬停数据窗口，显示诸如数据点名称、值、位置等详细信息。

要使用 conda 安装 pydeck，请在命令行中输入以下内容：

`conda install -c conda-forge pydeck`

要使用 pip 安装，请输入：

`pip install pydeck`

有关安装 pydeck 的更多信息，以及查看示例库，请访问 [Gallery — pydeck 0.6.1 documentation](https://pydeck.gl/)。

# 代码

以下代码是在 JupyterLab *单元格中* 输入的。

## 导入库

除了 pydeck，我们还将使用 *pandas* 数据分析库来加载和操作数据。你可以通过以下方式安装它：

`conda install pandas`

或

`pip install pandas`

这是导入内容：

```py
import pandas as pd
import pydeck as pdk
```

## 准备美国人口数据

以下代码将美国城市数据集读取到 pandas 数据框中，并仅保留城市名称、纬度、经度、估计人口和密度（以每平方公里人口为单位）的列。由于人口值的范围非常大，它还通过将人口值除以 100 来创建一个新列。这将使我们能够更容易地比较美国和澳大利亚之间的 3D 柱状图，这将在项目后续部分进行。

```py
# Specify the column names to keep:
columns_to_keep = ["city", "lat", "lng", "population", 'density']

# Load the CSV file into a DataFrame and keep only the specified columns:
df_us = pd.read_csv('https://bit.ly/3ObClvP', usecols=columns_to_keep)

# Scale the population column for easier comparison to Australia:
df_us['popl_div_100'] = (df_us['population'] / 100)  

display(df_us)
```

![](img/d400b11c6b630fec23d547b9debacc95.png)

显示美国城市数据框

## 绘制美国人口数据

以下代码分三步创建主题地图。第一步实例化一个 pydeck `Layer`对象。第二步设置`ViewState`参数，如地图的中心点位置、缩放级别、俯视角度和方向。最后一步实例化一个`Deck`对象并在 HTML 中渲染地图。

`Layer()`类中使用的第一个参数是`type`。在这里，我们使用`ColumnLayer`类型，它创建条形图（严格来说是圆柱形列）。要查看其他选项，如热图层和图标层，请访问 pydeck [gallery](https://deckgl.readthedocs.io/en/latest/)。

`Layer()`类的其他重要参数包括`get_elevation`，它是用于条形图高度的 DataFrame 列；`elevation_scale`，用于缩放条形图的高度；`pickable`，在光标悬停在条形图上时启用数据提示；以及`coverage`，用于设置条形图的宽度。这些参数，加上`get_fill_color`的参数，将帮助你最终调整地图的外观。

`ViewState()`类的参数非常简单。`bearing`控制视图的方向，`pitch`设置视图角度（`0` = 直接向下）。

```py
# Build the map layer:    
layer = pdk.Layer(type='ColumnLayer',
                  data=df_us,
                  get_position=['lng', 'lat'],
                  get_elevation='population',
                  auto_highlight=True,
                  elevation_scale=0.03,
                  pickable=True,
                  get_fill_color=['population', 255],
                  coverage=5)

# Set the view parameters:
view_state = pdk.ViewState(longitude=-95, 
                           latitude=36,
                           zoom=3.8,
                           min_zoom=3,
                           max_zoom=15,
                           pitch=45.0,
                           bearing=0)

# Render the map:
r = pdk.Deck(layers=[layer], initial_view_state=view_state)
r.to_html('usa_popl.html')
```

![](img/d9992d1e9d63c0856391b759a15990ba.png)

超过 30,000 个美国城市的人口地图

虽然我们只绘制了美国约三分之一的城市，但这张地图仍然令人印象深刻。最明显的特点之一是[100 度经线](https://medium.com/better-programming/tell-a-climate-story-with-plotly-express-bca33a723bc4)，这是一条虚拟的垂直线，将人口更密集的美国东半部与人口更稀少的西部内陆地区分开。

有一个稍微误导的方面是像纽约市和洛杉矶这样的地方的极高列。我们使用的数据库免费版本提供的是*城市*人口，而不是*市区*人口，这意味着它报告的是市区及其周围的郊区和工业区的人口，即*大都市区*。这有点重复，但从另一个角度看是有用的，因为你不需要识别和合计这个更大区域的组成部分。

在功能方面，你可以直观地使用鼠标或键盘操作这张地图。滚轮让你缩放。第一个鼠标按钮（MB1）让你平移。SHIFT-MB1 让你倾斜视图角度或旋转地图。最后，你可以将鼠标悬停在条形图上，以获取数据点的详细信息（你可能首先需要缩放）。

![](img/0451366dead5218cdb26fb1cf60c1441.png)

“pickable”弹出窗口，显示德克萨斯州 Cut and Shoot 城市

> 注意：在 pydeck 中创建颜色条或图例需要使用像 Matplotlib 这样的外部库，然后将其*放置在*你的 pydeck 可视化旁边，而不是*在*其中。你可以在[这里](https://matplotlib.org/stable/tutorials/colors/colorbar_only.html)了解有关独立 Matplotlib 颜色条的信息。

## 绘制美国人口密度数据

以下代码绘制了密度数据。我调整了一些参数以改善显示效果。

```py
# Build the map layer:
layer = pdk.Layer(type='ColumnLayer', 
                  data=df_us,
                  get_position=['lng', 'lat'],
                  get_elevation='density',
                  auto_highlight=True,
                  elevation_scale=20,
                  pickable=True,
                  get_fill_color=['density', 220],
                  coverage=2)

# Set the view parameters:
view_state = pdk.ViewState(longitude=-95,
                           latitude=36,
                           zoom=3.8,
                           min_zoom=3,
                           max_zoom=15,
                           pitch=45.0,
                           bearing=0)

# Render the map:
r = pdk.Deck(layers=[layer], initial_view_state=view_state)
r.to_html('usa_density.html')
```

![](img/5ce0b9bb068f0aafee4e262e9a076c64.png)

30,000+ 美国城市的**人口密度地图**

![](img/d636d74265abb98e3225e0bcc7eaa9a7.png)

地图放大显示了美国东北部的**人口密度**

在前面的图中，最高的柱状图代表的是纽约市的曼哈顿岛，每平方公里居住了高达**28,654**人。但这与马尼拉相比则显得微不足道，马尼拉的世界最高人口密度为每平方公里**46,178**人。

## 准备澳大利亚人口数据

以下代码将澳大利亚城市数据集读取到 pandas DataFrame 中，并仅保留城市名称、经度和纬度及其估计人口的列。由于人口值范围非常广，因此还会通过将人口值除以 100 来创建一个新列。这将使得稍后比较美国和澳大利亚的 3D 柱状图更容易。

```py
## Specify the column names to keep:
columns_to_keep = ["city", "lat", "lng", "population"]

# Load the Australia CSV file into a DataFrame:
df_au = pd.read_csv('https://bit.ly/3PXwziA', usecols=columns_to_keep)
df_au['popl_div_100'] = (df_au['population'] / 100)
display(df_au)
```

![](img/44e95c5b587a39776387689753d05fbd.png)

显示澳大利亚城市的 DataFrame

## 绘制澳大利亚人口数据

要绘制澳大利亚数据，我们只需重复绘图代码，并根据数据集调整参数。一个重要的参数是更改视图状态的经度和纬度！

```py
# Build the map layer:      
layer = pdk.Layer(type='ColumnLayer',
                  data=df_au,
                  get_position=['lng', 'lat'],
                  get_elevation='population',
                  auto_highlight=True,
                  elevation_scale=0.2,
                  pickable=True,
                  get_fill_color=['popl_div_100', 220],
                  coverage=6)

# Set the view parameters:
view_state = pdk.ViewState(longitude=138,
                           latitude=-33,
                           zoom=3.6,
                           min_zoom=3,
                           max_zoom=15,
                           pitch=55.0,
                           bearing=310)

# Render the map:
r = pdk.Deck(layers=[layer], initial_view_state=view_state)
r.to_html('au.html')
```

![](img/4db86ae0f2be4bb2d9a9524e11fac053.png)

1,000+ 澳大利亚城市的**人口地图**

澳大利亚被描述为沿海城市国家的集合，你可以明白为什么。大约 86%的人口居住在城市地区，其中 72%居住在主要城市，如墨尔本、悉尼和珀斯。这个现象是有原因的，内陆荒凉，他们称其为“红色中心”是有原因的！

## 更改地图样式

默认情况下，pydeck 绘图使用深色背景（具体来说，是 Carto 的“Dark Matter”地图）。这可以通过`map_style`参数在`Deck()`类中设置。要将背景改为白色，请传入`pdk.map_styles.LIGHT`。其他选项包括卫星图、道路图，或无标签的深色和浅色版本。

这是一个示例，展示了使用浅色背景绘制的美国数据集，海拔设置为`popl_div_100`列，柱状图填充颜色设置为黑色（使用 RGB 颜色代码`[0, 0, 0]`）：

```py
# Build the map layer:      
layer = pdk.Layer(type='ColumnLayer',
                  data=df_us,
                  get_position=['lng', 'lat'],
                  get_elevation='popl_div_100',
                  auto_highlight=True,
                  elevation_scale=30,
                  pickable=True,
                  get_fill_color=[0, 0, 0],
                  coverage=3)

# Set the view:
view_state = pdk.ViewState(longitude=-95,
                           latitude=36,
                           zoom=3,
                           min_zoom=3,
                           max_zoom=15,
                           pitch=0,
                           bearing=0)

# Render the map:
r = pdk.Deck(layers=[layer], initial_view_state=view_state,
             map_style=pdk.map_styles.LIGHT)
r.to_html('us_popl_light.html')
```

![](img/03bf9e007d51a1f97acc63945f4a7694.png)

带有浅色背景和黑色条形图的美国城市人口地图

## 比较澳大利亚和美国的人口

如果你使用`df_au` DataFrame 重复前面的代码，`longitude` 为 138 和 `latitude` 为 -26，你将生成一张可以与之前的美国地图进行比较的澳大利亚地图：

![](img/2f3219bc39728b3100b7840655825d1c.png)

在相同尺度下对比美国和澳大利亚城市的人口

尽管与大陆美国面积相近，澳大利亚的人口却少得多。它的两个最大城市每个都拥有 500 万到 600 万人口，与美国城市如休斯顿、迈阿密和亚特兰大的人口相当。

# 总结

*主题*地图，如 3D 拉伸图，帮助你突出与物理空间相关的特定主题。所有相关的地理空间数据都会被提取并投影到地图上，使你的观众能够快速理解主题与位置之间的联系。

pydeck 库使得使用 Python 创建有趣的 3D 主题可视化变得简单。它针对 Jupyter Notebook、流行的 pandas 库以及大型数据集进行了优化。

除了 pydeck，Python 还有一个大型的地理空间库生态系统。要查看最重要库的总结——包括如何选择最适合你需求的库——请参考我的最新书籍，[*Python 工具箱：Anaconda、JupyterLab 和 Python 科学库使用指南*](https://a.co/d/1aFyztS)。

# 引用

1.  美国城市数据库（2023），[*https://simplemaps.com/data/us-cities*](https://simplemaps.com/data/us-cities)。

1.  [澳大利亚城市数据库 | Kaggle](https://www.kaggle.com/datasets/maryamalizadeh/worldcities-australia)（2020），来自[*https://simplemaps.com/data/world-cities*](https://simplemaps.com/data/world-cities)

# 谢谢！

感谢阅读，请关注我以获取更多未来的*快速成功数据科学*项目。
