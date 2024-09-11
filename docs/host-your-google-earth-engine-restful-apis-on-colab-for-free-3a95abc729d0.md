# 在 Colab 上免费托管您的 Google Earth Engine RESTful APIs

> 原文：[https://towardsdatascience.com/host-your-google-earth-engine-restful-apis-on-colab-for-free-3a95abc729d0?source=collection_archive---------18-----------------------#2023-01-06](https://towardsdatascience.com/host-your-google-earth-engine-restful-apis-on-colab-for-free-3a95abc729d0?source=collection_archive---------18-----------------------#2023-01-06)

## 使用 FastAPI 和 ngrok

[](https://dgg32.medium.com/?source=post_page-----3a95abc729d0--------------------------------)[![黄思行](../Images/48f445636ed1e32a53b610f6afc93ef2.png)](https://dgg32.medium.com/?source=post_page-----3a95abc729d0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3a95abc729d0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3a95abc729d0--------------------------------) [黄思行](https://dgg32.medium.com/?source=post_page-----3a95abc729d0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fff9d63e09a67&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhost-your-google-earth-engine-restful-apis-on-colab-for-free-3a95abc729d0&user=Sixing+Huang&userId=ff9d63e09a67&source=post_page-ff9d63e09a67----3a95abc729d0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3a95abc729d0--------------------------------) · 8 分钟阅读 · 2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3a95abc729d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhost-your-google-earth-engine-restful-apis-on-colab-for-free-3a95abc729d0&user=Sixing+Huang&userId=ff9d63e09a67&source=-----3a95abc729d0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3a95abc729d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhost-your-google-earth-engine-restful-apis-on-colab-for-free-3a95abc729d0&source=-----3a95abc729d0---------------------bookmark_footer-----------)![](../Images/5ed2171b29624898c3a85015cd02aad1.png)

图片由 [NASA](https://unsplash.com/@nasa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/images/nature/earth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

地理空间数据需求很高。它揭示了我们星球随时间变化的情况。当我们谈论地理空间时，我们会想到 [Google Earth Engine](https://earthengine.google.com/)（GEE）。这个服务有几个优势。它托管了许多超过 37 年的大型数据集合。所有计算都在 Google 强大的云基础设施上运行。此外，它对非营利项目免费。通过 GEE，我们可以免费研究 [土地利用和覆盖](https://medium.com/geekculture/monitor-land-use-changes-with-google-earth-engine-65cd15e10c6c)（LULC）、[植被](https://medium.com/p/909a2ad51a48)、本地气候（[这里](https://medium.com/p/c6aa77fecdb1) 和 [这里](https://medium.com/p/ae21261854d6)），甚至 [美国的作物生产](https://medium.com/p/9cfd14813e99)。

然而，GEE 确实有较高的门槛。首先，必须熟练掌握 JavaScript 或 Python。其次，我们需要了解许多地理空间概念，如图像集合、几何体和卫星波段。第三，其异步请求-响应模式对于新手来说需要适应。

这给许多数据科学家带来了一个小挑战。他们往往只是想快速获取一组坐标的某些值，例如土壤 pH 值或平均地表温度。截至目前，他们需要进行相当多的编码，因为 GEE 没有提供 RESTful API。如果我们能自己填补这个空白就好了（Video 1）？我们的 API 应该封装一些常见的 GEE 计算并提供 HTTP…
