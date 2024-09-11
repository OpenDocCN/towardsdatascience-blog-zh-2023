# geotiff.js: 如何获取纬度-经度坐标的投影 GeoTIFF 数据

> 原文：[https://towardsdatascience.com/geotiff-js-how-to-get-projected-data-for-a-latitude-longitude-coordinate-87ca437b5aa0?source=collection_archive---------0-----------------------#2023-03-12](https://towardsdatascience.com/geotiff-js-how-to-get-projected-data-for-a-latitude-longitude-coordinate-87ca437b5aa0?source=collection_archive---------0-----------------------#2023-03-12)

![](../Images/5196241c4e66b4b7d69b25384b8338d6.png)

作者提供的图片

## 使用 Javascript 将纬度和经度坐标重新投影到 GeoTIFF 的坐标系统中，并使用 geotiff.js 查询数据

[](https://medium.com/@potion_cellar?source=post_page-----87ca437b5aa0--------------------------------)[![托马斯·霍纳](../Images/9a50d463fb872d97f10f3ed454c08ac0.png)](https://medium.com/@potion_cellar?source=post_page-----87ca437b5aa0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87ca437b5aa0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----87ca437b5aa0--------------------------------) [托马斯·霍纳](https://medium.com/@potion_cellar?source=post_page-----87ca437b5aa0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9e4260fa5487&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeotiff-js-how-to-get-projected-data-for-a-latitude-longitude-coordinate-87ca437b5aa0&user=Thomas+Horner&userId=9e4260fa5487&source=post_page-9e4260fa5487----87ca437b5aa0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----87ca437b5aa0--------------------------------) · 10 min 阅读 · 2023年3月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87ca437b5aa0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeotiff-js-how-to-get-projected-data-for-a-latitude-longitude-coordinate-87ca437b5aa0&user=Thomas+Horner&userId=9e4260fa5487&source=-----87ca437b5aa0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87ca437b5aa0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeotiff-js-how-to-get-projected-data-for-a-latitude-longitude-coordinate-87ca437b5aa0&source=-----87ca437b5aa0---------------------bookmark_footer-----------)

Javascript 已被使用多年来提供交互式网页地图，这些地图通常由矢量数据和 RGB 图像切片组成。这些简单的前端 historically 需要更强大的语言和服务器技术来实际提供和渲染正在可视化的地理空间数据，以及查询或分析它们的机制。

随着Javascript语言和生态系统的成熟，曾经几乎不可能的事情现在变得简单而且性能不错。多亏了专注的开源开发者，现在在前端（浏览器）或后端（*NodeJS*）直接处理地理空间光栅数据变得相当容易。甚至还有多线程支持！

让我们看看在纯Javascript中曾经非常困难的一个功能：查询特定坐标的地理空间光栅数据。这些数据集通常以GeoTIFF格式提供，为了处理这些数据集，我们将使用一个更流行的处理文件格式的库：*geotiff.js*。

使用这个库从GeoTIFF中提取像素数据很简单。但是，给定一个纬度-经度坐标，你怎么知道读取哪个像素？如果GeoTIFF在一个投影坐标系统中怎么办？

与*GDAL*（一个流行的开源地理空间库，具有NodeJS绑定）不同，*geotiff.js*没有重投影功能：它仅仅是解析GeoTIFF文件。这意味着我们的解决方案将比*GDAL*中的等效方法稍微复杂一些，后者在最简单的情况下只需一个命令：

```py
gdallocationinfo -valonly -wgs84 <geotiff filename> <longitude> <latitude>
```

我们只需编写一些代码来镜像GDAL在后台进行的额外工作：从光栅中提取投影信息，将纬度-经度坐标重新投影到光栅的坐标系统中，并确定该点适用于光栅的哪个像素。

## 为什么不首先重新投影整个GeoTIFF？

通过将我们的GeoTIFF重新投影到像EPSG:4326这样的纬度-经度坐标系统，整个过程大部分可以被规避。但对于许多投影坐标系统，这会引入严重的不准确性。

例如，考虑查询NBM天气模型以获取科罗拉多州小熊峰顶的预测温度：

![](../Images/0a24cfc9e19b012557b65ffcd2903554.png)

图片来源：作者

选择这个例子是因为它靠近数据集中一个像素的边缘。

现在让我们使用最近邻重采样将数据集重新投影到EPSG:4326。相同的坐标现在返回了不同的值：

![](../Images/0844fda3e81165158fe1d5a000d53369.png)

图片来源：作者

我们可以通过使用不同的重采样方法来稍微改进重投影，比如双线性插值：

![](../Images/3f51da4954cd2d1b19b3cb25bdf3b944.png)

图片来源：作者

但显然，我们丢失了一些原始天气模型的信息。我们可以通过使重新投影的光栅图像变得非常非常大来减少丢失的信息量，但即使在巨大的尺寸下，仍然会有一些边缘情况，其中靠近像素边界的地点无法正确提取原始天气模型数据。

这是否重要取决于数据的用途和分辨率。如果你使用的是高分辨率的数字高程模型来为长达数英里的路线构建高程剖面图，那么如果高程数据在某些地方略有偏差可能不是什么大问题。相反，如果你使用的是分辨率较粗的气象模型数据，那么处于错误的网格单元可能会导致该区域的预测结果有显著不同。

***总体而言：最好使用原始投影来查询来自地理空间光栅的原始数据。***

## 将纬度和经度重投影到光栅坐标系统

第一步是获取GeoTIFF的投影信息，并使用该信息将我们期望的坐标重新投影到光栅的坐标系统中。

使GeoTIFF成为地理空间光栅而非普通TIFF图像的一个重要因素是使用GeoKeys（参见[OGC GeoTIFF规范](https://docs.opengeospatial.org/is/19-008r4/19-008r4.html#_underlying_tiff_requirements)）。我们可以使用geotiff.js读取给定光栅的GeoKeys：

```py
// assume the variable gtiff is the geotiff object
// that was created by loading data with geotiff.js
const image = await gtiff.getImage();
const geoKeys = image.getGeoKeys();
```

我们将把这些GeoKeys传递给一个投影库，它将处理繁重的计算，因为投影往往是相当复杂的！

例如，北美的气象模型数据往往使用Lambert等角圆锥投影。如果我们从一个源自椭球基准（通常是WGS84）的纬度经度坐标开始，那么我们会面临一个相当复杂的公式：

![](../Images/4619872ac569874dee5d248f682cbb47.png)

Snyder, John (1987). [“地图投影：工作手册（USGS专业论文：1395）”](https://pubs.er.usgs.gov/publication/pp1395) — 公有领域

不需要重新发明轮子，因为开源社区已经投入了多年的工作，创建了全面的重投影库，这些库可以轻松处理各种复杂的地图投影之间的转换。

对于JavaScript，最强大、最流行且功能齐全的库之一是[*Proj4js*](https://github.com/proj4js/proj4js)，它是常见的[*proj*](https://proj.org/)库的分支，支持*GDAL*。我们只需使用光栅的GeoKeys生成一个*proj*字符串，并将最终字符串传递给库即可。

然而，有很多有效的GeoKeys：

```py
/**
 * Geokeys. If you're working with `geotiff` library, this is result of `image.getGeoKeys()`.
 * @typedef {Object} module:geokeysToProj4.GeoKeys
 * @property {number} GeographicTypeGeoKey See GeoTIFF docs for more information
 * @property {number} GeogGeodeticDatumGeoKey See GeoTIFF docs for more information
 * @property {number} GeogPrimeMeridianGeoKey See GeoTIFF docs for more information
 * @property {number} GeogLinearUnitsGeoKey See GeoTIFF docs for more information
 * @property {number} GeogLinearUnitSizeGeoKey See GeoTIFF docs for more information
 * @property {number} GeogAngularUnitsGeoKey See GeoTIFF docs for more information
 * @property {number} GeogAngularUnitSizeGeoKey See GeoTIFF docs for more information
 * @property {number} GeogEllipsoidGeoKey See GeoTIFF docs for more information
 * @property {number} GeogSemiMajorAxisGeoKey See GeoTIFF docs for more information
 * @property {number} GeogSemiMinorAxisGeoKey See GeoTIFF docs for more information
 * @property {number} GeogInvFlatteningGeoKey See GeoTIFF docs for more information
 * @property {number} GeogPrimeMeridianLongGeoKey See GeoTIFF docs for more information
 * @property {number} ProjectedCSTypeGeoKey See GeoTIFF docs for more information
 * @property {number} ProjectionGeoKey See GeoTIFF docs for more information
 * @property {number} ProjCoordTransGeoKey See GeoTIFF docs for more information
 * @property {number} ProjLinearUnitsGeoKey See GeoTIFF docs for more information
 * @property {number} ProjLinearUnitSizeGeoKey See GeoTIFF docs for more information
 * @property {number} ProjStdParallel1GeoKey See GeoTIFF docs for more information
 * @property {number} ProjStdParallel2GeoKey See GeoTIFF docs for more information
 * @property {number} ProjNatOriginLongGeoKey See GeoTIFF docs for more information
 * @property {number} ProjNatOriginLatGeoKey See GeoTIFF docs for more information
 * @property {number} ProjFalseEastingGeoKey See GeoTIFF docs for more information
 * @property {number} ProjFalseNorthingGeoKey See GeoTIFF docs for more information
 * @property {number} ProjFalseOriginLongGeoKey See GeoTIFF docs for more information
 * @property {number} ProjFalseOriginLatGeoKey See GeoTIFF docs for more information
 * @property {number} ProjFalseOriginEastingGeoKey See GeoTIFF docs for more information
 * @property {number} ProjFalseOriginNorthingGeoKey See GeoTIFF docs for more information
 * @property {number} ProjCenterLongGeoKey See GeoTIFF docs for more information
 * @property {number} ProjCenterLatGeoKey See GeoTIFF docs for more information
 * @property {number} ProjCenterEastingGeoKey See GeoTIFF docs for more information
 * @property {number} ProjCenterNorthingGeoKey See GeoTIFF docs for more information
 * @property {number} ProjScaleAtNatOriginGeoKey See GeoTIFF docs for more information
 * @property {number} ProjScaleAtCenterGeoKey See GeoTIFF docs for more information
 * @property {number} ProjAzimuthAngleGeoKey See GeoTIFF docs for more information
 * @property {number} ProjStraightVertPoleLongGeoKey See GeoTIFF docs for more information
 * @property {number[]} GeogTOWGS84GeoKey Datum to WGS transformation parameters, unofficial key
 */
```

使用类似[*geotiff-geokeys-to-proj4*](https://github.com/matafokka/geotiff-geokeys-to-proj4)的辅助库来处理*geotiff.js*的geokeys对象到*proj*字符串的正确转换可能是值得的。实际上，上面的注释块来自于那个库。

安装了这两个库后，我们无需编写太多代码就能设置我们的重投影功能：

```py
import proj4 from 'proj4';
import * as geokeysToProj4 from 'geotiff-geokeys-to-proj4';

... // Not shown: importing geotiff.js and loading a geotiff file.
    // for this example, the variable `gtiff` references the file.

const image = await gtiff.getImage();
const geoKeys = image.getGeoKeys();
const projObj = geokeysToProj4.toProj4( geoKeys );
const projection = proj4( `WGS84`, projObj.proj4 );
```

我们现在创建了一个名为“projection”的proj投影对象。使用起来非常简单：

```py
const { x, y } = projection.forward( {
    x: -105,   // the longitude
    y: 40      // the latitude
} );
```

上面的代码将纬度经度坐标重新投影到光栅坐标系统（可能是米或英尺）。

现在我们需要获取与这些新坐标关联的实际栅格像素。首先，让我们获取有关栅格大小及其边界框的一些信息：

```py
const width = image.getWidth();
const height = image.getHeight();
const [ originX, originY ] = image.getOrigin();
const [ xSize, ySize ] = image.getResolution();
const uWidth = xSize * width;
const uHeight = ySize * height;
```

上述代码建立了栅格像素大小与投影坐标系统中栅格的实际坐标/大小之间的关系。

然后我们将投影坐标相对于栅格的原点坐标进行处理，查看它在栅格的宽度和高度中的位置。

```py
// x and y come from the projection.forward example earlier
const percentX = ( x - originX ) / uWidth;
const percentY = ( y - originY ) / uHeight;

const pixelX = Math.floor( width * percentX );
const pixelY = Math.floor( height * percentY );
```

现在我们有了原始经纬度坐标的像素值！

你可能会想确保这个像素确实在栅格内。如果原始坐标没有与栅格相交，那么 pixelX 或 pixelY 可能会是负值或大于栅格的宽度/高度。

## 提取像素的 GeoTIFF 数据

如果像素在栅格内，那么考虑到一个单一波段的 GeoTIFF，我们有两种提取数据的方法。

第一种方法是将一个 1x1 像素的窗口传递给 *geotiff.js* 的“readRasters”函数：

```py
const [ value ] = await image.readRasters( {
    interleave: true,
    window: [ pixelX, pixelY, pixelX + 1, pixelY + 1],
    samples: [ 0 ]
} );
```

这种方法简单明了，但调用“readRasters”有一个小的开销，如果你查询数千个坐标，这个开销可能会累积。

在这种情况下，采用不同的方法可能会带来轻微的性能提升——但代价是 RAM，这对于非常大的 GeoTIFF 可能会相当可观。我们可以将整个栅格读取到内存中作为 ArrayBuffer：

```py
const data = await image.readRasters( {
    interleave: true,
    samples: [ 0 ]
} );
```

然后我们只需要获取相应的索引：

```py
const value = data[ width * pixelY + pixelX ];
```

实际上，*geotiff.js* 在内部缓存瓦片和条带，因此性能提升充其量是微乎其微的，并且对于适中的数据集通常不明显。

因此，如果查询的数据分布在 GeoTIFF 中的大多数条带或瓦片上，这种方法只会提供（轻微的）速度提升。否则，读取整个图像的开销，包括未使用的瓦片或条带，将超过潜在的性能提升，不过这可以通过读取一个大小调整到最小和最大 x 和 y 或坐标集合的窗口来缓解。

使用上述技术，可以在交互式网页地图中实现以下功能，无论是完全在浏览器中还是通过基本的 NodeJS API：

+   点击网页地图以获取特定点的栅格数据

+   使用底层栅格数据对点特征进行标注（例如，显示天气预报温度的城市名称的天气地图）

+   对栅格数据执行基本的几何操作（例如，使用点集合或边界框进行交集操作），并将该信息显示在表格中或导出为 CSV

+   为二维横截面或线生成图表或数据列表（例如，海拔剖面）

+   对原始 GeoTIFF 数据进行统计分析和数据转换

这种功能（更不用说实际将地理空间栅格数据渲染到地图上）传统上需要专用的 GIS 服务器软件和专用基础设施，交互式网络地图的作用仅仅是向服务器发送请求并显示响应。随着 JavaScript 生态系统的发展，浏览器变得越来越能够解析地理空间数据，从而减少了对专用后端技术的依赖。

传统上，使 GeoTIFF/地理空间栅格数据可供网络地图使用的一种流行方法是拥有一个提供通用开放地理空间联盟接口的 GIS 服务器应用程序——如 WMS（Web 地图服务）或 WMTS（Web 地图瓦片服务）。这些接口允许浏览器请求图像文件或数据的瓦片，这些图像在服务器端渲染后作为 RGB 图像文件发送到浏览器。

服务器还可以提供 WCS（Web 覆盖服务）接口，允许浏览器查询特定坐标的底层地理空间栅格数据。如果使用专有的 GIS 服务器技术（如 Esri 的 ArcGIS 产品），端点和语法可能会有所不同，但最终结果是相同的。

然而，随着云优化 GeoTIFF 标准的成熟以及使用 *openlayers* + *geotiff.js* 直接在浏览器中访问和渲染数据的能力，这种技术在某些场景下已经消除了对提供 WMS 和 WMTS 端点的 GIS 服务器的需求。本文描述的查询技术展示了一种去除对 WCS 或查询端点需求的方法。

取而代之的是，可以仅仅利用现有的云提供商（例如，一个 S3 存储桶）来托管 COGs，并让网络浏览器负责解析和处理地理空间数据。具有 NodeJS 后端的组织可以将 GIS 功能整合到这些堆栈中，从而减少对专用软件和额外硬件的需求。

最后，能够在浏览器中处理更多地理空间栅格数据的另一个巨大好处是减少或消除了对互联网连接的需求。对于智能手机地图应用程序，特别是那些旨在远程地区使用的应用程序，这为已经用 JavaScript 编写的应用程序提供了一种离线地理空间工具的途径。

这些应用程序中有几款非常流行，提供了诸如坡度角计算、地形剖面图、保存的天气地图等功能。这些应用允许你保存地图或区域以供离线使用——但一旦用户离线，许多功能会停止工作，因为它们依赖于服务器来实际解析和分析地理空间数据。

这些功能在失去互联网连接后不一定会中断——本文提到的技术只是众多可以让应用在离线时仍然保持功能的技术之一。
