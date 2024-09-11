# dbt 增量模型在大数据中的威力

> 原文：[https://towardsdatascience.com/the-power-of-dbt-incremental-models-for-big-data-c8ba821eb078?source=collection_archive---------5-----------------------#2023-02-09](https://towardsdatascience.com/the-power-of-dbt-incremental-models-for-big-data-c8ba821eb078?source=collection_archive---------5-----------------------#2023-02-09)

## 在 BigQuery 上的实验

[](https://medium.com/@suela.isaj?source=post_page-----c8ba821eb078--------------------------------)[![Suela Isaj](../Images/2749bad708a0446216a7e0bb6656026c.png)](https://medium.com/@suela.isaj?source=post_page-----c8ba821eb078--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c8ba821eb078--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c8ba821eb078--------------------------------) [Suela Isaj](https://medium.com/@suela.isaj?source=post_page-----c8ba821eb078--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6aa4db597456&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-dbt-incremental-models-for-big-data-c8ba821eb078&user=Suela+Isaj&userId=6aa4db597456&source=post_page-6aa4db597456----c8ba821eb078---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c8ba821eb078--------------------------------) ·5分钟阅读·2023年2月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc8ba821eb078&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-dbt-incremental-models-for-big-data-c8ba821eb078&user=Suela+Isaj&userId=6aa4db597456&source=-----c8ba821eb078---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc8ba821eb078&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-power-of-dbt-incremental-models-for-big-data-c8ba821eb078&source=-----c8ba821eb078---------------------bookmark_footer-----------)

如果你在使用 dbt 模型处理几MB或GB的数据，这篇文章不适合你；你已经做得很好了！这篇文章是为那些需要在 BigQuery 中扫描 TB 级数据，以便计算某些计数、总和或滚动总计的可怜的灵魂准备的，这些计算可能是每天进行的，甚至更频繁。在这篇文章中，我将介绍一种针对“大数据”进行便宜的数据摄取和数据消费的技术。

![](../Images/a7ab7fb73c98a2f43f123fd1f58a350e.png)

图片由 [Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

让我们假设我们有以时间戳为粒度的原始数据，并且我们需要计算每个客户的总数。此外，我们希望进行按天和按月的分析，因为我们需要在不同粒度上汇总数据。我将以 Twitter 点赞为例，其中 `user_id` 是 Twitter 用户，`post_id` 是 Twitter 帖子，`timestamp` 是 `user_id` 点赞 `post_id` 的时间戳。原始数据以天为粒度存储为分区表。如果你的原始数据没有分区，这已经是你的第一个改进，因此考虑将原始数据迁移到分区表，参考 [这个](https://www.revisitclass.com/gcp/how-to-add-partition-to-existing-table-in-bigquery/) 示例。确保按需设置分区过滤器 [https://cloud.google.com/bigquery/docs/managing-partitioned-tables#require-filter](https://cloud.google.com/bigquery/docs/managing-partitioned-tables#require-filter)，以避免扫描所有原始数据的意外情况。

![](../Images/b10e53c96af999ec201f8a59190e72a8.png)

Twitter 点赞的原始日志

这些数据实际上大约是 869.91 GB！所以如果我想知道每个用户所执行的点赞数量，我的[昂贵的]简单查询将是：

```py
SELECT 
  user_id,
  COUNT(*) as nr_likes
FROM twitter_likes
```

然而，Twitter 上的点赞是*不可变*的事件，无法被修改。这允许我逐步加载这些数据。我将遍历原始数据来加载当天的数据，检查每日汇总的数据以加载本月的数据，最后通过每日数据来更新发生变化的用户的点赞数。

对于这两种增量模型，我将使用静态分区技术，这被证明是性能最优的 [https://discourse.getdbt.com/t/benchmarking-incremental-strategies-on-bigquery/981](https://discourse.getdbt.com/t/benchmarking-incremental-strategies-on-bigquery/981)。这意味着我将对分区进行操作而不是扫描数据，因此我会选择最近的两个日期分区（以防我有延迟到达的事件），对它们进行汇总，然后将其添加到我的 dbt 模型 `twitter_likes_daily` 中。

```py
{% set partitions_to_replace = [
    'current_date',
    'date_sub(current_date, interval 1 day)'
] %}

{{
    config(
        materialized='incremental',
        partition_by = { 'field': 'date', 'data_type': 'date' },
        cluster_by = ["user_id"],
        incremental_strategy = 'insert_overwrite'
        partitions = partitions_to_replace
    )
}}

SELECT 
    user_id,
    DATE(timestamp) as date, 
    COUNT(*) as nr_likes 
FROM {{ source ('twitter', 'twitter_likes')}}
-- I have a fake condition here because 
-- my raw table does not let me query without a partition filter
WHERE DATE(timestamp) >= "1990-01-01"

{% if is_incremental() %}
  -- this filter will only be applied on an incremental run
   AND DATE(timestamp) in ({{ partitions_to_replace | join(',') }})
{% endif %}
GROUP BY 1, 2
```

`twitter_likes_daily` 仅为 55.52 GB，日加载扫描 536.6 MB！让我们创建每月汇总数据，为此，我们可以直接在 `twitter_likes_daily` 上工作，而不是原始的 `twitter_likes`。

```py
{% set partitions_to_replace = [
    'date_trunc(current_date, month)',
    'date_trunc(date_sub(current_date, interval 1 month), month)'
] %}

{{
    config(
        materialized='incremental',
        partition_by = { 'field': 'month_year', 'data_type': 'date', "granularity": "month" },
        cluster_by = ["user_id"],
        incremental_strategy = 'insert_overwrite'
        partitions = partitions_to_replace
    )
}}

SELECT 
    user_id,
    DATE_TRUNC(date, MONTH) as month_year, 
    SUM(nr_likes) as nr_likes 
FROM {{ref('twitter_likes_daily')}}

{% if is_incremental() %}
  -- this filter will only be applied on an incremental run
   AND DATE_TRUNC(date, MONTH) in ({{ partitions_to_replace | join(',') }})
{% endif %}
GROUP BY 1, 2
```

`twitter_likes_monthly` 的大小为 14.32 GB，而一次加载仅扫描 2.5 GB！

现在让我们来看看总数。在这种情况下，我们可以决定运行一个 `merge` 操作，只更新过去 2 天内有新点赞的用户的总数。其余用户没有更新。因此，在这种情况下，我可以从 `twitter_likes_daily` 中挑选出今天有点赞的用户，计算他们在 `twitter_likes_monthly` 中的总数，最后将它们合并到包含总数的表 `twitter_likes_total` 中。请注意，该表使用 `user_id` 进行 `clustered`，因为与 id 的集群合并比常规合并更高效 [https://discourse.getdbt.com/t/benchmarking-incremental-strategies-on-bigquery/981](https://discourse.getdbt.com/t/benchmarking-incremental-strategies-on-bigquery/981)。

```py
{{
    config(
        materialized='incremental',
        unique_key = 'user_id',
        cluster_by = ["user_id"],
        incremental_strategy = 'merge'
    )
}}

{% if is_incremental() %}
WITH users_changed as (
    SELECT DISTINCT user_id
    FROM {{ref('twitter_likes_daily')}}
    WHERE date = current_date()
)

{% endif %}
SELECT 
    user_id,
    sum(nr_likes) as total_likes
FROM {{ref('twitter_likes_monthly')}}

{% if is_incremental() %}
  AND user_id in (select user_id from users_changed)
{% endif %}
group by  1 
```

`twitter_likes_total` 是 1.6 GB，但扫描了 16 GB！所以从 869.91 GB 中，我们仅扫描了 16 GB，并生成了一些轻量且便宜的分析表。

然而，`twitter_likes_total` 的方法也可以是一个仅扫描 `twitter_likes_monthly` 的 `dbt` 表配置。如下所示：

```py
{{
    config(
        materialized='table',
        cluster_by = ["user_id"]
    )
}}

SELECT 
    user_id,
    sum(nr_likes) as total_likes
FROM {{ref('twitter_likes_monthly')}} 
```

上述脚本将扫描 14.5 GB，因此实际上比 `merge` 过程更轻量！这到底发生了什么？重要的是要理解，`merge` 可能比完全重建表的效果差。鉴于我们扫描的表 `twitter_likes_monthly` 不大且为 14.5 GB，完全重建表比 `merge` 更高效。因此，当数据量不是很大时，`merge` 实际上可能是过度工程。

**总结一下**

原始不可变事件的扫描可以通过在 `dbt` 上使用增量模型高效处理，特别是使用静态插入+覆盖策略。改进可能是巨大的，从 TB 减少到每天几 GB 的 dbt 负载，但最重要的是，能够让数据分析师在原本成本高昂的原始表扫描上运行查询，转而使用一些轻量、预聚合的表。最后，虽然向表中追加数据在理论上可能比重建表便宜，但根据用例，有时使用 `merge` 可能比完全重建表的性能差，尤其是当源数据只有几 GB 时。
