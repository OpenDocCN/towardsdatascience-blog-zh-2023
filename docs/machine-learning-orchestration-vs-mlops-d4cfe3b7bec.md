# 机器学习编排与 MLOps

> 原文：[https://towardsdatascience.com/machine-learning-orchestration-vs-mlops-d4cfe3b7bec?source=collection_archive---------4-----------------------#2023-06-05](https://towardsdatascience.com/machine-learning-orchestration-vs-mlops-d4cfe3b7bec?source=collection_archive---------4-----------------------#2023-06-05)

[](https://fletchjeff.medium.com/?source=post_page-----d4cfe3b7bec--------------------------------)[![Jeff Fletcher](../Images/b5018f623161faa43a6a3289e25f7eb7.png)](https://fletchjeff.medium.com/?source=post_page-----d4cfe3b7bec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d4cfe3b7bec--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d4cfe3b7bec--------------------------------) [Jeff Fletcher](https://fletchjeff.medium.com/?source=post_page-----d4cfe3b7bec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff9a90e9b6352&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-orchestration-vs-mlops-d4cfe3b7bec&user=Jeff+Fletcher&userId=f9a90e9b6352&source=post_page-f9a90e9b6352----d4cfe3b7bec---------------------post_header-----------) 发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----d4cfe3b7bec--------------------------------) · 5 分钟阅读 · 2023年6月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd4cfe3b7bec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-orchestration-vs-mlops-d4cfe3b7bec&user=Jeff+Fletcher&userId=f9a90e9b6352&source=-----d4cfe3b7bec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd4cfe3b7bec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmachine-learning-orchestration-vs-mlops-d4cfe3b7bec&source=-----d4cfe3b7bec---------------------bookmark_footer-----------)![](../Images/50208277d57962573099116c20acd825.png)

图片由 [Mark Williams](https://unsplash.com/@markwilliamspics?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我听到过我合作过的 ML 工程师说“机器学习操作（MLOps）的主要部分只是数据工程”。有一篇博客文章将实际百分比定为[98%](https://mlops.community/mlops-is-mostly-data-engineering/)。这显然是夸张的说法，但我认为这个观点是正确的。然而，MLOps 的含义仍在变化。虽然有很多组件和变动的部分可以被认为是 MLOps 的一部分，[Cristiano Breuel 的这个定义](/ml-ops-machine-learning-as-an-engineering-discipline-b86ca4874a3f)足够好，适合我在这篇文章中讨论的内容：

> ML Ops 是一组结合了机器学习、DevOps 和数据工程的实践，旨在可靠且高效地部署和维护生产中的 ML 系统。

![](../Images/306fe372a11a2ca898c4ebdbf186ca6f.png)

作者改编自[原始图像](https://en.wikipedia.org/wiki/MLOps#/media/File:ML_Ops_Venn_Diagram.svg)由 CM Beuel 提供

我想补充的是，一个“好”的 ML 系统是：

> 解决业务需求并有效交付的系统。MLOps 团队的关注点应该始终放在解决业务需求上。

像大多数基于工作流（或管道）的系统一样，MLOps 系统需要一个协调器。在这个上下文中，它可以被称为机器学习协调器（或现在的 MLOx）。MLOx 的工作就是一个协调器的工作，即一个可以在非常具体的时间表上管理和协调复杂工作流和过程的机制。在 MLOps 和许多其他 ML 相关系统中，常用的一个协调器是[Apache Airflow](https://airflow.apache.org/)，其他包括 [Dagster](https://dagster.io/)、[Prefect](https://www.prefect.io/)、[Flyte](https://flyte.org/)、[Mage](https://www.mage.ai/)等。本文将重点关注 Airflow。

我喜欢把 MLOx 想象成类似于电影导演（或管弦乐队的指挥，但那可能会让人困惑，因为我们会讨论指挥管弦乐队）。导演有一个剧本，他们依据这个剧本来指导各种过程，以交付最终产品——电影。在 MLOx 的上下文中，工作流就相当于剧本。协调器的角色是确保工作流中的各种过程按计划、按正确的顺序执行，并妥善处理故障。

然而，Airflow 给这个类比带来了复杂性。Airflow 还具有计算能力，因为它可以利用运行环境来执行任何 Python 代码（类似于 Dagster、Prefect 等）。作为一个可扩展的开源工具，它更像是一个演员兼导演，其中一个 Airflow 任务可以像导演在电影中扮演角色一样成为工作流的一个重要部分。使用 Airflow，你可以将数据加载到内存中，进行一些处理，然后将数据传递到下一个任务。通过这种方式，Airflow 可以作为 MLOps 工具，也可以作为协调器。它可以直接执行所需的机器学习操作，或者仅仅作为协调器，实例化 TensorFlow 集群上的进程或启动 Spark 作业等。

简而言之，MLOx 是一个利用机器学习工具进行操作的协调器。

[ZenML](https://docs.zenml.io/user-guide/component-guide/orchestrators) 定义的 MLOx 如下：

> 协调器是任何 MLOps 堆栈中的关键组件，因为它负责运行你的机器学习管道。为此，协调器提供了一个环境，以便执行管道的各个步骤。它还确保只有在所有输入（即管道之前步骤的输出）都可用时，管道的步骤才会被执行。

![](../Images/bf1998d620181093d5cce5a520f2f5bf.png)

由作者改编自 [原始图像](https://en.wikipedia.org/wiki/MLOps#/media/File:ML_Ops_Venn_Diagram.svg) CM Beuel

使 Airflow 特别适合作为 MLOx 的特点包括：

+   **DAGs**（有向无环图）：DAGs 是机器学习工作流的可视化表示。DAGs 使得查看任务之间的依赖关系以及跟踪工作流进度变得更加容易。

+   **调度**：Airflow 可以用来定期调度机器学习工作流。这有助于确保机器学习模型始终保持最新，并及时用于预测。

+   **监控**：Airflow 提供了多种工具来监控机器学习工作流。这些工具可以用于跟踪机器学习模型的性能，并识别潜在的问题。

# 想想“Airflow 和”，而不是“Airflow 或”

我发现当人们决定选择 ML 工具时，一个复杂的问题是寻找一个能够完成所有工作的工具，包括协调。某些*一体化工具*包括一个基本的工作流调度器以满足最低要求，但它的能力可能远不如 Airflow。一旦你的 MLOx 需求超出了包含的协调器的能力，你就需要引入一个更强大的协调器，重新进行调度工作，并且可能还需要重写大量代码。

另一个问题是我看到有人将 Airflow 与做着截然不同事情的 ML 工具进行比较，有些工具的名称恰好以“flow”结尾，比如 [MLFlow](https://mlflow.org/) 或 [Kubeflow](https://www.kubeflow.org/)。MLFlow 主要用于实验跟踪，与 Airflow 的操作方式完全不同。Airflow 只是广泛 MLOps 工具空间中的另一个组件，从模型注册到实验跟踪到专门的模型训练平台。MLOps 涵盖了有效 ML 工作流管理所需的许多组件。

一些进入 MLOps 的人来自更具实验性的 数据科学 环境，并且没有 Dev Ops 和 数据工程 带来的更严格要求的经验。数据科学家以更随意的方式工作，而 MLOps 则需要结构化的方法。要实现端到端的 MLOps 管道，需要一种系统化、可重复的方法来处理数据、提取特征、训练模型和部署模型。Airflow 经常用于编排像这样的结构化过程，可能需要对那些习惯于更灵活的数据科学方法的人进行一些学习。然而，如果你计划扩展你的 MLOps 能力，应该从一开始就使用正确的工具。

使用 Airflow 作为 MLOx 的最终考虑是许多组织已经拥有一个正在进行某种数据编排工作的 Airflow 实现。如果已经有人知道如何管理 Airflow 基础设施，并且能够帮助创建和运行 DAG，你就拥有了启动和运行 MLOx 所需的一切。加上像 [gusty](https://github.com/pipeline-tools/gusty) 和 [Astro SDK](https://astro-sdk-python.readthedocs.io/en/stable/) 这样的自动 DAG 生成工具，或者来自 [ZenML](https://docs.zenml.io/user-guide/component-guide/orchestrators/airflow) 和 [Metaflow](https://docs.metaflow.org/production/scheduling-metaflow-flows/scheduling-with-airflow) 的机器学习专用 DAG 生成器，让你可以在不需要深入了解 Airflow 的情况下获得一个可用的 MLOx。

# 结论

机器学习编排，也称为 MLOx，是 MLOps 的一个重要组成部分。它需要一个全面且适应性强的解决方案。Apache Airflow 是一个强大的编排工具，能够实现无缝的工作流管理和执行。通过涵盖编排者和 MLOps 工具的角色，Airflow 使组织能够高效地部署和维护机器学习模型。随着 MLOps 领域的不断发展，采用像 Airflow 这样的工具对于最大化生产力和释放机器学习的真正潜力变得至关重要。
