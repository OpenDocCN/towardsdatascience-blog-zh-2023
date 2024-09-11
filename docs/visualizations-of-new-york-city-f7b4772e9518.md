# 纽约市的可视化

> 原文：[`towardsdatascience.com/visualizations-of-new-york-city-f7b4772e9518?source=collection_archive---------9-----------------------#2023-08-18`](https://towardsdatascience.com/visualizations-of-new-york-city-f7b4772e9518?source=collection_archive---------9-----------------------#2023-08-18)

## 使用 Python 和 Plotly 使纽约市开放数据生动起来

[](https://medium.com/@anthonybaum?source=post_page-----f7b4772e9518--------------------------------)![Anthony Baum](https://medium.com/@anthonybaum?source=post_page-----f7b4772e9518--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f7b4772e9518--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7b4772e9518--------------------------------) [Anthony Baum](https://medium.com/@anthonybaum?source=post_page-----f7b4772e9518--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad510de786e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizations-of-new-york-city-f7b4772e9518&user=Anthony+Baum&userId=ad510de786e8&source=post_page-ad510de786e8----f7b4772e9518---------------------post_header-----------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7b4772e9518--------------------------------) 上发布 · 阅读时间 9 分钟 · 2023 年 8 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff7b4772e9518&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizations-of-new-york-city-f7b4772e9518&user=Anthony+Baum&userId=ad510de786e8&source=-----f7b4772e9518---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff7b4772e9518&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualizations-of-new-york-city-f7b4772e9518&source=-----f7b4772e9518---------------------bookmark_footer-----------)![](img/3253b6702c804c0143397b8f62aed9b8.png)

图片由 [Fabien BELLANGER](https://unsplash.com/@fabbel78?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

纽约市的 [开放数据平台](https://opendata.cityofnewyork.us/) 是一个不可思议的信息源。所有由城市收集和生成的公共数据都被 [法律规定](https://opendata.cityofnewyork.us/open-data-law/) 通过该门户网站提供，并且对公众免费使用。

数据集涵盖了从交通、住房和机动车事故，到中央公园松鼠普查，甚至是公园护林员关于攻击性乌龟的报告。

地理、基础设施和社会学等数据集代表了现实世界的过程和事件。即使你与纽约市或城市区域没有任何关联或兴趣，它们也给你提供了一个处理数据的机会，这些数据更接近你在专业角色中遇到的情况，而不像 MNIST 或泰坦尼克号幸存者数据。更好的是，它们几乎一样容易获取。

我们将演示这些数据集使用起来有多么简单，并在过程中构建一些有趣的可视化效果。

为了保持代码块的简洁，以下是本帖中所有代码所需的模块：

```py
import folium
import geopandas as gpd
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import plotly.express as px
import…
```
