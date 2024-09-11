# 如何使用 Plotly 创建地图绘图

> 原文：[https://towardsdatascience.com/how-to-create-map-plots-with-plotly-26111d38fff9?source=collection_archive---------4-----------------------#2023-09-10](https://towardsdatascience.com/how-to-create-map-plots-with-plotly-26111d38fff9?source=collection_archive---------4-----------------------#2023-09-10)

## 来自显著火山喷发数据库的5个示例

[](https://medium.com/@caroline.arnold_63207?source=post_page-----26111d38fff9--------------------------------)[![Caroline Arnold](../Images/2560e106ba9deda7889c7d253792d814.png)](https://medium.com/@caroline.arnold_63207?source=post_page-----26111d38fff9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26111d38fff9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----26111d38fff9--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----26111d38fff9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9367198e7a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-map-plots-with-plotly-26111d38fff9&user=Caroline+Arnold&userId=9367198e7a3c&source=post_page-9367198e7a3c----26111d38fff9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26111d38fff9--------------------------------) · 5分钟阅读 · 2023年9月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F26111d38fff9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-map-plots-with-plotly-26111d38fff9&user=Caroline+Arnold&userId=9367198e7a3c&source=-----26111d38fff9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F26111d38fff9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-map-plots-with-plotly-26111d38fff9&source=-----26111d38fff9---------------------bookmark_footer-----------)![](../Images/460866e5b3e61c4ac92a13b8c511d114.png)

照片由 [Willian Justen de Vasconcellos](https://unsplash.com/@willianjusten?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[Plotly](https://plotly.com/) 是一个出色的开源库，用于数据可视化。在这篇博客文章中，我将向你展示如何使用 Plotly 生成制图绘图，使用 Python 后端。

为了说明的目的，我将使用由美国国家环境信息中心发布的显著火山喷发数据库，该数据库在[美国政府工作许可证](https://www.usa.gov/government-works)下发布。数据集可以在此处下载：[https://public.opendatasoft.com/explore/dataset/significant-volcanic-eruption-database/information/](https://public.opendatasoft.com/explore/dataset/significant-volcanic-eruption-database/information/)

你将看到以下五个可视化：

1.  显著火山喷发的全球分布

1.  北美的火山类型

1.  与海啸相关的火山喷发

1.  最具破坏性的火山喷发

1.  有趣的地图投影

对于有兴趣使用plotly进行数据分析的读者，请参阅我最近关于可视化女子世界杯数据的帖子：
