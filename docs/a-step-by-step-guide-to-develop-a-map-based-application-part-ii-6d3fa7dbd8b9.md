# 如何使用 React 构建 Web 应用程序

> 原文：[https://towardsdatascience.com/a-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9?source=collection_archive---------4-----------------------#2023-02-09](https://towardsdatascience.com/a-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9?source=collection_archive---------4-----------------------#2023-02-09)

## 开发基于地图的应用程序的逐步指南（第二部分）

[![Jacky Kaub](../Images/e66c699ee5a9d5bbd58a1a72d688234a.png)](https://medium.com/@jacky.kaub?source=post_page-----6d3fa7dbd8b9--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6d3fa7dbd8b9--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----6d3fa7dbd8b9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ccb7065ef90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9&user=Jacky+Kaub&userId=7ccb7065ef90&source=post_page-7ccb7065ef90----6d3fa7dbd8b9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d3fa7dbd8b9--------------------------------) · 20 min read · 2023年2月9日

--

![](../Images/6a0d5cd09cb96bc373b944c80753e707.png)

照片由 [Lautaro Andreani](https://unsplash.com/@lautaroandreani?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

地图是可视化和理解地理数据的强大工具，但需要特定的技能来高效设计。

在这份逐步指南中，我们将深入探讨如何构建一个基于地图的应用程序，以显示客户周围加油站的价格。我们将覆盖产品的不同关键步骤，从最初的概念验证（POC）到最小可行产品（MVP）。

## 系列文章：

[第一部分：概念验证 — 构建一个简约的演示](https://medium.com/p/757766b04f77)

第二部分：如何使用React构建网页应用程序（静态布局）

[第三部分：使用React为网页应用程序添加互动性](/a-step-by-step-guide-to-develop-a-map-based-application-part-iii-ad501c4aa35b)

[第四部分：使用PostgreSQL、FastAPI和Docker构建后端](https://medium.com/towards-data-science/build-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06)

## 关于这篇文章的背景

在这篇文章中，我们将继续之前启动的加油站寻找应用程序的工作。这次，我们将专注于准备第一个可以使用静态数据集在本地运行的网页应用程序。该网页应用程序将使用**React框架**构建，本文将尽可能详细地介绍……
