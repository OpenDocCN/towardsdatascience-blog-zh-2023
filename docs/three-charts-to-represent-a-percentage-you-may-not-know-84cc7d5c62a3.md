# 三种你可能不知道的百分比表示图表

> 原文：[https://towardsdatascience.com/three-charts-to-represent-a-percentage-you-may-not-know-84cc7d5c62a3?source=collection_archive---------10-----------------------#2023-08-23](https://towardsdatascience.com/three-charts-to-represent-a-percentage-you-may-not-know-84cc7d5c62a3?source=collection_archive---------10-----------------------#2023-08-23)

## 数据可视化，Python

## 一个可以运行的 Python Altair 教程，展示如何构建图表来表示百分比

[](https://alod83.medium.com/?source=post_page-----84cc7d5c62a3--------------------------------)[![Angelica Lo Duca](../Images/45aa2e2e504bb3af6d3b8009dc6f030e.png)](https://alod83.medium.com/?source=post_page-----84cc7d5c62a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----84cc7d5c62a3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----84cc7d5c62a3--------------------------------) [Angelica Lo Duca](https://alod83.medium.com/?source=post_page-----84cc7d5c62a3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8bc34d63aee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-charts-to-represent-a-percentage-you-may-not-know-84cc7d5c62a3&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=post_page-f8bc34d63aee----84cc7d5c62a3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----84cc7d5c62a3--------------------------------) ·5 min read·2023年8月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F84cc7d5c62a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-charts-to-represent-a-percentage-you-may-not-know-84cc7d5c62a3&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=-----84cc7d5c62a3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F84cc7d5c62a3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthree-charts-to-represent-a-percentage-you-may-not-know-84cc7d5c62a3&source=-----84cc7d5c62a3---------------------bookmark_footer-----------)![](../Images/d46d667b4e2bbd94a23ace4256ce9b2a.png)

照片由 [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

以视觉方式表示百分比可能更能吸引观众，并帮助他们更好地理解数据。以视觉方式表示百分比有不同的方法。最直接的策略是“大数字”（BAN），如下图所示。

![](../Images/f2864c0592164be216760afeb2a19b2b.png)

图片来源：作者

除了 BAN，还有其他几种表示百分比的视觉方式。在本文中，我们将重点介绍三种策略：

+   圆环图

+   100% 堆叠图

+   华夫图。

我们将使用 Python Altair 来展示如何构建每种策略。Vega-Altair 库（简称 Altair）是一个基于 Vega 和 Vega-Lite 可视化语法的声明式 Python 库。有关如何开始使用 Altair 的更多细节，你可以阅读官方的 [Python Altair 文档](https://altair-viz.github.io/)。

# 设置场景
