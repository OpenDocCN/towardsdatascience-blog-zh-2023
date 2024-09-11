# 什么是卫星图像时间序列？

> 原文：[https://towardsdatascience.com/what-is-a-satellite-image-time-series-c0516c534ba9?source=collection_archive---------5-----------------------#2023-03-02](https://towardsdatascience.com/what-is-a-satellite-image-time-series-c0516c534ba9?source=collection_archive---------5-----------------------#2023-03-02)

## 当前和未来全球挑战的基础

[](https://mattiagatti.medium.com/?source=post_page-----c0516c534ba9--------------------------------)[![Mattia Gatti](../Images/9d5aeb356ff01ed9e4ead66c18994595.png)](https://mattiagatti.medium.com/?source=post_page-----c0516c534ba9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c0516c534ba9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c0516c534ba9--------------------------------) [Mattia Gatti](https://mattiagatti.medium.com/?source=post_page-----c0516c534ba9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F19bc376db93c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-satellite-image-time-series-c0516c534ba9&user=Mattia+Gatti&userId=19bc376db93c&source=post_page-19bc376db93c----c0516c534ba9---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c0516c534ba9--------------------------------) ·7分钟阅读·2023年3月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc0516c534ba9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-satellite-image-time-series-c0516c534ba9&user=Mattia+Gatti&userId=19bc376db93c&source=-----c0516c534ba9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc0516c534ba9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-satellite-image-time-series-c0516c534ba9&source=-----c0516c534ba9---------------------bookmark_footer-----------)![](../Images/65a2ad782a2caf240cc0382ef93bce99.png)

图片由[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在之前的[文章](/the-ultimate-beginners-guide-to-geospatial-raster-data-feb7673f6db0)中，我详细讨论了关于地理空间栅格数据的所有内容。这种数据与特定的地理区域相关。然而，仅通过分析单一的栅格文件，无法研究该区域随时间的变化。此指南旨在介绍卫星图像时间序列，并解释为什么这些数据在应对地球面临的当前和未来挑战中极为重要。

# 介绍

卫星图像时间序列（SITS）可能是研究特定地区随时间变化的最重要资源：

> SITS是一组在不同时间拍摄于同一地区的卫星图像。

每张图片都作为地理栅格文件存储。这些图片也按日期排序，以便可以顺序分析。这里展示的是一个作物区域的SITS：

![](../Images/c73f1f6edfb9af73207c2a9e0273afe0.png)

这是来自意大利伦巴第地区的作物区域的卫星图像时间序列。图像来自[Copernicus开放访问中心](https://scihub.copernicus.eu/)。
