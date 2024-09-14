# 如何在 Landsat 卫星图像上绘制坐标，使用 Python

> 原文：[`towardsdatascience.com/how-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa`](https://towardsdatascience.com/how-to-plot-coordinates-on-landsat-satellite-images-with-python-5671613887aa)

## 使用 Landsat 元数据和 Rasterio 将像素位置映射到地理坐标

[](https://conorosullyds.medium.com/?source=post_page-----5671613887aa--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----5671613887aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5671613887aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5671613887aa--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----5671613887aa--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5671613887aa--------------------------------) ·阅读时间 8 分钟·2023 年 5 月 16 日

--

![](img/45e764239f69e9589c7b61ffd50583f1.png)

图片来源于 [GeoJango Maps](https://unsplash.com/@geojango_maps?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

位置，位置，位置！这不仅仅是房地产市场的陈词滥调，而且对于遥感也极其重要。无论是监测环境变化、分析城市发展，还是跟踪作物健康，精确的地理定位至关重要。我们需要知道卫星图像中物体的确切坐标，以确保分析和解释的准确性。

因此，我们将探讨如何使用两种方法将坐标直接绘制到 Landsat 场景上：

+   Landsat 元数据文件 (MLT)

+   Rasterio — 一个用于访问地理空间栅格数据的包

我们还将使用 Rasterio 来 **重新投影** 卫星图像的坐标。特别是，我们将从原始坐标参考系统 **(UTM)** 转换到 Google Maps 使用的坐标系统 **(EPSG:4326)**。在此过程中，我们将讨论代码，你可以在 [GitHub](https://github.com/conorosully/medium-articles/blob/master/src/remote%20sensing/landsat_GPS.ipynb) 上找到完整的项目。

# 下载 Landsat 场景

我们需要从下载一个 Landsat 场景开始。你可以使用 [EarthExplorer](http://earthexplorer.usgs.gov) 门户完成此操作。或者，如果你想使用 Python，下面的文章会引导你完成整个过程：

[](/downloading-landsat-satellite-images-with-python-a2d2b5183fb7?source=post_page-----5671613887aa--------------------------------) ## 使用 Python 下载 Landsat 卫星图像

### 使用 landsatxplore Python 包简化 Landsat 场景下载

towardsdatascience.com

最终，你应该有一个类似于**图 1**的文件夹。这些是所有可用的[Landsat 二级科学产品](https://www.usgs.gov/landsat-missions/landsat-collection-2-level-2-science-products)文件。我们将使用可见红光带（***_B4.TIF**）和 JSON 元数据文件（***_MTL.json**）。这些文件在下面已被突出显示。

![](img/72690c5614f470746158f244bfcedea7.png)

图 1：Landsat 二级科学产品文件（来源：作者）

这个特定场景拍摄于爱尔兰东海岸上方。为了查看这个，我们可视化红光带。我们加载 tiff 文件（第 9 行）。我们[缩放光带](https://www.usgs.gov/faqs/how-do-i-use-scale-factor-landsat-level-2-science-products)（第 12 行）并裁剪以增强对比度（第 15 行）。你可以在**图 2**中看到结果图像。

```py
import tifffile as tiff
import numpy as np
import matplotlib.pyplot as plt

# Landsat scene ID 
ID = 'LC08_L2SP_206023_20221118_20221128_02_T1'

# Load Red (B4) band
B4 = tiff.imread('./data/{}/{}_SR_B4.TIF'.format(ID, ID))

# Scale band
B4 = np.clip(B4*0.0000275-0.2, 0, 1)

# Clip to enhance contrast
B4 = np.clip(B4,0,0.2)/0.2

# Display grayscale image
fig, ax = plt.subplots(figsize=(10, 10))
plt.imshow(B4, cmap='gray')
ax.set_axis_off()
```

![](img/479bbc78afa84f057620906d134e887e.png)

图 2：爱尔兰东海岸 Landsat 场景的可见红光带（来源：作者）

我们希望将一个点直接绘制到这个图像上，指向爱尔兰村庄 Howth。看着**图 2**，你可以看到它大约在沿海的 1/3 处突出。我们要绘制的具体经纬度点在**图 3**中给出。

![](img/67741b06fe8bf6bff3d54c9dc98456ba.png)

图 3：Howth, Dublin 的纬度和经度坐标（来源：谷歌地图）

# 使用 Landsat 元数据（MLT）绘制点

好的，因此使用 MLT 文件绘制点不像使用 Rasterio 那样简单。不过，探索这种方法是值得的。你会发现我们不必依赖于任何包。我们还将了解 Landsat 使用的坐标参考系统（CRS）。

## Landsat 元数据

MLT 文件包含大量关于 Landsat 场景的元数据。你可以在[数据格式控制手册](https://www.usgs.gov/media/files/landsat-8-9-olitirs-collection-2-level-2-data-format-control-book)中找到所有细节。我们对**投影属性**感兴趣。这些是关于场景 CRS 的详细信息，并将允许我们对**图 2**中的像素进行地理定位。我们加载这些属性（第 4–5 行）。

```py
import json

# Load metadata file
MTL = json.load(open('./data/{}/{}_MTL.json'.format(ID, ID)))
projection = MTL['LANDSAT_METADATA_FILE']['PROJECTION_ATTRIBUTES']
projection
```

**图 4**提供了投影属性的列表。第一个项目告诉我们[UTM](https://www.usgs.gov/faqs/how-are-utm-coordinates-measured-usgs-topographic-maps#:~:text=The%20UTM%20(Universal%20Transverse%20Mercator,Zone%2019%2C%20which%20includes%20Maine.) 是该场景的 CRS。这是一个投影坐标系统（PCS）。它用于将地球表面（球体）上的地理位置投影到场景（二维表面）上。稍后我们将看到它与地理坐标系统（GCS）的不同之处。

![](img/4464cca16e268f97e8662b0f17b47f3a.png)

图 4：Landsat 场景的投影属性（来源：作者）

在将 UTM 转换为像素位置时，我们需要从**图 4**中获取另外两条信息。这些是：

+   GRID_CELL_SIZE_REFLECTVE——这告诉我们每个像素覆盖 30 平方米的土地

+   投影坐标（例如 CORNER_UL_PROJECTION_X_PRODUCT）——这些给出了场景边界框的 UTM 坐标（以米为单位）

实际上，我们只需要前两个坐标。这些坐标是用于边界框的左上角（UL）。查看**图 5**，我们可以看到这些信息如何用于转换为像素位置。我们知道每个像素是 30 平方米。这意味着我们可以从（543000，6004200）开始，增加/减少 30 米来获取下一个像素的起始位置。如果 UTM 坐标落在该像素范围内，我们就选择这个像素。

![](img/6e3a6a8f18549a06a93222c69d1a2ef4.png)

图 5：像素起始和结束的 UTM 坐标

## 在 Landsat 波段上绘制坐标

我们使用这些信息创建了**coords_to_pixels**函数。作为参数，它接受 UL 角的 UTM 坐标（**ul**）和我们想要转换的坐标（**utm**）。

```py
def coords_to_pixels(ul, utm, m=30):
    """ Convert UTM coordinates to pixel coordinates"""

    x = int((utm[0] - ul[0])/m)
    y = int((ul[1] - utm[1])/m)

    return x, y
```

我们现在可以使用这个函数来找到我们在 Howth 的点的像素。我们从 UL 角的 UTM 坐标开始（第 4-5 行）。我们直接从谷歌地图上获取了我们的点，因此它们以纬度和经度的形式给出（第 8-9 行）。我们将这些转换为 UTM 坐标（第 12 行），得到**（694624，5918089）**。最后，我们将这些转换为像素（第 16 行）。这给了我们像素值**（5054，2870）**。

```py
import utm 

# Get UTM coordinates of upper left corner
ul = [float(projection['CORNER_UL_PROJECTION_X_PRODUCT']),
        float(projection['CORNER_UL_PROJECTION_Y_PRODUCT'])]

# Lat/long coordinates of Howth
lat=53.3760
long=-6.0741

# Convert GPS to UTM coordinates
utmx,utmy,_,_ = utm.from_latlon(lat,long)
print(utmx,utmy)

# Convert UTM to pixel coordinates
x,y = coords_to_pixels(ul, [utmx,utmy])
print(x,y)
```

让我们看看这有多准确。我们在**图 2**中给定的像素值（第 5 行）上向图像中添加一个白色圆圈。为了看到我们的点，我们通过裁剪图像周围的圆圈来放大（第 8-9 行）。将**图 6**与**图 3**中的点进行比较，我们可以看到我们已经准确地将点绘制到 Landsat 场景中。一般而言，[90%的 Landsat 点位置误差小于 12 米](https://www.sciencedirect.com/science/article/pii/S003442571400042X)。

```py
import cv2 

# Add circle to image at pixel coordinates
image = B4.copy()
image = cv2.circle(image, (int(x), int(y)), 10, 1,5)

# Crop image around pixel coordinates
h = 100
crop_image = image[y-h:y+h, x-h:x+h]
```

![](img/eab237551bd84541beea2e6fb4a53305.png)

图 6：使用 Landsat 元数据文件绘制的 Howth 坐标（来源：作者）

# 使用 rasterio 绘制坐标

上述过程有效！但是，我们可以使用 Rasterio 包让我们的生活更轻松。我们打开红色可见光波段的 tiff 文件（第 4 行）。场景的元数据嵌入在文件中。例如，我们打印 CRS（第 7 行），输出**“EPSG:32629”**。这是[EPSG 代码](https://epsg.io/32629)，表示 UTM 区域 29N。换句话说，与 MLT 文件中给出的 CRS 相同。

```py
import rasterio as rio

# Open red channel with rasterio
B4 = rio.open('./data/{}/{}_SR_B4.TIF'.format(ID, ID))

# Display image map projection 
print(B4.crs)
```

我们可以使用**index**函数将 UTM 转换为像素坐标（第 9 行）。这输出**（5054，2870）**。和之前一样！

```py
# Lat/Long coordinates of Howth
lat=53.3760
long=-6.0741

# Get UTM coordinates
utmx,utmy,_,_ = utm.from_latlon(lat,long)

# Convert UTM to pixel coordinates
y,x = B4.index(utmx,utmy)
print(x,y)
```

所以，使用 Rasterio，我们不再需要担心边界框甚至 CRS。然而，我们仍然需要将纬度和经度转换为 UTM。如果我们想直接绘制纬度和经度，我们将不得不将 Landsat 场景重新投影到 Google Maps CRS。

## 重投影 Landsat 场景

我们使用下面的代码来完成这一任务。总的来说，它将红色光谱带图像重投影到 EPSG:4326（Google Maps CRS），并将重投影图像保存到新文件中。有几点需要注意：

+   我们计算一个新的转换函数（第 12 行）。这是 Rasterio 用来将坐标转换为像素的内容。

+   当我们重投影文件时，我们使用最近邻重采样方法（第 42 行）。这意味着我们使用最近的单元格的值来填充其他单元格的值。

我们需要一种重采样方法，因为重投影过程会“扭曲”图像以适应新的 CRS。我们将在输出重投影图像时看到这一点。

```py
from rasterio.warp import reproject, Resampling, calculate_default_transform

dst_crs = "EPSG:4326"  # google maps CRS
filename = './data/{}/{}_SR_B4.TIF'.format(ID, ID)
new_filename = './data/{}/{}_SR_B4_EPSG4326.tif'.format(ID, ID)

# Open file
with rio.open(filename) as src:
    src_transform = src.transform

    # calculate the transform matrix for the output
    dst_transform, width, height = calculate_default_transform(
        src.crs,
        dst_crs,
        src.width,
        src.height,
        *src.bounds,  # unpacks outer boundaries
    )

    # set properties for output
    dst_kwargs = src.meta.copy()
    dst_kwargs.update(
        {
            "crs": dst_crs,
            "transform": dst_transform,
            "width": width,
            "height": height,
            "nodata": 0,  
        }
    )

    # write to disk
    with rio.open(new_filename, "w", **dst_kwargs) as dst:
        # reproject to new CRS
        reproject(
            source=rio.band(src, 1),
            destination=rio.band(dst, 1),
            src_transform=src.transform,
            src_crs=src.crs,
            dst_transform=dst_transform,
            dst_crs=dst_crs,
            resampling=Resampling.nearest)
```

为了确认过程已成功完成，我们加载投影图像（第 2 行）并输出其 CRS（第 3 行）。这给我们 **“EPSG:4326”**。成功！

```py
# Open red channel
B4 = rio.open('./data/{}/{}_SR_B4_EPSG4326.TIF'.format(ID, ID))
print(B4.crs)
```

现在我们可以直接从经纬度坐标获取像素坐标。这给出了**（6336，2227）**的值。这些值与之前不同，因为如前所述，投影图像已经被扭曲。

```py
# GPS coordinates of Howth
lat=53.3760
long=-6.0741

# Convert UTM to pixel coordinates
y,x = B4.index(long,lat)
print(x,y)
```

当我们在**图 7**中绘制点在投影图像上时，我们可以看到这一点。与**图 6**相比，纵横比发生了变化。实际上，许多像素已经被更改。这是因为，与 UTM 不同，EPSG:4326 是一个*地理*坐标系统（GCS）。它用于在球体上（即地球）绘制点。在二维平面上显示具有此 CRS 的图像会导致纵横比的扭曲。

![](img/19ba93f168f4927b41283bbe8b63ac18.png)

图 6：霍斯坐标在投影图像上绘制（来源：作者）

本文重点讨论了在单个光谱带上绘制坐标。如果你正在处理多个光谱带或 RGB 可视化，底层方法将是相同的。这是因为 Landsat 场景中的所有光谱带将具有相同的 CRS。有关 RGB 通道可视化的详细信息，请参见下面的文章。

[](/visualising-the-rgb-channels-of-satellite-images-with-python-6d541af1f98d?source=post_page-----5671613887aa--------------------------------) ## 使用 Python 可视化卫星图像的 RGB 通道

### 如何处理可视化卫星图像时的多个光谱带、大像素值和扭曲的 RGB 通道

[towardsdatascience.com

希望你喜欢这篇文章！你可以在 [Mastodon](https://sigmoid.social/@conorosully) | [Twitter](https://twitter.com/conorosullyDS) | [YouTube](https://www.youtube.com/channel/UChsoWqJbEjBwrn00Zvghi4w) | [Newsletter](https://mailchi.mp/aa82a5ce1dc0/signup) 上找到我 — 免费注册以获取 [Python SHAP 课程](https://adataodyssey.com/courses/shap-with-python/)

[](https://conorosullyds.medium.com/membership?source=post_page-----5671613887aa--------------------------------) [## 使用我的推荐链接加入 Medium — Conor O’Sullivan

### 作为 Medium 会员，你的会员费的一部分将用于支持你阅读的作者，并且你可以全面访问每个故事……

[conorosullyds.medium.com](https://conorosullyds.medium.com/membership?source=post_page-----5671613887aa--------------------------------)

## **参考资料**

[Maurício Cordeiro](https://medium.com/u/8878c77fe1a3?source=post_page-----5671613887aa--------------------------------) **Python 在地球科学中的应用：使用 Rasterio 进行栅格合并、裁剪和重投影** [`medium.com/analytics-vidhya/python-for-geosciences-raster-merging-clipping-and-reprojection-with-rasterio-9f05f012b88a`](https://medium.com/analytics-vidhya/python-for-geosciences-raster-merging-clipping-and-reprojection-with-rasterio-9f05f012b88a)

BGU **栅格 (rasterio)** [`geobgu.xyz/py/rasterio.html#raster-file-connection`](https://geobgu.xyz/py/rasterio.html#raster-file-connection)

[gerdoo](https://medium.com/u/4334b7a2376e?source=post_page-----5671613887aa--------------------------------) **地理参考 Landsat 图像的索引** [`medium.com/@gerdoo/georeferencing-landsat-images-in-python-92d5d5aafc67`](https://medium.com/@gerdoo/georeferencing-landsat-images-in-python-92d5d5aafc67)

PyGIS **栅格坐标参考系统 (CRS)** [`pygis.io/docs/d_raster_crs_intro.html`](https://pygis.io/docs/d_raster_crs_intro.html)
