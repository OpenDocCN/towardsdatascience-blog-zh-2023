# 提升你在 BigQuery 上 dbt 运行速度的简单方法

> 原文：[`towardsdatascience.com/an-easy-way-to-speed-up-your-dbt-runs-on-bigquery-24238ac20f8a?source=collection_archive---------14-----------------------#2023-04-11`](https://towardsdatascience.com/an-easy-way-to-speed-up-your-dbt-runs-on-bigquery-24238ac20f8a?source=collection_archive---------14-----------------------#2023-04-11)

## *通过并发查询加速 dbt 运行*

[](https://medium.com/@tobisam?source=post_page-----24238ac20f8a--------------------------------)![Tobi Sam](https://medium.com/@tobisam?source=post_page-----24238ac20f8a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----24238ac20f8a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----24238ac20f8a--------------------------------) [Tobi Sam](https://medium.com/@tobisam?source=post_page-----24238ac20f8a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F92fab82e0c7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-easy-way-to-speed-up-your-dbt-runs-on-bigquery-24238ac20f8a&user=Tobi+Sam&userId=92fab82e0c7a&source=post_page-92fab82e0c7a----24238ac20f8a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----24238ac20f8a--------------------------------) ·3 min read·2023 年 4 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F24238ac20f8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-easy-way-to-speed-up-your-dbt-runs-on-bigquery-24238ac20f8a&user=Tobi+Sam&userId=92fab82e0c7a&source=-----24238ac20f8a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F24238ac20f8a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-easy-way-to-speed-up-your-dbt-runs-on-bigquery-24238ac20f8a&source=-----24238ac20f8a---------------------bookmark_footer-----------)![](img/dd8787608c94122a0e69e49364a7e974.png)

图片来源：作者

在构建和测试 dbt 项目中的数据模型时，你可能已经意识到，你需要等待相当长的时间才能看到 SQL 转换的结果，通常是在 `dbt build` 或 `dbt run` 之后，特别是当你有一个中型到大型的 dbt 项目时。

dbt 中有一个鲜为人知的功能，可以显著加快你的 dbt 运行速度！这对于拥有许多模型的大型 dbt 项目尤其有用，特别是当你希望更快地查看或运行结果，或者只是为了确保下游业务利益相关者能更快地看到你的转换结果时。

这个功能叫做 Threads。

# Threads

dbt 具有一个名为“[**threads**](https://docs.getdbt.com/docs/get-started/connection-profiles#understanding-threads)”的配置功能，用于确定 dbt 在同一时间可以构建的最大模型数量。你可以在你的 `profiles.yml` 文件中找到它。

```py
jaffle_shop:
  target: dev # this is the default target
  outputs:
    dev:
      type: bigquery
      schema: dataset
      threads: 4
```

线程有助于**并行化**在 dbt 有向无环图 [(DAG)](https://docs.getdbt.com/terms/dag) 中的节点执行。默认线程数目前为 4，但没有最大限制，dbt 允许你**达到数据库的最大限制**。
