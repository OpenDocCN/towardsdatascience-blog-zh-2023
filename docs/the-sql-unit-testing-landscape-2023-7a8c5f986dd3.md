# SQL 单元测试现状：2023

> 原文：[`towardsdatascience.com/the-sql-unit-testing-landscape-2023-7a8c5f986dd3`](https://towardsdatascience.com/the-sql-unit-testing-landscape-2023-7a8c5f986dd3)

## 提升 SQL 开发的速度和安全性

[](https://medium.com/@cisenbe?source=post_page-----7a8c5f986dd3--------------------------------)![Chad Isenberg](https://medium.com/@cisenbe?source=post_page-----7a8c5f986dd3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a8c5f986dd3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a8c5f986dd3--------------------------------) [Chad Isenberg](https://medium.com/@cisenbe?source=post_page-----7a8c5f986dd3--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a8c5f986dd3--------------------------------) ·阅读时间 9 分钟·2023 年 5 月 2 日

--

![](img/7df8aad2d9bf6732036f59b3df2c4a19.png)

图片由[Ilse Orsel](https://unsplash.com/@lgtts?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# SQL 开发的演变

尽管 SQL 已经有将近[50 年](https://en.wikipedia.org/wiki/SQL)的历史（或者说*正因为*它已经将近 50 年了），但该语言的发展在采用现代实践和工具方面一直较为缓慢。几十年来，我们主要的“IDE”是像[SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)和[DBeaver](https://dbeaver.io/)这样的管理工具，版本控制则通过像*stored_proc_2022_03_15*这样的命名约定来完成，测试则是通过将查询运行在生产表上并导出到电子表格中进行手动核对。

但在过去十年中，情况已经迅速改善。那些数据库管理工具以及云数据仓库控制台都具有丰富的功能，包括语法高亮、代码自动完成和计划可视化。我们通过像[Liquibase](https://docs.liquibase.com/home.html)、[Flyway](https://documentation.red-gate.com/fd/welcome-to-flyway-184127914.html)和[dbt](https://docs.getdbt.com/docs/introduction)这样的工具实现了数据库版本控制。即使是测试/审计方面也不再那么令人头疼，有了像[dbt-audit-helper](https://github.com/dbt-labs/dbt-audit-helper)这样的工具。

最后一个前沿领域之一是 SQL [单元测试](https://en.wikipedia.org/wiki/Unit_testing)，它仍然处于起步阶段。让我们深入探讨一下！

# 我们为什么要进行单元测试？

对于那些从软件工程之外进入 SQL 开发领域的人来说，单元测试的价值可能并不明显。通常，查询开发是一个相对快速的“编写查询、运行查询、查看结果集、修改查询、重复”的过程。但随着查询复杂性和数据量的增长，你会遇到一些障碍。

首先，你可能会进行大量冗余测试。考虑以下转换：

```py
select
  id,
  case when order_count < 50
  then 1
  else 0
  end as is_small_customer
from customer
```

查询本身并不令人兴奋。但从测试的角度来看，我们实际上需要测试什么，正在测试什么？我们只需要四条记录来覆盖我们的案例：

+   *order_count < 50*

+   *order_count > 50*

+   *order_count = 50*

+   *order_count IS NULL*

对于你的生产数据库，你可能需要扫描数百万或数十亿条记录进行测试，这可能会积累成本。你可以（也应该）拥有一个数据子集较小的开发环境，但这仍然可能留下比实际测试查询所需的记录更多的数据。

另一个主要问题是，即使是你的完整生产数据集也可能不包含所有合理的测试用例。特别是当你引入新的数据源，或者开发或运维团队更改生成数据的方式时，你可能会需要覆盖尚不存在于数据中的场景的测试！

最重要的收益通常是在维护阶段获得的。当你需要对现有功能进行更改时，一个稳健的单元测试套件将让你确信不会破坏你的查询。如果你的更改破坏了一个单元测试，你就知道你需要做工作：要么调整你的查询以保持所需的功能，要么修改你的测试以反映新的逻辑。由于你使用的是非常小的模拟数据集，你可以在开发过程中几乎立即获得反馈。

是的，你仍然需要进行集成测试和审计，但通过良好的测试覆盖率，你可以在此阶段之前捕捉到许多错误，从而节省时间和计算成本。

为了说明这些想法，这里有一个场景。

假设我在处理一个特别复杂的查询，这个查询在生产数据集上执行需要 30 分钟和 10 美元。在开发过程中，我必须执行这个查询 5 次，每次进行小调整，每次花费 1 美元和 2 分钟，同时我的审计查询完成（注意，如果我手动审计结果，这一步可能会更昂贵）。作为一名平均年薪为 12 万美元的[美国数据工程师](https://www.salary.com/research/salary/listing/data-engineer-salary)，我的时间成本约为 57 美元/小时（不考虑税收、福利等）。我的开发成本为 171 美元的劳动成本和 55 美元的计算成本，总计 226 美元。

实现单元测试以覆盖相同的查询可能需要我一到两个小时，因为这真的很复杂。但现在我的前四次查询基本上是免费的；由于我们处理的记录数量有限，执行只需几秒钟，而我仅需为最终验证负责：$11 和 30 分钟的时间（$28.50），总共为 $39.50。加上单元测试的开销，我们总共花费 $153.50，节省了大约 33%。

还有很多反驳的观点。如果查询本身只花费几分钱并且执行时间仅需几秒钟呢？如果我只需进行一个微不足道的更改并在一次操作中完成工作呢？如果我在一个削减了成本一半或更多的缩减开发环境中工作呢？

你的具体使用案例可能决定了你能从单元测试中获得多少价值，但我认为无论你的情况如何，你都需要考虑你的测试策略和你所做的权衡。测试会产生成本，但对开发高质量产品至关重要；在数据世界中，这一点与软件世界没有不同。

# 那我们的 pytest 等价物在哪里？

回答标题：没有一个。希望我已经让你相信，单元测试可以以非常少的数据给我们所需的覆盖范围。但当然，也有陷阱。在 SQL 世界中，什么是“单元”？对于像 Python 或 Java 这样的语言，单元测试通常在方法或类级别进行；正如名称所示，这些是需要测试的最基本的功能“单元”。

在 SQL 中，一个单元是整个查询吗？是单个 CTE 吗？还是传达某种意义的一组 CTE？后者为单元测试解决方案增加了复杂性，因为 SQL 并不像通用语言那样容易组合；你不能像调用方法那样“调用”一个 CTE。良好的数据建模可以通过使查询更具模块化来帮助减少这种负担，但良好的测试设计仍然具有挑战性，而且可用的最佳实践很少。

对于 SQL 单元测试，有许多现成的选项，但目前还没有标准。有一个很棒的 [dbt 讨论帖](https://github.com/dbt-labs/dbt-core/discussions/4455) 详细介绍了这些选项的优点和潜在方法，但不幸的是，这还没有成为路线图的一部分。让我们回顾一些更流行的选项。

# tSQLt

[tSQLt](https://tsqlt.org/) 是一个开源项目，由 SQL Server 开发领域的知名公司 [Redgate](https://www.red-gate.com/) 维护。像许多 SQL Server 解决方案一样，tSQLt 实现为一系列存储过程，作为数据库对象添加到你的目标数据库中。

从 [教程](https://tsqlt.org/user-guide/tsqlt-tutorial/) 中，这里是一个存储过程的测试，模拟作为变量处理：

```py
EXEC tSQLt.NewTestClass 'testFinancialApp';
GO

CREATE PROCEDURE testFinancialApp.[test that ConvertCurrency converts using given conversion rate]
AS
BEGIN
    DECLARE @actual MONEY;
    DECLARE @rate DECIMAL(10,4); SET @rate = 1.2;
    DECLARE @amount MONEY; SET @amount = 2.00;

    SELECT @actual = FinancialApp.ConvertCurrency(@rate, @amount);

    DECLARE @expected MONEY; SET @expected = 2.4;   --(rate * amount)
    EXEC tSQLt.AssertEquals @expected, @actual;

END;
GO
```

tSQLt 还提供了模拟表和视图的功能，以及断言各种数据库对象的相等、不等和存在性。这个工具特别适合应用数据库开发，因为这正是 SQL Server 的主要用例之一。你当然*可以*将其应用于分析（而且 SQL Server 实现了许多数据仓库），但显然关注的是不同类型的开发。

# dbt-unit-testing

[dbt-unit-testing](https://github.com/EqualExperts/dbt-unit-testing) 是一个由 [Equal Experts](https://www.equalexperts.com/) 维护的 dbt 软件包，Equal Experts 是一家全球技术咨询公司。其方法是创建利用软件包宏的自定义测试，然后运行它们。在这里，模拟数据可以用查询风格或 CSV 风格定义，一些巧妙的 Jinja 宏处理将模拟数据替换为实际的表引用。根据他们的文档：

```py
{{ config(tags=['unit-test']) }}

{% call dbt_unit_testing.test('customers', 'should sum order values to calculate customer_lifetime_value') %}

  {% call dbt_unit_testing.mock_ref ('stg_customers') %}
    select 1 as customer_id, '' as first_name, '' as last_name
  {% endcall %}

  {% call dbt_unit_testing.mock_ref ('stg_orders') %}
    select 1001 as order_id, 1 as customer_id, null as order_date
    UNION ALL
    select 1002 as order_id, 1 as customer_id, null as order_date
  {% endcall %}

  {% call dbt_unit_testing.mock_ref ('stg_payments') %}
    select 1001 as order_id, 10 as amount
    UNION ALL
    select 1002 as order_id, 10 as amount
  {% endcall %}

  {% call dbt_unit_testing.expect() %}
    select 1 as customer_id, 20 as customer_lifetime_value
  {% endcall %}
{% endcall %}
```

一旦你建立了测试，只需运行 dbt test，你就会得到格式化的输出（也来自文档）：

```py
MODEL: customers
TEST:  should sum order values to calculate customer_lifetime_value
Rows mismatch:
| diff | count | customer_id | customer_lifetime_value |
| ---- | ----- | ----------- | ----------------------- |
| +    |     1 |           1 |                      20 |
| -    |     1 |           1 |                      30 |
```

除了手动创建模拟数据之外，这个软件包还支持通过直接查询你的数据仓库来推断列。我个人的经验是，在使这一功能正常工作时遇到了困难，但对于宽表来说，这一功能可能非常有用，因为否则你必须列举每一列。

# dbt-datamocktool

[dbt-datamocktool](https://github.com/mjirv/dbt-datamocktool) 是另一个 dbt 软件包，它采用了稍微不同的方法。模拟数据和期望值被创建为种子文件，然后测试被定义为 *schema.yml* 中软件包现有测试的输入。

```py
models:
  - name: stg_customers
    tests:
      - dbt_datamocktool.unit_test:
          input_mapping:
            source('jaffle_shop', 'raw_customers'): ref('dmt__raw_customers_1')
          expected_output: ref('dmt__expected_stg_customers_1')
          depends_on:
            - ref('raw_customers')
    columns: ...

  - name: stg_orders
    tests:
      - dbt_datamocktool.unit_test:
          input_mapping:
            ref('raw_orders'): ref('dmt__raw_orders_1')
          expected_output: ref('dmt__expected_stg_orders_1')
    columns: ...
```

除了提供模拟数据之外，你还会将模型的来源和引用映射到适当的模拟数据上。这种方法更加动手操作，但在项目需求方面也更加轻量。与 dbt-unit-testing 不同，你不必用软件包特定版本覆盖 *ref()* 和 *source()* 宏。大多数功能来自 dbt 的原生组件：测试和种子。

另一个令人兴奋的功能是对支持 *MERGE* 操作的适配器的增量模型的支持。这在 dbt 领域尤为重要，因为增量物化根据 dbt 调用的“模式”运行不同的查询；你需要能够测试查询的两个“版本”以实现全面覆盖。

# SQLMesh

新 comer 进入 SQL 建模/模板空间，[SQLMesh](https://sqlmesh.readthedocs.io/en/stable/) 带来了大量的好东西，其中之一是原生单元测试支持。测试在 YAML 文件中定义，然后在按需执行或重建 SQLMesh 计划时执行。

模拟数据非常简单，模拟数据和期望值都定义在同一个文件中。我没有见过的一个杀手级功能是能够测试*单个 CTEs*：

```py
test_example_full_model:
  model: sqlmesh_example.example_full_model
  inputs:
    sqlmesh_example.example_incremental_model:
        rows:
        - id: 1
          item_id: 1
          ds: '2020-01-01'
        - id: 2
          item_id: 1
          ds: '2020-01-02'
        - id: 3
          item_id: 2
          ds: '2020-01-03'
  outputs:
    ctes:
      filtered_orders_cte:
        rows:
          - id: 1
            item_id: 1
          - id: 2
            item_id: 1
    query:
      rows:
      - item_id: 1
        num_orders: 2
```

正如我们在 dbt-unit-testing 中看到的，测试输出的可视化效果很好：

```py
$ sqlmesh test
F
======================================================================
FAIL: test_example_full_model (/Users/izeigerman/github/tmp/tests/test_suite.yaml:1)
----------------------------------------------------------------------
AssertionError: Data differs
- {'item_id': 1, 'num_orders': 3}
?                              ^

+ {'item_id': 1, 'num_orders': 2}
?                              ^

----------------------------------------------------------------------
Ran 1 test in 0.008s

FAILED (failures=1)
```

对我来说，这是我见过的最有说服力的 SQL 单元测试选项；然而，复杂之处在于它是一个不是 dbt 的 SQL 框架。这是一个处于早期阶段的工具，面临着一个事实上的行业标准，在功能缺口和采纳惯性（即，它必须足够好才能让团队转而使用）之间，它的未来充满不确定性。

尽管如此，还是要阅读文档，了解为什么即使单元测试框架很令人印象深刻，这也不是 SQLMesh 最好的特性。

# 我们已经准备好了吗？

不。归根结底，现有的解决方案在功能上都非常接近，并且都存在类似的缺陷。所有工具或多或少做着相同的事情：使用模拟输入运行查询，并将查询输出与预期进行比较。在构建测试场景和生成输出的方式上，有些工具比其他工具更复杂，但差异相对较小。

我能看到三个采纳障碍：

1.  行业尚未达成一个标准。尽管 dbt 取得了显著成功，但它也存在着重要的缺陷（列级血缘追踪、单元测试、可扩展性）；采纳度推动了更多的采纳。没有任何单元测试解决方案达到了临界规模。

1.  文化尚未成熟。SQL 开发人员在没有单元测试的情况下已经成功地解决了几十年的问题。尽管分析工程师的兴起将越来越多的软件工程最佳实践带入了 SQL 开发，但并未涵盖所有最佳实践，单元测试就是一个缺口。

1.  数据模拟仍然是一个未解决的问题。在所有记录的例子中，我们看到的是少数几列和少数几张表之间的简单关系。实际查询可能复杂得多。开发人员感受到许多、甚至数十个连接条件以及为了使测试通过所需的引用完整性的负担。在某种程度上，这可以被视为一种特性：即开发人员必须考虑每个连接和每条记录。但从实际角度来看，这是一项繁重的任务，特别是当真正能用的数据就在眼前时，尤其令人心痒。

我希望关于行业中测试、质量和标准重要性的对话越来越多，这将推动我们朝着更成熟的实践和更好的成果前进。当我们能够像开发其他软件一样安全迅速地开发查询时，我们将释放出大量的价值。
