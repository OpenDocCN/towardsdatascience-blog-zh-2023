# 利用维度建模构建更好的数据仓库：数据工程师指南

> 原文：[`towardsdatascience.com/building-better-data-warehouses-with-dimensional-modeling-a-guide-for-data-engineers-422b3cd52df4`](https://towardsdatascience.com/building-better-data-warehouses-with-dimensional-modeling-a-guide-for-data-engineers-422b3cd52df4)

## 数据仓库维度建模设计 101

[](https://chengzhizhao.medium.com/?source=post_page-----422b3cd52df4--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----422b3cd52df4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----422b3cd52df4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----422b3cd52df4--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----422b3cd52df4--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----422b3cd52df4--------------------------------) ·阅读时间 9 分钟·2023 年 5 月 8 日

--

![](img/2aaba6fe12b34ed39479fd86886ac73a.png)

图片由[Erin Doering](https://unsplash.com/@edoering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/wSWWyNJSL40?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

关于数据密集型应用的系统设计，通常有两个选项：优化写入或优化读取。

没有一种数据库设计既适合于写入也优化读取。就像所有系统设计视角一样，没有绝对对错的解决方案，只有优缺点。作为从事数据模型设计的数据专业人士，角色中的一个关键部分是识别用例，并进一步确定应应用哪种设计原则。

数据仓库历史上一直作为向最终用户提供数据的层，它是将数据转化为洞察的最后一公里。Ralph Kimball 开发了一种著名的建模设计技术，称为[维度建模](https://en.wikipedia.org/wiki/Dimensional_modeling)。他的[《数据仓库工具包：维度建模的权威指南，第 3 版》](https://www.amazon.com/Data-Warehouse-Toolkit-Definitive-Dimensional/dp/1118530802/ref=asc_df_1118530802/?tag=hyprod-20&linkCode=df0&hvadid=312128454859&hvpos=&hvnetw=g&hvrand=12986151952939713448&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9032171&hvtargid=pla-396828635481&psc=1)是维度建模的最关键书籍。

尽管大数据和云计算技术解放了我们，使用更多的计算能力和更便宜的存储，但新手或经验丰富的数据工程师却忽视了数据仓库建模设计。越来越少的人关注慢变化维度（SCD）、代理键、表粒度等概念。

数据模型设计对于建立任何数据仓库系统的基础至关重要。我希望引起社区对以下内容的关注——《通过维度建模构建更好的数据仓库：数据工程师指南》。

# 为什么你应该关心 OLTP 与 OLAP

如果你对数据库有所了解，两个缩写词应该不会陌生：OLTP 和 OLAP。

OLTP 代表在线事务处理，以规范化的设计理念而闻名。在早期，存储是昂贵的。**规范化的核心思想是减少重复并提高写入效率。**

例如，我们可以有一个二维电子表格表。我们有产品及其子类型。爱丽丝和约翰都以 $1.99 购买了 Honeycrisp 苹果。对于不同的客户，相同属性的相同苹果重复出现——我们有重复。

当我们需要执行更新/插入/删除操作时，情况会变得更糟。例如，我们需要添加苹果来源于哪个农场，因此我们必须返回到这个表中并重复录入数千次。

![](img/d28fefbde9fba84bc8b82e9b11d84fac.png)

OLTP 规范化以去除重复 | 图片来源：作者

为了解决这种重复并提高效率，我们可以通过规范化来实现——创建一个单独的产品表，并将所有相关字段移动到该表中。在原始表中，仅保留一个标识符以引用新创建的表中的行。

这节省了存储空间，并减少了在需要执行更新/插入/删除操作时接触的行数。

那么 OLTP 方面的难点是什么？*它不适用于分析模式查询。*

**分析模式查询专注于使用选择/过滤/聚合在读取数据时的查询效率。** 对这种查询模式缺乏支持导致了 OLAP。

OLAP 代表在线分析处理，以非规范化的设计理念而闻名。**非规范化的核心思想是减少复杂的业务逻辑和大型表连接（后面我们称之为事实到事实表）在运行时的需求。**

数据的准备和编写通常发生在 ETL 过程的前期，这通常需要更长的时间来执行和将业务逻辑预计算到最终表中。

回到我们的苹果数据示例，我们之前共享的电子表格是分析设计的一个优秀例子。我们避免了电子表格之间的联接，并且可以直接进行选择/过滤。

大多数时候，数据工程的目标是为终端用户提供 OLAP 数据仓库。与尽量避免重复的 OLTP 不同，OLAP 的设计原则是——**冗余（反规范化）并不坏。我们牺牲了存储成本，但在运行时获得了更快的查询速度。**

# 成功维度模型设计的 4 个步骤

[数据仓库工具包：维度建模的权威指南，第 3 版](https://www.amazon.com/Data-Warehouse-Toolkit-Definitive-Dimensional/dp/1118530802/ref=asc_df_1118530802/?tag=hyprod-20&linkCode=df0&hvadid=312128454859&hvpos=&hvnetw=g&hvrand=12986151952939713448&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9032171&hvtargid=pla-396828635481&psc=1) 是维度建模最关键的书籍。该书涵盖了成功构建数据仓库的四个关键步骤：

## 1\. 选择业务流程

在我的数据工程职业生涯中，我学到的最重要的事情就是找到并坚持业务用例。你可以拥有所有高端工具、毫秒级延迟的实时处理和强大的分布式计算查询引擎。但没有用例就像空中楼阁。

无论你的设计多么周到，如果你也是终端用户（通常不太可能对于数据工程来说），咨询终端用户始终是明智的第一步。我们可以采访终端用户，了解他们如何利用领域知识解析和理解数据，他们当前的报告是如何生成的，以及了解他们在过程中遇到的痛点。

Kimball 的[企业数据仓库总线架构](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/kimball-data-warehouse-bus-architecture/)也是一种优秀的技术，用于桥接业务使用流程和标准维度。目标是评估在事实表中共享的大小。利用总线矩阵技术可以简化后续阶段的维度设计决策，并帮助你可视化整体结构。

![](img/3493a709ca2cfa113945b2b2b59b6827.png)

数据仓库总线矩阵 | 图片由作者提供

## 2\. 声明粒度

粒度是什么？它指的是表中保留的信息的最低级别。

我们如何知道需要什么粒度？我们需要与用户咨询以做出这样的决定。通常，提供的信息越详细，表格中的灵活性就越高。仅有聚合数据是不可能获得详细的行级数据的。例如，我们可以从每日聚合中获得每周销售数据，但很难将每周销售数据轻松拆解为每日数据。

聚合视图在查询时更快，但需要更多细节。另一方面，更低的粒度可以在后期聚合以提供相同的见解，但在运行时可能需要额外的时间。与终端用户协商并帮助他们理解利弊和潜在的未来需求是至关重要的。

我建议将粒度设定得尽可能低，因为更详细的数据可以在后续聚合，但反之则不可。如果需要更高粒度的事实表，可以使用现有的事实表来构建。

## 3\. 确定维度

一旦确定了粒度，接下来应该确定维度。维度是事实表的基石。

维度用于使事实表对业务更具意义。通常，维度是名词，如日期、类别、产品等。它们的作用是切片和切块。最终用户可以使用维度来关注特定数据区域或汇总多个字段以观察潜在模式。例如，在我们之前的例子中，前 3 种苹果是什么？

维度表通常包含的数据比事实表少得多。如果你熟悉分布式计算或曾在 Spark 物理计划中工作过，那么维度表通常适合广播连接，这意味着这个数据集足够小，可以发送到每个节点以提高连接效率。

缓慢变化维度（SCD）是核心维度设计概念。虽然有超过六种 SCD 类型，但**最关键的是 SCD 类型 2**，因为它在行业中的广泛应用。

如果维度值中的一行已被修改，如何在你的维度表设计中跟踪这种变化？

这就是 SCD 类型 2 的魅力。我们会添加另一行，使用相同的业务键。新添加的行包括未更改的字段和修改过的字段。为了知道哪一行是当前的，哪一行是过去的，我们会为特定行的有效期添加开始和结束日期时间。例如，如果供应商从 CA 迁移到 IL，我们仍然可以在缓慢变化维度中追踪。

![](img/6f678099620f9b322f1384523a244cb6.png)

缓慢变化维度（SCD）类型 2 | 作者图片

Suppier_Code — ABC 通常是从 OLTP 系统提供的业务键。为了保持一个主键，以便以后与事实表连接，我们需要创建一个[代理键](https://en.wikipedia.org/wiki/Surrogate_key) — Suppkier_Key——在这个维度中是唯一的。

## 4\. 确定事实

事实表是维度建模的“灵魂”。事实表保留了核心业务数据。大多数事实表的行仅为数字，例如总销售量、交易费用和利润。

事实表有多种类型。你可以构建基于事务的事实表或快照事实表，如每月销售数据。

事实表的核心思想是跟踪给定时期的数据变化。例如，假设 Alice 购买了 Honeycrisp 苹果。然后第二天，她因为发现苹果里有虫子而退货。

+   如果我们采用事务基础视图，我们将记录两行。第一行是$2.99 的利润，第二行是-$2.99（假设没有额外操作成本），因为她退货了。

+   如果我们对每月的基本视图进行快照，Alice 的购买不会影响业务，因为 Alice 的净利润为 0。

事实表是金字塔的顶端。它需要所有基础工作准备好。因此，在开始设计事实表之前，拥有一个坚实的基础是至关重要的。

# 我应该选择星型模式还是雪花模式？

[星型模式](https://en.wikipedia.org/wiki/Star_schema)和[雪花模式](https://en.wikipedia.org/wiki/Snowflake_schema)的选择对于熟悉 OLTP 设计并试图进行一定程度规范化的数据仓库来说可能更直接。

在大多数情况下，星型模式应该是首选。原因有几个：

+   我们希望避免额外的联接，这会降低查询性能。

+   维度之间的关系越多，管理和维护数据完整性就越复杂。

+   最终用户通常对数据模式不够熟悉，数据仓库的设计应该简单且用户友好。

+   存储额外数据的额外成本是微乎其微的。

我们考虑雪花模式的一个例外情况是为了节省成本，或者如果维度本身频繁更新，需要添加规范化以减少需要处理的字段数量。然而，首要目标是减少联接，以减少最终用户的查询时间。

# 为什么不将所有内容保存在单个表中？

另一种选择是将所有字段存储在一个表中，而不是分开存储事实表和维度表。

我们可以选择类似电子表格的单表，并且通过像[Parquet](https://parquet.apache.org/)这样的列式存储，查询可能变得更快。

需要考虑的关键标准是——维度表也会在多个事实表中共享。这样可以更容易地管理，因为维度在多个地方被引用。

关于联接呢？维度表和事实表的联接是否缓慢？我们之前提到过“维度通常比事实表包含的数据少得多。”对于分布式计算，最有效的方法是将较小的数据集发送到每个节点，以避免大量数据的传输。在这种情况下，事实表的联接通常是最小的，由工程师评估查询性能和数据管理工作之间的权衡。

# 最后的思考

维度建模及其原则在行业中已经存在超过 40 年。它已被证明是一种典型的 OLAP 用例设计模式。尽管大数据时代带来了比以往更强大的分布式计算引擎，但 OLAP 的基本设计原则不应被忽视。正确设计数据项目始终至关重要，而不是盲目投入资源。

我希望这篇文章能够引起更多数据工程师的关注，促使他们在追逐新工具之前重新考虑数据仓库设计。
