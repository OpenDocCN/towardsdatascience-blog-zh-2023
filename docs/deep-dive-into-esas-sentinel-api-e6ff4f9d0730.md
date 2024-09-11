# 深入了解 ESA 的 Sentinel API

> 原文：[https://towardsdatascience.com/deep-dive-into-esas-sentinel-api-e6ff4f9d0730?source=collection_archive---------9-----------------------#2023-10-26](https://towardsdatascience.com/deep-dive-into-esas-sentinel-api-e6ff4f9d0730?source=collection_archive---------9-----------------------#2023-10-26)

![](../Images/11263c751d93403401ed501eb089fe4b.png)

基于 10 米分辨率的 Sentinel 数据，布达佩斯的 RGB 卫星地图片段。

## 如何使用 Python 获取、分析和可视化卫星图像

[](https://medium.com/@janosovm?source=post_page-----e6ff4f9d0730--------------------------------)[![米兰·贾诺索夫](../Images/77b62460041f66ec4585a81baef81a03.png)](https://medium.com/@janosovm?source=post_page-----e6ff4f9d0730--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e6ff4f9d0730--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e6ff4f9d0730--------------------------------) [米兰·贾诺索夫](https://medium.com/@janosovm?source=post_page-----e6ff4f9d0730--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F838408aa2ad4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-esas-sentinel-api-e6ff4f9d0730&user=Milan+Janosov&userId=838408aa2ad4&source=post_page-838408aa2ad4----e6ff4f9d0730---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e6ff4f9d0730--------------------------------) · 13 分钟阅读 · 2023 年 10 月 26 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe6ff4f9d0730&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-esas-sentinel-api-e6ff4f9d0730&user=Milan+Janosov&userId=838408aa2ad4&source=-----e6ff4f9d0730---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe6ff4f9d0730&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-esas-sentinel-api-e6ff4f9d0730&source=-----e6ff4f9d0730---------------------bookmark_footer-----------)

*本文中的所有图像均由作者创建。*

欧洲航天局一直在运行其[哨兵任务](https://www.esa.int/Applications/Observing_the_Earth/Copernicus/The_Sentinel_missions#:~:text=These%20missions%20carry%20a%20range,for%20land%20and%20ocean%20services.)，支持欧盟的地球观测组件科幻计划，提供各种类型的遥感数据，如用于陆地、海洋和大气监测的雷达和多光谱成像仪。

目前有六个正在进行的 Sentinel 任务，其中三个可以通过其 [Python API](https://sentinelsat.readthedocs.io/en/latest/api_overview.html) 轻松访问。这些任务来自[官方来源](https://www.esa.int/Applications/Observing_the_Earth/Copernicus/The_Sentinel_missions#:~:text=These%20missions%20carry%20a%20range,for%20land%20and%20ocean%20services.):

> **Sentinel-1** 是一个极轨、全天候、昼夜雷达成像任务，用于陆地和海洋服务。Sentinel-1A 于 2014 年 4 月 3 日发射，Sentinel-1B 于 2016 年 4 月 25 日发射。两者都通过欧洲法属圭亚那的航天发射场的联盟火箭送入轨道。Sentinel-1B 的任务在 2022 年结束，目前计划尽快发射 Sentinel-1C。
> 
> **Sentinel-2** 是一个极轨、多光谱高分辨率成像任务，用于陆地监测，例如提供植被、土壤和水体覆盖、内陆水道和沿海地区的影像。Sentinel-2 也可以提供紧急服务信息。Sentinel-2A 于 2015 年 6 月 23 日发射……
