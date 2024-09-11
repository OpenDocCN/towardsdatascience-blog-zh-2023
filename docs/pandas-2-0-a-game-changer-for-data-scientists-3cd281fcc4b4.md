# Pandas 2.0：数据科学家的游戏改变者？

> 原文：[https://towardsdatascience.com/pandas-2-0-a-game-changer-for-data-scientists-3cd281fcc4b4?source=collection_archive---------0-----------------------#2023-06-27](https://towardsdatascience.com/pandas-2-0-a-game-changer-for-data-scientists-3cd281fcc4b4?source=collection_archive---------0-----------------------#2023-06-27)

## 高效数据处理的五大特点

[](https://medium.com/@miriam.santos?source=post_page-----3cd281fcc4b4--------------------------------)[![Miriam Santos](../Images/decbc6528a641e7b02934a03e136284a.png)](https://medium.com/@miriam.santos?source=post_page-----3cd281fcc4b4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3cd281fcc4b4--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3cd281fcc4b4--------------------------------) [Miriam Santos](https://medium.com/@miriam.santos?source=post_page-----3cd281fcc4b4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F243289394aaa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-2-0-a-game-changer-for-data-scientists-3cd281fcc4b4&user=Miriam+Santos&userId=243289394aaa&source=post_page-243289394aaa----3cd281fcc4b4---------------------post_header-----------) 发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----3cd281fcc4b4--------------------------------) ·7分钟阅读·2023年6月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3cd281fcc4b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-2-0-a-game-changer-for-data-scientists-3cd281fcc4b4&user=Miriam+Santos&userId=243289394aaa&source=-----3cd281fcc4b4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3cd281fcc4b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-2-0-a-game-changer-for-data-scientists-3cd281fcc4b4&source=-----3cd281fcc4b4---------------------bookmark_footer-----------)![](../Images/7969af49055231191e3ed95318f9168d.png)

今年四月，[pandas 2.0.0 正式发布](https://github.com/pandas-dev/pandas/releases/tag/v2.0.0)，在数据科学界掀起了巨大波澜。照片由 [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

**由于其广泛的功能和多样性，** `pandas` **在每位数据科学家的心中占据了一席之地。**

从数据输入/输出到数据清洗和转换，几乎无法想象在没有`import pandas as pd`的情况下进行数据处理，*对吧*？

*现在，请耐心点：* 在过去几个月围绕 LLM 的热潮中，我在某种程度上忽略了 `pandas` 刚刚经历了重大版本更新！没错，`pandas 2.0` [已经发布并带来了巨大的变化](https://pandas.pydata.org/docs/dev/whatsnew/v2.0.0.html)！

尽管我并不了解所有的炒作， [Data-Centric AI Community](https://tiny.ydata.ai/dcai-medium) 很快就给予了援助：

![](../Images/fb154dd1a6cf10801128616a7b33ecea.png)

2.0 版本似乎在数据科学社区中产生了相当大的影响，许多用户赞扬了新版本中添加的修改。作者提供的截图。

**有趣的是：** *你知道这个版本的开发花了令人惊讶的 3 年时间吗？这就是我所说的“对社区的承诺”！*

*那么 `*pandas 2.0*` 带来了什么？让我们直接深入了解！*

# 1\. 性能、速度和内存效率

众所周知，`pandas` 是使用 `numpy` 构建的，而 `numpy` [并非故意设计为数据框库的后端](https://datapythonista.me/blog/pandas-20-and-the-arrow-revolution-part-i)。因此，`pandas` 的一个主要限制是处理大型数据集时的内存处理。

**在这个版本中，最大的变化来自于引入了** [**Apache Arrow**](https://arrow.apache.org) **后端来处理 pandas 数据。**

实质上，Arrow 是一种标准化的内存列式数据格式，提供了多种编程语言的可用库（如 C、C++、R、Python 等）。对于 Python，有 [PyArrow](https://arrow.apache.org/docs/python/)，它基于 Arrow 的 C++ 实现，因此，*速度很快*！

**简而言之，PyArrow 解决了我们在 1.X 版本中的内存限制，使我们能够进行更快、更节省内存的数据操作，特别是对于大型数据集。**

这是**读取数据的比较**，没有和使用 `pyarrow` 后端，使用 [Hacker News](https://www.kaggle.com/datasets/santiagobasulto/all-hacker-news-posts-stories-askshow-hn-polls) 数据集（约 650 MB，许可证 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)）：

read_csv() 比较：使用 pyarrow 后端快 35 倍。作者提供的代码片段。

正如你所见，使用新后端使得**读取数据的速度快了近 35 倍**。其他值得指出的方面有：

+   没有 `pyarrow` 后端时，每列/特征作为其独特的数据类型存储：**数值**特征存储为 `**int64**` 或 `**float64**`，而 **字符串** 值存储为 **对象**；

+   使用 `pyarrow` 时，所有特性都使用 Arrow 数据类型：请注意 `[pyarrow]` 注释和不同的数据类型：`int64`、`float64`、`string`、`timestamp` 和 `double`：

`df.info()`: 调查每个 DataFrame 的数据类型。作者提供的代码片段。

# 2\. Arrow 数据类型和 Numpy 索引

除了读取数据这一最简单的情况之外，你还可以**期待一系列其他操作的改进**，特别是[那些涉及字符串操作](https://medium.com/@santiagobasulto/pandas-2-0-performance-comparison-3f56b4719f58)，因为`pyarrow`对字符串数据类型的实现非常高效：

比较字符串操作：展示arrow实现的效率。代码片段由作者提供。

事实上，Arrow支持的数据类型比`numpy`更多（且支持更好），这些类型在科学（数值）范围之外是必需的：**日期和时间**、**持续时间**、**二进制**、**十进制**、**列表**和**映射**。浏览一下[pyarrow支持的数据类型与`numpy`的等效性](https://pandas.pydata.org/docs/dev/reference/arrays.html#pyarrow)可能是一个很好的练习，特别是如果你想学习如何利用它们。

**现在也可以在索引中保存更多的numpy数值类型**。传统的`int64`、`uint64`和`float64`为所有numpy数值数据类型的索引值腾出了空间，因此我们可以，例如，**指定它们的32位版本**：

利用32位的numpy索引，使代码更具内存效率。代码片段由作者提供。

这是一个受欢迎的变化，因为索引是`pandas`中最常用的功能之一，允许用户筛选、连接和打乱数据等操作。**本质上，索引越轻量，这些过程就会越高效！**

# 3\. 更容易处理缺失值

基于`numpy`构建使得`pandas`在处理缺失值时显得困难且不够灵活，因为`**numpy**`**不支持某些数据类型的空值**。

例如，**整数会自动转换为浮点数**，这并不是理想的：

缺失值：转换为浮点数。代码片段由作者提供。

注意`points`在引入单个`None`值后如何自动从`int64`变为`float64`。

**没有什么比错误的类型集合更糟糕的数据流**，*特别是在数据驱动的人工智能范式中*。

错误的类型集合直接影响数据准备决策，导致不同数据块之间的不兼容，即使在静默传递时，它们也可能影响某些操作，使其返回无意义的结果。

举个例子，在[数据驱动的AI社区](https://tiny.ydata.ai/dcai-medium)中，我们目前正在进行一个关于[数据隐私的合成数据](https://github.com/Data-Centric-AI-Community/nist-crc-2023)的项目。一个特性`NOC`（子女数量）有缺失值，因此当数据加载时，它会自动转换为`float`。然后，当将数据作为`float`传入生成模型时，我们可能会得到像2.5这样的十进制输出值——除非你是一位有2个孩子、新生儿，且有奇怪幽默感的数学家，*拥有2.5个孩子是不合适的*。

**在 pandas 2.0 中，我们可以利用** `dtype = 'numpy_nullable'`**，在不更改数据类型的情况下处理缺失值**，因此我们可以保持原始数据类型（在此情况下为`int64`）。

利用‘numpy_nullable’，pandas 2.0 可以在不更改原始数据类型的情况下处理缺失值。片段由作者提供。

**这可能看起来是一个微妙的变化**，但在底层它意味着现在`pandas`可以原生**使用 Arrow 处理缺失值的实现**。这使得操作**更高效**，因为`pandas`不再需要为每种数据类型实现处理空值的版本。

# 4\. 复制时写入优化

**Pandas 2.0 还增加了一个新的延迟复制机制，该机制在修改 DataFrames 和 Series 对象之前延迟复制**。

这意味着 [某些方法](https://pandas.pydata.org/docs/dev/user_guide/copy_on_write.html#copy-on-write-optimizations) 在**启用复制时写入**时将返回**视图而非副本**，这通过最小化不必要的数据复制提高了内存效率。

**这也意味着在使用链式赋值时需要格外小心。**

如果启用了复制时写入模式，**链式赋值将不起作用**，因为它们**指向一个临时对象**，这是索引操作的结果（在复制时写入下表现为副本）。

当`copy_on_write`**被禁用**时，像切片这样的操作 [**可能会更改原始数据框**](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#why-does-assignment-fail-when-using-chained-indexing) `df`，如果新的数据框被更改：

禁用复制时写入：在链式赋值中原始数据框会被更改。片段由作者提供。

当`copy_on_write`**被启用**时，[会在赋值时创建副本](https://stackoverflow.com/questions/23296282/what-rules-does-pandas-use-to-generate-a-view-vs-a-copy)，因此**原始数据框不会被更改**。Pandas 2.0 在这些情况下会引发 `ChainedAssignmentError` 以避免静默错误：

启用复制时写入：在链式赋值中原始数据框不会被更改。片段由作者提供。

# 5\. 可选依赖项

使用`pip`时，2.0版本给予我们**安装可选依赖项**的灵活性，这在*定制*和*优化*资源方面是一个加分项。

**我们可以根据具体要求定制安装，而无需在不需要的内容上浪费磁盘空间。**

此外，它节省了许多“依赖性头痛”，**减少了与开发环境中可能存在的其他包的兼容性问题或冲突的可能性**：

安装可选依赖项。片段由作者提供。

# 来试试吧！

然而，问题仍然存在：这些噪音是否真的有意义？我很好奇`pandas 2.0`是否在我日常使用的一些包上提供了显著改进：ydata-profiling、matplotlib、seaborn、scikit-learn。

**其中，我决定尝试一下** [**ydata-profiling**](https://github.com/ydataai/ydata-profiling) **— 它刚刚添加了对 pandas 2.0 的支持，这[似乎是社区的必备](https://github.com/ydataai/ydata-profiling/issues/1321)！在新版本中，用户可以放心，如果他们使用 pandas 2.0，管道不会中断，这是一个很大的优点！*那还会有什么其他的呢？***

说实话，ydata-profiling 一直是我最喜欢的[探索性数据分析](/a-data-scientists-essential-guide-to-exploratory-data-analysis-25637eee0cf6)工具之一，它也是一个很好的基准测试工具 —— **在我这只需一行代码，但在幕后却充满了作为数据科学家需要解决的计算** —— 描述性统计、直方图绘制、分析相关性，*等等。*

那么，有什么比用最小的努力测试`pyarrow`引擎对所有这些的影响更好的方法呢？

与 ydata-profiling 进行基准测试。作者提供的代码片段。

再次强调，使用`pyarrow`引擎读取数据无疑更好，尽管在创建数据分析报告的速度上变化不大。

然而，差异可能依赖于内存效率，对此我们需要进行不同的分析。此外，我们还可以进一步调查对数据进行的分析类型：对于某些操作，[1.5.2版本和2.0版本之间的差异似乎微不足道](https://medium.com/@santiagobasulto/pandas-2-0-performance-comparison-3f56b4719f58)。

**但我注意到的主要问题是** ydata-profiling 还没有利用`pyarrow`数据类型。这个更新[可能对速度和内存有很大影响](https://twitter.com/rasbt/status/1632090412117532672?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1632334005264580608%7Ctwgr%5E5b02b1e083a5a8172dee5c827232d185e41f54ff%7Ctwcon%5Es3_&ref_url=https%3A%2F%2Fcdn.embedly.com%2Fwidgets%2Fmedia.html%3Ftype%3Dtext2Fhtmlkey%3Da19fcc184b9711e1b4764040d3dc5c07schema%3Dtwitterurl%3Dhttps3A%2F%2Ftwitter.com%2FRitchieVink%2Fstatus%2F1632334005264580608%2Fphoto%2F1image%3Dhttps3A%2F%2Fi.embed.ly%2F1%2Fimage3Furl3Dhttps253A252F252Fabs.twimg.com252Ferrors252Flogo46x38.png26key3Da19fcc184b9711e1b4764040d3dc5c07) ，这是我在未来发展中期待的！

# 结论：性能、灵活性、互操作性！

这个新的`pandas 2.0`版本带来了许多灵活性和性能优化，**包括“引擎盖下”细微但至关重要的修改**。

也许对于数据处理领域的新手来说，这些功能并不“引人注目”，但对于那些曾经为了克服之前版本限制而不得不费尽周折的资深数据科学家们来说，这些功能无疑就像沙漠中的水源一样宝贵。

**总结一下，这些是新版本中引入的主要优势：**

+   **性能优化：** 引入了 Apache Arrow 后端、更多的 numpy dtype 索引和写时复制模式；

+   **额外的灵活性和定制化：** 允许用户控制可选依赖项，并利用 Apache Arrow 数据类型（包括从一开始就支持的可空性！）。

+   **互操作性：** 也许这是新版本一个较少被“赞誉”的优势，但影响巨大。由于 Arrow 是语言独立的，内存中的数据可以在使用 Apache Arrow 后端构建的程序之间进行传输，不仅限于 Python，还包括 R、Spark 等！

*那么，这就是了，各位！* 我希望这次总结能解答一些关于`pandas 2.0`及其在我们数据处理任务中的适用性的问题。

我仍然好奇你们是否在日常编码中发现了`pandas 2.0`带来的主要变化！如果你愿意，来[数据驱动 AI 社区](https://tiny.ydata.ai/dcai-medium)找我，告诉我你的想法！*在那里见？*

# 关于我

博士，机器学习研究员，教育者，数据倡导者，以及全面的“万事通”。在 Medium 上，我写关于**数据驱动 AI 和数据质量**的内容，教育数据科学与机器学习社区如何从不完善的数据转向智能数据。

[数据驱动 AI 社区](https://tiny.ydata.ai/dcai-medium) | [GitHub](https://github.com/Data-Centric-AI-Community) | [Google Scholar](https://scholar.google.com/citations?user=isaI6u8AAAAJ&hl=en) | [LinkedIn](https://www.linkedin.com/in/miriamseoanesantos/)
