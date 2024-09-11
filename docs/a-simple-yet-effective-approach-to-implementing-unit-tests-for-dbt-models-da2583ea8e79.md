# 实施 dbt 模型单元测试的简单（但有效）方法

> 原文：[https://towardsdatascience.com/a-simple-yet-effective-approach-to-implementing-unit-tests-for-dbt-models-da2583ea8e79?source=collection_archive---------0-----------------------#2023-08-18](https://towardsdatascience.com/a-simple-yet-effective-approach-to-implementing-unit-tests-for-dbt-models-da2583ea8e79?source=collection_archive---------0-----------------------#2023-08-18)

## 单元测试 dbt 模型一直是 dbt 生态系统中最关键的缺失部分之一。本文提出了一种新的单元测试方法，依赖于标准和 dbt 最佳实践。

[](https://mahdiqb.medium.com/?source=post_page-----da2583ea8e79--------------------------------)[![Mahdi Karabiben](../Images/f1aac76435b8db295c306c76796a3201.png)](https://mahdiqb.medium.com/?source=post_page-----da2583ea8e79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----da2583ea8e79--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----da2583ea8e79--------------------------------) [Mahdi Karabiben](https://mahdiqb.medium.com/?source=post_page-----da2583ea8e79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cda12823b7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-yet-effective-approach-to-implementing-unit-tests-for-dbt-models-da2583ea8e79&user=Mahdi+Karabiben&userId=7cda12823b7a&source=post_page-7cda12823b7a----da2583ea8e79---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----da2583ea8e79--------------------------------) · 9分钟阅读 · 2023年8月18日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fda2583ea8e79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-yet-effective-approach-to-implementing-unit-tests-for-dbt-models-da2583ea8e79&user=Mahdi+Karabiben&userId=7cda12823b7a&source=-----da2583ea8e79---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fda2583ea8e79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-simple-yet-effective-approach-to-implementing-unit-tests-for-dbt-models-da2583ea8e79&source=-----da2583ea8e79---------------------bookmark_footer-----------)![](../Images/9fa3a5ea813d037c94ab620c1126f1a6.png)

照片由 [Fabio Ballasina](https://unsplash.com/@fabiolog?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

自从 [dbt](https://www.getdbt.com/product/what-is-dbt/) 将软件工程最佳实践引入数据工程领域以来，它的功能和周边生态系统不断扩展，覆盖了更多的数据转换领域。

然而，“*数据工程与软件工程最佳实践*”的一个关键部分仍然模糊不清且尚未解决：单元测试。

单元测试的重要性、它们为何对任何一行代码在称为“生产就绪”之前至关重要，以及它们为何不同于 dbt 测试或数据质量测试，已经被 [精彩阐述和解释](https://www.youtube.com/watch?v=hxvVhmhWRJA)。但如果我们要用一分钟的电梯陈述总结它们的重要性，那就是：

> 在数据工程中，通常有两个不同的元素需要测试：数据和我们的代码——dbt 测试（以及其他数据质量系统/工具）允许我们测试数据，而单元测试允许我们测试代码。

考虑到上述情况，社区自然进行了多个倡议，以通过开源单元测试功能增强 dbt（如 Equal Experts 的 [dbt 单元测试包](https://github.com/EqualExperts/dbt-unit-testing) 或 GoDataDriven 的 [dbt 专用 Pytest 插件](https://github.com/godatadriven/pytest-dbt-core)）。然而，这些包在功能上仍然有限，并且具有较高的学习曲线。

本文介绍了一种更简单却更优雅的方法，依靠标准和 dbt 最佳实践来实现可扩展且可靠的单元测试流程。

# 单元测试模型与 CTE

在深入探讨方法之前，让我们首先定义一下我们希望在何种级别运行单元测试。这里需要回答的问题与我们在 dbt 项目中的“可测试单元”的定义有关。

## 我们的可测试单元是什么？

如果我们坚持软件工程最佳实践，那么一个可测试单元就是一个具有清晰输入和输出的小段代码——并且不依赖于外部因素。根据 dbt 项目及其复杂性，这一定义可以适用于 dbt 模型及其构成的公共表表达式（CTEs）。

如果我们的 dbt 项目主要包含短小而相对简单的模型（每个模型少于 100 行代码），那么在模型级别定义单元测试是合适的。然而，如果我们的 dbt 项目包含长而复杂的模型，那么在 CTE 级别定义单元测试将是更高效的方法。

## 这是一种视角问题

dbt Labs（dbt Core的开发公司）[更倾向于第一种方法](https://www.getdbt.com/analytics-engineering/modular-data-modeling-technique/#data-model-readability)。因此，毫不奇怪，[他们的单元测试提案](https://github.com/dbt-labs/dbt-core/discussions/8275)专注于模型级别的测试，并将CTE级别的测试视为*反目标*。对于小型或新建的dbt项目，这可能是一个非常合理的设计决策。然而，如果你拥有一个成熟或大型的dbt项目，那么为了避免复杂模型而增加不必要的临时模型[可能不是一个好主意](https://medium.com/p/d49550f95580#:~:text=Why%20do%20we%20need%20design%20docs%20for%20data%20pipelines%3F)。

对于大多数dbt项目，在一个dbt模型中对同一实体执行多个操作（变换、聚合等）是完全可以接受的：我们可以首先对给定的列进行变换，然后使用它来聚合数据，或者我们可以创建一个新的标志，然后依赖它来应用一些业务逻辑或筛选数据。这些操作可以在同一模型中的不同CTE中进行，使CTE成为一个自然的可测试单元。

本文中介绍的方法将重点关注CTE的单元测试，而不是整个模型，尽管相同的原则可以调整用于模型级别的单元测试。

# 秘密武器：缩小宇宙

实施dbt模型的单元测试系统的主要复杂性在于“dbt模型”可以有很多种形式。从一个1000行的庞大`select`语句到一长串CTE和子查询，如果我们想构建一个可以用于任何dbt项目的通用单元测试系统，就需要考虑所有这些场景。

然而，我们可以通过限制问题的范围来完全避免这种复杂性。与其构建一个在任何dbt项目中都能工作的单元测试系统，不如只为***我们的***dbt项目构建一个。[正如Don Draper会告诉你的](https://www.youtube.com/watch?v=ijJonVw7YPY)，我们可以随时“***改变对话***”：

> 与其为dbt模型（***一个我们无法控制的宇宙***）构建单元测试系统，不如为我们的dbt模型（***一个我们可以控制的更小宇宙***）构建系统。

改变我们目标宇宙的范围大大简化了问题，因为我们了解我们的dbt模型及其特性。更重要的是，我们可以为它们的结构和内容定义标准和指南。

![](../Images/afca9da593d06a0764695eab547f05a0.png)

如何通过“缩小”宇宙来简化单元测试方法的视觉表示（作者提供的图像）

上述原则也可以应用于我们希望构建与dbt相关的自定义和功能的其他领域。设定标准并限制我们对“dbt模型”的期望，可以简化问题，并为直接解决方案打开大门。

# 导航熟悉的宇宙：标准化我们的dbt模型结构

现在我们已经将工作的范围限制在我们的 dbt 模型上，我们可以通过定义一个强制执行的模型结构来明确我们的输入，这个结构可以通过自动化（如 CI 操作）来强制执行。如果你已经在维护没有推荐模型结构的 dbt 项目，你可以在更新现有模型时逐步开始强制执行它。

理想的结构将取决于你的用例，但 dbt Labs 推荐的 ***import-intermediate-final*** 方法在大多数情况下是一个安全的选择。它基本上将 dbt 模型分为三种类型的 CTEs：

+   **导入 CTEs**：这些是模型的第一部分，仅仅是从其他模型中读取数据。我们可以进行列重命名和过滤，但在这个阶段不应对数据进行转换。理想情况下，它们的名称应以 `import_` 前缀开头。

+   **中间 CTEs**：这是工作的大部分内容发生的地方。我们可以使用这些 CTE 来转换、连接和汇总数据，应用业务逻辑，以及进行所有其他操作以生成模型的期望输出——使得这些 CTE 成为我们希望单元测试的对象。理想情况下，它们的名称应以 `intermediate_` 前缀开头。

+   **最终 CTE**：在这个 CTE 中，我们将不同的导入和中间 CTE 连接起来，以定义模型的输出——而不应用任何额外的转换。这个 CTE 应该命名为 `final`。

然后，作为模型的最后一部分，`select * from final` 语句将生成其输出。

![](../Images/b5463781d6ed959fd945029667e14bff.png)

使用 import-intermediate-final 结构的示例 dbt 模型（作者提供的图像）

既然我们有了可以围绕其设计的模型结构，定义单元测试系统变得更加容易。

但是，值得注意的是，理想情况下我们应该在开始使用 dbt 之前定义模型结构——以及其他 dbt 使用的基础。这是我和 Zendesk 团队在决定实施基于 dbt 的数据转换框架时关注的关键领域之一，我们在 Zendesk 工程博客上发布了[这篇文章](https://zendesk.engineering/dbt-at-zendesk-part-i-setting-foundations-for-scalability-34b55e6a6aa1)详细描述了我们的标准工作。

# 基于标准：一个简单的单元测试系统

基于我们讨论的结构，为了确保对 dbt 模型进行高效的单元测试代码覆盖，我们可以依赖测试中间 CTEs 和最终 CTE——因为导入 CTEs 不包含我们需要测试的代码。

![](../Images/011f38aae61b82b635f72cee65815e1c.png)

将 dbt 模型分为需要测试的输入和代码（作者提供的图像）

现在我们可以根据上述标准定义单元测试系统的不同组件及其交互方式。

## 模拟输入：导入 CTEs 的输出

我们系统的第一个组件是模拟输入（基本上是我们希望用来测试不同执行场景的样本数据）。在结构上，这应该类似于我们的导入 CTE 的输出——因此，如果我们从名为 `customers` 的模型中读取两列数据，则模拟输入应包含相同的结构。

![](../Images/fc2574b360143165136dd435a6bc18e5.png)

将导入 CTE 的输出转换为单元测试的模拟输入（图片由作者提供）

这些输入应该定义为 [dbt 种子](https://docs.getdbt.com/docs/build/seeds)（因此我们提供包含模拟数据的 CSV 文件），理想情况下放在一个单独的 `tests` dbt 项目中（以避免使我们的主项目混杂单元测试种子）。

## 预期结果：中间 CTE 的期望输出

现在我们已经定义了输入，我们可以使用相同的过程（dbt 种子）来定义期望的输出。这些输出对应于每个中间 CTE 执行后的数据的预期状态。

对于每个中间 CTE，我们可以有多个输出，以对应我们想测试的场景，以最大化代码覆盖率并确保我们测试边界情况。（可以在同一个 CSV 中使用 `test_id` 列来分隔不同的测试，或者提供多个 CSV 文件。）

## 运行测试：选择的奢侈

系统的最后一个架构组件是执行断言的过程（将 CTE 的实际输出与预期输出进行比较）。

从技术上讲，我们只想比较数据仓库中的两个表：

+   从我们的预期 dbt 种子生成的表（我们在 CSV 文件中提供的预期输出）

+   通过运行我们正在测试的 CTE 生成的表（使用模拟数据作为其输入）

然后可以使用现有的包，如 [dbt 审计助手](https://github.com/dbt-labs/dbt-audit-helper) 或数据质量系统如 [Soda Core](https://github.com/sodadata/soda-core)（使用 [SodaCL 的内置比较检查](https://docs.soda.io/soda-cl/compare.html)）。

## 连接各部分

最后，现在所有组件都已定义，我们只需通过一个模块（可以用 Python 或其他任何语言编写）将这些组件连接起来，该模块执行以下操作：

1.  编译我们要测试的 dbt 模型（以用实际值替换变量和宏）。

1.  解析编译后的模型并检索中间 CTE 的列表（理想情况下，我们可以依赖 `intermediate_` 前缀，但这可以通过多种方式实现）以及最终 CTE（使用其名称）。

1.  对于每个 CTE，将导入 CTE 和其他中间 CTE 的引用替换为相应的模拟数据引用（因此，我们将引用 dbt 种子，而不是 CTE）。在这里，我们可以依赖前缀（`import_` 和 `intermediate_`）来找到需要替换的 CTE 名称。

1.  对于每个 CTE，将其新主体（在替换引用后）作为 dbt 模型写入我们的测试 dbt 项目中。

1.  运行 `dbt seed` 命令在我们的测试项目中（以重新创建模拟数据表和预期输出表），理想情况下使用一个仅刷新与我们当前测试的模型相关的表的标签。

1.  使用我们选择的工具（一个 dbt 包或类似 Soda 的工具）执行断言，并根据输出打印结果或执行某些操作。

通过上述过程，我们可以在一次执行中测试每个中级 CTE（以及最终 CTE），而不必担心一个 CTE 的实际输出会干扰下一个 CTE。这意味着，对于给定的 CTE，dbt 仅会在运行时使用模拟输入（因为它引用了我们提供的 dbt 种子）——输入包括导入 CTE 的输出和我们引用的其他中级 CTE 的预期输出。

![](../Images/107eb6753a3c94defea5d20318729d39.png)

一个中级 CTE 的示例输入，其中我们引用了两个导入 CTE 和另一个中级 CTE（图像来源：作者）

# 结论

在本文中，我们介绍了一种可扩展且灵活的单元测试方法，用于 dbt 模型（在 CTE 级别），这需要相对较少的工程工作，并专注于执行标准。尽管该方法不能解决所有 dbt 项目的单元测试问题，但我相信许多团队可以在他们的项目中利用它（或其变体）来增加一个重要的安全层：测试代码。

这种方法可以根据 dbt 项目的特定情况进行调整和增强，并不适用于所有场景（例如，如果大部分代码位于宏中）——但标准化仍然是一个强大的盟友，能够简化在不同领域提升 dbt 的路径。

*如需更多数据工程内容，您可以订阅我的新闻通讯《数据浓缩》，我将在其中讨论各种与数据工程和技术相关的主题：*

[## Data Espresso | Mahdi Karabiben | Substack](https://dataespresso.substack.com/?source=post_page-----da2583ea8e79--------------------------------)

### 数百名订阅者。数据工程更新和评论，伴随您的下午浓缩咖啡。点击阅读…

[dataespresso.substack.com](https://dataespresso.substack.com/?source=post_page-----da2583ea8e79--------------------------------)
