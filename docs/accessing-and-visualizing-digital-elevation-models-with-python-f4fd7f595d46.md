# 使用 Python 访问和可视化数字高程模型

> 原文：[`towardsdatascience.com/accessing-and-visualizing-digital-elevation-models-with-python-f4fd7f595d46`](https://towardsdatascience.com/accessing-and-visualizing-digital-elevation-models-with-python-f4fd7f595d46)

## 一个使用公开可用的 DEM 数据的 Python 教程

[](https://parvathykrishnank.medium.com/?source=post_page-----f4fd7f595d46--------------------------------)![Parvathy Krishnan](https://parvathykrishnank.medium.com/?source=post_page-----f4fd7f595d46--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f4fd7f595d46--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f4fd7f595d46--------------------------------) [Parvathy Krishnan](https://parvathykrishnank.medium.com/?source=post_page-----f4fd7f595d46--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f4fd7f595d46--------------------------------) ·阅读时间 7 分钟·2023 年 3 月 5 日

--

![](img/6936cf7714b6a92e007098b93bcefd97.png)

图片由 [Maria Teneva](https://unsplash.com/@miteneva?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

*这项工作由* [*Mahdi Fayazbakhsh*](https://medium.com/u/3138ef9e59d5) *和* [*Kai Kaiser*](https://medium.com/u/ea66398a2a31)* 合著。所有错误和遗漏均由作者负责。*

**数字高程模型 (DEMs)** 代表地形的 3D 表面模型。它通过一系列单元格表示连续的地形高程表面，每个单元格表示其位置 (X 和 Y) 上特征的高程 (Z)。数字高程模型只包含地质特征的高程信息，如山谷、山脉和滑坡，不包括植物或建筑物等特征的高程数据。

精确的高分辨率 DEM 数据对于灾害制图非常重要，因为它提供了详细的地形表示，这对于评估自然灾害可能带来的风险至关重要。通过允许科学家测量温度、降水量以及其他气候相关因素对不同海拔土地表面的影响，这些数据可以更好地告知气候变化将如何影响各种土地表面。DEM 数据还可用于识别面临洪水、滑坡和其他极端天气事件风险的区域，这有助于制定减轻气候变化影响的政策决策。此外，DEM 数据有助于创建详细的地图，这些地图可以用于制定疏散计划、警报系统和其他风险管理策略。

美国地质调查局（USGS）[EarthExplorer](https://earthexplorer.usgs.gov/)是全球数字高程模型（DEM）数据集最受欢迎的来源之一。USGS 提供从 10 米到 1 弧秒的分辨率数据集，更新间隔为 5 到 10 年。其他全球 DEM 数据集的来源包括[哥白尼陆地监测服务](https://www.eea.europa.eu/data-and-maps/data/copernicus-land-monitoring-service-eu-dem)、国际热带农业中心和全球土地覆盖设施。

> 本博客总结了如何使用 Python 下载和可视化任何感兴趣的地理区域的开放全球数字高程模型。

首先，我们将从人道数据交换（[*HDX*](https://data.humdata.org/dataset/cod-ab-arm)）下载我们选择的区域的 shapefile。然后，我们将使用[*elevation*](https://pypi.org/project/elevation/)包，这个包提供了对由 NASA 和 NGA 托管在 Amazon S3 上的全球数据集 SRTM 30m Global 1 arc second V003 以及由 CGIAR-CSI 编制的 SRTM 90m Digital Elevation Database v4.1 的简单下载、缓存和访问。

该软件包将允许我们将所选地理边界的 DEM 下载到本地系统。然后，我们可以使用如[*GDAL*](https://gdal.org/)或[*rasterio*](https://rasterio.readthedocs.io/en/stable/)等库将此 DEM 导入 Python。接下来，我们将使用[*matplotlib*](https://matplotlib.org/)探索和可视化 DEM，并计算诸如坡度和高程等导数。

## 从 OCHA 通过 HDX 提取任何国家行政区域的多边形

[OCHA 现场信息服务部门（FISS）](https://www.unocha.org/our-work/information-management)是位于瑞士日内瓦的联合国人道事务协调办公室的一部分。该组织提供[共同操作数据集](https://storymaps.arcgis.com/stories/dcf6135fc0e943a9b77823bb069e2578)，这些数据集是支持人道紧急情况初步响应中的操作和决策的权威参考数据集。其中一个数据集是通过[人道数据交换平台](https://data.humdata.org/organization/b3a25ac4-ac05-4991-923c-d25f47bef1ec)（HDX）提供的国家的次国家行政边界数据，使用的是[创意共享归属政府间组织](https://data.humdata.org/faqs/licenses)许可证。

以下代码可视化了来自[亚美尼亚](https://data.humdata.org/dataset/cod-ab-arm)（作为本教程示例）的行政边界，这些数据是通过[*folium*](https://python-visualization.github.io/folium/)从 HDX 下载的。

![](img/36e078423b6dd5c3c13b354a44b1316e.png)

***从 GADM 通过 GADMDownloader 提取的国家行政边界（图片由作者提供）***

要下载 DEM，我们选择一个省份 Gegharkunik。

![](img/f1786873218d6255b9f2dadb854c6474.png)

**提取的选定 ADM 边界多边形（作者提供的图像）**

## 使用*elevation*包将 DEM 下载到本地路径

该包允许下载两个数据集，这些数据集可以通过属性***product***（SRTM1 或 SRTM3）进行指定。

+   [**SRTM1**](https://lpdaac.usgs.gov/products/srtmgl1nv003/)（30m 分辨率）— NASA JPL（2013）。*NASA 航天飞机雷达地形任务全球 1 弧秒数据集*[数据集]。NASA EOSDIS 土地过程 DAAC。2023 年 3 月 6 日访问，网址：[`doi.org/10.5067/MEaSUREs/SRTM/SRTMGL1N.003`](https://doi.org/10.5067/MEaSUREs/SRTM/SRTMGL1N.003)

+   [**SRTM3**](https://bigdata.cgiar.org/srtm-90m-digital-elevation-database/)（90m 分辨率）— Jarvis A., H.I. Reuter, A. Nelson, E. Guevara, 2008, 填洞的无缝 SRTM 数据 V4，国际热带农业中心（CIAT），可从[`srtm.csi.cgiar.org`](http://srtm.csi.cgiar.org/)获取。

在本教程中，我们使用 SRTM1 数据，关于该数据的引用和使用政策可以在[土地过程分布活动档案中心](https://lpdaac.usgs.gov/data/data-citation-and-policies/)找到。

请注意，我们需要提供*绝对文件路径*来下载 DEM 作为 TIF 文件；否则将导致错误。

![](img/3ae4643796a2e2f7df8eeb6c151e41ba.png)

**使用*elevation*包在 Python 中提取的 DEM，并裁剪为感兴趣的多边形（作者提供的图像）**

## 使用 matplotlib 和 RichDEM 可视化 DEM 地形属性

在本节中，我们将使用[*matplotlib*](https://matplotlib.org/)和[*richdem*](https://pypi.org/project/richdem/)可视化数字高程模型的一些地形属性。

**DEM 中的等高线**是连接相同海拔点的线条。这些等高线有助于展示地形的三维形状，并可用于测量距离、计算流域和确定坡度。matplotlib 提供了将等高线绘制为*线条*或*填充区域*的选项，如下代码所示。

![](img/6db5271f246644245ae5748828731edb.png)

**使用 matplotlib 可视化等高线（作者提供的图像）**

DEM 的直方图以条形图形式表示，每个条形代表特定海拔值的频率。每个条形的高度对应于数据集中该特定海拔值出现的次数。DEM 直方图可用于识别海拔极值、检测海拔异常值或确定数据集中的总体海拔范围。

![](img/62ac957cb2297cec1f7f21a7f5414c54.png)

**DEM 海拔值的分布（作者提供的图像）**

坡度是一个地形属性或测量值，用于描述给定区域内海拔变化的速率。它是表面陡峭度或倾斜度的度量，通常以百分比或角度表示。

DEM 的坡度通常以度数表示，范围从 0°（平坦）到 90°（垂直）。亚美尼亚通常是一个山区国家，大部分地形都有陡峭的坡度。这个国家确实有一些平坦或缓坡的地区，主要位于阿拉斯河和阿拉克斯河的山谷中。

![](img/1fc722c9d5193b93023299327fa07ee1.png)

**地形属性坡度（图片由作者提供）**

当坡度以百分比表示时，它是通过给定距离（run）上的升高（rise）比率计算得出的，称为***坡度升降比。** 14% 的坡度升降比意味着每 100 个水平单位，坡度垂直升高 14 个单位。例如，亚美尼亚的高坡度升降比位于高加索山脉，靠近与格鲁吉亚和阿塞拜疆的边界。亚美尼亚的高加索山脉是欧亚山脉系统的一部分，位于该国南部。该山脉的平均海拔约为 4,500 米（14,764 英尺）。亚美尼亚高加索山脉的平均坡度为 15%。

![](img/5d2ef31041beaa8c3685dc2e75784ff5.png)

**地形属性坡度升降百分比（图片由作者提供）**

角度（Aspect）以度数为单位，是地球表面给定位置坡度方向的数值测量。它的测量角度从 0 到 360 度，0° 代表北方，90° 代表东方，180° 代表南方，270° 代表西方。

![](img/60adc6dc26e90fc4597c6df2fb1e6715.png)

**地形属性角度（图片由作者提供）**

# 结论

可视化和分析数字高程模型对许多气候变化、灾害风险减缓和基础设施规划的应用场景至关重要，因为它允许分析地形特征，这对于理解气候变化的动态、评估自然灾害风险以及设计有效的基础设施项目是必不可少的。DEM 数据也构成了生成数字地形模型的基础，这些模型可以用于绘制易受洪水、滑坡和其他极端天气事件影响的区域。此外，DEM 数据还可以用于创建详细的 3D 景观模型，以模拟潜在的基础设施项目，如道路和桥梁，并评估其环境影响。

我们希望你现在对如何使用 Python 下载、探索和可视化数字高程模型（DEM）及其属性有了更好的理解。有了这些知识，你可以使用 Python 创建详细的地形图，并分析不同区域的高程。此外，你可以使用 Python 比较不同的 DEM 及其属性，并洞察地形随时间的变化。有了这些工具，你将能够深入探索和分析景观。

> 让我们通过查看阿拉加茨省的 DEM 可视化图来结束这篇博客。阿拉加茨省是亚美尼亚的一个省份，拥有该国最高的山峰——阿拉加茨山。阿拉加茨山是一座已 extinct 火山山脉，包含四个独立的山峰，每个山峰都可以攀登，因此是登山者和攀岩爱好者的梦想目的地。

![](img/bc952d71b282207ca718906a68a10cd1.png)

**可视化阿拉加茨省的 DEM 高程值的坡度、等高线和分布（图片来源：作者）**
