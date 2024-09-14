# 如何在 Matplotlib 中绘制垂直和水平线

> 原文：[`towardsdatascience.com/plot-vertical-horizontal-lines-matplotlib-34cf7fe86ce`](https://towardsdatascience.com/plot-vertical-horizontal-lines-matplotlib-34cf7fe86ce)

## 使用 matplotlib 在 Python 图表中绘制水平线和垂直线

[](https://gmyrianthous.medium.com/?source=post_page-----34cf7fe86ce--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----34cf7fe86ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----34cf7fe86ce--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----34cf7fe86ce--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----34cf7fe86ce--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----34cf7fe86ce--------------------------------) ·阅读时间 5 分钟·2023 年 1 月 9 日

--

![](img/8981960da03285b8fc0bad6ff64a379f.png)

图片来源：[Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Matplotlib 是用于绘制静态或交互式可视化的最常用库之一。在处理图表时，一个常见的任务是需要在图表中的特定位置绘制线条。例如，你可能希望绘制一条水平线或垂直线，以标记阈值或突出显示某个数据点。

在本教程中，我们将演示如何使用 `matplotlib` 函数在现有图表中绘制垂直和水平线。我们还将讨论在将线条添加到图表时应考虑的一些选项和注意事项。

首先，我们创建一个基础图表来可视化正弦函数——一个广为人知的三角函数——我们将在本文中引用它以演示如何绘制附加的线条。

```py
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 20, 100)
y = np.sin(x)

fig, ax = plt.subplots()
ax.plot(x, y)
plt.show()
```

这是输出图表（希望它对你来说很熟悉！）：

![](img/b97b13daf295356a5425d24f290081e8.png)

使用 matplotlib 来可视化正弦函数（trigonometric function）——来源：作者

## 绘制一条水平线

现在，为了在坐标轴上绘制一条水平线，我们可以使用`[matplotlib.pyplot.axhline()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.axhline.html)`函数，该函数接受以下三个参数：

+   `y`：这是水平线将在 y 轴上定位的精确数据点。

+   `xmin`：这是一个浮点数，取值范围在 0 到 1 之间，表示相对于 x 轴的起始点。例如，如果设置为`0.5`，则水平线将从图的中间位置开始，位于指定的`y`位置。值为`0`表示图的最左边，而`1`对应于图的最右边。

+   `xmax`：类似地，这是一个浮点数参数，范围在 0 到 1 之间，表示绘制的水平线的终点。值为`0`表示图的最左边，而`1`对应于图的最右边。

在下面的示例中，我们在`0.75`的 y 轴点添加了一条水平线，起始和结束位置分别为指定的`xmin`和`xmax`值：

```py
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 20, 100)
y = np.sin(x)

fig, ax = plt.subplots()
ax.plot(x, y)

ax.axhline(y=0.75, color='r')

plt.show()
```

生成的图应如下所示：

![](img/d4442495308c7b0a03103ce8d3d88e3c.png)

带有水平线的正弦图 — 来源：作者

如果我们想绘制一条不限制于`xmin`和`xmax`位置的线（它们默认为`0`和`1`），我们可以简单地重新排列函数调用为

```py
ax.axhline(y=0.75, color='r')
```

![](img/b85b7d52ba83ac382cd52c35bf783897.png)

没有 xmin 和 xmax 的水平线 — 来源：作者

## 绘制垂直线

同样，要在坐标轴上绘制垂直线，我们需要调用`[matplotlib.pyplot.axvline()](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.axvline.html)`函数，该函数需要以下三个参数：

+   `x`：这是水平线将在 y 轴上定位的精确数据点。

+   `ymin`：这是一个浮点数，取值范围在 0 到 1 之间，表示相对于 y 轴的起始点。值为`0`表示图的底部，而`1`对应于图的顶部。

+   `ymax`：类似地，这是一个浮点数参数，范围在 0 到 1 之间，表示绘制的垂直线的终点。值为`0`表示图的底部，而`1`对应于图的顶部。

现在假设我们想在现有图上绘制一条垂直线，该垂直线将在`7.5`值处穿过 x 轴。

```py
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 20, 100)
y = np.sin(x)

fig, ax = plt.subplots()
ax.plot(x, y)

ax.axvline(x=7.5, color='y')

plt.show()
```

`axvline()`函数将在指定点创建一条垂直线，从图的顶部到最底部（因为我们没有指定`ymin`和`ymax`）：

![](img/a3b691359fe215bc87accb97d6f0f92a.png)

使用`axvline`绘制的垂直线 — 来源：作者

## 结论

总之，Python 中的 Matplotlib 库通过使用`axhline()`和`axvline()`函数分别允许在图表中创建水平和垂直线。

这些函数接受关于 x 或 y 轴上线的位置的参数，以及有关线的起始和结束点的可选参数。通过使用这些函数，可以很容易地突出显示特定数据点或在 Python 中的图表和图形中添加阈值。

[**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每一个故事。你的会员费用直接支持我和你阅读的其他作者。你还将全面访问 Medium 上的所有故事。**

[如何使用 Matplotlib 绘制对数轴](https://towardsdatascience.com/plot-logarithmic-axes-matplotlib-python-bb8533f430c0?source=post_page-----34cf7fe86ce--------------------------------)

### [通过我的推荐链接加入 Medium — Giorgos Myrianthous](https://gmyrianthous.medium.com/membership?source=post_page-----34cf7fe86ce--------------------------------)

了解何时以及如何在对数刻度上可视化数据

[**相关的文章你可能也会喜欢**](https://gmyrianthous.medium.com/membership?source=post_page-----34cf7fe86ce--------------------------------)

[如何将图例放置在 Matplotlib 图表之外](https://towardsdatascience.com/legend-outside-the-plot-matplotlib-5d9c1caa9d31?source=post_page-----34cf7fe86ce--------------------------------) 

### 展示如何使用 matplotlib 和 Python 将图例放置在图表之外

[如何在 Python 中使用 Matplotlib 绘制对数轴](https://towardsdatascience.com/legend-outside-the-plot-matplotlib-5d9c1caa9d31?source=post_page-----34cf7fe86ce--------------------------------) 

### 作为 Medium 会员，你的一部分会员费用会分给你阅读的作者，并且你可以全面访问所有故事……

[如何使用 Matplotlib 将图表保存为图像文件](https://towardsdatascience.com/plot-logarithmic-axes-matplotlib-python-bb8533f430c0?source=post_page-----34cf7fe86ce--------------------------------) 

### 了解如何将 matplotlib 图形和图表保存为图像文件

[如何将 Matplotlib 图表保存为图像文件](https://towardsdatascience.com/save-plots-matplotlib-1a16b3432d8a?source=post_page-----34cf7fe86ce--------------------------------)
