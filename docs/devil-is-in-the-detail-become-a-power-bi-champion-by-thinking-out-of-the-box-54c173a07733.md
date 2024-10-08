# 细节决定成败：通过跳出框框思维成为 Power BI 大师

> 原文：[`towardsdatascience.com/devil-is-in-the-detail-become-a-power-bi-champion-by-thinking-out-of-the-box-54c173a07733`](https://towardsdatascience.com/devil-is-in-the-detail-become-a-power-bi-champion-by-thinking-out-of-the-box-54c173a07733)

## Power BI 中充满了“无名英雄”！其中之一，分析面板，结合更改可视化类型，帮助我显著提升了 Power BI 报告的性能

[](https://datamozart.medium.com/?source=post_page-----54c173a07733--------------------------------)![Nikola Ilic](https://datamozart.medium.com/?source=post_page-----54c173a07733--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54c173a07733--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----54c173a07733--------------------------------) [Nikola Ilic](https://datamozart.medium.com/?source=post_page-----54c173a07733--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----54c173a07733--------------------------------) ·阅读时间 5 分钟·2023 年 7 月 7 日

--

![](img/f684b4174148291c4fbc04b52f8c0e21.png)

[照片由 Alice Dietrich 提供，来源于 Unsplash](https://unsplash.com/de/fotos/FwF_fKj5tBo)

几周前，我正在为我的一个客户进行[Power BI 报告的性能调优](https://data-mozart.com/mastering-dp-500-implement-performance-improvements-in-power-query-and-data-sources/)。报告页面的渲染非常缓慢（超过 15 秒）。为了给你一点背景：该报告使用了与托管在 SSAS Tabular 2016 中的表格模型的实时连接。

# 如果我告诉你，我在没有更改任何 DAX 代码的情况下，将报告页面的性能提升了两倍，你会如何反应？

继续阅读，你会发现为什么细节往往隐藏着问题，以及跳出框框思维如何帮助你成为真正的 Power BI 大师:)

![](img/1e6d958a420a66586a912bcac7d23c93.png)

图片来源：作者

让我快速解释上面的插图。这里有一个折线图和簇状柱形图，其中四条线代表用户从左侧的切片器中选择（报告阈值和三个层次），而柱形图则是总销售额。数据按年份和产品进行细分。每条线都使用 DAX 计算（顺便提一下，SSAS 2016 中没有[SELECTEDVALUE 函数](https://data-mozart.com/display-selected-slicers-in-power-bi/)）。

我已启用性能分析器，获取了 DAX 查询并在 DAX Studio 中执行了它：

![](img/70ab7ff401b4127da2b08fbab7ebbb25.png)

图片来源：作者

如你所见，查询执行时间为 13.5 秒（在运行前清除缓存），而大部分时间花费在[公式引擎](https://data-mozart.com/vertipaq-brain-muscles-behind-power-bi/)中（76%）。这很重要，因为我们将把这个结果与改进后的报告页面进行比较。

那么，一个有经验的 Power BI 开发人员会怎么做来优化这个场景？重写 DAX？错误！

# 让我们看看在不更改 DAX 逻辑的情况下可以做些什么！

如果你不知道，Power BI 提供了一个被低估的功能，或者我们可以称之为“无名英雄”，即分析面板。

![](img/dc45c4bc2ba263747e3e84e43bd0674b.png)

作者提供的图片

显然，我们中的大多数人将 Power BI 开发时间花在了另外两个面板——数据和格式上。所以，你会惊讶于有多少 Power BI 开发人员甚至不知道这个第三个面板，或者即使知道，也很少使用它。

不深入细节，这个面板让你可以向你的可视化中添加额外的分析成分——如最小值、最大值、平均值、中位数线、误差条等。根据可视化的类型，并非所有选项都可用！在我的使用案例中，这一点非常重要。

当我打开分析面板时，只能看到误差条：

![](img/8171688838a9b45cfccf5504e8b01a2c.png)

作者提供的图片

本质上，这里的想法是，由于这四条线的值不会根据可视化中的数字变化（它们的值是基于切片器选择的常量值），因此利用分析面板中的常量线功能。由于在折线图和聚合柱状图中没有常量线选项，我们将复制我们的可视化并将其类型更改为常规的聚合柱状图。

![](img/539e16094ec6dbc6608e832b31eff3a8.png)

作者提供的图片

如你所见，“数字”在这里，但我们的线条却不见了。让我们切换到分析面板，创建 4 条常量线，每条基于切片器选择生成的 DAX 度量：

![](img/9b9bb7f5120dec3d92f2a9ec83c6a875.png)

作者提供的图片

第一步是添加一条常量线。接下来，展开“折线”属性，并选择“fX”按钮，允许你基于表达式（在我们这里是由 DAX 度量生成的表达式）设置常量线的值。对所有四条线重复此过程。

***我再提醒你一次，请注意我完全没有触及 DAX 代码！***

一旦我关闭了 Y 轴，这就是我的“副本”可视化的样子：

![](img/a7c290be769629e7369a7eb3d6be1091.png)

作者提供的图片

基本上是一样的，对吧？

好的，现在让我们检查一下这个可视化的性能：

![](img/bc5b43fd15f309e59815037967229cc0.png)

作者提供的图片

比原始版本快了超过 5 秒！如果你仔细对比之前的 DAX Studio 截图，你会注意到 [存储引擎查询](https://data-mozart.com/inside-vertipaq-compress-for-success/) 的数量与之前的情况完全相同（而 SE 时间几乎相同），这意味着存储引擎需要完成的工作量完全相同以检索数据。

关键区别在于公式引擎的时间——与原始可视化中 75% 的总查询时间花费在 FE 上不同，这次减少到不到 60%！

我很好奇为什么会发生这种情况，以及公式引擎生成的两个查询计划之间的主要区别是什么。

![](img/d3e7af005949bd5a51f0438276cb0487.png)

较慢查询——DAX 代码部分

![](img/de6c1f4141f7af3f572cd664221291c0.png)

较快查询——DAX 代码部分

两个查询计划之间的“唯一”区别——在较慢的版本中，创建了两个虚拟表：一个用于计算可视化中的“列”值（_ScopedCoreI0），另一个用于计算同一可视化中的行值（_ScopedCoreDM0）。最后，这两个表使用 NATURALLEFTOUTERJOIN 函数连接。

在更快的版本中，没有计算行值的第二个表。此外，计算行值的度量值被包装在 [IGNORE](https://dax.guide/ignore/) 函数中，该函数标记 SUMMARIZECOLUMNS 表达式中的度量值以便从非空行的评估中省略。

# 结论

正如你所见，更改可视化类型，加上“无名英雄”分析面板的使用，确保了在这种情况下显著的性能提升。人们常说“魔鬼在细节中”绝非偶然——因此，往往跳出框框思考会带来一些创新的解决方案。

感谢阅读！

[成为会员并支持 Medium 上的成千上万位作者！](https://datamozart.medium.com/membership)
