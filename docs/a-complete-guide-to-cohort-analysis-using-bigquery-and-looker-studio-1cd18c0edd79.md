# 使用 BigQuery 和 Looker Studio 进行队列分析的完整指南

> 原文：[`towardsdatascience.com/a-complete-guide-to-cohort-analysis-using-bigquery-and-looker-studio-1cd18c0edd79`](https://towardsdatascience.com/a-complete-guide-to-cohort-analysis-using-bigquery-and-looker-studio-1cd18c0edd79)

## 逐步解密队列分析

[](https://medium.com/@damien.azzopardi?source=post_page-----1cd18c0edd79--------------------------------)![Damien Azzopardi](https://medium.com/@damien.azzopardi?source=post_page-----1cd18c0edd79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cd18c0edd79--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cd18c0edd79--------------------------------) [Damien Azzopardi](https://medium.com/@damien.azzopardi?source=post_page-----1cd18c0edd79--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cd18c0edd79--------------------------------) ·阅读时间 5 分钟·2023 年 3 月 9 日

--

![](img/92801613a39771c131cf360df1915772.png)

图片来源：[Hunter Harritt](https://unsplash.com/ja/@hharritt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/photos/Ype9sdOPdYc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

让我们承认，队列分析乍看起来可能令人望而却步。但它是一个强大的工具，可以提供有价值的洞察，有时甚至是可视化数据的唯一正确方法。掌握它们将使你在数据分析的旅程中占据明显优势。

但首先，我们所说的队列分析是什么意思？

> 队列分析是一种研究和比较不同人群随时间变化的方法。

这些群体通过一个共同的特征来定义，例如他们加入服务的日期或首次购买的日期。

队列分析经常用于分析客户终止与产品或服务关系的速率，这一概念通常被称为“流失”。对于基于订阅的企业来说，流失率代表在给定时间内取消订阅的客户百分比。

流失是一个重要的业务指标，可能对收入和增长产生重大影响。虽然高流失率可能是客户不满意的迹象，但低流失率则相反，可能是客户忠诚度和满意度的证明。

现在，让我们通过对一个希望分析其客户在 2023 年行为的基于订阅的应用程序进行流失分析，来展示队列分析的强大功能。

## 第一步——了解您的数据集

在这个示例中，我们将使用存储在 BigQuery 中的 `subscriptions` 表。它包含我们产品上的订阅列表，包括注册日期，最重要的是，关于其状态的信息，***活跃*** 或 ***取消***。下面是表的样子：

![](img/beafb0d1b5eac836043ed989e9874e4f.png)

查询结果（作者提供的图片）

现在你可能想按月查看流失演变。你可以使用以下查询，计算每月流失的客户数，并将其除以相同期间的总客户数：

![](img/34f00bb2804a19feb83b8958be23937b.png)

查询结果（作者提供的图片）

![](img/3e8217ee91c607cc3384fdbefe720427.png)

查询结果绘制图（作者提供的图片）

好消息，流失率在这一年中有所下降。但这是否反映了实际情况？我担心它并没有。用这种方式查看流失率只能代表部分情况。这就是为什么需要引入 cohort。

## 第 2 步 — 数据转换

记住，cohorts 是由共同特征定义的组，在这种情况下，是它们的注册日期。所以我们将其拆分为不同的 cohort；1 月份注册的用户，2 月份注册的用户，依此类推。对于每个 cohort，我们希望知道有多少客户注册，以及在每个时间段内有多少人取消。换句话说，就是在一个月、两个月等之后有多少人取消。

为此，让我们使用以下查询：

![](img/d4a40d922522503e56c3f545861bf57d.png)

查询结果，其中 signup_date = 2023 年 1 月（作者提供的图片）

这个查询将返回总计 78 行；1 月份 12 行，2 月份 11 行，3 月份 10 行，依此类推。以下是帮助你更好理解结果的视觉提示：

![](img/6c653d89da412ac454729b535de49cd5.png)

查询结果的示意图（作者提供的图片）

现在让我们拆解查询结果中的每一个字段：

+   `signup_date`：客户注册的日期。

+   `cancellation_date`：客户取消的日期。

+   `cohort_month`：`signup_date` 和 `cancellation_date` 之间的月份差。

+   `max_subscriptions`：该月注册的客户总数。

+   `sum_cancellations`：每月取消订阅的客户数。

+   `r_sum_cancellations`：随时间推移取消订阅的成员的累计总和。我们将在构建可视化时用到这个字段。

例如，看一下第 5 行，我们看到，在 67 个 1 月份注册的客户中，有 2 个在 5 月取消了订阅，即在加入服务后四个月，共有 10 个客户在 1 月到 5 月期间取消订阅。

## 第 3 步 — 在 Looker Studio 中整合所有内容

现在我们的数据集准备好了，让我们在 Looker Studio 中使用它来可视化 cohorts。

首先，创建一个新的计算字段，名为 `churn_rate`，使用以下公式：

```py
SUM(r_sum_cancellations)/MAX(max_subscriptions)
```

然后，创建一个新的 **Pivot Table** 图表，按照以下标准：

+   **行维度**：`signup_date`

+   **列维度**：`cohort_month`

+   **指标**：`churn_rate` 以百分比表示

+   **排序行 #1**：`signup_date` **升序**

+   **排序列 #1**：`cohort_month` **升序**

为了为仪表板添加更多背景信息，我们将把一个包含总订阅数的表格添加到 cohort 图表的左侧。为此，创建一个新的 **表格** 图表，满足以下条件：

+   **维度**：`signup_date`

+   **指标**：`max_subscriptions` 使用 **最大值** 聚合

+   **排序**：`signup_date` **升序**

添加一点格式设置，就完成了！

![](img/9bf84527d6195b4acfdf34bbb8585e1d.png)

[Medium — Cohort Analysis Looker Studio](https://lookerstudio.google.com/reporting/ddcbc823-199a-4317-8898-9ea09a0d9cf3)（作者提供的图片）

通过这种方式查看流失率，我们可以迅速得出关于用户参与度的结论。例如，2023 年 4 月的 cohort 表现优于所有其他 cohort。换句话说，流失率最低的组表明在 4 月加入的客户对产品更有承诺和参与感。通过分析这个 cohort 成功的原因，我们可以利用这些见解来提高未来的客户留存率和忠诚度。

## 结论

Cohort 分析对任何愿意监控客户行为和流失的订阅型业务至关重要。它提供了宝贵的见解，用于制定明智的营销和留存策略，从而带来更高的收入和客户满意度。按照本文中的步骤，你已准备好实施 cohort 分析并开始获得其好处。分析愉快！

# 资源

[1] Google Sheets，[medium_cohorts_dataset_public](https://docs.google.com/spreadsheets/d/1TcjHMBoaZZsrlY17rOMIGtGIpEr9wXcX_-dQwv6Rxgw/edit?usp=sharing) (2023)，Damien Azzopardi

[2] Looker Studio，[Medium — Cohort Analysis Looker Studio](https://lookerstudio.google.com/reporting/ddcbc823-199a-4317-8898-9ea09a0d9cf3) (2023)，Damien Azzopardi

[](https://medium.com/@damien.azzopardi/membership?source=post_page-----1cd18c0edd79--------------------------------) [## 使用我的推荐链接加入 Medium。

### 你喜欢这篇文章吗？立即成为会员，发现关于任何话题的故事、思考和专业知识。

medium.com](https://medium.com/@damien.azzopardi/membership?source=post_page-----1cd18c0edd79--------------------------------)
