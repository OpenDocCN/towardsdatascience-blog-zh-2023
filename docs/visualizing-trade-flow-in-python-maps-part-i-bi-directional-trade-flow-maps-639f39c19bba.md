# 使用 Python 地图可视化贸易流量 — 第一部分：双向贸易流量地图

> 原文：[https://towardsdatascience.com/visualizing-trade-flow-in-python-maps-part-i-bi-directional-trade-flow-maps-639f39c19bba?source=collection_archive---------4-----------------------#2023-12-16](https://towardsdatascience.com/visualizing-trade-flow-in-python-maps-part-i-bi-directional-trade-flow-maps-639f39c19bba?source=collection_archive---------4-----------------------#2023-12-16)

[](https://medium.com/@himalaya.birshrestha?source=post_page-----639f39c19bba--------------------------------)[![Himalaya Bir Shrestha](../Images/9766140c1c44381029d0a78154217775.png)](https://medium.com/@himalaya.birshrestha?source=post_page-----639f39c19bba--------------------------------)[](https://towardsdatascience.com/?source=post_page-----639f39c19bba--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----639f39c19bba--------------------------------) [Himalaya Bir Shrestha](https://medium.com/@himalaya.birshrestha?source=post_page-----639f39c19bba--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fba33e6d0d27b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-trade-flow-in-python-maps-part-i-bi-directional-trade-flow-maps-639f39c19bba&user=Himalaya+Bir+Shrestha&userId=ba33e6d0d27b&source=post_page-ba33e6d0d27b----639f39c19bba---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----639f39c19bba--------------------------------) ·7分钟阅读·2023年12月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F639f39c19bba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-trade-flow-in-python-maps-part-i-bi-directional-trade-flow-maps-639f39c19bba&user=Himalaya+Bir+Shrestha&userId=ba33e6d0d27b&source=-----639f39c19bba---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F639f39c19bba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-trade-flow-in-python-maps-part-i-bi-directional-trade-flow-maps-639f39c19bba&source=-----639f39c19bba---------------------bookmark_footer-----------)

商品和服务的交换以及它们相应的价值是我们日常生活中的复杂部分。同样，国家之间也会进行各种类型的贸易关系，交换产品和服务，如电力、能源商品、原材料、加工品、旅游等。理解国家之间的贸易流动（进出口）对于评估一个国家的收入和支出、经济实力、供应安全以及国家之间的关系性质至关重要。

在这个两部分系列中，我将分享如何使用Python将国家之间的贸易流动可视化在地图上。本系列的第一部分将重点介绍可视化**双向（进口和出口）贸易流动**。第二部分将重点介绍可视化**净贸易流动**。我将使用一个假设产品的虚拟数据集来进行这种可视化。我将以我的国家和地区（尼泊尔/南亚）作为演示的例子。让我们开始吧。

![](../Images/12ae7950c17b8aab24d7838276dea766.png)

照片由[GeoJango Maps](https://unsplash.com/@geojango_maps?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

# **查找箭头的坐标**

在贸易流动地图中，我的目标是表示国家之间的双向贸易关系。例如，从尼泊尔到印度的出口将由第一个箭头（A1-A2）表示，从印度到尼泊尔的进口将由第二个箭头（A3-A4）表示。这样，每对国家关系将需要四个坐标点来定义箭头的起点和终点，以分别表示出口和进口。

尽管也可以假设一个可以自动检测到的坐标（例如，国家几何形状的中心点），我打算在地图上标记这些点并逐一获取其坐标。为此，可以在类似Google Earth的应用程序中创建一个项目，导出KML文件，并通过转换器提取坐标（例如，[MyGeodata Cloud](https://mygeodata.cloud/)网站上的GIS数据转换器）。

> Keyhole Markup Language (KML)是一种用于在应用程序中显示地理数据的文件格式，例如Google Earth。它使用基于标签的结构，具有嵌套的元素和属性，并基于XML标准（Google, 2023）。

# **数据**

我的输入数据结构如下图所示。它包含了五种不同的邻国之间的贸易关系：**尼泊尔-印度，尼泊尔-孟加拉国，尼泊尔-中国，印度-巴基斯坦，以及印度-斯里兰卡**。对于每对国家，有四个坐标点用于表示两个箭头的起点和终点。Value1表示从Country1到Country2的出口。Value2表示Country1从Country2的进口。目标是将这种关系在Python地图中展示出来。

![](../Images/f958bd65599f0ebadb0b11f148220325.png)

贸易流动地图的数据输入。图片由作者提供。

我将上述数据读入pandas dataframe `df`。此外，我创建了包含每对国家之间出口和进口量的字典对象，例如`transfers`，以及包含第一个箭头起点坐标的字典对象`startarrow1_dict`。

![](../Images/a585a531a1a562ce73ccd49ff3dfaba6.png)

创建必要的字典对象。图片由作者提供。

# 代码描述

在本节中，我将描述用于可视化贸易流图的代码。我将主要使用matplotlib和cartopy包。我还使用了相同的包来可视化[全球表面温度异常](https://assessing-global-temperature-anomaly-using-nasas-space-studies-part-ii-29e5e313a7b3)中的全球表面温度异常。

1.  **导入所需包**

我从导入主要的必需包和依赖项开始，如下所示：

```py
import cartopy.crs as ccrs
import cartopy.io.shapereader as shpreader

import matplotlib.pyplot as plt
import matplotlib.patches as mpatches
from matplotlib import colormaps
from matplotlib.colors import Normalize
from matplotlib.cm import ScalarMappable

import numpy as np
import pandas as pd
import os
```

**2\. 读取形状文件**

作为形状文件，我使用了Natural Earth [Vector](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/10m-admin-0-countries/)。矢量文件可以直接通过cartopy包的shapereader [模块](https://scitools.org.uk/cartopy/docs/latest/reference/generated/cartopy.io.shapereader.Reader.html)读取。

```py
# get the country border file (10m resolution) and extract
shpfilename = shpreader.natural_earth(
                           resolution=”10m”,
                           category=”cultural”,
                           name=”admin_0_countries”,
                          )
reader = shpreader.Reader(shpfilename)
countries = reader.records()
```

使用名为Fiona的包，可以读取所有国家的列表，如下所示。

![](../Images/aab7c11e4a540ec539451b8549163ff6.png)

使用Fiona包来打开形状文件并提取所有国家名称的列表。图片由作者提供。

**3\. 提取所需国家的信息**

接下来，我创建了`required`，这是一个包含六个有贸易关系的国家的列表。我还创建了一个字典对象`c`，其中包含FionaRecord，即所有相关的国家信息，可用于绘图。

```py
# required countries
required = [“Nepal”, “India”, “Bangladesh”,”China”,”Pakistan”,”Sri Lanka”]

# extract the specific country information
c = {
     co.attributes["ADMIN"]: co
     for co in countries if co.attributes["ADMIN"] in required
    }
```

**4\. 绘制** `**required**` **国家及裁剪**

在这一步，我首先绘制了`required`国家在PlateCarree投影中的几何形状，如下所示：

![](../Images/a9f5242012ec08d75c817328bfd2f179.png)

绘制所需的国家。图片由作者提供。

接下来，我希望裁剪掉其余世界的几何形状，以便只查看六个国家的放大视图。我确定了能够覆盖所有六个国家的最大和最小经纬度值，设置了坐标轴的范围，并绘制了这些国家。在for循环中，我还添加了一个代码，显示每个国家的名称在其重心几何形状上。

matplotlib包的`zorder` [属性](https://matplotlib.org/stable/gallery/misc/zorder_demo.html) 将决定艺术家的绘制顺序。具有较高`zorder`的艺术家会被绘制在顶部。

```py
# get overall boundary box from country bounds
extents = np.array([c[cn].bounds for cn in c])
lon = [extents.min(0)[0], extents.max(0)[2]]
lat = [extents.min(0)[1], extents.max(0)[3]]

ax = plt.axes(projection=ccrs.PlateCarree())

# get country centroids
ax.set_extent([lon[0] - 1, lon[1] + 1, lat[0] - 1, lat[1] + 1])

for key, cn in zip(c.keys(),c.values()):
    ax.add_geometries(cn.geometry,
                      crs=ccrs.PlateCarree(),
                      edgecolor="gray",
                      facecolor="whitesmoke",
                     zorder = 1)

    # Add country names
    centroid = cn.geometry.centroid

    ax.text(
        centroid.x,
        centroid.y,
        key,  # Assuming 'name' is the attribute containing the country names
        horizontalalignment='center',
        verticalalignment='center',
        transform=ccrs.PlateCarree(),
        fontsize=8,  # Adjust the font size as needed
        color='black',  # Set the color of the text
        zorder = 2
        )

plt.axis("off")
plt.show()
```

**5\. 设置色彩图，添加箭头补丁和颜色条。**

这是代码中最重要的部分。首先，我选择了`viridis_r`，即`viridis`的反向颜色调色板作为我的色彩图。接下来，我确定了任何国家之间的贸易值的最小和最大值，分别为`tmin`和`tmax`。这些值被标准化，使得`tmin`对应于色彩图`cmap`的最低端（0），`tmax`对应于最高端（1），并在随后的代码中相应使用。

然后，我遍历了`transfers`，并使用了[FancyArrowPatch](https://matplotlib.org/stable/api/_as_gen/matplotlib.patches.FancyArrowPatch.html)对象在国家之间绘制箭头。每个箭头对象都与一个独特的颜色`col`相关联，表示从一个国家到另一个国家的贸易流。虽然也可以使用第一个箭头坐标的偏移量来绘制第二个箭头，但我在代码中指定了第二个箭头的坐标。在代码中，`mutation_scale`属性用于控制箭头头部的长度，而`linewidth`属性用于控制主线的宽度。

最后，我在主图下方添加了水平颜色条。

```py
ax = plt.axes(projection=ccrs.PlateCarree())

# get country centroids
ax.set_extent([lon[0] - 1, lon[1] + 1, lat[0] - 1, lat[1] + 1])

for key, cn in zip(c.keys(),c.values()):
    ax.add_geometries(cn.geometry,
                      crs=ccrs.PlateCarree(),
                      edgecolor="grey",
                      facecolor="whitesmoke",
                     zorder = 1)

    # Add country names
    centroid = cn.geometry.centroid

    ax.text(
        centroid.x,
        centroid.y,
        key,  # Assuming 'name' is the attribute containing the country names
        horizontalalignment='center',
        verticalalignment='center',
        transform=ccrs.PlateCarree(),
        fontsize=8,  # Adjust the font size as needed
        color='black',  # Set the color of the text
        zorder = 2
       )

# set up a colormap
cmap = colormaps.get("viridis_r")
tmin = np.array([v for v in transfers.values()]).min()
tmax = np.array([v for v in transfers.values()]).max()
norm = Normalize(tmin, tmax)

for tr in transfers:
    c1, c2 = tr.split(",")
    startarrow1 = startarrow1_dict[tr]
    endarrow1 = endarrow1_dict[tr]

    startarrow2 = startarrow2_dict[tr]
    endarrow2 = endarrow2_dict[tr]

    t1 = transfers[tr][0]
    col = cmap(norm(t1))

    # Use the arrow function to draw arrows
    arrow = mpatches.FancyArrowPatch(
        (startarrow1[0], startarrow1[1]),
        (endarrow1[0], endarrow1[1]),
        mutation_scale=20,    #control the length of head of arrow 
        color=col,
        arrowstyle='-|>',
        linewidth=2,  # You can adjust the linewidth to control the arrow body width
        zorder = 3
    )
    ax.add_patch(arrow)

    #OTHER WAY
    offset = 1
    t2 = transfers[tr][1]
    col = cmap(norm(t2))
    arrow = mpatches.FancyArrowPatch(
        (startarrow2[0], startarrow2[1]),
        (endarrow2[0], endarrow2[1]),
        mutation_scale=20,
        color=col,
        arrowstyle='-|>',
        linewidth=2,  # You can adjust the linewidth to control the arrow body width
        zorder = 4
    )
    ax.add_patch(arrow)

sm = ScalarMappable(norm, cmap)
fig = plt.gcf()
cbar = fig.colorbar(sm, ax=ax,
            orientation = "horizontal",
            pad = 0.05,  #distance between main plot and colorbar
            shrink = 0.8, #control length
            aspect = 20  #control width
            )
cbar.set_label("Trade flow")

plt.title("Trade flow in South Asia")
plt.axis("off")

plt.savefig("trade_flow2_with_labels.jpeg",
           dpi = 300)
plt.show()
```

下面展示了最终产品。在我的虚拟数据集中，贸易流最少的是从斯里兰卡到印度的出口（53单位），用黄色表示。贸易流最多的是从孟加拉国到尼泊尔的出口（98单位），用紫色表示。

![](../Images/c95a7096d62472f4a7570c42e85afbe2.png)

通过箭头表示的国家间双向贸易流。图像由作者提供。

# 结论

在这篇文章中，我展示了如何通过使用两个箭头在 Python 地图中可视化国家之间的贸易流，包括出口和进口关系。我使用了 cartopy 和 matplotlib 包来实现这一目的。在本系列的第二部分，我将展示如何可视化“净”贸易流关系，并突出显示净出口国和净进口国。

本帖的笔记本可以在这个 GitHub [仓库](https://github.com/hbshrestha/Geospatial-Analysis/tree/main)中找到。感谢阅读！

**参考文献**

Google Developers, 2023\. [KML 教程 | Keyhole 标记语言 | Google 开发者](https://developers.google.com/kml/documentation/kml_tut)。本页面的内容根据[知识共享署名 4.0 国际许可证](https://creativecommons.org/licenses/by/4.0/)进行许可。
