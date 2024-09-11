# 使用扩展的 Databricks MLFlow 保障 MLOps 的安全

> 原文：[https://towardsdatascience.com/secure-mlops-with-extended-databricks-mlflow-ee9b7310c5b3?source=collection_archive---------31-----------------------#2023-01-10](https://towardsdatascience.com/secure-mlops-with-extended-databricks-mlflow-ee9b7310c5b3?source=collection_archive---------31-----------------------#2023-01-10)

## 安全管理模型目标环境

[](https://luukvandervelden.medium.com/?source=post_page-----ee9b7310c5b3--------------------------------)[![Luuk van der Velden](../Images/56280e17ccb18454acbb5fb8be94a850.png)](https://luukvandervelden.medium.com/?source=post_page-----ee9b7310c5b3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee9b7310c5b3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ee9b7310c5b3--------------------------------) [Luuk van der Velden](https://luukvandervelden.medium.com/?source=post_page-----ee9b7310c5b3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff6c293b24331&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsecure-mlops-with-extended-databricks-mlflow-ee9b7310c5b3&user=Luuk+van+der+Velden&userId=f6c293b24331&source=post_page-f6c293b24331----ee9b7310c5b3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee9b7310c5b3--------------------------------) ·6 分钟阅读·2023年1月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fee9b7310c5b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsecure-mlops-with-extended-databricks-mlflow-ee9b7310c5b3&user=Luuk+van+der+Velden&userId=f6c293b24331&source=-----ee9b7310c5b3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fee9b7310c5b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsecure-mlops-with-extended-databricks-mlflow-ee9b7310c5b3&source=-----ee9b7310c5b3---------------------bookmark_footer-----------)

MLFlow 是一个开源的 Databricks 产品，支持机器学习模型生命周期的部分功能。它的模型注册表允许将模型版本以模型名称注册，以便于模型部署。我们希望使用一个 MLFlow 实例和一个 Databricks 工作区来支持多个部署目标（接受、预发布、生产等），同时为生产模型提供安全保障。我们扩展了 MLFlow 客户端，以安全的方式在一个模型注册表中管理多个环境。该客户端与 Databricks 权限协同工作，我们将展示 Azure terraform 代码片段。

github 仓库：[环境 MLFlow 客户端存储库](https://github.com/nederlandsespoorwegen/environment_mlflow_client)

![](../Images/48f61f10f7c7dcae556cfa6085454c82.png)

由 [Aleksa Kalajdzic](https://www.pexels.com/photo/mushroom-on-brown-tree-log-3780917/) 拍摄的照片

# 1 为什么选择一个 Databricks 工作区？

安全的 Databricks 工作区部署在虚拟网络中，这要求控制平面和计算平面分别使用不同的子网。计算平面子网的 IP 范围限制了可以同时使用的并行计算节点的最大数量。由于公司范围内的 IP 范围大小限制，我们选择为一个 Databricks 工作区使用一个大的子网，而不是多个较小的子网绑定到多个 Databricks 工作区。因此，我们希望在一个 Databricks 工作区中管理多个逻辑环境（验收、预生产和生产）。

# 1.2 使用 Databricks 池进行扩展

Databricks 池允许在集群创建过程中重用闲置实例（虚拟机）。池提供了适中的启动和自动缩放速度的好处。池中的每个实例需要计算平面子网中的一个 IP。如果达到最大限制，下一个实例创建请求将失败，且没有优雅的错误处理或回退机制。Databricks 池的这个限制使得拥有一个大的子网对于你的活动至关重要，因为你随时可能达到最大限制，从而导致应用程序崩溃。

# 2 基于身份的安全

我们的安全策略基于身份，主要是 Azure 和 Databricks 用户的应用程序注册，背后由 Azure Active Directory (AAD) 用户支持。例如，我们在生产数据的相同 Databricks 工作区中运行验收测试，使用具有只读权限的独立身份，将生产数据复制到临时验收测试存储帐户。身份在 Azure 上非常方便，因为 Databricks 支持凭证透传，利用我们在身份上设置的所有 AAD 角色和组。我们还依赖于 Databricks 权限和组，因为我们在一个 Databricks 工作区中使用多个身份。

# 2.1 Databricks 组

我们的单个 Databricks 工作区中的每个逻辑环境都有一个名为 f”apps_{env_name}”（例如 apps_acc, apps_prod）的 Databricks 组，用于其应用程序主体，以及一个包含所有活动应用程序主体的组“apps_all”。这些组及其权限由 terraform 管理。

Databricks 组 terraform 摘录

# 3 环境 MLFlow 客户端

# 3.1 MLFlow 实验跟踪

MLFlow 实验存储被调整以支持多个逻辑环境，通过按环境管理存储位置。我们的解决方案分配了如“/experiments/acc”和“experiments/prod”的目录来存储实验数据。使用 Databricks 权限管理来授予相应 Databricks 组（在本例中为“apps_acc”和“apps_prod”）目录权限。这允许对每个逻辑环境的实验和模型进行安全日志记录，无需用户考虑。

MLFlow 实验 Databricks 权限 terraform 摘录

# 3.2 MLFlow 模型注册表

MLFlow 模型注册表是一个中央位置，用于注册模型和模型版本以供 ML 系统使用。在不同环境中的数据科学实验中记录和注册的模型最终会进入同一个模型注册表。MLFlow 模型注册表较难调整以支持多个逻辑环境。实际上，MLFlow 中的部署目标概念假设每个特定模型的版本可以具有不同的“阶段”。模型阶段是注册在 MLFlow 模型注册表中的模型版本的一个属性。它可以设置为“None”（无）、“Staging”（暂存）、“Production”（生产）和“Archived”（归档）。尽管有帮助，但模型阶段非常有限，因为我们不能为其定义自己的值，因此它不能完全支持我们定义各种逻辑环境的需求。我们确实使用它来管理与 Databricks 的模型版本权限，我们将在后文中描述。

## 模型命名

我们决定模型命名将作为区分任意数量逻辑环境的第一层。每个注册的模型名称都后缀带有环境标识符 f”{model_name}_{env_name}”。我们的 MLFlow 客户端在模型注册和检索过程中透明地管理这种命名，基于从系统环境变量获取的环境名称或传递给其构造函数的环境名称。与实验管理类似，由于我们在环境 MLFlow 客户端中的抽象，接口与原版 MLFlow 大致相同。

MLFlow 客户端摘录显示环境模型命名

## 模型权限

我们的目标是将生产模型与其他环境分离，并防止任何非生产主体注册或检索生产模型版本。如前所述，模型阶段可以设置为“None”（无）、“Staging”（暂存）、“Production”（生产）和“Archived”（归档）。Databricks 权限管理与模型阶段的值相关联。我们可以控制谁可以设置“Staging”和“Production”模型阶段值，以及谁可以使用“CAN_MANAGE_STAGING_VERSIONS”和“CAN_MANAGE_PRODUCTION_VERSIONS”权限管理具有这些模型阶段值的模型。“Production”阶段权限还允许主体访问“Staging”阶段模型版本并将其过渡到生产模型版本。

为了将生产模型与其他环境中的模型区分开来，我们在注册模型版本时自动分配模型阶段。所有非生产环境将模型阶段值分配为“Staging”。从生产环境注册的模型版本分配“Production”阶段值。我们利用分配给我们 Databricks 组的 Databricks 权限，安全地将我们的生产模型与其他逻辑环境分开管理。

Databricks MLFlow 模型权限 Azure terraform 摘录

# 3.3 汇总

除了对原生 MLFlow 客户端进行的抽象之外，我们还添加了一些封装各种操作的方法以方便使用。其中一个额外的方法是“log_model_helper”，它处理记录和注册模型版本以及设置适当模型阶段的各种步骤。

注册模型版本通常分为两个步骤，首先在实验运行期间记录一个模型，然后将记录的模型注册为已注册模型的模型版本。在实验运行期间记录模型会返回一个 ModelInfo 对象，其中包含一个指向本地工件位置的 model_uri。我们使用 model_uri 将模型版本注册到已注册模型名称下，并将其上传到 MLFlow 注册表。注册模型版本会返回一个 ModelVersion 对象，该对象告诉我们自动递增的模型版本和模型的 current_stage，创建后会始终是“None”。

在模型版本注册期间的第三步是根据逻辑环境将模型版本阶段从“None”过渡到“Staging”或“Production”。每个人都可以创建模型版本，但阶段转换受到 Databricks 权限的限制。这三个步骤都封装在“log_model_helper”方法中。使用这个辅助方法，我们可以假设所有已注册的模型版本都有环境感知的名称和适当的阶段值。

log_model_helper 摘录

# 3.4 测试我们的 MLFlow 客户端

为了维护我们自己的客户端，我们需要详细测试它，以检查是否符合我们的权限设计。对上游 MLFlow 客户端的任何更改也会同时进行测试，例如 MLFlow 模块的重构。我们在一个 PyTest 固定装置内启动一个本地 MLFlow 服务器，该装置的作用范围是整个测试会话。使用 Python tempfile 模块生成一个临时工件位置和一个临时 sqlite 数据库文件。我们测试会话的目标是记录和注册模型，并对其进行各种变更和检索。第一步是在空模型注册表中记录和注册一个模型版本。我们在 PyTest 固定装置内执行此操作，因为所有其他测试都依赖于它，尽管从技术上讲，它本身就是一个测试，因为它包括断言。

MLFlow 测试服务器固定装置，感谢 @Wessel Radstok 和 [Rik Jongerius](https://medium.com/u/8d3b69256f17?source=post_page-----ee9b7310c5b3--------------------------------)

模型注册的测试夹具

# 结论

我们扩展的环境 MLFlow 客户端允许我们将多个逻辑环境中的模型注册到同一个模型注册表中。它利用 Databricks 中的最小权限选项，在模型注册和部署检索期间安全地将开发环境与生产环境分开。此外，MLFlow API 在各环境中基本保持不变。

*最初发布于* [*https://codebeez.nl*](https://codebeez.nl/blogs/secure-mlops-with-databricks-mlflow/)*。*
