# PandasGUI — 轻松数据分析的终极秘密

> 原文：[`towardsdatascience.com/is-pandas-easy-to-use-try-this-tool-to-make-it-easier-2071eeffe482`](https://towardsdatascience.com/is-pandas-easy-to-use-try-this-tool-to-make-it-easier-2071eeffe482)

![](img/e61660035f8758fff18ff6b8aab08a9f.png)

图片由 [Alan Frijns](https://pixabay.com/users/alanfrijns-16705522/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7897513) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7897513)

## PandasGUI 的数据分析实用概述

[](https://christophertao.medium.com/?source=post_page-----2071eeffe482--------------------------------)![Christopher Tao](https://christophertao.medium.com/?source=post_page-----2071eeffe482--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2071eeffe482--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2071eeffe482--------------------------------) [Christopher Tao](https://christophertao.medium.com/?source=post_page-----2071eeffe482--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2071eeffe482--------------------------------) ·阅读时间 7 分钟·2023 年 4 月 14 日

--

数据分析已成为各个行业不可或缺的一部分，因为它使我们能够根据收集的数据做出明智的决策。Python 中最受欢迎的数据分析库之一是 Pandas，它提供了强大的数据操作和清理工具。然而，使用 Pandas 有时可能会感到不知所措，特别是对于那些刚接触数据分析或偏好更直观方法的人来说。这时，PandasGUI 就派上用场了 — 这是一个将图形用户界面引入 Pandas 的库，使数据操作和可视化变得更加易于访问和用户友好。

在本文中，我们将详细了解 PandasGUI 及其功能，指导您完成安装过程，并展示其能力。

# 1\. 安装与启动

![](img/6d35855288306933da3883ec39c872ad.png)

图片由 [traditional chinese medician · 素君](https://pixabay.com/users/%E7%B4%A0%E5%90%9B-21058920/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7869586) 提供，来源于 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7869586)

首先，我们需要安装 PandasGUI。和往常一样，我们可以使用 `pip` 来安装它。

```py
pip install pandasgui
```

## 1.1 非 Windows 操作系统的小问题

本节适用于使用非 Windows 操作系统的用户，如果您实际使用的是 Windows 操作系统上的 Python，则可以跳过此步骤。

看起来作者是在 Windows PC 上创建了这个库，因此假设操作系统会有一个环境变量 `APPDATA`。但是，对于其他操作系统如 Mac 或 Linux，这种情况并非如此。具体来说，当我们尝试导入 PandasGUI 时，会出现这个错误。

```py
import pandas as pd
import pandasgui
```

![](img/fb7dd46aabedf241e7a8b537e38391f8.png)

修复此问题的最简单方法是手动为该环境变量指定一个空字符串。

```py
import os
os.environ['APPDATA'] = ""
```

然后，我们将能够毫无问题地使用 PandasGUI。

![](img/6a261ff34f45f4cff19394cc3017f112.png)

警告信息是正常的。我猜它没有实现一些 Mac OS 中推荐的接口，因此我的系统给出了这个警告。

## 1.2 加载示例数据集

为了演示这个库，我们需要使用一个示例数据集。如果你是一名数据科学家，你可能对 Iris 数据集很熟悉，它在许多分类或聚类机器学习演示中被使用。

我们从 Datahub.io 获取数据集。它是一个用于发现、共享和发布来自各种来源的高质量开放数据集的平台。这里的大多数数据集是开源的，并且可以根据许可证用于学习目的，包括 Iris 数据集。

```py
df = pd.read_csv("https://datahub.io/machine-learning/iris/r/iris.csv")
df.head()
df.shape
```

![](img/7eff0db04da1cb2a6f8a6f88d5086c96.png)

## 1.3 启动 PandasGUI

现在，让我们极其简单地启动 PandasGUI。只需按如下方式调用 `show()` 函数即可。

```py
pandasgui.show(df)
```

![](img/79ddb9735dc21f841b263c5175d1928e.png)

不用担心关于缺失字体家族的警告，这再次是由操作系统造成的。指定的字体家族在我的 Mac OS 上不存在。这不影响我们使用 GUI。

运行这一行代码后，GUI 应该会以桌面应用程序的形式弹出。

![](img/9d54acc2c01ed3d7a4760cd390071626.png)

# 2\. PandasGUI 的功能

![](img/822198047bd13dfb76e1be28a297f459.png)

图片由 [Adrian](https://pixabay.com/users/qgadrian-317687/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4461766) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4461766)

UI 相当直观。它由以下组件组成。我将在后续小节中介绍它们。

+   **DataFrame 列表** — 我们可以在这里导航和切换数据帧。它还显示了数据帧的形状以方便查看。

+   **过滤查询** — 创建和选择查询表达式以过滤当前数据帧

+   **列列表** — 查看和导航当前数据帧的列

+   **功能标签** — 切换标签以导航不同的工具

+   **主要区域** — 显示当前操作的结果

![](img/6cca06479ca2c9317102d39fbe103720.png)

## 2.1 过滤数据帧

我想介绍的第一个功能是过滤。它依赖于 DataFrame 查询表达式来快速为我们过滤数据帧。

具体来说，我们只需输入类似`sepallength > 7`的查询并按回车。过滤器将应用于数据框。我们可以在主区域查看过滤后的结果。

![](img/6cc5dffffabd692de8c63f2ed5eed2b4.png)

如果我们想回到查看整个数据框，我们可以取消勾选表达式以移除过滤器。

![](img/403403caf693fa9a11604dfd25d364da.png)

此外，还可以添加许多查询表达式并使用复选框灵活应用它们。例如，下面的截图显示了两个已选中的表达式，它们都用于过滤数据框。

![](img/4c7c10326bf5bcdc133fc7a389971c88.png)

## 2.2 排序、类型转换和颜色编码

在数据框主区域，我们还可以轻松地完成许多类似 Excel 的操作，如排序和颜色编码。除此之外，我们还可以轻松地转换列的数据类型。

![](img/fdf04c00c5d247875512a1d64920aa57.png)

例如，下面的截图显示了数据框按`sepalwidth`列的降序排序，并且数值列根据其值的范围进行了颜色编码。

![](img/916feef9146a97e2ff4c2d66bae88721.png)

## 2.3 统计信息

在第二个特征标签中，我们可以看到该数据框的统计信息。

![](img/29bc41d94cc62a8a1942ae308d15c098.png)

还值得一提的是，我们也可以在左侧选择查询表达式。然后，统计信息将基于过滤后的数据框重新计算。

![](img/6c3cc0083cf2ea0d83db8558f813bf68.png)

## 2.4 绘图

我不得不说，当我们想使用代码绘制图表时，Python 是最简单的语言之一。然而，我们毕竟还是要写一些代码。

在 PandasGUI 中，我们可以在几秒钟内使用其列绘制数据框。例如，下面的演示显示了我只需切换到“绘图器”标签并选择“散点图 3D”。然后，将一些列拖到轴字段中。

![](img/e120e433f729ad4349b6f1c64afbc0dd.png)

如果我们想切换到其他类型的图表，也不需要花费多少时间。这实际上让我们可以快速测试不同类型的图表，并决定哪种图表能更好地讲述数据故事。

## 2.5 重塑数据框

我们还可以使用 PandasGUI 通过拖放重塑数据框。例如，我们可以通过将“class”转换为列来透视 Iris 数据框，然后计算每个属性的平均值，例如花瓣长度。

![](img/807a3b1984a40ae15097036fcf31d954.png)

拖动列后，点击“完成”按钮。将生成如下的新数据框。

![](img/af2c34d3311a23210cab2ebffbab9a0a.png)

## 2.6 生成代码

对于上述大多数功能，PandasGUI 也可以为我们生成代码。当我们使用 GUI 决定哪种图表最好时，这非常有用，然后可以轻松生成代码并将其放入实际脚本中。

![](img/4cc4f0b6b61d11381f1050933104486b.png)

类似地，重塑功能也提供了代码导出功能。它允许我们多次实验重塑，然后输出正确的代码。

好吧，我们可能可以在 ChatGPT 中完成这个，但需要解释很多内容，并将其应用到我们的背景中 :)

# 总结

![](img/d01c5c561af13e4b1134be77ed83c905.png)

图片由[Elias](https://pixabay.com/users/sch%C3%A4ferle-3372715/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2040340)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2040340)。

总结来说，本文深入探讨了 PandasGUI 的各种功能，这是一种强大的库，为广泛使用的数据处理和可视化的 Pandas 库带来了图形用户界面。我们演示了安装过程，加载示例数据集，并探索了如筛选、排序、统计分析、绘图、重塑和代码生成等功能。

PandasGUI 是一个宝贵的工具，可以通过简化常见任务和提供互动体验来显著增强你的数据分析工作流。尽管它极大地便利了初学者和经验丰富的数据科学家的数据处理，但需要注意的是，它可能不支持极其复杂的操作。对于高级操作，可能需要依赖传统的 Pandas 脚本。

[](https://medium.com/@qiuyujx/membership?source=post_page-----2071eeffe482--------------------------------) [## 通过我的推荐链接加入 Medium - Christopher Tao

### 感谢阅读我的文章！如果你不介意的话，请请我喝杯咖啡 :) 你的会员费用支持成千上万的…

medium.com](https://medium.com/@qiuyujx/membership?source=post_page-----2071eeffe482--------------------------------)

**如果你觉得我的文章有帮助，请考虑加入 Medium 会员，以支持我和成千上万的其他作者！（点击上面的链接）**

> *除非另有说明，否则所有图片均由作者提供*
