# 可视化气候变化：使用 Python 逐步指南重现气候条纹

> 原文：[`towardsdatascience.com/visualizing-climate-change-a-step-by-step-guide-to-reproduce-climate-stripes-with-python-ea1d440e8e8d?source=collection_archive---------7-----------------------#2023-02-17`](https://towardsdatascience.com/visualizing-climate-change-a-step-by-step-guide-to-reproduce-climate-stripes-with-python-ea1d440e8e8d?source=collection_archive---------7-----------------------#2023-02-17)

## 一份快速的 Matplotlib 教程，用于构建出色的可视化

![Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----ea1d440e8e8d--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea1d440e8e8d--------------------------------) [Guillaume Weingertner](https://guillaume-weingertner.medium.com/?source=post_page-----ea1d440e8e8d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4ebea49e580e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizing-climate-change-a-step-by-step-guide-to-reproduce-climate-stripes-with-python-ea1d440e8e8d&user=Guillaume+Weingertner&userId=4ebea49e580e&source=post_page-4ebea49e580e----ea1d440e8e8d---------------------post_header-----------) 发表于 [数据科学探索](https://towardsdatascience.com/?source=post_page-----ea1d440e8e8d--------------------------------) ·6 分钟阅读·2023 年 2 月 17 日

--

![](img/5959b4c0517550dd98a542c2781646a4.png)

全球温度变化（1850–2022）— 作者提供的图像 — 灵感来自 Ed Hawkins

# 动机

气候变化是我们时代最紧迫的问题之一。为了提高意识并传达问题的紧迫性，科学家和数据分析师们开发了多种可视化气候数据的方法。其中一种特别有效的方法是“气候条纹”可视化，最初由雷丁大学的气候科学家**艾德·霍金斯**推广。在本文中，我们将详细讲解如何使用 Python 再现气候条纹，为任何有兴趣制作自己气候条纹可视化的人提供逐步指南。无论你是气候科学家、数据分析师，还是对了解这个问题感兴趣的普通人，这个指南将为你提供制作和分享气候条纹所需的工具。

这种可视化的美在于它的简洁。只需快速浏览一下，读者就能理解信息，无需在技术细节中迷失。

自发布以来，气候条纹已广泛应用于各种媒体。2019 年 9 月，这一可视化甚至登上了《The…》的封面。
