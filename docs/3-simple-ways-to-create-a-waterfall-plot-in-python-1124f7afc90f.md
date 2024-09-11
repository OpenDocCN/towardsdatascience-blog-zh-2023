# 在Python中创建瀑布图的3种简单方法

> 原文：[https://towardsdatascience.com/3-simple-ways-to-create-a-waterfall-plot-in-python-1124f7afc90f?source=collection_archive---------4-----------------------#2023-02-07](https://towardsdatascience.com/3-simple-ways-to-create-a-waterfall-plot-in-python-1124f7afc90f?source=collection_archive---------4-----------------------#2023-02-07)

![](../Images/40810d57459b31d7d73d9fd02ec236e0.png)

图片由Midjourney生成

## 学习如何快速创建一个适合演示的图表，以帮助你的数据讲述故事。

[](https://eryk-lewinson.medium.com/?source=post_page-----1124f7afc90f--------------------------------)[![Eryk Lewinson](../Images/56e09e19c0bbfecc582da58761d15078.png)](https://eryk-lewinson.medium.com/?source=post_page-----1124f7afc90f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1124f7afc90f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1124f7afc90f--------------------------------) [Eryk Lewinson](https://eryk-lewinson.medium.com/?source=post_page-----1124f7afc90f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44bc27317e6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-simple-ways-to-create-a-waterfall-plot-in-python-1124f7afc90f&user=Eryk+Lewinson&userId=44bc27317e6b&source=post_page-44bc27317e6b----1124f7afc90f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1124f7afc90f--------------------------------) ·6分钟阅读·2023年2月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1124f7afc90f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-simple-ways-to-create-a-waterfall-plot-in-python-1124f7afc90f&user=Eryk+Lewinson&userId=44bc27317e6b&source=-----1124f7afc90f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1124f7afc90f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-simple-ways-to-create-a-waterfall-plot-in-python-1124f7afc90f&source=-----1124f7afc90f---------------------bookmark_footer-----------)

瀑布图（或图表）经常用于展示某个值随时间的累积变化。或者，它们也可以使用固定的类别（例如，某些事件）而不是时间。因此，这种图表在向业务利益相关者进行演示时非常有用，因为我们可以轻松展示，例如，公司收入/客户基础随时间的变化。

在这篇文章中，我将向你展示如何轻松地在Python中创建瀑布图。为此，我们将使用3个不同的库。

# 设置和数据

和往常一样，我们首先导入一些库。

```py
import pandas as pd

# plotting
import matplotlib.pyplot as plt
import waterfall_chart
from waterfall_ax import WaterfallChart
import plotly.graph_objects as go

# settings
plt.rcParams["figure.figsize"] = (16, 8)
```

然后，我们为我们的示例准备虚拟数据。假设我们是一个初创公司的数据科学家，该公司创建了一种移动应用程序。为了为下一次全员大会做准备，我们被要求提供一张显示我们应用程序在2022年的用户基础的图表。为了…
