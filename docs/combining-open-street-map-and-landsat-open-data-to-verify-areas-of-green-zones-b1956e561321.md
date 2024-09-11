# 结合开放街道地图和Landsat开放数据来验证绿色区域

> 原文：[https://towardsdatascience.com/combining-open-street-map-and-landsat-open-data-to-verify-areas-of-green-zones-b1956e561321?source=collection_archive---------4-----------------------#2023-10-02](https://towardsdatascience.com/combining-open-street-map-and-landsat-open-data-to-verify-areas-of-green-zones-b1956e561321?source=collection_archive---------4-----------------------#2023-10-02)

## 快速简便的分析以便准确评估

[](https://medium.com/@mik.sarafanov?source=post_page-----b1956e561321--------------------------------)[![Mikhail Sarafanov](../Images/88869a3c6f664785c90539dd7aab6d74.png)](https://medium.com/@mik.sarafanov?source=post_page-----b1956e561321--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1956e561321--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b1956e561321--------------------------------) [Mikhail Sarafanov](https://medium.com/@mik.sarafanov?source=post_page-----b1956e561321--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F209c78c40898&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-open-street-map-and-landsat-open-data-to-verify-areas-of-green-zones-b1956e561321&user=Mikhail+Sarafanov&userId=209c78c40898&source=post_page-209c78c40898----b1956e561321---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1956e561321--------------------------------) ·7 分钟阅读·2023年10月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb1956e561321&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-open-street-map-and-landsat-open-data-to-verify-areas-of-green-zones-b1956e561321&user=Mikhail+Sarafanov&userId=209c78c40898&source=-----b1956e561321---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb1956e561321&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombining-open-street-map-and-landsat-open-data-to-verify-areas-of-green-zones-b1956e561321&source=-----b1956e561321---------------------bookmark_footer-----------)![](../Images/b24f6f438be853dba67cd6596a75981e.png)

预览图（作者提供）

今天我们想讨论结合来自各种开放数据源的空间数据的好处。例如，我们将考虑以下任务：评估某个特定属性是否位于“绿色”区域。废话少说，让我们开始吧！

附言：下面，我们考虑了一种仅基于开放数据的简单计算方法，涵盖了大多数（我希望是所有）城市。因此，下面你不会看到 [基于这些数据训练的机器学习方法的描述，这些数据不太可能免费获得](https://www.mdpi.com/2072-4292/12/18/3017)。

**方法 1\. 最准确，但不适合懒人**

假设在“*柏林，Neustädtische Kirchstraße 4–7*”的城市属性上出现了这样的评估任务。这个任务不是由某个全职程序员给出的，而是由一个可能不会编程但勤奋且愿意学习新知识的人给出的。在网上搜索后，“区域绿化评估专家”已经下载了 QGIS 并学会了使用 Quick Map Services 模块。接下来的步骤可能很明显，但迄今为止并不容易：需要弄清楚空间数据在地理信息系统（GIS）中是以何种格式表示的（矢量和栅格），然后学习如何创建这些对象并将其进行比较（以计算面积）。

![](../Images/ee2543df8da252e01b0b6ad7052f95b4.png)

图 1\. 以免你在阅读时感到无聊：这就是用于描述空间对象的 GIS 原语（作者图像）

尽管除了上述操作外，还需要填补许多其他知识空白（例如，什么是坐标参考系统以及它们是什么），但最耗时的过程可能是对象的创建——矢量化。而且虽然有了知识和经验，GIS 操作会变得越来越容易，但矢量化难以自动化（几乎总是如此）。

那么，我们的专家能做些什么呢？他们可以通过 Quick Map Services 模块下载 Google 卫星数据并手动标记数据——绘制对象本身的几何形状。在这种情况下——公园和所有看起来像公园的地方。然后可以在（例如）该属性周围放置 2000 米的缓冲区来计算其面积。然后选择位于相同区域的属性，并将其归类为绿地，然后计算其面积。比率“邻里绿地面积 / 邻里总面积”将是我们要寻找的值。

一个重要的说明是建议我们的专家住在柏林，并且对调查的区域非常熟悉。因此，从图像中手动选择的对象可以被视为地面调查与手动卫星图像绘制的某种组合。

![](../Images/50871188ad2a3988e23a70e9131f057a.png)

动画 1\. 简单的矢量化。选择地图上的对象（卫星图像），并将其表示为矢量图层（作者动画）

**优点：** 相当准确和客观的估计。计算出的比率充分表征了“邻里绿化程度”。我们的专家肯定会得到奖金。

**缺点：** 提出的方案需要大量的手动步骤。这种工作无法快速完成（我总共花了 4 小时）。此外，我们还必须重复很多相同的操作——这种工作可能会让我们的专家工作几周，并去到一个不需要他/她用这种方式计算绿色区域的公司。

**方法 2. 便宜且高效**

当然，我们在谈论的是 OpenStreetMap……与此同时，我们的英雄意识到 IT 总体上比房地产更有趣，开始学习 Python 编程语言（当然）。他/她熟悉了 [osmnx](https://osmnx.readthedocs.io/en/stable/) 模块和 [geopandas](https://geopandas.org/en/stable/) 库来处理空间数据，以及 [shapely](https://shapely.readthedocs.io/en/stable/manual.html) 库来处理几何形状。通过这三者的配合，可以对区域的绿色度进行程序化评估。需要执行以下步骤：

1.  在点（物业）周围创建一个多边形——分析区域的边界

1.  查询位于此多边形中的 OSM 数据（要查询它，需要了解 [OSM 的标签系统](https://wiki.openstreetmap.org/wiki/Tags)）

1.  计算区域多边形和根据 OSM 获得的公园及绿色区域的面积

这种方法要快得多，因为它不需要手动创建多边形。事实上，其他人已经为我们创建了这些多边形——这就是 OSM 的伟大之处。然而，OSM 也有缺点——数据可能不准确。此外，即使用户正确渲染了多边形，我们仍然有可能错误计算查询的标签集合，遗漏一些重要数据（图 2）。

![](../Images/78023f6fce264d1330d0eeffb898cb39.png)

图 2. 一个绿色区域的示例，根据 OSM 这是一个院子区域，并且没有通过与“绿色区域”相关的标签来区分（图片由作者提供）

这些区域确实不少。图 2 仅显示了一个小部分。然而，即使是这种方法也允许我们快速且大致，但仍然客观地评估区域的绿色度。

**优点：** 评估快速且简单。尽管存在不准确性，OSM 数据在大多数情况下仍然可用

**缺点：** 如果源数据不正确或查询的标签集合不够详细，则估计误差较大

**方法 3. 利用 Landsat 图像作为客观数据来源**

哦，关于 Landsat 说了和写了那么多。还有关于基于计算的植被指数 NDVI 的应用。如果你感兴趣，我们建议查看以下材料：

+   [NDVI: 标准化植被指数](https://gisgeography.com/ndvi-normalized-difference-vegetation-index/)

+   [NDVI 的优缺点](https://naxsolutions.com/en/agriculture-precision-dictionary/ndvi-pros-cons/)

+   [NDVI 应用](https://www.cropin.com/blogs/ndvi-normalized-difference-vegetation-index)

长话短说：

1.  Landsat是一个绕地球飞行并拍摄具有相当高空间分辨率的图像的卫星（从光学谱段的15–30米到远红外谱段的100米）。

1.  归一化植被指数（NDVI）是一个可以从Landsat拍摄的图像中计算出的植被指数。NDVI的值范围从-1到1，其中-1表示该像素中完全没有绿色（例如水域），而1表示该像素区域非常绿色——例如森林。

因此，我们可以获取一个Landsat图像（可以从[这里](https://earthexplorer.usgs.gov/)下载 — 搜索“Landsat 8–9 OLI/TIRS C2 L2”产品）用于所选区域，并计算NDVI。然后，通过调整阈值，我们可以将图像分为两类：0 — 非绿色区域和1 — 绿色区域。我们可以根据需要调整阈值（见图3），但老实说，我们并不确定哪个二值化阈值效果最好。此外，这个“最佳阈值”会因每个新获得的Landsat图像而异：无论是新的区域还是不同的时间日期索引。

![](../Images/bb8dc0274b2403e3d7ad1255e31c86cf.png)

图3\. 按阈值0.10、0.15、0.20对NDVI矩阵进行分割。“绿色区域”的边界用黑色显示（作者提供的图像）

**优点：** 这种方法提供了关于某个区域是否绿色的最新信息。

**缺点：** 选择最佳阈值是复杂的——不清楚选择哪个值来通过阈值将植被与非植被分开。当然，也有[来源](https://www.researchgate.net/publication/351832336_Vegetation_Change_Analysis_using_Normalized_Difference_Vegetation_Index_and_Land_Surface_Temperature_in_Greater_Gir_Landscape)，可以找到关于建议阈值的信息，例如0.35及以上。但需要注意的是，这个最佳阈值可能因图像、季节等不同而有所变化。

**方法4\. 基于Landsat的区域，基于OpenStreetMap的阈值**

对于阈值定义问题，一个合理的解决方案可能是使用OSM数据。尽管OSM数据可能有伪影或遗漏了一些现实世界中的物体，但总体上它提供了物体（公园、广场、建筑物）所在位置的完整图像。因此，我们可以将从OSM和NDVI数据获得的几何映射叠加，然后构建一个直方图（见图4）。

![](../Images/aac4576052e284e2cc92740d271d01a6.png)

图4\. 两类直方图：根据OSM数据的绿色区域NDVI值变化及图像中其他NDVI值（作者提供的图像）

图中的黑线显示了可以用作阈值的值。然后，可以通过对Landsat矩阵进行自动矢量化，使用额外的多边形来扩充OSM数据。因此，所有位于计算阈值右侧的区域将变成“绿色区域”。由于这种方法，边界得到了扩展（见图5）。

![](../Images/b635c05866f08163bcc841b84273b72e.png)

图 5\. 根据 Landsat 数据调整前后的绿色区域边界（图像由作者提供）

**而不是结论**

将会有一张图片……

![](../Images/9f61339b293ddbe581d1a0cb9736fe23.png)

图 6\. 使用不同方法提取的边界比较（图像由作者提供）

如所见，使用 Landsat 数据调整对象边界的第三种方法结果最接近基准（人工矢量化）。计算出的面积值也证实了这一点：

+   **（基准）人工矢量化：** 25%

+   使用 OSM 数据的计算：17%

+   基于改进的 OSM 和 NDVI Landsat 数据的计算：28%

如我们所见，尽管我们的估计稍微有些偏高，但仍然比根据 OSM 数据的计算结果更接近实际值。此外，我们的方法发现了在人工矢量化过程中未探索的绿色区域——北部的公墓。如果将这些区域从分析中排除，计算出的面积将更接近基准值。

附言：基于这个想法，我们留下了以下遗留物：

+   [https://github.com/wiredhut/estaty](https://github.com/wiredhut/estaty) — 处理空间数据和为房地产分析准备 MVP 算法的 Python 开源库。该库比较新，但我们计划对其进行开发和改进。

+   [http://api.greendex.wiredhut.com/](http://api.greendex.wiredhut.com/) — 用于计算“绿色”指数的服务和内部工具（Google 表格的扩展），使我们可以通过 API 和表格中的公式方便地使用上述算法（对于不懂编码的人）。该版本使用了简化的计算方法，而未使用 Landsat 卫星。

关于结合 OSM 和 Landsat 数据的故事由 [Mikhail Sarafanov](https://github.com/Dreamlone) 和 [Wiredhut team](https://wiredhut.com/) 讲述。
