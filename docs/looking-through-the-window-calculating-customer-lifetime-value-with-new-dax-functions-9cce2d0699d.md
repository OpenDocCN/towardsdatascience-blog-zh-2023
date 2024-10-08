# 透过窗户——利用新的 DAX 函数计算客户生命周期价值

> 原文：[`towardsdatascience.com/looking-through-the-window-calculating-customer-lifetime-value-with-new-dax-functions-9cce2d0699d`](https://towardsdatascience.com/looking-through-the-window-calculating-customer-lifetime-value-with-new-dax-functions-9cce2d0699d)

## 新的窗口函数是 DAX 语言迄今为止最重要的增强功能之一！学习如何利用它们来计算客户生命周期价值

[](https://datamozart.medium.com/?source=post_page-----9cce2d0699d--------------------------------)![尼古拉·伊利奇](https://datamozart.medium.com/?source=post_page-----9cce2d0699d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9cce2d0699d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cce2d0699d--------------------------------) [尼古拉·伊利奇](https://datamozart.medium.com/?source=post_page-----9cce2d0699d--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cce2d0699d--------------------------------) ·7 分钟阅读·2023 年 1 月 3 日

--

![](img/6190e47157c687068627731da77f3602.png)

图片由 Pexels 上的 Pixabay 提供

在我之前作为 SQL 专业人员的“生涯”中，我广泛使用 T-SQL 窗口函数来完成各种分析任务。我在[这篇文章](https://data-mozart.com/island-adventures-with-t-sql-window-functions/)中描述了其中一种可能的使用场景，但实际上有很多场景可以通过窗口函数快速直观地解决。

因此，当我转向 Power BI 时，我感到非常惊讶（更不用说失望了），因为没有 DAX 等效的 SQL 窗口函数。好吧，我们可以通过编写更复杂的 DAX 来解决在某些行上执行不同计算的挑战——但说实话，这常常是非常痛苦的经历。

所以，当 Power BI Desktop 2022 年 12 月更新宣布了一组全新的 DAX 函数——统称为窗口函数——这应该实现与 SQL 窗口函数相同的目标时，我感到非常兴奋。目前，有三个 DAX 窗口函数：***OFFSET***、***INDEX*** 和 ***WINDOW***。

如果你想了解更多关于这些函数及其背后工作原理的信息，我强烈推荐阅读[Jeffrey Wang 的这篇文章](https://pbidax.wordpress.com/2022/12/15/introducing-dax-window-functions-part-1/)——这是深入了解 DAX 窗口函数的最佳起点。

如果你特别对 OFFSET 函数感兴趣，我鼓励你阅读[这篇精彩的文章](https://www.minceddata.info/2022/12/14/unlock-an-ample-new-world-by-seeing-through-a-window/)由我的朋友 Tom Martens 撰写，或者[这篇文章](https://www.linkedin.com/pulse/offset-its-usage-calculation-groups-%C5%A1t%C4%9Bp%C3%A1n-re%C5%A1l/)由Štěpán Rešl 撰写。

在这篇文章中，我不会花太多时间解释窗口函数的细节，因为我想专注于解释如何利用这些函数来满足一个非常常见的业务需求——计算客户的终身价值。

# 客户终身价值

让我们从了解什么是客户终身价值开始。它是一个广泛的术语，可能有多种解释。在我们的案例中，我们想深入了解单个客户的行为——例如，他们下了多少订单，他们在我们的产品上花了多少钱，他们的订单之间隔了多少天，以及这些与所有客户的平均情况相比如何。最后，我们想知道我们的客户有多忠诚——即他们的第一次和最后一次订单之间隔了多少天。

那么，让我们从一个非常基本的场景开始。我正在分析 Adventure Works 数据集中的两个客户：Adam Young 和 Alexandra Jenkins。以下是他们的订单摘要和总销售额：

![](img/ebf024c6f0e4fc61a16def9414c437d8.png)

图片由作者提供

首先需要理解的概念是我们想将每个客户视为一个单独的“实体”——意味着，我们想为每个客户创建一个“窗口”，并分析该特定行集合的数字。在我们的案例中，我们将有两个“窗口”：

![](img/6673de0e94e2a712aea076f31b4dcbfd.png)

图片由作者提供

DAX 中的窗口函数可以以两种不同的方式工作——要么操作相对（REL）值，基于当前行（再次阅读 Jeffrey 的文章以了解如何确定当前行），要么操作绝对（ABS）值。在我的情况下，我总是希望我的窗口从分区的第一行开始（每个客户代表一个单独的分区），并在分区的最后一行结束。

所以，让我们创建一个度量来计算每个分区的订单数量和销售额的累积总和：

```py
Window Quantity = CALCULATE([Total Quantity],
                   WINDOW(
                        1,ABS,
                        -1,ABS,
                        SUMMARIZE(ALLSELECTED(Sales),Customer[Customer],'Date'[Date]),
                        ORDERBY ('Date'[Date]),
                        KEEP,
                        PARTITIONBY(Customer[Customer])
                   )
                   )
```

```py
Window Sales Amount = CALCULATE([Total Sales],
                   WINDOW(
                        1,ABS,
                        -1,ABS,
                        SUMMARIZE(ALLSELECTED(Sales),Customer[Customer],'Date'[Date]),
                        ORDERBY ('Date'[Date]),
                        KEEP,
                        PARTITIONBY(Customer[Customer])
                   )
                   )
```

让我们停下来解释一下这个度量定义。作为 CALCULATE 筛选器修饰符，我们将使用一个新的窗口 DAX 函数 WINDOW。第一个参数（1）确定窗口的开始位置。由于第二个参数是 ABS（绝对），这意味着窗口从分区的开始处开始。接下来，我们定义窗口的结束位置。

由于我们使用了负值（-1）和 ABS，这意味着窗口结束于分区的最后一行。之后，我们定义一个表表达式，从中返回输出行。最后，数据将在窗口内按日期排序（从最早的日期开始），并在客户上进行分区（每个客户是一个独立的“窗口”）。

![](img/1df0f364a0adbec4c8e86b7b2546beba.png)

作者提供的图片

现在，我们有了每个客户的累计总数！所以，我们可以例如计算每笔个别购买在整体中的百分比：

```py
% of Window Quantity = DIVIDE([Total Quantity],[Window Quantity],0)
```

```py
% of Window Sales = DIVIDE([Total Sales],[Window Sales Amount],0)
```

![](img/4a25ae0d6db928847c497106b013ed16.png)

作者提供的图片

到目前为止，一切顺利！让我们做一些更酷的事情。首先，我将计算两个连续订单之间的天数。为此，另一种 DAX 窗口函数 OFFSET 将派上用场。实际上，我想抓取上一行的日期，并计算当前行日期与上一行日期之间的天数差：

```py
Offset Date = CALCULATE(
                        MIN('Date'[Date]),
                        OFFSET(
                            -1,
                            SUMMARIZE(ALLSELECTED(Sales),Customer[Customer],'Date'[Date]),
                            ORDERBY('Date'[Date]),
                            KEEP,
                            PARTITIONBY(Customer[Customer])
                        )
                        )
```

![](img/15c96a092407974bb49f6dbefdc9cc79.png)

作者提供的图片

这是计算订单之间天数的度量：

```py
Days Between Orders = DATEDIFF([Offset Date],MIN('Date'[Date]),DAY)
```

![](img/592a24a8d758dc3682c66cb1bf59985b.png)

作者提供的图片

好的，现在，让我们计算每个客户订单之间的平均天数：

```py
AVG Days Between Orders = CALCULATE(AVERAGEX(
                  SUMMARIZE(
                            ALLSELECTED(Sales),Customer[Customer],'Date'[Date]),[Days Between Orders]),
                   WINDOW(
                        1,ABS,
                        -1,ABS,
                        SUMMARIZE(ALLSELECTED(Sales),Customer[Customer],'Date'[Date]),
                        ORDERBY ('Date'[Date]),
                        KEEP,
                        PARTITIONBY(Customer[Customer])
                   )
                   )
```

![](img/351daacb32b3bf88156edd4faa5d3449.png)

作者提供的图片

那么，我们目前可以得出什么结论？亚历山德拉平均每 22 天下单，而亚当平均需要 89 天才会下新订单。与整体相比如何？89 天的间隔是否过长？

那么，让我们将这些数字放入整个数据集的背景中。

![](img/1d44c0b1526e11d7f82c52b01252c3ea.png)

作者提供的图片

虽然 89 天相比于亚历山德拉·詹金斯的 22 天看起来不太好，但我们可以得出结论，与所有客户的平均天数 287 天相比，这 89 天根本不算差！确实很有洞察力！

让我们通过计算总客户生命周期来总结一下。这是每个客户首次和最后一次订单之间的天数。所以，让我们在我们的“窗口”内计算第一次和最后一次订单的日期：

```py
Window Min Date = CALCULATE(MIN('Date'[Date]),
                   WINDOW(
                        1,ABS,
                        -1,ABS,
                        SUMMARIZE(ALLSELECTED(Sales),Customer[Customer],'Date'[Date]),
                        ORDERBY ('Date'[Date]),
                        KEEP,
                        PARTITIONBY(Customer[Customer])
                   )
                   )
```

```py
Window Max Date = CALCULATE(MAX('Date'[Date]),
                   WINDOW(
                        1,ABS,
                        -1,ABS,
                        SUMMARIZE(ALLSELECTED(Sales),Customer[Customer],'Date'[Date]),
                        ORDERBY ('Date'[Date]),
                        KEEP,
                        PARTITIONBY(Customer[Customer])
                   )
                   )
```

![](img/5ba9401851bae76554eb7e7154903178.png)

作者提供的图片

这是客户生命周期天数的计算：

```py
Customer Lifetime Days = DATEDIFF([Window Min Date],[Window Max Date],DAY)
```

一旦我把它拖到表格中，就能看到它在每个窗口（每个客户单独计算）内被计算：

![](img/fd085451b63d3f1b9ce9894a41ce9ecd.png)

作者提供的图片

当然，这只是使用 DAX 窗口函数的一个基本示例。说实话，有无数的用例可以考虑——例如，我还可以在窗口内对单独的行进行排名（并快速识别每个客户的最高销售额）。

我还可以通过多个属性进行分区——例如，按客户和月份。这样，我们将有一个包含某个客户在单个月份内所有行的“窗口”。想象一下：亚当·杨——七月，亚当·杨——八月，亚历山德拉·詹金斯——七月，亚历山德拉·詹金斯——八月，等等。根据你的具体业务需求，“窗口”可以有不同的定义。

# 结论

窗口函数是 DAX 语言最重要的增强功能之一，这一点毫无疑问！一些之前需要编写复杂且冗长的 DAX 的业务用例，现在可以以更优雅、更优化的方式实现。正如 SQL 语言中，窗口函数是最强大的分析工具之一，DAX 窗口函数也将使许多 Power BI 开发任务更易于实现。

感谢阅读！
