# 公有云上的机器学习工具层次结构

> 原文：[https://towardsdatascience.com/the-hierarchy-of-ml-tooling-on-the-public-cloud-ed387cac3c27?source=collection_archive---------11-----------------------#2023-03-21](https://towardsdatascience.com/the-hierarchy-of-ml-tooling-on-the-public-cloud-ed387cac3c27?source=collection_archive---------11-----------------------#2023-03-21)

## 并非所有机器学习服务都是一样的

[](https://natworkeffects.medium.com/?source=post_page-----ed387cac3c27--------------------------------)[![Nathan Cheng](../Images/d7477ae4d208a039ab75e402e1abe3ce.png)](https://natworkeffects.medium.com/?source=post_page-----ed387cac3c27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ed387cac3c27--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ed387cac3c27--------------------------------) [Nathan Cheng](https://natworkeffects.medium.com/?source=post_page-----ed387cac3c27--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dbd159f0e0a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hierarchy-of-ml-tooling-on-the-public-cloud-ed387cac3c27&user=Nathan+Cheng&userId=9dbd159f0e0a&source=post_page-9dbd159f0e0a----ed387cac3c27---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ed387cac3c27--------------------------------) · 8分钟阅读·2023年3月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fed387cac3c27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hierarchy-of-ml-tooling-on-the-public-cloud-ed387cac3c27&user=Nathan+Cheng&userId=9dbd159f0e0a&source=-----ed387cac3c27---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fed387cac3c27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-hierarchy-of-ml-tooling-on-the-public-cloud-ed387cac3c27&source=-----ed387cac3c27---------------------bookmark_footer-----------)![](../Images/20c61a3cb7e2b3b7579d4ddcfdaddb05.png)

机器学习系统中的隐藏技术债务。图片来自[Google Developers](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning)。

# # 1 公有云上的机器学习服务

并非所有机器学习服务都是一样的。作为一名在公有云上工作的顾问，我可以告诉你，人工智能（AI）/机器学习（ML）工具在三大公有云——Azure、AWS 和 GCP——上可供选择的范围广泛。

处理和综合信息的潮流可能让人感到不知所措，尤其是当这些服务不断推出新功能时。

想象一下，向外行解释选择哪个平台，并说明为什么选择这个特定工具来解决你的机器学习问题，将会是多么棘手的事情。

我写这篇文章是为了帮助其他人以及我自己缓解这个问题，以便你能清晰简洁地了解公共云提供了什么。为了简化起见，我将在整个文章中将 AI 和 ML 交替使用。

# # 2 建立定制机器学习系统……应该是最后的选择

在我们深入工具比较之前，先来了解一下为什么我们应该使用公共云上的托管服务。这个问题是合理的，值得质疑——为什么不从头开始构建自己的定制基础设施和机器学习模型？为了回答这个问题，我们来快速看看机器学习的生命周期。

下面的图示展示了一个典型的机器学习生命周期（这个循环是迭代的）：

![](../Images/44aace5540cdfe6ec4d2ae291f2d783e.png)

机器学习生命周期。图片来源：[作者](https://natworkeffects.medium.com)。

正如你所看到的，整个生命周期有很多部分需要考虑。

[Google 发表的一篇著名论文](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf)显示，在构建可维护的生产环境中的机器学习模型时，投入的少量精力是编写模型训练代码。

这种现象被称为生产环境中机器学习系统的隐性技术债务，也被业内称为机器学习运维（MLOps），这是一个涵盖上述技术债务的统称。

下面是一个支持上述统计数据的视觉解释，[改编自 Google 的论文](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning)：

![](../Images/c3e181b30f72df8c61a308649ccb8919.png)

机器学习系统中的隐性技术债务。图片来源：[Google 开发者](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning)。

我不会详细解释生命周期中的每个阶段，但这里有一个总结的定义列表。如果你有兴趣了解更多，我推荐阅读[《机器学习设计模式》](https://www.oreilly.com/library/view/machine-learning-design/9781098115777/)第9章关于机器学习生命周期和 AI 准备性的详细解答。

**机器学习生命周期总结定义：**

1.  **数据预处理** —— 为机器学习训练准备数据；数据管道工程

1.  **特征工程** —— 将输入数据转化为与机器学习模型学习目标密切对齐的新特征

1.  **模型训练** —— 训练和初步验证机器学习模型；迭代算法，进行训练/测试分割，执行超参数调优

1.  **模型评估** —— 根据预定的评估指标评估模型性能

1.  **模型版本控制** — 模型工件的版本控制；模型训练参数、模型管道

1.  **模型服务** — 通过批处理或实时推理提供模型预测

1.  **模型部署** — 自动构建、测试、部署到生产和模型重新训练

1.  **模型监控** — 监控基础设施、输入数据质量和模型预测

## **不要忘记平台基础设施和安全性！**

ML生命周期不考虑支持平台基础设施，这些基础设施必须在加密、网络和身份与访问管理（IAM）方面保持安全。

云服务提供托管计算基础设施、开发环境、集中IAM、加密功能和网络保护服务，这些可以实现与内部IT政策的安全合规 —— 因此你真的不应该自己构建这些ML服务，而是利用云的力量将ML能力融入你的产品路线图。

本节说明了编写模型训练代码只是整个ML生命周期中的一个相对微小的部分，实际的数据准备、评估、部署和生产中的ML模型监控都非常困难。

自然地，结论是构建自己定制的基础设施和ML模型需要相当的时间和精力，这样做的决定应当是最后的手段。

# # 3 ML工具层级

这里是利用公共云服务来填补空白的地方。这些超大规模提供商主要提供两种服务；**ML工具层级：**

+   🧰 **AI服务。或者：**

1.  🔨 `**预训练标准**` - 仅使用基础模型，无法通过带来自己的训练数据进行自定义。

1.  ⚒️ `**预训练可定制**` - 可以使用基础模型，且通过带来自己的训练数据进行可选定制。

1.  ⚙️ `**带来自己的数据**` - 强制要求带来自己的训练数据。

+   🪛 **ML平台。**

## 荣誉AI服务提及

对于那些阅读此帖的更敏锐的读者，我故意省略了一些在层次结构中的荣誉AI服务提及：

+   内置ML模型的数据仓库，通过SQL语法实现ML开发。进一步阅读可以参考 [**BigQuery ML**](https://cloud.google.com/bigquery-ml/docs/introduction), [**Redshift ML**](https://docs.aws.amazon.com/redshift/latest/dg/machine_learning.html), 和 [**Synapse专用SQL池PREDICT函数**](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-predict)。这些服务旨在供数据分析师使用，前提是你的数据已在云数据仓库中。

+   [**AI Builder**](https://learn.microsoft.com/en-us/ai-builder/overview) 适用于 Microsoft Power Platform，以及 [**Amazon SageMaker Canvas**](https://docs.aws.amazon.com/sagemaker/latest/dg/canvas.html)。这些服务旨在供非技术性业务用户即公民数据科学家使用。

+   [**Azure OpenAI**](https://learn.microsoft.com/en-us/azure/cognitive-services/openai/overview) 是一个新兴服务，由微软监管；你需要申请试用批准。

# # 3.1 ML 平台 🪛

我们将首先讨论 ML 平台，然后讨论 AI 服务。该平台提供 MLOps 所需的辅助工具。

每个公共云都有自己版本的 ML 平台：

+   Azure — [Azure Machine Learning](https://learn.microsoft.com/en-us/azure/machine-learning/overview-what-is-azure-machine-learning)

+   AWS — [Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis-features-alpha.html)

+   GCP — [Vertex AI](https://cloud.google.com/vertex-ai/docs/start/introduction-unified-platform)

## 它适合谁？

就角色而言，这是为拥有内部数据科学家资源的团队准备的，他们希望使用自己的训练数据构建自定义的最先进（SOTA）模型，并开发框架以在 ML 生命周期中进行自定义 MLOps 管理。

## 我该如何使用它？

从需求角度来看，业务用例需要他们设计一个自定义 ML 模型实现，而 AI 服务在 **3.2 节** 中无法满足这些能力。

在寻找公共云上的服务时，这不应该是你的首选。

即使有了 ML 平台，仍然需要投入大量时间和精力来学习 ML 平台的功能，并编写代码以使用超大规模软件开发工具包（SDKs）构建自定义 MLOps 框架。

相反，请首先查看下一节 **3.2** 中的 AI 服务，看是否能满足你的需求。

## 该服务提供了哪些技术能力？

当你使用云平台时，你可以获得一个完全由超大规模管理的环境，否则你可能会为此抓狂：

1.  **托管计算基础设施** — 这些是具有默认环境的机器集群，包含普遍存在的内置 ML 库和云原生 SDK。计算资源可用于分布式训练，或为批量和实时预测提供模型端点。

1.  **托管开发环境** — 以笔记本的形式提供，或者通过你选择的 IDE，只要它与 ML 平台集成。

这些工具使数据科学家和 ML 工程师能够完全专注于 ML 生命周期，而不是基础设施配置和依赖管理。

内置库和云原生 SDK 使数据科学家能够编写自定义代码，在整个 ML 生命周期中实现更无缝的工程。

**下表显示了每个云 ML 平台的技术特性：**

ML 平台比较表。Gist 由 [作者](https://natworkeffects.medium.com) 提供。

# # 3.2 AI 服务 🧰

接下来，我们将讨论 AI 服务。

它们使用低代码/无代码的方法来促进 ML 开发，并减轻管理 MLOps 的开销。

Jeff Atwood 对这些服务的总体论点如下：

> **最好的代码是没有代码。**
> 
> **每一行你自愿编写的代码，都是需要调试的代码，需要阅读和理解的代码，需要支持的代码。每次你编写新代码时，都应该是勉强的，在压力下，因为你已经完全耗尽了所有其他选项。**

## 这适合谁？

就人员而言，这些适用于**没有以下资源的团队：**

1.  内部数据科学家资源。

1.  使用自己的训练数据训练自定义 ML 模型。

1.  投资资源、精力和时间来端到端地工程化一个自定义 ML 模型。

## 我该如何使用？

从需求角度来看，ML 业务应用场景可以通过云提供商的 AI 服务能力来满足。

目标是通过利用大规模服务商的基础模型和训练数据将 ML 功能添加到产品中；因此，团队可以优先开发核心应用程序，通过从 API 端点检索预测结果与 AI 服务集成，并最终在模型训练和 MLOps 上花费最少的精力。

## 该服务提供了什么技术能力？

我们将根据 AI 服务提供的技术能力组织以下比较表。这与 ML 业务应用场景密切相关，但应有所区分。

例如，Amazon Comprehend 服务提供了***文本分类能力***。该能力用于构建***业务应用场景***模型，如：

1.  **客户评论的情感分析。**

1.  **内容质量审核。**

1.  **将多类项分类到自定义定义的类别中。**

对于某些 AI 服务，技术能力和业务应用场景完全相同；在这种情况下，AI 服务是为解决该特定 ML 业务应用场景而构建的。

## **行业特定版本的 AI 服务**

请注意，我已经排除了或避免提及行业特定版本的 AI 服务。只需知道，大规模服务商训练模型以在这些领域实现更高的模型性能，您应该在特定行业或领域使用它们，而不是通用版本的服务。

这些服务的显著提及包括 [**Amazon Comprehend Medical**](https://docs.aws.amazon.com/comprehend-medical/latest/dev/comprehendmedical-welcome.html)、[**Amazon HealthLake**](https://docs.aws.amazon.com/healthlake/latest/devguide/what-is-amazon-health-lake.html)、**Amazon Lookout for**`**{domain}**`、[**Amazon Transcribe Call Analytics**](https://docs.aws.amazon.com/transcribe/latest/dg/call-analytics.html)、[**Google Cloud Retail Search**](https://cloud.google.com/retail/docs/overview) 等。

**以下图例和表格展示了每个云 AI 服务的技术能力：**

+   🔨 `**预训练标准**` - 仅使用基础模型，无选项通过自定义训练数据进行定制。

+   ⚒️ `**预训练可定制**` - 可以使用基础模型，并通过提供自己的训练数据进行可选的定制。

+   ⚙️ `**自带数据**` - 必须提供自己的训练数据。

`**--- 语音 ---**`

语音 AI 比较表。摘要由 [作者](https://natworkeffects.medium.com) 提供。

`**--- 自然语言 ---**`

自然语言 AI 比较表。摘要由 [作者](https://natworkeffects.medium.com) 提供。

`**--- 视觉 ---**`

视觉 AI 比较表。摘要由 [作者](https://natworkeffects.medium.com) 提供。

`**--- 决策 ---**`

决策 AI 比较表。摘要由 [作者](https://natworkeffects.medium.com) 提供。

`**--- 搜索 ---**`

搜索 AI 比较表。摘要由 [作者](https://natworkeffects.medium.com) 提供。

# # 4 个进一步阅读的主题

在这篇文章中，我们已经涵盖了公共云提供的 ML 服务的广泛内容，但在构建 ML 系统时，还有其他概念需要考虑。

我鼓励你探索并找到你自己的答案，尤其是那些未被讨论的概念，因为 AI / ML 正在越来越深入地融入我们使用的产品中。

**三个公共云提供哪些 ML 工具以实现以下功能？**

+   模型数据血统和来源

+   模型目录

+   预测后地面真实标签的人工审核

+   处理视频数据的模型

+   执行通用回归和分类的模型

# 特别感谢 / 参考资料

> *特别感谢以下资源的作者和创作者，他们帮助我撰写了这篇文章：*

**ML 工具**

+   📚 [选择你在 Google Cloud 上的 AI / ML 路径](https://cloud.google.com/blog/topics/developers-practitioners/pick-your-aiml-path-google-cloud)

**AI 服务**

+   📚 [什么是 Azure 机器学习？](https://learn.microsoft.com/en-us/azure/machine-learning/overview-what-is-azure-machine-learning)

+   📚 [Azure 机器学习的工作原理：架构和概念](https://learn.microsoft.com/en-us/azure/machine-learning/v1/concept-azure-machine-learning-architecture)

+   📚 [Amazon SageMaker 特性](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis-features-alpha.html)

+   📚 [Vertex AI](https://cloud.google.com/vertex-ai)

+   📚 [Vertex AI 文档](https://cloud.google.com/vertex-ai/docs/start/introduction-unified-platform)

**ML 平台**

+   📚 [选择 Microsoft 认知服务技术](https://learn.microsoft.com/en-us/azure/architecture/data-guide/technology-choices/cognitive-services)

+   📚 [Azure 认知服务文档](https://learn.microsoft.com/en-us/azure/cognitive-services)

+   📚 [什么是 Azure AutoML？](https://learn.microsoft.com/en-us/azure/machine-learning/concept-automated-ml)

+   🎦 [Azure OpenAI 用例](https://www.youtube.com/watch?v=YKY0V42MCtk)

+   📚 [AWS 机器学习服务](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/machine-learning.html)

+   📚 [AWS AutoML 解决方案](https://aws.amazon.com/machine-learning/automl)

+   📚 [Google AI 和机器学习产品](https://cloud.google.com/products/ai)

+   📚 [Google AI 和机器学习解决方案](https://cloud.google.com/solutions/ai)

+   📚 [Google Cloud AutoML](https://cloud.google.com/automl/docs)
