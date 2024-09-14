# 在 BigQuery 中使用 SQL 识别新客户和回头客

> 原文：[`towardsdatascience.com/identifying-new-and-returning-customers-in-bigquery-using-sql-81f44c9e3598`](https://towardsdatascience.com/identifying-new-and-returning-customers-in-bigquery-using-sql-81f44c9e3598)

## 为了更好地理解客户的兴趣和行为，以及改进你的营销策略。

[](https://romaingranger.medium.com/?source=post_page-----81f44c9e3598--------------------------------)![Romain Granger](https://romaingranger.medium.com/?source=post_page-----81f44c9e3598--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81f44c9e3598--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----81f44c9e3598--------------------------------) [Romain Granger](https://romaingranger.medium.com/?source=post_page-----81f44c9e3598--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----81f44c9e3598--------------------------------) ·8 分钟阅读·2023 年 1 月 4 日

--

![](img/91184e9b57fb6e50c3e8cec94a069d4e.png)

由[Vincent van Zalinge](https://unsplash.com/@vincentvanzalinge?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 主要好处

对客户进行分类，包括新客户和回头客，有助于定义使用哪种营销或销售策略。毫无疑问，这将取决于你的业务性质，你是否优先考虑获取、留存或两者兼顾。

在深入探讨 SQL 方法和步骤之前，以下是这些术语的简单定义和业务示例：

+   **新客户：** 指首次购买的顾客

+   **回头客：** 指已经进行了多次购买的顾客

以**汽车公司**和**咖啡店**公司为例：

**对于汽车公司**来说，回头客的可能性较低，因为汽车购买通常不频繁，价格较高，客户可能几年才进行一次购买。策略可能是专注于获取新客户和接触新客户。

**相比之下，在线咖啡店**销售的是定期购买的消耗品，如咖啡豆或咖啡粉。价格点更为实惠，这提高了客户回头的可能性。

策略可以适用于两种情况：通过**赠品**、上层漏斗广告等方式获取新客户，或尝试推动品牌**发现和认知**。也可以用于回头客，通过定制的营销和信息传递、**产品推荐**、激励和**折扣**、忠诚度计划等方式。

在 SQL 中，为客户贴标签可能有助于启用一些洞察项目：

+   **理解客户行为**：通过检查回头客的行为和衍生模式，你可能会了解什么激励他们再次购买。

+   **个性化和自定义消息**：通过将属性（例如`new`或`returning`）转发到你的营销工具，并为每种客户类型创建特殊的营销细分

+   **客户体验和满意度**：通过进行调查或查看按客户类型提出的客户服务问题

+   **产品教育或理解**：通过查看产品的入门和使用（有些产品可能起初较难理解或使用）。

让我们深入数据，一步一步地进行分类！

# 创建一个包含客户、订单和日期的表格

为了说明如何实现这种分类，我们将使用一些来自[Google Merchandise Store](https://shop.googlemerchandisestore.com/)的数据，这是一家销售 Google 品牌产品的在线商店。

在这个数据集中，我们有**三个月的历史**（从 2020 年 11 月 1 日到 2021 年 1 月 31 日），包含各种不同的事件（购买、页面查看、会话开始、加入购物车等）

我们的第一步将是构建一个包含**3 个字段**的基本表格，其中包含**按客户和日期排列的订单**。

Google Analytics 4（GA4）数据的 SQL 查询如下：

```py
SELECT
  user_pseudo_id AS user_id,
  ecommerce.transaction_id AS order_id,
  PARSE_DATE('%Y%m%d', event_date) AS order_date
FROM
  `bigquery-public-data.ga4_obfuscated_sample_ecommerce.events_*`
WHERE
  event_name = 'purchase'
  AND ecommerce.transaction_id <> '(not set)'
GROUP BY
  1,
  2,
  3
```

数据被过滤以仅包含具有有效交易 ID 的**购买记录行**。

值得注意的是，如果数据无法访问、未被正确跟踪或出于其他原因，Google Analytics 将存储一个默认的`(not set)`值。

然后，结果在所有三个维度上**进行分组**，使每一行代表每个客户在每个日期的唯一订单。

这会生成以下数据表，**我们的第一步完成了！**

![](img/1676d04d273ee0f3b418fa3670a13c95.png)

每一行代表客户在特定日期下的一个订单（图片来自[作者](https://romaingranger.medium.com/)）

如果你想了解更多关于“未设置”值的信息，可以参考 Google Analytics 文档。

[## “未设置”值的含义

### 是 Analytics 在未收到你选择的维度的任何信息时使用的占位符名称……

support.google.com](https://support.google.com/analytics/answer/2820717?hl=en&source=post_page-----81f44c9e3598--------------------------------#zippy=%2Cin-this-article)

如果你想访问和探索 Google Analytics 4 的样本数据集，你可以从这个链接获取。

[](https://developers.google.com/analytics/bigquery/web-ecommerce-demo-dataset?source=post_page-----81f44c9e3598--------------------------------) [## BigQuery 样本数据集用于 Google Analytics 4 电子商务网页实现 | Google Analytics…

### Google Merchandise Store 是一个销售 Google 品牌商品的在线商店。该网站使用 Google Analytics 4 的……

developers.google.com](https://developers.google.com/analytics/bigquery/web-ecommerce-demo-dataset?source=post_page-----81f44c9e3598--------------------------------)

# 将客户分类为新客户或回头客

我们的目标是根据客户的订单历史确定他们是新客户还是回头客，并在我们的表格中添加一列以给他们打上标签。

我们使用窗口函数 `DENSE_RANK()` 来找到每个客户的第一次订单。

+   当**订单的排名为 1**时，我们认为该客户是 `'new'`

+   当**订单的排名大于 1**时，我们认为该客户是 `'returning'`

```py
SELECT
  *,
  CASE
    WHEN DENSE_RANK() OVER(PARTITION BY user_id ORDER BY order_date) = 1 
    THEN 'new' ELSE 'returning'
END
  AS customer_type
FROM
  `datastic.new_existing.base_table`
```

使用 `DENSE_RANK()` 允许我们**将相同的排名分配给同一天内下的多个订单**。有关此函数和其他编号函数的更多详细信息，您可以阅读这篇中等文章：

[](/differences-between-numbering-functions-in-bigquery-using-sql-658fb7c9af65?source=post_page-----81f44c9e3598--------------------------------) [## 在 BigQuery 中使用 SQL 的编号函数之间的差异]

### 了解如何使用排名、密集排名、行号、累计分布、百分位排名、四分位数、百分位数等…

towardsdatascience.com](/differences-between-numbering-functions-in-bigquery-using-sql-658fb7c9af65?source=post_page-----81f44c9e3598--------------------------------)

您将在新列 `customer_type` 中看到，根据 `order_date` 和 `customer_id` 来确定的标签。

![](img/83c431eb7d49d79ef0fce8dc695dca0f.png)

基于订单历史对客户进行新客户或回头客分类（图片来自 [作者](https://romaingranger.medium.com/)）

在 2020 年 9 月 12 日，客户 `13285520` 下了第一笔订单，并被标记为 `'new'`。

然后，在 2020 年 12 月 12 日，这个相同的客户下了第二笔订单，这使其被标记为 `'returning'` 类型。

## 直观分析新客户和回头客

然后，我们可以绘制新客户与回头客户的比例随时间的变化：

![](img/83eba1fa48a7ace94e18e6072c89468e.png)

随着时间的推移，新客户与回头客的比较（图片来自 [作者](https://romaingranger.medium.com/)）

在这种情况下，我们观察到大多数客户都是新的，而真正回头的客户很少。但请记住，我们没有很多历史数据。

## 回购率

将客户分类为 `new` 或 `returning` 的一个潜在好处是，我们可以计算一个指标，提供一个总体评分，表示有多少客户会回购。

从我们之前的加班图表中，我们可以假设 Google 商户商店的回购率较低。为了计算这个指标，我们使用以下公式：

**回头客率 (%) = (回头客数量 / 客户总数) × 100**

在这 3 个月的时间里，我们有 3713 个客户（注意这些客户在某个时点都是新的），其中 256 个客户下了多于一个订单。

这给出的是 (256/3713) * 100 = **6.9% 回头客率**

# 额外的建议和想法

## 考虑时间分辨率

在我们的例子中，我们每天查看客户订单，考虑时间的作用，而不仅仅是订单的数量。

如果你只运行一个查询，查看每个客户的订单数量，而不考虑时间（例如，假设一个有超过 1 个订单的客户是`returning`），可能会发生一些客户在同一天有多个订单，这样他们会被认为是回头客，但可能不会在之后再回来。

此外，回顾按月或按年也可能导致不同的数字。时间段越长，客户同时是`new`和`returning`的机会就越高。在这种情况下，这涉及到管理重复数据并添加额外的分类，如`both`，或优先考虑`new`或`returning`类型。

为了说明这个想法，让我们只查看 2020 年的每个月（我们将之前的查询结果存储在一个名为`customer_table`的表中）：

```py
SELECT
  user_id,
  DATE_TRUNC(order_date,MONTH) AS month,
  STRING_AGG(DISTINCT customer_type
  ORDER BY
    customer_type ASC) AS customer_type
FROM
  customer_table
WHERE
  order_date < '2021-01-01'
GROUP BY
  1,
  2
```

`STRING_AGG()`函数用于将不同的`customer_type`值按字母顺序连接成一个字符串（这样在使用`CASE WHEN`语句时更容易，因为值的顺序将保持按字母顺序排列）。

查询将返回以下结果：

![](img/174c79c099c2da8051b6464d6372b8f8.png)

一个客户可能在同一个月内既是新客户又是回头客（图片来自[作者](https://romaingranger.medium.com/)）。

如你所见，在 2020 年 12 月，客户（`234561…901`）第一次下单，并在同一个月再次购买。

在这种情况下，你可能想要定义以下分类：

+   将这些客户视为`new`。

+   将这些客户视为`both`。

要在 SQL 中更改标签，你可以这样做：

```py
WITH
  customer_month AS (
  SELECT
    user_id,
    DATE_TRUNC(order_date,MONTH) AS month,
    STRING_AGG(DISTINCT customer_type
    ORDER BY
      customer_type ASC) AS customer_type
  FROM
    customer_table
  WHERE
    order_date < '2021-01-01'
  GROUP BY
    1,
    2)

  -- Main Query
SELECT
  *,
  CASE
    WHEN customer_type LIKE 'new,returning' THEN 'both'
  ELSE
  customer_type
END
  AS c_type
FROM
  customer_month
```

我们可以通过直接按月分组找到更理想的查询，然而，这在**使用报告工具并保持每日分辨率时**可能是**有用的**，例如，在仪表板上添加过滤器。

带有新字段的表格将如下所示：

![](img/efaa5db1652502e6313e44f9ce48a9df.png)

同时是新客户和回头客的客户被分类为不同类型（图片来自[作者](https://romaingranger.medium.com/)）。

## 额外的支持指标

虽然重复购买率可以是客户对你的产品的忠诚度或满意度的一个指标，但它并不表示客户产生了多少收入（他们是否带来了更多价值）或客户是否对他们的购买感到满意。

为了支持这个假设，我们可以将重复购买率与其他指标结合起来，例如：

+   **客户终身价值 (CLV)**，例如，可能是一个重要的增长指标，展示了客户将与你保持多久以及他们在你的产品上花费了多少。

+   **净推荐值 (NPS)** 可能是另一个有趣的指标，用于评估客户满意度，并可能通过附加问题或调查了解他们为何再次购买。

我们还可以添加其他几个指标，包括平均订单值 (AOV) 或使用按月或按年分组的方法来计算客户保留率。

我们还可能使用 RFM 评分（最近性、频率、货币性）或其他方法来更好地理解您的客户群体。

# 参考资料与数据集

本文使用的数据集来自于[BigQuery 公共数据](https://cloud.google.com/bigquery/public-data)，是 Google Analytics 数据的一个样本，遵循[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) 许可。

[](https://developers.google.com/analytics/bigquery/web-ecommerce-demo-dataset?source=post_page-----81f44c9e3598--------------------------------) [## BigQuery 示例数据集用于 Google Analytics 4 电子商务网站实施 | Google Analytics…

### Google 商品商店是一个在线商店，销售 Google 品牌商品。该网站使用 Google Analytics 4 的…

developers.google.com](https://developers.google.com/analytics/bigquery/web-ecommerce-demo-dataset?source=post_page-----81f44c9e3598--------------------------------)
