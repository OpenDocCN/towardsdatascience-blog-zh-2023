# **5 个迹象显示你的数据建模不佳**

> 原文：[https://towardsdatascience.com/5-signs-that-your-data-is-modeled-poorly-a646e8d33be0?source=collection_archive---------7-----------------------#2023-06-20](https://towardsdatascience.com/5-signs-that-your-data-is-modeled-poorly-a646e8d33be0?source=collection_archive---------7-----------------------#2023-06-20)

## 云时代的常见挑战

[](https://medium.com/@mattgazzano?source=post_page-----a646e8d33be0--------------------------------)[![Matthew Gazzano](../Images/23f154b154d05847c2c13ea17ceb7a57.png)](https://medium.com/@mattgazzano?source=post_page-----a646e8d33be0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a646e8d33be0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a646e8d33be0--------------------------------) [Matthew Gazzano](https://medium.com/@mattgazzano?source=post_page-----a646e8d33be0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F626000912ce9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-signs-that-your-data-is-modeled-poorly-a646e8d33be0&user=Matthew+Gazzano&userId=626000912ce9&source=post_page-626000912ce9----a646e8d33be0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a646e8d33be0--------------------------------) · 7分钟阅读 · 2023年6月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa646e8d33be0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-signs-that-your-data-is-modeled-poorly-a646e8d33be0&user=Matthew+Gazzano&userId=626000912ce9&source=-----a646e8d33be0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa646e8d33be0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-signs-that-your-data-is-modeled-poorly-a646e8d33be0&source=-----a646e8d33be0---------------------bookmark_footer-----------)![](../Images/6ab63520d19e990128b66de3607dc34e.png)

[照片](https://unsplash.com/photos/lRoX0shwjUQ) 由 [Jan Antonin Kolar](https://unsplash.com/@jankolar) 在 [Unsplash](https://unsplash.com/)

随着过去十年云技术的扩展和廉价存储成本的降低，许多组织积累的数据量比以前想象的要大得多。许多云数据仓库提供商提供的按需付费模式（[AWS](https://aws.amazon.com/)，[GCP](https://cloud.google.com/gcp?utm_source=google&utm_medium=cpc&utm_campaign=na-US-all-en-dr-bkws-all-all-trial-e-dr-1605212&utm_content=text-ad-none-any-DEV_c-CRE_491349594127-ADGP_Desk+%7C+BKWS+-+EXA+%7C+Txt+_+Google+Cloud+Platform+Core-KWID_43700064423315751-aud-1436107373682%3Akwd-26415313501&utm_term=KW_google+cloud+platform-ST_google+cloud+platform&gclid=CjwKCAjw-b-kBhB-EiwA4fvKrCt5QBXsaaB2cBQvmRsEyXEsswdmkhHXOP3PQtEGbPxx07cD1S5cABoCFJcQAvD_BwE&gclsrc=aw.ds&hl=en)，[Azure](https://azure.microsoft.com/en-us))减少了前期资本资源的需求和数字基础设施的考虑。

好消息是，这最终使得数据科学工作对大多数组织来说更加可及。

坏消息是数据湖正变得越来越像数据沼泽。

## 什么是数据建模？它面临哪些挑战？

对数据工程师来说，向高层管理人员传达一个良好建模生态系统的价值往往是困难的。这是因为利益相关者能看到的只是展示出的BI工具和预测模型。然而，建模不良的数据从数据治理的角度来看会给分析团队带来重大挫折。这不可避免地会拖慢工作流程，引入重复任务，降低报告准确性，以及产生许多其他负面副作用。

定义“良好建模”数据是一个独立的话题。但你可以通过以下概念在你的数据仓库中考虑它：

+   存在一个清晰的模式来查找与业务实体相关的数据表。

+   使用了一个有意的/已知的建模技术，例如维度模型、实体关系模型、数据保险库等。

+   表格和字段命名约定是一致的，文档化良好，并且具有业务价值。

还应注意到，数据建模采用的是一种整体性和多系统的方法。它从你的OLTP（在线事务处理）系统开始，这是数据最初被记录的地方。以下是一些例子：

+   CRM系统如[Salesforce](https://www.salesforce.com/)

+   销售点系统如[Stripe](https://stripe.com/)

+   电子商务平台如[亚马逊](https://www.amazon.com/)

理想情况下，你的数据在通过源系统收集时应该被规范化到[第三范式](https://www.geeksforgeeks.org/third-normal-form-3nf/)。然后，它应该被摄取到分析环境中，也就是一个OLAP（在线分析处理）系统，在这里应用了分析建模技术。在本文的背景下，OLAP系统与云数据仓库同义。但OLAP系统也可以包括像SQL Server、MySQL或PostgreSQL这样的独立托管工具。

尽管数据分析师和数据科学家只与 OLAP 系统互动，但组织的数据建模策略需要考虑 OLTP 和 OLAP，才能保持可持续性。

# 以下是说明您的分析环境建模不佳的5个关键迹象。

## 1.) 需要部落知识来理解数据的位置

为了使新分析师在入职时取得成功，他们需要一个明确的路线图，了解数据仓库中有哪些数据，数据来源于哪里，以及其业务背景是什么。然而，数据建模不佳的团队往往难以使新员工顺利上手，因为不理解新员工回答基本业务问题为何需要如此长时间。如果没有适当的指导，分析团队可能会经历高离职率，因为新成员没有获得成功所需的工具。

数据分析师和数据科学家应该专注于解决业务问题，而不是浪费时间寻找业务实体的位置。团队越快熟悉可用数据，仪表板和预测模型的完成速度就越快。这*最终*会提升团队的生产力。

如果只有少数分析师知道如何回答基本的业务问题，那就是一个问题。采用这种孤立的方式无法扩展，并且只会限制团队能够解决的问题数量。

![](../Images/ba6eb54e39b719cb755fb9048b9fbe69.png)

[照片](https://unsplash.com/photos/IgUR1iX0mqM) by [Desola Lanre-Ologun](https://unsplash.com/@disruptxn) on [Unsplash](https://unsplash.com/)

## 2.) 不同分析师为相同指标产生不同结果

如果没有单一的真实来源，不同团队成员很容易以不同的方式计算相同的指标。例如，收入是如何定义的？计算这一指标使用了哪些表？需要有一个明确的路径来定义业务逻辑，这一切都始于一个有意图的数据模型。

我曾在这样的环境中工作，那里有3个不同的表表示相同的业务实体，它们使用了不同的 SQL 逻辑来得出相似但不完全相同的记录输出。再加上管理不善的报告请求队列，就会导致两个不同的分析师用不同的结果回答相同的问题。这不仅导致利益相关者对数据失去信任，还需要跨团队进行繁琐且不必要的调解工作。

## 3.) 团队需要重用冗余的业务逻辑代码块

我见过团队使用 Google 表格记录 SQL CASE 语句，以概述具体的业务指标。这些语句往往很长且难以阅读。虽然这种做法试图在团队之间提供一致性，但问题在于它违反了组织内的 DRY（不要重复自己）原则。

对于许多面临此类问题的团队，使用像DBT这样的转换工具可以让分析工程师在一个地方定义业务逻辑，然后让分析师在多个地方引用它。

想象以下例子——如果你是一个电子商务公司，且有一种复杂的方式来计算页面浏览量（这是可以的），那么为什么要在你的BI工具中分发和重复这个业务逻辑呢？不仅在逻辑没有被逐字复制和粘贴的情况下风险较大，而且还浪费计算资源，而这通常是大多数云服务提供商的最大开支。

为了解决这个问题，考虑绘制出需要进行的常见聚合和业务逻辑，运行一个每天（或根据需要的频率）进行的转换作业，并将其写入一个表中。然后让你的BI层基于这个表运行。

## 4.) 你的数据仓库表现不佳

如上所述，建模不良的数据会引入冗余。但它也会产生不必要的复杂性。过多的计算资源是这的副产品，并且所有云数据仓库都有一定的定价阈值限制。一旦达到这个限制，执行新的查询可能会变得极其缓慢，甚至在某些情况下不可行。

任何数据工程师都会告诉你，单纯购买额外资源并不是解决这个问题的可持续方案。

长且复杂的查询不仅执行时间长，还会消耗环境中的可用资源。考虑一个需要执行涉及20个连接的查询的例子。在这种情况下，理想的解决方案非常少，因为它表明用于解决业务问题的数据没有以易于访问的格式存储。这么多的连接可能会计算开销很大，尤其是当相关表的数据量很大或ON子句涉及多个列时。如果你正在实施一个维度模型，你的团队可能需要考虑在这些场景下在数据库中创建一个新的事实表。

资源的衡量方式因所使用的云服务提供商而异，但它们都遵循使用固定数量虚拟CPU的相同概念。例如，BigQuery使用[槽位](https://cloud.google.com/bigquery/docs/slots)的概念，槽位实际上是执行查询时可用的计算资源数量。采用按需定价的组织在任何给定时刻可使用2000个槽位。因此，如果一个查询非常复杂并且占用的槽位超过了可用数量，其他查询将会排队，直到它们能够被执行。

## 5.) 你经常需要在SQL中硬编码值

硬编码的值通常是数据仓库中缺失所需数据的明显迹象。在维度模型的上下文中，这通常意味着需要创建一个新的维度表来提供额外的列。

[扎克·奎因](https://zachl-quinn.medium.com/) 写了一篇 [文章](https://medium.com/learning-sql/sql-users-reduce-your-case-statements-with-a-lookup-table-d09da47bf65)，很好地阐述了这个概念，演示了如何通过查找表消除冗长的CASE语句。将这个示例放在维度模型的背景下——假设你的组织需要进行大量的地理空间分析。你有一个*customer_dimension*表，它提供了州的缩写，但你想将其显示为完整的州名。你可以写这样的代码：

```py
SELECT
customer_id
, customer_name
, address
, city
, state AS state_abrevaition
, CASE
    WHEN state = 'NJ' THEN 'New Jersey'
    WHEN state = 'NY' THEN 'New York'
    WHEN state = 'PA' THEN 'Pennsylvania'
    ..............
  END AS state_full_name
, zip_code
FROM customer_dimension
```

但这种CASE语句并不可持续。如果我们想更详细地改进这个解决方案，我们需要将*zip_code_dimension*表连接到*customer_dimension*表。你会看到下面的*zip_code_dimension*表将为我们的分析提供更大的粒度。这个表可能看起来像这样：

![](../Images/80b22cf424ec049028ea0f961df9694a.png)

作者拍摄

所以有了那个新表，想象一下能够运行*这个*查询：

```py
SELECT
c.customer_id
, c.customer_name
, c.address
, c.state AS state_abreviation
, z.state_name
, c.zip_code
, z.county_name
, z.country_name
, z.timezone
, z.lat
, z.lng
FROM customer_dimension c
LEFT JOIN zip_code_dimension z
  ON c.zip_code = z.zip_code
```

这不仅是一种更优雅和可读的查询方式来生成完整的州名，而且让我们能够回答更多的问题。通过*zip_code_dimension*，我们现在可以将该邮政编码的纬度和经度附加到地图可视化中，以更清晰的格式呈现。此外，还有一些其他维度字段，如果我们想在输出中包含它们（如国家、时区等），则需要数百行硬编码。

# 结论

如果你发现上述任何要点与你的环境相关，那么现在可能是时候全面审视你的数据管道，并了解分析团队在哪些方面需要填补空白。为了能够正确建模你的团队数据，你需要能够概念化相关的业务实体，并将其组织成有利于组织内常见问题的方式。没有一种通用的方法，但它需要对所有团队成员明确，并以一致的方式创建。
