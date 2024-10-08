# 《Azure 机器学习工作室简介》

> 原文：[`towardsdatascience.com/azure-machine-learning-studio-a21abfc3b1aa?source=collection_archive---------13-----------------------#2023-01-16`](https://towardsdatascience.com/azure-machine-learning-studio-a21abfc3b1aa?source=collection_archive---------13-----------------------#2023-01-16)

## 模型创建、部署和评分

[](https://medium.com/@jonathanbogerd?source=post_page-----a21abfc3b1aa--------------------------------)![Jonathan Bogerd](https://medium.com/@jonathanbogerd?source=post_page-----a21abfc3b1aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a21abfc3b1aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a21abfc3b1aa--------------------------------) [Jonathan Bogerd](https://medium.com/@jonathanbogerd?source=post_page-----a21abfc3b1aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3863776b2716&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fazure-machine-learning-studio-a21abfc3b1aa&user=Jonathan+Bogerd&userId=3863776b2716&source=post_page-3863776b2716----a21abfc3b1aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a21abfc3b1aa--------------------------------) ·8 分钟阅读·2023 年 1 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa21abfc3b1aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fazure-machine-learning-studio-a21abfc3b1aa&user=Jonathan+Bogerd&userId=3863776b2716&source=-----a21abfc3b1aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa21abfc3b1aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fazure-machine-learning-studio-a21abfc3b1aa&source=-----a21abfc3b1aa---------------------bookmark_footer-----------)

在本文中，我们将涵盖在 Azure 机器学习工作室中创建、部署和使用模型所需的所有步骤。在之前的系列中，我已经介绍过这个主题。然而，在过去一年中，Azure 机器学习工作室（AML Studio）进行了许多更新，包括 Python SDK 的新版本。因此，需要对这个系列进行更新。本文的结构如下：首先，我们将介绍 AML Studio 的概况，然后创建计算资源和环境。第三，我们将创建一个数据存储并上传数据。接下来，在本文的剩余部分，我们将创建模型、部署模型并进行测试。

![](img/d2e74147ca6b8ef25a96f90eea6114d0.png)

由[Ricardo Resende](https://unsplash.com/@rresenden?utm_source=medium&utm_medium=referral)拍摄的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**概述介绍**

Azure Machine Learning Studio 是微软 Azure 平台上的机器学习套件。它可以用于创建、部署和使用模型，包括数据和模型的版本控制，并且可以与低代码和 SDK 选项一起使用。在本文中，我们将讨论 Python SDK（V2），但也请务必查看 AML Studio 的低代码和 AutoML 功能。

AML Studio 包含用于创建脚本的笔记本，其中可以使用 SDK 来创建计算、环境和模型。要运行笔记本，我们首先需要一个计算，你可以通过计算选项卡创建。我们还将展示如何使用 SDK 创建新的计算，但你首先需要一个计算来运行所需的命令。笔记本可以直接在 AML Studio 中创建和更改，或者你可以连接到 Visual Studio Code 工作区（如果你愿意）。有关如何做到这一点的详细信息，请参见[此处](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-setup-vs-code)。

在本文中，我们将在 AML Studio 中创建笔记本。在开始之前，有一个非常方便的快捷键可以运行笔记本中的所有单元格：Alt+R。在我们可以在工作区中创建任何内容之前，我们首先需要进行身份验证以获取工作区句柄。这可以通过运行以下脚本来完成：

这是我们在本文中创建的所有其他笔记本所必需的，因此请始终先运行此命令。现在，让我们开始创建一个计算。

**创建计算**

可以使用 UI 创建计算，确保创建具有所需规格的计算实例。然后可以使用该计算运行例如笔记本。以下代码片段可以用来使用 Python SDK 创建新的计算实例：

如果你需要多个节点，你还可以创建一个计算集群。实现这一点的代码略有不同，下面有示例代码：

**创建环境**

为了训练和部署模型，我们还需要一个指定所需安装包的环境。你还可以提供包的版本，以确保代码按预期运行，并且只有在你首先测试了这些包后才会更新它们。AML Studio 中的环境与例如 Conda 环境非常相似。要创建它们，我们将首先创建一个名为‘dependencies’的子目录。然后我们将创建一个用于训练和部署模型的.yml 文件。完成后，我们需要触发实际环境的创建。AML Studio 中的环境可以基于微软维护的预定义镜像。详细说明请参见[此处](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-manage-environments-v2?tabs=python)。

以下代码在 AML Studio 中创建并注册一个环境：

**添加数据**

在这篇文章中，我们将使用来自 Kaggle 的数据来训练模型。我们使用的数据集可以在[Ahsan Raza 提供的这里](https://www.kaggle.com/datasets/ahsan81/hotel-reservations-classification-dataset)找到，并且在 Creative Commons 4.0 许可证下提供。

任务是预测客户是否会取消预订。为了使用这个数据集，我们首先需要将其上传到连接到 AML 工作区的 blob 存储中的容器。一旦完成，我们可以通过在 AML Studio 中选择 Data 选项卡来创建一个数据存储区（如果尚未创建）。在那里，通过 UI 注册数据存储区。现在，我们可以通过使用新的 AzureMachineLearningFileSystem 类来访问文件。为此，需要数据集的完整 URI，可以通过选择数据存储区，选择文件并复制 URI 来获得。

默认情况下，azureml-fsspec 包未安装，因此在使用之前，我们需要通过魔法命令 `%pip` 安装它。用于打开文件的代码如下：

**创建模型**

为了创建模型，我们需要创建一个包含训练模型逻辑的 Main.py 文件。为此，我们首先创建一个目录来存储该文件。接下来，我们将编写主文件，类似于我们为 Environment 编写 .yml 文件的方式。然而，请确保在提交为作业之前先测试所有组件。这将节省你很多时间，因为创建作业的时间相当长，大约 10 分钟。

首先，我们需要一种方法来向训练脚本添加参数。我们将使用 argparse 模块来实现这一点。为了日志记录，我们将使用 mlflow 模块。autolog 方法使得不需要编写评分脚本，因为这会自动完成。处理好这些后，让我们构建实际的模型吧！

我们将使用一个非常基础的 sklearn 设置来创建模型。如果你还没有安装 sklearn，请确保安装。首先，我们将数据集分为训练集和测试集。由于这仅仅是一个展示 AML Studio 的示例，我们不会使用验证集或复杂模型。数据集包含数值特征和分类特征。因此，我们将使用 sklearn 的 OneHotEncoder 和 StandardScaler。我们将这些保存到一个包含逻辑回归模型的管道中。这样创建管道可以确保我们对训练数据和测试数据应用完全相同的转换，这些数据稍后会发送到模型中。在拟合模型和变换器之后，我们保存并注册模型。这将产生如下代码：

现在我们有了一个 Main.py 文件，我们需要运行一个命令来执行该文件并注册模型。一个命令包含多个元素，并结合了我们迄今为止采取的所有步骤。首先，我们在字典中提供 Main.py 文件中所有变量的输入。接下来，我们指定代码的位置和实际命令。我们需要一个可以运行此代码的环境，因此我们指定了之前创建的环境。计算也是如此。命令的最后两个部分是实验和显示名称，用于 AML Studio。在触发此作业后，将提供一个包含详细页面的链接，在该页面中你可以跟踪模型的进展。最重要的是，你还可以查看详细的日志，以调试你遇到的任何错误。

**模型部署**

现在我们已经创建了一个模型，我们需要将模型部署到一个端点才能使用它。为此，我们需要执行两个步骤。首先，我们创建一个端点，其次，我们将模型部署到该端点。在本文中，我们将创建一个在线端点。如果你需要处理大量数据，创建一个批量端点可能更合适。执行这个过程非常相似，关于批量端点的更多细节可以在[这里](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-use-batch-endpoint?tabs=python)找到。

创建在线端点非常简单。你需要指定一个名称，并可以选择认证到端点的方法。提供的两个选项是密钥和 aml_token。主要区别在于密钥不会过期，而令牌会。在此示例中，我们将使用密钥。可以向端点添加标签，以提供额外的信息，例如所使用的数据和模型类型。标签需要以字典形式提供。下面是创建在线端点的代码。

在创建端点之后（这需要几分钟时间），我们需要将模型部署到该端点。首先，我们通过名称检索模型的最新版本。然后，我们需要指定部署配置。请注意，instance_type 设置了运行端点所用的计算类型，并且必须从可用的实例列表中选择。指定配置后，我们需要运行部署。这同样需要几分钟才能完成。

**评分**

在创建和部署模型后，是时候测试它了！我们可以将请求文件发送到端点，并接收预测。可以在 Endpoint 选项卡中找到从 AML Studio 外部执行此操作的代码和密钥。那里提供了 C#、Python 和 R 的模型消费代码。然而，让我们首先直接从 AML Studio 笔记本测试模型。为此，我们创建一个包含 input_data 的 JSON 文件。它需要三个值。首先，我们在列表中指定列。使用与模型训练期间完全相同的列名非常重要。接下来，我们指定一个索引列表，计算所需的预测数量。最后，我们创建一个列表，每个数据输入一个列表。数据输入的顺序必须与列列表中的顺序一致。下面您可以找到我们创建的模型的示例和将此文件发送到端点所需的代码。如果您不再需要端点，请务必删除它并终止计算实例。执行此操作的代码在同一示例中提供，并在最后一行注释掉。

**结论**

在本文中，我们使用 Azure 机器学习 Studio 创建了计算、环境和模型。然后，我们部署了模型，并使用示例数据进行了测试。再次提醒，如果您不再需要端点，请不要忘记删除它并终止计算实例。现在就到这里，祝您使用 AML Studio 进行数据科学项目愉快！

**来源**

[](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-train-model?tabs=python&source=post_page-----a21abfc3b1aa--------------------------------) [## 训练机器学习模型 - Azure 机器学习

### 适用于：Azure CLI ml 扩展 v2（当前） Python SDK azure-ai-ml v2（当前）要使用 SDK 信息，请安装…

learn.microsoft.com](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-train-model?tabs=python&source=post_page-----a21abfc3b1aa--------------------------------) [](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-access-data-interactive?tabs=adls&source=post_page-----a21abfc3b1aa--------------------------------) [## 在互动开发期间从 Azure 云存储访问数据 - Azure 机器学习

### 适用于：Python SDK azure-ai-ml v2（当前）通常，机器学习项目的开始涉及…

learn.microsoft.com](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-access-data-interactive?tabs=adls&source=post_page-----a21abfc3b1aa--------------------------------) [](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-train-scikit-learn?source=post_page-----a21abfc3b1aa--------------------------------) [## 训练 scikit-learn 机器学习模型（v2） - Azure 机器学习

### 适用于：Python SDK azure-ai-ml v2（当前）在本文中，了解如何运行您的 scikit-learn 训练脚本…

[learn.microsoft.com](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-train-scikit-learn?source=post_page-----a21abfc3b1aa--------------------------------)
