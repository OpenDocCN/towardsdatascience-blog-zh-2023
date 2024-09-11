# 开发基于地图的应用程序的逐步指南（第一部分）

> 原文：[https://towardsdatascience.com/a-step-by-step-guide-to-develop-a-map-based-application-part-i-757766b04f77?source=collection_archive---------6-----------------------#2023-01-24](https://towardsdatascience.com/a-step-by-step-guide-to-develop-a-map-based-application-part-i-757766b04f77?source=collection_archive---------6-----------------------#2023-01-24)

## 概念验证——构建一个极简的演示

[](https://medium.com/@jacky.kaub?source=post_page-----757766b04f77--------------------------------)[![Jacky Kaub](../Images/e66c699ee5a9d5bbd58a1a72d688234a.png)](https://medium.com/@jacky.kaub?source=post_page-----757766b04f77--------------------------------)[](https://towardsdatascience.com/?source=post_page-----757766b04f77--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----757766b04f77--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----757766b04f77--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ccb7065ef90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-develop-a-map-based-application-part-i-757766b04f77&user=Jacky+Kaub&userId=7ccb7065ef90&source=post_page-7ccb7065ef90----757766b04f77---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----757766b04f77--------------------------------) ·19分钟阅读·2023年1月24日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F757766b04f77&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-develop-a-map-based-application-part-i-757766b04f77&source=-----757766b04f77---------------------bookmark_footer-----------)![](../Images/c4b4a0fc6043af44431cec2c84ae5ec0.png)

图片由[GeoJango Maps](https://unsplash.com/@geojango_maps?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

地图是可视化和理解地理数据的强大工具，但它们需要特定的技能才能有效设计。

在这份逐步指南中，我们将深入探讨如何构建一个基于地图的应用程序，以展示周围加油站的价格。我们将覆盖从原始概念验证（POC）到最小可行产品（MVP）的不同关键步骤。

## 系列文章：

第一部分：概念验证 — 构建一个极简的演示

[第二部分：如何使用 React 构建网页应用（静态布局）](/a-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9)

[第三部分：使用 React 为你的网页应用添加互动性](/a-step-by-step-guide-to-develop-a-map-based-application-part-iii-ad501c4aa35b)

[第四部分：使用 PostgreSQL、FastAPI 和 Docker 构建后端](https://medium.com/towards-data-science/build-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06)

## 这篇文章涵盖的内容

在这篇系列文章的第一篇中，我们正在准备我们应用程序的概念验证。

我们刚刚有了一个想法，希望将其转化为有价值的东西，但还不确定是否可行。我们需要寻找数据，评估其相关性，并构建一个基础的演示，以展示是否...
