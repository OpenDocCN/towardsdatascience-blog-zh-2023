# 优化数据仓库存储：视图与表

> 原文：[`towardsdatascience.com/optimize-data-warehouse-storage-with-views-and-tables-659dd588345d`](https://towardsdatascience.com/optimize-data-warehouse-storage-with-views-and-tables-659dd588345d)

## 表与视图的区别及其使用方法

[](https://madison-schott.medium.com/?source=post_page-----659dd588345d--------------------------------)![Madison Schott](https://madison-schott.medium.com/?source=post_page-----659dd588345d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----659dd588345d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----659dd588345d--------------------------------) [Madison Schott](https://madison-schott.medium.com/?source=post_page-----659dd588345d--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----659dd588345d--------------------------------) ·阅读时间 6 分钟·2023 年 3 月 24 日

--

![](img/836edeb9f20b6e4112718edec85e7a93.png)

照片由 [Sophia Baboolal](https://unsplash.com/@sophiababoolal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/table?orientation=landscape&utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随着现代数据技术堆栈的兴起，许多公司正在将其数据库从本地迁移到云端。他们开始利用像 Snowflake、Redshift 和 DuckDB 这样的数据仓库工具，以充分发挥云的所有优势。

尽管这些数据仓库通常帮助较小的公司节省开支，但云上的计算成本很容易累积。因此，优化存储和计算成本至关重要。这意味着你需要了解最佳的数据存储方式，以便数据团队能够以具有成本效益的方式使用这些数据。

在本文中，我们将讨论视图与表的区别、数据仓库中存在的不同类型视图以及每种视图的使用场景。阅读完本文后，你应该能够确定存储不同数据集的最佳选项，同时节省成本。

# 什么是视图？

视图是**一个定义的查询，位于表的顶部**。与表不同，它不存储实际数据。它始终包含最新的数据，因为每次查询时都会重新运行。而表的内容仅与上次创建或更新时一样，无论你什么时候查询。

视图主要有两种类型：**非物化视图**和**物化视图**。

# 非物化视图

非物化视图是人们通常想到的视图。这种类型的视图仅在实际查询时运行，否则不会存储在数据库中。

非物化视图很棒，因为它们不占用存储空间，这意味着你不必担心为大量存储付费。它们也只有在需要时才运行，从而节省计算资源费用。这意味着，如果源表几个月或几周都不需要，你无需为其维护付费。只有当分析师或分析工程师重新开始使用该表时，你才需为其付费。

最棒的部分？非物化视图仍然具有与表相同的所有功能！如果需要，你可以在它们上面执行连接、聚合和窗口函数。

不幸的是，就像其他所有事物一样，所有优点背后总会有缺点。非物化视图不适合处理大量数据和复杂逻辑，因为每次查询视图时，这些逻辑都会被执行。

例如，我通常将所有的源数据表创建为非物化视图，这些视图引用我的原始数据。这些是简单的 SELECT 语句，包含基本函数，如列重命名、数据类型转换和数据清理。由于它们的底层逻辑简单，所以每当我查询这些源表时，它们运行得很快。

如果我创建包含连接和窗口函数的复杂数据模型作为视图，可能在查询时这些视图永远不会加载。或者它们需要极其长的时间！显然，这不是理想的情况。你将会花费更多的计算能力来运行视图查询，而不是将视图创建为表。

**记住：非物化视图非常适合使用，但仅当创建它们的逻辑是简单的 SELECT 语句时。**

# 物化视图

物化视图在我们讨论的两种视图中较为少见。物化视图的行为更像表。它们查询速度更快，被认为比非物化视图更易于访问。并且，就像表一样，它们在数据仓库中占用更多的存储空间，需要更多的计算资源。这意味着它们是两种视图中更昂贵的选项。

你不会经常需要使用它们。事实上，我从未遇到过一个合理的使用场景。根据[Snowflake 的文档](https://docs.snowflake.com/en/user-guide/views-materialized)，你应该仅在以下所有条件都满足时使用物化视图：

+   视图的结果被频繁使用

+   驱动视图的查询使用了大量资源

+   视图变化频繁

这三者同时适用于你的基础/暂存、中间和核心 dbt 模型的情况非常少见。基础/暂存模型不会消耗大量资源，而中间和核心数据模型变化不频繁。当然，总会有例外情况，但我还没遇到过这种情况。

# 这些视图在数据建模中的使用方式

如果你是分析工程师，你可能会想知道未物化视图和物化视图在数据建模中如何使用。让我们深入了解一下 dbt 基础（或阶段）模型以及核心模型。

## ‍dbt 基础模型

dbt 基础模型作为视图存在于你的原始数据之上。它们被创建为未物化视图，以保持原始数据的完整性，同时利用适当的命名约定和公司标准。这些模型中的代码是从原始数据中直接读取的基本 SQL 选择语句，通过像 Airbyte 这样的摄取工具进行 ELT。一个典型的基础模型如下：

```py
select
  ad_id AS facebook_ad_id,
  account_id,
  ad_name AS ad_name_1,
  adset_name,
  month(date) AS month_created_at,
  date::timestamp_ntz AS created_at,
  spend
from {{ source('facebook', 'basic_ad')}}
```

‍如果你查看 dbt 中此文件的底层逻辑，它实际上会在 Snowflake（我首选的数据仓库）中编译为如下所示：

```py
create or replace view data_mart_dev.base.base_facebook_ads 
  as (

    select
      ad_id AS facebook_ad_id,
      account_id,
      ad_name AS ad_name_1,
      adset_name,
      month(date) AS month_created_at,
      date::timestamp_ntz AS created_at,
      spend
    from raw.facebook.basic_ad

    );
```

由于你仅使用基本的日期函数和重命名列，视图在按需查询时仍然很快。这反过来节省了你原本会用来保存几乎相同的原始数据副本的存储空间。

## dbt 核心模型

在 dbt 中，你的核心模型比基础模型更复杂，通常包含多个 CTE、联接和窗口函数。虽然你可能有特定的用例来将这些创建为物化视图，但你很可能会将这些创建为数据仓库中的表。表对于处理复杂的转换非常理想，如果将其存储为视图，会需要较长的运行时间。

这是我一个核心数据模型的代码示例：

```py
with

  fb_spend_unioned AS (

    select created_at, spend, 'company_1' AS source from {{   ref('base_fb_ads_company1')}}
    UNION ALL
    select created_at, spend, 'company_2' AS source from {{ ref('base_fb_ads_company2')}}

  ),

  fb_spend_summed AS (

    select
      month(created_at) AS spend_month,
      year(created_at) AS spend_year,
      created_at AS spend_date,
      sum(spend) AS spend
    from fb_spend_unioned 
    where spend != 0
    group by
      created_at,
      month(created_at),
      year(created_at)

  )

  select * from fb_spend_summed
```

当在 Snowflake 中编译为 SQL 时，代码将如下所示：

```py
create or replace table data_mart_dev.core.fb_spend_summed

  as (

    with 

    fb_spend_unioned AS (

    select created_at, spend, 'company_1' AS source from {{   ref('base_fb_ads_company1')}}
    UNION ALL
    select created_at, spend, 'company_2' AS source from {{ ref('base_fb_ads_company2')}}

),

  fb_spend_summed AS (

    select
      month(created_at) AS spend_month,
      year(created_at) AS spend_year,
      created_at AS spend_date,
      sum(spend) AS spend
    from fb_spend_unioned 
    where spend != 0
    group by
      created_at,
      month(created_at),
      year(created_at)

  )

  select * from fb_spend_summed

  );
```

请注意，这里创建的是一个 Snowflake 中的表而不是视图。这对于任何将直接用于 BI 工具的数据是理想的，大多数核心数据模型都是这样。它们可以按需轻松查询，无需运行底层逻辑。这确保了快速的仪表板，能够让利益相关者信赖。

# 结论

视图和表在数据仓库中存在不同的原因。视图不存储实际数据，可以作为一个工具来节省开支，通过简单查询在其他表之上运行。表应当用于存储由更复杂逻辑生成的数据，确保性能和可用性始终较高。

正确使用时，非物化视图是在 Snowflake 中节省开支的好工具，同时不牺牲性能。我强烈推荐在 dbt 中将其用于你的基础模型，以创建符合你设定的公司标准的高质量数据。而且，不要忘记在核心 dbt 模型中使用表。性能提升是值得更高成本的！

欲了解更多[分析工程](https://madisonmae.substack.com/)的信息，订阅我的免费每周通讯，我会分享学习资源、教程、最佳实践等。

查看我的第一本电子书，[分析工程的基础知识](https://madisonmae.gumroad.com/l/learnanalyticsengineering)，这是一本关于如何开始从事分析工程角色的全方位指南。
