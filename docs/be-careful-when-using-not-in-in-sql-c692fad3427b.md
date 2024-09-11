# 使用 SQL 中的“NOT IN”时请小心

> 原文：[https://towardsdatascience.com/be-careful-when-using-not-in-in-sql-c692fad3427b?source=collection_archive---------11-----------------------#2023-12-15](https://towardsdatascience.com/be-careful-when-using-not-in-in-sql-c692fad3427b?source=collection_archive---------11-----------------------#2023-12-15)

## + 3 个简单解决方案，确保你不会被困住

[](https://medium.com/@mattchapmanmsc?source=post_page-----c692fad3427b--------------------------------)[![Matt Chapman](../Images/7511deb8d9ed408ece21031f6614c532.png)](https://medium.com/@mattchapmanmsc?source=post_page-----c692fad3427b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c692fad3427b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c692fad3427b--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----c692fad3427b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf7d13fc53db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbe-careful-when-using-not-in-in-sql-c692fad3427b&user=Matt+Chapman&userId=bf7d13fc53db&source=post_page-bf7d13fc53db----c692fad3427b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c692fad3427b--------------------------------) ·5 分钟阅读·2023年12月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc692fad3427b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbe-careful-when-using-not-in-in-sql-c692fad3427b&user=Matt+Chapman&userId=bf7d13fc53db&source=-----c692fad3427b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc692fad3427b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbe-careful-when-using-not-in-in-sql-c692fad3427b&source=-----c692fad3427b---------------------bookmark_footer-----------)

最近，我阅读了[本杰明·图尔尔（Benjamin Thürer）](https://medium.com/@benjamin.thuerer)的一篇优秀文章：

[](/how-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88?source=post_page-----c692fad3427b--------------------------------) [## 如何避免 Google BigQuery / SQL 中的五个常见错误

### 在多年使用 BigQuery 的过程中，我观察到即使是经验丰富的数据科学家也经常犯的 5 个问题

towardsdatascience.com](/how-to-avoid-five-common-mistakes-in-google-bigquery-sql-6fafab396d88?source=post_page-----c692fad3427b--------------------------------)

…在其中他警告我们在 BigQuery 中使用 `NOT IN` SQL 子句时的注意事项。

在这篇文章中，我将通过提供更多示例、解决方案和练习题来扩展他的观点。

如果你想了解为什么 `NOT IN` 子句是有风险的——以及应该如何应对——请继续阅读！

# 问题：`NOT IN` 不能按照你预期的方式处理 `NULL` 值

`IN` 和 `NOT IN` 运算符提供了一种逻辑方式来比较数组。例如，如果你写：

```py
SELECT 
  3 IN (1, 2, 3) # Output = true
```

BigQuery 将返回 `true`。如果你写：

```py
SELECT 
  3 NOT IN (1, 2, 3) # Output = false
```

BigQuery 将返回 `false`。

看起来很简单，对吧？但有一个问题：当查找数组包含 `NULL` 值时，`IN` 和 `NOT IN` 会做出奇怪的操作。以下代码，例如，将返回 `NULL`，而不是 `false`：

```py
SELECT
  3 NOT IN (1, 2, NULL) # Output = NULL
```
