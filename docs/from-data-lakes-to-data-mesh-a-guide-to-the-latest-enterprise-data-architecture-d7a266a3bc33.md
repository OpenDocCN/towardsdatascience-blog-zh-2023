# 从数据湖到数据网格：最新企业数据架构指南

> 原文：[https://towardsdatascience.com/from-data-lakes-to-data-mesh-a-guide-to-the-latest-enterprise-data-architecture-d7a266a3bc33?source=collection_archive---------2-----------------------#2023-05-30](https://towardsdatascience.com/from-data-lakes-to-data-mesh-a-guide-to-the-latest-enterprise-data-architecture-d7a266a3bc33?source=collection_archive---------2-----------------------#2023-05-30)

## 了解为什么大型公司正在采用数据网格

[](https://col-jung.medium.com/?source=post_page-----d7a266a3bc33--------------------------------)[![Col Jung](../Images/45ef9475b60f22a3c78c9c8e428812c3.png)](https://col-jung.medium.com/?source=post_page-----d7a266a3bc33--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d7a266a3bc33--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d7a266a3bc33--------------------------------) [Col Jung](https://col-jung.medium.com/?source=post_page-----d7a266a3bc33--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d4e2c520037&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-lakes-to-data-mesh-a-guide-to-the-latest-enterprise-data-architecture-d7a266a3bc33&user=Col+Jung&userId=8d4e2c520037&source=post_page-8d4e2c520037----d7a266a3bc33---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d7a266a3bc33--------------------------------) ·17 min read·2023年5月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd7a266a3bc33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-lakes-to-data-mesh-a-guide-to-the-latest-enterprise-data-architecture-d7a266a3bc33&user=Col+Jung&userId=8d4e2c520037&source=-----d7a266a3bc33---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd7a266a3bc33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-data-lakes-to-data-mesh-a-guide-to-the-latest-enterprise-data-architecture-d7a266a3bc33&source=-----d7a266a3bc33---------------------bookmark_footer-----------)![](../Images/d3b6f86acb42601cc9493acc18a92e28.png)

图片由 [takahiro takuchi](https://unsplash.com/photos/McmRfLKw8yg) 提供 (Unsplash)

全球大型组织正在经历重大“数据震荡”。

这是公司数据湖向**数据网格**的***去中心化***转型。

在我曾在澳大利亚的‘四大’银行之一从事分析工作的[五年半](/from-data-warehouses-and-lakes-to-data-mesh-a-guide-to-enterprise-data-architecture-e2d93b2466b1)中，我们正处于一个巨大的转型旅程中，同时构建多个大型基础设施项目：

+   向[云原生](https://generativeai.pub/cloud-computing-unleashed-how-to-harness-the-power-of-cloud-for-your-business-f72e8e23be9)数据平台迁移到Azure PaaS；

+   构建一个[战略数据产品](https://generativeai.pub/data-products-why-your-organisation-needs-them-4ac7bf2e5953)的阵列；

+   将我们的数据湖联邦化成一个**去中心化的数据网格**。

不过我得承认……这对我们可怜的数据分析师和数据科学家来说有点破坏性。

想象一下，在建筑工人装修你的房间时尝试书法。

**更新**：我现在在[YouTube](https://www.youtube.com/@col_builds)上发布分析内容。

数据湖——数据科学家们偏爱的“热饮”——目前正如面团般被拆解，并作为网格梦想的一部分，联邦化到各个业务领域。
