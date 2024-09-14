# 使用笔记本风格工作区简化 dbt 模型开发

> 原文：[`towardsdatascience.com/streamline-dbt-model-development-with-notebook-style-workspace-eb156fe6e81`](https://towardsdatascience.com/streamline-dbt-model-development-with-notebook-style-workspace-eb156fe6e81)

## 互动式构建和编排数据模型

[](https://khuyentran1476.medium.com/?source=post_page-----eb156fe6e81--------------------------------)![Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----eb156fe6e81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb156fe6e81--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb156fe6e81--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----eb156fe6e81--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb156fe6e81--------------------------------) ·7 分钟阅读·2023 年 6 月 5 日

--

![](img/fd11a3304be743c99f498095736e978a.png)

作者提供的图片

*最初发布于* [*https://mathdatasimplified.com*](https://mathdatasimplified.com/2023/06/05/dbt-mage-interactively-build-and-orchestrate-data-models/) *2023 年 6 月 5 日。*

# 动机

dbt（数据构建工具）是一个强大的数据仓库数据转换工具。

然而，它确实存在一些限制，包括以下几点：

1.  **缺乏输出预览：** 使用 dbt core 时，无法在构建模型之前预览模型的输出，这可能会阻碍对数据转换过程的验证和迭代。

1.  **特征工程的局限性：** 由于 SQL 是 dbt 的主要语言，因此在执行复杂的特征工程任务时存在一定的局限性。为了执行超出 SQL 能力的复杂特征工程，可能需要额外的工具或语言，如 Python。

1.  **部分 ETL 解决方案：** 虽然 dbt 在数据转换方面表现出色，但它并未提供全面的端到端解决方案来处理数据加载、数据提取和编排等任务。

为了缓解这些挑战，dbt Cloud 提供了诸如开发数据模型、预览输出和通过用户友好的网页界面调度 dbt 作业等功能。然而，随着项目数量的增加，使用 dbt Cloud 的成本可能会变得相当高。

![](img/4b76d92f30a0e5e0369276c6808a37fc.png)

作者提供的图片

# Mage + dbt 集成

dbt cloud 的一个免费替代品是 [Mage](https://www.mage.ai/)，这是一个开源的数据管道工具，用于数据转换和集成任务。

Mage 无缝地与 dbt 互补，带来了诸多好处，包括：

1.  **集成的基于网页的 IDE：** Mage 提供了一个方便的基于网页的 IDE，你可以在一个界面内轻松开发和探索数据模型。

1.  **语言灵活性：** 使用 Mage，你可以将不同工具和语言的优势与 dbt 结合，以增强数据处理能力。

1.  **可视化 dbt 模型输出：** Mage 提供了内置的可视化功能，允许用户通过几次点击轻松地可视化 dbt 模型生成的输出。

1.  **数据提取和加载：** 除了数据转换，Mage 还提供了数据提取和加载的功能，实现更全面的端到端数据管道解决方案。

1.  **管道调度和重试机制：** Mage 允许你调度数据管道并自动重试失败的组件，确保数据集成过程的顺利和可靠执行。

让我们深入探讨这些功能。

随意通过克隆这个 GitHub 仓库来探索和实验源代码：

[](https://github.com/khuyentran1401/dbt-mage?source=post_page-----eb156fe6e81--------------------------------) [## GitHub - khuyentran1401/dbt-mage

### 当前无法执行该操作。你在另一个标签页或窗口中登录了。你在另一个标签页或窗口中登出了…

[github.com](https://github.com/khuyentran1401/dbt-mage?source=post_page-----eb156fe6e81--------------------------------)

# 设置

## 安装 Mage

你可以通过 Docker、pip 或 conda 安装 Mage。本文将使用 Docker 安装 Mage 并初始化项目。

```py
docker run -it -p 6789:6789 -v $(pwd):/home/src mageai/mageai /app/run_app.sh mage start [project_name]
```

例如，我们将项目命名为“dbt_mage”，因此命令变为：

```py
docker run -it -p 6789:6789 -v $(pwd):/home/src mageai/mageai /app/run_app.sh mage start dbt_mage
```

其他安装 Mage 的方式请参考 [这里](https://docs.mage.ai/getting-started/setup)。

## 创建管道

在浏览器中打开 [`localhost:6789/`](http://localhost:6789/) 查看 Mage UI。

点击“新建”，选择“标准（批量）”来创建一个新的批量管道。将其重命名为“dbt_pipeline”。

![](img/aefab4440f8d9a685302053608dfebbf.png)

作者提供的图片

## 安装依赖

由于我们将使用 BigQuery 作为 dbt 的数据仓库，我们需要通过将 `dbt-bigquery` 添加到“requirements.txt”文件中并点击“安装包”来安装它。

![](img/df8ce2927179b6bddbd000755ee3c758.png)

作者提供的图片

## 创建 dbt 项目

要创建 dbt 项目，请导航到右侧面板并点击终端按钮。

![](img/5c0a8e128cf7ecb0f434fc148007b30b.png)

作者提供的图片

移动到项目下的“dbt”文件夹并执行命令 `dbt init`：

```py
cd dbt_mage/dbt 
dbt init demo -s
```

该命令将“demo”文件夹添加到 dbt 目录。

![](img/5ee41c700d091ea10f38900cab3a2166.png)

作者提供的图片

右键点击“demo”文件夹，创建一个名为“profiles.yml”的新文件。在此文件中指定你的 BigQuery 凭证。

![](img/83a273dd0238768dac9dafc9ae336dcf.png)

作者提供的图片

请参考 [此文档](https://docs.getdbt.com/docs/core/connect-data-platform/profiles.yml) 以获取指定其他数据平台提供商凭证的说明。

现在设置完成，我们准备好探索 Mage 的令人兴奋的功能了！

# 集成的基于 Web 的 IDE

Mage 提供了一个用户友好的基于 Web 的 UI，简化了 dbt 模型的创建。它的独特功能允许进行交互式代码开发，类似于在 Jupyter Notebook 中工作。每个块都作为一个独立的可执行代码文件。

![](img/20fef85ea8ad1f7869d57db2a7eef7f9.png)

图片来源：作者

要创建一个新的 dbt 模型，点击“DBT model”并提供名称和位置。

![](img/c5683bce691e9675d7aa79a3dcfe79e6.png)

图片来源：作者

编写查询并点击“Compile & preview”以预览查询结果。

预览结果后，通过点击三点图标并选择“Run model”来执行模型。

![](img/5c0343050d83c8140eab807fc8adfb9e.png)

图片来源：作者

你可以在同一个编辑器中创建额外的模型，只需再次点击“DBT model”。

![](img/faf67e4f7ab97b638ff7d55cee7d8ae3.png)

图片来源：作者

使用`{{ ref('model_name') }}`来引用另一个模型，就像在 dbt 中一样。

![](img/89da464d979c3ece49db1a6823d6aedd.png)

图片来源：作者

# 语言灵活性和 ETL 功能

Mage 使你能够将 dbt 模型与其他语言（包括 Python、R 或 SQL）结合，用于数据加载、转换和导出目的。

![](img/fd11a3304be743c99f498095736e978a.png)

图片来源：作者

例如，我们创建两个 Python 数据加载器——一个用于美国数据，一个用于印度数据。

![](img/8bbb451225e99440e0e89a38c64fb0e9.png)![](img/952d01de67a665bf1acd6386968f0b4e.png)

图片来源：作者

然后，我们将结合一个 dbt 块以连接这些数据加载器的输出。通过点击“Edit parent block”并选择数据加载器块来设置`join_tables`块的父块。

![](img/7724cfc7f26ce3b120f4d8221a4d10fb.png)

图片来源：作者

当一个 dbt 模型依赖于非 dbt 上游块时，Mage 会自动将该块的源添加到“dbt/demo/models/mage_sources.yml”文件中。

![](img/57e6df96eb3c9eb3494984603a6dcff0.png)

图片来源：作者

现在你可以在 dbt 块中利用 Python 块的输出。

![](img/99aa2fc1b05dd0a5440d5c3117f8a460.png)

图片来源：作者

然后，我们可以设置一个名为`convert_object_to_int`的变换器块，将其作为`join_tables`的下游块以处理其输出。

![](img/6a99edcf881c978e8599769bdb066244.png)

图片来源：作者

# 可视化 dbt 模型输出

虽然传统工具如 Tableau 可以用于可视化，但 Mage 提供了一个集中化的解决方案，可以在一个地方处理和分析 dbt 块的输出。

为了演示这一点，我们创建另一个块，名为`convert_week_to_datetime`，它将`Week`列转换为 datetime 类型。

![](img/1053f6a2e1af5caf7334bd206f4cb569.png)

图片来源：作者

点击“Add chart”图标，选择“Time series line chart”，并创建一个时间序列可视化。

![](img/8a89286821028fb221cda2e5debf4a3c.png)

图片来源：作者

你将看到如下的折线图：

![](img/5d0ef1c46b420dd7fcd617e4ece6d7c1.png)

作者提供的图片

# 管道调度和重试机制

Mage 使你能够调度管道运行，并包含一个重试机制，以处理失败的块而无需重新运行整个管道。

要立即执行管道，请点击左侧面板上的“触发器”图标，然后选择“立即运行管道”。

你可以通过选择新创建的管道触发器并点击“查看块运行”来监控所有块运行的实时状态。

![](img/08b2b8004fbf56fee717563e84bb8f00.png)

作者提供的图片

要调度管道，请点击“创建新触发器”，选择“调度”，并定义所需的频率。

![](img/f90954f3e23bf2f828c186d3d0e00185.png)

作者提供的图片

点击“重试未完成的块”以重试失败的块而无需重新启动整个管道。

![](img/2ad5dac76ee03303a5b9ea6c603575f0.png)

作者提供的图片

# 缺点

虽然 Mage 是 dbt 的绝佳补充，但在使用 Mage 时需要考虑一些缺点：

1.  **项目复杂性增加：** 将 Mage 和 dbt 集成可能会增加项目结构的复杂性。

1.  **更长的错误信息：** 由于 Mage 在块代码周围添加了额外的代码，错误信息通常比标准错误信息要长。

1.  **学习曲线：** 虽然 Mage 提供了直观的用户体验，但熟悉这个新工具需要一些时间和努力。

# 结论

如果你寻求提高效率并且愿意接受项目复杂性的轻微增加，Mage 是补充你的 dbt 项目的理想工具。

我喜欢写关于数据科学概念的文章，并玩弄各种数据科学工具。你可以通过以下方式保持最新：

+   订阅我的新闻通讯，内容在[数据科学简化](https://mathdatasimplified.com/)。

+   在[LinkedIn](https://www.linkedin.com/in/khuyen-tran-1401/)和[Twitter](https://twitter.com/KhuyenTran16)上与我联系。
