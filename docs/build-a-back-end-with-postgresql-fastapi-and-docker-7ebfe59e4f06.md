# 使用 PostgreSQL、FastAPI 和 Docker 构建后端

> 原文：[https://towardsdatascience.com/build-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06?source=collection_archive---------0-----------------------#2023-03-14](https://towardsdatascience.com/build-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06?source=collection_archive---------0-----------------------#2023-03-14)

## 一步一步的指南：开发基于地图的应用程序（第四部分）

[](https://medium.com/@jacky.kaub?source=post_page-----7ebfe59e4f06--------------------------------)[![Jacky Kaub](../Images/e66c699ee5a9d5bbd58a1a72d688234a.png)](https://medium.com/@jacky.kaub?source=post_page-----7ebfe59e4f06--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7ebfe59e4f06--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7ebfe59e4f06--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----7ebfe59e4f06--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ccb7065ef90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06&user=Jacky+Kaub&userId=7ccb7065ef90&source=post_page-7ccb7065ef90----7ebfe59e4f06---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7ebfe59e4f06--------------------------------) · 28分钟阅读·2023年3月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7ebfe59e4f06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06&user=Jacky+Kaub&userId=7ccb7065ef90&source=-----7ebfe59e4f06---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7ebfe59e4f06&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06&source=-----7ebfe59e4f06---------------------bookmark_footer-----------)![](../Images/0c94c9aabf27aae32cad325a9448c7da.png)

照片由 [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) 拍摄，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

地图是可视化和理解地理数据的强大工具，但它们需要特定的技能才能高效设计。

在这本逐步指南中，我们将深入探讨如何构建一个基于地图的应用程序，以显示客户周围加油站的价格。我们将涵盖产品的不同关键步骤，从最初的概念验证（POC）到最小可行产品（MVP）。

# 系列文章：

[第一部分：概念验证 — 构建一个简约的演示](/a-step-by-step-guide-to-develop-a-map-based-application-part-i-757766b04f77)

[第二部分：如何使用 React 构建网页应用（静态布局）](/a-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9)

[第三部分：使用 React 为网页应用添加交互性](/a-step-by-step-guide-to-develop-a-map-based-application-part-iii-ad501c4aa35b)

第四部分：使用 PostgreSQL、FastAPI 和 Docker 构建后端

## 关于这篇文章的一些背景

在本系列的前几篇文章中，我们使用**React**构建了加油站查找器的前端，并将后端视为一个“黑箱”，只提供相关数据。

在这一部分，我们将详细分步介绍如何使用强大的工具构建后端…
