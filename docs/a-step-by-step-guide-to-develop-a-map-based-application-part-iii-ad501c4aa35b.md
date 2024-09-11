# 使用 React 为你的 Web 应用添加互动功能

> 原文：[https://towardsdatascience.com/a-step-by-step-guide-to-develop-a-map-based-application-part-iii-ad501c4aa35b?source=collection_archive---------9-----------------------#2023-02-20](https://towardsdatascience.com/a-step-by-step-guide-to-develop-a-map-based-application-part-iii-ad501c4aa35b?source=collection_archive---------9-----------------------#2023-02-20)

## 开发基于地图的应用程序的分步指南（第三部分）

[](https://medium.com/@jacky.kaub?source=post_page-----ad501c4aa35b--------------------------------)[![Jacky Kaub](../Images/e66c699ee5a9d5bbd58a1a72d688234a.png)](https://medium.com/@jacky.kaub?source=post_page-----ad501c4aa35b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ad501c4aa35b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ad501c4aa35b--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----ad501c4aa35b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ccb7065ef90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-develop-a-map-based-application-part-iii-ad501c4aa35b&user=Jacky+Kaub&userId=7ccb7065ef90&source=post_page-7ccb7065ef90----ad501c4aa35b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ad501c4aa35b--------------------------------) · 24 分钟阅读 · 2023年2月20日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fad501c4aa35b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-develop-a-map-based-application-part-iii-ad501c4aa35b&user=Jacky+Kaub&userId=7ccb7065ef90&source=-----ad501c4aa35b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fad501c4aa35b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-step-by-step-guide-to-develop-a-map-based-application-part-iii-ad501c4aa35b&source=-----ad501c4aa35b---------------------bookmark_footer-----------)![](../Images/87da49187774c09f8bfac884d0cfcb7a.png)

图片由 [Edgar](https://unsplash.com/@ymoran?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

地图是可视化和理解地理数据的强大工具，但需要特定的技能才能有效设计。

在本分步指南中，我们将深入探讨如何构建一个基于地图的应用程序，用于显示客户周围加油站的价格。我们将涵盖从最初的概念验证（POC）到最简可行产品（MVP）的不同关键步骤。

## 系列文章：

[第一部分：概念验证——构建一个极简演示](/a-step-by-step-guide-to-develop-a-map-based-application-part-i-757766b04f77)

[第二部分：如何使用 React 构建 Web 应用（静态布局）](/a-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9)

第三部分：使用 React 为你的 Web 应用添加互动性

[第四部分：使用 PostgreSQL、FastAPI 和 Docker 构建后端](/build-a-back-end-with-postgresql-fastapi-and-docker-7ebfe59e4f06)

## 本文的背景信息

本文直接延续了第二部分，在那里我们开始使用 React 构建 Web 应用的 UI。

[在上一篇文章中](/a-step-by-step-guide-to-develop-a-map-based-application-part-ii-6d3fa7dbd8b9)，我们开始探索 React 在构建 Web 应用方面的潜力，特别是用于我们的加油站查找器。我们最终得到了一个很棒的应用……
