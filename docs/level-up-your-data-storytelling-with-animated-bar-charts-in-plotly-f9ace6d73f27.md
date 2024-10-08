# 在 Plotly 中提升你的数据讲述能力，使用动画柱状图

> 原文：[`towardsdatascience.com/level-up-your-data-storytelling-with-animated-bar-charts-in-plotly-f9ace6d73f27`](https://towardsdatascience.com/level-up-your-data-storytelling-with-animated-bar-charts-in-plotly-f9ace6d73f27)

## 将静态图表转变为引人入胜的叙述

[](https://brian-mattis.medium.com/?source=post_page-----f9ace6d73f27--------------------------------)![Brian Mattis](https://brian-mattis.medium.com/?source=post_page-----f9ace6d73f27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9ace6d73f27--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9ace6d73f27--------------------------------) [Brian Mattis](https://brian-mattis.medium.com/?source=post_page-----f9ace6d73f27--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9ace6d73f27--------------------------------) ·6 分钟阅读·2023 年 12 月 2 日

--

![](img/4e12d61700b107cf6c44cc6091ebec87.png)

图片来源：[Teemu Paananen](https://unsplash.com/@xteemu?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Plotly 支持动画图表的**优秀基础**。我强烈推荐他们的基础教程 [这里](https://plotly.com/python/animations/)。然而，plotly 动画主要是为了给可视化添加另一个维度——通常是时间。这对为图表增添更多意义非常棒。

然而，*动画不必用于增加图表的复杂性*。利用动画进行**强调**是非常强大的。当我们为观众准备好关键图表时，我们希望吸引他们的注意，而不是明确地喊出“这是你应该特别关注的图表！”我们的视线自然会被移动的东西吸引，动画图表可以在某种程度上营造出期待感。你的观众会参与其中，试图实时预测下一条柱状图或线条。如果关键业务决策依赖于这个图表——你的观众会屏息以待，眼睁睁看着结果展开！

为了更好地理解——想象一下你已经看了同事的演示文稿 20 分钟——哪个图表吸引了你的注意？这个简单的（pandas 生成的）图表：

![](img/9f8ff96c7540f05c923ad44140386e00.png)

基本柱状图——作者提供的图像

或者这张相同数据的动画图表：

![](img/ffbeeee8afa37382e4c7e854e7600139.png)

自定义 Plotly 动画柱状图——作者提供的图像

# 在 Plotly 中构建

要在 Plotly 中实现这一点，我们实际上需要稍微重新排列数据。同样，Plotly 动画的设置是按某种顺序（通常是时间）进行的。因此，我们将扩展数据以有效地为每个数据点制作一个帧。来吧：

首先，让我们导入 plotly 包（如果还没有安装，请使用 pip 安装），并创建一个虚拟数据集进行操作：

要直接使用 plotly 的动画，我们需要将数据集增强为以下内容：

![](img/73e28d4e10301c794f68cd18faf1f64d.png)

动画条形图的数据转换 — 图片来自作者

我们需要有效地堆叠数据框的副本，副本的数量与原始数据框的长度相同。对于每个副本，我们需要添加一个 Frame 值来设置其在动画中的序列。问题在于，我们希望每一步序列中出现更多的条形图。我们希望在绘图中开始时不显示任何条形图，因此对于第一个 Frame，我们将所有值设置为 0。然后，在第二个 Frame 上，我们将所有值设置为 0 ***除了*** 第一个数据点（A: 10）。这个方法一直持续到最后一个 Frame（在这种情况下是 Frame #6）显示我们原始数据中的所有值。

到此为止，我们已经为 Plotly 设置了单独的“Frame”元素，以便平滑地动画过渡。请注意，我们**没有**为动画编码每一帧，我们只是逐个快照每个 Label 的最终值，让 Plotly 填补空白。现在我们只需要一个辅助函数，这样我们就可以将这个转换概念应用于任何简单的数据框：

数据框堆叠和排序函数

在这段代码中，我们首先创建一个空白的目标数据框，它将作为返回的输出。我们循环遍历传入数据框中的元素数量 — `range(len(starting_df)+1)`。我们在这里加了 1，因为第一个 Frame 将所有值都置为零，以便我们的图开始时为空。在循环内部，我们从原始数据框的深拷贝开始。这意味着我们对这个副本所做的更改不会传递回原始数据框。在这个干净的副本上，我们添加一个 Frame 值，并根据我们所在的循环次数将一些数据点置为零。最后，我们可以将这个副本添加到我们正在构建的最终数据框的末尾。

# 绘图

从这里开始我们准备开始绘图。如果我们只是使用默认的 plotly 函数，它看起来会像这样：

![](img/390f7cfac691f0ebb596a05707ad2729.png)

默认绘图设置 — 图片来自作者

正如你所见，开始时有点怪异。这主要与绘图如何自动缩放 y 轴以适应数据有关。这并不特别令人惊讶，因为在它第一次绘制时（Frame=0），所有值都是零。除此之外，序列的速度可能不是你想要的。也许你想让条形图绘制得慢一点？也许在绘制条形图之间有一个暂停会很好？这些都是可调整的参数。最好的部分是，使用 plotly，我们甚至不需要条形图线性增长 — 你可以使用一系列的过渡设置。为了将这个图形清理成我想展示的样子，我将调整：

+   Y 轴范围（取数据框中的最大值并加上 10%）

+   图形大小和纵横比

+   每帧的持续时间和帧之间的时间

+   动画移动本身（过渡）

![](img/b5aaa768847c84bedf9fe3ceb36050b4.png)

使用“quad-out”动画格式化的动画条形图——作者提供的图像

在这种情况下，我使用了“quad-out”过渡效果，但还有很多可以选择的效果，这些效果会影响可视化效果。可以查看文档 [这里](https://plotly.com/python/reference/layout/sliders/#layout-sliders-items-slider-transition-easing) 并尝试几种效果——特别是“bounce”效果相当有趣（尽管可能不是最专业的）。还有许多可以调整的 [设置](https://plotly.com/python/reference/layout/sliders/)，可以用来修改或移除图表下方的按钮，不过这可能取决于你导出图表的方式，有时可能并不必要。

# 导出为 GIF

现在有个坏消息——Plotly 没有内置的方法来提取这些动画。你现在需要做一些变通的方法。有些包可以用来提取成电影文件，如 `moviepy`，但如果我们要将其放入演示文稿或基于网页的客户仪表板中，这可能有些过于复杂。我们真正需要的是一个动画 GIF。虽然使用 Plotly 的 graph_objects 提取动画帧为 PNG 并使用其他包在线生成 GIF 是一个选项，但这超出了本帖的范围。现在，可以使用基本的 GIF 屏幕捕捉软件在 Jupyter Notebook 窗口上进行操作。这里有很多选择——[这里](https://www.screentogif.com/) 有一个免费的软件对我有效。

## 额外加分

当然，这个序列是为了从左到右绘制条形图。如果我们想要相反的效果，帧#1 的 F 值将为 16，而 A 值仍为 0。这在绘制折线图时可能会很有用，其中多条线可以从图表的不同侧面生长。🤯

# 结论

我希望你能看到添加一点动画如何使简单的图表对观众更具影响力。通过使用这个相对简单的功能，我们可以快速而轻松地将静态图表转换成极具吸引力的图像。当然，这只是一个开始！正如我频道的粉丝所知道的，我喜欢用有意义的 [条形颜色渐变](https://medium.com/towards-data-science/beautiful-bars-scaled-gradient-fill-on-bar-plots-e4da4cdae033) 和 [背景过渡](https://medium.com/towards-data-science/custom-matplotlib-colormaps-for-danger-zone-plots-62310983eb67) 来直观地向观众传达更多信息。更多内容即将到来。

和往常一样，整个代码演示笔记本可以 [从我的 github 获取](https://github.com/bamattis/Blog/tree/main/Plotly_animation)。***如果你觉得这有用，请关注我！*** 祝好，编码愉快。
