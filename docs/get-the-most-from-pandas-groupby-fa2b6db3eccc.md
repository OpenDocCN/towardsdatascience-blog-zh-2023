# 从 Pandas GroupBy 中获取最多收益

> 原文：[`towardsdatascience.com/get-the-most-from-pandas-groupby-fa2b6db3eccc?source=collection_archive---------8-----------------------#2023-10-06`](https://towardsdatascience.com/get-the-most-from-pandas-groupby-fa2b6db3eccc?source=collection_archive---------8-----------------------#2023-10-06)

## 从基本示例到实际练习

[](https://medium.com/@kurt.klingensmith?source=post_page-----fa2b6db3eccc--------------------------------)![库尔特·克林根史密斯](https://medium.com/@kurt.klingensmith?source=post_page-----fa2b6db3eccc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fa2b6db3eccc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa2b6db3eccc--------------------------------) [库尔特·克林根史密斯](https://medium.com/@kurt.klingensmith?source=post_page-----fa2b6db3eccc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbaf16815de65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-the-most-from-pandas-groupby-fa2b6db3eccc&user=Kurt+Klingensmith&userId=baf16815de65&source=post_page-baf16815de65----fa2b6db3eccc---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa2b6db3eccc--------------------------------) ·6 分钟阅读·2023 年 10 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffa2b6db3eccc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-the-most-from-pandas-groupby-fa2b6db3eccc&user=Kurt+Klingensmith&userId=baf16815de65&source=-----fa2b6db3eccc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffa2b6db3eccc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fget-the-most-from-pandas-groupby-fa2b6db3eccc&source=-----fa2b6db3eccc---------------------bookmark_footer-----------)![](img/7ec1d3c4e2e305e8ddda6adf5ac61ee5.png)

照片由[亚历克斯·苏普伦](https://unsplash.com/@sooprun?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布于[Unsplash](https://unsplash.com/photos/A53o1drQS2k?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

Python 的 pandas 库包含许多用于查询和操作数据的有用工具，其中之一就是强大的 GroupBy 函数。这个函数允许按不同类别对观测结果进行分组，并以多种方式进行汇总。

这可能一开始听起来有些混乱，但本指南将逐步讲解如何使用该功能及其各种特性。该教程包括：

+   GroupBy 简介。

+   将 GroupBy 应用于实践数据集。

+   各种 GroupBy 技术。

+   实践练习与应用。

## 代码和数据：

本文所用的数据和包含完整 Python 代码的 Jupyter 笔记本可以在[链接的 GitHub 页面](https://github.com/kurtklingensmith/GroupBy)上找到。下载或克隆该仓库以进行跟随。本指南使用了由作者为本文生成的虚假姓名的合成数据；数据可以在链接的 GitHub 页面上找到。

代码需要以下库：

```py
# Data Handling
import pandas as pd
import numpy as np

# Data visualization
import plotly.express as px
```

## 1.1\. 入门 — 数据加载和 GroupBy 基础
