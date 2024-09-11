# 使用 GEE 监测全球范围内的海洋表面温度

> 原文：[`towardsdatascience.com/monitoring-sea-surface-temperature-at-the-global-level-with-gee-1d7349c7da6?source=collection_archive---------15-----------------------#2023-03-08`](https://towardsdatascience.com/monitoring-sea-surface-temperature-at-the-global-level-with-gee-1d7349c7da6?source=collection_archive---------15-----------------------#2023-03-08)

## 如何使用 Python 创建用于海洋监测的 Streamlit 应用

[](https://bryanvallejo16.medium.com/?source=post_page-----1d7349c7da6--------------------------------)![Bryan R. Vallejo](https://bryanvallejo16.medium.com/?source=post_page-----1d7349c7da6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d7349c7da6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d7349c7da6--------------------------------) [Bryan R. Vallejo](https://bryanvallejo16.medium.com/?source=post_page-----1d7349c7da6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcbd681aaa725&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-sea-surface-temperature-at-the-global-level-with-gee-1d7349c7da6&user=Bryan+R.+Vallejo&userId=cbd681aaa725&source=post_page-cbd681aaa725----1d7349c7da6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d7349c7da6--------------------------------) ·7 分钟阅读·2023 年 3 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d7349c7da6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-sea-surface-temperature-at-the-global-level-with-gee-1d7349c7da6&user=Bryan+R.+Vallejo&userId=cbd681aaa725&source=-----1d7349c7da6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d7349c7da6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-sea-surface-temperature-at-the-global-level-with-gee-1d7349c7da6&source=-----1d7349c7da6---------------------bookmark_footer-----------)![](img/fd89182793e896002d4011c0213a962d.png)

图片由作者提供。 [SST 监测仪](https://ocean-temperature-monitoring-bryanrvallejo.streamlit.app/) 2023 年 1 月 14 日，使用 GEE

# 介绍

地球观测及其技术基础设施在过去十年中显著增加。许多卫星星座提供了开放且方便的数据访问，研究人员可以轻松获取这些数据。例如，***Google Earth Engine*** 是一个云基础设施，提供了对许多提供商的数据访问，如 Modis、NOAA、ASTER、Lansat 等，你可以直接进行探索和分析。可以在[GEE](https://developers.google.com/earth-engine/datasets)查看。如果你想在 Python 中使用 API，可以使用由[Qiusheng Wu](https://medium.com/u/ddf2451fc08e?source=post_page-----1d7349c7da6--------------------------------)开发的[geemap](https://geemap.org/) Python 库，该库具有出色的功能，并可以在本教程中直接使用。

基础设施模型以接近实时的速度运行，因此可以获得最新的数据，这有助于监测陆地和海洋。对于本教程，我们将使用[HYCOM](https://developers.google.com/earth-engine/datasets/catalog/HYCOM_sea_temp_salinity#description)数据集，该数据集包含一个数据同化混合模型，显示全球范围的海表温度和盐度。该模型包含每个时间层的深度值，用户可以在特定深度和日期可视化海洋。混合坐标海洋模型已经成为许多出版物的一部分，网站上包含了…
