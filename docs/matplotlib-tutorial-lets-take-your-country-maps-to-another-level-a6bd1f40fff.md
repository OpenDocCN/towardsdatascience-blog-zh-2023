# Matplotlib 教程：将你的国家地图提升到另一个水平

> 原文：[`towardsdatascience.com/matplotlib-tutorial-lets-take-your-country-maps-to-another-level-a6bd1f40fff`](https://towardsdatascience.com/matplotlib-tutorial-lets-take-your-country-maps-to-another-level-a6bd1f40fff)

## Matplotlib 教程

## 如何使用 Python 和 Matplotlib 绘制美丽的地图

[](https://medium.com/@oscarleo?source=post_page-----a6bd1f40fff--------------------------------)![Oscar Leo](https://medium.com/@oscarleo?source=post_page-----a6bd1f40fff--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a6bd1f40fff--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6bd1f40fff--------------------------------) [Oscar Leo](https://medium.com/@oscarleo?source=post_page-----a6bd1f40fff--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6bd1f40fff--------------------------------) ·阅读时间 10 分钟·2023 年 9 月 24 日

--

![](img/d6439d0845f481950b5a256c886b44bb.png)

地图由作者创建

是的，我使用 Matplotlib 创建了上面的地图，我会在本教程中向你展示如何做。

这个想法是创建一个可重用且灵活的函数，使我能够立即绘制任何区域的美丽地图。

有了这样的功能，我可以节省大量时间，当我想创建带有地理组件的图表或信息图时。

我还想展示可重用代码的力量，因为许多从事数据可视化的程序员往往忽视了这样的最佳实践。

本教程包含了创建上述非洲地图所需的每一行代码。

让我们开始吧。

## 第一步：下载地理数据

在开始本教程之前，你需要做的唯一一件事是从这里下载地理数据：

[`datacatalog.worldbank.org/search/dataset/0038272/World-Bank-Official-Boundaries`](https://datacatalog.worldbank.org/search/dataset/0038272/World-Bank-Official-Boundaries)

我使用的是名为 World Boundaries GeoJSON — Very High Resolution 的数据。

这是来自世界银行的官方边界数据集，你可以按任何你想要的方式使用。

## 第二步：导入库

一如既往，我们从导入必要的库开始，我们不需要很多库。由于我们有地理数据，我们希望使用`geopandas`使绘图尽可能简单。

```py
import numpy as np
import pandas as pd
import seaborn as sns
import geopandas as gpd
import matplotlib.pyplot as plt

import matplotlib.patheffects as PathEffects
from matplotlib.patches import Polygon
```

你可能还没见过的一个导入是`PathEffe`。我将使用它来稍后在国家标签周围创建边框。

## 第三步：创建 seaborn 风格

在绘图之前，我总是创建一个 seaborn 风格，以获得一致的外观。在这里，我只定义了一个`background_color`、`font_family`和`text_color`。我将背景设置为浅蓝色，以代表海洋。

```py
font_family = "sans"
background_color = "#D4F1F4"
text_color = "#040303"

sns.set_style({
    "axes.facecolor": background_color,
    "figure.facecolor": background_color,
    "font.family": font_family,
    "text.color": text_color,
})
```

你可以样式化其他图表方面，例如网格，但我更喜欢在绘制地图时使用`plt.axis(“off”)`隐藏大多数标准图表组件。

## 第 4 步：加载地理数据

现在是时候使用 geopandas 从世界银行加载地理数据。我正在更改塞舌尔的`CONTINENT`和赤道几内亚的`INCOME_GRP`。

这看起来并不像它看起来那么奇怪，因为塞舌尔是非洲的一部分，根据世界银行最新的数据，赤道几内亚属于“中上收入”组别。

```py
world = gpd.read_file("WB_Boundaries_GeoJSON_highres/WB_countries_Admin0.geojson")
world.loc[world.NAME_EN == "Seychelles", "CONTINENT"] = "Africa"
world.loc[world.NAME_EN == "Equatorial Guinea", "INCOME_GRP"] = "3\. Upper middle income"

africa = world[world.CONTINENT == "Africa"].reset_index(drop=True)

disputed_areas = gpd.read_file("WB_Boundaries_GeoJSON_highres/WB_Admin0_disputed_areas.geojson")
disputed_areas = disputed_areas[disputed_areas.CONTINENT == "Africa"]
```

接下来，我将非洲国家分开，因为我想将它们单独绘制，以作为我的图表的重点，对争议区域也进行同样的处理。

## 第 5 步：创建绘制地图函数

现在，我准备创建绘制地图的第一个版本函数。稍后我将扩展它以添加一些额外的函数。

这个第一个版本循环遍历一系列 geopandas 数据帧，并使用`color`和`edgecolor`列绘制它们。

该函数接受一些我还未使用的参数，但它们在教程的后续部分将会很有用。

```py
def draw_map(
    maps_to_draw, 
    boundry_map_index=0,
    use_hatch_for_indexes=[],
    padding={},
    labels=[],
    figsize=(40, 40)
):

    assert "color" in maps_to_draw[0].columns, "Missing color column in map dataframe"
    assert "edgecolor" in maps_to_draw[0].columns, "Missing edgecolor column in map dataframe"

    fig = plt.figure(figsize=figsize)
    ax = fig.add_subplot()

    for map_index, map_to_draw in enumerate(maps_to_draw):
        map_to_draw.plot(
            ax=ax, color=map_to_draw.color, edgecolor=map_to_draw.edgecolor,
            hatch="//" if map_index in use_hatch_for_indexes else "",
        )

    # Additional functions below this comment

    return ax
```

我希望`draw_map()`返回`ax`对象，因为我经常希望根据使用案例扩展图表以添加更多信息。

要使用`draw_map()`，我必须为每个要绘制的 geopandas 数据帧定义`color`和`edgecolor`列。

我为`world`定义了颜色，以将每个国家绘制为淡背景。对于`africa`，我选择了使用数据中表示的三种收入组别的更显眼的颜色。

选择颜色时的一个提示是使用[Coloring for Colorblindness](https://davidmathlogic.com/colorblind/#%23D81B60-%231E88E5-%23FFC107-%23004D40)检查你的颜色是否适合色盲人士。

```py
world["color"] = "#f0f0f0"
world["edgecolor"] = "#c0c0c0"

africa["edgecolor"] = "#000000"
africa.loc[africa.INCOME_GRP == "5\. Low income", "color"] = "#dadada"
africa.loc[africa.INCOME_GRP == "4\. Lower middle income", "color"] = "#89bab2"
africa.loc[africa.INCOME_GRP == "3\. Upper middle income", "color"] = "#1B998B"

disputed_areas["color"] = "#FFD6D6"
disputed_areas["edgecolor"] = "#000000"
```

现在我已经创建了所需的列，我可以运行`draw_map()`。

```py
ax = draw_map(maps_to_draw=[world, africa, disputed_areas])

plt.axis("off")
plt.show()
```

这是我得到的结果。

![](img/b63b39ca16c073d416726465ae4e5f04.png)

地图由作者创建

这是一个很好的开始，但我们还有很长的路要走。

## 第 6 步：设置边界

由于我想创建一张非洲地图，所以显示整个世界是没有意义的。

这就是为什么我创建了以下函数，它接收一个地理数据帧，并设置`ax`对象的范围。

```py
def set_limits(ax, data, pad_left=0, pad_right=0, pad_top=0, pad_bottom=0):
    xmin_ = data.bounds.minx.min()
    ymin_ = data.bounds.miny.min()
    xmax_ = data.bounds.maxx.max()
    ymax_ = data.bounds.maxy.max()

    xmin = xmin_ - pad_left * (xmax_ - xmin_)
    xmax = xmax_ + pad_right * (xmax_ - xmin_)
    ymin = ymin_ - pad_bottom * (ymax_ - ymin_)
    ymax = ymax_ + pad_top * (ymax_ - ymin_)

    ax.set(xlim=(xmin, xmax), ylim=(ymin, ymax))
```

你可以在区域的每一侧添加填充，以便为额外的信息（如图例）留出空间。

让我们使用`boundary_map_index`和`padding`参数将其添加到`draw_map()`函数中。

```py
def draw_map(
    maps_to_draw, 
    boundry_map_index=0,
    use_hatch_for_indexes=[],
    padding={},
    labels=[],
    figsize=(40, 40)
):

    ...
    # Additional functions below this comment
    set_limits(ax, maps_to_draw[boundry_map_index], **padding)

    return ax
```

我将`boundry_map_index`设置为`maps_to_draw`列表中我们希望显示的 geopandas 数据帧的索引，并添加了一些`padding`。

注意：我还传递了`use_hatch_for_indexes=[2]`以在争议区域绘制斜纹，以显示它们与其他区域的不同。

```py
ax = draw_map(
    maps_to_draw=[world, africa, disputed_areas], boundry_map_index=1,
    padding={"pad_bottom": -0.08, "pad_top": 0.07, "pad_left": 0.07, "pad_right": 0.05},
    use_hatch_for_indexes=[2]
)

plt.axis("off")
plt.show()
```

现在我们得到了一个看起来不错的非洲地图，还包括了邻近国家的轮廓。如果你不想显示邻近国家，请从`maps_to_draw`中删除`world`。

这是生成的地图。

![](img/f71377f824c3aff345b260e782aa91e6.png)

地图由作者创建

我们正在取得进展。

## 第 7 步：添加国家标签

由于大多数人不认识所有非洲国家，下一步是为每个国家添加标签。

我想在大国家的中间附近添加标签，对于较小的国家，则在国家和标签之间添加一条线。

```py
def add_label(ax, label, fontsize=24, fontweight="bold", va="center", ha="center"):            
    annotation = plt.annotate(
        label["label"], 
        xy=label["xytext"] if "xypin" not in label.keys() else label["xypin"], 
        xytext=None if "xypin" not in label.keys() else label["xytext"], 
        xycoords="data", fontsize=fontsize, va=va, ha=ha,
        linespacing=1.3, color=label["color"], fontweight=fontweight, 
        arrowprops={
            "arrowstyle": "-",
            "linewidth": 2,
        })

    annotation.set_path_effects([PathEffects.withStroke(linewidth=6, foreground='w')])
```

定义国家标签的位置是这个教程中唯一繁琐的过程，但我已经为你完成了。

你可以尝试使用 geopandas 中的几何图形计算每个标签的位置，但这说起来容易做起来难，因为国家的形状各异。

有时候，最快的方法是卷起袖子直接硬编码值，这也是我在这里做的。

```py
country_labels = [
    {"label": "Algeria", "color": "#040303", "xytext": (2.0, 27.5)},
    {"label": "Angola", "color": "#040303", "xytext": (17.7, -13.1)},
    {"label": "Benin", "color": "#040303", "xytext": (3.2, 5.4), "xypin": (2.3, 7.6)},
    {"label": "Botswana", "color": "#040303", "xytext": (24.4, -22.3)},
    {"label": "Burkina\nFaso", "color": "#040303", "xytext": (-1.4, 12.6)},
    {"label": "Burundi", "color": "#040303", "xytext": (43.3, -4.9), "xypin": (29.8, -3.6)},
    {"label": "Cameroon", "color": "#040303", "xytext": (12.5, 5.2)},
    {"label": "Cape Verde", "color": "#040303", "xytext": (-23.7, 19), "xypin": (-23.7, 16)},
    {"label": "Central African\nRepublic", "color": "#040303", "xytext": (21.1, 6.5)},
    {"label": "Chad", "color": "#040303", "xytext": (18.5, 16.0)},
    {"label": "Comoros", "color": "#040303", "xytext": (46.8, -9.6), "xypin": (43.3, -11.7)},
    {"label": "Cote\nd'Ivoire", "color": "#040303", "xytext": (-5.5, 8.5)},
    {"label": "Democratic\nRepublic of\nthe Congo", "color": "#040303", "xytext": (23.3, -2.7)},
    {"label": "Djibouti", "color": "#040303", "xytext": (47.0, 13.4), "xypin": (43.0, 12.2)},
    {"label": "Egypt", "color": "#040303", "xytext": (29.2, 26.6)},
    {"label": "Equatorial\nGuinea", "color": "#040303", "xytext": (5.9, -2.5), "xypin": (10.5, 1.6)},
    {"label": "Eritrea", "color": "#040303", "xytext": (43.0, 16.9), "xypin": (38.5, 16.2)},
    {"label": "Lesotho", "color": "#040303", "xytext": (35.0, -31.0), "xypin": (28.4, -29.5)},
    {"label": "Ethiopia", "color": "#040303", "xytext": (39.9, 8.5)},
    {"label": "Gabon", "color": "#040303", "xytext": (11.8, -0.7)},
    {"label": "Ghana", "color": "#040303", "xytext": (-1.3, 6.6)},
    {"label": "Guinea", "color": "#040303", "xytext": (-11.6, 11.0)},
    {"label": "Guinea-\nBissau", "color": "#040303", "xytext": (-20.3, 10.3), "xypin": (-14.5, 12.2)},
    {"label": "Kenya", "color": "#040303", "xytext": (37.9, 0.5)},
    {"label": "Eswantini", "color": "#040303", "xytext": (35.5, -29.3), "xypin": (31.5, -26.8)},
    {"label": "Liberia", "color": "#040303", "xytext": (-10.6, 3.6), "xypin": (-9.6, 6.7)},
    {"label": "Libya", "color": "#040303", "xytext": (17.5, 27.5)},
    {"label": "Madagascar", "color": "#040303", "xytext": (46.7, -19.6)},
    {"label": "Malawi", "color": "#040303", "xytext": (38.9, -21.3), "xypin": (35.0, -15.6)},
    {"label": "Mali", "color": "#040303", "xytext": (-1.9, 17.8)},
    {"label": "Mauritania", "color": "#040303", "xytext": (-11.1, 19.6)},
    {"label": "Morocco", "color": "#040303", "xytext": (-6.9, 31.3)},
    {"label": "Mozambique", "color": "#040303", "xytext": (40.8, -15.2)},
    {"label": "Namibia", "color": "#040303", "xytext": (17.3, -20.7)},
    {"label": "Niger", "color": "#040303", "xytext": (9.8, 17.5)},
    {"label": "Nigera", "color": "#040303", "xytext": (7.8, 9.8)},
    {"label": "Republic of\nthe Congo", "color": "#040303", "xytext": (7.8, -7.2), "xypin": (12.0, -4.1)},
    {"label": "Rwanda", "color": "#040303", "xytext": (43.8, -3.6), "xypin": (30.1, -2.0)},
    {"label": "São Tomé and\nPríncipe", "color": "#040303", "xytext": (-0.9, 0.2), "xypin": (6.8, 0.2)},
    {"label": "Senegal", "color": "#040303", "xytext": (-15.0, 14.7)},
    {"label": "Seychelles", "color": "#040303", "xytext": (55.6, -2), "xypin": (55.6, -4.5)},
    {"label": "Sierra Leone", "color": "#040303", "xytext": (-16.4, 6.3), "xypin": (-12.0, 8.5)},
    {"label": "Somalia", "color": "#040303", "xytext": (45.7, 2.7)},
    {"label": "South\nAfrica", "color": "#040303", "xytext": (22.4, -31.0)},
    {"label": "South\nSudan", "color": "#040303", "xytext": (30.2, 7.0)},
    {"label": "Sudan", "color": "#040303", "xytext": (29.7, 16.0)},
    {"label": "Tanzania", "color": "#040303", "xytext": (35.0, -6.7)},
    {"label": "The\nGambia", "color": "#040303", "xytext": (-20.3, 13.6), "xypin": (-15.4, 13.6)},
    {"label": "Togo", "color": "#040303", "xytext": (1.0, 4.1), "xypin": (1.0, 7.5)},
    {"label": "Tunisia", "color": "#040303", "xytext": (9.3, 38.9), "xypin": (9.3, 35.7)},
    {"label": "Uganda", "color": "#040303", "xytext": (32.6, 0.9)},
    {"label": "Zambia", "color": "#040303", "xytext": (26.1, -14.9)},
    {"label": "Zimbawe", "color": "#040303", "xytext": (29.7, -19.1)},
]
```

我们将函数直接添加到`set_limits`下方。

```py
def draw_map(
    maps_to_draw, 
    boundry_map_index=0,
    use_hatch_for_indexes=[],
    padding={},
    labels=[],
    figsize=(40, 40)
):

    ...
    # Additional functions below this comment
    set_limits(ax, maps_to_draw[boundry_map_index], **padding)

    for label in labels:
        add_label(ax, label)

    return ax
```

然后将其传递给`draw_map()`。

```py
ax = draw_map(
    maps_to_draw=[world, africa, disputed_areas], boundry_map_index=1,
    padding={"pad_bottom": -0.08, "pad_top": 0.07, "pad_left": 0.07, "pad_right": 0.05},
    use_hatch_for_indexes=[2]
    labels=country_labels
)

plt.axis("off")
plt.show()
```

这是我们得到的结果。

![](img/65d73dbba94e51d1aeab43adb396cac5.png)

地图由作者制作

太棒了！现在，我可以看到我正在查看哪些国家，最后要做的是解释颜色。

## 第 8 步：添加图例和标题

在 matplotlib 中添加图例很简单，所以为了让它更有趣，我想使用国家的形状，而不是标准的圆圈或方块。

这个步骤是一个例子，其中我使用`draw_map()`返回的 ax，而不是直接将其添加到函数中。

为了将一个国家的几何图形转移到另一个位置，我创建了以下函数，它接受一个`row`并将形状放置在（`x_loc`，`y_loc`）。

```py
def draw_legend_geometry(ax, row, x_loc, y_loc, height):
    x = np.array(row.geometry.boundary.coords.xy[0])
    y = np.array(row.geometry.boundary.coords.xy[1])

    x = x - (row.geometry.centroid.x - x_loc)
    y = y - (row.geometry.centroid.y - y_loc)

    ratio = height / (y.max() - y.min())
    x = x * ratio + (x_loc - x_loc * ratio)
    y = y * ratio + (y_loc - y_loc * ratio)

    ax.add_artist(Polygon(np.stack([x, y], axis=1), facecolor=row.color, edgecolor=row.edgecolor, hatch=row.hatch))
```

除了改变位置，它还会改变几何图形的比例，以达到特定的`height`。

为了使用`draw_legend_geometry()`，我创建了一个名为`legend`的数据框，其中选择了适当的国家来代表每种颜色。目前，它仅适用于具有单个`Polygon`的国家，而不适用于`MultiPolygon`。

我添加`INCOME_GRP`到西撒哈拉的方式有点“hacky”，利用它来排序值，并将`row.INCOME_GRP[3:]`作为文本，但现在这样也没关系。

我还在绘制图例后添加了标题和数据来源。

```py
ax = draw_map(
    maps_to_draw=[world, africa, disputed_areas], boundry_map_index=1,
    padding={"pad_bottom": -0.08, "pad_top": 0.07, "pad_left": 0.07, "pad_right": 0.05},
    use_hatch_for_indexes=[2]
    labels=country_labels,
)

legend = pd.concat([
    disputed_areas[disputed_areas.NAME_EN == "Western Sahara"],
    africa[africa.NAME_EN.isin(["Niger", "Senegal", "Botswana"])]
])

legend.loc[legend.NAME_EN == "Western Sahara", "INCOME_GRP"] = "6\. Disputed area"
legend = legend.sort_values("INCOME_GRP")
legend["hatch"] = ["", "", "", "//"]

for i, row in legend.reset_index().iterrows():
    draw_legend_geometry(ax, row, -25, -20 - 3.5*i, 2.5)
    ax.annotate(row.INCOME_GRP[3:], (-22, -20 - 3.5*i), fontsize=28, fontweight="bold", va="center")

fontstyles = {"fontweight": "bold", "ha": "left"}
plt.annotate("Data source:", xy=(0.05, 0.32), fontsize=24, xycoords="axes fraction", **fontstyles)
plt.annotate("The World Bank", xy=(0.133, 0.32), fontsize=24, xycoords="axes fraction", color="#1B998B", **fontstyles)
plt.title("Income Groups in Africa", x=0.05, y=0.29, fontsize=42, **fontstyles)

plt.axis("off")
plt.show()
```

如果我运行上述代码，我会得到以下地图，它与您在教程开始时看到的地图完全相同。

![](img/e7bc0b03cddbb3939afe4d6c954a889a.png)

地图由作者制作

这就是教程的最后一步。恭喜你完成了！

## 结论

你可以通过添加更多样式参数来使`draw_map()`函数更灵活（我这里有几个硬编码的值），但对我来说，这覆盖了 95%的使用场景。

我希望你喜欢这个教程，并学到了可以在项目中使用的东西。

如果你做到了，鼓掌，订阅，并分享，以便更多人能学会如何用 Python 和 Matplotlib 绘制美丽的地图。

你也应该看看我的其他教程：

![Oscar Leo](img/a3badd168c6bfbbdc3d060f9191ca1d2.png)

[Oscar Leo](https://medium.com/@oscarleo?source=post_page-----a6bd1f40fff--------------------------------)

## Matplotlib 教程

[查看列表](https://medium.com/@oscarleo/list/matplotlib-tutorials-262e5d7f0847?source=post_page-----a6bd1f40fff--------------------------------)8 篇故事！[](../Images/51b77b8f6d7ea69abdcd113427d4a52a.png)![](img/56c078b5447338a07b7bce2b23cf7133.png)![](img/c3088ee7cd4994f027ddddbc6ae423cd.png)

此外，订阅我的免费通讯，[Data Wonder](https://datawonder.substack.com/)，并在 Twitter 上关注我 [@oscarl3o](https://twitter.com/oscarl3o)。

感谢阅读，下次见。
