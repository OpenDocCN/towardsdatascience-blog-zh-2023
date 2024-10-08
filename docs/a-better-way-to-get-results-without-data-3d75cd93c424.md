# 一种更好的在没有数据的情况下获得结果的方法

> 原文：[`towardsdatascience.com/a-better-way-to-get-results-without-data-3d75cd93c424`](https://towardsdatascience.com/a-better-way-to-get-results-without-data-3d75cd93c424)

## *想象一下有一个数据集，并希望看到零而不是空单元格。这听起来简单？实际上，这个案例有一些陷阱。让我们来深入研究一下。*

[](https://medium.com/@salvatorecagliari?source=post_page-----3d75cd93c424--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----3d75cd93c424--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3d75cd93c424--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d75cd93c424--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----3d75cd93c424--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----3d75cd93c424--------------------------------) ·5 分钟阅读·2023 年 2 月 27 日

--

![](img/d040e5a4f041e47f5477c2568f858ba9.png)

图片由[Ramón Salinero](https://unsplash.com/@donramxn?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

几周前，我已经写过一篇关于这个主题的文章：

[](/how-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf?source=post_page-----3d75cd93c424--------------------------------) ## 如何在 Power BI 中在没有数据的情况下显示结果

### 这个标题似乎不直观。为什么在没有数据的情况下我还要要求结果？让我们看看请求…

towardsdatascience.com

在那篇文章中，我使用 Power Query 生成了额外的数据行来填充数据表并获得所需的结果。

现在我找到了一种更简单的方法来解决这个挑战。

首先，我将解释这个挑战。如果你知道我上面链接的文章，以下部分描述了我如何找到新的方法。然后我将向你展示新的解决方案。

# 挑战

想象一个报告，其中包含显示多个任务及其相关成本和每个任务状态的表格：

![](img/a52ffabefcdfcfaf9d20e6c543bbcdb5.png)

图 1 — 起始点的表格（图由作者提供）

现在，想象一个类似的表格。但现在在“失败”状态中没有任务：

![](img/5d5290eff3f4371c087e9e5270f87c45.png)

图 2 — 缺失状态的表格（图由作者提供）

现在，我的客户希望看到零而不是空单元格，并且他希望看到所有可能的状态，即使没有任务具有特定状态，如上图所示。

问题是，如果数据表中没有值，则不会执行任何计算，这意味着不可能显示零而不是空单元格。

# 新方法的灵感

几天前，我的一位客户问我关于他的 Power BI 报告的问题。

请理解，我只能在这里展示一些细节。

总而言之：他计算了一个百分比，并从 1 中扣除了结果金额。

这个度量看起来是这样的：

```py
Measure Name = 1 - ( CALCULATE(SUM(column1)
                               ,[Column2] = "Filter Text"
                               )
                           / 12345 )
```

结果是，他总是得到 100 %，而期望得到一个空结果。

经过一些测试，我意识到“1 –”部分是罪魁祸首。

当 CALCULATE() 函数返回一个值时，结果会被正确计算。

如果没有 CALCULATE() 的结果，引擎计算 1–0 = 1，即 100 %

Power BI 将值 1 视为常量，并且没有应用任何过滤器。

结果是，当表中没有数据时，Power BI 返回了 100 %。

我必须添加一个 IF() 来解决这个问题，检查 CALCULATE() 函数是否返回了一个值：

```py
Measure Name = 
 VAR Result = CALCULATE(SUM(column1)
                        ,[Column2] = "Text"
                        )
RETURN
IF (NOT ISBLANK(Result)
     , 1 - ( Result / 12345 )
     )
```

现在，只有当 CALCULATE() 返回一个值时，度量才返回一个值。

# 反转想法

从中产生了一个想法：将这个“问题”反转并转化为解决方案。

如果我能向我的解决方案中添加一个度量，并计算 *0 + SUM(‘table’[column])* 呢？

在这种情况下，如果 SUM 没有返回任何结果，我将得到 0。而当 SUM() 返回一个值时，添加 0 不会改变结果。

但等等，这里有个问题：当没有状态为“失败”的行时，我怎么能显示任何结果？

为了解决这个问题，我必须稍微改变我的数据模型。

最初，我的数据模型只包含包含值的表。

为了强制计算，我需要一个包含所有状态的表，并将其用作维度，以便为表中的每一行强制设置过滤上下文：

![](img/c1637ef9bfc5ca1064bf2922c0821780.png)

图 3 — 状态表（作者绘制）

然后，我必须向我的演示数据表中添加一个关系：

![](img/9563484191ec8a597ddd523b83a1758d.png)

图 4 — 包含两个表的数据模型（作者绘制）

现在我更改了我的报告，从状态表中获取状态列，并添加了一个新的度量：

```py
Costs = 0 + SUM('Demo Data 2'[Cost])
```

下面，你可以看到原始表的数据（包括所有状态的数据），以及使用新解决方案的没有状态失败的表的数据：

![](img/3cde1be53292256475e598a54ca87088.png)

图 5 — 比较两个结果（作者绘制）

这是我客户需要的结果。

# 结论

数据集是虚构的，像之前的文章一样，在 Excel 中从头开始创建。

虽然我的第一个解决方案是建立在数据表是我所拥有的一切的假设上，但现在我必须创建一个新的状态表，并确保这个表中包含所有可能的状态。

你可能可以从数据源中提取这些信息。

如果没有，你可以使用我基于 Power Query 的旧解决方案，或者使用 Power BI 的“输入数据”功能创建一个表。

无论你如何添加额外的表格，你都需要生成一个过滤上下文，以便度量值可以始终返回一个值。

![](img/8f2d0d07a59ca3e9b8016596d514386c.png)

照片由 [Bill Jelen](https://unsplash.com/@billjelen?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果你对 Power Query 不熟悉，可以阅读我之前的文章以了解更多关于这个强大工具的信息。

在某个时刻，这可能会节省你一天的时间。

[](https://medium.com/@salvatorecagliari/membership?source=post_page-----3d75cd93c424--------------------------------) [## 通过我的推荐链接加入 Medium - Salvatore Cagliari

### 阅读 Salvatore Cagliari 的每一篇故事（以及 Medium 上成千上万其他作者的故事）。你的会员费用将直接……

medium.com](https://medium.com/@salvatorecagliari/membership?source=post_page-----3d75cd93c424--------------------------------)
