# 初级开发者编写多页 SQL 查询；高级开发者使用窗口函数

> 原文：[`towardsdatascience.com/junior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378?source=collection_archive---------5-----------------------#2023-03-13`](https://towardsdatascience.com/junior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378?source=collection_archive---------5-----------------------#2023-03-13)

## 一种优雅的在记录上下文中执行计算的方法

[](https://thuwarakesh.medium.com/?source=post_page-----f82dfeb8e378--------------------------------)![Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f82dfeb8e378--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f82dfeb8e378--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f82dfeb8e378--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----f82dfeb8e378--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjunior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----f82dfeb8e378---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f82dfeb8e378--------------------------------) ·7 min read·Mar 13, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff82dfeb8e378&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjunior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----f82dfeb8e378---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff82dfeb8e378&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjunior-developers-write-multi-page-sql-queries-seniors-use-window-functions-f82dfeb8e378&source=-----f82dfeb8e378---------------------bookmark_footer-----------)![](img/5ecd316079b782495a6f38876a60a489.png)

图片来源：[R Mo](https://unsplash.com/@mooo3721?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

想象一下你经营一家全国性的电子产品商店。你必须确定每个商店对各州销售额的贡献。

如果你刚开始学习 SQL，你可能会思考一会儿，然后编写出一个令人惊叹的查询，可能如下所示。

```py
SELECT
 s.state,
 s.store_name,
 s.total_sales,
 s.total_sales / t.state_total_sales AS sales_contribution
FROM
 (
 SELECT
  state,
  store_name,
  SUM(sales) AS total_sales
 FROM
  store_sales
 GROUP BY
  state,
  store_name
) s
INNER JOIN (
 SELECT
  state,
  SUM(sales) AS state_total_sales
 FROM
  store_sales
 GROUP BY
  state
) t ON
 s.state = t.state
ORDER BY
 s.state,
 sales_contribution DESC;
```

```py
|state|store_name|total_sales|sales_contribution|
|-----|----------|-----------|------------------|
|California|MegaMart|82500.00|0.61797752808988764045|
|California|ABC Mart|51000.00|0.38202247191011235955|
|New York|SuperMart|57000.00|0.60638297872340425532|
|New York|Corner Shop|37000.00|0.39361702127659574468|
|Texas|XYZ Store|35000.00|0.67961165048543689320|
|Texas|My Store|16500.00|0.32038834951456310680|
```
