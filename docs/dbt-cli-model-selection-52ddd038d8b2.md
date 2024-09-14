# dbt CLI 模型选择

> 原文：[`towardsdatascience.com/dbt-cli-model-selection-52ddd038d8b2`](https://towardsdatascience.com/dbt-cli-model-selection-52ddd038d8b2)

## 一个完整的选择特定模型的备忘单，当运行 dbt 命令时

[](https://gmyrianthous.medium.com/?source=post_page-----52ddd038d8b2--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----52ddd038d8b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----52ddd038d8b2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----52ddd038d8b2--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----52ddd038d8b2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----52ddd038d8b2--------------------------------) ·7 分钟阅读·2023 年 1 月 25 日

--

![](img/86526646e5b957d9c7aba56a61a39a50.png)

照片由 [Yulia Matvienko](https://unsplash.com/@yuliamatvienko?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/kgz9vsP5JCU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在处理 dbt 项目时，你需要确保用于运行或测试模型、种子和快照的 CLI 命令只包含感兴趣的资源（或其子集）。换句话说，你需要能够针对特定的模型、测试、种子或快照，以避免浪费资源和金钱。当你处理处理大量数据的大型模型时，这一点尤其重要。

默认情况下，`dbt run|test|seed|snapshot` 会在依赖图中执行所有相应的节点（即 `dbt run` 将运行所有模型，`dbt test` 将运行所有模型测试，依此类推）。在本文中，我们将介绍你在通过 dbt 命令行界面（CLI）运行或测试模型、种子或快照时可以利用的所有可能的模型选择简写。

如果你希望尝试我们在接下来的几个部分中介绍的命令，可以随时在本地创建一个示例 dbt 项目。你可以通过 这个逐步指南创建（可能不到两分钟）本地 dbt 环境。

## 运行 dbt 项目中的所有资源

为了选择 dbt 项目中的所有资源，你只需选择项目名称：

```py
# Runs all models in project my_dbt_project
dbt run --select my_dbt_project

# Runs all tests in project my_dbt_project
dbt test --select my_dbt_project

# Runs all snapshots in project my_dbt_project
dbt snapshot --select my_dbt_project

# Runs all seeds in project my_dbt_project
dbt seed --select my_dbt_project
```

## 选择特定资源

为了执行 `run`、`test`、`snapshot` 或 `seed` 命令以针对特定模型，你只需在 `--select` 选项中指定模型名称：

```py
# Run model with name `my_model`
dbt run --select my_model

# Run test with id `not_null_orders_order_id`
dbt test --select not_null_orders_order_id

# Run snapshot with name `my_snapshot`
dbt snapshot --select my_snapshot

# Run seed with name `my_seed`
dbt seed --select my_seed
```

你甚至可以通过指向定义模型的 SQL 文件的特定路径来运行特定的模型、种子或快照：

```py
# Run model my_model
dbt run --select path/to/my_model.sql

# Run snapshot my_snapshot
dbt snapshot --select path/to/my_snapshot.sql

# Run seed my_seed
dbt seed --select path/to/my_seed.sql
```

## 选择多个模型

`--select`接受多个参数，这意味着它能够同时运行多个模型（或测试、快照和种子）。要做到这一点，只需在运行命令时提供所有模式、测试、快照或种子名称：

```py
# Run multiple models
dbt run --select my_model another_model

# Run multiple tests
dbr test --select not_null_orders_order_id unique_orders_order_id

# Run multiple snapshots
dbt snapshot --select my_snapshot another_snapshot

# Run multiple seeds
dbt seed --select my_seed another_seed
```

## 选择节点和下游依赖

为了运行一个 dbt 节点及其下游依赖，您需要在资源名称后指定`+`操作符。

```py
# Run the model with name `my_model` as well as its downstream dependencies
dbt run --select my_model+

# Run my_model tests and the tests of its downstream dependencies 
dbt test --select my_model+

# Run seed my_seed and its downstream dependencies
dbt seed --select my_seed+
```

## 选择模型和上游依赖

同样地，要选择一个节点及其上游依赖，需要在节点名称之前指定`+`操作符：

```py
# Run the upstream dependencies of model `my_model` and the model itself
dbt run --select +my_model

# Run the tests of my_model and the tests of its upstream dependencies
dbt test --select +my_model

# Run the upstream dependencies of snapshot my_snapshot and the snapshot itself
dbt snapshot --select +my_snapshot

# Run the upstream dependencies of seed my_seed and the seed itself
dbt seed --select +my_seed
```

## 选择具有下游和上游依赖的模型

现在，为了运行一个模型及其所有下游和上游依赖，您只需在两个`+`操作符之间指定模型名称：

```py
# Run the model `my_model` including its parents and children nodes
dbt run --select +my_model+

# Run the tests for model `my_model` including the tests of its parents and children
dbt test --select +my_model+

# Run the snapshot `my_snapshot` and all downstream and upstream dependencies
dbt snapshot --select +my_snapshot+

# Run the seed `my_seed` and all of the downstream and upstream depdencies
dbt seed --select +my_seed+
```

## 选择模型和 N 个下游依赖

有可能您需要运行的不是模型的所有下游（子级）依赖，而是仅需遍历一定数量的边。这可以通过再次使用`+`操作符来实现，但这次需要指定要执行的父级模型的度数/级别。

```py
# Run model my_model and its first-degree children
dbt run --select my_model+1

# Run tests for `my_model` model and the tests of its first-degree children
dbt test --select my_model+1

# Run `my_snapshot` snapshot and its first-degree children
dbt snapshot --select my_snapshot+1

# Run seed `my_seed` and its first-degree children
dbt seed --select my_seed+1 
```

## 选择模型和 N 个上游依赖

以相同的方式，您可以指定在上游（或父级）依赖中要遍历的边数

```py
# Run my_model and its first and second degree parent nodes
dbt run --select 2+my_model

# Run tests of my_model and the tests of its first and second degree parents
dbt test --select 2+my_model

# Run snapshot my_snapshot and its first and second degree parent nodes
dbt snapshot --select 2+my_snapshot

# Run seed my_seed and its first and second degree parent nodes
dbt seed --select 2+my_seed
```

## 选择模型和 N 个上游以及 M 个下游依赖

最后，要选择一个模型以及 N 个父节点和 M 个子节点，您可以在上游和下游依赖的边数之间指定模型：

```py
# Run model `my_model`, its parents up to the 4th level and its downstreams up to the 5th level
dbt run --select 4+my_model+5

# Run tests of model `my_model` and the tests of its parents up to the 4th level and its downstreams up to the 5th level
dbt test --select 4+my_model+5

# Run snapshot `my_snapshot`, its parents up to the 4th level and its downstreams up to the 5th level
dbt snapshot --select 4+my_snapshot+5

# Run seed `my_seed`, its parents up to the 4th level and its downstreams up to the 5th level
dbt seed --select 4+my_seed+5
```

## 排除一个模型

除了`--select`之外，dbt CLI 还提供了`--exclude`标志（其语义与`--select`相同）。在`--exclude`参数中指定的任何模型将从用`--select`选择的模型集合中移除。

以下命令将运行除名为`my_model`的模型之外的所有模型：

```py
dbt run --exclude my_model
```

`--exclude`参数也适用于其他 dbt 命令：

```py
# Run all tests except the one with id `not_null_orders_order_id`
dbt test --exclude not_null_orders_order_id
# Run all tests except the tests of customers model
dbt test --exclude customers

# Run all snapshots except `my_snapshot`
dbt snapshot --exclude my_snapshot

# Run all seeds except `my_seed`
dbt seed --exclude my_seed
```

请注意，`--select`和`--exclude`参数可以在单个 dbt 命令中组合使用。

例如，以下命令将运行`my_package`包中的所有模型，但排除`user_base_model`及其下游依赖。

```py
dbt run --select my_package --exclude my_package.user_base_model+
```

## 运行特定包中的模型

要运行属于特定 dbt 包的模型、测试、快照或种子，您需要遵循点表示法，如下命令所示：

```py
# Runs model my_model in package mypackage
dbt run --select mypackage.my_model

# Runs tests of my_model model in package mypackage
dbt test --select mypackage.my_model

# Runs snapshot my_snapshot in package mypackage
dbt snapshot --select mypackage.my_snapshot

# Runs seed my_seed in package mypackage
dbt seed --select mypackage.my_seed
```

## 运行特定路径中的所有模型

为了运行位于特定目录下的模型、测试、快照或种子，您可以使用以下选择器表示法：

```py
# Runs all models under path.to.my.models directory
dbt run --select path.to.my.models

# Runs all tests under path.to.my.models directory
dbt test --select path.to.my.models

# Runs all snapshots under path.to.my.snapshots directory
dbt snapshot --select path.to.my.snapshots

# Runs all seeds under path.to.my.seeds directory
dbt seed --select path.to.my.seeds
```

除了点表示法，您还可以如下面所示在特定路径中运行模型：

```py
# Runs all models under path/to/my/models directory
dbt run --select path/to/my/models

# Runs all tests under path/to/my/models directory
dbt test --select path/to/my/models

# Runs all snapshots under path/to/my/snapshots directory
dbt snapshot --select path/to/my/snapshots

# Runs all seeds under path/to/my/seeds directory
dbt seed --select path/to/my/seeds
```

## 选择具有特定标签的模型

如果您有标记的资源并且希望执行所有这些资源，您可以提供`tag`选择器并跟上标签名称，如下命令所示：

```py
# Run all models with "finance" tag
dbt run --select tag:finance
```

## 组合多个选择器

注意，你实际上可以将教程中描述的几乎任何选择器组合在一个命令中。例如，以下命令将运行每一个标记为 `finance` 标签的资源，单个模型 `my_model` 以及路径 `path.to.my.marketing.models` 中的所有模型：

```py
dbt run --select tag:finance my_model path.to.my.marketing.models
```

像往常一样，这可以应用于几乎所有的资源，包括测试、种子和快照：

```py
# Tests
dbt test --select tag:finance not_null_orders_order_id path.to.my.marketing.models

# Seeds
dbt seed --select tag:finance my_seed path.to.my.marketing.seeds

# Snapshots
dbt snapshot --select tag:finance my_snapshot path.to.my.marketing.snapshots
```

## 最终思考

总结来说，在处理 dbt 项目时，能够针对特定的模型、测试、种子或快照进行操作是很重要的，以避免浪费资源和金钱。 dbt 命令行界面（CLI）提供了多种简写方式，允许你选择特定的资源进行运行、测试、种子或快照。这些包括在运行任何 dbt 命令时包含或排除这些模型的能力。

[**成为会员**](https://gmyrianthous.medium.com/membership) **并阅读 Medium 上的每一个故事。你的会员费直接支持我和其他你阅读的作者。你还将获得 Medium 上每一个故事的完全访问权限。**

[](https://gmyrianthous.medium.com/membership?source=post_page-----52ddd038d8b2--------------------------------) [## 通过我的推荐链接加入 Medium — Giorgos Myrianthous

### 作为 Medium 的会员，你的一部分会员费会支付给你阅读的作者，同时你可以完全访问每一个故事…

[gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership?source=post_page-----52ddd038d8b2--------------------------------)

**相关的文章你可能也会喜欢**

[](/etl-vs-elt-68e221d78719?source=post_page-----52ddd038d8b2--------------------------------) ## ETL vs ELT：有什么区别？

### 数据工程中的 ETL 和 ELT 的比较

[towardsdatascience.com [](/staging-intermediate-mart-models-dbt-2a759ecc1db1?source=post_page-----52ddd038d8b2--------------------------------) ## dbt 中的分阶段 vs 中间 vs mart 模型

### 理解在数据构建工具（dbt）背景下，分阶段、中间和 mart 模型的目的

[towardsdatascience.com [](/dbt-models-structure-c31c8977b5fc?source=post_page-----52ddd038d8b2--------------------------------) ## 如何构建你的 dbt 项目和数据模型

### 强化 dbt 数据模型的有效结构

[towardsdatascience.com
