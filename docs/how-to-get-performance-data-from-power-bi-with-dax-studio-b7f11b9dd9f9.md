# 如何使用 DAX Studio 从 Power BI 获取性能数据

> 原文：[`towardsdatascience.com/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9`](https://towardsdatascience.com/how-to-get-performance-data-from-power-bi-with-dax-studio-b7f11b9dd9f9)

## *有时候我们会遇到报告变慢的情况，需要找出原因。我将向你展示如何收集性能数据以及这些指标的含义。*

[](https://medium.com/@salvatorecagliari?source=post_page-----b7f11b9dd9f9--------------------------------)![Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----b7f11b9dd9f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b7f11b9dd9f9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7f11b9dd9f9--------------------------------) [Salvatore Cagliari](https://medium.com/@salvatorecagliari?source=post_page-----b7f11b9dd9f9--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7f11b9dd9f9--------------------------------) ·9 分钟阅读·2023 年 1 月 25 日

--

![](img/be3df699528c306993e55c7a3e5b64cb.png)

图片来源：[Aleks Dorohovich](https://unsplash.com/@doctype?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 简介

说清楚一点：今天我不会讨论如何优化 DAX 代码。

未来会有更多的文章，集中讨论常见错误及如何避免它们。

但在我们能理解性能指标之前，我们需要了解 Power BI 中 Tabular 模型的架构。

相同的架构适用于 SQL Server Analysis Services 中的 Tabular 模型。

任何 Tabular 模型都有两个引擎：

+   存储引擎

+   公式引擎

这两个引擎具有不同的属性，并在 Tabular 模型中执行不同的任务。

让我们来调查一下。

# 存储引擎

存储引擎是 DAX 查询和存储在 Tabular 模型中的数据之间的接口。

该引擎接受任何给定的 DAX 查询，并将查询发送到 Vertipaq 存储引擎，该引擎将数据存储在数据模型中。

存储引擎使用一种叫做 xmSQL 的语言来查询数据模型。

这种语言基于标准 SQL 语言，但功能较少，只支持简单的算术运算符（+、-、/、*、=、<> 和 IN）。

要对数据进行聚合，xmSQL 支持 SUM、MIN、MAX、COUNT 和 DCOUNT（去重计数）。

它支持 GROUP BY、WHERE 和 JOIN。

如果你在尝试理解 xmSQL 时对 SQL 查询有基本的了解，这会有所帮助。如果你不懂 SQL，那么在深入分析性能不佳的 DAX 代码时，学习基础知识会很有帮助。

最重要的事实是，存储引擎是多线程的。

因此，当存储引擎执行查询时，它将使用多个 CPU 核心来加快查询执行。

最后，存储引擎可以缓存查询和结果。

因此，相同查询的重复执行将加快执行速度，因为结果可以从缓存中检索。

# 公式引擎

公式引擎是 DAX 引擎。

存储引擎无法执行的所有函数都由公式引擎执行。

通常，存储引擎从数据模型中检索数据，并将结果传递给公式引擎。

这个操作叫做物化，因为数据被存储在内存中以供公式引擎处理。

正如你所想，避免大规模物化是至关重要的。

当 xmSQL 查询包含存储引擎无法执行的函数时，存储引擎可以调用公式引擎。

这个操作 ID 叫做 CallbackDataID，如果可能的话应该避免。

关键是，公式引擎是单线程的，并且没有缓存。

这意味着：

+   不使用多个 CPU 核心进行并行处理

+   不重复执行相同查询

这意味着我们希望将尽可能多的操作卸载到存储引擎上。

不幸的是，无法直接定义我们的 DAX 代码的哪个部分由哪个引擎执行。我们必须避免特定模式，以确保正确的引擎在最短的时间内完成工作。

这又是另一个可以填满整本书的故事。

那么我们如何看到每个引擎使用了多少时间呢？

# 获取性能数据

我们需要在机器上安装 DAX Studio 以获取性能指标。

我们可以在下面的参考部分找到 DAX Studio 的下载链接。

如果你无法安装软件，可以从同一网站获取便携版 DAX。下载 ZIP 文件并解压到任何本地文件夹中。然后你可以启动 DAXStudio.exe，所有功能将不受限制。

但首先，我们需要从 Power BI 中获取 DAX 查询。

首先，我们需要在 Power BI Desktop 中启动性能分析器：

![](img/b0715b1a1ed7d836d26a22407175d2e8.png)

图 1 — 在 Power BI Desktop 中启动性能分析器（作者提供的图示）

一旦我们看到性能分析器面板，我们就可以开始记录性能数据和所有可视化的 DAX 查询：

![](img/9098aa9c3c1b08efdb8b7e0b693e05b3.png)

图 2 — 开始记录性能数据和 DAX 查询（作者提供的图示）

首先，我们必须点击“开始记录”

然后点击“刷新可视化”以重新启动当前页面上所有可视化的渲染。

我们可以点击列表中的一行，并注意到相应的可视化也被激活。

当我们展开报告中的一行时，会看到几行和一个将 DAX 查询复制到剪贴板的链接。

![](img/e83d3bd604c57192b1ddef72be669823.png)

图 3 — 选择可视化并复制查询（作者提供的图示）

如我们所见，Power BI 需要 80'606 毫秒来完成矩阵视觉效果的渲染。

单独的 DAX 查询使用了 80'194 毫秒。

这是在此视觉效果中使用的一个性能极差的度量。

现在，我们可以启动 DAX Studio。

如果我们的机器上安装了 DAX Studio，我们可以在外部工具功能区中找到它：

![](img/492fbe27ee7e4fe525d3007994fc12b3.png)

图 4 — 作为外部工具启动 DAX Studio（作者提供的图）

DAX Studio 将自动连接到 Power BI Desktop 文件。

如果我们必须手动启动 DAX Studio，我们也可以手动连接到 Power BI 文件：

![](img/8ae7b7aa83ba0d3e542eb4a971f35272.png)

图 5 — 手动将 DAX Studio 连接到 Power BI Desktop（作者提供的图）

连接建立后，DAX Studio 中将打开一个空查询。

在 DAX Studio 窗口的底部部分，你将看到一个日志部分，里面显示了发生了什么。

但在从 Power BI Desktop 粘贴 DAX 查询之前，我们必须在 DAX Studio 中启动服务器时间（DAX Studio 窗口的右上角）：

![](img/5f15a308c6bf4c9f9637f06e31e59bf9.png)

图 6 — 在 DAX Studio 中启动服务器时间（作者提供的图）

将查询粘贴到空编辑器中后，我们必须启用“清除运行时”按钮并执行查询。

![](img/ab18a9ad6d92ff139916960852054955.png)

图 7 — 启用“清除运行时”功能（作者提供的图）

“清除运行时”确保在执行查询之前清除存储引擎缓存。

在测量性能指标之前清除缓存是确保测量有一致起点的最佳实践。

执行查询后，我们将在 DAX Studio 窗口底部看到一个服务器时间页面：

![](img/ab3c2eab86457034e3c243a91ec0984f.png)

图 8 — DAX Studio 中的服务器时间窗口（作者提供的图）

现在我们看到很多信息，我们将接下来探讨。

# 解读数据

在服务器时间的左侧，我们将看到执行时间：

![](img/985c6bbfe96681403c04ded41c96c49f.png)

图 9 — 执行时间（作者提供的图）

在这里我们看到以下数字：

+   总计 — 总执行时间（以毫秒 ms 为单位）

+   SE CPU — 存储引擎（SE）执行查询所花费的 CPU 时间总和。

    通常，这个数字大于总时间，因为使用多个 CPU 核心进行并行执行

+   FE — 公式引擎（FE）花费的时间及其占总执行时间的百分比

+   SE — 存储引擎（FE）花费的时间及其占总执行时间的百分比

+   SE 查询 — DAX 查询所需的存储引擎查询数量

+   SE 缓存 — 存储引擎缓存的使用情况（如有）

一般规则是：存储引擎时间所占百分比越大，相对于公式引擎时间越好。

中间部分显示了一个存储引擎查询列表：

![](img/eae07325f5f0e203416e5534a760fd3a.png)

图 10 — 存储引擎查询列表（作者绘制）

此列表显示了为 DAX 查询执行了多少 SE 查询，并包括一些统计列：

+   行 — 索引行。通常，我们不会看到所有的行。但我们可以通过点击服务器时间面板右上角的缓存和内部按钮查看所有行。不过，这些行的实际用途不大，因为它们是可见查询的内部表示。有时查看缓存查询以及查询的哪些部分被 SE 缓存加速可能会有帮助。

+   子类 — 通常是“扫描”

+   持续时间 — 每个 SE 查询花费的时间

+   CPU — 每个 SE 查询花费的 CPU 时间

+   Par. — 每个 SE 查询的并行度

+   行数和 KB — SE 查询物化的大小

+   瀑布图 — 按 SE 查询的时间序列

+   查询 — 每个 SE 查询的开始

在这种情况下，第一个 SE 查询使用 1 GB 内存返回了 12527422 行到公式引擎（整个事实表中的行数）。这不好，因为像这样的巨大物化会严重影响性能。

这清楚地表明我们在 DAX 代码中犯了一个大错误。

最后，我们可以读取实际的 xmSQL 代码：

![](img/ca17bbc32b3f207c040a9c97de57f1a4.png)

图 11 — 存储引擎查询代码（作者绘制）

在这里我们可以看到 xmSQL 代码，并尝试理解 DAX 查询的问题。

在这种情况下，我们可以看到有一个高亮的 CallbackDataID。DAX Studio 会在查询文本中高亮显示所有 CallbackDataID，并将包含 CallbackDataID 的查询列表中的所有查询加粗。

我们可以看到，在这种情况下，一个 IF()函数被推送到公式引擎（FE），因为 SE 无法处理这个函数。但 SE 知道 FE 可以处理它。因此，它为结果中的每一行调用 FE。在这种情况下，调用了超过 1200 万次。

从时间来看，这需要很长时间。

现在我们知道我们编写了不良的 DAX 代码，并且 SE 多次调用 FE 来执行 DAX 函数。我们也知道我们使用了 1 GB 的 RAM 来执行查询。

此外，我们知道并行度仅为 1.9 倍，实际可以更好。

# 应该是什么样的

DAX 查询仅包含由 Power BI Desktop 创建的查询。

但在大多数情况下，我们需要度量值的代码。

DAX Studio 提供了一个名为“定义度量值”的功能来获取度量值的 DAX 代码：

1.  在查询中添加一到两行空白行

1.  将光标放在第一个（空的）行上

1.  在数据模型中找到度量值

1.  右键点击度量值，然后点击定义度量值

![](img/787afb605e4ed80a73d9ac73b9d0b62e.png)

图 12 — DAX Studio 中的定义度量值（作者绘制）

5. 如果我们的度量值调用了另一个度量值，我们可以点击定义依赖度量值。在这种情况下，DAX Studio 提取所选度量值所使用的所有度量值的代码。

结果是一个 DEFINE 语句，后跟一个或多个 MEASURE 语句，其中包含我们有问题的度量值的 DAX 代码。

在优化代码后，我执行了新的查询，并获取了服务器时间以与原始数据进行比较：

![](img/a55f0eb458b9c31e49cb6c6bb6eca38c.png)

图 13 — 比较慢速和快速 DAX 代码（图由作者提供）

现在，整个查询仅花费了 55 毫秒，SE 创建了仅 19 行的数据物化。

并行度为 2.6 倍，比 1.9 倍更好。看起来 SE 并不需要那么多处理能力来增加并行度。

这是一个非常好的迹象。

查看这些数字后，优化效果非常好。

![](img/ca7d0ce12c7117ccba7758416cf79298.png)

照片由 [Marc-Olivier Jodoin](https://unsplash.com/@marcojodoin?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上。

# 结论

当你的 Power BI 报告中有慢速视觉效果时，我们需要一些信息。

第一步是使用 Power BI Desktop 中的性能分析器来查看时间花费在渲染视觉效果的结果上。

当我们发现执行 DAX 查询需要较长时间时，我们需要使用 DAX Studio 来查找问题并尝试解决它。

在本文中我没有涵盖任何 DAX 优化的方法，因为这不是我的目标。

但现在我已经奠定了获取和理解 DAX Studio 中可用性能指标的基础，我可以撰写更多文章，展示如何优化 DAX 代码、应该避免什么以及为什么。

我期待与你一起踏上这段旅程。

# 参考文献

在这里免费下载 DAX Studio: [`www.sqlbi.com/tools/dax-studio/`](https://www.sqlbi.com/tools/dax-studio/)

免费 SQLBI 工具培训: [DAX 工具视频课程 — SQLBI](https://www.sqlbi.com/p/dax-tools-video-course/)

SQLBI 还提供 DAX 优化培训。

我使用 Contoso 示例数据集，就像在我之前的文章中一样。你可以从 Microsoft [这里](https://www.microsoft.com/en-us/download/details.aspx?id=18279) 免费下载 ContosoRetailDW 数据集。

Contoso 数据可以根据 MIT 许可证自由使用，如 [此处](https://github.com/microsoft/Power-BI-Embedded-Contoso-Sales-Demo) 所述。

[](https://medium.com/@salvatorecagliari/membership?source=post_page-----b7f11b9dd9f9--------------------------------) [## 通过我的推荐链接加入 Medium - Salvatore Cagliari

### 阅读 Salvatore Cagliari 的每一个故事（以及 Medium 上成千上万的其他作者的作品）。你的会员费直接…

medium.com](https://medium.com/@salvatorecagliari/membership?source=post_page-----b7f11b9dd9f9--------------------------------)
