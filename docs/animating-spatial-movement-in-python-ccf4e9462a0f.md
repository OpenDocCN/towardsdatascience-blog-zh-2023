# Python 中的空间移动动画

> 原文：[`towardsdatascience.com/animating-spatial-movement-in-python-ccf4e9462a0f?source=collection_archive---------9-----------------------#2023-11-23`](https://towardsdatascience.com/animating-spatial-movement-in-python-ccf4e9462a0f?source=collection_archive---------9-----------------------#2023-11-23)

## 如何将起点-终点矩阵转化为迷人的动画

[](https://medium.com/@haavardwallinaagesen?source=post_page-----ccf4e9462a0f--------------------------------)![Håvard Wallin Aagesen](https://medium.com/@haavardwallinaagesen?source=post_page-----ccf4e9462a0f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ccf4e9462a0f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ccf4e9462a0f--------------------------------) [Håvard Wallin Aagesen](https://medium.com/@haavardwallinaagesen?source=post_page-----ccf4e9462a0f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe803da03596c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimating-spatial-movement-in-python-ccf4e9462a0f&user=H%C3%A5vard+Wallin+Aagesen&userId=e803da03596c&source=post_page-e803da03596c----ccf4e9462a0f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ccf4e9462a0f--------------------------------) ·6 分钟阅读·2023 年 11 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fccf4e9462a0f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimating-spatial-movement-in-python-ccf4e9462a0f&user=H%C3%A5vard+Wallin+Aagesen&userId=e803da03596c&source=-----ccf4e9462a0f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fccf4e9462a0f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanimating-spatial-movement-in-python-ccf4e9462a0f&source=-----ccf4e9462a0f---------------------bookmark_footer-----------)![](img/61608028da72395e20f21dca85cb6f13.png)

来自自行车共享数据的静态移动地图。图像由作者提供。

空间数据本质上是可视化的，Python 中在（地理）空间数据可视化方面的进展使得快速绘制各种形状和形式的地图变得非常简单。即使是创建图表和简单地图的动画也是相当容易的。特别是色斑图，具有静态多边形和变化的颜色，有现成的函数可以完成。

但是，当涉及到移动数据和动画化线条时，任务就变得有些繁琐。在这里，我将尝试举例说明如何在 Python 中解决空间移动数据的动画化问题。

# 初始数据

要开始，我们需要一些带有时间戳的（行）数据；在这个例子中，我将使用来自挪威奥斯陆市自行车共享系统的自行车共享数据。这些数据在[挪威政府开放数据许可（NLOD）2.0](https://data.norge.no/nlod/no/2.0#_lisensavtalens_innledning)/开放政府许可下公开可用，可从[奥斯陆市自行车](https://oslobysykkel.no/en/open-data)的主页获取。

```py
import geopandas as gpd
import pandas as pd

# Import data from csv
data = pd.read_csv("https://data.urbansharing.com/oslobysykkel.no/trips/v1/2023/10.csv")
data = data[['started_at','ended_at','duration', 'start_station_latitude', 'start_station_longitude','end_station_latitude', 'end_station_longitude']]
#…
```
