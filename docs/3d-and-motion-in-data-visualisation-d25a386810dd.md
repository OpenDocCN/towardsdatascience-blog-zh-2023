# 数据可视化中的 3D 和动效

> 原文：[`towardsdatascience.com/3d-and-motion-in-data-visualisation-d25a386810dd`](https://towardsdatascience.com/3d-and-motion-in-data-visualisation-d25a386810dd)

![](img/6db0891546a06ab70a9125be5acba83a.png)

数据可视化、Python 和 3D 创作软件，完美的融合。来源：作者

## 许多好莱坞特效背后的开源工具如何帮助创建令人惊叹的数据可视化

[](https://medium.com/@thejoshtaylor?source=post_page-----d25a386810dd--------------------------------)![Josh Taylor](https://medium.com/@thejoshtaylor?source=post_page-----d25a386810dd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d25a386810dd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d25a386810dd--------------------------------) [Josh Taylor](https://medium.com/@thejoshtaylor?source=post_page-----d25a386810dd--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d25a386810dd--------------------------------) ·3 分钟阅读·2023 年 1 月 20 日

--

数据可视化领域已经非常成熟，拥有一些出色的文献[1,2]，指导如何以视觉方式传达数据。

不幸的是，随着这种成熟度的提高，带来了平淡。如今的大多数可视化都是单调的，原始性被摒弃，采用经过验证的方法。

当我们有证据表明，使用条形对比长度是最有效的数据表示方式时[3]，我们如何避免一切都变成条形图呢？

![](img/3b808497c8c2caef44711a25877fceac.png)

有用的数据可视化备忘单。来源：作者

## 为数据可视化注入兴奋感

在我之前的两篇文章中，我讨论了为什么我们应该使用动画和[3D](https://medium.com/towards-data-science/when-to-use-3d-in-data-visualisation-f3739e320da7)在数据可视化中的应用。

两种方法的有力论点是需要为一个应该引发兴趣和刺激的领域注入兴奋感。创造新的、不同的和原创的东西是关键。真正的原创性很难[4]，但对数据可视化的持续发展至关重要。

在这篇文章中，我们将覆盖一个实际示例，展示如何实现这一点。我们将从电影行业获取灵感；将特效工具和数据结合在一起，创造出不同的东西。

下面的示例可以看到。它比较了基于 GitHub 活动的编程语言的变化。创建此图表的代码在本文末尾提供。

首次(?) 3D 蝴蝶图表比赛的视频。来源：作者

但首先，让我们更详细地探讨一下我们将使用的工具。

## Blender 简介

Blender 是一个免费的开源 3D 创作套件。它可以用于创建各种计算机生成的内容，包括多个[大片电影](https://inspirationtuts.com/blender-movies-top-movies-made-with-blender-3d/)的特效。

Blender 在数据科学和数据可视化方面特别有趣，因为它拥有**一个完整的 Python API**，可以用于程序化地构建可视化。

这开启了几乎无限的可能性。单个 Python 工作流可以用于处理数据，然后使用 Blender 根据这些数据创建强大、逼真的 3D 渲染！

你可以在其网站上找到更多信息以及下载 Blender，[blender.org](https://www.blender.org/)。

## 一个简单的示例

虽然完整的教程超出了本文的范围，但以下内容展示了如何轻松入门。

它基于 Python 列表创建一系列立方体，并将这些立方体放置在场景中，并添加一个光源：

少于 15 行代码的条形图

经过一些小的调整，Blender 可以渲染出下面的条形图，代表上面脚本中的数据列表：

![](img/828ed51fdfb4a007e7230b80e541feee.png)

只需几行代码即可生成条形图。来源：作者

Blender 网站上提供了[全面的文档](https://docs.blender.org/api/current/info_quickstart.html)，允许你轻松在此示例基础上进行扩展。如前所述，实际上没有什么限制，因为一切都可以通过 Python API 来控制。这意味着你可以：

+   通过设置关键帧创建动画。

+   设置一个场景，完全控制光照、颜色和材质。

+   确定相机位置并控制其随时间的移动。

有了这些考虑，创建上面视频中展示的“蝴蝶竞赛”图表相对简单。

可以在此[找到完整的脚本和数据](https://github.com/joshua-taylor/butterfly_race)。

## 结论

希望这篇文章能激发一些灵感，帮助我们摆脱数据可视化的现状，通过借鉴数据科学以外的技术和工具，为内容注入兴奋感和原创性。

如果你对数据可视化的原创方法有其他想法，请在评论中添加。

**参考文献：**

1.  《定量信息的视觉展示》。爱德华·R·塔夫特，1983 年

1.  《功能艺术：信息图形和可视化导论》。阿尔贝托·卡伊罗，2011 年

1.  [科学数据的图形感知和图形方法](https://courses.ischool.berkeley.edu/i247/f05/readings/Cleveland_GraphicalPerception_Science85.pdf)。威廉·S·克里夫兰；罗伯特·麦吉尔，1985 年

1.  [结构性想象：类别结构在示例生成中的作用](https://www.sciencedirect.com/science/article/abs/pii/S0010028584710103?via%3Dihub=)。沃德·T·B，1994 年
