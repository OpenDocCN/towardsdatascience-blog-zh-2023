# 数据工程师的数据建模

> 原文：[`towardsdatascience.com/data-modelling-for-data-engineers-93d058efa302`](https://towardsdatascience.com/data-modelling-for-data-engineers-93d058efa302)

## 初学者的终极指南

[](https://mshakhomirov.medium.com/?source=post_page-----93d058efa302--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----93d058efa302--------------------------------)[](https://towardsdatascience.com/?source=post_page-----93d058efa302--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----93d058efa302--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----93d058efa302--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----93d058efa302--------------------------------) ·阅读时间 9 分钟·2023 年 12 月 3 日

--

![](img/38909e6454cd7c166ad2897c1828b42d.png)

图片来源：[Sebastian Svenson](https://unsplash.com/@sebastiansvenson?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据建模是数据工程的重要组成部分。在这个故事中，我想讨论不同的数据模型、SQL 在数据转换中的作用以及数据增强过程。SQL 是一个强大的工具，帮助操作数据。通过数据转换管道，我们可以转换和增强加载到数据平台中的数据。我们将讨论各种数据操作方法、调度和增量表更新。为了使这个过程高效，我们需要首先了解一些关于数据建模的基本知识。

## 什么是数据建模？

**数据模型**旨在组织数据元素并标准化数据元素之间的关系。

**数据模型**确保数据的质量、语义配置和命名一致性。它有助于从***概念上***设计数据库，并在数据元素之间建立逻辑连接，例如主键和外键、表格等。

如果你需要最可靠和具有成本效益的数据转换，良好而全面的**数据模型设计**是至关重要的。它保证数据处理没有延迟和不必要的步骤。

公司使用一种称为**维度数据建模**的程序来处理数据。**来源** — **生产** — **分析**级别在模式（数据集）之间的划分实现了有效的数据治理，并确保我们的数据为业务智能和机器学习做好准备。

任何可衡量的信息都存储在**事实表**中，即***事务、会话、请求等***。

**外键** 用于事实表中，并且它们与维度表相连接。**维度表** 拥有与事实表相关的描述性数据，例如 ***品牌、产品类型/代码、国家等。***

根据业务需求，**维度和事实** 被整合到 **模式** 中。

最受欢迎的两种模式类型是 **星型模式和雪花模式**。并且这些也是数据工程师面试中最常见的问题之一 [1]。

[](/data-engineering-interview-questions-fdef62e46505?source=post_page-----93d058efa302--------------------------------) [## 数据工程面试问题]

### 准备工作面试的提示

towardsdatascience.com

+   **星型模式**：星型模式是一个具有**高度非规范化**结构的模式。这在现代数据仓库中非常常见，数据会在多个表中重复。每个维度代表一个维度表。维度表具有与事实表连接的外键，以获取结果。数据冗余较高，但由于模式设计的简单性，它可以提供性能上的好处。它更为非规范化；这意味着表之间的连接较少，因此它在查询时表现更优。

![](img/9b14f90ff13ea78dd0d641f374a68a32.png)

星型模式示例。作者提供的图片。

+   **雪花模式**：雪花模式扩展了星型模式，具有更多规范化的维度表。此模式使用较少的磁盘空间，因为所有表都经过规范化，并拆分成进一步的维度表。添加维度到此模式中很容易，而且由于复杂的模式设计，数据冗余也较少。

![](img/8adb908d8436beaba878067695fffce1.png)

雪花模式示例。作者提供的图片。

+   **星系模式**：类似于上述模式，星系模式具有来自 **多个维度模型** 的多个事实表。

## 关系型、维度型和多维数据建模

纯粹的 **关系型数据建模** 通常出现在 OLTP 数据管道和向应用程序提供数据的关系（事务）数据库中。在这种情况下，数据事务速度和大小至关重要。在这种情况下，我们希望应用高度规范化的数据模式设计。数据总是最新的，但由于在各种维度表上的连接次数较多，运行报告可能成为一个具有挑战性的任务。

**维度建模** 作为一个过程，旨在确保最有效的数据存储结构以及更快的数据转换，以处理大量数据。我们希望这样做的原因很简单——提供一个更高效的数据平台，便于管理和数据治理。例如，雪花模式有助于减少数据冗余，这与星型模式相反，更适合用于 OLAP 分析和报告（无需执行昂贵的连接操作）。

**多维数据模型**以**立方体**形式表示数据。

![](img/238f2155f5f94f08b9e98346270ad4b9.png)

三维数据立方体中的数据点（值）。作者提供的图片。

它增加了复杂性，但也使数据结构对复杂报告更加高效。数据存储在聚合立方体（表）中，这为用户提供了更多执行强大 OLAP 数据转换的能力，即透视、钻取功能和同时处理多个维度的高级仪表板。通常，不需要对数据进行聚合和保留。我们可以使用 SQL 和 ROLLUP 将数据模型定义为**立方体**[2]。

ROLLUP 函数用于在多个级别上执行聚合。这在处理维度图时非常有用。

![](img/866e98ef7b2cc52b52fefcdbf67e3bd9.png)

作者提供的图片

请参考下面的 SQL 代码。它提供了输入数据和所需的 SQL 转换，以创建一个多维数据模型。

```py
with data as (
    select
    current_timestamp() as ts           
    ,'stage'            as context_type 
    ,1                  as user_id      
    ,100                as credit_value 
    , true              as is_gift
union all
    select
    timestamp_sub(current_timestamp(), interval 24 hour) as ts           
    ,'user'             as context_type 
    ,1                  as user_id      
    ,200                as credit_value 
    ,false              as is_gift
union all
    select
    timestamp_sub(current_timestamp(), interval 24*2 hour) as ts           
    ,'user'             as context_type 
    ,3                  as user_id      
    ,300                as credit_value 
    ,true               as is_gift

)
, results as (
select 
     date(ts) as date 
    ,context_type
    ,sum(credit_value)/100 as daily_credits_spend
from data    

group by rollup(1, context_type)
order by 1
)

select
  date
  ,if(context_type is null, 'total', context_type) as context_type
  ,daily_credits_spend
from results
order by date
; 
```

我们创建的任何数据集作为自定义 SQL 查询都可以被视为维度模型。

[高级 SQL 技术入门](https://towardsdatascience.com/advanced-sql-techniques-for-beginners-211851a28488?source=post_page-----93d058efa302--------------------------------)

### 在 1 到 10 的评分中，你的数据仓库技能有多好？

[高级 SQL 技术入门](https://towardsdatascience.com/advanced-sql-techniques-for-beginners-211851a28488?source=post_page-----93d058efa302--------------------------------)

维度和多维模型在数据仓库解决方案中非常常见。维度数据模型使模式对业务用户更加简单和清晰。

## 如何创建一个维度模型？

为了将维度模型元素引入我们的数据平台设计中，我们创建维度模型。它可以是一个模式、一个数据库，甚至是数据湖中我们数据平台的一部分，能够描述由许多事实表定义的各种业务流程。让我们考虑一个包含两个事实表的单一业务流程。

首先，我们需要**识别一个业务流程**，并将其转换到我们的数据模型中，定义每个事实表的**粒度**。让我们假设这是一个应用内购买，支付交易的事实表行数据如下：

```py
create table if not exists production.payment_transaction_raw (
  src string
)
partition by date(_partitiontime)
;
insert production.payment_transaction_raw (src)
select
'{"transaction_id":1,"user_id":1,"reputation":99,"total_cost:9.99,"product_id":1001,"product_type":"bot", "currency":"USD","created_at":"2021-01-01 01:00:01 UTC"}' union all select
'{"transaction_id":2,"user_id":2,"reputation":99,"total_cost:4.99,"product_id":2001,"product_type":"charm", "currency":"USD","created_at":"2022-01-01 01:00:01 UTC"}' union all select
'{"transaction_id":3,"user_id":1,"reputation":99,"total_cost:6.99,"product_id":3001,"product_type":"bot", "currency":"USD","created_at":"2023-01-01 01:00:01 UTC"}' union all select
;
```

**粒度**指的是我们表中存储的信息级别。例如，在我们的例子中，应用内支付按每个交易记录；因此，粒度是**交易级别**，可以很容易地转换为**每日**。重要的是，我们的维度模型中的事实表必须与指定的粒度一致。这意味着我们希望将原始数据加载到相同的维度结构中。

通常，我们希望**转换和增量更新**数据，如下所示：

```py
create table if not exists production.payment_transaction (
  transaction_id   int64   
 ,user_id          int64   
 ,reputation       int64   
 ,total_cost       float64 
 ,product_id       int64
 ,product_type     string
 ,currency         string  
 ,created_at       timestamp
)
partition by date(created_at)
;
insert production.payment_transaction (
  transaction_id  
 ,user_id         
 ,reputation      
 ,total_cost      
 ,product_id      
 ,product_type
 ,currency        
 ,created_at      
)

-- insert new batch and avoid duplicates:
select
  raw.transaction_id  
 ,raw.user_id         
 ,raw.reputation         
 ,raw.total_cost      
 ,raw.product_id          
 ,raw.product_type        
 ,raw.currency        
 ,raw.created_at      
from (
   select
     cast(json_extract(src,'$.transaction_id') as int64)              as transaction_id  
    ,cast(json_extract(src,'$.user_id') as int64)                     as user_id         
    ,cast(json_extract(src,'$.reputation') as int64)                  as reputation         
    ,cast(json_extract(src,'$.total_cost') as float64)                as total_cost      
    ,cast(json_extract(src,'$.product_id') as int64)                  as product_id      
    ,cast(json_extract(src,'$.product_type') as string)               as product_type      
    ,cast(json_extract(src,'$.currency') as string)                   as currency        
    ,cast(json_extract_scalar(src, '$.created_at') as timestamp)      as created_at      
   from
    production.payment_transaction_raw
) raw
left join production.payment_transaction self on
   raw.transaction_id = self.transaction_id
where
   self.transaction_id is null -- insert only new records that don't exit.
;
```

接下来，我们需要为每个事实表定义**维度**。在我们的案例中，**支付交易**作为事实。这些**事实**可以包含有用的数据，例如销售的总数量、货币兑换率、总收入、税收等。进一步的**维度**将为我们的应用内购买数据提供背景，即**时间、产品和用户**维度数据。每个维度都可以帮助回答重要的业务问题。*例如，我们可能想知道哪个是最畅销的产品，或者用户的平均花费是多少。*

每个应用内支付交易将拥有描述事实与维度关系的外键，并指向相关的产品、购买时间或用户资料。

接下来，我们希望定义**层级**。层级有助于业务用户理解数据集的不同**粒度**级别。它还帮助导航和高级报告功能，即钻取。例如，产品维度可能具有一个**层级**，从`类别`到`类型`再到`ID`和`子 ID`。

我们希望在模型中使用一些称为**度量**的计算。度量是我们使用事实表计算以生成数据洞察的内容。可以是总`收入`、`净收入`、`税额`等。

使用这个多维模型，企业可以回答一些关键问题：

+   去年或上个月我们每个产品的总收入是多少？

+   具有何种声誉级别的用户花费更多？

+   哪个产品是最畅销的？

+   今年的收入如何与去年同期相比？

最后，我们希望通过用户验证我们的数据模型。

## 维度建模的好处

**灵活性。** 一个维度建模框架扩展了数据仓库的过程。该设计高度适应以适应新的业务需求或更改中央仓库。为了表示更新后的业务流程，可以向模型中添加新实体或调整现有实体的布局。维度模型可以轻松处理这些变化。可以向维度表中添加更多列，而不会干扰使用原始表的当前商业智能应用程序。

**多维分析：** 正如前面的示例所示，我们可以使用额外的维度来创建更好的图表和更复杂的图形。层级结构有助于理解数据，并同时进行分析，报告仪表板中还增加了钻取功能。它针对商业智能（BI）需求进行了优化，并且易于理解。现代 BI 工具内置了原生设计的 AI 功能，以理解维度结构和数据[3]。

[](/artificial-intelligence-in-analytics-f11d2deafdf0?source=post_page-----93d058efa302--------------------------------) ## 分析中的人工智能

### AI 驱动的商业智能。是炒作还是现实？

towardsdatascience.com

**改进的查询速度：** 多维表通常是非规范化的，并带来星型架构设计的所有性能优势。与雪花架构相对，它通过减少`join`操作实现**更快的数据检索**。

## 维度数据建模的局限性

尽管维度建模是 OLAP 用途的强大技术，但它确实有一定的限制，在某些情况下，可能不是最佳选择。

**数据经常变化：** 由于维度数据集通常是非规范化的，这里的数据冗余较高。非规范化的数据集可能在实时修改的需求下表现不佳。OLTP 方法可能更适合这种情况。

**非结构化数据：** 维度数据建模在**稀疏和非结构化数据**上表现不佳。对于有限的事实数据，这些数据可以与维度关联，可能会低效，例如文档和文本数据。在这种情况下，面向文档的数据库解决方案可能是更好的选择。

**存储：** 非规范化的数据集需要比 OLTP 模式更多的存储空间。

## 结论

由于其提供的好处，维度建模仍然是开发业务数据仓库时最常用的数据建模方法。它通过多维分析提供更好的报告能力，通过减少联接优化查询性能并加快数据检索。维度建模使数据仓库设计过程更灵活，更具适应性。它也有一定的局限性，使其与非结构化数据集和 OLTP 管道不兼容，因为后者对数据冗余的要求较高。总体而言，多维建模是一个出色的 OLAP 和报告技术，能够提供更好的查询性能。

## 推荐阅读

[1] [`medium.com/towards-data-science/data-engineering-interview-questions-fdef62e46505`](https://medium.com/towards-data-science/data-engineering-interview-questions-fdef62e46505)

[2] [`medium.com/towards-data-science/advanced-sql-techniques-for-beginners-211851a28488`](https://medium.com/towards-data-science/advanced-sql-techniques-for-beginners-211851a28488)

[3] [`medium.com/towards-data-science/artificial-intelligence-in-analytics-f11d2deafdf0`](https://medium.com/towards-data-science/artificial-intelligence-in-analytics-f11d2deafdf0)

[4] `towardsdatascience.com/modern-data-engineering-e202776fb9a9`

[5] `towardsdatascience.com/how-to-become-a-data-engineer-c0319cb226c2`
