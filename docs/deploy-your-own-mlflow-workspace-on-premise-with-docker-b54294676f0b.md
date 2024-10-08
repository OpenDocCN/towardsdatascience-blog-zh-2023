# 在本地使用 Docker 部署自己的 MLflow 工作区

> 原文：[`towardsdatascience.com/deploy-your-own-mlflow-workspace-on-premise-with-docker-b54294676f0b`](https://towardsdatascience.com/deploy-your-own-mlflow-workspace-on-premise-with-docker-b54294676f0b)

## 像专业人士一样管理你的 ML 模型生命周期

[](https://tinztwinspro.medium.com/?source=post_page-----b54294676f0b--------------------------------)![Janik and Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----b54294676f0b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b54294676f0b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b54294676f0b--------------------------------) [Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----b54294676f0b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b54294676f0b--------------------------------) ·8 分钟阅读·2023 年 4 月 17 日

--

![](img/fa03e98f888a6fd039fc9344664945b8.png)

图片由 [Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[MLflow](https://mlflow.org) 是一个 **开源平台**，用于端到端管理 ML 模型的生命周期。它跟踪每次 ML 实验的代码、数据和结果，这意味着你可以随时查看所有实验的历史记录。每位数据科学家的梦想。此外，MLflow 是 **库无关** 的，这意味着你可以使用所有 ML 库，如 TensorFlow、PyTorch 或 scikit-learn。所有 MLflow 功能都可以通过 [REST API](https://mlflow.org/docs/latest/rest-api.html#rest-api)、[CLI](https://mlflow.org/docs/latest/cli.html)、[Python API](https://mlflow.org/docs/latest/python_api/index.html)、[R API](https://mlflow.org/docs/latest/R-api.html) 和 [Java API](https://mlflow.org/docs/latest/java_api/index.html) 访问。

作为数据科学家，你花费大量时间优化 ML 模型。最佳模型通常依赖于最佳的超参数或特征选择，而找到最佳组合具有挑战性。此外，你还需要记住所有实验，这非常耗时。MLflow 是解决这些挑战的高效平台。

在这篇文章中，我们简要介绍了 MLflow 的基础知识，并展示了如何在本地设置 MLflow 工作区。我们在 Docker 堆栈中设置了 MLflow 环境，以便在所有系统上运行它。在这个背景下，我们设置了 Postgres 数据库、SFTP 服务器、JupyterLab 和 MLflow 追踪服务器 UI。让我们开始吧。

# MLflow 基础知识

目前，MLflow 提供了四个组件来管理 ML 生命周期。下图显示了一个概述。

![](img/6322e381f4201221dcd3172e5b3ba394.png)

MLflow 组件概述（图片来源：作者）

**MLflow 追踪**用于追踪和查询实验。它跟踪模型参数、代码、数据和模型工件。此外，MLFlow 的追踪服务器提供了一个 web 界面，显示所有实验的历史记录。MLflow 库已经提供了 web 界面。追踪服务器区分不同的实验。您可以通过可视化结果来比较实验中的 ML 模型。

**MLflow 项目**是一个用于以可重用和可复现的方式打包数据科学代码的组件。

**MLflow 模型**格式为使用不同库（例如 TensorFlow、PyTorch 或 scikit-learn）创建的 ML 模型提供了统一的存储格式。统一格式可以在多种环境中进行部署。

**模型注册表**组件允许将生成的模型从暂存阶段转移到生产阶段。它使得在中央模型库中管理 ML 模型成为可能。

您可以在[官方文档](https://mlflow.org/docs/latest/index.html)或 MLflow 的[GitHub 仓库](https://github.com/mlflow/mlflow)中了解更多关于组件的信息。

# 技术要求

您将需要以下先决条件：

+   您的机器上必须安装最新版本的 Docker。如果您尚未安装，请按照[说明](https://docs.docker.com/get-docker/)进行操作。

+   您的机器上必须安装最新版本的 Docker Compose。请按照[说明](https://docs.docker.com/compose/install/)进行操作。

+   访问终端（macOS、Linux 或 Windows）。

# 设置 MLflow 工作区

首先，您应该检查是否正确安装了 Docker 和 Docker Compose。打开您选择的终端并输入以下命令：

```py
$ docker --version
# Output: $ Docker version 20.10.23
```

如果安装正确，您可以看到 Docker 版本（也许您有不同的版本）。接下来，您可以检查 Docker Compose 的安装情况。

```py
$ docker-compose --version
# Output: Docker Compose version v2.15.1
```

好了，一切正常。现在我们可以开始设置 MLflow 工作区了。

有[几种方法](https://mlflow.org/docs/latest/tracking.html#how-runs-and-artifacts-are-recorded)可以使用 MLflow。您可以在本地主机上使用 MLflow，或者为生产环境部署一个完整的栈。本文将重点介绍第二种选项。我们已经设置了一个生产就绪的 MLflow 工作区，我们将在本文中进行说明。

请从我们的 GitHub 仓库下载或克隆[MLflow 工作区](https://github.com/tinztwins/mlflow-workspace)。该项目包含四个服务，如下图所示。

![](img/e93e48a936372041b38426e4f5712a47.png)

各种服务 MLflow 工作区（图像由作者提供）

您可以通过在终端中执行以下命令来启动所有服务。

```py
$ sh start_docker_stack.sh
```

第一次启动时，所有 Docker 镜像下载需要一些时间。现在正是去喝咖啡的时候。☕️

一切启动后，请打开你选择的 web 浏览器。接下来，你可以通过输入以下网址 [`127.0.0.1:8888`](http://127.0.0.1:8888/) 来访问 Jupyter 服务器。你可以使用密码 `mlflow` 登录。接下来，访问 MLflow UI 网站 [`127.0.0.1:5001`](http://127.0.0.1:5001/)。请注意，我们在本地主机上。如果你在远程服务器上运行 MLflow 工作区，请指定你服务器的 IP 地址。

你可以通过运行笔记本 `/notebooks/mlflow_example.ipynb` 来测试 MLflow 工作区。如果没有出现错误，则设置成功。祝贺你！

当你完成在笔记本上的工作后，可以关闭工作区。工作区会持久保存你的工作，以便你下次继续工作。你可以使用以下命令关闭工作区：

```py
$ sh stop_docker_stack.sh
```

完成设置后，我们可以更深入地查看各个服务。

## JupyterLab

该服务提供了一个 JupyterLab 环境。在 MLflow 工作区中，我们使用了来自 DockerHub 的 [jupyter/scipy-notebook](https://hub.docker.com/r/jupyter/scipy-notebook) 镜像。如果你愿意，也可以使用其他 Jupyter 镜像。未来，我们计划将镜像更换为 JuypterHub 镜像，以便每个数据科学家都有自己的账户。这样做的一个好处是，这些账户还能与 MLflow 跟踪服务器进行通信，从而实现更好的用户管理。请期待有关工作区的更多更新。

## SFTP 服务器

该服务提供了远程数据存储。SFTP（安全文件传输协议）是一种文件传输协议，提供对远程计算机的安全访问。有关 SFTP 的更多信息，请阅读以下文章。

[](https://levelup.gitconnected.com/set-up-an-sftp-server-with-docker-353fb513ccfd?source=post_page-----b54294676f0b--------------------------------) [## 使用 Docker 设置 SFTP 服务器

### 免费 — 开源 — 安全

levelup.gitconnected.com](https://levelup.gitconnected.com/set-up-an-sftp-server-with-docker-353fb513ccfd?source=post_page-----b54294676f0b--------------------------------)

在这个项目中，我们使用了来自 DockerHub 的 [atmoz/sftp](https://hub.docker.com/r/atmoz/sftp) 镜像。你也可以使用其他存储技术，如 AWS S3 或 HDFS。在 MLflow 工作区中，SFTP 服务器提供了工件存储。在这个存储中，我们保存 ML 模型以及其他工件，如 Jupyter 笔记本或 Python 脚本。

## MLflow 跟踪服务器

该服务提供了 MLflow UI。这个 web UI 使得管理你的 ML 实验变得简单。你还可以通过 web UI 比较不同的 ML 模型。我们使用了 Docker 镜像 [python](https://hub.docker.com/_/python) 来构建该服务。可以通过 [`127.0.0.1:5001`](http://127.0.0.1:5001/) 访问 web UI。

## Postgres 数据库

该服务为后端存储提供了一个 Postgres 数据库。PostgreSQL 是一个免费的开源关系数据库管理系统。我们使用它来存储参数和评估指标。MLflow 工作区使用来自 DockerHub 的官方[Postgres](https://hub.docker.com/_/postgres) Docker 镜像。

MLflow 工作区正在运行，我们已经理解了服务的基本功能。现在是时候用工作区实现一个实际示例了。

# MLflow 工作区的使用

我们通过一个小的机器学习示例来解释工作区的功能。在这个示例中，我们将尝试在[sklearn moon 数据集](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.make_moons.html)中分离两个类别。我们使用随机森林作为机器学习模型。首先，我们加载数据并进行可视化。

```py
n = 1000
test_size = 0.25
data_seed = 73 

X, y = datasets.make_moons(
    n_samples = n, 
    noise = 0.25, 
    random_state = data_seed)

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size = test_size,
    random_state = 42)

plt.scatter(
    x = X[:,0], 
    y = X[:,1], 
    s = 40, 
    c = y, 
    cmap = plt.cm.Accent);
```

![](img/96c9b37bb71e27ca0560768aa721b204.png)

月亮数据（图片来自作者）

你可以看到我们有两个类别。我们想用随机森林将这两个类别分开。此外，我们还希望跟踪各个实验的指标和参数。我们通过在代码中包含 MLflow 命令来实现这一点。你可以在我们的[GitHub 仓库](https://github.com/tinztwins/mlflow-workspace/blob/main/notebooks/examples/logging/logging_example.ipynb)中查看完整代码。

```py
with mlflow.start_run(run_name='random_forest_model') as run:
    model_rf = RandomForestClassifier(random_state = 42)
    model_rf.fit(X_train, y_train)
    y_pred = model_rf.predict(X_test)

    # metrics
    precision_0 = classification_report(
    y_true=y_test, 
    y_pred=y_pred, 
    target_names=target_names, 
    output_dict=True)["0"]["precision"]

    ...

    f1_score_1 = classification_report(
    y_true=y_test, 
    y_pred=y_pred, 
    target_names=target_names, 
    output_dict=True)["1"]["f1-score"]

    err, acc, tpr, tnr, fnr, fpr = get_confusion_matrix_metrics(y_test=y_test, y_pred=y_pred)

    # log metrics
    mlflow.log_metric("precision_0", precision_0)

    ...

    mlflow.log_metric("f1_score_1", f1_score_1)

    mlflow.log_metric("err", err)
    mlflow.log_metric("acc", acc)
    mlflow.log_metric("tpr", tpr)
    mlflow.log_metric("tnr", tnr)
    mlflow.log_metric("fnr", fnr)
    mlflow.log_metric("fpr", fpr)

    ...

    mlflow.log_artifact("logging_example.ipynb")
```

在使用不同参数运行随机森林模型的代码后，我们的实验会显示在网页用户界面上。

![](img/e0a5fa4af75e3c5b9d1b7601f56ab4dd.png)

MLflow 跟踪用户界面（图片来自作者）

你可以看到两个运行。接下来，我们可以比较这两个运行。为此，勾选这两个运行的复选框，然后点击“比较”。一个新网页会打开，你可以在这里进行详细比较。下图显示了一个示例。

![](img/9e99af1090b6dc942c9461608127853e.png)

两次随机森林运行的比较（图片来自作者）

我们可以看到两个运行的详细信息。MLflow 通过运行 ID 区分运行。它还保存了开始时间、结束时间和运行持续时间。第二部分列出了模型参数。在第一次运行中我们使用了 100 棵树，而在第二次运行中仅用了两棵树。你可以按差异进行筛选，这一点很实用。第三部分列出了我们跟踪的所有指标。指标显示两个模型的效果相似，第一模型稍微好一些。MLflow 提供了许多其他比较功能，例如，你可以在图表中比较不同的指标或参数（平行坐标图、散点图、箱线图和轮廓图）。MLflow 工作区对数据科学家极为有用，因为它跟踪了所有模型组合的指标和代码。它避免了众多实验的混乱，并且你始终可以保持概览。

# 结论

在这篇文章中，我们展示了一个生产就绪的 MLflow 工作区。我们简要描述了使用 Docker 进行设置，并在工作区中运行了一个示例。此外，我们邀请所有数据科学家测试 MLflow 工作区，以便我们可以不断改进它。欢迎反馈。请在评论中写下你的想法。谢谢。

👉🏽 [**你可以在我们的数字产品页面找到所有免费资源！**](https://shop.tinztwins.de/)

👉🏽 [**加入我们免费的每周魔法 AI 新闻通讯，获取最新的 AI 更新！**](https://magicai.tinztwins.de)

[**免费订阅**](https://tinztwinspro.medium.com/subscribe) **以便在我们发布新故事时获得通知：**

[](https://tinztwinspro.medium.com/subscribe?source=post_page-----b54294676f0b--------------------------------) [## 每当 Janik 和 Patrick Tinz 发布文章时，获取电子邮件通知。

### 每当 Janik 和 Patrick Tinz 发布文章时，您会收到电子邮件。通过注册，您将创建一个 Medium 账户（如果您还没有的话）...

tinztwinspro.medium.com](https://tinztwinspro.medium.com/subscribe?source=post_page-----b54294676f0b--------------------------------)

在我们的 [关于页面](https://medium.com/@tinztwinspro/about) 了解更多关于我们的信息。别忘了在 [X](https://twitter.com/tinztwins) 上关注我们。非常感谢你的阅读。如果你喜欢这篇文章，欢迎分享。**祝你有美好的一天！**

使用 [我们的链接](https://tinztwinspro.medium.com/membership) 注册 Medium 会员，以便阅读无限量的 Medium 故事。
