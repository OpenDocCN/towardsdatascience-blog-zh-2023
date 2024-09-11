# 如何在 Matplotlib 中绘制垂直线和水平线

> 原文：[`towardsdatascience.com/plot-vertical-horizontal-lines-matplotlib-34cf7fe86ce?source=collection_archive---------6-----------------------#2023-01-09`](https://towardsdatascience.com/plot-vertical-horizontal-lines-matplotlib-34cf7fe86ce?source=collection_archive---------6-----------------------#2023-01-09)

## 在 Python 绘图中使用 matplotlib 绘制水平线和垂直线

[](https://gmyrianthous.medium.com/?source=post_page-----34cf7fe86ce--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----34cf7fe86ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----34cf7fe86ce--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----34cf7fe86ce--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----34cf7fe86ce--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-vertical-horizontal-lines-matplotlib-34cf7fe86ce&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----34cf7fe86ce---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----34cf7fe86ce--------------------------------) ·5 min read·2023 年 1 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F34cf7fe86ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-vertical-horizontal-lines-matplotlib-34cf7fe86ce&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----34cf7fe86ce---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F34cf7fe86ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fplot-vertical-horizontal-lines-matplotlib-34cf7fe86ce&source=-----34cf7fe86ce---------------------bookmark_footer-----------)![](img/8981960da03285b8fc0bad6ff64a379f.png)

图片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

Matplotlib 是用于在 Python 中绘制静态或交互式可视化图表的最常用库之一。在处理图表和图形时，一个常见的任务是需要在图表的特定位置绘制线条。例如，你可能想要绘制一条水平线或垂直线来标记一个阈值，或者简单地突出显示一个特定的数据点。

在本教程中，我们将演示如何使用 `matplotlib` 函数在现有图中绘制垂直线和水平线。我们还将讨论一些在向图表中添加线条时需要注意的选项和考虑事项。

首先，让我们创建一个基础图，展示 `sin` —— 一个著名的三角函数 —— 我们将在本文中引用它，以演示如何绘制额外的线条。

```py
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 20, 100)
y = np.sin(x)

fig, ax = plt.subplots()
ax.plot(x, y)
plt.show()
```

这是输出的图（希望它对你来说很熟悉！）：
