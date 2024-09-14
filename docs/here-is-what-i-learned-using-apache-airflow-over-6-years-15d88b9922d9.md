# 这是我使用 Apache Airflow 6 年学到的东西

> 原文：[`towardsdatascience.com/here-is-what-i-learned-using-apache-airflow-over-6-years-15d88b9922d9`](https://towardsdatascience.com/here-is-what-i-learned-using-apache-airflow-over-6-years-15d88b9922d9)

## 从实验到生产无忧的 Apache Airflow 之旅

[](https://chengzhizhao.medium.com/?source=post_page-----15d88b9922d9--------------------------------)![Chengzhi Zhao](https://chengzhizhao.medium.com/?source=post_page-----15d88b9922d9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----15d88b9922d9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----15d88b9922d9--------------------------------) [程智钊](https://chengzhizhao.medium.com/?source=post_page-----15d88b9922d9--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----15d88b9922d9--------------------------------) ·8 分钟阅读·2023 年 1 月 9 日

--

![](img/2e84fe2488de04a2676a5ff7e1454684.png)

照片由[Karsten Würth](https://unsplash.com/@karsten_wuerth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，来源于[Unsplash](https://unsplash.com/photos/UbGYPMbMYP8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Apache Airflow 无疑是多年来最受欢迎的数据工程开源项目。它在与[数据工程师崛起](https://medium.com/free-code-camp/the-rise-of-the-data-engineer-91be18f1e603)的关键时刻获得了流行，其核心理念是将代码作为一等公民，而不是数据管道（即 ETL）的拖放，这标志着一个里程碑。Apache Airflow 于 2016 年 3 月成为 Apache 孵化器项目，并于 2019 年 1 月成为顶级项目。我从 2017 年起作为用户使用 Apache Airflow。在此过程中，我也为 Apache Airflow 做出了贡献。今天，我想分享我与 Airflow 的旅程以及我在 6 年中学到的东西。

# **什么是 Airflow**

> Airflow 是由社区创建的平台，用于以编程方式创建、调度和监控工作流。 — [Airflow 官方文档](https://airflow.apache.org/)

Apache Airflow 由[Maxime Beauchemin](https://medium.com/@maximebeauchemin)开发，他曾在 Airbnb 和 Facebook 工作。他在 Airbnb 开发了 Airflow（*可以从项目名称中看出*）。然而，激发他的核心想法是 Facebook 使用的内部工具。

Apache Airflow 的主要用户是数据工程师或需要调度工作流的工程师，主要是 ETL（提取、转换、加载）作业。这些 ETL 作业通常按日或按小时运行。作业本身执行数据操作，以从未结构化的原始数据中获取见解。

# 为什么 Airflow 如此受欢迎？

自 2017 年以来，我一直在使用 Apache Airflow，在它成为任何数据工程师必备技能之前。我仍然记得早期我们“黑客”式地将大量努力投入到 Airflow 调度程序系统中，以使其稳定运行的经历。如果你想了解更多，我几年前写了一篇文章。

[](https://medium.com/making-meetup/data-pipeline-infrastructure-at-meetup-with-fewer-nightmares-running-apache-airflow-on-kubernetes-54cb8cdc69c3?source=post_page-----15d88b9922d9--------------------------------) [## Meetup 的数据管道基础设施，减少噩梦：在 Kubernetes 上运行 Apache Airflow

### Meetup 数据平台基础设施——一个起点

medium.com](https://medium.com/making-meetup/data-pipeline-infrastructure-at-meetup-with-fewer-nightmares-running-apache-airflow-on-kubernetes-54cb8cdc69c3?source=post_page-----15d88b9922d9--------------------------------)

从一开始，Airflow 就以一个孵化项目的身份闪耀。它在不断发展和稳定。以下是我记录的几个让 Airflow 一开始就如此迷人的原因：

## 首先编写代码

编码（尤其是 Python）已成为数据工程师的新常态。然而，10 年前编写数据管道主要是通过像 SSIS 或 Informatica 这样的 UI 工具拖放完成的。拖放一开始很简单。然而，它很快会遇到开发周期难以扩展的情况，UI 有时也会变得更加棘手，并且拖放框时可能会出现问题。

编码是一种很好的抽象方式。我们可以用 Python 代码编写包含所有逻辑的有向无环图（DAG），分享并部署它。这很好地达到了目的。它迫使一些仅了解 SQL 的人学习 Python，但这是值得的。

## 良好的可视化

无论我们编写多少代码，作为一个协调者，它需要某种方式来可视化过程。最终，良好的可视化看起来既吸引人又解决了多个问题。它可以有效地捕捉故障、理解依赖关系和监控作业状态。

作为最终用户，可视化并不像仪表板那样需要打磨。一旦编码完成，Airflow 会处理剩余部分并为你提供可视化。你的 DAG 可以以七种不同的视图进行可视化，每种视图都有其重点。我每天使用的最常见视图是 [Grid view](https://airflow.apache.org/docs/apache-airflow/stable/ui.html#grid-view)（曾经是树形视图）和 [Graph view](https://airflow.apache.org/docs/apache-airflow/stable/ui.html#graph-view)。

在 UI 上，你可以执行诸如重启作业、重置 DAG 中某些任务的状态、检查失败日志或检查长期运行的作业等任务。虽然 UI 并没有提供 100% 的功能，但它覆盖了日常使用的场景。

## 扩展性

为了让更多人将其用作 ETL 编排工具，该项目必须具有可扩展性，以便其他项目能够轻松集成。集成的丰富性为 Airflow 成为顶级 Apache 项目之一奠定了基础。此外，Airflow 允许用户编写自己的 `PythonOperator`，这进一步鼓励开发者通过代码构建自己的逻辑，而不是等待插件的新升级来满足他们的 ETL 需求。

出色的可扩展性还促进了更多创新项目和插件在 Airflow 生态系统中入驻。使用插件的核心思想是避免每个人都重新发明轮子来编写自己的逻辑。它还帮助服务或产品与最终用户无缝集成。

完整的 Airflow 插件使最终用户迁移到其他工具变得更难。例如，EDI 835：电子汇款通知是一种在医疗保健行业提供索赔支付信息的特殊格式。它需要特殊的解析逻辑才能正确读取。这些特定领域的用例使 Airflow 形成了一个很好的“护城河”，从而具备竞争力。

## 社区

最终，Airflow 是 Apache 基金会的一部分。我们都知道成为 Apache 顶级项目意味着什么。名声本身就能吸引更多的人尝试 Airflow。

社区正在健康增长。在 StackOverflow 上，你可以找到超过 9k 个标记为 [Airflow] 的问题。Airflow 每年都会举办 [AirflowSummit](https://airflowsummit.org/)。支持 Airflow 的公司 [astronomer.io](https://docs.astronomer.io/learn?utm_term=airflow&utm_campaign=ch.sem_br.nonbrand_tp.prs_tgt.airflow-general_mt.xct_rgn.namer_lng.eng_dv.all_con.airflow-general&utm_source=google&utm_medium=sem&gclid=CjwKCAiAwc-dBhA7EiwAxPRylOkLoT8HZFAl1bT--Q42KXOwFm2d4a9fv8wnLEAAgAePgiOnkJVkkRoCd2EQAvD_BwE) 也提供了出色的文档和认证。自从我在 2017 年加入以来，其 Slack 群组一直很活跃，现在已有 25k+ 人加入。

# 我使用 Airflow 的旅程

当我加入 [Meetup](https://www.meetup.com) 时，Airflow 是由一位首席工程师在 2017 年 4 月作为概念验证带来的实验工具。

当时引入 Airflow 的目的是解决我们使用自定义 Scala 基于 ETL 时遇到的多个问题。我仍然欣赏那些建立复杂 DAG 逻辑依赖关系来使 ETL 首先正常工作的人员。但我们都知道 ETL 有时会失败。这就是自定义 Scala 基于 ETL 的头疼之处。不容易调试和重新启动作业，并且你需要非常小心以确保作业是幂等的。

最终，我们找到了一种运行 Airflow 1.7 的方法，并部署了新的 Python 代码用于 POC。结果非常好。自从引入了 Airflow 1.9 和 Kubernetes 执行器，Airflow 已经处于一个更加成熟的阶段。我还为 Airflow 项目做出了贡献，并获得了对内部核心代码的深入了解。在生产环境中，我们遇到的问题比早期要少得多，我们可以更多地关注于提高可扩展性和增加更多用例。

不过，还有一些事情我希望我能早些知道，以避免一些耗时的调查。

# 学到的经验

## **一开始没有利用宏**

你没看错。我在 Airflow 的早期阶段不知道宏的概念。因此，像 `{{ ds }}` 这样的 Jinja 模板我一开始并没有利用。在新的一天中，为了触发 ETL，它只是使用了 Python 函数 `date()-1`。

最大的问题是回填，我使用了 Airflow 的参数进行处理，但用户必须提供这些信息，并且只能逐日回填。整个过程变成了一个不容小觑的过程。在仔细阅读 Airflow 文档后，发现宏非常适合获取作业的元数据。我们将回填过程简化为清除现有的 dag，而无需额外的参数修改。

## **ETL 在 EST**

日期在任何数据系统中都是关键的。我们的数据系统按 EST 分区。是 EST!!! 如果你和我处于同样的情况，你会理解在日光节约时间更改期间我经历了多么痛苦。一切都崩溃了！

我们有多个 DAG 依赖于 [external_task_sensor](https://airflow.apache.org/docs/apache-airflow/1.10.3/_api/airflow/sensors/external_task_sensor/index.html#module-airflow.sensors.external_task_sensor)，它依赖于 **execution_delta —** 与之前执行的时间差来查看。想象一下，你有一些任务完成了，然后突然待处理的任务 +1/-1 小时变得混乱不堪。我必须构建一个解决方案来避免这种情况。这不是一个有趣的维护工作。如果你开始构建任何数据系统，我建议尽可能使用 UTC 时间，以简化工作和保持内心的平静。

## 在调度 DAG 时要温和。

**Airflow 不是数据流处理解决方案。** 这在 Airflow 官方文档的 [Beyond The Horizon](https://airflow.apache.org/docs/apache-airflow/1.10.1/#beyond-the-horizon) 部分开头就提到过。

一个原因是 Airflow 不会在任务之间传递数据。遇到这种情况时，你需要寻找外部存储。Airflow 的架构并没有以流式设计开始。如果你有一个需要每分钟触发的作业，Airflow 可以完成这个任务，但它的扩展性不好。对于数据流处理的用例，使用 Flink/Spark 进行数据流处理，不要把这个负担放到 Airflow 上。

另一个不常提及的原因是，Airflow 调度器在更新所有 DAG 状态时非常繁重。当 Airflow 调度器触及实时时钟时，它会扫描下一个潜在的 DAG 以触发其执行日期。即使 Airflow 调度器宕机，状态也必须被实际化以跟踪状态。大规模的状态更新在 OLTP 数据库中进行，Airflow 仍建议使用 [**MySQL** 或 **Postgres**](https://airflow.apache.org/docs/apache-airflow/1.10.1/howto/initialize-database.html?highlight=database#initializing-a-database-backend)**。** 在这种情况下，[scheduler_heartbeat_sec](https://airflow.apache.org/docs/apache-airflow/stable/configurations-ref.html#scheduler-heartbeat-sec) 需要正确配置，默认值为 5 秒。这个值可以作为调节的关键。

当我们面临数百个 DAG 和数千个任务时，作业逐渐被安排和运行。将此值增加到 60 秒以上会使 Airflow 调度器冷却。为了更好地了解 Airflow 调度器，我写了一篇文章 Airflow Schedule Interval 101 帮助你深入学习。

[](/airflow-schedule-interval-101-bbdda31cc463?source=post_page-----15d88b9922d9--------------------------------) ## Airflow Schedule Interval 101

### 气流调度间隔可能是一个具有挑战性的概念，即使对于正在使用 Airflow 的开发人员也是如此……

towardsdatascience.com

# **最终想法**

Apache Airflow 是一个了不起的开源项目，专为数据工程师设计。回顾过去，许多概念是主导实施的基础，这些实施为其今天的流行和成功铺平了道路。许多公司将 Airflow 作为数据工程师招聘的必备技能。对我来说，早期参与 Apache Airflow 并见证项目的成长是一种幸运。一些新项目已开始挑战 Apache Airflow，例如 [mage-ai](https://github.com/mage-ai/mage-ai) 和 [Perfect](https://docs-v1.prefect.io/)。我将在未来的帖子中介绍它们。

Airflow 的创建者，[Maxime](https://medium.com/@maximebeauchemin)，也是 [Apache Superset](https://superset.apache.org/) 的创建者，是这两个项目中最令人钦佩的数据工程师之一。以下是他在 2015 年关于 Airflow 最佳实践的演讲。这仍然是今天使用 Airflow 和了解 Airflow 设计理念的一个很好的参考。

与 Airflow 的最佳实践 - 一个开源的工作流和调度平台 | 作者 [Maxime Beauchemin](https://medium.com/@maximebeauchemin)

我希望这个故事对你有帮助。本文是**我工程与数据科学故事系列的一部分**，目前包括以下内容：

![程智赵](img/51b8d26809e870b4733e4e5b6d982a9f.png)

[赵成志](https://chengzhizhao.medium.com/?source=post_page-----15d88b9922d9--------------------------------)

## 数据工程与数据科学故事

[查看列表](https://chengzhizhao.medium.com/list/data-engineering-data-science-stories-ddab37f718e7?source=post_page-----15d88b9922d9--------------------------------) 53 个故事![](img/8b5085966553259eef85cc643e6907fa.png)![](img/9dcdca1fc00a5694849b2c6f36f038d4.png)![](img/2a6b2af56aa4d87fa1c30407e49c78f7.png)

你还可以 [**订阅我的新文章**](https://chengzhizhao.medium.com/subscribe) 或成为 [**推荐的 Medium 会员**](https://chengzhizhao.medium.com/membership)，获取对 Medium 上所有故事的无限访问权限。

如果有任何问题/评论，**请随时在此故事的评论区留言**，或通过 [Linkedin](https://www.linkedin.com/in/chengzhizhao/) 或 [Twitter](https://twitter.com/ChengzhiZhao) **直接联系我**。
