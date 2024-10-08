# 添加一行 SQL 语句以优化你的 BigQuery 表格

> 原文：[`towardsdatascience.com/add-one-line-of-sql-to-optimise-your-bigquery-tables-304761b048f0`](https://towardsdatascience.com/add-one-line-of-sql-to-optimise-your-bigquery-tables-304761b048f0)

## 聚类：一种简单的方法，用于将相似的行分组，防止不必要的数据处理

[](https://medium.com/@mattchapmanmsc?source=post_page-----304761b048f0--------------------------------)![Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----304761b048f0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----304761b048f0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----304761b048f0--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----304761b048f0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----304761b048f0--------------------------------) ·阅读时长 5 分钟·2023 年 12 月 8 日

--

在我之前的文章中，我解释了如何使用分区来优化 SQL 查询：

[](/use-the-partitions-luke-a-simple-and-proven-way-to-optimise-your-sql-queries-43e24ea4c5d0?source=post_page-----304761b048f0--------------------------------) ## 使用分区，卢克！一种简单且经过验证的优化 SQL 查询的方法

### 如果你曾经写过一个运行时间非常长的 SQL 查询，这篇文章适合你

towardsdatascience.com

现在，我正在写**续集**！（有爸爸笑话吗？）

这篇文章将探讨**聚类**：另一种强大的优化技术，你可以在 BigQuery 中使用。与分区类似，聚类可以帮助你编写更高效的查询，使查询运行更快、成本更低。如果你想提升你的 SQL 工具包并建立更高级的数据科学技能，这是一个很好的起点。

# 什么是聚类表？

在 BigQuery 中，聚类表是一种将相似的行在物理“块”中分组的表。

例如，设想一个名为`user_signups`的表格，用于跟踪所有在虚构网站上注册帐户的人员。它有四列：

+   `registration_date`：用户创建帐户的日期

+   `country`：用户所在的国家

+   `tier`：用户的计划（“免费”或“付费”）

+   `username`：用户的用户名

如果我们愿意，我们可以通过`country`对表格进行聚类，使得来自同一国家的用户在表格中彼此靠近存储：

![](img/e5cb967bc391fbb357a09655caed09c8.png)

图片由作者提供

如你所见，表中的每个“块”包含来自特定国家的用户。聚簇表仍然包含相同的数据，只是以更高效的方式排序。

# 聚簇加速了我们的查询，因为这意味着 BigQuery 需要处理的数据更少。

当你查询一个聚簇表时，BigQuery 会首先识别执行查询所需的相关块。这一步骤——称为**块修剪**——使你的查询更快且成本更低，因为 BigQuery 不会对无关的块执行不必要的操作。它只使用实际需要的块/数据。

为了看到这一点的好处，让我们假设我们的`user_signups`表包含 1,000,000 行，并且我们想要获取来自黎巴嫩且在 2023-12-01 注册的用户。我们会写：

```py
SELECT *
FROM user_signups
WHERE
  country = 'Lebanon'
  AND registration_date = '2023–12–01'
```

当我们运行那段代码时，BigQuery 会（理论上）从找到相关块（在这种情况下，就是包含来自黎巴嫩的用户的块）开始，然后筛选该块以找到`registration_date`等于“2023-12-01”的行。它不需要读取表中的所有行，只需读取相关聚簇中的行。

# 重要提示：聚簇不会总是按照你期望的方式工作（特别是对于小表格）

![](img/71d367afd9b83008bb674aaf6f1a0637.png)

图片由[Madison Oren](https://unsplash.com/@artbyhybrid)提供，来源于[Unsplash](https://unsplash.com/photos/low-angle-photo-of-pink-and-orange-balloons-uGP_6CAD-14)

BigQuery 很聪明。

它知道创建（和修剪）聚簇/块需要计算能力，有时管理这些聚簇所需的工作量太大，无法带来任何性能提升。

因此，BigQuery 不*一定*会为聚簇列中的每个不同值创建一个新的块（[1](https://usercentrics.com/knowledge-hub/optimizing-your-bigquery-tables-using-clustering/)）。正如数据工程师[Alessandro](https://stackoverflow.com/questions/66951917/how-to-know-the-minimum-cluster-size-in-a-bigquery-table)所写，

> “聚簇不同于分区。事实上，没有保证每个列值会有一个聚簇……这也是为什么 BigQuery 不能在运行查询之前给你一个数据使用量的良好估算（就像它对分区所做的那样）。”

从这个角度考虑：如果你的整个表只有 10 行，那么扫描整个表可能比进行块修剪和管理聚簇的过程要更快。BigQuery 知道这一点，所以它不会在聚簇上浪费资源。

要获得聚簇的好处，你的表需要有多大？根据[一位](https://stackoverflow.com/a/52674573)前谷歌工程师的说法，

> “如果每个[你想要聚簇的组]的数据少于 100MB，聚簇对你帮助不大。”

不得不说，值得注意的是，如果你在非聚类列上进行查询，聚类将无法帮助你的查询（例如，如果我在上述查询中没有包含`WHERE country = ...`筛选条件，我们将无法在`country`列上应用块裁剪）。

# 创建聚类表

创建聚类表非常简单。只需在`CREATE TABLE`语句的末尾添加`CLUSTER BY`子句：

```py
CREATE TABLE `myproject.mydataset.clustered_table` (
  registration_date DATE,
  country STRING,
  tier STRING,
  username STRING
) CLUSTER BY country; # Add this
```

你可以通过最多四列进行聚类，并且（与分区不同）你不限于`INT64`或`DATE`列；你还可以通过如`STRING`和`GEOGRAPHY`等数据类型的列进行聚类。

这就是将我们的`user_signups`表按两列进行聚类的效果：

![](img/bbdf57aa6b9807a7941e1bf1529a1039.png)

作者提供的图片

# 结合使用聚类和分区以获得最佳性能

在我之前的文章中，我介绍了分区：一种基于日期或整数将表格拆分成物理分区的方法。

幸运的是，将聚类与分区结合起来非常简单。

下面是一个示例，展示了我们的`user_signups`表，现在按`registration_date`进行分区，并按`country`进行聚类：

![](img/e87d5ffac70137bc047be14ff8535041.png)

作者提供的图片

当你查询表格时，BigQuery 会首先尝试应用**分区裁剪**（以识别相关的分区），然后在相关分区内应用**块裁剪**以查找相关的行。

这也是我如此喜欢使用 BigQuery 的原因之一：它将两者的优点结合在了一起！

# 想找一个练习 SQL 的地方吗？

如果你喜欢这篇文章，你可能会喜欢我的网站[the-sql-gym.com](https://the-sql-gym.com/)，它包含了 100 多个 SQL 练习题。如果你想提升你的 SQL 技能，赶紧看看吧！

感谢阅读，欢迎通过[Twitter](https://twitter.com/matt_chapma)或[LinkedIn](https://www.linkedin.com/in/matt-chapman-ba8488118/)与我联系！ :-)
