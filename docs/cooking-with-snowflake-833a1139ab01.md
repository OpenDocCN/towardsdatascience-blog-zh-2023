# 使用 Snowflake 进行数据处理

> 原文：[https://towardsdatascience.com/cooking-with-snowflake-833a1139ab01?source=collection_archive---------6-----------------------#2023-05-17](https://towardsdatascience.com/cooking-with-snowflake-833a1139ab01?source=collection_archive---------6-----------------------#2023-05-17)

## Snowflake 优化

## 简单的配方与数据仓库中的即时满足

[](https://medium.com/@pbd_94?source=post_page-----833a1139ab01--------------------------------)[![Prabodh Agarwal](../Images/c4aa2193795fbc56edecbd78172da021.png)](https://medium.com/@pbd_94?source=post_page-----833a1139ab01--------------------------------)[](https://towardsdatascience.com/?source=post_page-----833a1139ab01--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----833a1139ab01--------------------------------) [Prabodh Agarwal](https://medium.com/@pbd_94?source=post_page-----833a1139ab01--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9856a06a88a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcooking-with-snowflake-833a1139ab01&user=Prabodh+Agarwal&userId=9856a06a88a6&source=post_page-9856a06a88a6----833a1139ab01---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----833a1139ab01--------------------------------) ·10 分钟阅读·2023年5月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F833a1139ab01&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcooking-with-snowflake-833a1139ab01&user=Prabodh+Agarwal&userId=9856a06a88a6&source=-----833a1139ab01---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F833a1139ab01&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcooking-with-snowflake-833a1139ab01&source=-----833a1139ab01---------------------bookmark_footer-----------)![](../Images/73dfb2244fbf2936dcb1412336728421.png)

使用 Midjourney 生成（付费订阅）

Snowflake 社区充斥着有关如何优化昂贵查询的信息倾销。我们知道这一点，因为我们翻阅了大量这样的信息。我们在这里展示的是我们在[Toplyne](https://www.toplyne.io/)中采用的三种战术方法。

Toplyne 的业务涉及从实时数据中提取实时洞察。这些数据目前来源于我们客户的产品分析、CRM 和支付系统。

CRM 和支付数据的体量大多是可控的。一个产品会有有限的付费客户，以及在 CRM 中跟踪的略多一些的客户。然而，产品分析数据的体量要大得多。

Toplyne 的POC（概念验证）和MVP（最简可行产品）是基于产品分析数据构建的。我们从一开始就知道需要使用数据仓库解决方案来处理这些数据。该解决方案必须满足两个明确的要求：

1.  它应该能轻松处理几百GB的数据。

1.  它应该提供一个简单而简明的 API 来与这些数据交互。

我们比较了三种解决方案：BigQuery、Redshift 和 Snowflake。

探索后，Snowflake 是明显的选择。简单的原因是它的 SQL 基础接口。SQL 意味着我们的工程操作没有冷启动延迟。Toplyne 的工程师没有来自 DWH 背景的经验，但我们很快上手了。

与客户产品分析数据交互的过程简单如下：

1.  产品分析数据通过连接器进入 Snowflake。（有很多现成的和本地的连接器可以使用）。

1.  登录 Snowflake 并使用内置的工作表编写 SQL。🎉

这个简单的 2 步过程意味着我们可以迅速掌握客户与我们分享的数据。

在短时间内，我们开发了两个算法，将我们接收到的数据转换为数据科学家可以训练的模式。第一个算法负责转换产品分析事件数据。第二个算法负责识别用户的个人资料数据。然后在这些数据上编写附加的特征工程算法。

SQL 是一种第四代语言（4GL），相对较容易学习。结合一个只需浏览器标签的基于工作表的界面——Snowflake；一个初创公司可以以最小的设置工作量完成大量的数据处理。

我们在工作表中编写了一些 SQL 语句来转换数据，然后我们的数据科学家可以直接 `SELECT *` 数据并编写他们的 ML 训练程序。

随着时间的推移，上述整个过程显著扩展。扩展发生在以下几个方面：

1.  我们有多个客户，每个客户的产品分析数据分布在多个平台上，如 Amplitude、Mixpanel、Segment、Clevertap 等。

1.  我们的团队编写了多个算法，以不同的维度处理数据。

1.  我们现在集成了 CRM 和支付数据。此外，这些数据集有自己的一套 ETL 算法。

1.  我们使用 Airflow 编排庞大的管道，这些管道有多个阶段。

我们 ETL 流程的样本架构图。Snowflake 是该系统的核心。

![](../Images/260a503ca0b67035740ce4d76415aaa0.png)

Toplyne 的数据管道架构

1.  将源数据同步到 Snowflake。

1.  使用 Apache Airflow 进行 ETL 编排。

1.  将转换后的数据存入 Snowflake。

1.  DS/ML/分析师/产品从 Snowflake 获取数据用于他们的工作流程。

几个月来，系统的不同组件经历了多次更改和重大重写，而 Snowflake 是唯一不变的。

由于我们已经运行并维护了一个系统，我们想提出一些关于Snowflake中查询优化的想法。我们有一种超级简单的技术，通过对现有查询进行轻微调整，能够从系统中提取出大量的性能。

# **查询优化**

我们运行一个多租户系统，其中单个Snowflake实例负责处理大量客户数据的ETL。ETL由Airflow编排。

我们为每个客户创建一个仓库，并在该仓库上运行所有ETL和特征工程。在整个ETL运行期间，对一个仓库会依次和/或并行地执行数百个SQL查询。一次运行可能持续一个小时，并且一天中可能有多次运行。

本质上，一个仓库的大小运行所有昂贵的以及便宜的查询。因此我们的目标是将仓库大小保持在最低限度。我们通过为不同的ETL运行定义服务水平协议（SLA）来定义最小值。然后，我们调整仓库大小，以便可以在该大小下满足ETL SLA。像任何工程组织一样，我们希望在满足SLA的情况下将仓库大小保持在最低限度。

我们有仪表板来监控最昂贵查询的查询模式。这些仪表板有不同的粒度级别。我们不断监控这些仪表板，并不断调整查询。随着时间的推移，我们识别出了一些昂贵查询中的模式，并制定了一本关于如何最小化这些查询运行时间的操作手册。我们将展示三个案例研究，概述查询的问题陈述、最初编写方式、该查询中的瓶颈以及其最终的最佳解决方案。

# 窗口查询

## 情景

我们从产品分析数据中跟踪用户的个人资料信息。产品分析系统保存有关用户的多个数据点，例如位置、设备、订阅状态等。一些数据点经常变化，而另一些则不那么频繁。鉴于这些数据的性质，这些信息在数据库中以追加日志的形式表示。

我们的一个特征工程需求是捕获用户最新的个人资料信息，作为ETL运行的一部分。

![](../Images/b758fff47289b6182bb792dbf288cb70.png)

上图给出了ETL的流程图。

**1** 是来自产品分析的原始数据，**2** 是我们想要应用的算法，**3** 是ETL的最终结果。

我们拥有的SQL查询是这样的：

```py
select
    *
from
    tbl
qualify
    row_number() over (partition by id order by timestamp desc) = 1
```

## 瓶颈

这个查询很简单，而且在Snowflake中效果很好。然而，这个查询中的窗口函数是一个瓶颈。

查询的工作原理如下：

1.  创建与`user_ids`数量相同的逻辑桶

1.  在每个桶中按降序`排序`数据

1.  将`row_numbers`分配给已排列的数据

1.  `标记`桶中的第一个条目

1.  *丢弃*剩余的数据。

根据上述解释，我们可以看到，随着表中数据的增加，桶的数量和桶的大小都会增加。由于我们处理的是仅追加数据集，我们应该为这种情况做好准备。在 Snowflake 中，您会在查询分析器中注意到大小增加的趋势，表现为**字节溢出**。

此外，我们需要理解，根据业务需求，桶的数量预计会增加，但作为工程师，我们仍然可以将**单个桶的大小**保持到最小。

## 最优解决方案

我们将采用一种技术，通过使用**CTE**和**聚合函数**来将桶的数量保持到最小。

```py
with
prune_condition as (
    select id, max(timestamp) as prune_column from tbl group by id
),
pruned_data as (
    select * from tbl left join prune_condition using (id)
)
select
    * exclude prune_column
from
    pruned_data
where
    timestamp >= prune_column
qualify
    row_number() over (partition by id order by timestamp desc) = 1
```

我们将 `descending sort expression` 的窗口查询子句转换为 `max() function`，然后将其与源数据连接以获得过滤器。通过使用此过滤器，我们确保数据不会被 `qualify` 子句丢弃，从而一开始就不会被桶化。这大大减少了窗口查询所执行的工作。此外，使用聚合函数的额外成本被减少的成本大大抵消，因此整体查询变得高效。

# Pivot 查询

## 场景

我们使用事件数据中的一个功能，该功能要求获取每个用户每个事件的计数。

为了获得这些数据，我们执行一个分组查询，然后将这些数据转置以组织成列，如下图所示。

**1** 是原始数据，**2** 是转换后的输出。

![](../Images/23964d6ab2492da9ff62c5fd92a86fd3.png)

我们的 SQL 查询如下：

```py
select * from (
    select id, id as users, data from tbl
) pivot(
    count(users)
    for data in (‘d1’, ‘d2’, ‘d3’)
)
```

## 瓶颈

尽管示例显示了沿着 3 个元素的 pivot，但我们的生产用例通常在大约 1000 个事件的情况下处理大约一百万用户。

此查询中的 pivot 函数是查询中最慢的步骤。因此，我们希望用手动 pivot 查询替换此逻辑。我们通过结合使用 `Group By` 子句和 `Filter` 子句来生成此查询。

## 最优解决方案 1

```py
select 
    id,
    sum(iff(data = ‘d1’, 1, 0)) as “‘d1’”,
    sum(iff(data = ‘d2’, 1, 0)) as “‘d2’”,
    sum(iff(data = ‘d3’, 1, 0)) as “‘d3’”
from
    tbl
group by id
```

这个查询显著提高了性能。

然后我们**减少**了仓库的大小，以查看查询是否保持同样的性能。我们观察到查询显著变慢，并且**字节溢出**也很严重。然而，字节溢出的一个优点是，我们在减少的仓库大小中有更多的改进空间。

## 最优解决方案 2

我们根据**Map-Reduce**框架重写了此查询，并观察到运行时间显著改善。

目标是对较小的事件集执行上述操作，然后一次性将所有数据合并在一起，如下所示：

```py
create temporary batch_1 as (
    select
        id,
        sum(iff(data = ‘d1’, 1, 0)) as “‘d1’”
    from
        tbl
    group by id
);

create temporary batch_2 as (
    select
        id,
        sum(iff(data = 'd2', 1, 0)) as "'d2'",
        sum(iff(data = 'd3', 1, 0)) as "'d3'"
    from
        tbl
    group by id
);

create final_table as (
    select * from batch_1 full outer join batch_2 using(id)
);
```

我们的生产系统将**1000个事件**拆分成**10个块**，每块包含**100个事件**。此查询显著加速，因为它将字节溢出减少到接近 0。

此外，一旦我们用最优解决方案 1 替换 Pivot 函数，这个优化是非常直观的。

*向下滚动以查找本文的配套代码。*

# 聚合查询

SQL规范定义了许多汇总函数，Snowflake在这方面表现出色。Snowflake中也有一个庞大的汇总函数库。

不同的汇总函数有不同的运行时间，在我们看来，每个汇总函数都应根据其优点进行处理。优化汇总函数的策略是首先识别出成为瓶颈的汇总函数，然后激励自己相信**可能**存在算法解决方案。

我们想与您分享一个案例研究，在这个案例中，我们识别了一个查询，其中选择了一个次优的汇总函数。我们重新设计了该解决方案的算法，使用了一个更简单的汇总函数，从而获得了相同结果的更优性能。

## 情景

我们有一个事件时间序列，这些事件在产品分析系统中触发。我们需要从这个数据集中回答两个问题，以进行特征工程。

Q1) 识别所有在一秒内大多被多次触发的数据点

Q2) 识别大多在至少一小时间隔内触发的数据点

![](../Images/94bc234e71ecdc85f1c613ee208e0eef.png)

为了回答这些问题，我们使用Snowflake的**window query**和**lag function**将`tbl_1`中的输入数据转换为`tbl_2`。

然后我们使用**median function**编写了解决方案查询。

```py
-- Q1)

select
    data
from
    tbl_2
group by data
having median(sec_diff) = 0;

-- Q2)

select
    data
from
    tbl_2
where
    data <> lag_data
group by data
having median(hour_diff) > 0;
```

## 瓶颈

`median`函数非常慢。

我们让ChatGPT建议一个最佳解决方案。它确实提出了使用**Percentile**函数的解决方案，但这同样很慢，并且似乎与**Median**函数本身类似。

然而，ChatGPT很好地解释了为什么提出了那个解决方案。然后我们通过迭代和改进ChatGPT的解决方案，提出了一个新的解决方案。

## 最优解决方案

我们确定，对于我们的需求，我们只需使用计数查询。对于**Q1)**和**Q2)**，我们希望大多数事件的**sec_diff**和**hour_diff**分别大于或等于0。

