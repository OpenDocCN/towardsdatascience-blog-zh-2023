# 探索大规模光栅人口数据

> 原文：[`towardsdatascience.com/exploring-large-scale-raster-population-data-72803cf7f2ad?source=collection_archive---------6-----------------------#2023-09-21`](https://towardsdatascience.com/exploring-large-scale-raster-population-data-72803cf7f2ad?source=collection_archive---------6-----------------------#2023-09-21)

![](img/447d132fd00738a285dfd225d655e478.png)

图片来源：作者。

## 使用 Python 可视化跨多个尺度的地理空间人口数据：全球、国家和城市级数据

[](https://medium.com/@janosovm?source=post_page-----72803cf7f2ad--------------------------------)![米兰·贾诺索夫](https://medium.com/@janosovm?source=post_page-----72803cf7f2ad--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72803cf7f2ad--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----72803cf7f2ad--------------------------------) [米兰·贾诺索夫](https://medium.com/@janosovm?source=post_page-----72803cf7f2ad--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F838408aa2ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-large-scale-raster-population-data-72803cf7f2ad&user=Milan+Janosov&userId=838408aa2ad4&source=post_page-838408aa2ad4----72803cf7f2ad---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72803cf7f2ad--------------------------------) · 9 分钟阅读 · 2023 年 9 月 21 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72803cf7f2ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-large-scale-raster-population-data-72803cf7f2ad&user=Milan+Janosov&userId=838408aa2ad4&source=-----72803cf7f2ad---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72803cf7f2ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-large-scale-raster-population-data-72803cf7f2ad&source=-----72803cf7f2ad---------------------bookmark_footer-----------)

我经常看到漂亮的人口地图在网上流传；然而，我通常会在一些技术细节上遇到困难，比如可视化教程中未显示的其他地图部分，或将大规模光栅数据转换为更适合计算的矢量格式。在这篇文章中，我通过实际操作指南克服了这些不足，介绍了两个主要的全球人口数据源。

还需要注意的是，除了它们的美学价值外，人口数据和显示这些数据的地图是任何城市发展或位置情报任务中最基本和最有价值的信息之一。它们在规划新设施、选址和区划分析、估算城市产品规模或描述不同社区等用例中特别有用，仅举几例。

# 1\. 数据来源

我依赖以下两个详细的人口估算数据来源，你可以通过附上的链接下载文件（在发布日期时）：

+   [欧洲委员会的 GHSL —](https://ghsl.jrc.ec.europa.eu/ghs_pop2019.php)…
