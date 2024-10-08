# FastAPI 和 Streamlit：你必须了解的 Python 双雄

> 原文：[`towardsdatascience.com/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243`](https://towardsdatascience.com/fastapi-and-streamlit-the-python-duo-you-must-know-about-72825def1243)

## [完整的 7 步 MLOps 框架](https://towardsdatascience.com/tagged/full-stack-mlops)

## 第 6 课：使用 FastAPI 和 Streamlit 消耗和可视化模型预测。对所有内容进行 Docker 化

[](https://pauliusztin.medium.com/?source=post_page-----72825def1243--------------------------------)![Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----72825def1243--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72825def1243--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----72825def1243--------------------------------) [Paul Iusztin](https://pauliusztin.medium.com/?source=post_page-----72825def1243--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72825def1243--------------------------------) ·阅读时间 14 分钟·2023 年 6 月 12 日

--

![](img/7beb5df667e2b1bf00cd37002c98447f.png)

图片由 [Hassan Pasha](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上的照片

本教程代表**7 节课程中的第六部分课**，将逐步指导你如何**设计、实现和部署 ML 系统**，并应用**MLOps 良好实践**。在课程中，你将构建一个生产级模型，用于预测丹麦多个消费者类型在接下来的 24 小时内的能源消耗水平。

*完成本课程后，你将理解如何设计、编码和部署一个使用批量服务架构的 ML 系统的所有基础知识。*

本课程*针对希望通过构建自己的端到端项目来提升技能的中级/高级机器学习工程师*。

> *如今，证书随处可见。构建先进的端到端项目并展示是获得专业工程师认可的最佳方式。*

# 目录：

+   课程简介

+   课程内容

+   数据来源

+   第 6 课：使用 FastAPI 和 Streamlit 消耗和可视化模型预测。对所有内容进行 Docker 化。

+   第 6 课：代码

+   结论

+   参考文献

# 课程简介

***在这 7 节课程结束时，你将知道如何：***

+   设计一个批量服务架构

+   使用 Hopsworks 作为特征存储

+   设计一个从 API 读取数据的特征工程管道

+   构建一个带有超参数调优的训练管道

+   使用 W&B 作为 ML 平台来跟踪实验、模型和元数据

+   实现一个批量预测管道

+   使用 Poetry 构建你自己的 Python 包

+   部署你自己的私有 PyPi 服务器

+   用 Airflow 编排一切

+   使用预测来编码一个使用 FastAPI 和 Streamlit 的 Web 应用

+   使用 Docker 将你的代码容器化

+   使用 Great Expectations 确保数据验证和完整性

+   监控预测结果的性能随时间的变化

+   将所有内容部署到 GCP

+   使用 GitHub Actions 构建 CI/CD 管道

如果这听起来很多，不用担心。在你完成这个课程后，你将理解我之前所说的一切。最重要的是，你会知道我为什么使用这些工具以及它们如何作为一个系统协同工作。

**如果你想最大限度地从这个课程中受益，** [**我建议你访问包含所有课程代码的 GitHub 仓库**](https://github.com/iusztinpaul/energy-forecasting) **。本课程旨在快速阅读和复制文章中的代码。**

到课程结束时，你将知道如何实现下图。即使有些内容对你来说不太明白，也不用担心。我会详细解释一切。

![](img/4b5c3b0b8e2162ea8fd268ca745199ec.png)

课程中你将构建的架构示意图 [作者提供的图片]。

到**第 6 课结束时**，你将知道如何使用 FastAPI 和 Streamlit 从 GCP 存储桶中消费预测和监控指标。

# 课程内容：

1.  [批量服务。特征存储。特征工程管道。](https://medium.com/towards-data-science/a-framework-for-building-a-production-ready-feature-engineering-pipeline-f0b29609b20f)

1.  [训练管道。ML 平台。超参数调整。](https://medium.com/towards-data-science/a-guide-to-building-effective-training-pipelines-for-maximum-results-6fdaef594cee)

1.  [批量预测管道。用 Poetry 打包 Python 模块。](https://medium.com/towards-data-science/unlock-the-secret-to-efficient-batch-prediction-pipelines-using-python-a-feature-store-and-gcs-17a1462ca489)

1.  私有 PyPi 服务器。用 Airflow 编排一切。

1.  使用 GE 进行数据质量和完整性验证。模型性能的持续监控。

1.  **使用 FastAPI 和 Streamlit 消费和可视化你的模型预测。将一切 Docker 化。**

1.  [将所有 ML 组件部署到 GCP。使用 GitHub Actions 构建 CI/CD 管道。](https://medium.com/towards-data-science/seamless-ci-cd-pipelines-with-github-actions-on-gcp-your-tools-for-effective-mlops-96f676f72012)

1.  [[附加内容] ‘不完美’ ML 项目的幕后 — 经验教训和见解](https://medium.com/towards-data-science/imperfections-unveiled-the-intriguing-reality-behind-our-mlops-course-creation-6ff7d52ecb7e)

查看 第 3 课 了解我们如何计算和存储 GCP 存储桶中的预测结果。

此外，在第五部分，你可以查看我们如何计算监控指标，这些指标也存储在 GCP 存储桶中。

你将从 GCP 存储桶中获取预测结果和监控指标，并使用 FastAPI 和 Streamlit 将其显示在一个友好的仪表板上。

# 数据源

我们使用了一个免费的开放 API，它提供了丹麦所有能源消费者类型的每小时能源消耗值[1]。

它们提供了一个直观的界面，你可以轻松查询和可视化数据。[你可以在这里访问数据](https://www.energidataservice.dk/tso-electricity/ConsumptionDE35Hour) [1]。

数据具有 4 个主要属性：

+   **小时 UTC：** 观测数据点时的 UTC 日期时间。

+   **价格区域：** 丹麦被划分为两个价格区域：DK1 和 DK2 —— 由大贝尔特分隔。DK1 位于大贝尔特以西，DK2 位于大贝尔特以东。

+   **消费者类型：** 消费者类型为行业代码 DE35，由丹麦能源公司拥有和维护。

+   **总消耗：** 总电力消耗（单位：千瓦时）

**注意：** 观测数据有 15 天的滞后！但对于我们的演示用例，这不是问题，因为我们可以模拟与实时相同的步骤。

![](img/4eab6debdb7ba94406b8d0a8e28e3438.png)

我们的网页应用程序的截图，显示了我们如何预测区域=1 和消费者类型=212 的能源消耗[图片由作者提供]。

数据点具有每小时分辨率。例如：“2023–04–15 21:00Z”，“2023–04–15 20:00Z”，“2023–04–15 19:00Z”等。

我们将数据建模为多个时间序列。每个唯一的**价格区域**和**消费者类型**元组代表一个独特的时间序列。

因此，我们将构建一个模型，为每个时间序列独立预测接下来的 24 小时的能源消耗。

*查看下面的视频以更好地理解数据的样子* 👇

课程和数据源概述 [视频由作者提供]。

# 第六部分：使用 FastAPI 和 Streamlit 获取并可视化模型的预测。将一切 Docker 化。

## 第六部分的目标

在第六部分中，你将构建一个 FastAPI 后端，该后端将从 GCS 中获取预测结果和监控指标，并通过 RESTful API 暴露这些数据。更具体地说，通过一组端点通过 HTTP(S)暴露数据。

此外，你将使用 Streamlit 实现两个不同的前端应用程序：

1.  显示预测的仪表板（即你的应用程序），

1.  显示监控指标的仪表板（即你的监控仪表板）。

两个前端应用程序将通过 HTTP(s)从 FastAPI RESTful API 请求数据，并使用 Streamlit 将数据呈现成一些美丽的图表。

我想强调的是，你可以在 Python 中同时使用这两个框架（FastAPI 和 Streamlit）。这对数据科学家或机器学习工程师非常有用，因为 Python 是他们的终极工具。

![](img/4a7712f71ecc251c99dd732bb42dc92c.png)

最终架构的示意图，Lesson 6 组件以蓝色突出显示 [图片来源：作者]。

请注意，从存储桶中获取预测结果与 3 个管道设计完全解耦。例如，运行 3 个管道：特征工程、训练和推理大约需要 10 分钟。但从存储桶中读取预测结果或监控指标几乎是瞬间完成的。

因此，通过将预测结果缓存到 GCP，你从客户端的角度在线提供了 ML 模型：预测结果是实时提供的。

*这就是批处理架构的魔力。*

接下来的自然步骤是将你的架构从批处理架构迁移到请求-响应或流式架构。

好消息是 FE 和训练管道几乎是相同的，你只需要将批处理预测管道（即推理步骤）迁移到你的 web 基础设施中。[阅读这篇文章以了解使用 Docker 以请求-响应方式部署模型的基础知识。](https://medium.com/faun/key-concepts-for-model-serving-38ccbb2de372)

***为什么？***

因为训练管道将训练模型的权重上传到模型注册表。从那里，你可以根据你的用例需求使用这些权重。

## 理论概念与工具

**FastAPI：** 最新且最著名的 Python API web 框架之一。我尝试过所有顶级 Python API web 框架：Django、Flask 和 FastAPI，我的心属 FastAPI。

***为什么？***

首先，它本地支持异步，这可以用更少的计算资源提升性能。

其次，它使用简单直观，适合各种规模的应用程序。尽管如此，对于庞大的单体应用，我仍然会选择 Django。但这是另一个话题。

**Streamlit：** Streamlit 使得用 Python 轻松创建简单的 UI 组件（主要是仪表盘）变得非常简单。

Streamlit 的范围是让数据科学家和 ML 工程师利用他们最擅长的东西，即 Python，快速构建他们模型的漂亮前端。

*这正是我们所做的*✌️

因此，你将使用 FastAPI 作为后端，Streamlit 作为前端，仅用 Python 构建一个 web 应用。

# Lesson 6：代码

[你可以在这里访问 GitHub 仓库。](https://github.com/iusztinpaul/energy-forecasting)

**注意：** 所有的安装说明都在仓库的 README 文件中。这里你将直接进入代码部分。

*Lesson 6 中的代码位于以下位置：*

+   [***app-api***](https://github.com/iusztinpaul/energy-forecasting/tree/main/app-api) 文件夹 — FastAPI 后端

+   [***app-frontend***](https://github.com/iusztinpaul/energy-forecasting/tree/main/app-frontend) 文件夹 — 预测仪表盘

+   [***app-monitoring***](https://github.com/iusztinpaul/energy-forecasting/tree/main/app-monitoring) 文件夹 — 监控仪表盘

使用 Docker，你可以迅速启动所有三个组件：

```py
docker compose -f deploy/app-docker-compose.yml --project-directory . up --build
```

直接将凭证存储在你的 git 仓库中是一个巨大的安全风险。这就是为什么你将通过**.env**文件注入敏感信息。

**.env.default**是你必须配置的所有变量的示例。它还帮助存储不敏感的属性的默认值（例如，项目名称）。

![](img/87b81fc121cea9485a6b41dd4d656eb8.png)

**.env.default**文件的截图[作者提供的图片]。

## 准备凭证

对于本课程，你需要访问的唯一服务是 GCS。在第 3 课的**准备凭证**部分，我们已经详细解释了如何操作。此外，你还可以在[GitHub README](https://github.com/iusztinpaul/energy-forecasting/blob/main/README.md#gcp)中找到更多信息。

为了保持简洁，在本课程中，我想强调的是，Web 应用的 GCP 服务账户应仅具有读取权限，以保证安全。

**为什么？**

因为 FastAPI API 只会读取 GCP 存储桶中的数据，并且保持最小权限是一种良好的实践。

因此，如果你的 Web 应用被黑客入侵，攻击者只能使用被盗的服务账户凭证读取数据。他不能删除或覆盖数据，这在这种情况下要安全得多。

因此，重复第 3 课的**准备凭证**部分中的相同步骤，但选择*Storage Object Viewer role*角色，而不是*Store Object Admin*角色。

记住，你现在需要下载一个不同的 JSON 文件，其中包含你的 GCP 服务账户密钥，并具有只读访问权限。

查看[README](https://github.com/iusztinpaul/energy-forecasting/blob/main/README.md#the-web-app)了解如何完成**.env**文件。我想强调的是，只有 FastAPI 后端需要加载**.env**文件。因此，你必须将**.env**文件仅放在[***app-api***](https://github.com/iusztinpaul/energy-forecasting/tree/main/app-api)文件夹中。

## FastAPI 后端

FastAPI 后端概述[作者提供的视频]。

提醒一下，FastAPI 代码可以在[***app-api/api***](https://github.com/iusztinpaul/energy-forecasting/tree/main/app-api/api)下找到。

**步骤 1:** 创建 FastAPI 应用，在其中配置文档、CORS 中间件和端点根 API 路由器。

**步骤 2:** 定义 Settings 类。该类的作用是保存 API 代码中需要的所有常量和配置，例如：

+   *通用配置:* 端口、日志级别或版本，

+   *GCP 凭证:* 存储桶名称或 JSON 服务账户密钥的路径。

你将在项目中使用**get_settings()**函数来使用 Settings 对象。

同时，在 **Config** 类中，我们编程使 FastAPI 查找当前目录中的 **.env** 文件，并加载所有以 **APP_API_** 为前缀的变量。

如你在 **.env.default** 文件中所见，所有变量都以 **APP_API_** 开头。

![](img/87b81fc121cea9485a6b41dd4d656eb8.png)

.env.default 文件的截图 [图片由作者提供]。

**步骤 3：** 使用 Pydantic 定义 API 数据的模式。这些模式将数据从 JSON 编码或解码为 Python 对象，反之亦然。同时，它们根据你定义的数据模型验证 JSON 对象的类型和结构。

在定义 Pydantic BaseModel 时，添加类型到每个变量是至关重要的，这将用于验证步骤。

**步骤 4：** 定义你的端点，在网络术语中称为视图。通常，一个视图可以访问某些数据存储，并根据查询，它将数据源的一个子集返回给请求者。

***因此，检索（即 GET 请求）数据的标准流程如下：***

“client → 请求数据 → 端点 → 访问数据存储 → 编码为 Pydantic 模式 → 解码为 JSON → 返回请求的数据”

让我们看看我们是如何定义一个端点来获取所有消费者类型的：

我们使用了 "**gcsfs.GCSFileSystem"** 作为标准文件系统来访问 GCS 存储桶。

我们将端点附加到**api_router**。

使用 **api_router.get()** Python 装饰器，我们将一个基本函数附加到 **/consumer_type_values** 端点。

在上面的示例中，当调用 "**https://<some_ip>:8001/api/v1/consumer_type_values"** 时，将触发 **consumer_type_values()** 函数，端点的响应将严格基于该函数的返回值。

另一个重要的事情是，**通过在 Python 装饰器中定义 response_model（即模式），**你不必显式地创建 Pydantic 模式。

如果你返回一个与模式结构 1:1 匹配的字典，FastAPI 将自动为你创建 Pydantic 对象。

*就这些。现在我们将重复相同的逻辑来定义其余的端点。FastAPI 使一切变得如此简单直观。*

现在，让我们看一下整个 [***views.py***](https://github.com/iusztinpaul/energy-forecasting/blob/main/app-api/api/views.py) 文件，在其中我们定义了以下端点：

+   **/health** → 健康检查

+   **/consumer_type_values** → 获取所有可能的消费者类型

+   **/area_values** → 获取所有可能的区域类型

+   **/predictions/{area}/{consumer_type}** → 获取给定区域和消费者类型的预测。请注意，使用 {<some_variable>} 语法，你可以向端点添加参数 —— [FastAPI 文档](https://fastapi.tiangolo.com/tutorial/path-params/) [2]。

+   **/monitoring/metrics** → 获取汇总的监控指标

+   **/monitoring/values/{area}/{consumer_type}** → 获取给定区域和消费者类型的监控值

我想再次强调，FastAPI 后端只读取 GCS 桶的预测。推理步骤完全在批量预测管道中完成。

你还可以访问“[**http://<your-ip>:8001/api/v1/docs**](http://35.207.134.188:8001/api/v1/docs)**”**来访问 API 的 Swagger 文档，在那里你可以轻松查看和测试所有端点：

![](img/f22922d4e71d0df403068efcff1c0d3c.png)

Swapper API 文档的截图[作者的图片]。

就是这样！现在你知道如何构建 FastAPI 后端了。添加数据库层和用户会话可能会使事情变得更复杂，但你已经掌握了所有主要概念，这将帮助你入门！

## Streamlit 预测仪表板

Streamlit 预测仪表板概述[作者的视频]。

访问[***app-frontend/frontend***](https://github.com/iusztinpaul/energy-forecasting/tree/main/app-frontend/frontend)下的代码。

使用 Streamlit 非常简单。整个 UI 通过下面的代码定义，代码执行以下操作：

+   它定义标题，

+   它向后端请求所有可能的区域类型，并根据此创建一个下拉列表，

+   它向后端请求所有可能的消费者类型，并根据此创建一个下拉列表，

+   基于当前选择的区域和消费者类型，它构建并渲染一个 plotly 图表。

直接了当，对吧？

请注意，我们本可以对 HTTP 请求的状态码进行额外的检查。例如，如果请求状态码与 200 不同，则显示一条文本“服务器宕机”。但我们希望保持简洁，只强调 Streamlit 代码✌️

我们将所有常量移到了一个不同的文件中，以便在整个代码中轻松访问。下一步，你可以通过**.env**文件使其可配置，类似于 FastAPI 的设置。

现在，让我们看看我们是如何构建图表的🔥

这一部分没有 Streamlit 代码，只有一些 Pandas 和 Plotly 代码。

**build_data_plot()**函数执行 3 个主要步骤：

1.  它从 FastAPI 后端请求某个区域和消费者类型的预测数据。

1.  如果响应有效（status_code == 200），则从响应中提取数据并构建一个 DataFrame。否则，它会创建一个空的 DataFrame，以便进一步传递相同的结构。

1.  它使用上述计算的 DataFrame 构建一个折线图——plotly 图表。

**build_dataframe()**函数的作用是接受 2 个列表：

1.  一个日期时间的列表，将作为折线图的 X 轴；

1.  一组将用作折线图 Y 轴的值；

…并将其转换为 DataFrame。如果一些数据点缺失，我们会将日期时间重采样为 1 小时的频率，以确保数据连续并突出显示缺失的数据点。

非常简单，对吧？这就是人们喜欢 Streamlit 的原因。

## Streamlit 监控仪表板

Streamlit 监控仪表板概述[作者的视频]。

监控代码可以在 [***app-monitoring/monitoring***](https://github.com/iusztinpaul/energy-forecasting/tree/main/app-monitoring)*** 下访问。***

你会发现代码几乎与预测仪表板相同。

在定义 Streamlit UI 结构时，我们还实现了一个包含汇总指标和分隔符的图表。

解耦 UI 组件定义和数据访问的好处在于，你可以在 UI 中注入任何数据，只要尊重预期数据的接口，而无需修改 UI。

**build_metrics_plot()** 函数几乎与预测仪表板中的 **build_data_plot()** 函数相同，只是我们从 API 请求的数据不同。

对于监控仪表板中的 **build_data_plot()** 函数也是如此：

如你所见，所有的数据访问和操作都在 FastAPI 后端处理。Streamlit UI 的工作是请求和展示数据。

很高兴我们只重用了 90% 的预测仪表板代码来构建一个友好的监控仪表板。

## 用 Docker 包装一切

最后的步骤是将这三个 web 应用程序 Docker 化，并将它们打包到一个 docker-compose 文件中。

因此，我们可以通过一个命令启动整个 web 应用程序：

```py
docker compose -f deploy/app-docker-compose.yml --project-directory . up --build
```

***这里是*** [***FastAPI Dockerfile***](https://github.com/iusztinpaul/energy-forecasting/blob/main/app-api/Dockerfile)***:***

值得一提的是，我们最初只复制并安装了 Poetry 依赖项。因此，当你修改代码时，Docker 镜像将仅从第 19 行开始重建，即复制你的代码。

这是一种常见的策略，利用 Docker 缓存功能在构建镜像时加快开发过程，因为你很少添加新的依赖项，而安装它们是最耗时的步骤。

此外，在 **run.sh** 中我们调用：

```py
/usr/local/bin/python -m api
```

但是等一下，命令中没有 Python 文件 😟

其实，你可以在模块内部定义一个 **__main__.py** 文件，使你的模块可执行。

因此，当调用 **api** 模块时，你会调用 [**__main__.py**](https://github.com/iusztinpaul/energy-forecasting/blob/main/app-api/api/__main__.py) 文件：

在我们的例子中，在[**__main__.py**](https://github.com/iusztinpaul/energy-forecasting/blob/main/app-api/api/__main__.py) 文件中，我们使用 uvicorn web 服务器来启动 FastAPI 后端，并用正确的 IP、端口、日志级别等进行配置。

**这里是** [**Streamlit 预测仪表板 Dockerfile**](https://github.com/iusztinpaul/energy-forecasting/blob/main/app-frontend/Dockerfile)**:**

如你所见，这个 Dockerfile 几乎与用于 FastAPI 后端的那个相同，除了最后的 **CMD** 命令，这是一个标准的 CLI 命令，用于启动你的 Streamlit 应用程序。

[Streamlit 监控仪表板 Dockerfile](https://github.com/iusztinpaul/energy-forecasting/blob/main/app-monitoring/Dockerfile)与预测仪表板 Dockerfile 完全相同。所以在这里重复粘贴是多余的。

好消息是，你可以利用我之前展示的 Dockerfile 模板来 Docker 化大部分 Python 应用程序✌️

最后，让我们看看如何使用 docker-compose 来完成所有工作。你可以在[***deploy/app-docker-compose.yml***](https://github.com/iusztinpaul/energy-forecasting/blob/main/deploy/app-docker-compose.yml)文件中找到相关内容：

如你所见，前端和监控服务必须等待 API 启动后才能开始。

另外，只有 API 需要从**.env**文件中加载凭证。

现在，你只需运行以下命令，Docker 将处理构建镜像和运行容器：

```py
docker compose -f deploy/app-docker-compose.yml --project-directory . up --build
```

# 总结

恭喜！你完成了**第六课**的**全栈七步 MLOps 框架**课程。这意味着你现在已经理解了如何使用你的机器学习系统的预测来构建你出色的应用程序。

在本课中，你学会了如何：

+   从 GCS 中消费预测和监控指标，

+   构建一个 FastAPI 后端来加载和服务来自 GCS 的数据，

+   在 Streamlit 中实现一个仪表板来展示预测，

+   在 Streamlit 中创建一个监控仪表板来可视化模型的性能。

现在你已经理解了基于批量预测架构的机器学习系统上构建应用程序的灵活性，你可以轻松设计全栈机器学习应用程序。

查看第 7 课，这是**全栈七步 MLOps 框架**的最后一步，即将所有内容部署到 GCP 并使用 GitHub Actions 构建 CI/CD 管道。

**另外，** [**你可以在这里访问 GitHub 仓库**](https://github.com/iusztinpaul/energy-forecasting)**。**

💡 我的目标是帮助机器学习工程师在设计和生产化机器学习系统方面提升水平。关注我在[LinkedIn](https://www.linkedin.com/in/pauliusztin/)或订阅我的[每周通讯](https://pauliusztin.substack.com/)以获取更多见解！

🔥 如果你喜欢阅读这样的文章并希望支持我的写作，请考虑[成为 Medium 会员](https://pauliusztin.medium.com/membership)。使用[我的推荐链接](https://pauliusztin.medium.com/membership)，你可以在不增加额外成本的情况下支持我，同时享受 Medium 丰富故事的无限访问权限。

[](https://pauliusztin.medium.com/membership?source=post_page-----72825def1243--------------------------------) [## 使用我的推荐链接加入 Medium - Paul Iusztin

### 🤖 加入以获取有关设计和构建生产就绪机器学习系统的独家内容🚀 解锁完全访问权限...

[pauliusztin.medium.com](https://pauliusztin.medium.com/membership?source=post_page-----72825def1243--------------------------------)

谢谢✌🏼！

# 参考资料

[1] [丹麦 API 每小时 DE35 行业代码的能源消耗](https://www.energidataservice.dk/tso-electricity/ConsumptionDE35Hour)，[丹麦能源数据服务](https://www.energidataservice.dk/about/)

[2] [路径参数](https://fastapi.tiangolo.com/tutorial/path-params/)，FastAPI 文档