```py
-- Q1) after optimisation

select
    data,
    count(iff(sec_diff = 0, 1, null)) / count(*) as seccount
from
    tbl_2
group by data
having seccount >= 0.5

-- Q2) after optimisation

select
    data,
    count(iff(hour_diff > 0, 1, null)) / count(*) as hourcount
from
    tbl_2
group by data
having hourcount >= 0.5
```

# 经验教训

我们持续观察我们的系统，然后确定哪些优化需要紧急分析，哪些可以积压。

Snowflake提供了多个配置参数，可以进行调优以获得性能。Snowflake社区定期发布技巧和技术。

在这些信息过载中，我们需要专注并建立一个适合我们的剧本和技术库，这些技术可以毫不费力地应用。

这些是我们用于目的的参数：

1.  检查查询分析器中的每个节点

1.  对节点进行输入与输出比率分析

    i) 尝试降低这个比率

    ii) 给定问题描述时，输出将保持不变

    iii) 因此，尽量减少对汇总节点的输入

1.  另一种方法是持续测量磁盘溢出。尽可能减少溢出

    i) 较大的仓库有较低的溢出，但成本也较高

    ii) 只有在减少同一仓库中的溢出时，你才能获得优化

***将本文作为代码复现：*** 你可以参考[这个 GitHub 链接](https://github.com/prabodh1194/sf-query-benchmarks)来获取与这些查询的基准测试相关的代码。

# 结论

这篇文章提供了三种高度可操作的优化技术，你可以立即应用。你可能在代码库中有类似于我们这里展示的情况的次优模式。随时使用这些代码示例来获得即时解决方案。在其他情况下，请动脑筋，我将很高兴了解你的解决方案。

[关注我](https://medium.com/@pbd_94)以获取更多关于 Snowflake、数据和 MLOps 的文章。

附言：除非另有说明，否则所有图片均来自作者。
