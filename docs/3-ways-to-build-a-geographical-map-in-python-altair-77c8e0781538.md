# 在 Python Altair 中构建地理地图的 3 种方法

> 原文：[https://towardsdatascience.com/3-ways-to-build-a-geographical-map-in-python-altair-77c8e0781538?source=collection_archive---------6-----------------------#2023-01-30](https://towardsdatascience.com/3-ways-to-build-a-geographical-map-in-python-altair-77c8e0781538?source=collection_archive---------6-----------------------#2023-01-30)

## 数据可视化，地理数据

## 一个关于如何在 Python Altair 中构建三种不同地图的教程：分级图、点密度图和比例符号图

[](https://alod83.medium.com/?source=post_page-----77c8e0781538--------------------------------)[![Angelica Lo Duca](../Images/45aa2e2e504bb3af6d3b8009dc6f030e.png)](https://alod83.medium.com/?source=post_page-----77c8e0781538--------------------------------)[](https://towardsdatascience.com/?source=post_page-----77c8e0781538--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----77c8e0781538--------------------------------) [Angelica Lo Duca](https://alod83.medium.com/?source=post_page-----77c8e0781538--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8bc34d63aee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-ways-to-build-a-geographical-map-in-python-altair-77c8e0781538&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=post_page-f8bc34d63aee----77c8e0781538---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----77c8e0781538--------------------------------) · 5 分钟阅读 · 2023 年 1 月 30 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F77c8e0781538&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-ways-to-build-a-geographical-map-in-python-altair-77c8e0781538&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=-----77c8e0781538---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F77c8e0781538&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-ways-to-build-a-geographical-map-in-python-altair-77c8e0781538&source=-----77c8e0781538---------------------bookmark_footer-----------)![](../Images/fa7bf2f1fe3245059ddc44cf4c558620.png)

作者提供的图片

对于数据科学家来说，数据可视化是一项必备技能。它有助于快速理解数据中的模式和相关性，否则这些信息可能会被遗漏。地理地图是可视化空间数据的一个绝佳方式，可以用来探索不同国家或地区的趋势。

本文将向你展示如何使用 Python Altair 库构建地理地图。

文章将涵盖以下内容：

+   下载地图

+   分级图

+   点密度图

+   比例符号图

# 下载地图

要在 Python Altair 中创建地理地图，你需要一个 topoJSON 文件来指定地图上各区域的地理边界，以及一个包含每个区域值的数据集。

创建 topoJSON 文件超出了本文的范围，但已有许多资源可以参考……
