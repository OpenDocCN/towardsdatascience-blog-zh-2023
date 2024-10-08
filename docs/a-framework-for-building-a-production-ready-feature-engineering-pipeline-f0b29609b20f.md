# 构建生产就绪特征工程管道的框架

> 原文：[`towardsdatascience.com/a-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f`](https://towardsdatascience.com/a-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f)

## [全栈 7 步 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 课程 1: 批量服务。特征存储。特征工程管道。

[](https://pauliusztin.medium.com/?source=post_page-----f0b29609b20f--------------------------------)![Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----f0b29609b20f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0b29609b20f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0b29609b20f--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----f0b29609b20f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0b29609b20f--------------------------------) ·13 分钟阅读·2023 年 4 月 28 日

--

![](img/2ae381d9f40ec629b5dacf7b06536a4e.png)

[Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

本教程代表**一个包含 7 课时的课程中的第 1 课**，将逐步指导你如何**设计、实施和部署 ML 系统**，使用**MLOps 优良实践**。在课程中，你将构建一个生产就绪的模型，用于预测丹麦未来 24 小时内的能源消耗水平，涵盖多个消费类型。

*完成本课程后，你将了解使用批量服务架构设计、编码和部署 ML 系统的所有基本知识。*

本课程*针对中级/高级机器学习工程师*，旨在通过构建自己的端到端项目来提升技能。

> 如今，证书随处可见。构建先进的端到端项目并展示是获得专业工程师认可的最佳途径。

# 目录：

+   课程介绍

+   课程内容

+   数据源

+   课程 1: 批量服务。特征存储。特征工程管道。

+   课程 1: 代码

+   结论

+   参考文献

# 介绍

***在这 7 课时的课程结束时，你将学会如何:***

+   设计批量服务架构

+   使用 Hopsworks 作为特征存储

+   设计一个从 API 读取数据的特征工程管道

+   构建带有超参数调优的训练管道

+   使用 W&B 作为 ML 平台来跟踪你的实验、模型和元数据

+   实现批量预测管道

+   使用 Poetry 构建自己的 Python 包

+   部署自己的私人 PyPi 服务器

+   使用 Airflow 协调一切

+   使用预测结果编码一个使用 FastAPI 和 Streamlit 的 Web 应用

+   使用 Docker 容器化你的代码

+   使用 Great Expectations 确保数据验证和完整性

+   监控预测性能的变化

+   将所有内容部署到 GCP

+   使用 GitHub Actions 构建 CI/CD 流水线

如果这些听起来很多，不用担心，完成本课程后你将理解我之前说的一切。最重要的是，你将了解我为何使用这些工具以及它们如何作为一个系统协同工作。

**如果你想最大化本课程的收益，** [**我建议你访问包含所有课程代码的 GitHub 仓库**](https://github.com/iusztinpaul/energy-forecasting) **。我设计了这些文章，使你在阅读课程的同时可以阅读并运行代码。**

到课程结束时，你将学会如何实现下面的图示。如果有些内容对你来说不太明白，不用担心。我会详细解释一切。

![](img/4b5c3b0b8e2162ea8fd268ca745199ec.png)

课程中你将构建的架构图 [图示来源于作者]。

***为什么批量服务？***

模型的部署主要有 4 种类型：

+   批量服务

+   request-response

+   流式处理

+   嵌入式

批量服务是获取实际操作经验的绝佳起点，因为大多数 AI 应用程序从使用批量架构开始，然后转向请求响应或流式处理。

# 课程内容：

1.  **批量服务。特征存储。特征工程流水线。**

1.  [训练流水线。ML 平台。超参数调整。](https://medium.com/towards-data-science/a-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee)

1.  [批量预测流水线。使用 Poetry 打包 Python 模块。](https://medium.com/towards-data-science/unlock-the-secret-to-efficient-batch-prediction-pipelines-using-python-a-feature-store-and-gcs-17a1462ca489)

1.  [私人 PyPi 服务器。使用 Airflow 协调一切。](https://medium.com/towards-data-science/unlocking-mlops-using-airflow-a-comprehensive-guide-to-ml-system-orchestration-880aa9be8cff)

1.  使用 GE 进行数据验证以确保质量和完整性。模型性能持续监控。

1.  [使用 FastAPI 和 Streamlit 消费和可视化你的模型预测。将一切容器化。](https://medium.com/towards-data-science/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243)

1.  [将所有 ML 组件部署到 GCP。使用 Github Actions 构建 CI/CD 流水线。](https://medium.com/towards-data-science/seamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012)

1.  [[附加] ‘不完美’ ML 项目的幕后——教训与见解](https://medium.com/towards-data-science/imperfections-unveiled-the-intriguing-reality-behind-our-mlops-course-creation-6ff7d52ecb7e)

# 数据源：

我们使用了一个开放 API，提供丹麦所有能源消费者类型的每小时能源消耗值。

他们提供了一个直观的界面，你可以轻松查询和可视化数据。[你可以在这里访问数据](https://www.energidataservice.dk/tso-electricity/ConsumptionDE35Hour) [1]。

数据有 4 个主要属性：

+   **小时 UTC：** 观察到数据点时的 UTC 日期时间。

+   **价格区域：** 丹麦被划分为两个价格区域：DK1 和 DK2——由大贝尔特海峡分隔。DK1 位于大贝尔特的西侧，DK2 位于东侧。

+   **消费者类型：** 消费者类型为工业代码 DE35，由丹麦能源公司拥有和维护。

+   **总消耗：** 总电力消耗（kWh）

**注意：** 观察值有 15 天的滞后！但对于我们的演示用例，这不是问题，因为我们可以模拟与实时相同的步骤。

![](img/e0bc098121320b6b981889d8d712952d.png)

应用程序中的截图展示了我们如何预测区域 = 1 和消费者类型 = 212 的能源消耗 [作者提供的图片]。

数据点具有每小时的分辨率。例如：“2023–04–15 21:00Z”，“2023–04–15 20:00Z”，“2023–04–15 19:00Z”等等。

我们将把数据建模为多个时间序列。每个独特的**价格区域**和**消费者类型**元组表示其独特的时间序列。

因此，我们将构建一个模型，独立预测每个时间序列接下来 24 小时的能源消耗。

*查看下面的视频，更好地理解数据的样子* 👇

课程与数据源概览 [作者提供的视频]。

# 第 1 课：批量服务。特征存储。特征工程管道。

## 第 1 课的目标

在第 1 课中，我们将关注图中蓝色突出显示的组件：“API”，“特征工程”和“特征存储”。

![](img/f8a16b0c5164f20ee8c313126196f321.png)

最终架构的示意图，其中第 1 课的组件用蓝色突出显示 [作者提供的图片]。

具体来说，我们将构建一个 ETL 管道，从能源消耗 API 中提取数据，经过特征工程管道，该管道清洗和转换特征，并将特征加载到特征存储中，以便在系统中进一步使用。

如你所见，特征存储站在系统的核心位置。

## 理论概念与工具

**批量服务：** 在批量服务模式中，你可以离线准备数据、训练模型并进行预测。之后，你将预测结果存储在数据库中，客户端/应用程序将在后续使用这些预测结果。**批量**这个词来源于你可以同时处理多个样本，这在这种模式下通常是有效的。我们计算了所有预测结果并将其存储在 blob 存储/桶中。

如果我们将架构过于简化，仅反映批量架构的主要步骤，它将如下所示 👇

![](img/8a22ababb6bd63765cd79be166f81414.png)

批处理架构 [作者提供的图片]。

批处理服务范式的最大缺点是你的预测几乎总是会滞后。例如，在我们的案例中，我们预测未来 24 小时的能耗，由于这种滞后，我们的预测可能会迟到 1 小时。

[查看这篇文章](https://medium.com/mlearning-ai/this-is-what-you-need-to-know-to-build-an-mlops-end-to-end-architecture-c0be1deaa3ce)以了解更多关于*Google Cloud* *建议的* *标准化架构*，这在几乎任何机器学习系统中都可以利用。

**特征存储：**特征存储位于任何机器学习系统的核心。使用特征存储，你可以轻松地存储和共享系统中的特征。你可以直观地将特征存储视为一个高级数据库，增加以下功能：

+   数据版本控制和血缘

+   数据验证

+   创建数据集的能力

+   保存训练/验证/测试拆分的能力

+   两种存储类型：离线（便宜，但延迟高）和在线（更贵，但延迟低）。

+   时间旅行：在给定时间窗口内轻松访问数据

+   除了特征本身外，还保存特征转换

+   数据监控等……

如果你想阅读关于特征存储的内容，[请查看这篇文章](https://www.kdnuggets.com/2020/12/feature-store-vs-data-warehouse.html) [3]。

我们选择了[Hopsworks](https://www.hopsworks.ai/)作为我们的特征存储，因为它是无服务器的，并提供了慷慨的免费计划，这足以创建本课程。

此外，Hopsworks 设计非常优秀，并提供了上述所有功能。如果你在寻找无服务器的特征存储，我推荐他们。

如果你还想在阅读本课程时运行代码，你需要去[Hopswork](https://www.hopsworks.ai/)，创建一个账户和项目。所有其他步骤将在课程的其余部分中解释。

我确保了本课程中的所有步骤都能保留在他们的免费计划中。因此，它不会花费你任何$$$。

**特征工程管道：**读取来自一个或多个数据源的数据，清洗、转换、验证数据并将其加载到特征存储中的代码片段（基本上是 ETL 管道）。

**Pandas vs. Spark：**我们在本课程中选择使用 Pandas 作为数据处理库，因为数据较小。因此，它可以轻松地适应计算机的内存，使用如 Spark 这样的分布式计算框架会使一切变得过于复杂。但在许多现实世界的场景中，当数据太大无法适应单台计算机（即大数据）时，你将使用 Spark（或其他分布式计算工具）来完成与本课程相同的步骤。[查看这篇文章](https://pub.towardsai.net/this-is-how-you-can-build-a-churn-prediction-model-using-spark-e187b7eca339)以了解 Spark 如何处理大数据预测流失。

# 课程 1: 代码

[你可以在这里访问 GitHub 仓库。](https://github.com/iusztinpaul/energy-forecasting)

**注意：** 所有安装说明都在仓库的 README 文件中。这里我们将直接进入代码部分。

*Lesson 1 中的所有代码都位于* [***feature-pipeline***](https://github.com/iusztinpaul/energy-forecasting/tree/main/feature-pipeline)*文件夹下。*

[**feature-pipeline**](https://github.com/iusztinpaul/energy-forecasting/tree/main/feature-pipeline)文件夹下的文件结构如下：

![](img/a00200e79a2ce7bf64f173db2f6975fc.png)

显示 feature-pipeline 文件夹结构的截图[作者提供]。

所有代码都位于[**feature_pipeline**](https://github.com/iusztinpaul/energy-forecasting/tree/main/feature-pipeline/feature_pipeline)目录下（注意是"_"而不是"-"）**。**

## ***准备凭证***

在本课程中，你将使用一个单一服务作为你的特征存储：[Hopsworks](https://www.hopsworks.ai/)（在我们的用例中，它将是*免费的*）。

在[Hopsworks](https://www.hopsworks.ai/)上创建一个账户和一个新项目（或使用默认项目）。注意不要将你的项目命名为“**energy_consumption**”，因为 Hopsworks 要求在其无服务器部署中项目名称唯一。

现在，你需要一个来自[Hopsworks](https://www.hopsworks.ai/)的**API_KEY**来登录并使用他们的 Python 模块访问云资源。

直接在你的 git 仓库中存储凭证是一个巨大的安全隐患。因此，你将使用**.env**文件注入敏感信息。**.env.default**是你必须配置的所有变量的示例。

![](img/a925923b5eecad13761347d1f873293c.png)

**.env.default**文件的截图[作者提供]。

从你的**feature-pipeline**目录中，在终端中运行：

```py
cp .env.default .env
```

…并在**FS_API_KEY**变量下填写你新生成的 Hopsworks API KEY，在**FS_PROJECT_NAME**变量下填写你的 Hopsworks 项目名称（在我们的例子中，它是*“energy_consumption”*）。

***查看下图，了解如何获取你自己的 Hopsworks API KEY 👇***

![](img/3eeba288d913985db231e149fe5148ff.png)

进入你的[Hopsworks](https://www.hopsworks.ai/)项目。然后，在右上角点击你的用户名，再点击“账户设置”。最后，点击“新建 API KEY”，设置一个名称，选择所有作用域，点击“创建 API KEY”，复制 API KEY，你就完成了。你已经拥有了 Hopsworks API KEY[作者提供]。

然后，在[**feature_pipeline/settings.py**](https://github.com/iusztinpaul/energy-forecasting/blob/main/feature-pipeline/feature_pipeline/settings.py)文件中，我们将使用老牌的**dotenv** Python 包从**.env**文件中加载所有变量。

如果你想从当前目录以外的地方加载**.env**文件，你可以在运行脚本时导出**ML_PIPELINE_ROOT_DIR**环境变量。这是一个指向其余配置文件的"HOME"环境变量。

我们还将使用**ML_PIPELINE_ROOT_DIR**环境变量来指向一个单一目录，从中加载**.env**文件，并在所有进程中读写数据。

这是一个如何使用**ML_PIPELINE_ROOT_DIR**变量的示例：

```py
export ML_PIPELINE_ROOT_DIR=/my/awesome/path python -m feature_pipeline.pipeline
```

使用以下代码，我们将通过 SETTINGS 字典访问代码中的所有凭据/敏感信息。

## ***ETL 代码***

在 [**feature_pipeline/pipeline.py**](https://github.com/iusztinpaul/energy-forecasting/blob/main/feature-pipeline/feature_pipeline/pipeline.py) 文件中，我们在**run()**方法下有管道的主要入口点。

如下所示，run 方法在高层次上遵循了 ETL 管道的确切步骤：

1.  **extract.from_api()** — 从能源消耗 API 提取数据。

1.  **transform()** — 转换提取的数据。

1.  **validation.build_expectation_suite()** — 构建数据验证和完整性套件。忽略这一步，因为我们将在第 6 课中重点讲解。

1.  **load.to_feature_store()** — 将数据加载到特征存储中。

请注意我如何使用日志记录器来反映系统的当前状态。当你的程序部署并全天候运行时，详细的日志记录对于调试系统至关重要。此外，总是使用 Python 的日志记录器而不是 print 方法，因为你可以选择不同的日志级别和输出流。

从高层次来看，这似乎很容易理解。让我们分别深入了解每个组件。

**#1\. 提取**

在提取步骤中，我们请求给定窗口长度的数据。窗口的长度将等于**days_export**。窗口的第一个数据点是**export_end_reference_datetime - days_delay - days_export**，而窗口的最后一个数据点等于**export_end_reference_datetime - days_delay**。

我们使用了参数**days_delay**来根据数据的延迟移动窗口。在我们的使用案例中，API 延迟为 15 天。

如上所述，该函数向 API 发出 HTTP GET 请求以请求数据。随后，响应被解码并加载到 Pandas DataFrame 中。

该函数返回 DataFrame 以及包含有关数据提取信息的附加元数据。

**#2\. 转换**

转换步骤将原始 DataFrame 进行如下转换：

+   将列重命名为 Python 标准化格式

+   将列转换为其适合的类型

+   将字符串列编码为整数

注意我们没有包括 EDA 步骤（例如，查找空值），因为我们的主要关注点是设计系统，而不是标准的数据科学过程。

**#3\. 数据验证**

这是我们确保数据符合预期的地方。在我们的案例中，基于我们的 EDA 和转换，我们期望：

+   数据中没有任何空值

+   列的类型如预期

+   值的范围如预期

*有关此主题的更多内容，请参见第 6 课。*

**#4\. 加载**

这是我们将处理后的 DataFrame 加载到特征存储中的地方。

Hopsworks 有一系列很棒的教程，你可以在[这里](https://docs.hopsworks.ai/3.1/tutorials/)查看。但让我解释一下发生了什么：

+   我们使用 API_KEY 登录到我们的 Hopsworks 项目中。

+   我们获取特征存储的引用。

+   我们获取或创建一个特征组，这基本上是一个数据库表，上面附加了特征存储的所有优点（更多信息请见[这里](https://docs.hopsworks.ai/3.1/concepts/fs/feature_group/fg_overview/) [5]）。

+   我们插入新的处理数据样本。

+   我们为数据中的每个特征添加一组特征描述。

+   我们指示 Hopsworks 为每个特征计算统计信息。

查看下面的视频，看看我刚才解释的内容在 Hopsworks 中是什么样的 👇

Hopsworks 概述[作者的视频]。

太棒了！现在我们有了一个 Python ETL 脚本，它从能耗 API 中提取数据，并将其加载到特征存储中。

## 创建特征视图与训练数据集

最后一步是创建一个特征视图和训练数据集，稍后将被引入训练管道中。

**注意：** 特征管道是唯一一个对特征存储进行写操作的过程。其他组件仅会查询特征存储中的各种数据集。通过这样做，我们可以安全地将特征存储作为唯一的真实来源，并在系统中共享特征。

在[**feature_pipeline/feature_view.py**](https://github.com/iusztinpaul/energy-forecasting/blob/main/feature-pipeline/feature_pipeline/feature_view.py)文件中，我们有一个**create()**方法，它运行以下逻辑：

1.  我们从特征管道中加载元数据。请记住，FE 元数据包含提取窗口的开始和结束时间、特征组的版本等。

1.  我们登录 Hopswork 项目并创建对特征存储的引用。

1.  我们删除所有旧的特征视图（通常，你不需要执行这一步。正好相反，你会希望保留旧的数据集。但是，Hopwork 的免费版本限制你只能使用 100 个特征视图。因此，我们想要保留我们的免费版本）。

1.  我们根据给定版本获取特征组。

1.  我们使用从加载的特征组中得到的所有数据创建一个特征视图。

1.  我们仅使用给定的时间窗口创建训练数据集。

1.  我们创建元数据的快照并保存到磁盘。

**注意：** 特征视图是一种将多个特征组组合成一个“数据集”的智能方法。它类似于 SQL 数据库中的 VIEW。你可以在[这里](https://docs.hopsworks.ai/3.1/concepts/fs/feature_view/fv_overview/) [4]了解更多关于特征视图的信息。

就这样。你建立了一个特征管道，它提取、转换并加载数据到特征存储中。基于特征存储中的数据，你创建了一个特征视图和训练数据集，这些将作为系统中的唯一真实来源。

**注意：** 你需要良好的软件工程原则和模式知识来构建健壮的特征工程管道。[你可以在这里阅读一些实践示例](https://pub.towardsai.net/10-underrated-software-patterns-every-ml-engineer-should-know-92e702b96407)。

## 重要的设计决策

正如你所看到的，我们在这节课中实际上没有计算任何特征。我们只是清理、验证并确保数据已经准备好用于系统中。

*但这被称为“特征管道”，为什么我们没有计算任何特征呢？*

让我解释一下。

**特征 = 原始数据 + 转换函数**

如果我们将原始数据和转换函数存储在特征库中，而不是计算和存储特征，会怎样呢？

这样做我们可以获得以下好处：

+   更快的实验，因为数据科学家不需要请求数据工程师计算新特征。他只需将新转换添加到特征库中。

+   你可以节省大量存储空间。例如，与其保存 5 个从同一原始数据列计算出的特征，不如只保存原始数据列和 5 个转换，这样只使用原来的 1/5 的空间。

使用这种方法的缺点：

+   你的特征将在运行时通过云端或推理管道进行计算。因此，你将在运行时增加额外的延迟。

但在使用批量服务范式时，延迟不是一个显著的限制。因此，我们确实这样做了！

[查看第 2 课](https://medium.com/towards-data-science/a-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee) 以了解我们如何建模时间序列以预测接下来 24 小时的能源消耗。在 [第 2 课](https://medium.com/towards-data-science/a-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee) 中，我们将展示如何将转换直接存储在特征库中。

# 结论

恭喜！你完成了**第一课**来自**全栈 7 步 MLOps 框架**课程。

你了解了如何设计批量服务架构以及开发自己的 ETL 管道，这些管道：

+   从 HTTP API 中提取数据

+   清理数据

+   转换数据

+   将数据加载到特征库中

+   创建一个新的训练数据集版本

现在你已经理解了使用特征库的强大功能及其对任何 ML 系统的重要性，你可以在几周内而不是几个月内部署你的模型。

[查看第 2 课](https://medium.com/towards-data-science/a-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee) 以了解有关训练管道、机器学习平台和超参数调整的信息。

**此外**，[**你可以在这里访问 GitHub 仓库。**](https://github.com/iusztinpaul/energy-forecasting)

💡 我的目标是帮助机器学习工程师在设计和生产化 ML 系统方面提升水平。关注我 [LinkedIn](https://www.linkedin.com/in/pauliusztin/) 或订阅我的 [每周通讯](https://pauliusztin.substack.com/) 以获取更多见解！

🔥 如果你喜欢阅读类似的文章并希望支持我的写作，考虑 [成为 Medium 会员](https://pauliusztin.medium.com/membership)。通过使用 [我的推荐链接](https://pauliusztin.medium.com/membership)，你可以在没有额外费用的情况下支持我，同时享受 Medium 丰富故事的无限访问。

[](https://pauliusztin.medium.com/membership?source=post_page-----f0b29609b20f--------------------------------) [## 通过我的推荐链接加入 Medium - 保罗·伊斯津]

### 🤖 加入以获取关于设计和构建生产级机器学习系统的独家内容 🚀 解锁完整访问权限…

[pauliusztin.medium.com](https://pauliusztin.medium.com/membership?source=post_page-----f0b29609b20f--------------------------------)

## 参考文献

[1] [丹麦 API 中的 DE35 行业代码能源消耗](https://www.energidataservice.dk/tso-electricity/ConsumptionDE35Hour)，[丹麦能源数据服务](https://www.energidataservice.dk/about/)

[2] [Hopsworks 教程](https://docs.hopsworks.ai/3.1/tutorials/)，Hopsworks 文档

[3] 吉姆·道林，[特征存储与数据仓库](https://www.kdnuggets.com/2020/12/feature-store-vs-data-warehouse.html)（2020 年），KDnuggets

[4] [Hopsworks 特征视图](https://docs.hopsworks.ai/3.1/concepts/fs/feature_view/fv_overview/)，Hopsworks 文档

[5] [Hopsworks 特征组](https://docs.hopsworks.ai/3.1/concepts/fs/feature_group/fg_overview/)，Hopsworks 文档
