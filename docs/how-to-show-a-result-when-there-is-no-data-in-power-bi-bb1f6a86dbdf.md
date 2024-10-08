# 如何在 Power BI 中显示没有数据的结果

> 原文：[`towardsdatascience.com/how-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf`](https://towardsdatascience.com/how-to-show-a-result-when-there-is-no-data-in-power-bi-bb1f6a86dbdf)

## *这个标题似乎有些反直觉。为什么在没有数据的情况下我还要请求结果？继续了解一下客户的要求以及我如何解决这个问题。*

[](https://medium.com/@salvatorecagliari?source=post_page-----bb1f6a86dbdf--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----bb1f6a86dbdf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb1f6a86dbdf--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb1f6a86dbdf--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----bb1f6a86dbdf--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb1f6a86dbdf--------------------------------) ·9 分钟阅读·2023 年 1 月 5 日

--

![](img/0a11a590227f8428e4480b4174093434.png)

图片由 [Emily Morter](https://unsplash.com/@emilymorter?utm_source=medium&utm_medium=referral) 供图，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

我的一个客户有一个数据集，其中包含任务、子任务和每个任务的度量值。

此外，每个任务都有一个状态。

这些状态是：

+   完成

+   进行中

+   待处理

+   已取消

+   失败

将数据导入 Power BI 后，结果如下：

![](img/a52ffabefcdfcfaf9d20e6c543bbcdb5.png)

图 1 — 第一个结果（作者提供的图）

但是，有时对于特定状态可能没有任务或子任务。

在这种情况下，结果可能是这样的：

![](img/5d5290eff3f4371c087e9e5270f87c45.png)

图 2 — 缺少状态的结果（作者提供的图）

客户有以下两个要求：

1.  每当没有结果（空白）时，他希望看到一个零

1.  他总是想看到所有状态，包括那些没有任务且结果为零的状态

很简单，不是吗？

好吧，我们来看看。

# 第一次尝试

第一个方法是编写一个如下的度量值：

```py
Costs with zero =
   VAR Costs = SUM('Demo Data 2'[Cost])
RETURN
   IF(ISBLANK( Costs ), 0, Costs)
```

正如你所想，结果并不是我预期的。如果结果如我所想，我不会坐在这里写一篇文章。

结果是一样的。

为什么？

为了找出答案，我将度量值改为：

```py
Costs with zero =
   VAR Costs = SUM('Demo Data 2'[Cost])
RETURN
   ISBLANK( Costs )
```

现在，我可以看到当 *Costs* 变量用 [ISBLANK()](https://dax.guide/isblank/) 函数检查时发生了什么。

结果是这样的：

![](img/0025ffa909606acdb8ea3f1c32d4e547.png)

图 3 — 使用 ISBLANK() 的结果（作者提供的图）

起初，这很奇怪。

然后我明白了：当表中没有行时，没有筛选上下文。因此，度量值不会被评估，我们不会看到任何结果。

为了解决这个问题，我明白我必须填补数据缺口，以便使用上面显示的度量值，并在度量值返回 NULL 时得到零。

# 准备工作

为了展示这个案例，我在 Excel 中创建了两个表格并将其导入到 Power BI Desktop 中：

1.  演示数据：包含所有状态的行（上面的表格来自这些数据）

1.  演示数据 2：与演示数据相同的数据，但行被重新分配，并且没有行的状态为失败

为了解决这个挑战，我使用了演示数据表来满足第一个要求。

之后，我使用演示数据 2 表来满足第二个要求。

# 用 Power Query 填补缺口

为了填补缺口，我需要为每个任务和步骤以及每个可能的状态保留一行。

这次我决定在 Power Query 中执行，以遵循以下规则（[罗氏准则](https://ssbipolar.com/2021/05/31/roches-maxim/)）：

*尽早转换数据并尽晚转换数据*

我不能在源中完成这项工作，因为这意味着要更改源数据，这是不可行的，我希望自动化转换过程。

出于这些原因，Power Query 是一个自然的选择。

所需的步骤如下：

1.  提取所有可能的状态列表

1.  提取所有任务和步骤的列表，不包括带有值列的列

1.  将状态列表与任务和步骤列表相乘（交叉连接）

1.  将生成的列表附加到原始表中

好了，开始吧：

我启动了 Power Query 编辑器，并创建了两个演示数据表的副本：

![](img/f017df9601468c633efcca79a7d15ed3.png)

图 4 — 复制演示数据表（图由作者提供）

我将一个表重命名为状态，另一个表重命名为任务。

接下来，我从这些表中删除所有不需要的列：

![](img/daa935706982610ab6b179674e18dc4c.png)

图 5 — 删除列以保留所需列（图由作者提供）

在任务中，我只保留任务名称和步骤列。

在你的情况下，你必须保留定义表中唯一组合的所有列。在我的客户的情况下，我必须保留任务/子任务所有者的名字和其他列。

在状态表中，我保留状态和状态顺序列。

为了获得唯一行的列表，我从两个表中删除重复项：

![](img/05af3e824ebc530770b2eac403868f88.png)

图 6 — 删除重复项（图由作者提供）

为了将任务表中的行与所有状态相乘，我在任务表中添加了一个自定义列：

![](img/783bb014b79990b99d15b8b447b9937b.png)

图 7 — 添加状态列（图由作者提供）

接下来，我必须展开新列以添加两个列：

![](img/cbfda302e87207cf44d1b6c618ddb500.png)

图 8 — 展开状态列（图由作者提供）

结果表格如下所示：

![](img/d30faedc64065c968b426ba9710953a6.png)

图 9 — 扩展状态表后的结果（图由作者提供）

我在这篇文章中详细解释了这个技术：

[](https://medium.com/the-techlife/use-power-bi-to-transpose-yearly-numbers-to-months-d538bcb7b9c2?source=post_page-----bb1f6a86dbdf--------------------------------) [## 使用 Power BI 将年度数据转换为月份数据

### 当你有年度数据时，你可能需要将其转换为月度数据。现在，我将展示如何使用 Power…

medium.com](https://medium.com/the-techlife/use-power-bi-to-transpose-yearly-numbers-to-months-d538bcb7b9c2?source=post_page-----bb1f6a86dbdf--------------------------------)

在将这个表追加到演示数据表之前，我必须添加缺失的成本列，值为 NULL：

![](img/3b3ec2922cf59d66218fbd00867dc647.png)

图 10 — 添加成本列（图由作者提供）

我必须为新建的成本列保持 NULL，以避免在需要计算平均值或其他统计数据时出现错误结果。

结果表与演示数据表具有相同的结构。

现在我可以将这个表追加到演示数据表中：

![](img/39d93ef3d87bd6f021d1d592a5e6ea53.png)

图 11 — 添加任务表（图由作者提供）

结果表如下所示：

![](img/7877cce161ebe857925050d7e3895964.png)

图 12 — 附加数据的演示数据表（图由作者提供）

正如你可能注意到的，成本列的数据类型不正确。我必须将数据类型更改为数值型：

![](img/765a95976a4e02e490ad82eece6c57f7.png)

图 13 — 将成本列转换为数值型（图由作者提供）

最后，由于我在 Power BI 中不需要任务和状态表，所以我禁用了这两个表的加载：

![](img/692fd30bbd2a686514c4343507c5c0c2.png)

图 14 — 禁用两个新增表的加载（图由作者提供）

在这个过程中，你可能会问自己，“他为什么没有为演示数据表创建引用？”。

好吧，我本来无法将引用的表追加到作为引用表源的表中。这会创建一个循环依赖，这是不允许的。因此，我不得不复制表。

在关闭 Power Query 编辑器并加载数据后，我得到了以下结果，使用了上述测量值：

![](img/554cb10a67bd250cbdd6b08a5d8d1a4d.png)

图 15 — 用零替代空白单元格的结果（图由作者提供）

现在，第一个要求已经满足。

# 添加缺失的状态

当我对“失败”状态的缺失行重复上述所有步骤时，我在状态表中得到了以下结果：

![](img/f8f082473b49e382c354c4f3bd5905a0.png)

图 16 — 缺少失败状态的状态表（图由作者提供）

为了解决这个问题，我必须添加一个包含所有可能状态的手动表（输入数据按钮）：

![](img/2338ab01a1e9514a27abb8dac1c5d735.png)

图 17 — 创建手动状态表（图由作者提供）

下一步是将这个手动表追加到状态表中，并删除所有重复项：

![](img/d036e2dd9cd07305384e01eae9cb693c.png)

图 18 — 完整的状态表，包含所有已知状态（图由作者提供）

但我为什么需要将手动表追加到现有的状态表中？

我这样做是以防我们得到一个新的状态，这样数据仍然会完整。

我只需要将新状态添加到手动表中。

手动表的替代方案是一个包含所有可能状态的 Excel 表格。这样，你就不需要每次添加或更改状态时都修改 Power BI 文件。

其余步骤保持不变，结果如下：

![](img/005b109dbb8af1a6aee71bb5af9f0ff0.png)

图 19 — 所有状态填充为零的结果（图由作者提供）

这一步满足了第二个要求。

# 修改模型的大小

你可能会想，“但这样，我们会把表扩大很多行？”。

是的，确实如此。

但正如开头所述：当没有数据时，没有过滤器上下文，也没有度量被评估。

所以，我们必须将这些行添加到我们的数据中。

但这有什么后果呢？

我用 DAX Studio 提取了原始数据模型和扩展模型的度量。

这些是结果：

![](img/dbd0f136b7a16407dfa7b2187f34d6e0.png)

图 20 — 两个数据模型的度量对比（图由作者提供）

首先，查看每个表的基数列。

我们知道原始表只有 27 行，而扩展后的表有 162 行。

这种增长是巨大的。

但当我们查看总大小列时，差异是微小的。

数据大小是原来的两倍多，但字典大小保持不变。为什么？

字典存储了所有文本列的信息。文本越多，字典就越大。

基数是关键度量（列中不同值的数量）。

因为这个度量在两个版本之间没有变化，所以字典不需要更多空间，即使数据需要更多空间。

你的数据可能具有更高的基数，这可能会有所不同。但我相信字典占用了大量空间。

差异可能仍然很小。

查看 ContosoRetail 数据模型中一些维度表的度量：

![](img/9dff71ab49ad918b8e4dcd588382f2d3.png)

图 21 — ContosoRetail 数据模型的度量（图由作者提供）

你会看到字典总是比数据大得多。

由于字典存储了每一列所有不同字符串值的编码列表，它会随着不同值的数量（更高的基数）而增长。但只要基数保持不变，即使表中的行数增加，它也会相对稳定。

# DAX 与 Power Query

但我们为什么不使用 DAX 来做这个？

是的，我们也可以用 DAX 表解决上述要求。

但这种方法也有一些反对的观点：

+   没有不必要的表

    我们可以在 Power Query 中禁用加载到 Power BI 中。这样，我们可以避免在数据模型中有不必要的表，从而节省内存而无任何好处。

+   更易于实现：

    在 Power Query 中完成这些操作要比使用 DAX 表更容易。

+   可能更好的数据压缩

我需要深入研究最后一点：

每当 Power BI 将数据加载到数据模型中时，引擎都会执行广泛的数据分析和分析，并优化数据以实现最佳压缩率。

在参考文献部分，我添加了 Marco Russo (SQLBI) 关于优化 Tabular 模型的视频，他解释了在数据刷新期间引擎执行的优化步骤。

由于 Power BI 引擎从 Power Query 中获取数据，因此它无法区分数据的来源。即使数据是在 Power Query 中生成的，它也会执行所有可能的优化以获取最有效的数据存储。

但在最坏的情况下，Power BI 中的计算表可能无法像 Power Query 中的表一样进行优化。

# 结论

我们都知道，在正确的时间选择正确的工具是至关重要的。

正如你在这里看到的，当选择使用 DAX 还是 Power Query 的解决方案时，这一点尤为关键。

当转换或修改数据时，如果你不能在数据源中完成，比如在数据库上进行查询，Power Query 是合适的工具。

使用图形用户界面提供的所有工具，实施复杂解决方案变得更加容易。

所以，没有理由不开始学习 Power Query 及其可能性。

![](img/1a88c076b44c7c04f08b8de8d0a8b696.png)

[Guille Álvarez](https://unsplash.com/@guillealvarez?utm_source=medium&utm_medium=referral) 的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 参考文献

Guy in a Cube 关于 Roche 数据转换原则的视频：

YouTube 视频中，Marco Russo 解释了如何优化 Tabular 数据模型：

数据集是虚构的，并且是在 Excel 中从头创建的。

我使用了 Contoso 示例数据集，就像在我之前的文章中一样。你可以从 Microsoft [这里](https://www.microsoft.com/en-us/download/details.aspx?id=18279) 免费下载 ContosoRetailDW 数据集。

Contoso 数据可以在 MIT 许可下自由使用，如 [这里](https://github.com/microsoft/Power-BI-Embedded-Contoso-Sales-Demo) 所述。

我扩大了数据集，以使 DAX 引擎工作得更努力。

在线销售表包含 7100 万行（而不是 1260 万行），零售销售表包含 1850 万行（而不是 340 万行）。

[](https://medium.com/@salvatorecagliari/membership?source=post_page-----bb1f6a86dbdf--------------------------------) [## 使用我的推荐链接加入 Medium - Salvatore Cagliari

### 阅读 Salvatore Cagliari 的每一篇故事（以及 Medium 上成千上万其他作家的故事）。你的会员费直接……

[medium.com](https://medium.com/@salvatorecagliari/membership?source=post_page-----bb1f6a86dbdf--------------------------------)
