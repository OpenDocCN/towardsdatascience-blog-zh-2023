# 医疗数据集的图像配准

> 原文：[https://towardsdatascience.com/image-registration-for-medical-datasets-ee605ff8eb2e?source=collection_archive---------3-----------------------#2023-02-22](https://towardsdatascience.com/image-registration-for-medical-datasets-ee605ff8eb2e?source=collection_archive---------3-----------------------#2023-02-22)

## 从 SimpleElastix 到空间变换网络

[](https://charlieoneill.medium.com/?source=post_page-----ee605ff8eb2e--------------------------------)[![Charlie O'Neill](../Images/17aa117fc5787f93ff1f547b919786c8.png)](https://charlieoneill.medium.com/?source=post_page-----ee605ff8eb2e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee605ff8eb2e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ee605ff8eb2e--------------------------------) [Charlie O'Neill](https://charlieoneill.medium.com/?source=post_page-----ee605ff8eb2e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2742adff9d49&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-registration-for-medical-datasets-ee605ff8eb2e&user=Charlie+O%27Neill&userId=2742adff9d49&source=post_page-2742adff9d49----ee605ff8eb2e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee605ff8eb2e--------------------------------) ·31分钟阅读·2023年2月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee605ff8eb2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-registration-for-medical-datasets-ee605ff8eb2e&user=Charlie+O%27Neill&userId=2742adff9d49&source=-----ee605ff8eb2e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee605ff8eb2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimage-registration-for-medical-datasets-ee605ff8eb2e&source=-----ee605ff8eb2e---------------------bookmark_footer-----------)![](../Images/ec059b4593a0bbe23845d204100454c1.png)

图片由 [Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

图像配准是图像处理中的一个基本任务，涉及将两个或多个图像对齐到一个公共坐标系统。通过这样做，图像中的对应像素代表现实世界中的同源点，从而使图像的比较和分析成为可能。图像配准的一个常见应用是在医学成像中，其中对同一患者进行多次扫描或拍摄，这些图像由于时间、位置或其他因素的变化而有所不同。对这些图像进行配准可以揭示细微的变化或模式，这些变化或模式可能指示疾病进展或治疗效果。

图像配准涉及找到一个空间变换，将一幅图像中的点映射到另一幅图像中的相应点，以便将这些图像重叠在一起。空间变换通常由一组控制点参数化，这些控制点用于将一幅图像扭曲以匹配另一幅图像。配准的质量通过相似性度量来衡量，该度量量化了图像之间的对应程度。

近年来，由于先进成像技术的可用性，医学图像配准引起了越来越多的关注…
