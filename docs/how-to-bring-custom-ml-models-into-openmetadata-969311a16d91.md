# 如何将自定义 ML 模型引入 OpenMetadata

> 原文：[https://towardsdatascience.com/how-to-bring-custom-ml-models-into-openmetadata-969311a16d91?source=collection_archive---------10-----------------------#2023-01-09](https://towardsdatascience.com/how-to-bring-custom-ml-models-into-openmetadata-969311a16d91?source=collection_archive---------10-----------------------#2023-01-09)

## 构建自定义 CICD 管道，以便将您的 ML 资产展现出来

[](https://blog.metadata.coffee/?source=post_page-----969311a16d91--------------------------------)[![Pere Miquel Brull](../Images/2641b6f44310747aa3150fb720d8ff3f.png)](https://blog.metadata.coffee/?source=post_page-----969311a16d91--------------------------------)[](https://towardsdatascience.com/?source=post_page-----969311a16d91--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----969311a16d91--------------------------------) [Pere Miquel Brull](https://blog.metadata.coffee/?source=post_page-----969311a16d91--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5d3218cd196e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-bring-custom-ml-models-into-openmetadata-969311a16d91&user=Pere+Miquel+Brull&userId=5d3218cd196e&source=post_page-5d3218cd196e----969311a16d91---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----969311a16d91--------------------------------) ·5 min read·2023年1月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F969311a16d91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-bring-custom-ml-models-into-openmetadata-969311a16d91&user=Pere+Miquel+Brull&userId=5d3218cd196e&source=-----969311a16d91---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F969311a16d91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-bring-custom-ml-models-into-openmetadata-969311a16d91&source=-----969311a16d91---------------------bookmark_footer-----------)

[OpenMetadata](https://open-metadata.org/) 不仅仅是一个数据目录。基于标准定义和 API，这个目录只是众多利用您平台的**元数据**的应用程序之一。从一开始，OpenMetadata 的目标就是解决行业中的元数据问题。团队无需解决诸如元数据摄取或如何将**协作**带回数据中的关键组件，可以专注于改进他们的流程和自动化。

本文旨在展示我们如何集成多个元数据源，无论是来自现有服务还是**内部解决方案**。由于每个操作都是通过API驱动的，来自Postgres等特定连接器的元数据与通过**Python SDK**发送的元数据没有区别。这种高灵活性使我们能够探索来自自定义构建的机器学习模型及其特征源的元数据。

![](../Images/45ff0ea15bc22feba2315fb7d8d374e3.png)

Postgres和自定义机器学习模型摄取模式。图片来源于作者。

如果这听起来有趣，请按照这个[仓库](https://github.com/open-metadata/openmetadata-demo/tree/main/mlmodel-cicd)中的材料进行操作。

# OpenMetadata与机器学习

机器学习模型生命周期的主要**挑战**之一是弥合机器学习模型与数据平台之间的差距。我们有助于训练、测试、调整和部署机器学习的工具，但这些工具很少将机器学习模型放在其存在的平台的背景下。

如何将所有部分整合在一起的信息通常由数据科学家或机器学习工程师掌握，但很少共享。典型原因有：

1.  **没有通用的方法**来定义和维护元数据。

1.  **没有中央位置**来发布结果供用户探索。

1.  这种缺乏明确性以及前两个任务所涉及的工作使得很难证明**好处**并衡量**影响**。

在本演示中，我们将跟随一个用例，其中：

1.  我们有一个使用Postgres特征的机器学习模型，

1.  模型会定期更新和部署，

1.  模型的文档作为代码托管，

1.  我们将使用OpenMetadata的Python SDK创建机器学习模型资产并将其推送到OpenMetadata中。

将我们的模型纳入OpenMetadata可以帮助我们分享文档，跟踪元数据的变化和版本控制，发现源的血缘关系，推动讨论和合作……从将这种整体视角引入机器学习和人工智能资产中获得的一些快速成果包括：

+   团队可以快速开始**协作**，而不是尝试以不同的方式达到类似的结果，

+   收集最常用特征的知识，以开始构建具有最高价值的**特征存储**，服务于整个组织。

+   机器学习团队可以直接在OpenMetadata中开始构建**数据质量测试和警报**，以防止特征漂移和性能下降。

# 摄取Postgres元数据

第一步将是获取Postgres元数据，因为我们在这里有机器学习特征的来源。你可以按照这些[步骤](https://docs.open-metadata.org/connectors/database/postgres)配置和部署Postgres元数据摄取。

![](../Images/6bd02ac8c663e3676c6a36e1d986362b.png)

在OpenMetadata中创建Postgres服务。图片来源于作者。

OpenMetadata用户界面将引导我们完成两个主要步骤：

1.  创建**数据库服务**：服务表示我们要摄取的源系统。在这里我们将定义与Postgres的连接，这个服务将持有将发送到OpenMetadata的资产：数据库、模式和表。

1.  创建和部署**数据摄取管道**：这些管道由OpenMetadata使用[摄取框架](https://pypi.org/project/openmetadata-ingestion/)内部处理，该Python库包含连接到多个源、将其原始元数据转换为OpenMetadata标准并通过[API](https://docs.open-metadata.org/swagger.html)发送到服务器的逻辑。

![](../Images/2cad6199c7f4f7cae309642254fd4e06.png)

在OpenMetadata中管理数据摄取管道。图像由作者提供。

有趣的是，摄取框架包可以直接用于配置和托管摄取流程。此外，UI或摄取框架中的任何操作完全开放，并得到服务器API的支持。这意味着任何与元数据相关的活动都可以实现全面自动化，这可以通过REST或[OpenMetadata SDKs](https://docs.open-metadata.org/sdk)直接实现。

这些是我们接下来在创建CICD流程时将利用的功能。

# 构建CICD管道

在上述讨论中，我们强调了通常成为维护更新的机器学习模型元数据阻碍的两个问题：没有通用的元数据定义和没有一个地方来发布它。幸运的是，OpenMetadata解决了这两个问题。

构建成功流程的缺失环节？它应该是**简单**维护和演变的。这就是为什么我们将示例基于一个[YAML 文件](https://github.com/open-metadata/openmetadata-demo/blob/main/mlmodel-cicd/ml_model.yaml)，该文件在代码库中检查出来。因此，数据科学家和机器学习工程师可以依靠他们的部署管道来更新其最新生产模型的元数据。

![](../Images/66394af9fb03e282e9097154839540fa.png)

包含机器学习模型元数据的示例YAML文件。图像由作者提供。

CICD流程将有一个特定步骤：

1.  阅读包含元数据的YAML文件，

1.  将YAML的结构转换为[ML模型](https://docs.open-metadata.org/main-concepts/metadata-standard/schemas/entity/data/mlmodel)实体定义，以符合OpenMetadata标准，

1.  使用Python SDK将机器学习模型资产推送到OpenMetadata。

[这里](https://github.com/open-metadata/openmetadata-demo/blob/main/mlmodel-cicd/mlmodel_cicd.py)你会找到这样的管道示例。希望这能帮助你开始将你的机器学习资产纳入视野！

![](../Images/86bbd7a379354568c95892e335d83fc7.png)

示例CICD管道。GIF由作者提供。

脚本运行完成后，我们将在OpenMetadata中看到我们的收入预测模型：

![](../Images/769179cb8842d8f0b811ddd353399505.png)

OpenMetadata中的收入预测模型。图像由作者提供。

在平台上可用的一个关键好处是能够查看我们模型与包含特征的数据源之间的血统信息。在我们的示例中，我们已经获取了Postgres元数据。然后，如果我们查看血统标签页，我们将能够看到我们所有模型的依赖关系。

![](../Images/fb56bb06bf9d02cb3e9bfcab601a0982.png)

收入预测的血统与Postgres表格。作者提供的图片。

# 总结

在这篇文章中，我们：

+   讨论了行业对定义、获取和利用元数据的共同方法的需求，以及OpenMetadata如何满足这些需求。

+   从≈ UI直接获取了Postgres元数据。

+   构建了一个CICD过程，在发布过程中推送自定义构建的ML模型元数据。

将ML模型置于[上下文](https://medium.com/openmetadata/ml-is-not-just-about-ml-c08eab242e84)中具有重要的好处，比如探索依赖关系和促进协作。如果你需要一种简单的方法来展示你的ML资产，OpenMetadata可以满足你的需求。
