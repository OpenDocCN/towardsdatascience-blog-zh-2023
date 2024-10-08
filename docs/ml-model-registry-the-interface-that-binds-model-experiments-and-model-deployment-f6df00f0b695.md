# ML 模型注册中心——绑定模型实验和模型部署的“接口”

> 原文：[`towardsdatascience.com/ml-model-registry-the-interface-that-binds-model-experiments-and-model-deployment-f6df00f0b695`](https://towardsdatascience.com/ml-model-registry-the-interface-that-binds-model-experiments-and-model-deployment-f6df00f0b695)

## 实践中的 MLOps——深入探讨 ML 模型注册中心、模型版本控制和模型生命周期管理

[](https://medium.com/@weiyunna91?source=post_page-----f6df00f0b695--------------------------------)![YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----f6df00f0b695--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f6df00f0b695--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6df00f0b695--------------------------------) [YUNNA WEI](https://medium.com/@weiyunna91?source=post_page-----f6df00f0b695--------------------------------)

·发表于 [数据科学之路](https://towardsdatascience.com/?source=post_page-----f6df00f0b695--------------------------------) ·阅读时间 8 分钟·2023 年 2 月 16 日

--

## **背景**

在我之前的文章中：

实践中的 MLOps——将 ML 解决方案架构拆解为 10 个组件

我谈到了管理 ML 实验运行中生成的模型元数据和工件的架构重要性。我们都知道，模型训练过程会产生许多工件，用于进一步调优 ML 模型性能以及后续的 ML 模型部署。这些工件包括训练后的模型、模型参数和超参数、指标、代码、笔记本、配置等。集中管理和利用这些模型工件和元数据对于构建稳健的 MLOps 架构至关重要。因此，在今天的文章中，我将讨论 ML 模型注册中心，它作为绑定模型实验和模型部署的“接口”。

今天的文章将重点讨论以下方面：

+   ML 注册中心是什么？以及 ML 注册中心执行的关键功能

+   ML 模型注册中心带来的关键好处

+   如何将 ML 模型注册中心集成到端到端的 MLOps 解决方案中

+   ML 注册中心背后的技术以及流行的开源 ML 注册中心解决方案

![](img/e5ac4442dd93d7bac0482abd94be00af.png)

图片来源：[Robert Anasch](https://unsplash.com/ko/@diesektion?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 什么是 ML 注册中心？

ML 注册表是一个集中存储所有 ML 工件及其元数据的地方，从早期阶段的实验到生产就绪的模型。类似于 DockerHub 这样的容器注册表或 PyPi 这样的 Python 包注册表，ML 注册表允许数据科学家和 ML 从业者发布和共享 ML 模型及工件。通常，ML 注册表提供一个用户界面（UI），以及一套 API，供 ML 管理员和用户注册、发现、共享、版本控制和管理 ML 模型的权限和生命周期。

存储在 ML 注册表中的信息可以总结为以下几类：

+   **模型元数据** 包括模型名称、模型注释和描述、模型标签、模型创建时间、模型修改时间和模型架构（模型输入架构和输出架构）；

+   **模型生命周期管理** 包括模型生命周期的所有阶段，从创建到退役。例如，[MLflow](https://mlflow.org/) 提供了预定义的阶段，适用于常见的用例，如 Staging、Production 和 Archived。

+   **模型版本控制。** 对于任何注册的 ML 模型，几乎可以肯定会有多个版本。原因是 ML 模型需要不断监控和更新，以反映业务环境和数据的变化。模型版本控制不仅仅是提供版本号，而是一种将 ML 模型的每个版本与用于训练的相应数据、特征和代码对齐的机制，实现端到端的模型沿袭。

+   **模型治理 —** 包括管理模型权限（控制谁可以管理和更新模型）、审计模型活动和使用记录、在部署到生产环境之前审查和批准模型、关键模型变更的通知以及模型沿袭。

+   **模型服务**。模型注册表可以通过提供 Webhooks 来促进模型服务，使 ML 工程师能够监听模型注册表事件。当特定事件发生时，可以自动触发相应的操作。你可以使用 Webhooks 来自动化并将你的 ML 流水线与现有的 CI/CD 工具和工作流集成。例如，当创建新版本的模型时，你可以触发 CI 构建，或在每次请求模型转移到生产时通过 Slack 通知你的团队成员。

+   **模型监控和调试**。在 ML 模型部署到生产环境后，监控模型的性能是必要的。ML 模型注册表提供了数据科学家使用生产数据测试已部署模型的机制，以密切监控模型的表现。如果发现模型降级，数据科学家可以利用模型沿袭信息来识别根本原因。

![](img/552fea98b636c845b90b95cdac652d4d.png)

ML 模型注册表的关键组件 | 图片来源：作者

## ML 模型注册表存储带来的主要好处

+   **生产力** — 一个中央 ML 注册表显著地消除了每个数据科学/ML 团队管理自己 ML 模型和工件的孤岛。一个 ML 注册表可以像一个 ML 模型市场一样运作，团队可以在其中发布、分享和重用其他团队的工作。总体而言，这可以显著提高团队的生产力，并且用更少的资源开发更多的 ML 应用。

+   **治理** — 负责任的 AI 和 ML 治理一直是许多组织在监管、伦理、社会和法律方面的重要话题。一个中央 ML 注册表可以通过提供模型权限、模型使用记录、模型审计报告以及模型从原始数据和特征的血统等信息来协助 AI/ML 治理工作。

+   **协作** — 一个 ML 模型注册表是数据科学家和 ML 工程师共享的单一统一接口，可以促进和简化数据科学家与 ML 工程师之间的交接。当数据科学家对经过多轮实验后的整体模型性能感到满意时，他们将把代码和模型交给 ML 工程师进行生产部署。拥有 ML 模型注册表，ML 工程师可以清晰地了解模型是如何训练的、使用了哪些数据和特征以及特征工程逻辑是什么。这显著减少了数据科学家和 ML 工程师之间的沟通工作，提高了团队之间的协作。

![](img/86c924d8f744970f8422d290f1dff5fa.png)

ML 模型注册表存储带来的关键好处 | 作者提供的图像

## ML 注册表存储背后的技术

在幕后，模型注册表通常包括以下两个元素：

+   一个是 ML 实体（元数据）存储 — 实体存储存储 ML 实体的元数据，例如 ML 实验、运行、参数、指标、标签、备注、来源、生命周期阶段以及 ML 工件位置。ML 实体存储通常由 SQL 关系数据库实现，如 PostgreSQL、MySQL、MSSQL 和 SQLite。

+   另一个是 ML 工件存储 — 工件存储持久化工件文件、模型、图像、内存对象、模型摘要或任何记录到 ML 注册表存储的对象。工件存储是一个适合大数据的位置，是客户端记录其工件输出（例如模型）的地方。工件存储的实现通常由持久文件系统支持，如 Amazon S3 和 S3 兼容存储、Azure Blob 存储、Google Cloud 存储、FTP 服务器、SFTP 服务器、NFS 和 HDFS。

![](img/7caf11682d37e4f2d9b114270a2f2534.png)

ML 注册表存储背后的技术 | 作者提供的图像

## 将 ML 模型注册表集成到端到端 MLOps 解决方案中

一个 ML 模型注册表在端到端 MLOps 解决方案的 3 个关键阶段中扮演着非常重要的角色：

+   第一个是**模型开发**——模型开发是非常迭代和实验性的，这意味着数据科学家需要尝试各种算法、框架以及这些算法的不同特征、参数和超参数组合，以找出最适合问题的方案。因此，能够重现机器学习实验的运行可以显著提高数据科学家的生产力，帮助他们更快地找到最理想的解决方案。机器学习模型注册提供了谱系功能，使数据科学家可以从注册的机器学习模型追溯到产生模型的训练运行，以便他们可以重现模型或做出必要的更改以重新训练模型的新版本。

+   第二个是**模型部署**——模型注册事件（例如，为相关模型创建新模型版本或将模型版本的阶段从测试阶段更改为生产阶段）可以被利用来自动触发机器学习模型的部署。例如，你可以在创建新模型版本时触发 CI 构建，或者每次请求模型转生产时通过 Slack 通知团队成员。你还可以将模型注册事件集成到现有的 CI/CD 管道和工作流中，以自动触发这些过程。

+   第三个是**生产中的模型**——模型注册提供了所有生产中模型的整体视图，并允许 ML 操作团队相应地监控这些模型。机器学习模型非常依赖数据。因此，机器学习模型的性能可能会因数据环境的不断变化而恶化，而不仅仅是由于编码不佳。一旦识别出模型性能恶化，ML 注册服务可以通过提供必要的模型工件和模型谱系功能，帮助 ML 操作团队调试和重新训练模型。

因此，MLOps 的整体实现，直到你拥有一个最先进的模型注册系统，才能完全且正确地进行。

![](img/0d65ddb593fceda9a275ed5a14dc2e53.png)

将机器学习模型注册集成到端到端的 MLOps 解决方案中 | 图片作者

## 流行的机器学习注册存储解决方案

+   [MLflow 模型注册](https://mlflow.org/docs/latest/model-registry.html)——MLflow 是一个开源平台，用于管理机器学习生命周期，包括实验、可重复性、部署和集中模型注册。MLflow 的第四个主要组件是模型注册。使用 MLflow，你可以在运行 MLflow 的机器的本地文件系统中建立一个注册存储，或者你可以启动一个远程的中央跟踪服务器，团队可以在其中集中注册和共享机器学习模型工件。如果你已经在 Databricks 上，你将可以访问一个托管的跟踪服务器。

+   [VertaAI ModelDB](https://github.com/VertaAI/modeldb)——一个开源系统，用于机器学习模型版本控制、元数据和实验管理。Verta 库提供了一个模型目录组件，用户可以在其中查找、发布和使用 ML 模型或 ML 模型管道组件。

+   [Amazon SageMaker Model Registry](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html)——众所周知，SageMaker 是 AWS 的 ML 管理工具，提供各种组件供用户构建、训练和部署 ML 模型。SageMaker 还有一个模型注册服务，用户可以将 ML 模型的版本编入预定义的模型包组。如果您在 SageMaker 上构建了您的 ML 平台，那么 SageMaker 原生的模型注册表可能是一个不错的选择。

## 总结

ML 模型注册表是 MLOps 的核心组件，有助于减少模型实验、活动和模型生产活动之间的已知差距。我相信，MLOps 只有在拥有先进的模型注册表时才能做到正确。

如果您对这个话题或其他 MLOps 相关话题有任何意见或问题，请随时告诉我！我通常每周发布 1 篇与数据和 AI 相关的文章。请随时在 Medium 上关注我，以便在这些文章发布时获得通知。

如果您想查看更多关于现代高效数据+AI 堆栈的指南、深入探讨和见解，请订阅我的免费新闻通讯——[***Efficient Data+AI Stack***](https://yunnawei.substack.com/)，谢谢！

注意：如果您还没有成为 Medium 会员，而且您真的应该成为会员，因为您将获得对 Medium 的无限访问权限，您可以使用我的[推荐链接](https://medium.com/@weiyunna91/membership)注册！

非常感谢您的支持！

## 参考资料

+   [`mlflow.org/docs/latest/model-registry.html`](https://mlflow.org/docs/latest/model-registry.html)

+   [`docs.databricks.com/mlflow/model-registry-webhooks.html`](https://docs.databricks.com/mlflow/model-registry-webhooks.html)

+   [`www.tensorflow.org/tfx/guide/mlmd`](https://www.tensorflow.org/tfx/guide/mlmd)

+   [`docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html`](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html)

+   [`github.com/VertaAI/modeldb`](https://github.com/VertaAI/modeldb)
