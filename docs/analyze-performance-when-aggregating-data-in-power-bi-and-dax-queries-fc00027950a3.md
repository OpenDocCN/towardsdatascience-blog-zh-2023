# 分析在 Power BI 和 DAX 查询中聚合数据的性能

> 原文：[`towardsdatascience.com/analyze-performance-when-aggregating-data-in-power-bi-and-dax-queries-fc00027950a3`](https://towardsdatascience.com/analyze-performance-when-aggregating-data-in-power-bi-and-dax-queries-fc00027950a3)

## *我们在 Power BI 中经常聚合数据。有时我们需要手动查询数据模型，或者在度量中需要中间表。让我们看看如何做到这一点。*

[](https://medium.com/@salvatorecagliari?source=post_page-----fc00027950a3--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----fc00027950a3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc00027950a3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc00027950a3--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----fc00027950a3--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc00027950a3--------------------------------) ·8 分钟阅读·2023 年 5 月 1 日

--

![](img/e798626b974acc167cbccbdc87b18161.png)

[Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

你是否曾经问过自己：

Power BI 视觉效果背后发生了什么？

或者

我如何编写查询以获得在 Power BI 视觉效果中显示的结果？

好吧，你可以用性能分析器捕捉查询，并将查询复制到文本编辑器中，或者更好地使用 DAX Studio。

但你是否理解查询中发生了什么？

当你查看 DAX 的函数文档时，无论是在 [Microsoft DAX 函数参考](https://learn.microsoft.com/en-us/dax/dax-function-reference) 还是在 [DAX.Guide](https://dax.guide/)，你会发现至少有五个函数用于在查询中生成表：

+   [SELECTCOLUMNS()](https://dax.guide/selectcolumns/) 和 [ADDCOLUMNS()](https://dax.guide/addcolumns/)

+   [SUMMARIZE()](https://dax.guide/summarize/)

+   [SUMMARIZECOLUMNS()](https://dax.guide/summarizecolumns/)

+   [CALCULATETABLE()](https://dax.guide/calculatetable/)

在这篇文章中，我将通过基础查询来设定场景。然后，我将使用不同的函数从头重建查询，并观察这些函数之间的差异。

我将研究功能差异以及在效率和性能方面的差异。

# 基础查询

让我们从基础查询开始。

查看 Power BI 中的以下矩阵：

![](img/fc4fa958091472c0605a0e9de17b9b89.png)

图 1 — 起始视觉效果（图由作者提供）

我用 Performance Analyzer 提取了查询，并在移除 Power BI 在计算国家和大陆级别的总数时需要的所有小计内容后，剩下的结果如下：

```py
DEFINE
  VAR __DS0FilterTable = 
    FILTER(
        KEEPFILTERS(VALUES('Date'[YearIndex]))
          ,AND('Date'[YearIndex] >= -3, 'Date'[YearIndex] <= 0)
          )

VAR __DS0Core = 
  SUMMARIZECOLUMNS(
              'Geography'[ContinentName]
              ,'Geography'[RegionCountryName]
              ,__DS0FilterTable
              ,"Sum_Online_Sales", 'All Measures'[Sum Online Sales]
            )

EVALUATE
  __DS0Core

ORDER BY
  'Geography'[ContinentName]
  ,'Geography'[RegionCountryName]
```

这里的关键函数是 [SUMMARIZECOLUMNS()](https://dax.guide/summarizecolumns/)。

这个函数从两个列 [ContinentName] 和 [RegionCountryName] 中获取唯一值，并对每一行执行 Measure [Sum Online Sales]，同时应用在 Variable __DS0FilterTable 中定义的过滤器。

在接下来的所有示例中，我将（尝试）保持 __DS0FilterTable 的定义，如上所示。

# SELECTCOLUMNS() 和 ADDCOLUMNS()

使用 [SELECTCOLUMNS()](https://dax.guide/selectcolumns/)，我可以将计算列添加到输入表中，例如，通过 Measure。

输入表可以是一个现有的表或表函数的结果。

让我们尝试这个形式：

```py
DEFINE
  VAR __DS0FilterTable =
    FILTER (
        KEEPFILTERS ( VALUES ( 'Date'[YearIndex] ) ),
            AND ( 'Date'[YearIndex] >= -3, 'Date'[YearIndex] <= 0 )
          )

EVALUATE
  SELECTCOLUMNS ('Geography'
                ,'Geography'[ContinentName]
                ,'Geography'[RegionCountryName]
                ,"Sum_Online_Sales", [Sum Online Sales]
              )
```

这是这个查询的结果：

![](img/45d7ad7b9cc68201bd2cb30581e068cf.png)

图 2 — ADDCOLUMNS() 的部分结果（作者提供的图）

如你所见，即使我仅为查询选择了两列，我仍然从地理表中获得了所有行，包括那些没有结果的行。

我想要其他东西。

另一个问题是，使用 SELECTCOLUMNS() 时，我不能引入上面的过滤器。

不管怎样，查看服务器时间，这个查询看起来还不错：

![](img/01c802c59bcd9f27337296565ccbfb6b.png)

图 3 — ADDCOLUMNS() 的服务器时间（作者提供的图）

大部分工作在存储引擎中完成，并且并行度接近 7.5，非常出色。

当将结果复制到 Excel 时，我们可以毫无问题地删除空行。

[SELECTCOLUMNS()](https://dax.guide/selectcolumns/) 与 [ADDCOLUMNS()](https://dax.guide/addcolumns/) 非常相似。

根据 DAX.guide，区别在于 SELECTCOLUMNS() 从一个空表开始，然后添加给定的列，而 ADDCOLUMNS() 从所有输入表列开始。

当我们尝试这个查询时：

```py
EVALUATE
  ADDCOLUMNS('Geography'
              ,"Sum_Online_Sales", [Sum Online Sales]
            )
```

我们得到一个包含所有地理表列的表，并且对于每一行，计算 Measure 的结果。

我需要特定的函数来创建一个表，因为我只能定义一个输入表。

我将在本文后面回到这个问题。

# SUMMARIZE()

函数 [SUMMARIZE()](https://dax.guide/summarize/) 允许我获取一个总结给定列并添加计算列（例如，通过 Measure）的表。

基于上面的示例，查询将如下所示：

```py
EVALUATE
  SUMMARIZE( 'Geography'
            ,'Geography'[ContinentName]
            ,'Geography'[RegionCountryName]
            ,"Sum_Online_Sales", [Sum Online Sales]
          )
```

再次，我们不能在这个查询中添加过滤器。

所以，我们将获取所有年份的结果：

![](img/30680cb2c702f399ef8a8089b2491286.png)

图 4 — 使用 SUMMARIZE() 的结果（作者提供的图）

但再一次，我将得到所有国家的列表，包括那些没有值的。

服务器时间看起来也不错：

![](img/d9f04632751c6a305cc89e77d6808b56.png)

图 5 — SUMMARIZE() 的服务器时间（作者提供的图）

但在使用 SUMMARIZE() 函数时存在一些问题。

你可以在下面的参考部分找到描述这些问题的文章。

现在，我将向你展示如何使用正确的形式完成任务。

# SUMMARIZECOLUMNS()

[SUMMARIZECOLUMNS()](https://dax.guide/summarizecolumns/) 函数将 ADDCOLUMN() 和 SUMMARIZE() 的优点结合成一个强大的函数。

我可以将多个列传递给用作汇总列的函数并添加计算列。我也可以向该函数传递筛选器。

看看这个例子：

```py
DEFINE
  VAR __DS0FilterTable =
    FILTER (
      KEEPFILTERS ( VALUES ( 'Date'[YearIndex] ) ),
          AND ( 'Date'[YearIndex] >= -3, 'Date'[YearIndex] <= 0 )
          )

EVALUATE
  SUMMARIZECOLUMNS(
            'Geography'[ContinentName]
            ,'Geography'[RegionCountryName]
            ,__DS0FilterTable
            ,"Sum_Online_Sales", [Sum Online Sales]
          )
ORDER BY 'Geography'[ContinentName]
  ,'Geography'[RegionCountryName]
```

当你查看本文开头的查询时，你会发现正是这个查询。

结果如下：

![](img/86ead1887ad3080e8c42b73da1b6ec5c.png)

图 6 — 使用 SUMMARIZECOLUMNS() 的结果（作者提供的图）

这是我们期望的查询结果。

服务器时间令人印象深刻：

![](img/b207c653a0b03e24997fb3caee0b606d.png)

图 7 — SUMMARIZECOLUMNS() 的服务器时间（作者提供的图）

完成了，不是吗？

![](img/9fd90500575cb44248c04d36b775b31b.png)

照片由 [Zac Durant](https://unsplash.com/@zacdurant?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

不，稍等一下。

如果我尝试在上述查询中添加筛选器以将数据限制为一年会怎么样？

结果是，我无法这样做，我只能将一个表作为筛选器传递给 SUMMARIZECOLUMNS()。

# CALCULATETABLE()

[CALCULATETABLE()](https://dax.guide/calculatetable/) 与其他三个函数不同。

我可以像使用 [CALCULATE()](https://dax.guide/calculate/) 一样使用 CALCULATETABLE()。但我将表作为第一个参数，而不是聚合函数或其他度量。然后，我可以将筛选器作为附加参数添加。

那么，让我们尝试将上一个查询的结果限制为一年：

```py
EVALUATE
  CALCULATETABLE(SUMMARIZECOLUMNS(
                      'Geography'[ContinentName]
                      ,'Geography'[RegionCountryName]
                      ,"Sum_Online_Sales", [Sum Online Sales]
                  )
                  ,'Date'[Year] = 2023
              )
ORDER BY 'Geography'[ContinentName]
  ,'Geography'[RegionCountryName]
```

如你所见，我将 SUMMARIZECOLUMNS() 函数作为 CALCULATETABLE() 的输入，并在查询中添加了列筛选器。

结果如下：

![](img/cf255442e3dae69dcfd335b648861771.png)

图 8 — 使用 CALCULATETABLE() 的结果（作者提供的图）

服务器时间非常高效，仅需一个 SE 查询：

![](img/6c694e0fb8f138937a284dcbe439354c.png)

图 9 — CALCULATETABLE() 的服务器时间（作者提供的图）

CALCULATETABLE() 可以将整个 DAX 查询合并为一个 SE 查询，使其非常高效。

但不要指望 CALCULATETABLE() 总是能提高效率。稍后，我们将看到一个示例，其中这个函数没有达到相同的效果。

# 组合函数

生成所需结果的另一种方法是将 ADDCOLUMNS() 和 SUMMARIZE() 函数结合使用，如 SQLBI 发布的文章中所述（参见下面的参考部分）。

```py
DEFINE
  VAR __DS0FilterTable =
    FILTER (
      KEEPFILTERS ( VALUES ( 'Date'[YearIndex] ) ),
        AND ( 'Date'[YearIndex] >= -3, 'Date'[YearIndex] <= 0 )
        )

EVALUATE
  ADDCOLUMNS(
      SUMMARIZE('Geography'
                ,'Geography'[ContinentName]
                ,'Geography'[RegionCountryName]
              )
              ,"Sum_Online_Sales", CALCULATE([Sum Online Sales]
              ,KEEPFILTERS(__DS0FilterTable)
            )
      )
ORDER BY 'Geography'[ContinentName]
  ,'Geography'[RegionCountryName]
```

请注意我如何将度量添加到结果中。我使用 CALCULATE 来包含使用 [KEEPFILTERS()](https://dax.guide/keepfilters/) 的筛选表。必须这样做，否则结果将是错误的。

再次，请阅读下面的 SQLBI 文章，以准确解释为什么这样做是必要的。

结果中的值是正确的，但我们再次看到所有国家，而不仅仅是有结果的国家：

![](img/4f8454ce0a050e72a4d41146ba0443c7.png)

图 10 — 结合 ADDCOLUMNS 和 SUMMARIZE 的结果（作者提供的图）

当我们查看服务器时间时，我们会发现 DAX 需要三个 SE 查询来完成这个查询：

![](img/a8ebaf37ece0118f709d6430dc53d09f.png)

图 11 — 结合 ADDCOLUMNS() 和 SUMMARIZE() 的服务器时间（作者提供的图）

另一种方法是使用 CALCULATETABLE() 引入过滤器：

```py
DEFINE
VAR __DS0FilterTable =
  FILTER (
    KEEPFILTERS ( VALUES ( 'Date'[YearIndex] ) ),
      AND ( 'Date'[YearIndex] >= -3, 'Date'[YearIndex] <= 0 )
       )

EVALUATE
  CALCULATETABLE(
    ADDCOLUMNS(
      SUMMARIZE('Geography'
                ,'Geography'[ContinentName]
                ,'Geography'[RegionCountryName]
                )
          ,"Sum_Online_Sales", [Sum Online Sales]
          )
          ,__DS0FilterTable
       )

ORDER BY 'Geography'[ContinentName]
  ,'Geography'[RegionCountryName]
```

结果仍然是相同的，服务器时间没有得到改善。

这证明了 CALCULATETABLE() 只是有时能提高效率。但它可以使查询更具可读性，而不是使用 KEEPFILTERS()，对于 KEEPFILTERS() 的所有效果我仍然难以理解。

# 结论

SELECTCOLUMNS()/ADDCOLUMNS() 是向表中添加计算列的良好起点。

但是我需要 SUMMARIZE()/SUMMARIZECOLUMNS() 仅对选定的列进行汇总，并能够向结果中添加计算列。

但是当我们想要向表表达式添加过滤器时，SUMMARIZE() 的能力有所减少。

在这种情况下，SUMMARIZECOLUMNS() 是正确的函数。

即使我需要 CALCULATETABLE() 来向查询添加某些类型的过滤器（例如，对单列的过滤器）。

在我的工作中，我总是需要编写查询来将结果与源系统中的数据进行比较，以验证结果。

通过查询和相应结果来记录验证要比截取 Power BI 中设置特定结果的所有过滤器的屏幕截图并从可视化中导出数据要容易得多。

当你希望自动生成一个报告并自动执行并发送给用户时，查询是非常有用的。

在某些情况下，编写查询比在 Power BI 中直接操作要更为合适。

我希望我能激发你探索 DAX 查询的可能性。

![](img/74c898ccb5ccd13d9fe26c953dfad619.png)

[Casey Horner](https://unsplash.com/@mischievous_penguins?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供的照片

# 参考文献

如果你想了解更多关于在 DAX Studio 中测量性能的内容，请阅读以下文章：

[## 如何使用 DAX Studio 从 Power BI 获取性能数据](https://example.org/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9?source=post_page-----fc00027950a3--------------------------------)

### 有时我们会遇到运行缓慢的报告，需要找出原因。我们将学习如何收集性能数据以及……

[towardsdatascience.com](https://example.org/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9?source=post_page-----fc00027950a3--------------------------------)

在 [Articles — SQLBI](https://www.sqlbi.com/articles/) 上，你可以找到关于这些函数的更多深入文章以及为什么要选择一个函数而不是另一个。

例如，SUMMARIZE() 函数的问题在这里有记录：

+   [SUMMARIZE 的所有秘密 (SQLBI.com)](https://www.sqlbi.com/articles/all-the-secrets-of-summarize/)

+   [使用 SUMMARIZE 和 ADDCOLUMNS 的最佳实践 (SQLBI.com)](https://www.sqlbi.com/articles/best-practices-using-summarize-and-addcolumns/)

我使用 Contoso 示例数据集，就像我之前的文章中一样。你可以从 Microsoft [这里](https://www.microsoft.com/en-us/download/details.aspx?id=18279) 免费下载 ContosoRetailDW 数据集。

Contoso 数据可以在 MIT 许可证下自由使用，如 [此处](https://github.com/microsoft/Power-BI-Embedded-Contoso-Sales-Demo) 所述。

我扩大了数据集，以使 DAX 引擎更加高效地工作。

在线销售表包含 7100 万行（而不是 1260 万行），零售销售表包含 1850 万行（而不是 340 万行）。

[](https://medium.com/@salvatorecagliari/membership?source=post_page-----fc00027950a3--------------------------------) [## 使用我的推荐链接加入 Medium - Salvatore Cagliari

### 阅读 Salvatore Cagliari 的每一个故事（以及 Medium 上成千上万其他作者的故事）。你的会员费用直接...

medium.com](https://medium.com/@salvatorecagliari/membership?source=post_page-----fc00027950a3--------------------------------)
