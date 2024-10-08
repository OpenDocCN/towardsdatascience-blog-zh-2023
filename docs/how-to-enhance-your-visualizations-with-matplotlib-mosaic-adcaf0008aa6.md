# 如何通过 Matplotlib 马赛克提升你的可视化效果

> 原文：[`towardsdatascience.com/how-to-enhance-your-visualizations-with-matplotlib-mosaic-adcaf0008aa6`](https://towardsdatascience.com/how-to-enhance-your-visualizations-with-matplotlib-mosaic-adcaf0008aa6)

## 我从 Matplotlib 学到的最酷的一个方法

[](https://gustavorsantos.medium.com/?source=post_page-----adcaf0008aa6--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----adcaf0008aa6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----adcaf0008aa6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----adcaf0008aa6--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----adcaf0008aa6--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----adcaf0008aa6--------------------------------) ·阅读时间 5 分钟·2023 年 6 月 8 日

--

![](img/6fa68ce84e5bfd42fc18ef09855c0ba9.png)

微笑吧！马赛克子图来了！图像由作者创建。

# 介绍

数据可视化非常重要。我们可以仅从这句话中发展出很多内容，但我相信你已经明白了“大致情况”。我相信这不是你第一次听到这些。即使是，假设这也不会让你感到惊讶，对吧？

> 一张图胜过千次观察。

如果*一张图片胜过千言万语*，我会说，对数据科学而言，*一张图胜过千次观察*。

无论如何，让我们转到这里的重要内容。这个简短的文章的目标是向你展示`subplot_mosaic()`方法。这个功能真的很棒。最近我在浏览和阅读一些数据科学相关的内容时看到了它。我惊讶于在同一图中快速创建多个图的容易程度。

让我们在下一部分中看看它是如何完成的。

将使用的数据集是 Python 中 Seaborn 包原生的*Tips*数据集。

```py
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Data
df = sns.load_dataset('tips')
```

# 马赛克

在马赛克图之前，还有其他方法可以创建子图。这些方法并不难，但它们没有`subplot_mosaic()`方法那样灵活。你可以阅读下一篇文章来了解我在说什么。

[](/plot-multiple-graphics-in-the-same-figure-using-python-1cbe5a23e89f?source=post_page-----adcaf0008aa6--------------------------------) ## 使用 Python 在同一图中绘制多个图形

### 学习如何在同一 matplotlib 图中绘制多个图形

towardsdatascience.com

马赛克方法给我们带来的巨大贡献是灵活性，可以用很少的代码创建许多设计。看看下一个图像：如果我们现在要在 matplotlib 中重现它，我们将使用马赛克函数。

请注意，我们有 3 个图。**图 A**将在左上方占据一个位置。**图 B**将在图形的左下方占据一个位置。**图 C**将在右侧的上方和下方占据两个位置。因此，现在我们只需将其转换为 Python，你会发现这真的很简单。

![](img/b4258bff75396fb8b6ae7a14ddf23d65.png)

要在 matplotlib 中创建的马赛克图。图片由作者提供。

让我们来看看代码。我们首先创建一个图形，然后创建一个马赛克变量。马赛克变量可以接受列表或字符串。我认为使用三重引号的字符串更容易，它能“绘制”出最终图形的样子。请注意，代码中`ac bc`的使用顺序和形状与之前的图形相同。通过在右侧重复两次`c`，我告诉 matplotlib 我希望图 C 占据图形的那两个位置。其余的代码只是常规的图形创建代码。

如果你查看变量`mosaic`，你会看到这个：`{‘a’: <Axes: label=’a’>, ‘c’: <Axes: label=’c’>, ‘b’: <Axes: label=’b’>}`。所以，它创建了一个字典，我们现在只需指定每个图形的位置。

```py
# Plot with Mosaic
fig = plt.figure(layout= 'constrained')
mosaic = fig.subplot_mosaic('''
                            ac
                            bc
                            ''')

# Plot A
mosaic['a'].bar(df.sex, df.tip, color='coral')

# Plot B
mosaic['b'].scatter(df.total_bill, df.tip, color='forestgreen')

# Plot C
mosaic['c'].boxplot(df.tip, patch_artist=True);
```

这是生成的可视化效果。

![](img/5ae8ac4d3ace6331890d1f68e15f38ae.png)

上一个代码生成的马赛克图。图片由作者提供。

好的。我希望我已经引起了你的注意。

我们可以创建不同的布局。看看这个。

```py
# Plot with Mosaic
fig = plt.figure(layout= 'constrained', figsize=(12,6))
mosaic = fig.subplot_mosaic('''
                            aaa
                            bcc
                            ''')

# Plot A
mosaic['a'].bar(df.day, df.tip, color='coral')

# Plot B
mosaic['b'].boxplot(df.total_bill, patch_artist=True)

# Plot C
mosaic['c'].scatter(df.total_bill, df.tip, color='forestgreen');
```

![](img/0202910a94b094e3504c104ca7ded5c3.png)

上一个代码生成的马赛克图。图片由作者提供。

或者，这里有另一个例子，包含更多的图形。

```py
# Plot with Mosaic
fig = plt.figure(layout= 'constrained', figsize=(10,6))
mosaic = fig.subplot_mosaic('''
                            ab
                            ac
                            de
                            ''')

# Plot A
mosaic['a'].bar(df.day, df.tip, color='coral')

# Plot B
mosaic['b'].boxplot(df.total_bill, patch_artist=True)

# Plot C
mosaic['c'].scatter(df.total_bill, df.tip, color='forestgreen')

# Plot D
mosaic['d'].scatter(df.tip, df.total_bill, color='purple')

# Plot E
mosaic['e'].scatter(df['size'], df.tip, color='gold');
```

![](img/312c1abee08d2fe959b9fa14af234261.png)

上一个代码生成的马赛克图。图片由作者提供。

## 设置标题

但你可能会问自己：*那图表标题呢？*

好问题，我会说！让我们学习如何设置标题。

第一个选项是最简单的：我们可以通过一行额外的代码为整个图像添加一个标题。

```py
# Adding a single title to the mosaic

plt.suptitle('''        -- P L O T S --
                - Top Left: Sum of tips by Sex -
                - Bottom Left: Tip by Total Bill -
                - Right: Boxplot of Tips - ''')
```

![](img/8a6adfdbe148b180d8710e327973ec1c.png)

带有单一标题的马赛克图。图片由作者提供。

另外，我们可能希望为每个图添加一个标题。因此，让我们创建一个循环，遍历每个坐标轴和标题列表，并逐一匹配它们。如果我们调用`mosaic.items()`，我们将看到一个包含元组的字典，如前所示：`（‘a’，<Axes: label=’a’>）`。要设置标题，我们需要访问`Axes`部分——元组中的第二项——因此在代码片段中我们使用`ax[1]`。

这就是如何实现它的。

```py
# Define Titles
titles = ['Sum of tips by Sex', 'Tip by Total Bill', 'Boxplot of Tips']

# One title per plot
for ax, g_title in zip(mosaic.items(), titles):
    ax[1].set_title(g_title, fontstyle='italic')
```

这就是结果。

![](img/4a20ecc02d6f96e5c75fa0f25c47dcb4.png)

每个图都有一个标题的马赛克图。图片由作者提供。

很不错，不是吗？

# 在你离开之前

我相信在一个图形中绘制多个图形对于比较和展示是有帮助的。而且拥有一种简化操作的方法能够带来很大的不同。

记住这些步骤

1.  创建图形

1.  使用 `subplot_mosaic()` 创建一个马赛克变量，并使用‘’’三重引号‘’’ “绘制”你的马赛克。根据需要多次写入每个图形标签以填充马赛克中的位置。

1.  使用 `plt.suptitle()` 或循环添加标题。

就这些了。

如果你喜欢这个内容， [关注我获取更多信息，点击这里。](https://gustavorsantos.medium.com/)

在 [LinkedIn](https://www.linkedin.com/in/gurezende/) 上也可以找到我。如果你考虑订阅 Medium 阅读我的帖子以及更多内容，可以考虑使用这个推荐代码，因为这会激励我继续创作内容。

[](https://medium.com/@gustavorsantos/membership?source=post_page-----adcaf0008aa6--------------------------------) [## 使用我的推荐链接加入 Medium - Gustavo Santos

### 阅读 Gustavo Santos 的每一个故事（以及 Medium 上其他成千上万的作家的故事）。你的会员费直接支持……

medium.com](https://medium.com/@gustavorsantos/membership?source=post_page-----adcaf0008aa6--------------------------------)

# 参考

[## matplotlib.pyplot.subplot_mosaic - Matplotlib 3.7.1 文档

### 基于 ASCII 艺术或嵌套列表构建 Axes 布局。这是一个帮助函数，用于构建复杂的 GridSpec 布局……

matplotlib.org](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplot_mosaic.html?source=post_page-----adcaf0008aa6--------------------------------) [](https://matplotlib.org/stable/gallery/text_labels_and_annotations/label_subplots.html?source=post_page-----adcaf0008aa6--------------------------------#sphx-glr-gallery-text-labels-and-annotations-label-subplots-py) [## 标记子图 - Matplotlib 3.7.1 文档

### 标记子图相对简单，方法有所不同，因此 Matplotlib 没有通用的方法来进行……

matplotlib.org](https://matplotlib.org/stable/gallery/text_labels_and_annotations/label_subplots.html?source=post_page-----adcaf0008aa6--------------------------------#sphx-glr-gallery-text-labels-and-annotations-label-subplots-py)
