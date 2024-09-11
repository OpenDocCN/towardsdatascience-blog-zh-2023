# 使用SQL构建客户旅程

> 原文：[https://towardsdatascience.com/build-customer-journeys-using-sql-10127eb2fbbe?source=collection_archive---------10-----------------------#2023-03-08](https://towardsdatascience.com/build-customer-journeys-using-sql-10127eb2fbbe?source=collection_archive---------10-----------------------#2023-03-08)

## 教程

## 学习如何跟踪跨多个渠道的消费者

[](https://medium.com/@premierservices_python?source=post_page-----10127eb2fbbe--------------------------------)[![Boris J](../Images/a154d20bd61d8c852ab9313cf0b773be.png)](https://medium.com/@premierservices_python?source=post_page-----10127eb2fbbe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----10127eb2fbbe--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----10127eb2fbbe--------------------------------) [Boris J](https://medium.com/@premierservices_python?source=post_page-----10127eb2fbbe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F47c418fbf3b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-customer-journeys-using-sql-10127eb2fbbe&user=Boris+J&userId=47c418fbf3b0&source=post_page-47c418fbf3b0----10127eb2fbbe---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----10127eb2fbbe--------------------------------) · 5 min read · 2023年3月8日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F10127eb2fbbe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuild-customer-journeys-using-sql-10127eb2fbbe&source=-----10127eb2fbbe---------------------bookmark_footer-----------)![](../Images/5c683b56816a0f5d9df4905e4c9bd066.png)

图片来源：Krivec Ales，[Pixabay](https://pixabay.com/photos/hiker-standing-woman-top-journey-918473/)

## 客户旅程市场

全球客户旅程分析（CJA）市场在2020年的价值达到了**83亿美元**，预计到2026年将增长到**251亿美元**，预测期内年均增长率为**20.3%**。参与这一市场的代价不菲，并不是每个公司都能拿出一大笔资金。尽管关于CJA的信息很多，但关于如何构建数据集以获得客户旅程洞察的资料却***几乎没有***。在本教程中，我将展示如何使用结构化查询语言（SQL）创建客户旅程数据集。

## 什么是客户旅程？

我们可以将客户旅程定义为客户在多个渠道之间的**一系列互动**。这些渠道可能包括**电子邮件**、网站、移动应用、**呼叫中心**、社交媒体或店内购买。一个旅程可能展示客户与电子邮件互动，接着是**网络**，然后是直接邮件，最后是购买。这些**时间戳互动**使我们能够理解客户与公司产品的互动时间和顺序。旅程中的每一步都可以提供有价值的见解，改善客户体验、转换率及后续的营销工作。

## **客户旅程分析：使用 SQL 构建**

旅程分析使 CX 团队能够可视化客户在不同渠道和时间上的行为，定义显示转换可能性的旅程信号，并监控终端旅程成功的表现。

**我们的数据**包括一个**虚构公司**的互动和订单，该公司销售汽车。`public.interactions` 表中的互动（即网页、移动端等）是客户在购买之前与我们公司互动的渠道。我们在 `public.orders` 表中捕捉购买数据。

构建客户旅程数据集需要三个步骤。**步骤 1：主要代码**根据互动日期对客户互动进行排序。**步骤 2：主要代码 CTE**创建主表的临时表，并使其可用于步骤 3。**步骤 3：构建客户旅程**如其名所示，创建客户旅程数据集。因此，让我们分析下面 SQL 代码中的每一步，以了解它们如何工作。

作者，SQL 代码

## **主要代码（步骤1）**

我们通过客户 ID 将互动表和订单表连接起来，`public.interactions = public.orders on t1.customer_id = t2.customer_id`。我们在订单表中捕捉**完成的购买**。因此，结果旅程将**仅包含**已购买的客户的互动。

为了捕捉和排序互动，我们使用[**Lead()**](https://www.geeksforgeeks.org/postgresql-lead-function/)函数。Lead 函数基于偏移值访问下一行的数据。Lead 函数的语法如下：

```py
LEAD(return_value [,offset[, default ]]) OVER (
    PARTITION BY expr1, expr2,...
 ORDER BY expr1 [ASC | DESC], expr2,...
)
```

在下表中，**第一次互动**由函数 `lead(t1.interaction,**0**) over (**partition by** t1.customer_id **order by** t1.interaction_date asc)` 决定。首先，`partition by` 基于 `t1.customer_id` 对客户记录进行分组。其次，`order by t1.interaction_date asc` 按日期对互动进行排序。最后，offset 访问下一行，或者当前行之后的第二行，或者当前行之后的第三行，依此类推。在这里，offset 为零，`lead(t1.interaction,**0**)` 返回值为 `Mobile`。如果偏移量为 1，`lead(t1.interaction,**1**)` 返回值为 `Web`。

![](../Images/6157499a61ca5b5a90d808902b87b824.png)

我建议将数据导入数据库，并应用上面的SQL代码以充分理解信息。你可以在[**这里**](https://github.com/bensondavies/customer_journey)找到数据集。

## **将主代码封装在CTE中（第2步）**

我们通过将**主代码**添加到公共表表达式（CTE）中来处理它。CTE的语法是：

```py
with ctedata as
(
   --Step 1 code
)
```

我们使用CTE的**主要原因**是为了能够使用`where`子句，并通过**Lead()**或[**Row_Number()**](https://www.geeksforgeeks.org/postgresql-row_number-function/)函数派生的列来过滤行。在主代码中使用函数创建的列不能在`where`子句中使用。这为第3步准备了数据。

## 构建客户旅程数据集（第3步）

在第1步创建的数据集中，一些行缺少数据。我们通过过滤`where offset = 0`来移除缺失的行。我们还通过对首次互动和第二次互动的总购买客户进行汇总来聚合数据，如下所示。

![](../Images/ec616f97e152420c4e47f68a2c8e4245.png)

作者，数据集

**上表**展示了客户在购买前的互动情况。在这个例子中，购买**复古汽车**的客户通过**移动**和**电子邮件**渠道进行互动。通过**客户旅程**数据，我们可以洞察**哪些渠道表现**最好，能促进互动并带来销售；哪些客户路径会导致流失；什么时机是与消费者互动的最佳时机，以及不同受众采取的路径[2]。

这些知识可能有助于制定营销策略。虽然表格可以有效地解释数据，但可视化数据可以帮助描绘直观的客户旅程。因此，让我们在下一部分尝试一下。

## 可视化客户旅程

下图中的**桑基图**对于说明跨多个渠道（如电子邮件、网站、移动应用、呼叫中心、社交媒体）的客户旅程顺序非常有用。

![](../Images/7fd1d8c0a31295d9f625585c4df5452c.png)

作者，桑基图数据

我们从左到右阅读**桑基图**。每个垂直条代表一个包含消费者的**节点**。例如，蓝色节点显示23.24%的消费者首先通过直接邮寄与品牌互动。这些客户接着通过移动、直接邮寄、网页和电子邮件的组合进行互动。带状图的厚度直观地表示了在互动之间流动的消费者数量。带状图越厚，表示流向第二个互动节点的消费者越多。如果你想创建桑基图，我推荐尝试**Chart Expo**或**Visual Paradigm**这两个付费服务。我偏爱Chart Expo，因为它提供了在线教程视频。如果你决定使用它，记得按照提供的SQL代码格式导出数据。

## **自定义旅程：继续学习**

学习如何跟踪客户在多个渠道中的行为有很多内容需要探索。为此，我在本教程中包含了一个**视频**。视频覆盖了相同的内容，但通过插图讨论示例可能会增强学习效果。如果你发现理解某些概念有困难，我鼓励你观看视频。

作者，视频

好的，目前就这些了。希望你觉得这个教程对你有帮助。请随时联系我提问。我在这里分享和成长。

**参考文献**：

[1]: Markets and Markets，[客户旅程分析市场按组件组织和规模](https://www.marketsandmarkets.com/Market-Reports/customer-journey-analytics-market-119398916.html)

[2]: [Karolina Matuszewska](https://piwik.pro/blog/author/karolina-matuszewska/)，[Marek Juszczyński](https://piwik.pro/blog/author/marek-juszczynski/)。 (2022年10月3日)。[什么是客户旅程分析以及它为何对你的业务重要](https://piwik.pro/blog/customer-journey-analytics-customer-experience-optimization/)
