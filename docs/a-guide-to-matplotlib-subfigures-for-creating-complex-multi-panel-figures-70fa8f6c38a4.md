# Matplotlib 子图形创建复杂多面板图的指南

> 原文：[https://towardsdatascience.com/a-guide-to-matplotlib-subfigures-for-creating-complex-multi-panel-figures-70fa8f6c38a4?source=collection_archive---------3-----------------------#2023-11-01](https://towardsdatascience.com/a-guide-to-matplotlib-subfigures-for-creating-complex-multi-panel-figures-70fa8f6c38a4?source=collection_archive---------3-----------------------#2023-11-01)

## 子图形——用于美丽的多面板图形的强大工具

[](https://tdrose1.medium.com/?source=post_page-----70fa8f6c38a4--------------------------------)[![Tim Rose](../Images/12bcd585b5dad388dad140b4ca049392.png)](https://tdrose1.medium.com/?source=post_page-----70fa8f6c38a4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----70fa8f6c38a4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----70fa8f6c38a4--------------------------------) [Tim Rose](https://tdrose1.medium.com/?source=post_page-----70fa8f6c38a4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb2b238bba8ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-matplotlib-subfigures-for-creating-complex-multi-panel-figures-70fa8f6c38a4&user=Tim+Rose&userId=b2b238bba8ac&source=post_page-b2b238bba8ac----70fa8f6c38a4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----70fa8f6c38a4--------------------------------) ·8 分钟阅读·2023年11月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F70fa8f6c38a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-matplotlib-subfigures-for-creating-complex-multi-panel-figures-70fa8f6c38a4&user=Tim+Rose&userId=b2b238bba8ac&source=-----70fa8f6c38a4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F70fa8f6c38a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-matplotlib-subfigures-for-creating-complex-multi-panel-figures-70fa8f6c38a4&source=-----70fa8f6c38a4---------------------bookmark_footer-----------)

# 动机

复杂的（科学）图形通常由多个具有不同大小或注释的子图组成。如果你使用 matplotlib/seaborn 生态系统，有很多方法可以创建复杂的图形，例如使用 [gridspec](https://matplotlib.org/stable/gallery/subplots_axes_and_figures/gridspec_multicolumn.html)。

然而，这可能会很快变得具有挑战性，特别是如果你想将 seaborn 中的多坐标轴图，如 [jointplot](https://seaborn.pydata.org/generated/seaborn.jointplot.html) 或 [pairgrid](https://seaborn.pydata.org/generated/seaborn.PairGrid.html#seaborn.PairGrid) 集成到你的图形中，因为它们没有提供坐标轴作为输入参数的选项。但在 matplotlib 中还有另一种组合图形的方法，而不仅仅是使用子图：[**Subfigures**](https://matplotlib.org/stable/gallery/subplots_axes_and_figures/subfigures.html)。这是一个强大的框架，可以创建像这样的多面板图形：

![](../Images/d51973be73c935f8aa76c990a9f91967.png)

本文的目标是向你展示如何制作这个图形。

在本文中，我将介绍子图及其功能。我们将结合子图、子图和 gridspecs 来重新创建这个图形。

要跟随本文，你应该对 matplotlib 的 [subplots](https://matplotlib.org/stable/gallery/subplots_axes_and_figures/subplot.html) 和 [gridspec](https://matplotlib.org/stable/gallery/subplots_axes_and_figures/gridspec_multicolumn.html) 有基本的了解（如果没有，你可以查看链接的教程）。

# Matplotlib 子图

首先，我们导入 matplotlib、seaborn，并加载一些示例数据，这些数据将用于填充图表内容：

```py
import matplotlib.pyplot as plt
import seaborn as sns
data = sns.load_dataset('mpg')
```

让我们从 matplotlib 中子图的概念开始。要创建子图，我们首先需要创建一个图形：

```py
fig = plt.figure(figsize=(10, 7))
```

从这一点开始，我们可以类似于子图的方式定义子图。通过提供行数（*2*）和列数（*1*），可以创建一个子图网格。我们还给图形背景上色以突出显示：

```py
(topfig, bottomfig) = fig.subfigures(2, 1)

topfig.set_facecolor('#cbe4c6ff')
topfig.suptitle('Top')

bottomfig.set_facecolor('#c6c8e4ff')
bottomfig.suptitle('Bottom')
```

仅有图形没有任何图表（坐标轴）将不会被显示，因此我们需要为每个子图定义子图。这里我们已经可以看到子图的一个伟大特性，对于每个子图，我们可以定义不同的子图布局：

```py
top_axs = topfig.subplots(2, 4)
bottom_axs = bottomfig.subplots(3, 7)

plt.show()
```

![](../Images/4f54a35709dd40f98ad718729e154456.png)

我们现在有两个独立的图形，可以分别设置它们，但将它们放在一个最终的图形中。当然，我们也可以调整子图的大小比例：

```py
figure = plt.figure(figsize=(10, 7))
figs = figure.subfigures(2, 2, height_ratios=(2,1), width_ratios=(2,1))

figs = figs.flatten()

for i, fig in enumerate(figs):
 fig.suptitle(f'Subfigure {i}')
 axs = fig.subplots(2, 2)

plt.show()
```

![](../Images/7d814070c4d843fbf4b69749262e512b.png)

然而，子图有一个缺点。为了消除标签或图形外部元素的重叠，`plt.tight_layout()` 是一个将所有内容紧凑地放入图形中的好方法。然而，这对子图不支持。这里你可以看到如果尝试使用它会发生什么：

```py
figure = plt.figure(figsize=(10, 7))
figs = figure.subfigures(2, 2, height_ratios=(2,1), width_ratios=(2,1))

figs = figs.flatten()

for i, fig in enumerate(figs):
 fig.suptitle(f'Subfigure {i}')
 axs = fig.subplots(2, 2)

plt.tight_layout()
plt.show()
```

![](../Images/b0e637eec7c27b22e48476cb9a6f08d9.png)

这并不是我们想要的……为了在图表之间插入间距并去除任何重叠，我们需要使用 `subplots_adjust` 函数，它允许我们在子图与边框之间插入（或移除）更多空间：

```py
fig = plt.figure(figsize=(10, 7))
(topfig, bottomfig) = fig.subfigures(2, 1)

topfig.set_facecolor('#cbe4c6ff')
topfig.suptitle('Top')
bottomfig.set_facecolor('#c6c8e4ff')
bottomfig.suptitle('Bottom')

top_axs = topfig.subplots(2, 4)
bottom_axs = bottomfig.subplots(3, 7)

# Adding more space between plots and reducing the space to the sides
topfig.subplots_adjust(left=.1, right=.9, wspace=.5, hspace=.5)

# We can also squeeze subplots to the bottom
bottomfig.subplots_adjust(wspace=.5, hspace=.8, top=.7, bottom=.3)

plt.show()
```

![](../Images/2e48ccfb91bc205dce89c6fb650be26c.png)

子图的另一个伟大方面是它们可以嵌套，这意味着我们可以将每个子图分成更多的子图：

```py
fig = plt.figure(figsize=(10, 7))
(topfig, bottomfig) = fig.subfigures(2, 1)

topfig.set_facecolor('#cbe4c6ff')
topfig.suptitle('Top')
top_axs = topfig.subplots(2, 4)

(bottomleft, bottomright) = bottomfig.subfigures(1, 2, width_ratios=(1,2))

bottomleft.set_facecolor('#c6c8e4ff')
bottomleft.suptitle('Bottom left')
bottom_axs = bottomleft.subplots(2, 2)

bottomright.set_facecolor('#aac8e4ff')
bottomright.suptitle('Bottom right')
bottom_axs = bottomright.subplots(3, 3)

# Spacing between subplots
topfig.subplots_adjust(left=.1, right=.9, wspace=.4, hspace=.4)
bottomleft.subplots_adjust(left=.2, right=.9, wspace=.5, hspace=.4)
bottomright.subplots_adjust(left=.1, right=.9, wspace=.4, hspace=.4)

plt.show()
```

![](../Images/8683969839caafef791e8bd49c4bf8ca.png)

让我们将一个[联合图](https://seaborn.pydata.org/generated/seaborn.jointplot.html)插入到这个图形中。不幸的是，这并不简单，因为seaborn函数没有提供图形对象作为输入的选项。但如果我们查看[函数的源代码](https://github.com/seaborn/seaborn/blob/d1c04f2c2c4dbd11ede016405b5ea51380e37f51/seaborn/axisgrid.py#L1640)，我们可以看到这个图由三个共享x和y轴的子图组成，这些子图通过网格规范定义。

这意味着我们可以轻松地在子图中绘制它：

```py
fig = plt.figure(figsize=(10, 7))
(topfig, bottomfig) = fig.subfigures(2, 1)

topfig.set_facecolor('#cbe4c6ff')
topfig.suptitle('Top')
top_axs = topfig.subplots(2, 4)

# We are using the bottom left subfigure for the jointplot
(bottomleft, bottomright) = bottomfig.subfigures(1, 2, width_ratios=(1,2))

# This parameter defines the size ratio between the main plot and the margin plots
ratio=2

# Defining a gridspec where the subplots are places
gs = plt.GridSpec(ratio + 1, ratio + 1)
# The main scatterplot
ax_joint  = bottomleft.add_subplot(gs[1:, :-1])
# The margin plots are sharing an axis with the main plot
ax_marg_x = bottomleft.add_subplot(gs[0, :-1], sharex=ax_joint)
ax_marg_y = bottomleft.add_subplot(gs[1:, -1], sharey=ax_joint)

# Axis labels and ticklabels for the margin plots are set to not visible
# Since they are shared with the main plot,
# removing them for the margin will also remove them from the main plot
plt.setp(ax_marg_x.get_xticklabels(), visible=False)
plt.setp(ax_marg_y.get_yticklabels(), visible=False)
plt.setp(ax_marg_x.get_xticklabels(minor=True), visible=False)
plt.setp(ax_marg_y.get_yticklabels(minor=True), visible=False)

# Filling the plots with data:
sns.scatterplot(data=data, y='horsepower', x='mpg', ax=ax_joint)
sns.histplot(data=data, y='horsepower', ax=ax_marg_y)
sns.histplot(data=data, x='mpg', ax=ax_marg_x)

bottomright.set_facecolor('#aac8e4ff')
bottomright.suptitle('Bottom right')
bottom_axs = bottomright.subplots(3, 3)

# Spacing between subplots
topfig.subplots_adjust(left=.1, right=.9, wspace=.4, hspace=.4)
bottomright.subplots_adjust(left=.1, right=.9, wspace=.4, hspace=.4)

plt.show()
```

![](../Images/f95a47ad62448090ef88650407028719.png)

你可以调整比例参数，看看图形如何变化。

现在，我们已经具备了创建复杂图形所需的所有工具，包括使用子图（subfigure）、子图布局（subplots）和网格（grids）。对于这种图形，通常需要用字母标注每个图，以便在说明中解释它们或在文本中引用。通常，这些标注是在图形创建后使用其他软件如Adobe Illustrator或Inkscape完成的。但我们也可以直接在Python中完成这项工作，这样可以节省以后额外的努力。

为此，我们将定义一个函数来进行这些标注：

```py
def letter_annotation(ax, xoffset, yoffset, letter):
 ax.text(xoffset, yoffset, letter, transform=ax.transAxes,
         size=12, weight='bold')
```

该函数将坐标轴作为输入，连同x和y坐标，这些坐标将转换为相对坐标轴坐标。我们可以用它来在我们之前创建的图形中标注一些图：

```py
fig = plt.figure(figsize=(10, 7))
(topfig, bottomfig) = fig.subfigures(2, 1)

topfig.set_facecolor('#cbe4c6ff')
topfig.suptitle('Top')
top_axs = topfig.subplots(2, 4)
letter_annotation(top_axs[0][0], -.2, 1.1, 'A')

(bottomleft, bottomright) = bottomfig.subfigures(1, 2, width_ratios=(1,2))

bottomleft.set_facecolor('#c6c8e4ff')
bottomleft.suptitle('Bottom left')
bottoml_axs = bottomleft.subplots(2, 2)
letter_annotation(bottoml_axs[0][0], -.2, 1.1, 'B')

bottomright.set_facecolor('#aac8e4ff')
bottomright.suptitle('Bottom right')
bottomr_axs = bottomright.subplots(3, 3)
letter_annotation(bottomr_axs[0][0], -.2, 1.1, 'C')

# Spacing between subplots
topfig.subplots_adjust(left=.1, right=.9, wspace=.4, hspace=.4)
bottomleft.subplots_adjust(left=.2, right=.9, wspace=.5, hspace=.4)
bottomright.subplots_adjust(left=.1, right=.9, wspace=.4, hspace=.4)

plt.show()
```

![](../Images/36c2342c620bdafac1e5ed2f1bc31e94.png)

我们现在可以创建文章开头显示的图形。它由三个子图组成。一个位于顶部，占据第一行，两个位于底部。左下角的子图将用于联合图（如前所示），而右下角的子图将定义一个网格规范（gridspec），以放置4个不同大小的子图。

```py
fig = plt.figure(figsize=(10, 7))

# Creating a subfigure for the first and second row
(row1fig, row2fig) = fig.subfigures(2, 1, height_ratios=[1, 1])
# Splitting the bottom row subfigure in two subfigures
(fig_row2left, fig_row2right) = row2fig.subfigures(1, 2, wspace=.08, width_ratios = (1, 2))

# #####
# Row 1 plots
# #####

# Make 4 subplots for the first row subfigure
row1_axs = row1fig.subplots(1, 4)

row1fig.subplots_adjust(wspace=0.5, left=0, right=1, bottom=.16)

ax = row1_axs[0]
sns.histplot(data=data, x='mpg', ax=ax)
ax.set_title('MPG')
# Annotate plotots with letters
letter_annotation(ax, -.25, 1, 'A')
# Some styling for figures to make them look better 
# and have a standardized look
sns.despine(offset=5, trim=False, ax=ax)

ax = row1_axs[1]
sns.histplot(data=data, x='displacement', ax=ax)
ax.set_title('Displacement')
letter_annotation(ax, -.25, 1, 'B')
sns.despine(offset=5, trim=False, ax=ax)

ax = row1_axs[2]
sns.histplot(data=data, x='weight', ax=ax)
ax.set_title('Weight')
letter_annotation(ax, -.25, 1, 'C')
sns.despine(offset=5, trim=False, ax=ax)

ax = row1_axs[3]
sns.histplot(data=data, x='horsepower', ax=ax)
ax.set_title('Horsepower')
letter_annotation(ax, -.25, 1, 'D')
sns.despine(offset=5, trim=False, ax=ax)

# #####
# Row 2 plots
# #####

# ##
# Seaborn jointplot
# ##

# Using code from the Seaborn JointGrid class

# size ratio between the main plots and the margin plots
ratio=2
# Defining a gridspec for inside the subfigure
gs = plt.GridSpec(ratio + 1, ratio + 1)
ax_joint  = fig_row2left.add_subplot(gs[1:, :-1])
# Share axis between the margin and main plots
ax_marg_x = fig_row2left.add_subplot(gs[0, :-1], sharex=ax_joint)
ax_marg_y = fig_row2left.add_subplot(gs[1:, -1], sharey=ax_joint)

# Remove Axis labels and ticklabels for the margin plots
plt.setp(ax_marg_x.get_xticklabels(), visible=False)
plt.setp(ax_marg_y.get_yticklabels(), visible=False)
plt.setp(ax_marg_x.get_xticklabels(minor=True), visible=False)
plt.setp(ax_marg_y.get_yticklabels(minor=True), visible=False)

sns.scatterplot(data=data, y='horsepower', x='mpg', ax=ax_joint)
sns.histplot(data=data, y='horsepower', ax=ax_marg_y)
sns.histplot(data=data, x='mpg', ax=ax_marg_x)

sns.despine(offset=5, trim=False, ax=ax_joint)
sns.despine(offset=5, trim=False, ax=ax_marg_y)
sns.despine(offset=5, trim=False, ax=ax_marg_x)

# Leaving some space to the right to remove overlaps
fig_row2left.subplots_adjust(left=0, right=.8)
letter_annotation(ax_marg_x, -.25, 1, 'E')

# ##
# Row 2 right plots
# ##

gs = plt.GridSpec(2, 3)
ax_left   = fig_row2right.add_subplot(gs[:, 0])
ax_middle = fig_row2right.add_subplot(gs[:, 1])
ax_up     = fig_row2right.add_subplot(gs[0, 2])
ax_down   = fig_row2right.add_subplot(gs[1, 2])

fig_row2right.subplots_adjust(left=0, right=1, hspace=.5)

ax = ax_left
sns.scatterplot(data=data, x='model_year', y='weight', hue='origin', ax=ax)
sns.despine(offset=5, trim=False, ax=ax)
letter_annotation(ax, -.3, 1, 'F')

ax = ax_middle
sns.boxplot(data=data, x='origin', y='horsepower', ax=ax)
sns.despine(offset=5, trim=False, ax=ax)
letter_annotation(ax, -.3, 1, 'G')

ax = ax_up
sns.kdeplot(data=data, x='mpg', y='acceleration', ax=ax)
sns.despine(offset=5, trim=False, ax=ax)
letter_annotation(ax, -.3, 1, 'H')

ax = ax_down
sns.histplot(data=data, x='weight', y='horsepower', ax=ax)
sns.despine(offset=5, trim=False, ax=ax)
letter_annotation(ax, -.3, 1, 'I')

plt.show()
```

![](../Images/d51973be73c935f8aa76c990a9f91967.png)

# 结论

子图是matplotlib中相对较新的概念。它们使得组装包含许多图的大型图形变得简单。本文中展示的所有内容也可以完全通过使用gridpec实现。然而，这需要一个大型网格，并且需要考虑每个子图的大小。子图更像是即插即用的，且用更少的工作就可以达到相同的效果。

对我来说，子图是创建科学图形的一个非常方便的工具，我希望它们对你也能有所帮助。

你还可以在GitHub上找到本文中的所有代码：[https://github.com/tdrose/medium-articles-code](https://github.com/tdrose/medium-articles-code)

除非另有说明，所有图像均由作者创建。
