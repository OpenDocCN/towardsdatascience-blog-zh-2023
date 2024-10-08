# 正确使用 DAX 中的 FILTER

> 原文：[`towardsdatascience.com/how-to-use-filter-in-dax-the-correct-way-eb621b49527a`](https://towardsdatascience.com/how-to-use-filter-in-dax-the-correct-way-eb621b49527a)

## *DAX 中的 FILTER()函数可能难以驾驭。你可能会陷入一些陷阱，导致 DAX 代码的性能不佳。以下是如何使用 FILTER()以及不该如何使用的一些示例。*

[](https://medium.com/@salvatorecagliari?source=post_page-----eb621b49527a--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----eb621b49527a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb621b49527a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb621b49527a--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----eb621b49527a--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb621b49527a--------------------------------) ·阅读时长 11 分钟·2023 年 2 月 13 日

--

![](img/e1e8439b16701f24cc9fe52f0c54ec2a.png)

图片由[Andrea De Santis](https://unsplash.com/@santesson89?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供

# 介绍

一年多前，我写了一篇关于[FILTER()](https://dax.guide/filter/)函数的文章：

[](/discover-the-power-of-filter-in-dax-4bfeac3dd786?source=post_page-----eb621b49527a--------------------------------) ## 发现 DAX 中 FILTER()的强大功能

### FILTER()函数在 DAX 中非常强大，但它有一些复杂之处。让我们深入探讨这些细节，以构建一个良好的…

towardsdatascience.com

我在那里解释了一些关于这个强大函数的细节。

我没有做的是解释和展示 FILTER()的一些性能相关细节。

如果你对这个函数不熟悉或不确定如何使用它，可以跳到那篇文章以获得基本了解。

我的旧文和这篇文章之间有一些冗余，但学习基础知识总是正确的。

除了展示如何使用 FILTER()函数外，我还将展示每种变体对性能和效率的影响。

为此，我将使用[DAX Studio](https://daxstudio.org/)和 DAX Studio 中的服务器时间来获取性能统计数据。

如果你不知道 DAX Studio 中的这个功能，或者对那里显示的度量解释不确定，可以阅读这篇文章，我将在其中深入探讨这个功能：

[](/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9?source=post_page-----eb621b49527a--------------------------------) ## 如何使用 DAX Studio 从 Power BI 获取性能数据

### 有时我们有一个慢报表，需要找出原因。我们将看到如何收集性能数据以及……

towardsdatascience.com

# 基本查询

为了确定我们的起点，我在我的演示数据集中定义了一个基本查询（详见参考部分）：

我想获取国家和相应的销售数据，但我希望将销售产品的品牌限制为这三个：

+   Contoso

+   Northwind Traders

+   Fabrikam

基本查询如下：

```py
// Basic Query with the Measure using a direct Filter
 DEFINE
   MEASURE 'All Measures'[RestrictedRetailSales] =
          VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

          VAR Result = 
              CALCULATE(
                  [Sum Online Sales]
                  ,'Product'[BrandName] IN ListOfBrands
                  )

 RETURN
   Result

EVALUATE
    CALCULATETABLE(
            ADDCOLUMNS(
                SUMMARIZE(Geography, Geography[RegionCountryName])
            ,"Restricted Online Sales", [RestrictedRetailSales])
            )
```

我将展示如何更改行 ‘Product’[BrandName] IN ListOfBrands 并查看这些更改的后果。

结果如下（片段）：

![](img/3d057bfbeb5a98a7ea4d74e090a042d0.png)

图 1 — 基本查询的部分结果（图由作者提供）

我们得到 35 行所有已知国家的值，包括中国、德国和美国。

基本查询的服务器时间如下：

![](img/e4f36006e0520f8139d94717db8bb620.png)

图 2 — 基本查询的服务器时间（图由作者提供）

查询执行得相当快，持续时间为 0.3 秒，效率也不错，因为 91.6%的时间花费在存储引擎上。

好的，让我们做一些修改并查看后果。

# 第一个变体 — 引入 FILTER()

这是查询的第一个变体：

```py
// Query with the Measure using a Filter using the FILTER Function on the table
DEFINE
  MEASURE 'All Measures'[RestrictedRetailSales] =
      VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

      VAR Result = 
            CALCULATE(
                [Sum Online Sales]
                ,FILTER('Product'
                      ,'Product'[BrandName] IN ListOfBrands)
                      )

RETURN
  Result

EVALUATE
    CALCULATETABLE(
            ADDCOLUMNS(
                SUMMARIZE(Geography, Geography[RegionCountryName])
            ,"Restricted Online Sales", [RestrictedRetailSales])
            )
```

这次，我在产品表上添加了一个过滤器，以根据这三个品牌进行过滤。

这里是服务器时间：

![](img/e161f30218fb4ddbdf3846dcc0e16cba.png)

图 3 — 第一个变体的服务器时间（图由作者提供）

总执行时间几乎相同，但存储引擎所花费的时间多了 100 毫秒。

并且，我们有五个存储引擎查询，而不是三个。

这些查询中的两个使用了 CallbackDataID 调用，这意味着存储引擎调用公式引擎执行其无法完成的操作。

在这种情况下，品牌列表创建了两次进行过滤。

DAX Studio 将包含 CallbackDataID 的查询标记为粗体，因为此调用不适合性能和效率。

此外，前两个查询各返回了超过 2’500 行。这称为物化。

我们应该尽量减少 DAX 表达式中的物化大小。

在这种情况下，2’517 是整个产品表的行数，这意味着公式引擎获取了整个表格，并需要随后进行过滤。

想象一下我们有一个包含数十万行的表。这个变体将加载整个表格，所需时间为：

· 很多内存

· 很多时间

好的，使用 FILTER() 这种方式完全不可取。

# 第二个变体 — 使用 ALL() 的 FILTER

使用以下变体，我们将[ALL()](https://dax.guide/all/)函数添加到 FILTER()调用中。

`ALL()` 函数移除给定表或列上的任何过滤器，并返回一个包含所有值的表。

```py
// Query with the Measure using a Filter using the FILTER Function on the Product[Brand] column with ALL()
  DEFINE
    MEASURE 'All Measures'[RestrictedRetailSales] =
      VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

      VAR Result = 
            CALCULATE(
              [Sum Online Sales]
              ,FILTER(ALL('Product'[BrandName])
                      ,'Product'[BrandName] IN ListOfBrands)
                      )

RETURN
  Result

EVALUATE
    CALCULATETABLE(
            ADDCOLUMNS(
                SUMMARIZE(Geography, Geography[RegionCountryName])
            ,"Restricted Online Sales", [RestrictedRetailSales])
            )
```

结果仍然相同，但服务器时间与第一种变体的差异很大：

![](img/3c44887a5a2dbd72f13de0c741cbf863.png)

图 4 — 第二种变体使用 ALL() 的服务器时间（图由作者提供）

使用这个变体，我们得到的查询和时间几乎与原始查询相同，而无需使用 FILTER()。

如果我们仔细观察，我们会发现存储引擎查询在两者之间是相同的。

因此，这两个查询在性能和效率上是等效的。

这非常有趣，因为它展示了两个引擎的智能。

差异如下：

+   基础和第二种变体替换了品牌列上的过滤器。

+   第一种变体添加了对品牌列的过滤器，而没有改变现有的过滤器上下文。

    这会导致额外的查询。

由你决定使用哪种变体。作为一个懒人，我更喜欢输入更少的代码来获得相同的结果。

# 第三种变体——添加 VALUES() 代替 ALL()

让我们将 ALL() 函数替换为[VALUES()](https://dax.guide/values/)函数。

VALUES() 函数对于一个列返回一个包含源列唯一值（排除重复项）的表。

```py
// Query with the Measure using a Filter using the FILTER Function on the Product[Brand] column with VALUES()
DEFINE
  MEASURE 'All Measures'[RestrictedRetailSales] =
      VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

      VAR Result = 
            CALCULATE(
                [Sum Online Sales]
                ,FILTER(VALUES('Product'[BrandName])
                      ,'Product'[BrandName] IN ListOfBrands)
                      )

RETURN
  Result

EVALUATE
    CALCULATETABLE(
          ADDCOLUMNS(
              SUMMARIZE(Geography, Geography[RegionCountryName])
          ,"Restricted Online Sales", [RestrictedRetailSales])
          )
```

结果仍然相同：35 行所有国家和三个值。

这些是服务器时间：

![](img/a1425af18116324913c4089235b45031.png)

图 5 — 第三种变体使用 VALUES() 的服务器时间（图由作者提供）

再次，我们看到相同的三个 SE（存储引擎）查询（与基础和第二个查询看起来相同）和几乎相同的持续时间。

数字在执行之间变化很小，但它们与基础查询非常相似。

这表明使用 VALUES() 就像使用简单的过滤谓词或使用 FILTER() 与 ALL() 相同。

# 第四种变体——添加一个额外的过滤器

除了过滤品牌外，我还想按产品颜色进行过滤：

```py
// Query with the Measure using a Filter using the FILTER Function on the Product[Brand] column with ALL()
// This time with two columns
DEFINE
    MEASURE 'All Measures'[RestrictedRetailSales] =
      VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

      VAR Result = 
            CALCULATE(
                [Sum Online Sales]
                ,FILTER(ALL('Product'[BrandName], 'Product'[ColorName])
                        ,'Product'[BrandName] = "Contoso"
                           && 'Product'[ColorName] = "Red"
                        )
                    )

RETURN
  Result

EVALUATE
    CALCULATETABLE(
            ADDCOLUMNS(
                SUMMARIZE(Geography, Geography[RegionCountryName])
                ,"Restricted Online Sales", [RestrictedRetailSales])
                )
```

当然，查询结果中的数字会变化，因为我们只想获取红色产品的销售数据。

以下是服务器时间：

![](img/51426980700d7e8e7cb51cb92dfcd9ee.png)

图 6 — 第四种变体过滤两个列的服务器时间（图由作者提供）

结果很吸引人。

查询比之前快得多，并且只使用了两个 SE 查询。

如上图所示，我们得到一个查询，该查询在产品表中使用一个 WHERE 过滤器来过滤两个列。

这意味着 SE 在组合一个表上的过滤器时非常智能，因为它消除了第一个查询，只执行了两个而不是三个查询。

# 第五种变体——从两个表中过滤列

下一步是尝试从两个不同的表中过滤两个列：

+   品牌 = “Contoso” 在产品表中

+   大陆 = “欧洲” 来自客户表

这是满足这一要求的起点。

```py
// Query with the Measure using a Filter using the FILTER Function on the Product[Brand] column with VALUES()
// This time with two columns from two different tables
DEFINE
    MEASURE 'All Measures'[RestrictedRetailSales] =
      VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

      VAR Result = 
          CALCULATE(
              [Sum Online Sales]
              ,FILTER(ALL('Product'[BrandName])
                  ,'Product'[BrandName] = "Contoso"
                    && 'Customer'[ContinentName] = "Europe" - < Why this cannot function? )
                )

RETURN
  Result

EVALUATE
  CALCULATETABLE(
        ADDCOLUMNS(
              SUMMARIZE(Geography, Geography[RegionCountryName])
        ,"Restricted Online Sales", [RestrictedRetailSales])
        )
```

这不起作用：

![](img/d5bc07c6d0323568bf540a95bc82dc8c.png)

图 7 — 使用 FILTER()时过滤来自不同表的列的错误（图由作者提供）

不能工作的原因是第二列来自另一个不属于 FILTER 函数输入的表：

```py
FILTER(ALL('Product'[BrandName]) <--Input table
         ,'Product'[BrandName] = "Contoso"
         & 'Customer'[ContinentName] = "Europe" <-- Not part of input table
        )
```

让我们尝试另一种方法：

```py
// Query with the Measure using a direct Filter in CALCULATE, but without FILTER
// Again with two columns from two different tables
DEFINE
    MEASURE 'All Measures'[RestrictedRetailSales] =
      VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

      VAR Result = 
          CALCULATE(
              [Sum Online Sales]
              ,'Product'[BrandName] = "Contoso"
              ,'Customer'[ContinentName] = "Europe" - < This works, But without FILTER()
              )

RETURN
  Result

EVALUATE
  CALCULATETABLE(
      ADDCOLUMNS(
            SUMMARIZE(Geography, Geography[RegionCountryName])
      ,"Restricted Online Sales", [RestrictedRetailSales])
      )
```

这次我们使用直接过滤器（谓词），不使用 FILTER()函数。

这种方法有效，并且执行速度非常快且高效：

![](img/87864b168a478b59f45e8c700c4d243e.png)

图 8 — 第五个变体的服务器时间，使用直接过滤器（图由作者提供）

但我们想看到其他内容。

我们如何在来自不同表的列中使用 FILTER 函数？

# 第六个变体 — 为 FILTER()构建表

我们必须从不同的源表中构建一个输入表以实现这一点。

为此，我们使用[SUMMARIZECOLUMNS()](https://dax.guide/summarizecolumns/)来生成一个结合给定列中值的表：

```py
// Query with the Measure using a Filter using the FILTER Function with two columns from two separate tables
DEFINE
  MEASURE 'All Measures'[RestrictedRetailSales] =
    VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

    VAR Result = 
        CALCULATE(
            [Sum Online Sales]
            ,FILTER(SUMMARIZECOLUMNS('Product'[BrandName]
                                    ,'Customer'[Continent])
                ,'Product'[BrandName] = "Contoso"
                && 'Customer'[Continent] = "Europe" - < This works, as we build a table with SUMMARIZECOLUMN() as the input for FILTER()
                )
            )

RETURN
  Result

EVALUATE
  CALCULATETABLE(
        ADDCOLUMNS(
            SUMMARIZE(Geography, Geography[RegionCountryName])
        ,"Restricted Online Sales", [RestrictedRetailSales])
        )
```

SE 可以将这个变体解释为执行仅一个查询以获得结果（第二个查询仅检索国家列表）：

![](img/5a223ba1d4d5eb3c036ee2f7f46b0580.png)

图 9 — 第六个变体的服务器时间和 SE 查询（图由作者提供）

此外，总执行时间非常短。

但在我们得出结论之前，我还有一个变体给你。

# 第七个变体 — 使用 CROSSJOIN()进行 FILTER()

最后的变体使用了[CROSSJOIN()](https://dax.guide/crossjoin/)函数。

这个函数返回一个表，其中包含第一个输入表的每一行与第二个表的每一行的乘积。

详细信息请阅读上面链接的[DAX Guide](https://dax.guide)文章或观看视频。

```py
// Query with the Measure using a Filter using the FILTER Function with two columns from two different tables
DEFINE
  MEASURE 'All Measures'[RestrictedRetailSales] =
    VAR ListOfBrands = {"Contoso", "Northwind Traders", "Fabrikam" }

    VAR Result = 
          CALCULATE(
              [Sum Online Sales]
              ,FILTER(CROSSJOIN(ALL('Product'[BrandName])
                                ,ALL('Customer'[ContinentName]))
                    ,'Product'[BrandName] = "Contoso"
                    && 'Customer'[ContinentName] = "Europe" - < This works, as we build a table with SUMMARIZECOLUMN() as the input for FILTER()
                    )
              )

RETURN
  Result

EVALUATE
  CALCULATETABLE(
        ADDCOLUMNS(
              SUMMARIZE(Geography, Geography[RegionCountryName])
          ,"Restricted Online Sales", [RestrictedRetailSales])
          )
```

结果仍然没有改变。但执行模式却大相径庭：

![](img/ea9c0caaf65539136b2e511382e94ce6.png)

图 10 — 使用 CROSSJOIN()的第七个变体的服务器时间（图由作者提供）

尽管总执行时间没有多长，但现在我们有四个 SE 查询。

但前两个查询执行速度极快。

第一个查询检索所有大洲的列表（6 行），第二个查询检索所有品牌的列表（14 行）。

第三和第四个查询与之前的变体相同。

但这个变体还有更多需要考虑的地方：

1.  Cross-Join 由公式引擎（FE）完成。对于如此少量的值来说，这不是问题。但当你有数百或数千个值时，这将花费大量时间。

1.  这个变体生成一个包含两个列（品牌和大洲）的表，其中包含所有可能的组合。该表用两个谓词（Brand = “Contoso”和 Continent = “Europe”）进行了过滤。最后，生成的表被作为过滤器应用于 CALCULATE()函数。

1.  使用 CROSSJOIN()函数必须配合使用 ALL()函数。如果不使用 ALL()函数，你将会遇到错误。作为替代方案，可以使用 VALUES()函数，它会产生相同的结果。

1.  这种变体需要大量编写而没有任何好处，因为 SE 查询与上述两个变体中的查询相同。但当你需要以这种特定方式构造 FILTER()的输入表时，现在你知道这是可能的。

![](img/51fb3e2b8cefedb2439a062b7845cc34.png)

由[Afif Ramdhasuma](https://unsplash.com/ko/@javaistan?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)拍摄的照片

# 结论

正如你所见，使用 DAX 中的 FILTER 函数有很多可能的变体。

但请注意。在我的示例中看起来很快的操作，在你的场景中可能会大相径庭，因为你可能会面临不同的数据情况。

你需要过滤的列中独特的值越多，你就越需要注意如何使用 FILTER()。

在我看来，直接过滤的方法是第一种也是最有效的方法，因为它需要最少的努力和输入。在大多数情况下，它可以正常工作。

所以，不要过度工作或过度思考你的度量值，因为最简单和直接的方法可能是正确的。

如果你想了解更多关于这种方法的信息，可以阅读以下内容：

[## 不要从优化代码开始。这可能不是一个好主意](https://medium.com/codex/dont-start-with-optimized-code-it-may-not-be-a-good-idea-2e9c6afa85a1?source=post_page-----eb621b49527a--------------------------------)

### 这篇文章的标题似乎违背直觉。为什么你不应该用最优化的方式来开发你的解决方案……

[medium.com](https://medium.com/codex/dont-start-with-optimized-code-it-may-not-be-a-good-idea-2e9c6afa85a1?source=post_page-----eb621b49527a--------------------------------)

# 参考文献

我使用了 Contoso 示例数据集，就像我之前的文章中提到的那样。你可以从微软[这里](https://www.microsoft.com/en-us/download/details.aspx?id=18279)免费下载 ContosoRetailDW 数据集。

Contoso 数据可以在 MIT 许可证下自由使用，如[这里](https://github.com/microsoft/Power-BI-Embedded-Contoso-Sales-Demo)所述。

我扩大了数据集，以使 DAX 引擎更加费劲。

在线销售表包含 7100 万行（而不是 1260 万行），零售销售表包含 1850 万行（而不是 340 万行）。

[## 通过我的推荐链接加入 Medium - Salvatore Cagliari](https://medium.com/@salvatorecagliari/membership?source=post_page-----eb621b49527a--------------------------------)

### 阅读 Salvatore Cagliari 的每一个故事（以及 Medium 上其他成千上万的作家）。你的会员费直接……

[medium.com](https://medium.com/@salvatorecagliari/membership?source=post_page-----eb621b49527a--------------------------------)
