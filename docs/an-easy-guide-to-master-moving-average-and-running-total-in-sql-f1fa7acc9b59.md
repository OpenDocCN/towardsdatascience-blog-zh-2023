# 轻松掌握 SQL 中的移动平均和累积总和

> 原文：[`towardsdatascience.com/an-easy-guide-to-master-moving-average-and-running-total-in-sql-f1fa7acc9b59`](https://towardsdatascience.com/an-easy-guide-to-master-moving-average-and-running-total-in-sql-f1fa7acc9b59)

## 解锁 SQL 中的高级数据分析

[](https://iffatm.medium.com/?source=post_page-----f1fa7acc9b59--------------------------------)![Iffat Malik](https://iffatm.medium.com/?source=post_page-----f1fa7acc9b59--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f1fa7acc9b59--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1fa7acc9b59--------------------------------) [Iffat Malik](https://iffatm.medium.com/?source=post_page-----f1fa7acc9b59--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1fa7acc9b59--------------------------------) ·阅读时间 10 分钟·2023 年 7 月 11 日

--

![](img/94d341250c8e2d199fd84500cc7e30b4.png)

图片来源：作者

如果你处理数据，你可能经常遇到‘*移动平均*’和‘*累积总和*’这两个术语。数据专业人员常常提到，

> “ 趋势是你的朋友。 ”

清晰理解趋势对准确预测和做出明智决策至关重要。然而，确定趋势并非总是简单直接的。这时，简单移动平均就派上用场了。通过跟踪在定义时间段内的趋势，它有助于识别和减轻噪音，同时平滑数据波动。这种技术提高了我们有效分析模式和做出可靠预测的能力。

在进入代码演示之前，让我们熟悉一些关键术语。

## 什么是移动平均？

*移动平均* 也称为 *滚动平均、运行平均* 或 *滚动均值*。你可以通过计算在特定时间段内的一组值的平均值来得到它。

它提供了一种标准化和简明的方法来总结和分析数据，揭示整体趋势，并使数据专业人员和决策者能够根据数据集中的分布、集中趋势、变异性和关系得出有意义的结论。

许多人热衷于跟踪他们的每日步数。那么，让我们用这个来理解移动平均的概念。假设，我们不是仅仅关注每天走的步数，而是计算步数的 7 天移动平均。

计算 7 天移动平均时，将过去七天的步数相加，然后将总和除以 7。

![](img/d944cba4ed0a758dd3219df14ab12fd1.png)

图片来源：作者

考虑上述图像中的计算，*7928.57* 步的移动平均值使我们更好地了解我们的整体活动水平。通过将此平均值与每日步数进行比较，我们可以查看是否持续达到或超过该平均值。

+   如果每日步数持续高于移动平均值，这表明我们保持了较高的活动水平。

+   另一方面，如果每日步数持续低于移动平均值，这可能表明我们的整体活动水平有所下降。

![](img/84c5fe34a623c4fde129f5db1d908c36.png)

图片由作者提供

使用移动平均帮助我们洞察步数的整体趋势，并提供了我们在时间上健身进展的更清晰图像，让我们能够在必要时进行调整，并设定现实的目标来提高我们的活动水平。

## **什么是滚动总计？**

*滚动总计* 也被称为 *累积和*。它是当前行及所有上述行的值的总和。

假设我们有一个预算电子表格来跟踪每日开支。通过计算开支的滚动总计，我们可以获得以下洞察：

+   到目前为止我们在本月的支出，

+   我们离达到支出上限还有多近，或者

+   是否有任何支出模式随着时间的推移而出现。

![](img/2aeec239669a06840cdc05f32a8c9c3e.png)

图片由作者提供

这将帮助我们做出明智的决策，例如调整开支习惯、重新分配资金或识别需要削减的领域。

## **我们什么时候使用它们？**

移动平均和滚动总计可以用于各种情况，从我们刚刚讨论过的日常例子到更复杂的情况，如时间序列预测或分析股票市场数据。这些技术为各种常见和更高级的场景提供了有用的洞察和分析能力：

+   移动平均和滚动总计有助于理解销售和需求如何随时间变化。通过使用现有的销售数据计算这些指标，企业可以识别趋势和需求模式，并发现销售的季节性变化。这些信息有助于预测需求、规划生产、管理现有库存、寻找在特定领域或时期增加销售的机会、优化生产计划以避免缺货或库存过剩等。

+   这些指标用于跟踪企业在一段时间内的表现。例如，通过汇总总销售收入、客户获取或网站流量等关键绩效指标（KPI），企业可以看到其表现如何变化、设定目标并衡量实现目标的进展。这帮助他们了解整体趋势并评估成功。

+   它们可以用来分析客户行为数据。例如，零售网站可以通过计算一定时间段内订单金额的移动平均来跟踪客户的平均消费额。这有助于识别客户消费模式随时间变化的情况。移动平均的下降可能预示着需要调查影响客户消费的因素，并实施策略以提升消费。

+   在股票市场中，上升趋势指的是股票价格在一段时间内的持续上涨。这表明市场情绪积极，并有价格升值的潜力。相反，下降趋势指的是股票价格的持续下跌，表明市场情绪消极，可能会进一步下跌。交易者使用移动平均作为参考点：如果股票价格保持在移动平均线之上，则表明上升趋势；如果低于，则表明下降趋势。这有助于交易者根据市场趋势做出买入或卖出的决策。

> 移动平均关注趋势，而运行总和提供了有关累计值的宝贵见解。

## 场景

比如，我们有一个名为*“ARTICLES”*的表格，记录了从*2023 年 6 月 1 日*到*2023 年 6 月 10 日*的各个文章的每日浏览量。表格中的每一条记录代表一个日期、文章标题和当天记录的浏览量。你可以在我的[*GitHub Repository*](https://github.com/PhoenixIM/All_Things_SQL/tree/main/MovingAverage_RunningTotal)中找到源数据和代码文件，

这是示例数据，

```py
--sample data from table ARTICLES
SELECT 
    * 
FROM 
    ARTICLES
LIMIT 10;
```

![](img/3a57a3617bb0952680aea5125bf4dd83.png)

作者提供的图片

计算移动平均和运行总和的最方便直接的方法是利用窗口函数。要复习窗口函数和聚合函数的概念，你可以阅读这里的详细解释：

[](/how-to-use-sql-aggregate-functions-92f7244a07cb?source=post_page-----f1fa7acc9b59--------------------------------) ## SQL 聚合函数为你的下一个数据科学面试

### 回归基础 | SQL 初学者基础知识

towardsdatascience.com [](/window-functions-a-must-know-for-data-engineers-and-data-scientists-4dd3e4ad0d2?source=post_page-----f1fa7acc9b59--------------------------------) ## 窗口函数——数据工程师和数据科学家必须了解的

### 回归基础 | SQL 初学者基础知识

towardsdatascience.com

## **使用窗口函数计算运行总和**

继续我们的演示，假设你需要找出每一天结束时，文章*“SQL 中的聚合函数”*的总浏览量的累计和，

```py
--total number for views for "Aggregate Functions in SQL" by end of each day
SELECT
    `DATE`,
    ARTICLE_TITLE,
    NO_OF_VIEWS,
    SUM(NO_OF_VIEWS) OVER (ORDER BY `DATE` 
                           RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS RUNNING_TOTAL
FROM 
    ARTICLES
WHERE 
    ARTICLE_TITLE = "Aggregate Functions in SQL";
```

![](img/26c8308c1a907989661e5377e5787cc1.png)

作者提供的图片

在上述查询中，我们使用了*OVER()*子句，它是必不可少的，因为它将函数标识为窗口函数，其目的是定义一个特定的行组（窗口），在其上窗口函数将执行计算。但等等，这还不是全部。

在上面的代码中，我们还使用了*FRAME*子句，

> 范围从**UNBOUNDED PRECEDING**到**CURRENT ROW**

这是什么？本质上，窗口函数依赖于*ROW*或*RANGE*来确定计算中应考虑哪些值，通过指定所选子集的起始和结束点。

在这里，*FRAME*子句指定了帧的大小——*当前行的值和当前行上方所有行的值*——在其上需要执行*SUM(NO_OF_VIEWS)*。它在进行计算时不断累加*“NO_OF_VIEWS”*的值，为每一行按*DATE*排序提供了一个累计总数。

在上面的示例中，如果我们省略*RANGE*子句，结果将保持不变。你能猜到为什么吗？每当我们在窗口函数中使用*ORDER BY*子句时，默认的帧是‘*RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW*’。然而，个人而言，我发现包含*RANGE*或*ROW*子句有助于提高清晰度和理解。不可避免地，将来会有人继承你的代码。总是建议以一种便于他人理解和使用的方式编写代码。

你可以在这里阅读更多关于*FRAME*子句的信息，

[](/anatomy-of-sql-window-functions-7256d8cf509a?source=post_page-----f1fa7acc9b59--------------------------------) ## SQL 窗口函数的结构

### 回到基础 | SQL 基础知识入门

[towardsdatascience.com

现在，让我们对所有文章进行相同的分析——找出每篇文章每天结束时的总浏览量的累计和，

```py
--running total
SELECT
    `DATE`,
    ARTICLE_TITLE,
    NO_OF_VIEWS,
    SUM(NO_OF_VIEWS) OVER (PARTITION BY ARTICLE_TITLE 
                           ORDER BY `DATE` 
                           RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) 
                           AS RUNNING_TOTAL
FROM 
    ARTICLES; 
```

![](img/134b4630d684ece90b15791e0676dd92.png)

作者提供的 GIF

在这里，我们根据*ARTICLE_TITLE*将数据分成多个分区。然后，根据*FRAME*子句定义，我们对每个分区执行了*SUM(NO_OF_VIEWS)*计算。

为了更好地理解，请参阅下面提供的图片。它展示了对单个分区进行的计算。相同的逻辑和计算也适用于所有其他分区。

![](img/0e6c4f7eac7288b772bd77d99f14405e.png)

作者提供的图片

## 使用窗口函数计算移动平均

让我们通过计算每天总浏览量的 3 天移动平均来评估每篇文章的表现。这将帮助我们了解文章随时间的表现情况。

简单来说，3 天移动平均值是指我们计算连续三天的平均观看次数。为此，我们将当前一天、前一天和前两天的观看次数相加，然后将总数除以 3 以找到平均值。这有助于我们了解文章在短时间内的平均受欢迎程度。

```py
--3 days moving average
SELECT
    `DATE`,
    ARTICLE_TITLE,
    NO_OF_VIEWS,
    FORMAT(AVG(NO_OF_VIEWS) OVER (PARTITION BY ARTICLE_TITLE 
                                  ORDER BY `DATE` 
                                  ROWS BETWEEN 2 PRECEDING AND CURRENT ROW),2) 
                                  AS MOVING_AVERAGE_3DAYS
FROM 
    ARTICLES;
```

![](img/83f08ac64793ba987fdbde7defeea430.png)

作者提供的图片

在这种情况下，我们使用了*FRAME*子句作为*“ROWS BETWEEN 2 PRECEDING AND CURRENT ROW.”* 这表示在计算*NO_OF_VIEWS*的平均值时，函数会考虑当前行的值以及前两天的值。

![](img/687650cd976993d11fa89b4f78ced595.png)

作者提供的图片

一个明显的观察结果是，当观看次数非常高或低时，移动平均值往往会平滑这些极端值，给出不那么极端的值。如果我们将每日观看次数和 3 天移动平均值绘制在折线图上，就是这样的效果，

![](img/718659998c0acac18c7e965a3a9fab7f.png)

作者提供的图片 — 每日观看次数与 3 天移动平均值 - 单个视图

在这里，蓝线表示每日观看次数，红线表示 3 天移动平均值。正如我之前提到的，移动平均线（红线）与表示每日观看次数的线（蓝线）相比，显得更平滑。

使用移动平均技术的主要目的是实现这种平滑效果，这有助于消除或减少数据中的噪声。

在这种情况下，噪声指的是数据中的短期不规则性或波动。通过最小化或去除噪声，我们可以专注于潜在的模式和趋势，而不被任何随机或临时的波动过度影响。

在这个例子中，我们计算了 3 天移动平均值。然而，您可以根据需求和可用数据使用不同的前值数量来计算平均值。您可以选择更多或更少的天数来计算平均值，这取决于最适合您特定情况和信息的要求。

> 如果您选择更多的前值，结果图形将有更平滑的曲线。另一方面，如果您选择更少的行来计算平均值，移动平均图将更接近原始值。

例如，3 天移动平均值与 7 天移动平均值之间存在显著差异。

![](img/6db79e68342911faf55e755bd3777c73.png)

作者提供的图片 — 每日观看次数与 3 天移动平均值及 7 天移动平均值对比

## 结论

本文旨在提供对如何在 SQL 中计算移动平均值和累积总和的全面理解。这些技术在识别数据集中的趋势时极为重要。为了确保结果的准确性，数据集必须没有日期间隙。值得注意的是，在本文中，我们使用了一个较小的数据集进行演示。在实际场景中，面对更大的数据集时，你可能会遇到各种波动，这使得确定清晰的趋势更具挑战性。大量数据的存在可以揭示更复杂的模式，并提供对基础趋势的更全面理解。

如果你记住了某些东西，你一定是好好地练习过它，

+   [HackerRank](https://www.hackerrank.com/) 或 [LeetCode](https://leetcode.com/) 用于练习基础/中级/高级 SQL 问题。

你可以在我的 [*GitHub 仓库*](https://github.com/PhoenixIM/All_Things_SQL/tree/main/MovingAverage_RunningTotal)* 中找到本文使用的源数据和代码文件。*

[*成为会员并阅读 Medium 上的每个故事*](https://medium.com/@iffatm/membership)*。*

祝学习愉快！
