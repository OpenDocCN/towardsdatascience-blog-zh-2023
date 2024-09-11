# 你的数据（终于）在云端了。现在，别再那么依赖本地了

> 原文：[https://towardsdatascience.com/your-datas-finally-in-the-cloud-now-stop-acting-so-on-prem-bbb7b4f35529?source=collection_archive---------2-----------------------#2023-08-16](https://towardsdatascience.com/your-datas-finally-in-the-cloud-now-stop-acting-so-on-prem-bbb7b4f35529?source=collection_archive---------2-----------------------#2023-08-16)

## *现代数据栈允许你以不同的方式操作，而不仅仅是在更大的规模上。充分利用它。*

[](https://barrmoses.medium.com/?source=post_page-----bbb7b4f35529--------------------------------)[![Barr Moses](../Images/4c74558ee692a85196d5a55ac1920718.png)](https://barrmoses.medium.com/?source=post_page-----bbb7b4f35529--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bbb7b4f35529--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bbb7b4f35529--------------------------------) [Barr Moses](https://barrmoses.medium.com/?source=post_page-----bbb7b4f35529--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2818bac48708&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-datas-finally-in-the-cloud-now-stop-acting-so-on-prem-bbb7b4f35529&user=Barr+Moses&userId=2818bac48708&source=post_page-2818bac48708----bbb7b4f35529---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bbb7b4f35529--------------------------------) ·10分钟阅读·2023年8月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbbb7b4f35529&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-datas-finally-in-the-cloud-now-stop-acting-so-on-prem-bbb7b4f35529&user=Barr+Moses&userId=2818bac48708&source=-----bbb7b4f35529---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbbb7b4f35529&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fyour-datas-finally-in-the-cloud-now-stop-acting-so-on-prem-bbb7b4f35529&source=-----bbb7b4f35529---------------------bookmark_footer-----------)![](../Images/126d8c1cdc8ff37952b613dc6706b95a.png)

图片由 [Massimo Botturi](https://unsplash.com/@wildmax?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/zFYUsLk_50Y?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

想象一下，你在大部分职业生涯中一直用锤子和钉子建房子，而我给了你一把钉枪。但你却不是将它对准木头按下扳机，而是将其侧向放置，就像用锤子一样敲钉子。

你可能会认为这既昂贵又效果不佳，而网站的检查员则会正确地将其视为安全隐患。

好吧，那是因为你正在使用现代工具，但却带着遗留的思维和流程。虽然这个类比并不完美地概括了某些数据团队从本地到现代数据栈后的运作方式，但它很接近。

团队很快理解到超弹性计算和存储服务如何使他们能够处理以前从未见过的多样数据类型和速度，但他们并不总是理解云对其工作流程的影响。

所以也许对这些最近迁移的数据团队来说，一个更好的类比是，如果我给你1,000把钉子枪，然后看着你将它们全部横着放，以同时钉1,000个钉子。

**无论如何，重要的是要理解现代数据栈不仅仅允许你更大更快地存储和处理数据，它允许你从根本上以不同的方式处理数据，以实现新的目标并提取不同类型的价值**。

这部分是由于规模和速度的增加，但也是由于[更丰富的元数据](https://medium.com/towards-data-science/conscious-decoupling-how-far-is-too-far-for-storage-compute-and-the-modern-data-stack-ce2d9c61ccd3)和生态系统中的更无缝集成。

![](../Images/14de43be204cd9ed7b69dc6d8b36f79d.png)

图片由Shane Murray和作者提供。

在这篇文章中，我突出了我看到数据团队在云端行为变化的三种更常见方式，以及五种它们没有做到（但应该做到）的方式。让我们深入探讨一下。

# 数据团队在云端变化的三种方式

数据团队转向现代数据栈的原因有很多（不仅仅是因为首席财务官最终解放了预算）。这些用例通常是数据团队进入云端后的第一个也是最容易的行为转变。它们包括：

## 从ETL迁移到ELT以加快洞察时间

你不能随便将任何东西加载到本地数据库中——特别是如果你希望查询在周末之前返回的话。因此，这些数据团队需要仔细考虑要提取哪些数据以及如何通过在Python中硬编码的管道将其转换成最终状态。

这就像是为每个数据消费者定制特定的餐点，而不是提供自助餐，就像任何去过邮轮的人都知道的那样，当你需要满足整个组织对数据的无限需求时，自助餐才是最好的选择。

这正是AutoTrader UK技术负责人Edward Kent与[我的团队去年](https://www.montecarlodata.com/blog-scaling-data-trust-how-auto-trader-migrated-to-a-decentralized-data-platform-with-monte-carlo/)讨论数据信任和自助分析需求的情况。

“我们希望赋能 AutoTrader 及其客户，使其能够做出数据驱动的决策，并通过自助平台实现数据的民主化……在我们将受信任的本地系统迁移到云端时，那些旧系统的用户需要相信新的云技术与他们过去使用的旧系统一样可靠，”他说。

当数据团队迁移到现代数据堆栈时，他们欣然采用如 Fivetran 这样的自动化数据摄取工具或如 dbt 和 Spark 这样的转化工具，以及更复杂的[数据策展](https://www.montecarlodata.com/blog-data-curation-explained/)策略。分析自助服务开启了一个全新的领域，谁来负责数据建模并不总是很明确，但总体而言，这是一种更高效的方式来解决分析（和其他！）使用案例。

## 实时数据用于操作决策

在现代数据堆栈中，数据可以快速移动，不再仅仅用于每日指标的脉搏检查。数据团队可以利用[Delta 实时表](https://www.databricks.com/product/delta-live-tables)、[Snowpark](http://snowpark/)、[Kafka](https://kafka.apache.org/)、[Kinesis](https://aws.amazon.com/kinesis/)、微批处理等更多工具。

并非每个团队都有实时数据的使用案例，但那些有的团队通常都非常清楚。这些通常是需要操作支持的物流密集型公司，或者是将强大报告集成到产品中的科技公司（尽管后一类公司中有相当一部分是在云中诞生的）。

挑战仍然存在，这些挑战有时涉及并行架构（分析批处理和实时流）并试图达到大多数人希望的质量控制水平。但大多数数据领导者很快理解了能够更直接支持实时操作决策的价值。

## 生成式 AI 和机器学习

数据团队[对生成式 AI 浪潮有深刻的认识](https://www.montecarlodata.com/2023-state-of-data-management-survey)，许多[行业观察者怀疑](https://tomtunguz.com/cloud-earnings-q323/)这项新兴技术正在推动基础设施现代化和利用的巨大浪潮。

但在 ChatGPT 生成其第一篇文章之前，机器学习应用已经从前沿技术逐渐成为数据密集型行业的标准最佳实践，包括媒体、电商和广告。

目前，许多数据团队一有可扩展的存储和计算资源就会立即开始检查这些使用案例（尽管有些团队可能会从构建更好的基础中受益）。

如果你最近迁移到云端，但还没有询问业务如何更好地支持这些使用案例，请将其安排到日程中。本周，或者今天。你会感谢我的。

# 数据团队仍像在本地部署一样工作的 5 种方式

现在，让我们看看一些以前在本地的数据团队可能较慢利用的未实现的机会。

*附注：我想澄清的是，虽然我之前的类比有些幽默，但我并不是在嘲笑那些仍在本地操作或在云中使用以下流程的团队。变革是困难的，尤其是在面对持续的积压和不断增加的需求时，变革更为艰难。*

## 数据测试

本地的数据团队没有规模或来自中央查询日志或现代表格格式的丰富元数据，因此无法轻松运行机器学习驱动的异常检测（换句话说，[数据可观察性](https://www.montecarlodata.com/blog-what-is-data-observability/)）。

相反，他们与领域团队合作，以理解数据质量要求，并将这些要求转化为SQL规则或数据测试。例如，customer_id 应该永远不能为NULL，或者 currency_conversion 应该永远不能有负值。还有一些[本地工具](https://www.informatica.com/)旨在帮助加速和管理这一过程。

当这些数据团队迁移到云端时，他们首先想到的不是以不同的方式处理数据质量，而是以云规模执行数据测试。这是他们所熟悉的做法。

我看到过一些案例研究，读起来像恐怖故事（不，我不会提名字），数据工程团队在数千个DAG上运行数百万个任务，以监控数百个管道中的数据质量。哎呀！

当你运行五十万条数据测试时会发生什么？我告诉你。即使绝大多数测试通过，仍然会有数万条测试失败。而且这些测试明天还会失败，因为没有上下文来加快根本原因分析，甚至不知道从哪里开始分类。

你不知何故使你的团队产生了警报疲劳，却仍未达到所需的覆盖范围。更不用说大规模的数据测试既耗时又昂贵。

![](../Images/01bcccff04c3a66c8a0b2469e336ace6.png)

图片由作者提供。[来源](https://www.montecarlodata.com/blog-data-quality-survey)。

相反，数据团队应该利用能够检测、分类和帮助根本原因分析的技术，同时将数据测试（或自定义监控）保留在最重要的表格中的最清晰的阈值上。

## 数据血缘的数据建模

支持中央数据模型有很多合理的理由，你可能在一篇[精彩的 Chad Sanderson 博客](https://www.linkedin.com/posts/chad-sanderson_dataengineering-activity-7072616818003099648-n41-?utm_source=share&utm_medium=member_desktop)中读到过这些理由。

但每隔一段时间，我会遇到在云端投入大量时间和资源来维护数据模型的团队，唯一的原因是维护和理解 [数据血统](https://www.montecarlodata.com/blog-data-lineage/)。在本地部署时，这基本上是你最好的选择，除非你想阅读长篇的SQL代码，并创建一个满是记忆卡片和纱线的公告板，让你的另一半开始问你是否没事。

![](../Images/be8f0e63dbd7ed334cf4ecfaa74e864e.png)

照片由 [Jason Goodman](https://unsplash.com/@jasongoodman_youxventures?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/Oalh2MojUuk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

（“不，Lior！我没事，我只是试图理解这个WHERE子句是如何改变这个JOIN中的列的！”）

现代数据堆栈中的多个工具——包括数据目录、数据可观测性平台和数据仓库——可以利用元数据来创建自动化的数据血统。这只是一个 [选择风味](https://www.montecarlodata.com/blog-data-lineage-and-data-observability/) 的问题。

## 客户细分

在旧的世界里，对客户的视角是平面的，而我们知道它实际上应该是一个360度的全球视图。

这种有限的客户视角是由于预建数据（ETL）、实验约束和本地数据库计算更复杂查询（独特计数、不同值）所需的时间较长。

不幸的是，数据团队在云端移除了这些约束之后，并不总是会从他们的客户视角中移除盲点。这通常有几个原因，但最大的罪魁祸首仍然是老式的 [数据孤岛](https://medium.com/towards-data-science/where-data-silos-live-in-your-organization-f51ecdd90809)。

市场营销团队运营的客户数据平台仍然充满活力。该团队可以通过丰富他们对潜在客户和客户的视角来从数据仓库/湖泊中的其他领域数据中受益，但多年来建立的习惯和责任感很难打破。

因此，与其根据最高的估计终身价值来定位潜在客户，不如将目标放在每个线索的成本或每次点击的成本上。这是数据团队以直接且高度可见的方式为组织贡献价值的一个错失机会。

## 导出外部数据共享

复制和导出数据是最糟糕的。这不仅耗时、增加成本，还会导致版本控制问题，使得访问控制几乎不可能。

与其利用你的现代数据堆栈创建一个将数据以极快速度导出到典型合作伙伴的管道，更多的云端数据团队应当利用[零拷贝数据共享](https://www.eckerson.com/articles/zero-copy-approaches-to-data-sharing)。就像管理云文件的权限在很大程度上取代了电子邮件附件，零拷贝数据共享允许在不将数据从宿主环境中移走的情况下访问数据。

[Snowflake](https://www.snowflake.com/guides/what-data-sharing) 和 [Databricks](https://www.databricks.com/product/delta-sharing) 在过去两年的[年度峰会](https://www.montecarlodata.com/blog-snowflake-summit-databricks-data-ai-2023-recap-features/)上宣布并大力推广了他们的数据共享技术，更多的数据团队需要开始加以利用。

## 成本和性能优化

在许多本地系统中，数据库管理员负责监督所有可能影响整体性能的变量，并根据需要进行调节。

然而，在现代数据堆栈中，你通常会看到两种极端情况。

在一些情况下，DBA的角色仍然存在，或者外包给一个中央数据平台团队，如果管理不善，可能会造成瓶颈。然而，更常见的情况是，成本或性能优化变成了“无人区”，直到一笔特别高额的账单送到CFO的桌上。

这通常发生在数据团队没有正确的成本监控工具时，且出现了特别激进的异常事件（可能是错误的代码或爆炸性JOIN）。

此外，一些数据团队未能充分利用“按使用付费”的模式，而是选择承诺预定数量的积分（通常有折扣）……然后超出这个数额。虽然积分承诺合同本身没有问题，但如果不加以注意，这种预留时间可能会形成一些坏习惯，随着时间的推移逐渐累积。

云计算使得DevOps/DataOps可以采用更加连续、协作和集成的方法，FinOps也是如此。我看到的[最成功的团队](https://www.montecarlodata.com/blog-data-optimization-tips/)是那些将成本优化融入日常工作流，并激励与成本最相关的人员的团队。

“消费型定价的兴起使得这一点更加关键，因为新功能的发布可能导致成本指数级上升，”Tenable的Tom Milner说。“作为我的团队的负责人，我每天检查我们的Snowflake成本，并将任何费用激增作为我们待办事项的优先项。”

这会创建反馈循环、共享学习以及成千上万的小型快速修复，从而带来重大成果。

Stijn Zanders 在 Aiven 说：“我们设立了警报，当有人查询任何可能花费我们超过 $1 的内容时。这是一个相当低的阈值，但我们发现费用不需要超过这个数额。我们发现这是一个良好的反馈循环。[当这个警报出现时]，通常是有人忘记了在分区或聚集列上设置过滤器，他们可以迅速学习。”

最后，在团队之间部署收费回收模型，在云计算之前的时代几乎是不可想象的，这是一项复杂但最终值得的工作，我希望看到更多的数据团队对此进行评估。

# 成为一个“学习型”人

微软首席执行官 Satya Nadella [曾谈到](https://www.microsoft.com/en-gb/industry/blog/cross-industry/2019/10/01/introduce-learn-it-all-culture/) 他如何有意将公司的组织文化从“全知型”转变为“学习型”。这将是我对数据领导者的最佳建议，无论你是刚刚迁移还是多年来一直处于数据现代化的前沿。

我理解这可能是多么令人不知所措。新技术的出现迅猛而猛烈，供应商的推销也不容忽视。最终，这不仅仅是关于拥有行业内“最现代化”的数据堆栈，而是关于在现代工具、顶尖人才和最佳实践之间创造对齐。

为了做到这一点，始终准备学习你的同行如何应对你面临的许多挑战。参与社交媒体，阅读 Medium，关注分析师，并参加会议。我会在那里见到你！

***在云环境中，其他哪些本地数据工程活动不再有意义？如有任何意见或问题，请联系*** [***Barr***](https://www.linkedin.com/in/barrmoses/) ***，通过 LinkedIn。***
