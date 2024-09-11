# Python 中的地理空间数据分析

> 原文：[`towardsdatascience.com/geospatial-data-analysis-in-python-d8fa8dd23a6c?source=collection_archive---------4-----------------------#2023-05-03`](https://towardsdatascience.com/geospatial-data-analysis-in-python-d8fa8dd23a6c?source=collection_archive---------4-----------------------#2023-05-03)

## 开始使用 OSMnx 和 Kepler.gl 在 Python 中进行地理数据分析

[](https://pierpaoloippolito28.medium.com/?source=post_page-----d8fa8dd23a6c--------------------------------)![Pier Paolo Ippolito](https://pierpaoloippolito28.medium.com/?source=post_page-----d8fa8dd23a6c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d8fa8dd23a6c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8fa8dd23a6c--------------------------------) [Pier Paolo Ippolito](https://pierpaoloippolito28.medium.com/?source=post_page-----d8fa8dd23a6c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8391a6a5f1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-in-python-d8fa8dd23a6c&user=Pier+Paolo+Ippolito&userId=b8391a6a5f1a&source=post_page-b8391a6a5f1a----d8fa8dd23a6c---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8fa8dd23a6c--------------------------------) ·6 分钟阅读·2023 年 5 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd8fa8dd23a6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-in-python-d8fa8dd23a6c&user=Pier+Paolo+Ippolito&userId=b8391a6a5f1a&source=-----d8fa8dd23a6c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd8fa8dd23a6c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgeospatial-data-analysis-in-python-d8fa8dd23a6c&source=-----d8fa8dd23a6c---------------------bookmark_footer-----------)![](img/be6e1af4ad9aced3d0d8e7b54d7033c3.png)

照片由[Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

地理空间数据无处不在，并在所有业务中用于多种不同的应用（例如，根据位置计算物业风险、设计新的建筑开发、规划货物运输以及寻找不同地点之间的可能路线）。

地理空间数据通常有两种存储格式：**栅格**和**矢量**：

+   栅格以像素矩阵的形式表示数据（因此具有固定分辨率）。在这种表示方式中，每个像素可以被赋予不同的值，多层叠加的网格可以用于进一步增强同一图像。例如，同一图像可以使用 3 个通道/波段（例如 RGB——红色、绿色、蓝色）或仅使用单个通道进行存储。

+   矢量可以用于抽象现实世界的几何形状，例如点、线、面等……通常，它们会与一些有关所表示对象的有用元数据（例如名称、地址、所有者等）一起存储。由于它们作为数学对象存储，可以在不影响分辨率的情况下对矢量进行放大。
