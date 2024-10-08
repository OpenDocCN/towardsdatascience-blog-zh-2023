# 使用 MLflow 的端到端 ML 管道：追踪、项目和服务

> 原文：[`towardsdatascience.com/end-to-end-ml-pipelines-with-mlflow-tracking-projects-serving-1b491bcdc25f`](https://towardsdatascience.com/end-to-end-ml-pipelines-with-mlflow-tracking-projects-serving-1b491bcdc25f)

## 高级 MLflow 使用教程

[](https://medium.com/@antonsruberts?source=post_page-----1b491bcdc25f--------------------------------)![Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----1b491bcdc25f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1b491bcdc25f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b491bcdc25f--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----1b491bcdc25f--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1b491bcdc25f--------------------------------) ·阅读时间 9 分钟·2023 年 2 月 16 日

--

![](img/537bead127eea662380418af3a0ce3b1.png)

照片由 [Jeswin Thomas](https://unsplash.com/@jeswinthomas?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

MLflow 是一个功能强大的工具，通常因其实验追踪功能而受到关注。这也很容易理解——它是一个用户友好的平台，用于记录机器学习实验的所有重要细节，从超参数到模型。但你知道 MLflow 提供的不仅仅是实验追踪吗？这个多功能框架还包括 MLflow Projects、模型注册和内置的部署选项。在这篇文章中，我们将探讨如何利用这些功能来创建一个完整且高效的 ML 管道。

对于完全没有 MLflow 基础的初学者来说，这个教程可能会有点难度，因此我强烈建议你在深入学习这个教程之前，先观看这两个视频！

# 设置

在这个项目中，我们将在本地进行操作，因此请确保正确设置你的本地环境。项目需要三个主要依赖项——`mlflow`、`pyenv` 和 `kaggle`。虽然可以通过 pip 安装 MLflow，但你需要按照不同的说明来设置 [pyenv](https://github.com/pyenv/pyenv#installation) 和 [kaggle](https://github.com/Kaggle/kaggle-api)。

完成安装后，确保拉取 [这个仓库](https://github.com/aruberts/tutorials) 的最新版本。当你在笔记本电脑上获取了仓库后，我们就可以开始了！

# 项目概述

移动到 `mlflow_models` 文件夹，你将看到以下结构：

![](img/6bfda34601ead55847c0cae7c342b41a.png)

mlflow_models 文件夹结构。图像由作者提供。

这里是这个项目中每个文件的简要概述：

+   `MLProject` — 描述 MLflow 项目的 yaml 风格文件

+   `python_env.yaml` — 列出运行项目所需的所有环境依赖项

+   `train_hgbt.py` 和 `train_rf.py` — 使用特定超参数的 HistGradientBoosterTree 和 RandomForest 模型的训练脚本

+   `search_params.py` — 用于执行超参数搜索的脚本

+   `utils` — 文件夹包含项目中使用的所有实用程序函数

如前所述，本项目是端到端的，因此我们将从数据下载到模型部署。大致的工作流程如下：

1.  从 Kaggle 下载数据

1.  加载和预处理数据

1.  调整随机森林（RF）模型，进行 10 次迭代

1.  注册最佳 RF 模型并将其放入生产桶

1.  使用内置的 REST API 部署模型

完成后，你可以自行重复步骤 2 到 5 以训练 HistGradientBoostedTrees 模型。在进入项目之前，让我们看看这些步骤如何得到 MLflow 的支持。

# MLflow 组件

一般而言，MLflow 有 4 个组件 — Tracking、Projects、Models 和 Registry。

![](img/1bee01ad3f7a3c35ea36d74bac6fa558.png)

MLflow 组件。来自 mlflow.org 的截图

回顾项目步骤，下面是我们将如何使用它们。首先，我使用了**MLflow Projects 来打包代码**，这样你或其他数据科学家/工程师可以重现结果。其次，**MLflow Tracking 服务将跟踪你的调优实验**。这样，你将在下一步中检索到最佳实验，**然后将你的模型添加到模型注册表中。** 从注册表中，部署模型将真正变成一行代码，因为它们以 MLflow Models 格式保存，并具有内置的 REST API 功能。

![](img/1dbaeb97d6d723db175c5b1c7363cd83.png)

项目步骤概览。图片由作者提供。

# 管道概览

## 数据

当你运行管道时，数据将会自动下载。作为示例，我将使用[贷款违约](https://www.kaggle.com/datasets/hemanthsai7/loandefault)数据集（CC0：公共领域许可证），但你可以通过重写`training_data`参数并将列名更改为相关的名称来进行调整。

## MLProject & 环境文件

`MLProject` 文件为你提供了一种方便的方式来管理和组织你的机器学习项目，允许你指定重要的细节，如项目名称、Python 环境的位置和管道的入口点。每个入口点可以通过独特的名称、相关参数和特定命令进行自定义。命令作为可执行的 shell 行，每当入口点被激活时就会执行，并且能够利用先前定义的参数。

`python_env.yaml` 文件概述了执行管道所需的精确 Python 版本，以及所有必需软件包的全面列表。

这两个文件是创建运行项目所需环境所必需的。现在，让我们查看管道将执行的实际脚本（入口点）。

## 训练和实验跟踪

训练是在 `train_rf.py` 和 `train_hgbt.py` 脚本中完成的。它们大体相同，唯一的区别在于传递的超参数和预处理管道。考虑下面的函数，它下载数据并训练一个随机森林模型。

实验在使用 `with mlflow.start_run()` 定义 MLflow 上下文时开始。在此上下文中，我们使用 `mlflow.log_metrics` 来保存 PR AUC 指标（有关更多信息，请查看 `eval_and_log_metrics` 函数）和 `mlflow.sklearn.log_model` 来保存预处理和建模管道。这样，当我们加载管道时，它将一起执行所有预处理和推理。如果你问我，这非常方便！

## 超参数调优

超参数调优使用 `search_params.py` 中的 Hyperopt 包进行。大部分代码来自官方[mlflow 仓库](https://github.com/mlflow/mlflow/tree/master/examples/hyperparam)，但我已经尽量简化了。这个脚本中最棘手的部分是理解如何组织这些调优轮次，使其与“主”项目运行连接起来。本质上，当我们使用 MLflow 运行 `search_params.py` 时，我们要确保实验的结构如下：

![](img/a03435cb340c618b037051fb8a491fff.png)

实验结构的可视化。图片来源于作者

正如你所见，`search_params` 脚本只会指定`train_rf.py` 接下来应该使用哪些参数（例如，10、2 和 5 的深度），以及它的父级运行 ID（在上面的示例中是 1234）。当你探索脚本时，确保注意以下细节。

+   当我们定义 `mlflow.start_run` 上下文时，我们需要确保 `nested` 参数设置为 `True`

+   当我们运行 `train_rf.py`（或 `train_hgbt.py`）时，我们显式传递 `run_id` 并使其等于先前创建的 `child_run` 运行

+   我们还需要传递正确的 `experiment_id`

请查看下面的示例，以了解代码中的全部工作原理。`eval` 函数是将通过 Hyperopt 最小化函数进行优化的函数。

实际的调优函数相对简单。我们所做的就是初始化一个 MLflow 实验运行（所有其他运行的父级运行），并使用提供的搜索空间优化目标函数。

请注意，这些函数只是为了说明代码的主要部分。有关完整的代码版本，请参考[github 仓库](https://github.com/aruberts/tutorials/tree/main/mlflow_models)。

# 运行 RF 流程

到现在为止，你应该对脚本如何工作有一个大致的了解了！所以，让我们用这一行运行随机森林管道：

```py
mlflow run -e search_params --experiment-name loan . -P model_type=rf
```

让我们分解这个命令行：

+   `mlflow run .` 意味着我们想要在此文件夹中运行项目

+   `-e search_params` 指定了我们要运行的 MLProject 文件中的入口点。

+   `--experiment-name loan` 将实验名称设置为“**loan**”。你可以将其设置为任何你想要的名称，但请记下，因为之后会用到。

+   `-P model_type=rf` 将`search_params`脚本中的`model_type`参数设置为“**rf**”（即随机森林）。

当我们运行这行代码时，应该会发生四件事：

1.  Python 虚拟环境将被创建。

1.  名为“**loan**”的新实验将被初始化。

1.  Kaggle 数据将被下载到新创建的文件夹`data`中。

1.  超参数搜索将开始。

实验完成后，我们可以在 MLflow UI 中检查结果。要访问它，只需在命令行中使用命令`mlflow ui`。在 UI 中，选择“loan”实验（或你给它起的任何名字），并将你的指标添加到实验视图中。

![](img/79bb66edb952c8cdde684cead0d3aeb2.png)

MLflow UI 截图。图像由作者提供。

最佳的 RF 模型在测试 PR AUC 中达到了 0.104，训练时间为 1 分钟。总体来说，超参数调整大约需要 5 分钟完成。

# 注册模型

到目前为止，我们已经训练、评估并保存了 10 个随机森林模型。从理论上讲，你可以直接去 UI 中找到最佳模型，手动将其注册到模型注册表并提升到生产环境。然而，更好的方法是通过代码进行，因为这样可以自动化这一步骤。这正是`model_search.ipynb`笔记本所涵盖的内容。使用它来跟随下面的各个部分。

首先，我们需要找到最佳模型。要程序化地完成此操作，你需要收集所有超参数调整实验（10 个）并按测试指标进行排序。

你的结果可能会有所不同，但主要目标是得到正确的`best_run`参数。请注意，如果你更改了实验名称，你也需要在此脚本中更改它。父级运行 ID 可以在 UI 中查找，点击父实验（在这种情况下名为**“capable-ray-599”**）。

![](img/1bc70d87384fefc8a70426d83f30d99b.png)

运行 ID 查找 MLflow UI。截图由作者提供。

为了测试模型是否按预期工作，我们可以轻松地将其加载到笔记本中。

如果你成功得到了预测——恭喜，你已经做对了一切！最后，注册模型并将其提升到生产环境也非常简单。

运行这 2 行代码会注册你的模型，并将其内部提升到“Production”桶。所有这些操作只是改变了我们访问模型及其元数据的方式，但在模型版本控制的背景下，这非常强大。例如，随时可以比较版本 1 和版本 2。

![](img/1170edae42bc8d8f2edf4aa3abd34132.png)

MLflow 模型注册表。截图由作者提供。

如果你进入 UI 的“Models”选项卡，你确实会看到一个名为`loan_model`的模型，其版本 1 目前在生产环境中。这意味着我们现在可以通过其名称和阶段来访问该模型，这非常方便。

# 服务模型

最简单的模型服务方式是在本地进行。这通常用于测试端点，并确保我们获得预期的输出。使用 MLflow 服务模型非常简单，特别是当我们已经注册了模型时。你只需运行以下命令：

```py
mlflow models serve — model-uri models:/loan_model/Production -p 5001
```

这条命令将启动一个本地服务器，该服务器将在 5001 端口托管你的模型（即`loan_model`，目前处于`Production`阶段）。这意味着你可以向`localhost:5001/invocations`端点发送请求，并获取预测结果（前提是请求格式正确）。

要在本地测试端点，你可以使用`requests`库来调用它并获取预测结果。

在上面的示例中，我们获得了与之前相同的概率，但现在这些分数是由本地服务器而不是你的脚本生成的。输入需要遵循非常具体的指南，这就是为什么我们有 4 行预处理的原因。你可以在[这里](https://mlflow.org/docs/latest/models.html#deploy-mlflow-models)阅读更多关于 MLflow 服务的期望格式的信息。

# 总结

如果你已经做到这些并且一切正常——给自己一个热烈的掌声吧！我知道这需要消化很多内容，所以让我们总结一下你迄今为止取得的成就。

1.  你看到了并理解了如何使用 MLflow Projects 来构建你的项目。

1.  你了解我们在脚本中记录参数、指标和模型的位置，以及`search_params.py`如何调用`train_rf.py`。

1.  你现在可以运行 MLflow Projects 并在 MLflow UI 中查看结果。

1.  你知道如何找到最佳模型，如何将其添加到模型注册中心，以及如何以编程方式将其推广到生产环境。

1.  你可以在本地从模型注册中心提供模型，并调用端点进行预测。

# 接下来做什么？

我强烈建议你通过尝试运行梯度提升树模型的管道并部署 HGBT 模型来检验你的技能。所有必要的脚本都已提供，因此只需你配置管道并完成部署。如果遇到任何挑战或有任何问题，请不要犹豫，在评论区留下你的问题。
