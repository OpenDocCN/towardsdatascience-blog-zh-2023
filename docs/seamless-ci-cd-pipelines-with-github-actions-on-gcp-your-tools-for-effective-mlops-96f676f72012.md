# 使用 GitHub Actions 在 GCP 上实现无缝 CI/CD 管道：你进行有效 MLOps 的工具

> 原文：[`towardsdatascience.com/seamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012`](https://towardsdatascience.com/seamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012)

## [完整的 7 步骤 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 第七部分：将所有机器学习组件部署到 GCP。使用 Github Actions 构建 CI/CD 管道。

[](https://pauliusztin.medium.com/?source=post_page-----96f676f72012--------------------------------)![Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----96f676f72012--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96f676f72012--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----96f676f72012--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----96f676f72012--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----96f676f72012--------------------------------) ·19 分钟阅读·2023 年 6 月 15 日

--

![](img/7c34e9ba645996ba5d4b808f1f130f93.png)

图片由[哈桑·帕沙](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本教程是**7 节课程中的第七部分**，将逐步指导你如何**设计、实施和部署一个机器学习系统**，使用**MLOps 良好实践**。在课程期间，你将构建一个生产就绪的模型，以预测丹麦多个消费者类型在未来 24 小时内的能源消耗水平。

*到课程结束时，你将了解设计、编码和部署机器学习系统的所有基础知识，使用批量服务架构。*

本课程*面向中级/高级机器学习工程师*，那些希望通过构建自己的端到端项目来提升技能的人。

> 现在，证书无处不在。构建高级端到端项目，并在之后展示出来，是获得专业工程师认可的最佳方式。

# 目录：

+   课程介绍

+   课程内容

+   数据源

+   第七部分：将所有机器学习组件部署到 GCP。使用 Github Actions 构建 CI/CD 管道。

+   第七部分：代码

+   结论

+   参考文献

# 课程介绍

***在这个 7 节课程的最后，你将知道如何：***

+   设计一个批量服务架构

+   使用 Hopsworks 作为特征存储

+   设计一个从 API 读取数据的特征工程管道

+   构建一个带有超参数调优的训练管道

+   使用 W&B 作为机器学习平台来跟踪你的实验、模型和元数据

+   实现一个批量预测管道

+   使用 Poetry 来构建你自己的 Python 包

+   部署你自己的私人 PyPi 服务器

+   用 Airflow 编排一切

+   使用预测来编写一个使用 FastAPI 和 Streamlit 的 web 应用

+   使用 Docker 来容器化你的代码

+   使用 Great Expectations 来确保数据验证和完整性

+   监控预测性能的变化

+   将一切部署到 GCP

+   使用 GitHub Actions 构建 CI/CD 流水线

如果这听起来有点多，不用担心。完成本课程后，你将理解我之前所说的一切。最重要的是，你将知道我为什么使用这些工具以及它们如何作为一个系统协同工作。

**如果你想充分利用本课程，** [**我建议你访问包含所有课程代码的 GitHub 仓库**](https://github.com/iusztinpaul/energy-forecasting) **。本课程设计旨在快速阅读和复制文章中的代码。**

到课程结束时，你将知道如何实现下面的图示。如果有任何内容让你困惑，不用担心，我会详细解释。

![](img/4b5c3b0b8e2162ea8fd268ca745199ec.png)

你将在课程中构建的架构图 [作者提供的图片]。

到**第七部分课结束时**，你将知道如何手动将 3 个 ML 流水线和 web 应用部署到 GCP。此外，你还将构建一个 CI/CD 流水线，使用 GitHub Actions 自动化部署过程。

# 课程内容：

1.  [批量服务。特征存储。特征工程流水线。](https://medium.com/towards-data-science/a-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f)

1.  [训练流水线。ML 平台。超参数调优。](https://medium.com/towards-data-science/a-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee)

1.  [批量预测流水线。用 Poetry 打包 Python 模块。](https://medium.com/towards-data-science/unlock-the-secret-to-efficient-batch-prediction-pipelines-using-python-a-feature-store-and-gcs-17a1462ca489)

1.  私人 PyPi 服务器。用 Airflow 编排一切。

1.  使用 GE 进行数据验证以确保质量和完整性。模型性能持续监控。

1.  [使用 FastAPI 和 Streamlit 消费和可视化你的模型预测。将一切 Docker 化。](https://medium.com/towards-data-science/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243)

1.  **将所有机器学习组件部署到 GCP。使用 Github Actions 构建 CI/CD 流水线。**

1.  [[附加内容] 一个‘不完美’的 ML 项目的幕后 — 经验与见解](https://medium.com/towards-data-science/imperfections-unveiled-the-intriguing-reality-behind-our-mlops-course-creation-6ff7d52ecb7e)

由于第 7 课专注于教你如何将所有组件部署到 GCP 并围绕它构建 CI/CD 管道，因此为了获得完整体验，我们建议你观看 [课程](https://towardsdatascience.com/tagged/full-stack-mlops) 的其他课程。

查看 第 4 课 了解如何使用 Airflow 编排 3 个机器学习管道，查看 [第 6 课](https://medium.com/towards-data-science/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243) 了解如何使用 FastAPI 和 Streamlit 消耗模型的预测。

# 数据来源

我们使用了一个免费的开放 API，提供了丹麦所有能源消费者类型的每小时能耗值 [1]。

它们提供了一个直观的界面，你可以轻松查询和可视化数据。 [你可以在这里访问数据](https://www.energidataservice.dk/tso-electricity/ConsumptionDE35Hour) [1]。

数据有 4 个主要属性：

+   **小时 UTC：** 数据点被观察到的 UTC 日期时间。

+   **价格区域：** 丹麦被分为两个价格区域：DK1 和 DK2——由大贝尔特海峡分隔。DK1 在大贝尔特西侧，DK2 在大贝尔特东侧。

+   **消费者类型：** 消费者类型是行业代码 DE35，由丹麦能源公司拥有和维护。

+   **总消费量：** 总电力消耗，单位为千瓦时。

**注意：** 观察数据有 15 天的滞后！但对于我们的演示用例，这不是问题，因为我们可以模拟与实时情况相同的步骤。

![](img/4eab6debdb7ba94406b8d0a8e28e3438.png)

我们的 Web 应用截图，展示了我们如何预测区域 = 1 和消费者类型 = 212 的能耗 [作者提供的图片]。

数据点的分辨率为每小时。例如：“2023–04–15 21:00Z”，“2023–04–15 20:00Z”，“2023–04–15 19:00Z”等等。

我们将数据建模为多个时间序列。每个独特的**价格区域**和**消费者类型元组代表其**唯一的时间序列。

因此，我们将构建一个模型，独立预测每个时间序列的未来 24 小时的能耗。

*查看下面的视频以更好地了解数据的样子* 👇

课程与数据源概述 [作者提供的视频]。

# 第 7 课：将所有机器学习组件部署到 GCP。使用 GitHub Actions 构建 CI/CD 管道。

## 第 7 课的目标

在第 7 课中，我将教你 2 件事：

1.  如何手动将 3 个机器学习管道和 Web 应用部署到 GCP。

1.  如何使用 GitHub Actions 自动化部署过程。

![](img/a0fb9d4a6918ea3cfabd95f66a8c7fc3.png)

最终架构图，Lesson 7 的组件以蓝色突出显示 [作者提供的图片]。

换句话说，你将展示到目前为止所做的一切。

只要你的工作停留在你的计算机上，它可能是世界上最好的机器学习解决方案，但不幸的是，它不会带来任何价值。

了解如何部署你的代码对任何项目至关重要。

所以记住…

我们将使用 GCP 作为云提供商，GitHub Actions 作为 CI/CD 工具。

## 理论概念与工具

**CI/CD:** CI/CD 代表持续集成和持续交付。

CI 步骤主要包括每次你将代码推送到 git 时构建和测试你的代码。

CD 步骤会自动将你的代码部署到多个环境：开发、测试和生产。

根据你的具体软件需求，你可能需要或不需要标准 CI/CD 管道的所有规范。

例如，你可能正在进行一个概念验证。然后测试环境可能会显得多余。但拥有一个开发和生产 CD 管道将极大地提高你的生产力。

**GitHub Actions:** GitHub Actions 是目前最流行的 CI/CD 工具之一。它直接集成在你的 GitHub 仓库中。最棒的是你不需要任何虚拟机来运行 CI/CD 管道。一切都在 GitHub 的计算机上运行。

你需要在 YAML 文件中指定一组规则，GitHub Actions 将处理其余的部分。我将在本文中向你展示它是如何工作的。

***GitHub Actions*** *完全* ***免费*** *用于公共仓库。这多么棒啊？*

顺便提一下。使用 GitHub Actions，你可以基于各种仓库事件触发任何作业，但将其作为 CI/CD 工具使用是最常见的用例。

# 第七课：代码

[你可以在这里访问 GitHub 仓库。](https://github.com/iusztinpaul/energy-forecasting)

**注意：** 所有的安装说明都在仓库的 README 中。这里你将直接跳转到代码部分。

*第七课的代码和说明在以下位置：*

+   [***deploy/***](https://github.com/iusztinpaul/energy-forecasting/tree/main/deploy)— Docker 和 shell 部署文件

+   [***.github/workflows***](https://github.com/iusztinpaul/energy-forecasting/tree/main/.github/workflows) — GitHub Actions CI/CD 工作流

+   [***README_DEPLOY***](https://github.com/iusztinpaul/energy-forecasting/blob/main/README_DEPLOY.md) — 用于将代码部署到 GCP 的 README

+   ***README_CICD*** — 用于设置 CI/CD 管道的 README

## 准备凭证

直接在你的 git 仓库中存储凭证是巨大的安全隐患。这就是为什么你需要通过 **.env** 文件注入敏感信息。

**.env.default** 是你必须配置的所有变量的示例。它也有助于存储非敏感属性的默认值（例如，项目名称）。

![](img/87b81fc121cea9485a6b41dd4d656eb8.png)

.env.default 文件的截图 [作者提供的图片]。

要复制这篇文章，你必须设置课程期间使用的所有基础设施和服务 [课程](https://towardsdatascience.com/tagged/full-stack-mlops)。

*2 个主要组件可以单独部署。*

**#1\. 三个 ML 管道：**

+   特征管道

+   训练管道

+   批量预测管道

对于 **#1.**，你需要设置以下内容：

+   [*Hopsworks*](https://www.hopsworks.ai/) *(免费)* — 特征存储：Lesson 1

+   [*W&B*](https://wandb.ai/site) *(免费)*— ML 平台：Lesson 2

+   [*GCS 桶*](https://cloud.google.com/storage) *(免费)* — GCP 上的存储：Lesson 3

+   [*Airflow*](https://airflow.apache.org/) *(免费)*— 开源编排工具：Lesson 4

**#2\. Web 应用：**

+   [*FastAPI*](https://fastapi.tiangolo.com/) *后端（免费）*：[Lesson 6](https://medium.com/towards-data-science/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243)

+   [*Streamlit*](https://streamlit.io/) *预测仪表盘（免费）*：[Lesson 6](https://medium.com/towards-data-science/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243)

+   [*Streamlit*](https://streamlit.io/) *监控仪表盘（免费）*：[Lesson 6](https://medium.com/towards-data-science/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243)

幸运的是，对于**#2.**，你只需要设置用作存储的 GCP [GCS 桶](https://cloud.google.com/storage)。

但请注意，如果你只完成**#2.**部分，你的 Web 应用中不会有任何数据可供使用。

我们不想让这篇文章充满无聊的内容，比如设置凭证。不过幸运的是，如果你打算实现和复制整个[课程](https://towardsdatascience.com/tagged/full-stack-mlops)，你可以在之前的文章中找到一步步的说明和[GitHub README](https://github.com/iusztinpaul/energy-forecasting)。

如果你想查看（而不是复制）我们如何将代码部署到 GCP 并构建 GitHub Actions 工作流，你无需担心任何凭证。只需继续查看以下部分 ✌️

**注意：** 唯一没有免费计划的服务就在这个课程中。*当我编写这个课程时，部署和测试 GCP 上的基础设施花费了我大约 20 美元。* 但我有一个全新的 GCP 账户，提供了 300 美元的 GCP 信用，因此间接使其免费。只要记得在完成后删除所有 GCP 资源，你就会没问题的。

## 手动部署到 GCP

所以，让我们手动将*2 个主要组件*部署到 GCP：

+   ML 管道

+   Web 应用

不过，作为第一步，让我们*设置所有部署所需的 GCP 资源*。之后，你将通过 SSH 连接到你的机器并部署你的代码。

[*更多信息，请访问 GitHub 部署 README。*](https://github.com/iusztinpaul/energy-forecasting/blob/main/README_DEPLOY.md)

***设置资源***

让我们去你的 GCP *energy_consumption* 项目中创建以下资源：

1.  具有 IAP 访问权限的 Admin VM 服务账户

1.  公开端口的防火墙规则

1.  IAP 用于 TCP 隧道的防火墙规则

1.  用于管道的 VM

1.  用于 Web 应用的 VM

1.  外部静态 IP

不要被华丽的名称吓到。您将通过这篇文章和我提供的 GCP 文档访问逐步指南。

**注意：** 如果您不打算在您的 GCP 基础设施上复制该基础设施，请跳过 ***“设置资源”*** 部分，直接进入“***部署 ML 管道”***。

*#1\. 具有 IAP 访问权限的管理员 VM 服务账户*

我们需要一个新的 GCP 服务账户，具有管理员权限和对 GCP VMs 的 IAP 访问权限。

您必须创建一个新的服务账户并分配以下角色：

+   计算实例管理员 (v1)

+   IAP 保护的隧道用户

+   服务账户令牌创建者

+   服务账户用户

IAP 代表身份感知代理。这是一种创建隧道以在您的私有网络内部路由 TCP 流量的方式。为了您的了解，您可以通过以下文档了解更多信息（您不必理解它才能继续下一步）：

+   [使用 IAP 进行 TCP 转发](https://cloud.google.com/iap/docs/using-tcp-forwarding) [2]

+   [TCP 转发概述](https://cloud.google.com/iap/docs/tcp-forwarding-overview) [3]

*#2\. 公开端口的防火墙规则*

创建一个防火墙规则，公开以下 TCP 端口：8501、8502 和 8001。

同时，添加一个名为 *energy-forecasting-expose-ports* 的 *目标标签*。

这里有 2 个文档帮助我们创建和配置防火墙规则的端口：

+   [如何在 Google Compute Engine 中打开特定端口，例如 9090](https://stackoverflow.com/questions/21065922/how-to-open-a-specific-port-such-as-9090-in-google-compute-engine) [4]

+   [如何在 GCP Compute Engine 实例上打开防火墙端口](https://www.howtogeek.com/devops/how-to-open-firewall-ports-on-a-gcp-compute-engine-instance/) [5]

这是我们的防火墙规则的样子 👇

![](img/97e2efc2f96dec4a2003b681538dc4a1.png)

GCP "公开端口" 防火墙规则的截图 [作者提供的图片]。

*#3\. IAP 用于 TCP 隧道的防火墙规则*

现在我们将创建一个防火墙规则，允许 IAP 对所有连接到 *default* 网络的 VM 进行 TCP 隧道。

[如何创建用于 TCP 隧道的 IAP 防火墙规则的逐步指南](https://cloud.google.com/iap/docs/using-tcp-forwarding#preparing_your_project_for_tcp_forwarding) [6]。

这是我们的防火墙规则的样子 👇

![](img/b62ecb6aa11acc85634ecba689e6e98e.png)

GCP "IAP TCP 转发" 防火墙规则的截图 [作者提供的图片]。

*#4\. 用于管道的 VM*

转到您的 GCP *energy_consumption* 项目 -> VM 实例 -> 创建实例。

选择 *e2-standard-2: 2 vCPU 核心 — 8 GB RAM* 作为您的 VM 实例类型。

命名为：*ml-pipeline*

将磁盘更改为 *20 GB 存储*。

选择区域 *europe-west3 (法兰克福)*` 和区域 *europe-west3-c.* 在这里，您可以选择任何其他区域和区域，但如果这是您第一次这样做，我们建议您像我们一样操作。

网络：*default*

此外，勾选 *HTTP* 和 *HTTPS* 选项，并添加我们在几步前创建的 *energy-forecasting-expose-ports* 自定义防火墙规则。

这里有两个文档帮助我创建和配置防火墙规则的端口：

+   [如何在 Google Compute Engine 中打开特定端口，如 9090](https://stackoverflow.com/questions/21065922/how-to-open-a-specific-port-such-as-9090-in-google-compute-engine) [4]

+   [如何在 GCP Compute Engine 实例上打开防火墙端口](https://www.howtogeek.com/devops/how-to-open-firewall-ports-on-a-gcp-compute-engine-instance/) [5]

*#5\. Web 应用程序的 VM*

现在，让我们对 Web 应用程序 VM 重复类似的过程，但设置略有不同。

这次选择 *e2-micro: 0.25 2 vCPU — 1 GB memory* 作为你的 VM 实例类型。

命名为：*app*

将磁盘更改为 *15 GB 标准持久磁盘*

选择区域 *europe-west3 (Frankfurt)* 和区域 *europe-west3-c*。

网络：*default*

此外，勾选 *HTTP* 和 *HTTPS* 选项，并添加我们在几步前创建的 *energy-forecasting-expose-ports* 自定义防火墙规则。

*#6\. 外部静态 IP*

这是最后一块拼图。

如果我们希望我们的 Web 应用程序的外部 IP 保持静态（即不更改），我们必须为 Web 应用程序 VM 附加静态地址。

我们建议将其仅添加到我们之前创建的*app* VM 中。

此外，为 ml-pipeline VM 添加静态外部 IP 是完全可以的。

[保留静态外部 IP 地址的文档](https://cloud.google.com/compute/docs/ip-addresses/reserve-static-external-ip-address) [7]。

*现在枯燥的部分结束了，我们开始部署代码 👇*

**部署 ML 管道**

首先，我们必须安装 [gcloud GCP CLI 工具](https://cloud.google.com/sdk/docs/install) 以便在我们的计算机和 GCP VM 之间进行通信。

为了进行身份验证，我们将使用配置了 VM 和 IAP SSH 访问管理员权限的服务帐户。

现在，我们必须告诉 *gcloud GCP CLI* 使用那个 *service account*。

为此，你必须为你的服务帐户创建一个密钥并将其下载为 JSON 文件。与为存储桶服务帐户所做的相同——[这里有一些文档可以刷新你的记忆](https://cloud.google.com/iam/docs/keys-create-delete) [8]。

下载文件后，你需要在终端中运行以下 *gcloud* 命令：

```py
gcloud auth activate-service-account SERVICE_ACCOUNT@DOMAIN.COM - key-file=/path/key.json - project=PROJECT_ID
```

[查看此文档以获取有关 *gcloud* 身份验证命令的更多详细信息](https://cloud.google.com/sdk/gcloud/reference/auth/activate-service-account)。

现在，每当你使用 *gcloud* 运行命令时，它将使用此服务帐户进行身份验证。

现在，让我们通过 SSH 连接到你在几步前创建的*ml-pipeline* GCP VM：

```py
gcloud compute ssh ml-pipeline - zone europe-west3-c - quiet - tunnel-through-iap - project <your-project-id>
```

+   **注意 1：** 如果你还没有在与我们相同的区域创建 VM，请更改 *zone*。

+   **注意 2：** 你的 *project-id* 不是你的 *project name*。前往你的 GCP 项目列表，找到项目 ID。

从此点开始，如果你正确配置了防火墙和服务账户，由于一切都已 Docker 化，所有步骤将与其余文章中的步骤 99% 相似。

查看 Github README — [*设置附加工具*](https://github.com/iusztinpaul/energy-forecasting#tools) 和 [*使用*](https://github.com/iusztinpaul/energy-forecasting#usage) 部分获取逐步说明。

当你通过 SSH 连接到 *ml-pipeline* GCP 机器时，可以按照相同的步骤操作。

请注意，GCP 机器使用的是 Linux 操作系统。因此，无论你在本地设备上使用什么操作系统，都可以直接复制和粘贴 README 中的命令。

![](img/32e7843fcd3316cc38aaff22d935fa92.png)

连接到 “app” 虚拟机的截图，使用 *gcloud* [作者提供的截图]。

你可以安全地重复你在本地设置 *流水线* 时所做的所有步骤，使用这个 SSH 连接，***但你必须注意以下 3 个边界情况*：**

*#1\. 将代码克隆到虚拟机的主目录中*

只需 SSH 到虚拟机并运行：

```py
git clone https://github.com/iusztinpaul/energy-forecasting.git
cd energy-forecasting
```

*#2\. 使用以下命令安装 Docker：*

安装 Docker：

```py
sudo apt update
sudo apt install --yes apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
sudo apt update
sudo apt install --yes docker-ce
```

为 Docker 添加 sudo 权限：

```py
sudo usermod -aG docker $USER
logout 
```

再次登录到你的机器：

```py
gcloud compute ssh ml-pipeline --zone europe-west3-c --quiet --tunnel-through-iap --project <your-project-id>
```

[查看这些文档获取完整的说明](https://tomroth.com.au/gcp-docker/) [9]。

*#3\. 用* ***gcloud******compute******scp**** *替换所有* ***cp*** *命令：*

这个命令将帮助你将文件从本地机器复制到虚拟机中。

例如，代替运行：

```py
cp -r /path/to/admin/gcs/credentials/admin-buckets.json credentials/gcp/energy_consumption
```

在不同的终端中运行（不是连接到虚拟机的 SSH 终端）：

```py
gcloud compute scp --recurse --zone europe-west3-c --quiet --tunnel-through-iap --project <your-project-id> /local/path/to/admin-buckets.json ml-pipeline:~/energy-forecasting/airflow/dags/credentials/gcp/energy_consumption/
```

这个命令将把你本地的 *admin-buckets.json* 文件复制到 *ml-pipeline* 虚拟机中。

在你设置好代码之后，前往 GCP 的虚拟机视图和 *网络标签* 部分。在那里你会找到 *外部 IP 地址* 列，如下图所示。复制该 IP 并将端口 *8080* 附加到它上面。

例如，根据下面图片中的 *外部 IP 地址*，我使用以下地址访问了 Airflow：*35.207.134.188:8080*。

*恭喜！你已连接到你自己托管的 Airflow 应用。*

**注意：** 如果无法连接，请稍等几秒钟以便正确加载。

![](img/761ce29a89b33a55d06a796a2a7a6037.png)

“app” GCP 虚拟机配置的截图 [作者提供的图片]。

**部署 Web 应用**

让我们使用 SSH 连接到你在前面几步创建的 *“app” GCP 虚拟机*：

```py
gcloud compute ssh app --zone europe-west3-c --quiet --tunnel-through-iap --project <your-project-id>
```

+   **注意 1：** 如果你没有在与我们相同的区域创建虚拟机，请更改 *区域*。

+   **注意 2：** 你的 *project-id* 不是你的 *项目名称*。请前往你的 GCP 项目列表，找到项目 ID。

这里的过程与 *“****部署 ML 流水线”*** 部分描述的类似*。

你可以按照 [Lesson 6](https://medium.com/towards-data-science/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243) 或 GitHub 仓库的 [设置附加工具](https://github.com/iusztinpaul/energy-forecasting#tools) 和 [使用方法](https://github.com/iusztinpaul/energy-forecasting#usage) 部分来部署 Web 应用。

> 但不要忘记关注 “**部署 ML 流水线**” 部分描述的 3 个边界情况。

请原谅我参考了这么多关于如何设置这些内容的外部文档。文章太长了，我不想在这里重复 GCP Google 的文档。

## 使用 GitHub Actions 的 CI/CD 流水线（免费）

GitHub Actions YAML 文件位于 [***.github/workflows***](https://github.com/iusztinpaul/energy-forecasting/tree/main/.github/workflows) 目录下。

首先，让我解释一下你需要了解的 GitHub Actions 文件的主要组件 👇

使用 "**on -> push -> branches:"** 部分，你指定要监听的分支事件。在这种情况下，当有新代码提交到 **"main"** 分支时，GitHub Action 将被触发。

在 "**env: "** 部分，你可以在脚本中声明所需的环境变量。

在 **"jobs -> ci_cd -> steps:"** 部分，你将声明 CI/CD 流水线步骤，这些步骤将按顺序运行。

在 **"jobs -> ci_cd -> runs-on:"** 部分，你需要指定步骤要运行的虚拟机镜像。

现在，让我们来看一些实际的 GitHub Action 文件 🔥

[**ML Pipeline GitHub Actions YAML 文件**](https://github.com/iusztinpaul/energy-forecasting/blob/main/.github/workflows/ci_cd_ml_pipeline.yml)

当新代码提交到 **"main"** 分支时，除了 Web 应用目录和 YAML 及 Markdown 文件外，该操作将被触发。

我们添加了包含 GCP 项目和 VM 信息的环境变量。

对于 CI/CD 步骤，我们主要做两件事：

1.  配置凭证并认证到 GCP，

1.  通过 SSH 连接到指定的 GCP VM 并运行一个命令，该命令：进入代码目录，拉取最新更改，构建 Python 包，并将其部署到 PyPi 注册表。现在，Airflow 下次运行时将使用新的 Python 包。

基本上，它做了你手动完成的工作，但现在，一切都通过 GitHub Actions 自动化了。

请注意，你不必记住或知道如何从零编写 GitHub Actions 文件，因为你可以找到大多数使用场景的现成模板。例如，这里是我们用来编写下面 YAML 文件的 [*google-github-actions/ssh-compute*](https://github.com/google-github-actions/ssh-compute) [11] 仓库。

你可以找到几乎任何你想到的使用场景的类似模板。

[**Web App GitHub Actions YAML 文件**](https://github.com/iusztinpaul/energy-forecasting/blob/main/.github/workflows/ci_cd_web_app.yml)

Web 应用的 actions 文件与用于 ML 流水线的文件 90% 相同，除了以下内容：

+   我们忽略了 ML 流水线文件；

+   我们运行一个构建和运行 Web 应用的 Docker 命令。

但“**${{ vars… }}"** 这种奇怪的语法来自哪里？我马上会解释，但你现在需要知道的是：

+   "**${{ vars.<name> }}":** 在 GitHub 内部设置的变量；

![](img/206d07d81bb7ac16fc28daae15a6d535.png)

+   "**${{ secrets.<name> }}":** 在 GitHub 内部设置的秘密。一旦设置了秘密，你将无法再看到它（你可以看到变量）；

![](img/d2e0ca9a85083a9bb94604bdf3f1ce92.png)

+   "**${{ env.<name> }}":** 在 "env:" 部分设置的环境变量。

**重要观察**

上面的 YAML 文件不包含 CI 部分，仅包含 CD 部分。

为了遵循稳健 CI 流水线的最佳实践，你应该运行一个构建 Docker 镜像并将其推送到 Docker 注册表的操作。

之后，你将通过 SSH 进入测试环境并运行测试套件。最后一步，你将通过 SSH 进入生产虚拟机，拉取镜像并运行它们。

系列太长了，我们想保持简单，但 *好消息是你已经学会了执行上述操作所需的所有工具和原则*。

## 设置 Secrets 和 Variables

此时，你需要 fork [energy-consumption 仓库](https://github.com/iusztinpaul/energy-forecasting/tree/main) 以用你自己的凭据配置 GitHub Actions。

[查看这篇文档以了解如何在 GitHub 上 fork 一个仓库](https://docs.github.com/en/get-started/quickstart/fork-a-repo) [10]。

**设置 Actions 变量**

进入你的 fork 仓库。点击 *"Settings -> Secrets and variables -> Actions"*。

现在，点击 "*Variables"*。你可以通过点击 *"New repository variable"* 来创建一个新变量。见下图 👇

![](img/77290affdaf28b1c0a7e8e582dde9ed8.png)

创建新仓库变量的截图 [作者提供的图片]。

*你需要创建 5 个 GitHub Actions 脚本将使用的变量：*

+   ***APP_INSTANCE_NAME***: Web 应用虚拟机的名称。在我们的例子中，它被称为 "*app"*。如果你使用我们推荐的命名约定，默认值应该没问题。

+   **GCLOUD_PROJECT**: 你 GCP 项目的 ID。在这里，你需要用你的项目 ID 替换它。

+   **ML_PIPELINE_INSTANCE_NAME**: ML 流水线虚拟机的名称。在我们的例子中，它是 "*ml-pipeline"*。如果你使用我们推荐的命名约定，默认值应该没问题。

+   **USER:** 你用来通过 SSH 连接到虚拟机的用户。在设置机器时我用的是 "*pauliusztin,*" 但你必须用你自己的用户名替换它。去虚拟机上运行 `echo $USER`。

+   **ZONE**: 你部署虚拟机的区域。如果你使用我们推荐的命名约定，默认值应该没问题。

**设置 Action 密钥**

在相同的 *"Secrets and variables/Actions"* 部分，点击 *"Secrets"* 标签。

你可以通过点击 "*New repository secret*" 按钮来创建一个新的秘密。

这些类似于我们刚刚完成的变量，但在你填写它们的值后，你将无法再看到它们。这就是为什么这些被称为秘密的原因。

这里是你添加所有敏感信息的地方。在我们的案例中，即 GCP 凭据和私钥。请参见下面的图片 👇

![](img/711957b341193790f0d96ab36562a3cb.png)

创建新仓库秘密的截图 [图片来源：作者]。

*GCP_CREDENTIALS* 秘密包含了你虚拟机管理员服务账户的 JSON 密钥内容。通过设置这一点，CI/CD 流水线将使用该服务账户来对虚拟机进行身份验证。

由于文件内容是 JSON 格式，为了正确格式化它，你需要执行以下步骤：

安装 ***jq*** CLI 工具：

```py
sudo apt update
sudo apt install -y jq
jq - version
```

格式化你的 JSON 密钥文件：

```py
jq -c . /path/to/your/admin-vm.json
```

获取此命令的输出并用它创建你的 *GCP_CREDENTIALS* 秘密。

*GCP_SSH_PRIVATE_KEY* 是你的 GCP 私有 SSH 密钥（不是你的个人密钥 — GCP 会自动创建一个额外的），它是在你使用 SSH 连接到 GCP 虚拟机时在本地计算机上创建的。

要复制它，请运行以下命令：

```py
cd ~/.ssh
cat google_compute_engine
```

复制终端中的输出并创建 *GCP_SSH_PRIVATE_KEY* 变量。

## **运行 CI/CD 流水线**

现在对代码进行任何更改，推送到主分支，GitHub Actions 文件应该会自动触发。

检查你 GitHub 仓库的 *“Actions”* 标签页以查看其结果。

![](img/2f0dbd5116f137e359a12e45ba458765.png)

GitHub Actions 运行日志的截图 [图片来源：作者]。

将触发两个动作。其中一个将构建和部署 *ml-pipeline* 模块到你的 *ml-pipeline GCP VM*，另一个将构建和部署 *web app* 到你的 *app GCP VM*。

# 结论

恭喜你！你完成了 **Full Stack 7-Steps MLOps Framework** 课程的**最后一课**。这意味着你现在是一名全栈 ML 工程师 🔥

对于这篇高度技术性的文章，我再次表示歉意。它可能不是一篇非常有趣的阅读材料，但却是完成本系列的关键步骤。

在第 7 课中，你学习了如何：

+   手动将 3 个 ML 流水线部署到 GCP；

+   手动将 Web 应用程序部署到 GCP；

+   构建一个 CI/CD 流水线，以使用 GitHub Actions 自动化部署过程。

现在你明白了如何通过部署你的 ML 系统并使其工作来增加实际业务价值，是时候构建你的精彩 ML 项目了。

*没有项目是完美构建的，这个项目也不例外。*

因此，[查看我们额外的课程](https://medium.com/towards-data-science/imperfections-unveiled-the-intriguing-reality-behind-our-mlops-course-creation-6ff7d52ecb7e) **The** **Full Stack 7-Steps MLOps Framework** 课程，我们将 *公开讨论其他设计选择*，这些选择可以用来 *改善在本课程中构建的 ML 系统*。

我真诚地感谢你选择我的课程来学习 MLE 和 MLOps✌️

在 [LinkedIn](https://www.linkedin.com/in/pauliusztin/) 上联系我，并告诉我如果你有任何问题，或者分享你在完成这个课程后构建的精彩项目。

[***在这里访问 GitHub 仓库。***](https://github.com/iusztinpaul/energy-forecasting)

💡 我的目标是帮助机器学习工程师在设计和生产机器学习系统方面提升水平。关注我 [LinkedIn](https://www.linkedin.com/in/pauliusztin/) 或订阅我的 [每周通讯](https://pauliusztin.substack.com/) 以获取更多见解！

🔥 如果你喜欢阅读这样的文章并希望支持我的写作，考虑 [成为 Medium 会员](https://pauliusztin.medium.com/membership)。使用 [我的推荐链接](https://pauliusztin.medium.com/membership)，你可以在不增加额外费用的情况下支持我，同时享受对 Medium 丰富故事库的无限访问。

[](https://pauliusztin.medium.com/membership?source=post_page-----96f676f72012--------------------------------) [## 使用我的推荐链接加入 Medium - 保罗·尤斯丁

### 🤖 加入以获取关于设计和构建生产就绪的机器学习系统的独家内容 🚀 解锁完整访问…

pauliusztin.medium.com](https://pauliusztin.medium.com/membership?source=post_page-----96f676f72012--------------------------------)

谢谢 ✌🏼 ！

# 参考资料

[1] [丹麦 API 的 DE35 行业代码能源消耗](https://www.energidataservice.dk/tso-electricity/ConsumptionDE35Hour)，[丹麦能源数据服务](https://www.energidataservice.dk/about/)

[2] [使用 IAP 进行 TCP 转发](https://cloud.google.com/iap/docs/using-tcp-forwarding)，GCP 文档

[3] [TCP 转发概述](https://cloud.google.com/iap/docs/tcp-forwarding-overview)，GCP 文档

[4] Google Cloud Collective，[如何在 Google Compute Engine 中打开特定端口如 9090](https://stackoverflow.com/questions/21065922/how-to-open-a-specific-port-such-as-9090-in-google-compute-engine) (2017)，Stackoverflow

[5] ANTHONY HEDDINGS，[如何在 GCP Compute Engine 实例上打开防火墙端口](https://www.howtogeek.com/devops/how-to-open-firewall-ports-on-a-gcp-compute-engine-instance/) (2020)，How-To Geek

[6] [为 IAP TCP 转发准备您的项目](https://cloud.google.com/iap/docs/using-tcp-forwarding#preparing_your_project_for_tcp_forwarding)，GCP 文档

[7] [保留一个静态外部 IP 地址](https://cloud.google.com/compute/docs/ip-addresses/reserve-static-external-ip-address)，GCP 文档

[8] [创建和删除服务账号密钥](https://cloud.google.com/iam/docs/keys-create-delete)，GCP 文档

[9] Tom Roth，[在 Google Cloud 虚拟机上安装 Docker](https://tomroth.com.au/gcp-docker/) (2018)，Tom Roth 博客

[10] [分叉一个仓库](https://docs.github.com/en/get-started/quickstart/fork-a-repo)，GitHub 文档

[11] [GCP GitHub Actions 仓库](https://github.com/google-github-actions/ssh-compute)，GitHub
