# 如何使用Plotly和Streamlit创建简单的GIS地图

> 原文：[https://towardsdatascience.com/how-to-create-a-simple-gis-map-with-plotly-and-streamlit-7732d67b84e2?source=collection_archive---------5-----------------------#2023-12-22](https://towardsdatascience.com/how-to-create-a-simple-gis-map-with-plotly-and-streamlit-7732d67b84e2?source=collection_archive---------5-----------------------#2023-12-22)

## 数据可视化

## Plotly地图功能与Streamlit用户界面组件相结合，提供了一种创建GIS风格仪表板的方法

[![Alan Jones](../Images/359379fab1d6685ff08080b98173e67c.png)](https://medium.com/@alan-jones?source=post_page-----7732d67b84e2--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7732d67b84e2--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----7732d67b84e2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d3f5fb94faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-simple-gis-map-with-plotly-and-streamlit-7732d67b84e2&user=Alan+Jones&userId=7d3f5fb94faa&source=post_page-7d3f5fb94faa----7732d67b84e2---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7732d67b84e2--------------------------------) ·14分钟阅读·2023年12月22日

--

![](../Images/2d61b8df3867dd582f1410384f6f274f.png)

约翰·霍普金斯大学的COVID数据仪表板 — 图片来源于[Clay Banks](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在2020年，我们都习惯了在媒体上看到比以往更多的数据。由约翰·霍普金斯大学等创建的复杂数据仪表板，成为了关于COVID-19传播的新闻展示的常见元素，并且成为了必看的内容。

因此，如果疫情带来了任何积极的变化，也许其中之一就是更多的人能够理解数据的图形表示。

这种经验或许导致了在各种类型的数据展示中（无论是医学数据、财务数据还是新闻中的其他数据）图表和仪表盘使用的增加。

除了表示感染率指数增长并跟踪R值的图表，我们还习惯了在上面的约翰斯·霍普金斯仪表盘中看到的那种地理信息。这显示了疫情发展的全球视图，但我们每个人也都有这些系统的本地版本。

这些应用程序使用了复杂的软件来表示数据以及数据映射的地理信息……
