# 向 Matplotlib 图形中添加插图坐标轴

> 原文：[`towardsdatascience.com/adding-inset-axes-to-matplotlib-figures-811a9482c43c`](https://towardsdatascience.com/adding-inset-axes-to-matplotlib-figures-811a9482c43c)

## 如何向 Matplotlib 图形中添加插图坐标轴以改善数据可视化

[](https://andymcdonaldgeo.medium.com/?source=post_page-----811a9482c43c--------------------------------)![Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----811a9482c43c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----811a9482c43c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----811a9482c43c--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----811a9482c43c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----811a9482c43c--------------------------------) ·6 分钟阅读·2023 年 11 月 3 日

--

![](img/c2611dd7ee8b182f605f24f27231806c.png)

图像由 DALL-E 3 创建，作者提供。

在创建 matplotlib 图形时，有时你可能会想在主图形中添加一个较小的图形或坐标轴。这可能有多种原因，但最常见的是用于突出显示感兴趣的区域并放大它，或包括与主图形相关的附加信息。

Matplotlib 通过允许我们在主图形内添加插图坐标轴，使这个过程变得非常简单。然后可以进一步调整，以获取我们想要显示的信息。

在这个简短的教程中，我们将学习如何使用网格坐标创建一个简单的井位置图，如下图所示。然后，我们将添加插图坐标轴以突出和放大主地图中的较小区域。

![](img/9dba1000ac81fb72734b47a229dbfb3c.png)

最终的 matplotlib 图形包含的 inset_axes 突出了较小的选井区域。图像由作者提供。

# 导入库和加载数据

我们教程的第一步是导入我们将要使用的库。

前两个导入语句应该很熟悉：导入 [**pandas**](https://pandas.pydata.org/) 和 [**matplotib**](https://matplotlib.org/stable/)。第三个导入自 `[mpl.toolkits.axes_grid1.inset_locator](https://matplotlib.org/stable/api/_as_gen/mpl_toolkits.axes_grid1.inset_locator.html)` 允许我们在主图形的范围内绘制插图图形。

```py
import pandas as pd
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1.inset_locator import inset_axes, mark_inset
```

导入库之后，我们可以导入数据。这里使用的数据来自 Force 2020 Xeek 机器学习竞赛，包含大约 100 个井的井测量数据。

由于该文件以分号（`；`）分隔，我们需要添加一个额外的参数来告诉 `pd.read_csv()` 函数这是值之间的分隔符。

```py
df = pd.read_csv('../data/Xeek Force 2020/train.csv', sep=';')
```

# 使用 X 和 Y 坐标在 Matplotlib 中创建井位置图

现在数据已加载，我们可以创建第一个图形。

该图将包含来自 CSV 文件的 X 和 Y 位置数据，并显示数据集中包含的井的位置。

这些位置是网格坐标，而不是纬度和经度。

首先，我们创建图形并将大小设置为 12 x 12。

```py
fig, ax = plt.subplots(figsize=(12, 12))
```

然后，我们创建散点图并传入 `X_LOC` 和 `Y_LOC` 列。为了使数据点位于网格前面，我们需要将 `zorder` 设置为 1。

```py
ax.scatter(df['X_LOC'], df['Y_LOC'], c='deepskyblue', 
                     edgecolors='black', zorder=1, s=100, marker='o')
```

接下来，我们通过定义标签和限制来设置 x 和 y 轴。

```py
ax.set_xlabel('X-Location (m)', fontsize=22, labelpad=20)
ax.set_ylabel('Y-Location (m)', fontsize=22, labelpad=20)

ax.set_xlim(420000, 570000)
ax.set_ylim(6400000, 6900000)
```

在处理非常大的数字时，matplotlib 将切换到科学计数法。如果我们想避免这种情况，可以使用以下代码：

```py
ax.ticklabel_format(style='plain')
```

最后，我们将使网格显示出来，然后将 `zorder` 设置为 0，这样网格将位于最底层。

```py
ax.grid(zorder=0)
```

我们的最终代码应类似于以下内容：

```py
fig, ax = plt.subplots(figsize=(12, 12))

ax.scatter(df['X_LOC'], df['Y_LOC'], c='deepskyblue', 
                     edgecolors='black', zorder=1, s=100, marker='o')

ax.ticklabel_format(style='plain')

ax.set_xlabel('X-Location (m)', fontsize=22, labelpad=20)
ax.set_ylabel('Y-Location (m)', fontsize=22, labelpad=20)

ax.set_xlim(420000, 570000)
ax.set_ylim(6400000, 6900000)

ax.grid(zorder=0)

plt.show()
```

当我们调用 `plt.show()` 时，我们将得到以下图表。

![](img/7052e1938e0596f5f7fcf156e16943f9.png)

主要的 matplotlib 散点图显示挪威大陆架上井的位置。图像由作者提供。

# 向 Matplotlib 添加插图图形

一旦主图设置好后，我们需要设置插图图形。此代码基本上跟随上一节的内容。

首先，我们声明一个名为 `ax_inset` 的新变量，并将其分配给我们之前导入的 `inset_axes` 方法。这将允许我们在现有坐标轴 `ax` 的右下角创建一个新的插图坐标系。

插图坐标系将设置为 `ax` 宽度和高度的 30%。`loc` 参数控制插图坐标系的位置，并将其放置在父坐标轴的右下角。

最后，对 `bbox_transform` 和 `bbox_to_anchor` 的调用将指定边界框 (`bbox`) 的坐标参考系统。

```py
ax_inset = inset_axes(ax, width="30%", height="30%", loc='lower right',
                      bbox_to_anchor=(0, 0.05, 0.95, 1),
                      bbox_transform=ax.transAxes)
```

一旦定义了插图坐标系，我们可以像设置普通坐标轴对象一样设置参数。这里我们将其设置为显示与主图相同的数据，但由于我们要放大图的一部分，因此需要调整 `xlim` 和 `ylim` 参数。

```py
ax_inset.scatter(df['X_LOC'], df['Y_LOC'], c='deepskyblue', 
                     edgecolors='black', zorder=2, s=50, marker='o')

ax_inset.set_xlim(472000, 480000)
ax_inset.set_ylim(6508000, 6528000)
```

为了避免坐标轴上的科学计数法问题，我们可以将刻度标签样式设置为普通。

```py
ax_inset.ticklabel_format(style='plain')
```

![](img/ba8ff39bf9a7e9c242d7b1c4f918e709.png)

添加插图坐标系后的 Matplotlib 图。图像由作者提供。

## 减少插图坐标系 X 和 Y 轴标签间距

如果我们保持插图坐标系不变，图表上可能会出现大量刻度标签。为了解决这个问题，我们可以使用以下代码将图分成三个均匀间隔的区域，然后放置刻度标记和标签。

```py
ax_inset.xaxis.set_major_locator(plt.MaxNLocator(nbins=3))
ax_inset.yaxis.set_major_locator(plt.MaxNLocator(nbins=3))

ax_inset.grid()
```

![](img/3a85a1335ecc9cb5f279339e2f1844db.png)

添加插图坐标系并将每个坐标轴的标签修正为三个后的 Matplotlib 图。图像由作者提供。

## 标记插图坐标系并将其与主坐标系关联

最后，为了突出显示我们的插图图形如何与主图形相关，我们可以调用`mark_inset`并传入两个坐标轴对象，首先是父坐标轴对象（`ax`），然后是插图坐标轴对象（`ax_inset`）。

然后，我们需要指定标记插图的连接线的位置。在这种情况下，我们使用位置 2（左上角）和位置 3（左下角）。如果需要，也可以在此处设置连接线的颜色（`ec`）。

如果需要，我们可以给主坐标轴对象添加一些颜色，以填充插图覆盖的区域。在这个示例中，我将其设置为填充红色区域。

```py
mark_inset(ax, ax_inset, loc1=2, loc2=3, fc='red', ec='0.5')
```

当我们展示我们的地图时，应该会有类似于下面的图像。

![](img/9dba1000ac81fb72734b47a229dbfb3c.png)

包含`inset_axes`的最终 matplotlib 图形突出了较小的井位选择。图像由作者提供。

这里是生成上述图形的最终代码。

```py
fig, ax = plt.subplots(figsize=(12, 12))

# Main Scatter Plot
ax.scatter(df['X_LOC'], df['Y_LOC'], c='deepskyblue', 
                     edgecolors='black', zorder=2, s=100, marker='o')

ax.ticklabel_format(style='plain')

ax.set_xlabel('X-Location (m)', fontsize=22, labelpad=20)
ax.set_ylabel('Y-Location (m)', fontsize=22, labelpad=20)

ax.set_xlim(420000, 570000)
ax.set_ylim(6400000, 6900000)

ax.grid(zorder=0)

ax_inset = inset_axes(ax, width="30%", height="30%", loc='lower right',
                      bbox_to_anchor=(0, 0.05, 0.95, 1),
                      bbox_transform=ax.transAxes)

ax_inset.scatter(df['X_LOC'], df['Y_LOC'], c='deepskyblue', 
                     edgecolors='black', zorder=2, s=50, marker='o')

ax_inset.set_xlim(472000, 480000)
ax_inset.set_ylim(6508000, 6528000)

ax_inset.ticklabel_format(style='plain')
ax_inset.xaxis.set_major_locator(plt.MaxNLocator(nbins=3))
ax_inset.yaxis.set_major_locator(plt.MaxNLocator(nbins=3))

ax_inset.grid()

mark_inset(ax, ax_inset, loc1=2, loc2=3, fc='red', ec='0.5')

plt.show()
```

# 总结

在这个简短的教程中，我们已经看到如何轻松地将插图坐标轴添加到我们的主 matplotlib 图形中，以突出显示井位地图的较小区域。这不仅突出了对读者重要的区域，还可以使图表看起来更具美观。

# 本教程中使用的数据集

作为 Xeek 和 FORCE 2020 机器学习竞赛的一部分使用的训练数据集 *(Bormann et al., 2020)*。该数据集的许可证为 Creative Commons Attribution 4.0 International。

完整的数据集可以通过以下链接访问: [`doi.org/10.5281/zenodo.4351155`](https://doi.org/10.5281/zenodo.4351155)。

*感谢阅读。在你离开之前，你一定要订阅我的内容，并将我的文章发送到你的收件箱。* [***你可以在这里订阅！***](https://andymcdonaldgeo.medium.com/subscribe)*此外，如果你喜欢这篇内容并想表示感谢，请考虑给它点个赞。*
