# 如何使用 FastAPI 和 Google Cloud Run 部署和测试你的模型

> 原文：[`towardsdatascience.com/how-to-deploy-and-test-your-models-using-fastapi-and-google-cloud-run-82981a44c4fe`](https://towardsdatascience.com/how-to-deploy-and-test-your-models-using-fastapi-and-google-cloud-run-82981a44c4fe)

## 在这个端到端教程中学习如何将你的模型转变为一个在云中运行的服务

[](https://medium.com/@antonsruberts?source=post_page-----82981a44c4fe--------------------------------)![Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----82981a44c4fe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----82981a44c4fe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----82981a44c4fe--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----82981a44c4fe--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----82981a44c4fe--------------------------------) ·阅读时间 12 分钟·2023 年 3 月 8 日

--

![](img/109864dbe39d0cc00200a8a32e4dab4a.png)

图片由 [Taylor Vick](https://unsplash.com/ko/@tvick?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

MLOps（机器学习运维）在数据专业人员中越来越受欢迎。能够将机器学习（ML）项目从训练到生产的全栈数据科学家需求不断增加，所以如果你在这一领域感觉有些不适，或者想要快速复习一下，这篇文章就是为你准备的！如果你从未部署过模型，我强烈建议你查看我之前的文章，它会更详细地介绍 ML 部署，并解释许多在本文中会用到的概念。

[](/introduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17?source=post_page-----82981a44c4fe--------------------------------) ## 机器学习部署简介：Flask、Docker 和 Locust

### 学习如何在 Python 中部署你的模型，并使用 Locust 测量性能

towardsdatascience.com

本帖将涵盖相当多的话题，所以可以随意跳到你最感兴趣的章节。本文的结构如下：

1.  **使用 FastAPI 指定推理端点的 API**

1.  **使用 Docker 将端点容器化**

1.  **将 Docker 镜像上传到 GCP 的 Artifact Registry**

1.  **将镜像部署到 Google Cloud Run**

1.  **监控推理速度并进行负载测试**

![](img/7b310e2855a73fa27a1ecbf856928cea.png)

项目步骤概述。图片由作者提供。

# 设置

首先，你需要在本地计算机上安装 `fastapi`、`uvicorn` 和 `Docker` 来完成前两步。第三步和第四步需要你拥有一个已启用 `Artifact Registry` 和 `Cloud Run` API 的有效 `Google Cloud Platform` 账户。幸运的是，GCP 提供了免费试用期和免费额度，你可以使用 Gmail 账户激活。此外，你还需要安装 `gcloud` CLI 工具，以便通过 CLI 与 GCP 账户进行交互。最后，你还需要安装 Locust 进行负载测试。具体的安装步骤会根据你的系统有所不同，但下面你可以找到所有安装说明的链接。

+   [安装 FastAPI 和 Uvicorn](https://fastapi.tiangolo.com/tutorial/)

+   [下载并安装 Docker](https://docs.docker.com/get-docker/)

+   [获取免费 GCP 账户](https://cloud.google.com/free)

+   [安装 gcloud CLI](https://cloud.google.com/sdk/docs/install-sdk)

+   [安装 Locust](https://docs.locust.io/en/stable/installation.html)

> 所有代码都可以在 [这个 GitHub 仓库](https://github.com/aruberts/tutorials/tree/main/deployment/fastapi) 中找到。一定要拉取代码并跟着操作，因为这是最好的学习方法（在我看来）。

# 项目概述

为了使学习更加实用，本文将展示如何部署一个贷款违约预测模型。模型训练过程超出了本文的范围，但你可以在 [Github 仓库](https://github.com/aruberts/tutorials/tree/main/deployment/fastapi) 找到一个已经训练好的模型。该模型是在经过预处理的 [美国小企业管理局数据集](https://www.kaggle.com/datasets/mirbektoktogaraev/should-this-loan-be-approved-or-denied)（CC BY-SA 4.0 许可证）上训练的。欢迎探索数据字典以了解每一列的含义。

本项目的主要目标是创建一个工作端点（称为 `/predict`），该端点将接收包含贷款申请信息（即模型特征）的 POST 请求，并输出一个包含违约概率的响应。

![](img/41e332a243b1327c9c909717142cc832.png)

简化的机器学习预测系统。图像由作者提供。

既然这些都处理完了，我们终于可以开始了！首先，让我们看看如何使用 FastAPI 库创建一个推理 API。

# 创建推理 API

## 什么是 API？

API（应用程序编程接口）是一组已建立的协议，用于外部程序与您的应用程序进行交互。为了更直观地解释一下，这里有一个来自 [Stack Overflow 上一个很好的回答](https://stackoverflow.com/questions/6220618/what-does-it-mean-to-create-an-api-on-the-web) 的引用：

> Web APIs 是在网络服务器上运行的应用程序的入口点，允许其他工具以某种方式与该网络服务交互。可以将其视为软件的“用户界面”。最终，任何 web API 其实就是一系列 URL 和如何与它们交互的说明，*仅此而已*。

换句话说，我们的 API 将负责指定如何与托管 ML 模型的应用程序进行交互。更准确地说，它将指定：

+   预测服务的 URL 是什么

+   服务期望的输入信息

+   服务将提供哪些信息作为输出

## FastAPI

FastAPI 是一个用于开发 Web API 的 Python 框架。它与 Flask 非常相似（在我 之前的文章 中有介绍），但有一些显著的优势。

+   内置数据验证使用 Pydantic

+   使用 SwaggerUI 的自动文档生成

+   使用 ASGI 的异步任务支持

+   更简单的语法

该框架的创建者基本上探索了构建 API 的其他所有解决方案，决定了他喜欢哪些功能，并将所有这些良好功能合并到了 FastAPI 中（我们对此非常感激，所以不要忘记给 [github 仓库](https://github.com/tiangolo/fastapi) 点个星）。

## 输入和输出验证

在创建端点之前，定义预期的输入和输出信息类型是很重要的。FastAPI 接受 JSON 请求和响应，但必须指定所需字段及其数据类型。为此，FastAPI 使用了 [Pydantic](https://docs.pydantic.dev/)，它在运行时验证类型提示，并在提交无效数据时提供清晰且用户友好的错误消息。

Pydantic 强制执行这些数据类型的方式是通过数据模型。数据模型只是继承自 `pydantic.BaseModel` 的 Python 类，并将预期字段及其类型列为类变量。上面你可以看到我如何在 `LoanApplication` 类中指定预期的输入，以及在 `PredictionOut` 类中指定预期的输出。这种严格的验证可以帮助你在开发过程中尽早发现错误，防止它们在生产中引发问题。我们还可以为这些参数指定可接受的范围，但我现在会跳过这一步。

## 应用程序和端点创建

FastAPI 用于创建应用程序并定义作为 API 的不同端点。这里有一个非常简单的 FastAPI 应用示例，其中包含一个预测端点。

让我们解析一下上面的代码：

1.  它导入必要的库并加载一个已经保存到文件中的预训练 CatBoost 模型

1.  它通过将 `FastAPI` 类分配给 `app` 变量来创建一个 FastAPI 应用实例

1.  它通过用 `@app.post("/predict", response_model=PredictionOut)` 装饰器装饰一个函数来定义一个用于进行预测的 API 端点，该端点期望 POST 请求。`response_model` 参数指定了 API 端点的预期输出格式。

1.  `predict`函数接收一个负载，该负载预计是一个名为`LoanApplication`的 Pydantic 模型的实例。这个负载用于创建一个 pandas DataFrame，然后用这个 DataFrame 使用预训练的`catboost`模型生成预测。预测的违约概率作为 JSON 对象返回。

请注意，之前定义的输入和输出数据类是如何发挥作用的。我们使用装饰器中的`response_model`参数指定期望的响应格式，并在`predict`函数中将请求格式作为输入类型提示。非常简单而干净，但非常有效。

就这样！在几行代码中，我们创建了一个带有预测端点（API）和完整数据验证能力的应用程序。要在本地启动此应用程序，你可以运行这个命令。

```py
uvicorn app:app --host 0.0.0.0 --port 80
```

如果一切顺利，你会看到应用程序在指定的地址上运行。

![](img/de344917331f9ea5a547173527fd9f6e.png)

截图由作者提供

## 使用 Swagger UI 的文档

这是 FastAPI 的一个酷炫之处——我们免费获得了文档！前往[**http://0.0.0.0:80/docs**](http://0.0.0.0:80/docs)，你应该能看到一个 Swagger UI 屏幕。

![](img/d2593915d4bab07b8326db21283924d2.png)

Swagger UI。截图由作者提供。

在同一屏幕上，你可以通过点击我们的 POST 端点（绿色的）并编辑默认请求体来测试你的 API。然后，点击执行，你将看到 API 的响应。

![](img/d292bf9bf95fcf1c29033bac55a0de06.png)

用户界面中的请求体。截图由作者提供。

如果你正确地指定了所有内容，你应该会收到响应代码 200，这意味着请求已成功。此响应的主体应包含之前指定的`default_probability`的 JSON，这是我们`/predict`端点的输出。

![](img/aa00fe6f08323137cb94e6495cdf7c1a.png)

用户界面中的响应体。截图由作者提供。

当然，你也可以使用`request`库以编程方式测试 API，但用户界面为你提供了一个不错且直观的替代方案。

# 使用 Docker 对应用程序进行容器化

正如我在之前的[帖子](https://medium.com/towards-data-science/introduction-to-ml-deployment-flask-docker-locust-b87b5bd78a17)中提到的，容器化是将应用程序及其所有依赖项（包括 Python）封装成一个自包含的、隔离的包的过程，这样可以在不同的环境（例如本地、云端、朋友的笔记本等）中一致地运行。FastAPI 有一个关于如何在 Docker 中运行 FastAPI 的极佳文档，如果你对更多细节感兴趣，可以查看[这里](https://fastapi.tiangolo.com/deployment/docker/)。

下面，你可以看到我们 API 的 Dockerfile（Docker 指令集）。

除了最后运行的命令外，它与我们在 Flask 中使用的完全相同。以下是对这个 Dockerfile 实现的简要描述：

1.  `FROM python:3.9-slim`：指定容器的基础镜像为 Python 版本 3.9 的 slim 变体，相较于普通 Python 镜像更小

1.  `WORKDIR /app`：这个指令将容器内部的工作目录设置为 `/app`

1.  `COPY requirements.txt requirements.txt`：这个指令将 `requirements.txt` 文件从主机复制到容器的 `/app` 目录

1.  `RUN pip install --upgrade pip & RUN pip install -r requirements.txt`：这个指令升级 `pip` 并使用 `pip` 安装 `requirements.txt` 文件中指定的所需包

1.  `COPY ["loan_catboost_model.cbm", "app.py", "./"] .`：这个指令将训练好的 CatBoost 模型和包含应用程序 Python 代码的 `app.py` 文件从主机复制到容器的 `/app` 目录

1.  `CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "80"]`：这个指令指定了容器启动时应该运行的命令。它运行 `uvicorn` 服务器，以 `app` 模块作为主应用程序，URL 为 `0.0.0.0:80`

## 构建 Docker 镜像

一旦 Dockerfile 被定义，我们需要基于它构建一个 Docker 镜像。构建镜像在本地非常简单，你只需在包含 Dockerfile 和应用代码的文件夹中运行以下命令即可。

```py
docker build -t default-service-fastpai:latest .
```

这将创建一个名为 `default-service-fastapi` 的镜像，并将其保存在本地。你可以在 CLI 中运行 `docker images` 来查看它是否在你拥有的其他 Docker 镜像中列出。

## 将镜像推送到 Google Artifact Registry

将此镜像推送到 Google Artifact Registry 的过程稍微复杂一些，但[Google](https://cloud.google.com/artifact-registry/docs/docker/pushing-and-pulling)提供了非常详尽的文档。这个过程包括五个步骤：

1.  配置 Docker 权限以访问你的 Artifact Registry

1.  在你的 GCP 中启用 Artifact Registry

1.  在 Registry 中创建一个仓库

1.  使用特定的命名约定标记你的镜像

1.  推送镜像

首先，你需要确定要存储容器的位置。你可以在[这里](https://cloud.google.com/artifact-registry/docs/repositories/repo-locations)找到完整的列表。我将选择`europe-west2`，所以我用来验证我的 Docker 的命令是：

```py
gcloud auth configure-docker europe-west2-docker.pkg.dev
```

接下来的两个步骤可以在你的 GCP UI 中完成。前往导航菜单中的 Artifact Registry，点击按钮以启用 API。然后，前往仓库并点击 **创建仓库** 按钮。在弹出的菜单中，指定这是一个 Docker 仓库，并将其位置设置为之前选择的那个位置（对我而言是 `europe-west2`）

![](img/396363d0e8cef31425f00c210fa61110.png)

Google Artifact Registry。作者截图。

所有设置完成后，让我们开始主要部分。为了用合适的名称标记镜像并推送它，我们需要运行两个特定的命令。

```py
docker tag IMAGE-NAME LOCATION-docker.pkg.dev/PROJECT-ID/REPOSITORY/IMAGE-NAME
docker push LOCATION-docker.pkg.dev/PROJECT-ID/REPOSITORY/IMAGE-NAME
```

由于镜像名称是 `default-servise-fastapi`，仓库名称是 `ml-images`，而我的项目 ID 是 `rosy-flames376113`，所以对我来说，命令如下：

```py
docker tag default-servise-fastapi europe-west2-docker.pkg.dev/rosy-flames-376113/ml-images/default-servise-fastapi
docker push europe-west2-docker.pkg.dev/rosy-flames-376113/ml-images/default-servise-fastapi
```

命令执行可能需要一些时间，但一旦完成，你将能够在你的 Artifact Registry 界面中看到你的 Docker 镜像。

顺便提一下，你可以在 GCP UI 中通过点击项目名称旁边的下拉按钮（下例中的“我的第一个项目”）来查找你的项目 ID 或创建一个新项目。

![](img/49da8f89293c5d16e97fa33f3c50e6e0.png)

GCP UI。截图由作者提供。

# 在 Google Cloud Run 上部署容器

实际的部署步骤在 GCP 中相对简单。你需要做的唯一事情是通过进入导航菜单中的 **Cloud Run** 部分并点击启用 API 的按钮来启用 Cloud Run API。

在 Cloud Run 中创建服务有两种方式 — 通过 CLI 或在 UI 中。我将向你展示如何使用 CLI，你可以自行探索 UI 选项。

```py
 gcloud run deploy default-service \
      --image europe-west2-docker.pkg.dev/rosy-flames-376113/ml-images/default-service-fastapi \
      --region europe-west2 \
      --port 80 \
      --memory 4Gi
```

该命令将从之前上传的 Artifact Registry 镜像中创建一个 `default-service`（即我们的 API）。此外，它还指定了区域（与我们的 Docker 镜像相同）、要暴露的端口以及可用于服务的 RAM。你可以在 [官方文档](https://cloud.google.com/sdk/gcloud/reference/run/deploy) 中查看你可以指定的其他参数。

![](img/937a02cab0e345f245d353fc3303a175.png)

如果你在命令行中看到一个 URL — 恭喜！你的模型已经正式部署了！

# 测试部署

与之前的帖子类似，我将使用 Locust 来测试部署。这里是将要测试的测试场景 — 用户发送带有贷款信息的请求，并接收带有默认概率的响应（虽然不现实，但适用于我们的目的）。

在本地 Flask 部署的情况下，我的笔记本电脑在开始出现故障之前可以处理多达 ~180 个请求每秒。此外，响应时间随着每秒请求数量的增加而逐渐增加。让我们对我们的云部署进行测试，看看它的表现是否更好。要启动测试，请运行 `locust -f app_test.py`（`app_test.py` 包含上述测试场景），并在默认位于 **http://0.0.0.0:8089** 的 UI 中输入你的服务器的 URL。我将用户限制设置为 300，生成速率为 5，但你可以根据需要进行更改。

> **请注意 Cloud Run 按请求计费**，所以要谨慎设置这些参数！

![](img/822f938fd0d4a7b8d8d2a9deeff7b56f.png)![](img/0026493fd63420c28ae190a4c0d1515e.png)

本地部署（左）与云部署（右）负载测试。图表由作者提供。

当流量达到峰值后，这里是我为我的服务器看到的图表。首先，服务器在每秒 300 个请求的情况下没有崩溃，这真是好消息。这意味着云部署比在本地机器上运行模型更为可靠。其次，中位响应时间更低，并且在流量增加的情况下几乎保持不变，这意味着该部署性能也更佳。

然而，在第 95 百分位数中有两个显著的峰值——测试开始时和接近结束时。第一个峰值可以解释为 Cloud Run 服务器在开始接收流量之前保持空闲。这意味着服务需要预热，因此在开始时可能会出现较低的速度甚至一些失败。第二个峰值可能是由于服务器开始接近其容量。然而，你可能会注意到，在测试结束时，第 95 百分位数速度开始下降。这是因为我们的服务开始自动扩展（感谢 Google！），正如你在下面的仪表板中所看到的。

![](img/fe4dc24a9bbe5b91d2066c6a598c12d2.png)

Cloud Run 中的指标仪表板。作者截图。

在峰值时，该服务实际上运行了 12 个实例，而不是 1 个。我认为这是像 Cloud Run 这样的托管服务的主要优势——你不需要担心部署的可扩展性。Google 会负责添加新资源（当然只要你能支付），并确保你的应用程序平稳运行。

# 结论

这段经历非常精彩，所以让我们总结一下我们在这篇文章中所做的一切：

+   使用 FastAPI 创建了一个带有推理端点的基本 API

+   使用 Docker 对 API 进行了容器化

+   构建了 Docker 镜像并将其推送到 GCP 中的 Artifact Registry

+   使用 Google Cloud Run 将此图像转化为服务

+   使用 Locust 测试了部署的鲁棒性和推理速度

非常棒，继续阅读这些章节！我希望现在你对 ML 过程中的部署步骤感到更自信，并且可以将自己的模型投入生产。如果你还有任何问题，请告诉我，我会在下一个帖子中尽量解答。

# 还不是会员？

[](https://medium.com/@antonsruberts/membership?source=post_page-----82981a44c4fe--------------------------------) [## 使用我的推荐链接加入 Medium - Antons Tocilins-Ruberts

### 阅读 Antons Tocilins-Ruberts 的每一个故事（以及 Medium 上的其他成千上万的作者的故事）。你的会员费直接…

medium.com](https://medium.com/@antonsruberts/membership?source=post_page-----82981a44c4fe--------------------------------)
