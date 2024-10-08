# 如何修复时间序列分析中的缺失日期

> 原文：[`towardsdatascience.com/how-to-fix-missing-dates-for-time-series-analysis-26358360abcc`](https://towardsdatascience.com/how-to-fix-missing-dates-for-time-series-analysis-26358360abcc)

## 学习如何在 BigQuery 中使用 TVFs 来轻松生成时间序列分析所需的日期范围。

[](https://medium.com/@thomas.ellyatt?source=post_page-----26358360abcc--------------------------------)![汤姆·艾利亚特](https://medium.com/@thomas.ellyatt?source=post_page-----26358360abcc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----26358360abcc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----26358360abcc--------------------------------) [汤姆·艾利亚特](https://medium.com/@thomas.ellyatt?source=post_page-----26358360abcc--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----26358360abcc--------------------------------) ·阅读时长 7 分钟·2023 年 7 月 20 日

--

![](img/50974d6ab642db33cc05beee117d3727.png)

本文的目标是帮助你理解 TVFs 及其使用方法，通过一个例子来解决时间序列分析中常见的缺失日期问题。

在某些情况下，零数据的日期是重要的，必须在数据集中显示/包含。例如：

+   企业可以通过识别零销售的日期获益。这些日期受到节假日或客户行为变化的影响。

+   发现数据中的缺失日期有助于通过揭示系统故障或数据捕获不完整引起的异常或离群值来提高数据质量。显示缺失日期是实现这一目标的有用工具。

这些缺失日期可能会导致分析和可视化问题。因此，你需要一个解决方案，确保所有日期都在输出中，即使没有对应的数据。

到本文结束时，你将拥有自己的 TVF，能够生成这个……

![](img/762c6ffcc7468c0ec8af7c7df722a9cd.png)

只需一行代码！

![](img/e1951c31f988a78b375c4c0834625455.png)

# 我们将涵盖：

+   如何**生成日期**以填补数据中的缺失空白

+   如何**创建一个 TVF**及其参数的使用

+   如何**调用** **TVF**

+   我们将探讨**扩展我们的日期生成器**以获得更大的灵活性。

+   最后，我将分享**如何访问我的 TVF**，并介绍一个名为**BigFunctions**的开源项目。

# 问题

考虑这种情况：你运行了一个查询，提供了过去四周的每日期总调查回应结果。然后，你将结果导入 Google Sheets 以快速可视化数据。

![](img/9a3eb50c7b520e100ba07bb36799c82c.png)

上图没有突出显示任何缺失数据；它看起来完全符合预期。即使你选择在 x 轴上显示所有日期，你也可以原谅没注意到七月中缺失的两天。

![](img/529c22c8f7b9142adfe5bfb04b200452.png)

## **我们如何解决这个问题**

在我们深入 TVF 话题之前，让我们谈谈我解决此类问题的方法，以及为什么我将其打包成 TVF。

为了解决这个问题，我创建了我喜欢称之为 **日期轴** 的东西。这一列日期/周/月，无论你需要哪个时间段，都与正在分析的数据集分开构建。这确保了日期是独立的，不依赖于数据的存在。

创建日期轴相对直接，但如果你经常需要创建日期轴，可能会觉得有些繁琐。

以下是一个简单的示例，它生成 2023 年 6 月 19 日至 7 月 16 日之间的日期。

```py
WITH date_axis as (SELECT
  dates
FROM
  UNNEST(generate_date_array("2023-06-19","2023-07-16")) as dates
)

SELECT
  dates
FROM
  date_axis
```

![](img/44c35747f5f5de5f1aea9a923f60c220.png)

`generate_date_array` 函数是关键部分，但正如函数名所示，输出是以数组形式返回的。因此，我们必须展开（扁平化）这个数组以进行下一步。

日期轴存在于 CTE 中，我们需要将其视为一个单独的表，以便将实际数据左连接到日期列表上。

```py
WITH date_axis as (SELECT
  dates
FROM
  UNNEST(generate_date_array("2023-06-19","2023-07-16")) as dates
)

SELECT
  dates,
  responses as original_responses,
  ifnull(responses,0) as new_responses
FROM
  date_axis as axis
LEFT JOIN
  `spreadsheep-20220603.Case_Studies.survey_responses` as survey
  ON axis.dates = survey.date
```

![](img/7852f57252c3f5ddb2cdceda525d8608.png)

如上所示，我们的 survey_responses 表中 7 月 1 日和 2 日的值为 *null*，因为这些日期不存在。使用日期轴，我们可以轻松发现这些并适当地处理，在这种情况下，*null* 值被替换为 0。

重新绘制更新后的数据，我们现在捕捉到了 7 月初响应的缺失。

![](img/4151967423399465309fa627c0e1b166.png)

## TVF 到底是什么？

TVF 是 Table-Valued Function 的缩写。与 UDF（用户定义函数）类似，它们允许你指定一系列任务，每当调用自定义函数时，这些任务都会运行。

二者的区别在于 UDF 为数据集中的每一行返回结果，而 TVF 返回的是整个表。

你可能会问，如果 CTE 方法已经能完美解决问题，那么 TVF 有什么意义。实际上，在 TVF 中，我们可以扩展日期轴函数的功能和重用性，简化我们的代码。

使用 TVF 有很多创造性和实用的方法，在这篇文章中，我们将使用一个生成日期轴。

## 创建一个 TVF

```py
CREATE OR REPLACE TABLE FUNCTION `spreadsheep-20220603.Case_Studies.generate_dates`(start_date DATE, end_date DATE)
AS (
SELECT
  dates
FROM
  UNNEST(generate_date_array(start_date,end_date)) as dates
)
```

创建 TVF 很简单；首先使用 `create or replace table function`，然后指定你希望将 TVF 保存到项目中的位置。接着，你可以添加参数，在这个例子中我们添加了两个。

`start_date DATE, end_date DATE`

如下所示，这两个参数替换了我们添加到 `generate_date_array` 函数中的静态值。

`unnest(generate_date_array(start_date,end_date)) as dates`

当你的 TVF 被创建后，你可以像调用表一样调用你的新函数。注意，我在 FROM 子句的末尾添加了括号，以指定我希望 TVF 使用的值，其中 7 月 1 日是开始日期，7 月 7 日是结束日期。

```py
SELECT 
  dates 
FROM 
  `spreadsheep-20220603.Case_Studies.generate_dates`("2023-07-01", "2023-07-07")
```

![](img/3d73f022e7d383fd3819474423cdaf30.png)

我们现在可以更新原始查询以使用新的 TVF。

```py
WITH date_axis as (
SELECT 
  dates 
FROM 
  `spreadsheep-20220603.Case_Studies.generate_dates`("2023-06-19", "2023-07-16")
)

SELECT
  dates,
  responses as original_responses,
  ifnull(responses,0) as new_responses
FROM
  date_axis as axis
LEFT JOIN
  `spreadsheep-20220603.Case_Studies.survey_responses` as survey
  ON axis.dates = survey.date
```

## 扩展 TVF

目前这个函数的功能相对有限，因为它只提供日期。如果我们想要以星期天为周开始的周起始日期，或者我们想要过去几年季度的开始和结束日期怎么办？

虽然我们可以将该逻辑添加到调用 TVF 的 CTE 中，但我们还是在 TVF 中处理它，这样当我们需要时就可以随时使用。

我的最终版本根据你需要的周、月或季度日期范围，增加了其他一些可能性。

```py
CREATE OR REPLACE TABLE FUNCTION `spreadsheep-20220603.Case_Studies.generate_dates`(start_date DATE, end_date DATE)
OPTIONS (description="Generate a table of dates") AS (
(
select
  date,
  format_date("%a", date) as day_of_week,
  date_trunc(date, week(monday)) as week_start_monday,
  date_trunc(date, week(monday)) + 6 as week_end_monday,
  date_trunc(date, week(sunday)) as week_start_sunday,
  date_trunc(date, week(sunday)) + 6 as week_end_sunday,
  date_trunc(date, month) as month_start,
  date_add(date_trunc(date, month), interval 1 month) - 1 as month_end,
  date_trunc(date, quarter) as quarter_start,
  date_add(date_trunc(date, quarter), interval 1 quarter) - 1 as quarter_end,
from unnest(
  generate_date_array(
    start_date,
    end_date
  )
) as date
)
);
```

这使我们得到了文章开头看到的输出，其中单行查询可以生成一年的日期，以及它们的周、月和季度部分。

![](img/762c6ffcc7468c0ec8af7c7df722a9cd.png)

作为额外的好处，我们创建的这个函数不会查询任何实际数据。也就是说，它完全可以免费运行且速度极快。

即使是生成从 1820 年到当前的日期也只花费了 1 秒钟。

```py
SELECT * FROM `spreadsheep-20220603.Case_Studies.generate_dates`("1820-07-01","2023-07-15")
```

![](img/c7d80a639a0da513b9572e15c5424439.png)![](img/4c37eeecc4001fb33124b51b18b25ff0.png)

由 [Benjamin Davies](https://unsplash.com/it/@bendavisual?utm_source=medium&utm_medium=referral) 提供的照片，出处 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 访问我的 TVF

为了节省时间，你不必在你的项目中创建这个 TVF；你可以使用公共版本，它存在于 BigFunctions 开源项目中。

要将 BigFunctions 添加到你的项目中，你可以使用探索器的添加功能，然后按照下面的示例‘*按名称标记项目*’。

![](img/6a1503b64ee92d1afdc931b6968cd42d.png)

这些函数在每个区域都可以使用，在每个数据集中，你会在 **Routines** 下找到 **generate_dates**。试试下面的代码吧！

```py
SELECT * FROM `bigfunctions.europe_west2.generate_dates`("2022-01-01", "2023-01-01");
```

更多关于 BigFunctions 的详细信息可以在 [这里](https://unytics.io/bigfunctions/) 找到，这里充满了很棒的自定义函数，其中一些甚至使用 Python 来运行各种有趣的功能。如果你在日常工作中使用 BigQuery，可以查看一下。

这篇文章就此结束。如果你有任何问题，请随时评论，我会尽快回答。

我经常为 BigQuery 和 Looker Studio 撰写文章。如果你感兴趣，可以考虑在 Medium 上关注我以获取更多内容！

> *所有图像，除非另有说明，均由作者提供。*

***保持优雅，大家！

Tom***
