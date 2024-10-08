# 数据管道编排

> 原文：[`towardsdatascience.com/data-pipeline-orchestration-9887e1b5eb7a`](https://towardsdatascience.com/data-pipeline-orchestration-9887e1b5eb7a)

## 正确的数据管道管理简化了部署，增加了数据分析的可用性和可及性。

[](https://mshakhomirov.medium.com/?source=post_page-----9887e1b5eb7a--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----9887e1b5eb7a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9887e1b5eb7a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9887e1b5eb7a--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----9887e1b5eb7a--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9887e1b5eb7a--------------------------------) ·7 分钟阅读·2023 年 4 月 3 日

--

![](img/6ab99cb7a0920a4d1f69a66bee5f00b7.png)

图片由 [Manuel Nägeli](https://unsplash.com/@gwundrig?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 什么是数据编排？

DataOps 团队使用**数据管道编排**作为集中管理和监督端到端数据管道的解决方案。

> 自动化数据管道的过程被称为数据编排。

正确管理数据管道很重要，因为它几乎影响一切，即数据质量、过程速度和数据治理。

> 什么是有效的数据管道管理？

- **透明性和可见性。** 让团队中的每个人都知道数据是如何转换的，它来自哪里，以及数据转换过程的结束地点是很重要的。

- **更快的部署。** 能够持续重现数据管道的各个元素至关重要。将这些元素视为数据管道的构建模块。因此，当需要创建新的数据管道时，重要的是能够轻松复制这些构建模块，而不是从头开始创建。

- **高效的数据治理。** 通过创建结构化的数据流，相关团队能够管理和源控流程，数据易于访问和管理。

## 处理数据工作流时的常见问题

## ETL 工作流复杂性

我的经验表明，管理数据管道时最典型的问题是**ETL 过程的复杂性**。数据管道通常由一系列数据转换步骤定义，这些步骤将数据从源头转移到目的地。有许多工具，甚至是框架、方法和技术可以做到这一点，我之前在这里写过：

[](/data-pipeline-design-patterns-100afa4b93e3?source=post_page-----9887e1b5eb7a--------------------------------) ## 数据管道设计模式

### 选择正确的架构示例

towardsdatascience.com

因此，对数据管道及其包含的步骤进行源代码控制是有意义的。记录所有内容非常重要，以便团队其他成员准确了解数据解决方案的功能。

有一个数据管道的可视化表示总是很有帮助的。

如果你使用 Airflow、DBT、Dataform、Jinja 或 AWS Step Functions 部署管道，那很好，通常这些工具提供了很好的依赖关系图功能。

![](img/9aea09abe9e5292450751d0bcf44658f.png)

示例依赖关系图。图像由作者提供。

## 难以复制和部署更改

数据管道通常很复杂，可能需要大量时间来更改相关资源并进行部署。换句话说，这可能成为数据和机器学习工程师非常耗时的任务。

还必须涵盖数据管道的所有部分，而不仅仅是数据转换部分。例如，Airflow 是一个很好的工具来协调数据管道，我们可能想在这里使用 S3 桶连接器来连接数据湖，但你可能还需要描述 S3 资源并将其保存在 Github 上。

> 这就是基础设施即代码（IAC）变得有用的地方。

使用像 Terraform 和 AWS CloudFormation 这样的 IAC 工具，我们可以描述我们数据管道可能需要的所有资源，而不仅仅是实际执行数据转换的资源。例如，我们可以描述不仅是转换数据的管道资源，即 AWS Lambda 函数和其他服务，还可以描述数据存储资源、通知设置和这些微服务的警报。一些 IAC 解决方案是平台无关的（Terraform），一些则不是（AWS CloudFormation），它们都有其优缺点**根据手头的数据堆栈和 DataOps 团队的技能而定。**

[](https://levelup.gitconnected.com/infrastructure-as-code-for-beginners-a4e36c805316?source=post_page-----9887e1b5eb7a--------------------------------) [## 初学者的基础设施即代码

### 使用这些模板像专业人士一样部署数据管道

levelup.gitconnected.com](https://levelup.gitconnected.com/infrastructure-as-code-for-beginners-a4e36c805316?source=post_page-----9887e1b5eb7a--------------------------------)

可以在不同环境中持续重复和部署的数据管道解决方案很棒，因为它们是源代码控制的，并具有很好的**CI/CD 特性**。所有这些有助于**避免潜在的人为错误，并减少数据工程时间和成本**。

现代的数据管道管理和协调方法是减少上述所有潜在问题的，即

> 为了减少人为错误，能够轻松复制数据管道资源，可视化依赖关系并提高数据质量。

**正确的数据协调可以增加数据的可用性和分析的可及性。**

## 需要管理的典型数据流

通常，数据管道指的是一组数据相关的资源，帮助我们将数据从 A 点传送到 B 点并进行转换。

资源指的是工具，在概念层面上，我们有三个主要的数据处理任务，必须能够有效执行：

**- 数据存储**

这可以是 Google Cloud Storage 中的数据湖、AWS S3 数据湖等，或者任何关系型和非关系型数据库，甚至是通过 API 提供的第三方资源。它们的共同目的是存储数据。在大多数情况下，这就是我们数据管道设计图中数据的来源。

**- 数据加载或摄取。**

这些可以是管理工具（如 Fivetran、Stitch 等），也可以是定制的无服务器微服务和各种 AWS Lambda 或 GCP Cloud Functions。它们的共同目的是执行 ETL 并将数据加载到目标中，例如数据仓库或根据我们的数据平台架构类型的其他数据湖。有时使用能够良好扩展的工具（例如 Spark）可能更高效。

[](/data-platform-architecture-types-f255ac6e0b7?source=post_page-----9887e1b5eb7a--------------------------------) ## 数据平台架构类型

### 它在多大程度上满足了你的业务需求？选择的困境。

towardsdatascience.com

**- 数据转换工具**

历史上，最自然的数据转换方式是 SQL。这是一种被所有团队、商业智能（BI）和数据分析师、软件工程师及数据科学家认可的常用数据操作语言。因此，目前有多种工具可以提供可靠的源控数据转换，例如 Jinja、DBT、Dataform、AWS Step Functions 等。

**- 商业智能**

这些工具帮助提供分析和见解。其中一些是免费的社区工具，如 Looker Studio，其他的是仅限订阅的付费工具。每种工具都有优缺点，选择时可能需要根据公司规模来决定。例如，中小企业通常不需要为 BI OLAP 立方体功能支付额外费用，这些功能用于额外分析和转换数据。他们只需通过电子邮件发送包含主要 KPI 的每日仪表盘。

工具列表庞大，一些出色的 BI 解决方案也提供免费功能。

**不是详尽的列表：**

+   AWS Quicksight

+   Mode

+   Sisense

+   Looker

+   Metabase

+   PowerBI

**问题在于没有完全集成功能的数据工具。**

## 我们如何将数据服务连接在一起？

将我们的数据处理过程**整合到一个解决方案中**也可以通过一点编程和基础设施即代码技术来实现。

从概念上讲，我们可能希望像这样部署数据管道：

![](img/b999ac569c1399f8eacbfa0d66c53a15.png)

图片由作者提供。

> 我们如何使这个解决方案既强大又具有成本效益？

毋庸置疑，最具成本效益的方法是使用**无服务器架构**。例如，考虑下面的数据管道。它可以通过 IAC（AWS Cloudformation 或 Terraform）进行部署，并且所有主要部分都集成在一个完整的数据解决方案中。

我们有 AWS Lambda 函数、AWS Step 函数、一个关系型数据库、数据湖存储桶和一个数据湖房解决方案（AWS Athena）。

![](img/3d6fa66e126398bfe0bd267f57bf7059.png)

示例 DAG。图片由作者提供。

考虑这个由 AWS Lambda 函数组成的数据管道示例。作为一个独立的微服务，它只是一个 lambda 函数，但我们可以用它做更多的事情，例如从 API 提取数据、从关系型数据库导出数据、调用和触发其他服务等。

我们可以通过一个 AWS CLI 命令使用基础设施即代码来部署管道：

![](img/a53bea5cc9239238a4bb2fce87a101e8.png)

部署结果。图片由作者提供。

有了这个，我们应该能够创建一个简单的 Step Function，并可以将其用作模板来扩展和改进我们的 ETL 管道。

如果我们选择**平台无关**，我们可以使用**Terraform**来部署完整的管道，并在不同的云平台上使用各种数据服务。我经常使用这种数据管道设计模式与 AWS Lambda 函数结合使用，以在我的 BigQuery 数据仓库解决方案中运行 SQL 查询或执行其他 ETL 任务，非常有用。

## 结论

将我们的数据处理过程打包成一个解决方案可能会很具挑战性，并且需要一些**想象力**。

基础设施即代码听起来是正确的选择。确实，它简化了数据解决方案的部署和复制，消除了人为错误，并有助于更快地执行数据工程和 MLOps 任务。尽管它可能看起来复杂和精致，但这都需要经验，并且需要时间来学习。不要犹豫，投入一点时间来学习它。这是一项非常有价值的技能。

> 它可以提供多种数据管道编排选项。

只需一点编码和了解 API 的工作方式，数据管道设计和管理的机会就变得无穷无尽。

## 推荐阅读

[](/create-mysql-and-postgres-instances-using-aws-cloudformation-d3af3c46c22a?source=post_page-----9887e1b5eb7a--------------------------------) ## 使用 AWS Cloudformation 创建 MySQL 和 Postgres 实例

### 数据库从业者的基础设施即代码

[towardsdatascience.com [](https://aws.amazon.com/cloudformation/?source=post_page-----9887e1b5eb7a--------------------------------) [## 作为代码的基础设施 - AWS CloudFormation - AWS

### AWS CloudFormation 通过 AWS CloudFormation，您可以在全球范围内扩展基础设施，并管理所有 AWS 账户和区域中的资源…

[aws.amazon.com](https://aws.amazon.com/cloudformation/?source=post_page-----9887e1b5eb7a--------------------------------) [](https://aws.amazon.com/step-functions/?source=post_page-----9887e1b5eb7a--------------------------------) [## 无服务器工作流编排 - AWS Step Functions - 亚马逊网络服务

### AWS Step Functions 每月支持 4,000 次状态转换 使用代码按需处理数据，进行大规模并行处理…

[aws.amazon.com](https://aws.amazon.com/step-functions/?source=post_page-----9887e1b5eb7a--------------------------------) [](https://www.terraform.io/?source=post_page-----9887e1b5eb7a--------------------------------) [## HashiCorp 的 Terraform

### Terraform 是一款开源的基础设施即代码软件工具，能够帮助您安全、可预测地创建…

[www.terraform.io](https://www.terraform.io/?source=post_page-----9887e1b5eb7a--------------------------------)
