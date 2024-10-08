# 通过使用端到端机器学习平台加速构建机器学习模型

> 原文：[`towardsdatascience.com/accelerate-building-ml-models-by-using-an-end-to-end-ml-platform-a4f2fe5c05b1`](https://towardsdatascience.com/accelerate-building-ml-models-by-using-an-end-to-end-ml-platform-a4f2fe5c05b1)

## 指导性教程：构建文本分类器

[](https://medium.com/@konkiewicz.m?source=post_page-----a4f2fe5c05b1--------------------------------)![Magdalena Konkiewicz](https://medium.com/@konkiewicz.m?source=post_page-----a4f2fe5c05b1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4f2fe5c05b1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4f2fe5c05b1--------------------------------) [Magdalena Konkiewicz](https://medium.com/@konkiewicz.m?source=post_page-----a4f2fe5c05b1--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4f2fe5c05b1--------------------------------) ·阅读时间 8 分钟·2023 年 3 月 21 日

--

![](img/7f20e3c7d470f9c3f0abe2bb0a9f5696.png)

图片来源于[OpenIcons](https://pixabay.com/users/openicons-28911/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=97860)，[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=97860)

**介绍**

作为数据科学家，我们熟悉用于构建机器学习算法的标准 Python 库。你很可能已经使用过 pandas、NumPy、scikit-learn、TensorFlow 或 PyTorch 来构建模型，然后使用 Docker、Kubernetes 以及大型云服务商来部署和服务这些模型。这些都是优秀的工具，提供了很多灵活性，但将所有这些组件连接起来往往并不那么简单。最终，从初步数据收集到构建模型，再到最终在生产环境中服务，往往需要几周时间。

机器学习平台的创建旨在简化这一过程，允许用户使用相同的工具在更短的时间内完成所有必要的步骤。这提高了生产力，允许快速原型设计，并最终可以减少开发机器学习解决方案所需的预算。

在这篇文章中，我想介绍如何使用一个新的[机器学习平台](https://tolokamodels.tech/home)构建文本分类模型，并且你可以在不到一个小时的时间里完成这一过程，而无需编写代码。你将通过从目录中选择一个预训练模型来进行建模，不需要 GPU 或自己的资源来训练或部署模型。这种方法比从头开始构建自己的解决方案要快得多，非常适合快速原型设计。

让我们开始吧。

**问题陈述——文本分类**

在本教程中，我们将构建一个文本分类器，以区分三个类别：政治、健康和娱乐。为了训练模型，我们将使用相对较小的样本，共 298 个对象，分布如下：

+   政治类 142 个实例

+   健康类 80 个实例

+   娱乐类 76 个实例。

这些条目是新闻类别数据集的一部分，该数据集由 [Rishabh Misra](https://rishabhmisra.github.io/) 在 [创作共用 4.0 国际 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) 许可证下发布。你可以从 [这里](https://drive.google.com/file/d/1zzS65JpjRHaYWSkCDV3aPwotemrrGOsh/view?usp=sharing) 下载本教程所需的样本。

**选择基础模型**

作为我们分类器的基础，我们将使用在 Toloka 机器学习平台上提供的预训练多语言大规模变换器模型。你可以通过这个 [链接](https://tolokamodels.tech/home) 注册该工具的免费版本。之后，你将可以访问平台 [模型/集合](https://tolokamodels.tech/models/podium) 部分中的各种预训练机器学习模型。

![](img/0ac1d4240f28f7ea118692fc40562c68.png)

预训练模型预览 — 作者图像

两个模型适合作为我们最终模型的基础：[多语言大规模变换器](https://tolokamodels.tech/models/568af330-28d8-4567-a184-872f3650cdf9?tabs-model=Interaction) 和 [多类别文本分类](https://tolokamodels.tech/models/0c21a74b-4a95-4aa2-b610-298819381833?tabs-model=Train)。这两个模型都由大型预训练变换器驱动，能够让你创建新的自然语言处理模型，而无需投入标注大量数据集、进行大量实验或设置 GPU 云。

在本教程中，我们将使用 [多语言大规模变换器](https://tolokamodels.tech/models/568af330-28d8-4567-a184-872f3650cdf9?tabs-model=Interaction)，但你可以随意尝试 [多类别文本分类](https://tolokamodels.tech/models/0c21a74b-4a95-4aa2-b610-298819381833?tabs-model=Train) 并比较结果。

你可以通过在 [提示](https://tolokamodels.tech/models/568af330-28d8-4567-a184-872f3650cdf9?tabs-model=Interaction) 中输入文本并检查模型的响应来尝试其推理。记住，这些是通用模型的结果，我们仍然需要为我们的任务对其进行微调。

![](img/1302bb06a5edbf57c11ac64972610f98.png)

作者截图

[多语言大规模变换器](https://tolokamodels.tech/models/568af330-28d8-4567-a184-872f3650cdf9?tabs-model=Interaction) 根据你的需求有两个微调选项。第一个是针对文本生成的微调。在这种情况下，你只需提供将用于任务的文本，模型将能够生成类似于你提供的数据的文本。

第二个选项是对文本分类模型进行微调，对于训练数据，你需要提供文本和标签的示例。我们将使用这个选项，因为我们的任务是分类新闻。要继续微调，只需点击模型部分的[Tune Classifier tab](https://tolokamodels.tech/models/568af330-28d8-4567-a184-872f3650cdf9?tabs-model=Tune+Classifier)选项卡，如下图所示。

![](img/787e23362b395b1d2de1599bf25f39d6.png)

作者截图

在这之后，你将被要求使用现有的数据集进行微调或创建一个新的数据集。如果你之前没有使用过这个平台，你需要选择后者，因为你还没有上传任何数据集。只需按照下面截图中的[*“创建你的数据集”*](https://tolokamodels.tech/datasets/creation)链接进行操作。

![](img/777a3abf04c079c1aca098e1405bbb4f.png)

作者截图

**创建数据集**

要在平台上创建数据集，你需要上传一个包含训练分类器所需实例的[CSV 文件](https://drive.google.com/file/d/1zzS65JpjRHaYWSkCDV3aPwotemrrGOsh/view?usp=sharing)。你还需要为其指定名称和简短描述。

![](img/59478884af2f87951e5d5a6efd0035a1.png)

作者截图

一旦你创建了数据集，你应该能看到其概览信息，如下图所示：

![](img/2f859613e53c33516b0520f3fc67e5e4.png)

作者截图

数据集是有版本的，第一版的名称总是带有 0.0 扩展名。它对应于你上传的原始数据。

你可以通过点击数据集版本旁边的“*标记此版本*”按钮来注释数据集（如上图所示）。在数据标注的第一步中，你将被询问你拥有的数据类型。我们正在处理文本，因此选择此选项，并选择应作为文本和标签使用的列。

![](img/c83aacab157d72bfd0977d53939fb8e7.png)

作者截图

在下一步中，你将被要求定义标签。我们在创建数据集时提供的 CSV 文件已经指定了三种不同的类别：POLITICS、WELLNESS 和 ENTERTAINMENT，这些应该会被平台自动识别。

![](img/acc6a67d50695f7d9ef5e89c43b00f5c.png)

作者截图

我们可以直接进入下一步，因为我们只会使用这些类别。

现在你将被要求标注每个单独的实例。我们在 CSV 文件中已经提供了带有标签的列，因此我们应该已经能够看到每个条目的标签。

![](img/fd204048f9db397b30c58de7e49a6ed2.png)

作者截图

这是你可以检查并修正标签的时间，如果需要，或者如果你的训练数据没有标签，你可以在这里添加它们。

这是标注部分的最后一步，所以完成后你应该会有数据集的版本 0.1。这个标注版本将用于微调模型。

**微调现有模型**

现在你的数据集已经准备好，你可以返回到[Models/Collections](https://tolokamodels.tech/models/podium)部分，选择[Multilingual Large Transformer](https://tolokamodels.tech/models/568af330-28d8-4567-a184-872f3650cdf9?tabs-model=Interaction)模型，并继续到“调优分类器”部分。

系统会要求你填写有关用于微调的训练数据的信息。我们希望使用之前步骤中创建的标记版本的数据集，请相应填写详细信息。

![](img/9f02a6915fba1f380889d8095d4685b8.png)

作者截图

在这一步，你还需要为模型提供一个名称和简短的描述。

完成后，按下“运行场景”按钮，微调将会触发。完成后，你将能够打开模型并使用提示进行测试。

**测试模型**

让我们在文本上运行模型推断。你可以在模型提示中输入这个短语：

*“美国总统上周访问了德国。”*

![](img/b87663d7de43492a29367536fbcacb0a.png)

作者截图

它被分类为政治！

你刚刚微调了一个大型语言变换模型，该模型旨在预测序列中的下一个单词。现在它可以将新闻分类为三个类别：政治、健康和娱乐。

此外，你通过使用仅 300 个训练样本，并且不到一个小时的时间完成了这一目标。你的模型完美吗？可能不，仍需要进一步改进，但令人惊讶的是，即使使用未标记的数据集，你也能如此迅速地原型化和测试新的机器学习想法。

如果你现在想使用我们的模型，你可以随时通过平台的[Models/Collections](https://tolokamodels.tech/models/registry)部分访问它。

**进一步改进**

你在本教程中使用的[ML platform](https://tolokamodels.tech/home)目前处于 Beta 版本，并且不断开发新的功能。下一步的功能将包括实验跟踪、模型比较功能以及指标可视化工具。

有经验的机器学习从业者如果寻找更高级的功能并通过代码与平台互动，也不会感到失望。用于实现这一点的 Python 库即将推出。

**总结**

在这篇文章中，你已经学习了如何使用[Toloka ML platform](https://tolokamodels.tech/home)快速构建一个简单的文本分类模型。我鼓励你根据自己的需求调整这个示例，创建独特的文本分类器或情感分析模型。

此外，你还可以尝试针对其他数据类型（如图像或音频）设计的模型。

玩得开心，并评论你在平台上喜欢的地方以及哪些部分可以改进。

*PS：我在 Medium 上撰写了以简单易懂的方式解释基本数据科学概念的文章，* [***关于数据的博客***](https://www.aboutdatablog.com/)*。你可以订阅我的* [***邮件列表***](https://medium.com/subscribe/@konkiewicz.m) *，每次我发布新文章时都会通知你。如果你还不是 Medium 会员，你可以* [***在这里***](https://medium.com/@konkiewicz.m/membership)*加入。*

*下面是一些你可能会喜欢的其他帖子：*

[](/detecting-and-fixing-data-drift-in-computer-vision-8d0853af1246?source=post_page-----a4f2fe5c05b1--------------------------------) ## 计算机视觉中的数据漂移检测与修复

### 实际案例研究，包含你可以运行的代码

towardsdatascience.com [](/top-8-magic-commands-in-jupyter-notebook-c1582e813560?source=post_page-----a4f2fe5c05b1--------------------------------) ## Jupyter Notebook 中的前 8 个神奇命令

### 通过学习最有用的命令提升你的生产力

towardsdatascience.com [](/how-to-do-data-labeling-versioning-and-management-for-ml-ba0345e7988?source=post_page-----a4f2fe5c05b1--------------------------------) ## 如何进行数据标注、版本管理和 ML 管理

### 关于丰富食品数据集的案例研究

towardsdatascience.com [](/jupyter-notebook-autocompletion-f291008c66c?source=post_page-----a4f2fe5c05b1--------------------------------) ## Jupyter Notebook 自动补全

### 如果你还没有使用，这将是你应当使用的数据科学生产力工具...

towardsdatascience.com
