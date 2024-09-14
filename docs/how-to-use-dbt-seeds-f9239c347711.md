# 如何使用 dbt 种子

> 原文：[`towardsdatascience.com/how-to-use-dbt-seeds-f9239c347711`](https://towardsdatascience.com/how-to-use-dbt-seeds-f9239c347711)

## 它们是什么，何时使用

[](https://madison-schott.medium.com/?source=post_page-----f9239c347711--------------------------------)![Madison Schott](https://madison-schott.medium.com/?source=post_page-----f9239c347711--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9239c347711--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9239c347711--------------------------------) [Madison Schott](https://madison-schott.medium.com/?source=post_page-----f9239c347711--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9239c347711--------------------------------) ·阅读时间 4 分钟·2023 年 4 月 25 日

--

![](img/c67417df60fabed2bd778b5ff088e330.png)

[engin akyurt](https://unsplash.com/@enginakyurt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/seeds?orientation=landscape&utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

我必须诚实地说，我已经很久没有使用 [dbt seeds](https://docs.getdbt.com/docs/build/seeds) 了。实际上，我是在几天前试图验证对我们一个数据模型所做的更改时重新发现了它们。

我的经理给我发送了一个包含在我们数据模型中发现的真正重复值的 CSV 文件。我正在处理在我们一个源的临时模型中删除这些重复项。修改后，我需要验证这些重复项是否不再出现在模型中。

我没有手动将 CSV 文件上传到我们的数据仓库，这在 Redshift 中几乎不可能。我使用了 dbt 种子将 CSV 文件导入我们的仓库。然后，我能够快速运行一个验证查询，以确认重复项已被删除。

这太简单了！作为一名分析工程师，我*喜欢*事情简单。因为老实说，它们很少如此简单。这让我思考我们如何进一步利用 dbt 种子来简化我们的分析工程师工作。

更不用说，dbt 种子还可以被数据科学家和机器学习工程师用来测试他们的模型。你可以用它来摄取测试数据和训练数据，使它们在你的数据仓库中存在为两个不同的参考点。

# 什么是 dbt 种子？

在 dbt 中，有三种主要类型的对象——源、模型和种子。你可以像对待源和模型一样测试和记录种子。唯一的区别是它们不是从你的 dbt 项目中的 SQL 代码创建的。它们不是数据模型。它们只是你已读取到 dbt 中的 CSV 文件，用于作为模型的*参考*。

# dbt 种子命令

要使用 dbt seeds，你需要一个要导入到数据仓库的 CSV 文件。首先，确保你能找到 CSV 文件的位置。然后，将其移动到 dbt 项目中的 seeds 目录。如果你已经在 dbt 项目中，可以运行以下命令将其移动到 seeds 目录：

```py
mv <CSV file path> seeds
```

每个 dbt 项目自动具有此目录，所以你不需要自己重新创建它。

注意：确保为这个 CSV 文件起一个描述性的名字！**最好在你的** **dbt 风格指南** **中设定关于 seeds 命名的标准。**

然后，简单地运行 dbt seed 命令：

```py
dbt seed
```

这将以你的 CSV 文件名在数据仓库的目标模式中创建一个表。现在，你可以像引用自己创建的 dbt 模型一样引用这个表！

如果你不熟悉，选择 dbt 模型的语法是：

```py
{{ ref('duplicate_users') }}
```

如果我的 CSV 文件名为 `duplicate_users.csv`，那么目标模式中的表将命名为 `duplicate_users`，我会像上面那样在另一个模型中引用这个表。

# 如何使用 dbt seeds

dbt seeds 可以用于许多不同的原因。每当你需要查询一个静态 CSV 文件时，你可以将其创建为 seed 并使用 dbt 转换这些数据！

## 验证

我在介绍中提到过一个使用 dbt seeds 进行验证的例子。这是我最近作为分析工程师使用 seeds 的方式。我有别人为我拉取的数据，我用它来检查对数据模型所做的一些更改的输出。

当业务团队找你寻求帮助时，他们提供给你一个 CSV 或 Excel 文件，指出某些数据问题，这时使用这个数据是很好的。你可以直接在 dbt 中使用这些数据来解决问题，而不是自己重新创建问题。

## 静态参考表

我看到大多数人使用 dbt seeds 来创建静态参考表。因为这些表始终保持不变，并且不需要更改，所以使用 seeds 将它们导入数据仓库是有意义的。

一些很好的例子是 `dim_date` 表、`country_code` 表或 `zipcode` 映射表。这些表在分析中经常使用但从不更改。与其手动创建这些表作为 dbt 数据模型，不如重用公开的文件并将其导入你的仓库。这样，你就不会增加额外的工作量！

## 测试和训练模型

与其处理大型数据集，不如使用 dbt seeds 导入 CSV 文件作为测试和训练数据集。这将节省运行模型的时间和在数据仓库中存储大量数据的空间，从而节省资金。

## 将业务知识转移到你的数据仓库

最后，你可以使用 seeds 来移动仅以电子表格形式存在的数据。如果公司还没有数据仓库，大多数数据通常存储在 Excel 电子表格中。

dbt seeds 允许你轻松导出这些电子表格，并将它们创建为数据表，旨在创建单一的真实数据源。只需记住，不要在业务团队仍在使用的电子表格上频繁种子/重新种子。这更适用于捕捉不再更新的静态或历史数据。

# 结论

当正确使用时，dbt seeds 功能强大。如果你已经是 dbt 用户，它们是将 CSV 文件移动到数据仓库的简单解决方案。它可以快速验证并创建静态数据表。因此，下次你需要使用 CSV 进行分析时，考虑一下种子文件吧！

想了解更多关于 dbt 和分析工程的信息，请查看我的[每周免费通讯](https://madisonmae.substack.com/)。
