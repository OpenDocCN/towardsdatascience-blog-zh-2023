# 5 种适用于 R 的极佳数据管道编排工具

> 原文：[`towardsdatascience.com/5-fantastic-data-pipeline-orchestration-tools-for-r-f34ab71b1730`](https://towardsdatascience.com/5-fantastic-data-pipeline-orchestration-tools-for-r-f34ab71b1730)

## 探索适用于 R 用户的数据管道编排的优秀选项

[](https://chengzhizhao.medium.com/?source=post_page-----f34ab71b1730--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----f34ab71b1730--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f34ab71b1730--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f34ab71b1730--------------------------------) [Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----f34ab71b1730--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f34ab71b1730--------------------------------) ·阅读时间 11 分钟·2023 年 1 月 30 日

--

![](img/c2f54ada9cd590d65eda4378e24afa28.png)

图片由 [Daria Nepriakhina 🇺🇦](https://unsplash.com/ko/@epicantus?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，[Unsplash](https://unsplash.com/photos/kXDHR_bXIZo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

数据管道编排工具对于生成健康且可靠的数据驱动决策至关重要。R 是数据科学家常用的语言之一。凭借 R 的优秀包，R 编程语言非常适合数据处理、统计分析和可视化。

一个常见的模式是将数据科学家在 R 中编写的本地脚本重写为 Python 或 Scala（Spark），然后通过现代数据管道编排工具如 Apache Airflow 调度数据管道和模型构建。

然而，许多现代数据编排项目如 Apache Airflow、Prefect 和 Luigi 都是基于 Python 的。它们能与 R 无缝配合吗？你能用 R 编写来定义 DAG 吗？在本文中，让我们探索适用于 R 脚本的流行数据管道编排工具，并评估哪个适合你的使用场景。

# 成功的数据管道编排的关键组成部分

根据我的经验，数据管道编排可以分为三个主要组成部分：**DAG（依赖关系）、调度程序和插件**。

## DAG（有向无环图）

**DAG 定义了数据管道的蓝图**。它为执行路径提供了方向。同时，你可以通过查看 DAG 来追踪依赖关系。

*为什么 DAG 对任何数据管道的成功至关重要？* 因为处理数据需要有一个顺序，以从数据中提取见解。这一顺序从业务规则的角度来看不能更改。否则，数据的输出将是无用的或出错的。

通过将有向无环图中的每个节点视为一个独立的功能，DAG 提供了一个对齐方式，使当前节点必须遵循上游节点定义的规则。例如，当前节点仅在所有上游节点成功时触发；或者当前节点在一个上游节点失败时可执行。

DAG 方便地提供了数据来源的视图。它提升了可见性，同时显著简化了在数据管道中追踪错误的能力。当数据管道在早晨值班时遇到不愉快的错误时，DAG 执行的实例可以快速指出错误发生的位置。

现如今，能够将 DAG 视为蓝图并在运行时查看其实例以检查作业运行状态的功能对于任何数据编排工具都至关重要。

## 调度器

**调度器是执行数据管道的驱动程序。** 一个调度器可以像 cron 作业一样简单。更复杂的调度器涉及构建自己的调度器，比如 Airflow 中的调度器，它管理所有任务状态并积极快照。

*调度器的作用是什么？* 调度器是一个守护进程，可以被视为后台进程。它应该全天候运行，并在达到某个时间或事件时进行监控。如果调用了调度的时间或事件，执行任务并等待下一个任务。

![](img/af4e7a2e23d3bf464651a20a259de223.png)

调度周期 | 图片由作者提供

## 插件

**插件用于扩展性，被视为数据管道的潜力。** 利用现有的软件包而不是重新发明轮子是很常见的。编排工具插件的丰富性可以节省你集中于业务逻辑的时间，而不是花费数天的时间搜索和编写“*如何编写脚本将 Spark 作业提交到 EMR？*”

如果数据编排工具不断发展，它会吸引更多的供应商和额外的社区开发者来添加更多插件，以吸引更多用户。迁移到其他数据管道编排工具也很昂贵。

# 不涵盖的选项

+   [taskscheduleR](https://cran.r-project.org/web/packages/taskscheduleR/readme/README.html)：一个 Windows 专用的调度器，与 Windows 任务调度程序一起使用。如果你使用的是 Windows，这绝对是一个值得探索的选项。

+   [`github.com/kirillseva/ruigi`](https://github.com/kirillseva/ruigi) — 这是一个令人钦佩的尝试。然而，该项目似乎处于闲置状态，自 2019 年 5 月 26 日以来没有更多活动。

# 探索 R 的数据管道编排工具

我们将讨论以下 5 种不同的工具，这些工具适用于不同的使用案例。

1.  [cronR](https://github.com/bnosac/cronR)

1.  [targets](https://github.com/ropensci/targets)

1.  [Kestra](https://kestra.io/)

1.  [Apache Airflow](https://airflow.apache.org/)

1.  [Mage](https://www.mage.ai/)

# 1\. cronR

成功的数据管道协调的关键组成部分之一是调度。调度程序让你在无需人工干预的情况下运行数据管道更加安心。要调度 R 脚本，cronR 是你首先要探索的解决方案。

[](https://github.com/bnosac/cronR?source=post_page-----f34ab71b1730--------------------------------) [## GitHub - bnosac/cronR: 一个简单的 R 包，用于管理你的 cron 作业。]

### 使用 cron 调度程序调度 R 脚本/进程。这允许在 Unix/Linux 上工作的 R 用户自动化 R 进程…

github.com](https://github.com/bnosac/cronR?source=post_page-----f34ab71b1730--------------------------------)

该包增强了一组对 `crontab` 的包装，使得仅使用 R 进行采纳变得更加直接。因此，你不需要担心设置 crontab，`cronR` 提供了一个减少复杂性的接口。

```py
library(cronR)

f   <- system.file(package = "cronR", "extdata", "helloworld.R")
cmd <- cron_rscript(f)

## schedule R script daily at 7 am
cron_add(
  command = cmd,
  frequency = 'daily',
  at = '7AM',
  id = 'my_first_cronR',
  description = 'schedule R script daily at 7 am'
)

## schedule same R script every 15 mins
cron_add(cmd,
         frequency = '*/15 * * * *',
         id = 'my_second_cronR',
         description = 'schedule same R script every 15 mins')
```

**使用建议**

如果你只想调度 R 脚本，这个选项是一个快速且轻量的解决方案。它适用于临时调度、简单依赖关系或涉及较少状态管理的用例。

**限制**

由于 cronR 仅提供调度程序。你需要自己构建工作流依赖关系。如果一个 R 脚本可管理，那是可行的。然而，如果脚本的规模变得巨大并且需要中间阶段，你可能希望将其拆分。这些是 cronR 不足以处理的情况，因为它仅处理调度部分而不包括 DAG 定义和插件。

# 2\. targets

> `[targets](https://github.com/ropensci/targets)` 包是一个 [Make](https://www.gnu.org/software/make/)-类似的 R 统计和数据科学管道工具包。

[targets](https://github.com/ropensci/targets) 是一个用于数据管道的 R 编程语言工具。你可以轻松定义一个 DAG 来创建依赖图。targets 的主要目标是提供可重现的工作流。它没有自带调度程序，但与我在这里提到的其他工具连接，不应该难以调度从 targets 生成的 DAG。

```py
# functions
get_data <- function() {
  print("getting data")
}

transform_data1 <- function() {
  print("transforming data 1")
}

transform_data2 <- function() {
  print("transforming data 2")
}

loading_data <- function() {
  print("loading data")
}

# _targets.R file
library(targets)
tar_option_set(packages = c("readr", "dplyr", "ggplot2"))
list(
  tar_target(extraction, get_data()),
  tar_target(transform_data, transform_data1_1(extraction)),
  tar_target(transform_data2, transform_data2(extraction)),
  tar_target(loading_data, loading_data(transform_data, transform_data2))
)
```

管道和函数定义被分为两个 R 文件。你可以在 `_targets.R` 文件中构建依赖树，并通过 `tar_visnetwork()` 进行可视化，从而自动生成 DAG。

![](img/9b33dea61e11ce0f8c89ea0cf57ee81d.png)

targets 可视化 DAG | 作者提供的图片

**使用建议**

你需要本地支持 R 来处理数据管道。targets 包可以帮助 R 用户提高日常数据分析工作的效率。它不需要后台运行额外的守护进程来获取 DAG 进行可视化和按需运行。如果你在寻找一个可以手动运行并寻找 DAG 管理解决方案的选项，targets 对于 R 用户来说是一个很好的选择。

**限制**

如果你决定使用 targets，你将拥有一个强大的 DAG 管理工具。然而，你需要一个调度器和额外的插件来使其成为生产中的数据管道编排工具。结合像 cronR 这样的调度器可以为你提供一个纯 R 解决方案。

# 3\. Kestra

[Kestra](https://kestra.io/) 是一个通用的数据管道编排工具。它目前仅支持三种类型的脚本：*Bash, Node, 和 Python*。

尽管 R 语言未被包含在支持的脚本中，但你仍然可以通过使用带有`Rscript`命令的 bash 脚本来实现 R 脚本的编排。

```py
id: "r_script"
type: "io.kestra.core.tasks.scripts.Bash"
commands:
- 'Rscript my_awesome_R.R'
```

可以通过在 YAML 文件中使用 [Flowable Task](https://kestra.io/docs/developer-guide/flowable/) 来设置更复杂的 DAG。调度也在 YAML 文件中通过 [Schedule](https://kestra.io/docs/developer-guide/triggers/schedule.html) 完成。

**使用建议**

作为 R 用户，Kestra 允许你通过 bash 命令编排 R 代码。此外，Kestra 具有在多种语言中运行数据管道的灵活性。它给你一种现代版的 [Oozie](https://oozie.apache.org/) 的感觉。如果你对 Oozie 比较熟悉，Kestra 应该更容易上手。

**限制**

运行 R 不是原生支持的。运行时检索元数据并非简单，单纯使用`Rscript`命令可能需要额外的学习时间来寻找合适的核心或插件以更有效地开发。

# 4\. Apache Airflow

Airflow 迄今为止是最受欢迎的数据管道编排工具。然而，要编写 DAG，你需要使用 Python。你可以使用 Airflow 操作符来执行你的 R 脚本。然而，定义 DAG 时，R 不在实现范围内。

Airflow 社区曾提出创建`ROperator`的提案。该提案是利用 [rpy2](https://rpy2.github.io/)，它为**R**在 Python 进程中嵌入运行创建接口。核心思想是传递`r_command`，然后将 R 脚本复制到临时文件中并进行源代码处理。

有很多 R 用户对这个拉取请求感兴趣。

[](https://github.com/apache/airflow/pull/3115?source=post_page-----f34ab71b1730--------------------------------) [## [AIRFLOW-2193] 为使用 R 添加 ROperator · briandconnelly · Pull Request #3115 · apache/airflow

### 确保你检查了以下所有步骤。JIRA 我的 PR 涉及以下 Airflow JIRA 问题并引用了它们……

github.com](https://github.com/apache/airflow/pull/3115?source=post_page-----f34ab71b1730--------------------------------)

然而，这项功能尚未实现。浏览了这个拉取请求后，我们注意到 R 语言的支持在当时不是 Airflow CI 的一部分，其他选项可以执行 R 脚本，因此优先级较低。如果你希望坚持使用 Airflow 生态系统，还有一些选择。

+   使用 `BashOperator` 来运行 R 代码。如果你可以从终端运行你的 R 脚本，`BashOperator` 满足这个要求。使用 `BashOperator` 也使得构建 DAG 关系、将参数传递到 `bash_command` 以及添加更复杂的逻辑，如重试和邮件警报，变得简单。

```py
run_this = BashOperator(
    task_id="my_first_r_task",
    bash_command="Rscript my_awesome_R.R",
)
```

+   在 Docker 容器中运行 R（参见 [rocker](https://www.rocker-project.org/)）并使用 `DockerOperator`。这个选项类似于 `BashOperator`。使用 Docker 容器的好处是，你可以减少在与 Airflow 相同环境中执行 R 脚本的配置时间。Docker 容器每次提供一个新的 R 环境。这是一个不错且干净的解决方案。

+   *[如果你仍然想要一个专用的* `*ROperator*`*]*，你可以复制并粘贴上述拉取请求中的 `r_operator.py`，并让 Airflow 基础设施团队将其添加进去。

类似的情况也出现在 [Prefect](https://github.com/PrefectHQ/prefect)。它有一个未解决的开源拉取请求，而且该 PR 被标记为低优先级。

[](https://github.com/PrefectHQ/prefect/issues/5449?source=post_page-----f34ab71b1730--------------------------------) [## 添加 `RTask` 到任务库 · 问题 #5449 · PrefectHQ/prefect

### 你目前不能执行该操作。你在另一个标签页或窗口中登录了。你在另一个标签页或窗口中注销了…

[github.com](https://github.com/PrefectHQ/prefect/issues/5449?source=post_page-----f34ab71b1730--------------------------------)

许多现代数据管道编排是使用 Python 构建的。然而，当涉及到利用另一个流行的数据相关语言如 R 时，由于其在设计时没有与其他语言一起开始，所以优先级较低。后来，项目变得过于庞大，难以进行根本性的更改，并且需要从头开始付出巨大努力。

**使用建议**

如果你已经有了作为数据管道编排器的 Airflow 基础设施，这个选项是好的。Airflow 提供了成功的数据管道编排平台的三个关键元素。你已经建立了基础和资源，使用上述选项运行 R 是可能的。

**限制**

R 并不是 Airflow 中的第一个公民。在 Airflow 中，原生不支持运行 R。虽然有多种解决方法，但 R 仍然被视为外语。无论你选择使用`BashOperator`还是`DockerOperator`，甚至是分叉那个 PR，你仍然需要额外的支持，与数据基础设施团队沟通，以帮助你使 R 脚本在 Airflow 中可运行。

另一个限制是，使用 R 时，实时拉取 Airflow 宏（Airflow 的元数据）并不简单。你仍然可以通过查询 Airflow 的后端来使用复杂的解决方案。然而，对于没有深入了解 Airflow 的 R 用户来说，这并不友好。

# 5\. 法师

Mage 是数据管道编排领域的新玩家。我们讨论的最大收获是 **Mage 默认将 R 识别为支持语言的一部分，并允许用户定义 DAG，而不论选择何种语言（目前为 python/SQL/R）。**

这是 R 用户的一个里程碑。喜欢 R 的用户在定义一个将 R 脚本封装在受限支持工具中的 DAG 时，不必切换到 Python 语法。

Mage 允许用户使用 R 编写主要的 ETL（提取、转换和加载）块。Mage 通过在 YAML 文件中维护 DAG 依赖关系来构建 DAG。这成为了一种灵活的选择，绕过了编程语言的选择。你还可以在开发 DAG 时可视化 DAG 及其 R 代码块。

![](img/173007d39faa27f767b129ceb431a85e.png)

Mage 管道编辑模式使用 R | 作者提供的图像

以下是我用来演示如何使用 R 编写管道并在 Mage 中构建 DAG 的三个主要块。此外，你可以轻松地在 R 中访问调度程序元数据，如 `execution_date`。

```py
## Data Loader (Extraction)
## You can download dataset here https://www.kaggle.com/datasets/yanmaksi/big-startup-secsees-fail-dataset-from-crunchbase
load_data <- function() {
    df <- read.csv(file='~/Downloads/big_startup_secsees_dataset.csv')
    ## Access scheduler metadata or user defined variables
    ## This part is powerful that you can access data orchestration metadata at runtime
    df['date'] <- global_vars['execution_date']
    df
}

## --------------------------------------------------------------##

## Trasformation (Transformation)
library("pacman") ## install pacman before
p_load(dplyr) ## dplyr makes it easier to recognize the dataframe column

transform <- function(df_1, ...) {
    ## filter on USA startup
    df_1 <- filter(df_1, country_code == 'USA')
    df_1
}

## --------------------------------------------------------------##

## Data Exporter (Loading)
export_data <- function(df_1, ...) {
    # You can write to file to locally
    write.csv(df_1, "~/Downloads/usa_startup_dataset.csv")
}
```

一旦在 Mage 中开发了管道，你可以通过使用 crontab 调度管道来附加触发器。

![](img/0c0338e2a94a4208e2a79a43b4d2b92c.png)

Mage 管道调度 | 作者提供的图像

在内部，Mage 仍然使用 Python 作为核心，并将 R 脚本解析成 `tmp` 文件，然后使用该文件运行 Rscript 命令。

```py
subprocess.run(
    [
        'Rscript',
        '--vanilla',
        file_path
    ],
    check=True,
)
```

如果你想了解更多关于 Mage 作为 Apache Airflow 替代方案的内容，我写了一篇文章。

[](https://chengzhizhao.medium.com/is-apache-airflow-due-for-replacement-the-first-impression-of-mage-ai-ade8208fb2a0?source=post_page-----f34ab71b1730--------------------------------) [## Apache Airflow 是否该被替代？mage-ai 的初步印象]

### 作为数据工程师对 Apache Airflow 的替代方案介绍 mage-ai

chengzhizhao.medium.com](https://chengzhizhao.medium.com/is-apache-airflow-due-for-replacement-the-first-impression-of-mage-ai-ade8208fb2a0?source=post_page-----f34ab71b1730--------------------------------)

**使用建议**

R 成为 Mage 的主要语言。你可以编写 R 块并顺利访问调度程序元数据，而无需担心如何注入或查询后台。在 Mage 中开发也具有互动性。工程师可以在开发过程中快速迭代测试；他们可以可视化结果，而不是调试一个庞大的 DAG。

**限制**

Mage 是一个成立于 2021 年的新项目，目前仍处于早期阶段。大量文档需要改进。此外，Mage 的插件数量无法与 Airflow 相比。

# **最终想法**

这里没有涵盖许多数据管道编排选项。对于 R 用户来说，更好地与 R 语言集成的数据管道编排可以减少将初始数据分析转移到生产数据管道中的压力。我希望这里的选项能为 R 用户提供对各种数据管道编排工具的更好见解。

我希望这篇文章对你有所帮助。这篇文章是我关于工程与数据科学故事的**系列之一**，目前包括以下内容：

![赵成志](img/51b8d26809e870b4733e4e5b6d982a9f.png)

[赵成志](https://chengzhizhao.medium.com/?source=post_page-----f34ab71b1730--------------------------------)

## 数据工程与数据科学故事

[查看列表](https://chengzhizhao.medium.com/list/data-engineering-data-science-stories-ddab37f718e7?source=post_page-----f34ab71b1730--------------------------------)53 个故事！[](../Images/8b5085966553259eef85cc643e6907fa.png)![](img/9dcdca1fc00a5694849b2c6f36f038d4.png)![](img/2a6b2af56aa4d87fa1c30407e49c78f7.png)

你也可以[**订阅我的新文章**](https://chengzhizhao.medium.com/subscribe)或成为[**推荐的 Medium 会员**](https://chengzhizhao.medium.com/membership)，获取对 Medium 上所有故事的无限访问权限。

如有问题/意见，请**随时在本文下方评论**或通过[Linkedin](https://www.linkedin.com/in/chengzhizhao/)或[Twitter](https://twitter.com/ChengzhiZhao)与我直接联系。
