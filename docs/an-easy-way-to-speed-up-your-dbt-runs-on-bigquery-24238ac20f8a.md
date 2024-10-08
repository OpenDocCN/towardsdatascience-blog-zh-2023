# 加速你的 dbt 在 BigQuery 上的运行的简单方法

> 原文：[`towardsdatascience.com/an-easy-way-to-speed-up-your-dbt-runs-on-bigquery-24238ac20f8a`](https://towardsdatascience.com/an-easy-way-to-speed-up-your-dbt-runs-on-bigquery-24238ac20f8a)

## *使用并发查询加速 dbt 运行*

[](https://medium.com/@tobisam?source=post_page-----24238ac20f8a--------------------------------)![Tobi Sam](https://medium.com/@tobisam?source=post_page-----24238ac20f8a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----24238ac20f8a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----24238ac20f8a--------------------------------) [Tobi Sam](https://medium.com/@tobisam?source=post_page-----24238ac20f8a--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----24238ac20f8a--------------------------------) ·阅读时间 3 分钟·2023 年 4 月 11 日

--

![](img/dd8787608c94122a0e69e49364a7e974.png)

作者提供的图片

在你的 dbt 项目中构建和测试数据模型时，你可能已经意识到，你需要等待一段时间才能看到 SQL 转换的结果，通常是在`dbt build`或`dbt run`之后，尤其是当你的 dbt 项目中等或较大时。

dbt 中有一个鲜为人知的功能，可以显著加速你的 dbt 运行！这特别适用于如果你有一个包含许多模型的大型 dbt 项目，并希望更快地查看或运行结果，或者确保下游业务利益相关者更快地看到你的转换结果。

这个功能叫做线程。

# 线程

dbt 有一个配置功能叫做“[**threads**](https://docs.getdbt.com/docs/get-started/connection-profiles#understanding-threads)”，它决定了 dbt 在同一时间可以构建的最大模型数量。你可以在你的`profiles.yml`文件中找到它。

```py
jaffle_shop:
  target: dev # this is the default target
  outputs:
    dev:
      type: bigquery
      schema: dataset
      threads: 4
```

线程帮助**并行**执行 dbt 有向无环图（[DAG](https://docs.getdbt.com/terms/dag)）中的节点。目前默认线程数量为 4，但没有最大限制，dbt 允许你**达到数据库的最大限制**。

![](img/a96c8affad68f0f68049dcaa18242904.png)

有向无环图（DAG）— 作者提供的图片

在上面的 DAG 中，最多有 3 个模型可以在不违反依赖关系的情况下同时运行，这意味着 dbt 一次最多只能运行 3 个线程/查询。将线程增加到 10 个，将不会减少你的 dbt 运行时间。

但是，如果你有 10 个以上的模型可以同时运行（而不违反依赖关系），将线程数量增加到 10 个或更多，可以减少总执行时间，这非常棒！

# **BigQuery 上的最大查询数**

BigQuery 对你可以同时运行的查询任务数量有一定限制。***任务***是 BigQuery 代表你运行的“任务”，无论是通过 Google Cloud 控制台、`bq` 命令行工具，还是通过 REST API 或客户端库程序化运行。

在 BigQuery 中有**两种类型的查询任务**：

+   **交互式查询任务**（这是 BigQuery 的默认行为）：BigQuery 按需运行的任务，也就是尽快运行。

+   **批处理查询任务**：即 BigQuery 等待空闲计算资源可用时才运行的任务（通常，查询仅排队几分钟）。

你的项目可以在 BigQuery 上[**运行多达 100 个并发交互查询**](https://cloud.google.com/bigquery/quotas#concurrent_rate_batch_queries)。请记住，从[查询缓存](https://cloud.google.com/bigquery/docs/cached-results)中返回结果的查询会计入这一限制，直到 BigQuery 确定这是缓存命中为止。

你的项目最多可以运行**10 个并发批处理查询**（虽然我在项目中看到批处理查询使用不多，但了解这一点仍然很有帮助）。

# 需要考虑的事项

+   增加线程会增加数据仓库的负担，并且**可能**会影响其他依赖于数据仓库的工具**在执行时**。

+   如果数据库/仓库有较小的限制，这将影响同时能够主动构建的模型数量。

在我看来，这些并不是决定性因素，因为好处远远大于限制。

希望这对你有所帮助。

## 参考资料

+   [BigQuery 并发查询](https://cloud.google.com/bigquery/quotas#concurrent_rate_batch_queries)

+   [DBT 线程](https://docs.getdbt.com/docs/core/connection-profiles#understanding-threads)
