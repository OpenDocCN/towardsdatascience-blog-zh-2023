# 单目深度估计用于预测火星表面起伏

> 原文：[https://towardsdatascience.com/monocular-depth-estimation-to-predict-surface-reliefs-of-mars-1b50aed3361a?source=collection_archive---------11-----------------------#2023-09-04](https://towardsdatascience.com/monocular-depth-estimation-to-predict-surface-reliefs-of-mars-1b50aed3361a?source=collection_archive---------11-----------------------#2023-09-04)

## 单目深度估计模型的另一种应用

[](https://mattiagatti.medium.com/?source=post_page-----1b50aed3361a--------------------------------)[![Mattia Gatti](../Images/9d5aeb356ff01ed9e4ead66c18994595.png)](https://mattiagatti.medium.com/?source=post_page-----1b50aed3361a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1b50aed3361a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1b50aed3361a--------------------------------) [Mattia Gatti](https://mattiagatti.medium.com/?source=post_page-----1b50aed3361a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F19bc376db93c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonocular-depth-estimation-to-predict-surface-reliefs-of-mars-1b50aed3361a&user=Mattia+Gatti&userId=19bc376db93c&source=post_page-19bc376db93c----1b50aed3361a---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b50aed3361a--------------------------------) · 5分钟阅读 · 2023年9月4日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1b50aed3361a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonocular-depth-estimation-to-predict-surface-reliefs-of-mars-1b50aed3361a&user=Mattia+Gatti&userId=19bc376db93c&source=-----1b50aed3361a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1b50aed3361a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonocular-depth-estimation-to-predict-surface-reliefs-of-mars-1b50aed3361a&source=-----1b50aed3361a---------------------bookmark_footer-----------)![](../Images/d2e0288d571818e893578b3268a36a16.png)

图片来源于[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral)，刊登在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

文献中讨论了几种从单张图像估算地表高程的方法。在之前的[文章](/generate-a-3d-mesh-from-an-image-with-python-12210c73e5cc)中，我讨论了如何使用单目估计模型预测单张2D图像的深度。然而，当模型的输入是特定地表的图像时，预测结果代表一个数字高程模型（DEM）。在我的第一篇研究论文中，我展示了如何使用深度学习方法从2D灰度图像获取火星表面的DEM。为了更好地理解我要提出的概念，我建议你首先尝试[这里](https://huggingface.co/spaces/mattiagatti/mars_dtm_estimation)的项目演示。

## 介绍

在另一个[故事](/the-ultimate-beginners-guide-to-geospatial-raster-data-feb7673f6db0)中详细讨论了，地表的数字高程模型（DEM）是一个高程值的网格，其中每个单元格存储地表上某一点的高程：

![](../Images/41059f8b3822f57ceff36a4448ebd9af.png)

DEM的图形可视化。[NSIDC](https://commons.wikimedia.org/wiki/File:Digital_elevation_model_(DEM)_of_the_Mt._Everest_region_-_50090548573.png)，[CC BY 2.0](https://creativecommons.org/licenses/by/2.0)，通过维基共享资源

DEM通常通过颜色地图以图形方式表示。在上图中，最高点为红色，最低点为紫色。
