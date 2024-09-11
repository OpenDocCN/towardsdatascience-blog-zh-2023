# 用 Hamilton 在 8 分钟内简化 Airflow DAG 的创建和维护

> 原文：[https://towardsdatascience.com/simplify-airflow-dag-creation-and-maintenance-with-hamilton-in-8-minutes-e6e48c9c2cb0?source=collection_archive---------6-----------------------#2023-07-05](https://towardsdatascience.com/simplify-airflow-dag-creation-and-maintenance-with-hamilton-in-8-minutes-e6e48c9c2cb0?source=collection_archive---------6-----------------------#2023-07-05)

## 如何利用 Hamilton 编写更易维护的 Airflow DAG

[](https://medium.com/@stefan.krawczyk?source=post_page-----e6e48c9c2cb0--------------------------------)[![Stefan Krawczyk](../Images/150405abaad9590e1dc2589168ed2fa3.png)](https://medium.com/@stefan.krawczyk?source=post_page-----e6e48c9c2cb0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e6e48c9c2cb0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e6e48c9c2cb0--------------------------------) [Stefan Krawczyk](https://medium.com/@stefan.krawczyk?source=post_page-----e6e48c9c2cb0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F193628e26f00&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-airflow-dag-creation-and-maintenance-with-hamilton-in-8-minutes-e6e48c9c2cb0&user=Stefan+Krawczyk&userId=193628e26f00&source=post_page-193628e26f00----e6e48c9c2cb0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e6e48c9c2cb0--------------------------------) ·8分钟阅读·2023年7月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe6e48c9c2cb0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-airflow-dag-creation-and-maintenance-with-hamilton-in-8-minutes-e6e48c9c2cb0&user=Stefan+Krawczyk&userId=193628e26f00&source=-----e6e48c9c2cb0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe6e48c9c2cb0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsimplify-airflow-dag-creation-and-maintenance-with-hamilton-in-8-minutes-e6e48c9c2cb0&source=-----e6e48c9c2cb0---------------------bookmark_footer-----------)![](../Images/2a7ff556e1e9539020d0b8b388dab73f.png)

一个抽象的表示，展示了 Airflow 和 Hamilton 之间的关系。Airflow 帮助将一切整合起来，而 Hamilton 则帮助管理内部细节。图片来自 [Pixabay](https://pixabay.com/illustrations/abstract-lines-perspective-plan-3242256/)。

*本文与* [*Thierry Jean*](https://medium.com/u/cf12dc7f8440?source=post_page-----e6e48c9c2cb0--------------------------------) *合作撰写，并最初发布于* [*此处*](https://blog.dagworks.io/p/supercharge-your-airflow-dag-with)*。*

本文介绍了两个开源项目，[Hamilton](https://github.com/dagworks-inc/hamilton)和[Airflow](https://airflow.apache.org/)，以及它们的[有向无环图](https://en.wikipedia.org/wiki/Directed_acyclic_graph)（DAGs）如何协同工作。在高层次上，Airflow负责编排（宏观），而Hamilton帮助编写干净和可维护的数据转换（微观）。

对于不熟悉Hamilton的人，我们推荐你查看[tryhamilton.dev](http://www.tryhamilton.dev/)上的互动概述，或我们的其他帖子，例如这篇[文章](/functions-dags-introducing-hamilton-a-microframework-for-dataframe-generation-more-8e34b84efc1d)。否则，我们将高层次地讨论Hamilton，并指向参考文档以获取更多细节。作为参考，我是Hamilton的共同创作者之一。

对于仍在尝试理解这两者如何协同运行的人来说，你可以与Airflow一起运行Hamilton的原因是Hamilton只是一个依赖性小的库，因此可以迅速将Hamilton添加到你的Airflow设置中！

仅作总结，Airflow是编排数据管道的行业标准。它支持各种数据计划，包括ETL、机器学习管道和商业智能。自2014年首次推出以来，Airflow用户在编写和维护数据管道方面面临一些挑战：

1.  可维护地管理工作流的演变；起初简单的工作流往往会变得复杂。

1.  编写模块化、可重用和可测试的代码，以便在Airflow任务中运行。

1.  跟踪Airflow DAG生成的代码和数据工件的血统。

我们相信**Hamilton**可以提供帮助！[Hamilton](https://github.com/dagworks-inc/hamilton)是一个用于编写数据转换的Python微框架。简而言之，用户以“声明性”风格编写python函数，Hamilton会根据函数的名称、参数和类型注释解析这些函数，并将它们连接成一个图。可以请求特定的输出，Hamilton将执行所需的函数路径以生成这些输出。由于它不提供宏观编排能力，因此与Airflow很好地配合，帮助数据专业人员编写更干净、更可重用的Airflow DAG代码。

![](../Images/946b089522c6fba64b5fc5cc216d6480.png)

Hamilton范式的示意图。此示例展示了如何将过程性pandas代码映射到定义DAG的Hamilton函数。注意：Hamilton可以用于任何Python对象类型，而不仅仅是Pandas。图片由作者提供。

# 编写可维护的Airflow DAG

Airflow 的一个常见用途是帮助进行机器学习/数据科学。生产中运行这种工作负载通常需要复杂的工作流。使用 Airflow 的一个必要设计决策是确定如何将工作流拆分为 Airflow 任务。创建太多任务会增加调度和执行开销（例如，移动大量数据），创建太少任务则会有大型任务可能需要较长时间运行，但可能更高效。这里的权衡是 Airflow DAG 的复杂性与每个任务中的代码复杂性。这使得调试和理解工作流变得更加困难，尤其是当你没有编写初始 Airflow DAG 时。往往，Airflow DAG 的初始任务结构变得固定，因为重构任务代码变得非常困难！

尽管像 `A->B->C` 这样的简单 DAG 是理想的，但结构的简单性和每个任务的代码量之间存在固有的紧张关系。每个任务的代码量越多，识别故障点就越困难，这可能会影响计算效率，但在故障的情况下，重试的成本随着任务的“大小”增长。

![](../Images/86d7f093205e068e4d4295544e29adac.png)

Airflow DAG 结构选择：任务数量？每个任务的代码量？图片作者。

相反，如果你可以同时处理 Airflow 任务中的复杂性，无论其中的代码量多大，并且能够以最小的努力轻松更改 Airflow DAG 的形状呢？这就是 Hamilton 的作用。

使用 Hamilton，你可以用 Hamilton DAG 替换每个 Airflow 任务中的代码，Hamilton 处理任务内代码的“微观”编排。注意：Hamilton 实际上使你能够逻辑上建模你希望 Airflow DAG 做的所有事情。更多内容见下文。

要使用 Hamilton，你需要加载一个包含 Hamilton 函数的 Python 模块，实例化一个 [Hamilton Driver](https://hamilton.dagworks.io/en/latest/concepts/driver-capabilities/)，然后在 Airflow 任务中执行一个 Hamilton DAG，只需几行代码。通过使用 Hamilton，你可以以任意粒度编写数据转换，允许你更详细地检查每个 Airflow 任务的操作。

具体的代码机制是：

1.  导入你的函数模块

1.  将它们传递给 Hamilton 驱动程序以构建 DAG。

1.  然后，调用 `Driver.execute()`，传入你希望从你定义的 DAG 中执行的输出。

让我们看一些代码，这些代码创建了一个单节点的 Airflow DAG，但使用 Hamilton 来训练和评估 ML 模型：

现在，我们这里没有展示 Hamilton 代码，但这种方法的好处是：

1.  **单元和集成测试。** Hamilton 通过其命名和类型注解要求，推动开发人员编写模块化的 Python 代码。这使得 Python 模块非常适合进行单元测试。一旦你的 Python 代码经过单元测试，你可以开发集成测试以确保它在 Airflow 任务中正常工作。相比之下，测试包含在 Airflow 任务中的代码并不简单，特别是在 CI/CD 环境中，因为这需要访问 Airflow 环境。

1.  **重用数据转换。** 这种方法将数据转换代码保留在 Python 模块中，与 Airflow DAG 文件分开。这意味着这些代码也可以在 Airflow *外部* 运行！如果你来自分析领域，这种方法应该类似于在外部 `.sql` 文件中开发和测试 SQL 查询，然后将其加载到 Airflow Postgres 操作员中。

1.  **轻松重新组织你的 Airflow DAG。** 更改 Airflow DAG 的难度现在大大降低。如果你在 Hamilton 中逻辑建模一切，例如端到端的机器学习管道，只需确定这个 Hamilton DAG 中有多少内容需要在每个 Airflow 任务中计算。例如，你可以将任务数量从一个庞大的 Airflow 任务更改为几个或许多——只需调整你从 Hamilton 请求的内容，因为你的 Hamilton DAG 完全不需要更改！

# 使用 Hamilton 和 Airflow 的迭代开发

在大多数数据科学项目中，从第一天起就编写最终系统的 DAG 几乎是不可能的，因为需求会发生变化。例如，数据科学团队可能希望尝试不同的特征集。在特征集确定和最终确定之前，将其包含在源代码中并进行版本控制可能并不理想；配置文件会更好。

Airflow 支持默认和运行时的 DAG 配置，并会记录这些设置以确保每次 DAG 运行都是可重复的。然而，添加可配置的行为需要在你的 Airflow 任务代码中添加条件语句和复杂性。这段代码可能在项目过程中变得过时，或仅在特定场景下有用，*最终*降低你的 DAG 可读性。

相比之下，Hamilton 可以使用 Airflow 的运行时配置动态执行函数图中的不同数据转换。这种分层方法可以大大增加 Airflow DAG 的表达能力，同时保持结构上的简单性。或者，Airflow 可以从配置中 [动态生成新的 DAG](https://airflow.apache.org/docs/apache-airflow/stable/howto/dynamic-dag-generation.html)，但这可能会降低可观察性，而且这些功能仍然是实验性的。

![](../Images/422eabbc6ecaa3194d10d42aba2a8e52.png)

Airflow UI 的 DAG 运行配置。图像由作者提供。

如果你在一个交接模型中工作，这种方法促进了数据工程师（负责 Airflow 生产系统）与数据科学家（负责编写 Hamilton 代码以开发业务解决方案）之间的关注点分离。这样的分离还可以提高数据一致性，减少代码重复。例如，可以用不同的 Hamilton 模块重用单个 Airflow DAG 以创建不同的模型。类似地，相同的 Hamilton 数据转换可以在不同的 Airflow DAG 中重用，以支持仪表板、API、应用程序等。

以下是两张图片。第一张展示了包含两个节点的高层次 Airflow DAG。第二张展示了在 Airflow 任务 `train_and_evaluate_model` 中导入的 Python 模块 `evaluate_model` 的低层次 Hamilton DAG。

![](../Images/0e45dd55064d29ec14ea5cd81351a6c6.png)

1\. Airflow UI：缺勤 Airflow DAG

![](../Images/5ec58be392b5bebce940e285035ee8d6.png)

2\. Hamilton 驱动器可视化：evaluate_model.py 的函数图

# 处理数据工件

数据科学项目会产生大量的数据工件，包括数据集、性能评估、图表、训练模型等。在项目生命周期（数据探索、模型优化、生产调试等）中所需的工件会有所变化。这对于 Airflow 来说是个问题，因为从 DAG 中删除任务会删除其元数据历史记录，并破坏工件的传承。在某些情况下，生成不必要或冗余的数据工件可能会产生显著的计算和存储成本。

Hamilton 可以通过其 [data saver API](https://hamilton.dagworks.io/en/latest/reference/decorators/save_to/#save-to) 提供生成数据工件所需的灵活性。被 `@save_to.*` 装饰的函数增加了存储其输出的可能性，只需通过 `Driver.execute()` 请求此功能。下面的代码中，调用 `validation_predictions_table` 将返回表格，而调用 `save_validation_predictions` 的 `output_name_` 值将返回表格并将其保存为 `.csv`

这种附加的灵活性使用户可以轻松切换生成的工件，并且可以直接通过 Airflow 运行时配置完成，无需编辑 Airflow DAG 或 Hamilton 模块。

此外，细粒度的 Hamilton 函数图允许精确的数据传承与来源追踪。工具函数 `what_is_downstream_of()` 和 `what_is_upstream_of()` 有助于可视化和编程探索数据依赖关系。我们指引感兴趣的读者了解更多详情 [这里](https://medium.com/towards-data-science/lineage-hamilton-in-10-minutes-c2b8a944e2e6)。

# 完成与示例入门

希望到现在为止我们已经让你印象深刻，结合 Hamilton 和 Airflow 将帮助你解决 Airflow 的 DAG 创建与维护挑战。由于这是篇短文，最后，让我们看看在[Hamilton 仓库](https://github.com/DAGWorks-Inc/hamilton/tree/main/examples)中的代码。

为了帮助你快速上手，我们提供了一个[示例](https://github.com/DAGWorks-Inc/hamilton/tree/main/examples/airflow)，展示如何将 Hamilton 与 Airflow 一起使用。它涵盖了你需要了解的所有基础知识。README 包括如何使用 Docker 设置 Airflow，这样你就不需要担心仅仅为了玩这个示例而安装依赖项。

关于示例中的代码，它包含两个 Airflow DAG，一个展示了一个[基本 Hamilton “使用方法”](https://github.com/DAGWorks-Inc/hamilton/blob/main/examples/airflow/dags/hamilton/hamilton_how_to_dag.py)用于创建用于训练模型的“特征”，另一个是一个更完整的[机器学习项目示例](https://github.com/DAGWorks-Inc/hamilton/blob/main/examples/airflow/dags/hamilton/absenteeism_prediction_dag.py)，完成了创建特征、拟合和评估模型的完整端到端流程。对于这两个示例，你可以在插件文件夹下找到 Hamilton 代码。

![](../Images/b27744e2f236a1e08e0a7f94ce091547.png)

你应该在 Airflow 示例中看到的内容。图片由作者提供。

如果你有问题或需要帮助——请加入我们的[Slack](https://hamilton-opensource.slack.com/join/shared_invite/zt-1bjs72asx-wcUTgH7q7QX1igiQ5bbdcg#/shared-invite/email)。否则，若要了解更多关于 Hamilton 的功能和特点，请参考 Hamilton 的[文档](https://hamilton.dagworks.io/)。

# 参考资料与进一步阅读

感谢你查看这篇文章。如果你想深入了解，或想了解更多关于 Hamilton 的信息，我们提供了以下链接供你浏览！

+   [Hamilton + Airflow 代码示例](https://github.com/DAGWorks-Inc/hamilton/tree/main/examples/airflow)

+   [Hamilton 文档](https://hamilton.dagworks.io/)

+   [tryhamilton.dev](https://www.tryhamilton.dev/)——一种互动方式来了解更多关于 Hamilton 的信息。

+   关于另一个与 Hamilton 集成的编排系统，你可以查看[Hamilton + Metaflow](https://outerbounds.com/blog/developing-scalable-feature-engineering-dags/)。

+   [Hamilton Slack 社区](https://hamilton-opensource.slack.com/join/shared_invite/zt-1bjs72asx-wcUTgH7q7QX1igiQ5bbdcg#/shared-invite/email)

+   [Lineage + Hamilton 在 10 分钟内](https://medium.com/towards-data-science/lineage-hamilton-in-10-minutes-c2b8a944e2e6)

+   [介绍 Hamilton](/functions-dags-introducing-hamilton-a-microframework-for-dataframe-generation-more-8e34b84efc1d)（背景故事和介绍）

+   [Hamilton + Pandas 5 分钟速成](/how-to-use-hamilton-with-pandas-in-5-minutes-89f63e5af8f5)

+   [如何在 Notebook 环境中使用 Hamilton](/how-to-iterate-with-hamilton-in-a-notebook-8ec0f85851ed)
