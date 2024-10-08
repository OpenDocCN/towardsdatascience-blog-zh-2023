# 在 DAX 度量中使用中间结果

> 原文：[`towardsdatascience.com/on-using-intermediary-results-in-dax-measures-9971efa72ae`](https://towardsdatascience.com/on-using-intermediary-results-in-dax-measures-9971efa72ae)

## *我们在 DAX 中一直使用表变量。但当我们需要计算中间结果并在 DAX 度量中稍后重用它们时，该怎么办？这个挑战听起来简单，但实际上并不容易。*

[](https://medium.com/@salvatorecagliari?source=post_page-----9971efa72ae--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----9971efa72ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9971efa72ae--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9971efa72ae--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----9971efa72ae--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9971efa72ae--------------------------------) ·8 分钟阅读·2023 年 3 月 13 日

--

![](img/56e23375995cfedbed0a540dd53def90.png)

由 [Mika Baumeister](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral) 提供的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

我们在 DAX 中一直使用中间表变量。

例如，查看以下度量：

```py
[SalesYTD] =
  VAR YTDDates = DATESYTD('Date'[Date])

RETURN
  CALCULATE(
      [Sum Online Sales]
      ,YTDDates
      )
```

在这种情况下，我们生成一个基于实际筛选上下文的年初至今表，借助于[DATESYTD()](https://dax.guide/datesytd/)函数，其中包含了从实际筛选上下文中的年初（即 1 月 1 日）到当前日期的所有日期（当然是基于当前的筛选上下文）。

但有时，我们需要做更多的事情。

例如，我们需要根据当前的筛选上下文查询一个表，并对结果进行进一步计算。

在这种情况下，我们必须生成一个中间表，并将结果分配给度量中的一个变量，以进行所需的计算。

由于我们需要筛选上下文，我们不能预先创建一个包含这些中间结果的表，因为这个表会非常庞大，以容纳所有可能的筛选组合。

所以，让我们来看看我们可以如何做到这一点。

# 基础查询

基础查询如下：

```py
EVALUATE
  ADDCOLUMNS(
        VALUES(Customer[CustomerKey])
          ,"AverageSales", CALCULATE( AVERAGEX( 'Online Sales'
          ,('Online Sales'[UnitPrice] * 'Online Sales'[SalesQuantity])-'Online Sales'[DiscountAmount]
          )
        )
      )
ORDER BY [AverageSales] DESC
```

这个查询的（截断）结果如下：

![](img/fcb5f7a619471291932e62a9a536de3b.png)

图 1 — 基础查询结果（图示作者提供）

但是如果这仅仅是一个起点，我们还需要在一个度量中基于这个结果进行进一步的计算，会发生什么呢？

例如，我们想要显示每个月所有行的总和。

在这种情况下，我们需要计算每个月的平均销售额，并对结果进行求和。

让我们看看我将如何直观地创建一个解决方案，它不起作用，然后，之后这个解决方案将如何有效。

# 不应该这样做——或者说它不工作的原因

解决上述需求的第一种直观方法是使用 ADDCOLUMNS() 函数生成一个变量，预计算表格中的平均销售额。然后对行进行聚合以获得所需的结果：

```py
DEFINE
  MEASURE 'All Measures'[AverageSalePerCustomer] =
    VAR AverageSalesPerCust =
            ADDCOLUMNS (
                VALUES ( Customer[CustomerKey] ),
                "AverageSales",
                -- Calculate the Average Sales per Customer using the Filter Context provided by ADDCOLUMNS()
                CALCULATE (
                  AVERAGEX (
                    'Online Sales',
                    ( 'Online Sales'[UnitPrice] * 'Online Sales'[SalesQuantity] ) - 'Online Sales'[DiscountAmount]
                    )
                  )
                )
RETURN
  -- Calculate a sum of all the rows calculated in the previous step
  SUM ( AverageSalesPerCust[AverageSales] )

EVALUATE
  -- Get the list of all Months and call the Measures defined above for each month
  ADDCOLUMNS (
      SUMMARIZECOLUMNS (
            'Date'[Year Month Short Name] 
            ),
            "AverageSalePerCustomer", [AverageSalePerCustomer]
      )
ORDER BY 'Date'[Year Month Short Name]
```

在[DAX Studio](https://www.sqlbi.com/tools/dax-studio/)中执行此查询后，我得到以下错误信息：

![](img/32d97083929e60a6dfe7976e58312d2e.png)

图 2 — 第一次尝试的错误信息（作者提供的图）

问题在于 SUM() 无法与表变量一起使用。

让我们用稍微不同的方式尝试一下。

让我们在 RETURN 语句后用[SUMX()](https://dax.guide/sumx/) 替换 SUM()：

```py
SUMX(AverageSalesPerCust
    ,[AverageSales])
```

错误信息如下：

![](img/16b8ecb1f882586ddff1073f20cded94.png)

图 3 — 使用第二种方法的错误信息（作者提供的图）

所以，SUMX() 也无法访问表变量中的列。

我尝试了其他方法来获得所需的结果：

+   使用[CALCULATETABLE()](https://dax.guide/calculatetable/) 生成可以被 SUM() 或 SUMX() 使用的表

    但这个函数也无法访问表变量中的列。

+   使用[FILTER()](https://dax.guide/filter/) 生成表并在 SUM() 中使用

    尽管 FILTER() 没有问题，但 SUM() 无法访问表变量。

但等等…… FILTER() 没有生成任何错误。

我可以将 FILTER() 与 SUMX() 一起使用吗？

让我们看看这种方法是否有效。

# 有效解决方案 — FILTER() 和 SUMX()

第一个有效的方法是以下查询：

```py
DEFINE
  MEASURE 'All Measures'[AverageSalePerCustomer] =
    VAR AverageSalesPerCust =
      ADDCOLUMNS (
        VALUES ( Customer[CustomerKey] ),
        "AverageSales",
        CALCULATE (
            AVERAGEX (
              'Online Sales',
              ( 'Online Sales'[UnitPrice] * 'Online Sales'[SalesQuantity] ) - 'Online Sales'[DiscountAmount]
              )
            )
          )

    VAR AvgSalesOver0 =
      -- Wrap the intermediary table variable in FILTER() to make it usable by aggregation function
      FILTER (
          AverageSalesPerCust
          ,[AverageSales] > 0
          )

RETURN
  SUMX (
      AvgSalesOver0,
      [AverageSales]
      )

EVALUATE
  ADDCOLUMNS (
      SUMMARIZECOLUMNS (
            'Date'[Year Month Short Name]
            ),
            "AverageSalePerCustomer", [AverageSalePerCustomer]
        )
  -- Order by the calculated column
  ORDER BY [Year Month Short Name] DESC
```

第一个表变量 AverageSalesPerCust 仍然是一样的。

第二步是使用 FILTER() 函数来定义新的表变量 AvgSalesOver0。

更新 2024 年 1 月：正如[AlexisOlson](https://medium.com/u/b20dbba0ee98?source=post_page-----9971efa72ae--------------------------------)的评论中所述，FILTER() 并不是必需的。在另一个案例中，我使用了相同的技巧而不使用 FILTER()，效果良好。目前，我不知道为什么在这里需要 FILTER()。

在这种情况下，我使用 FILTER() 函数将表变量 AverageSalesPerCust 转换为可以被聚合函数使用的表变量。

由于 FILTER() 需要至少两个参数，我必须添加“Filter” *[AverageSales] > 0* 以确保 FILTER 函数能正常工作。

出乎意料的是，SUMX 在访问使用 FILTER() 函数构建的表变量时没有问题，我可以使用这个表变量来计算聚合。

查询的（截断）结果如下：

![](img/c437046584344c9cf912b880cfbaf3ff.png)

图 4 — 工作解决方案的结果（作者提供的图）

但执行需要一些时间。

当我查看性能指标时，我注意到一些问题：

![](img/cbe4d3520dfb745d91115733236fe816.png)

图 5 — 第一个解决方案的性能指标（作者提供的图）

总执行时间超过三秒，其中超过两秒的时间花在了公式引擎（FE）上，因为它需要处理三百万行数据。

这需要更高效，我们必须尝试找到更高效的解决方案。

# 使用`CALCULATETABLE()`的优化尝试

我将`CALCULATETABLE()`与`FILTER()`结合以获得以下解决方案：

```py
DEFINE
  MEASURE 'All Measures'[AverageSalePerCustomer] =
        VAR AverageSalesPerCust =
          CALCULATETABLE (
            ADDCOLUMNS (
                VALUES ( Customer[CustomerKey] ),
                "AverageSales",
                CALCULATE (
                  AVERAGEX (
                    'Online Sales',
                      ( 'Online Sales'[UnitPrice] * 'Online Sales'[SalesQuantity] ) - 'Online Sales'[DiscountAmount]
                    )
                  )
                )
                -- Add a Dummy filter
                ,Customer[CustomerKey] <> 0
              )

VAR AvgSalesOver0 =
    FILTER (
        AverageSalesPerCust,
        [AverageSales] > 0
        )

RETURN
  SUMX (
      AvgSalesOver0
      ,[AverageSales]
      )

EVALUATE
  ADDCOLUMNS (
    SUMMARIZECOLUMNS (
            'Date'[Year Month Short Name]
            ),
            "AverageSalePerCustomer", [AverageSalePerCustomer]
          )
  -- Order by the calculated column is possible
  ORDER BY [Year Month Short Name] DESC
```

区别在于我用`CALCULATETABLE()`函数封装了`ADDCOLUMNS()`。

我添加了一个虚拟筛选器，希望执行计划会有所改变。

但这个版本保持了一切不变。

让我们尝试其他方法。

# 使用虚拟筛选器的优化尝试

下一个想法是用虚拟筛选器替换`FILTER`函数中的谓词：

```py
VAR AvgSalesOver0 =
    FILTER (
          AverageSalesPerCust
          -- Dummy-Filter 
          ,1 = 1
          )
```

假设没有列引用的谓词（1=1）可能会导致更高效的执行。

不幸的是，这并没有改变结果。

# 是否没有优化的可能或必要？

我尝试了其他表函数来构建第一个表变量，即每个客户的平均销售额，但一切保持不变。

经过一番思考，我意识到这个问题无法在 DAX 中解决。

存储引擎（SE）无法按照我们需要的方式工作，或者我未能找到正确的解决方案。

因此，DAX 引擎将依赖于公式引擎的能力，从而将三百万行数据加载到内存中并在其中汇总数据。

有时我们找到的解决方案已经是最好的了。

三秒是用户愿意等待结果的最大时间限制。

我们可以认为这个解决方案已经足够好。

但等一下。我使用包含十多年数据的整个数据集执行度量。这是一个现实的场景吗？

更现实的情况是用户只会分析一两年的数据。

使用一个筛选器将查询限制为仅一年，执行时间骤降至不到两分之一秒。

![](img/8d1d20868d21fd3faac2a2157608858b.png)

图 6 — 仅选择一年的性能指标（图由作者提供）

由于这将是报告中的常见场景，因此解决方案已准备好在报告中使用。

**经验教训：** 记得在测试复杂度量的性能时使用实际场景和用例。

![](img/c4e000b2b0fd5db5cd67c3d6eea3df23.png)

[Lucas Santos](https://unsplash.com/@_staticvoid?utm_source=medium&utm_medium=referral) 拍摄，照片来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 解决方案模板

根据这些结果，解决方案的模板如下：

```py
DEFINE
  MEASURE 'All Measures'[AverageSalePerCustomer] =
    VAR <TableVariable> =
          <Definition of the Table>

    VAR <FilterOfTable> =
        FILTER(
            <TableVariable>
            ,1=1 - Dummy Filter
            )
RETURN
  <AggregationWithXFunction over <FilterOfTable> >
```

另外，我们可以使用这种形式：

```py
DEFINE
MEASURE 'All Measures'[AverageSalePerCustomer] =
    VAR <TableVariable> =
      FILTER(
          <Definition of the Table>
          , 1=1 - Dummy Filter
          )
RETURN
  <AggregationWithXFunction over <TableVariable> >
```

第二种形式更短，并且一切都包含在一个变量中。

我鼓励你添加注释以解释为什么要在包含`FILTER 1=1`的度量中添加`FILTER()`。

对于不了解这种技术的人来说，这毫无意义。

# 参考文献

如果你不知道如何在 DAX Studio 中收集和解释性能指标，或者对那里显示的指标解释不确定，请阅读这篇文章，我将在其中详细探讨这个功能：

[如何使用 DAX Studio 从 Power BI 获取性能数据](https://wiki.example.org/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9?source=post_page-----9971efa72ae--------------------------------)

### 有时我们会遇到报告运行缓慢的情况，我们需要找出原因。我们将看到如何收集性能数据以及…

[towardsdatascience.com](https://wiki.example.org/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9?source=post_page-----9971efa72ae--------------------------------)

我使用了 Contoso 示例数据集，就像我之前的文章中一样。你可以从 Microsoft [这里](https://www.microsoft.com/en-us/download/details.aspx?id=18279) 免费下载 ContosoRetailDW 数据集。

Contoso 数据可以在 MIT 许可下自由使用，如 [这里](https://github.com/microsoft/Power-BI-Embedded-Contoso-Sales-Demo) 所述。

我扩大了数据集，以使 DAX 引擎的工作负荷增加。

在线销售表包含 7100 万行（而不是 1260 万行），零售销售表包含 1850 万行（而不是 340 万行）。

[加入 Medium 并使用我的推荐链接 - Salvatore Cagliari](https://medium.com/@salvatorecagliari/membership?source=post_page-----9971efa72ae--------------------------------)

### 阅读 Salvatore Cagliari（以及 Medium 上其他成千上万的作者）的每一个故事。您的会员费用直接…

[medium.com](https://medium.com/@salvatorecagliari/membership?source=post_page-----9971efa72ae--------------------------------)
