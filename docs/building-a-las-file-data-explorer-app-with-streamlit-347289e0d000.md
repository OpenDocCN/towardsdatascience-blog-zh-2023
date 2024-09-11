# 使用 Streamlit 构建 LAS 文件数据探索应用

> 原文：[https://towardsdatascience.com/building-a-las-file-data-explorer-app-with-streamlit-347289e0d000?source=collection_archive---------11-----------------------#2023-02-03](https://towardsdatascience.com/building-a-las-file-data-explorer-app-with-streamlit-347289e0d000?source=collection_archive---------11-----------------------#2023-02-03)

## 使用 Python 和 Streamlit 探索 Log ASCII 标准文件

[](https://andymcdonaldgeo.medium.com/?source=post_page-----347289e0d000--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----347289e0d000--------------------------------)[](https://towardsdatascience.com/?source=post_page-----347289e0d000--------------------------------)[![数据科学之道](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----347289e0d000--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----347289e0d000--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-las-file-data-explorer-app-with-streamlit-347289e0d000&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----347289e0d000---------------------post_header-----------) 发表在 [数据科学之道](https://towardsdatascience.com/?source=post_page-----347289e0d000--------------------------------) ·14分钟阅读·2023年2月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F347289e0d000&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-las-file-data-explorer-app-with-streamlit-347289e0d000&user=Andy+McDonald&userId=9c280f85f15c&source=-----347289e0d000---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F347289e0d000&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-las-file-data-explorer-app-with-streamlit-347289e0d000&source=-----347289e0d000---------------------bookmark_footer-----------)![](../Images/c6d1f16ce8d9cc0c63a877d3f82c55e0.png)

图片由 [Carlos Muza](https://unsplash.com/fr/@kmuza?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

LAS 文件是石油和天然气行业中传输和存储测井和/或岩石物理数据的标准且简单的方式。该格式由[加拿大测井协会](https://www.cwls.org/products/#products-las)在80年代末和90年代初开发，旨在标准化和组织数字测井信息。LAS 文件本质上是结构化的ASCII文件，包含多个部分，包含关于井及其数据的信息；因此，它们可以在如Notepad或TextEdit等常见文本编辑器中轻松查看。

[Streamlit](https://streamlit.io/) 是我最喜欢的 Python 库之一，用于创建快速且易于使用的仪表板或交互式工具。如果你想创建一个应用程序，不论是你还是最终用户都无需担心代码，它也非常适合。因此，在本文中，我们将探讨如何使用Streamlit构建一个用于LAS文件的数据探索应用程序。

如果你想查看完整的应用程序演示，请查看下面的短视频。
