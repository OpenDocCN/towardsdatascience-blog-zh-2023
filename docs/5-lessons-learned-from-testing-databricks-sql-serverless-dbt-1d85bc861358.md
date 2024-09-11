# 5个从测试Databricks SQL Serverless + DBT中获得的经验教训

> 原文：[https://towardsdatascience.com/5-lessons-learned-from-testing-databricks-sql-serverless-dbt-1d85bc861358?source=collection_archive---------3-----------------------#2023-10-17](https://towardsdatascience.com/5-lessons-learned-from-testing-databricks-sql-serverless-dbt-1d85bc861358?source=collection_archive---------3-----------------------#2023-10-17)

## 我们进行了一项$12K的实验，以测试Serverless仓库和dbt并发线程的成本和性能，并获得了意外的结果。

[](https://medium.com/@jeff.b.chou?source=post_page-----1d85bc861358--------------------------------)[![Jeff Chou](../Images/4b5b7a7f880209faf1e81806a0f9dfba.png)](https://medium.com/@jeff.b.chou?source=post_page-----1d85bc861358--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d85bc861358--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1d85bc861358--------------------------------) [Jeff Chou](https://medium.com/@jeff.b.chou?source=post_page-----1d85bc861358--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F124878bdd082&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-lessons-learned-from-testing-databricks-sql-serverless-dbt-1d85bc861358&user=Jeff+Chou&userId=124878bdd082&source=post_page-124878bdd082----1d85bc861358---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d85bc861358--------------------------------) · 9分钟阅读 · 2023年10月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d85bc861358&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-lessons-learned-from-testing-databricks-sql-serverless-dbt-1d85bc861358&user=Jeff+Chou&userId=124878bdd082&source=-----1d85bc861358---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d85bc861358&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-lessons-learned-from-testing-databricks-sql-serverless-dbt-1d85bc861358&source=-----1d85bc861358---------------------bookmark_footer-----------)

*作者：Jeff Chou，* [*Stewart Bryson*](https://medium.com/@stewartbryson)

![](../Images/8b40b0fb1e2e43d5fab293779781e128.png)

图片来自[Los Muertos Crew](https://www.pexels.com/@cristian-rojas/)

Databricks的SQL仓库产品对于希望简化生产SQL查询和仓库的公司具有很大的吸引力。然而，随着使用的扩展，这些系统的成本和性能变得至关重要。

在本文中，我们通过利用行业标准的TPC-DI基准测试，对他们的Serverless SQL数据仓库产品的成本和性能进行了技术深入分析。我们希望数据工程师和数据平台管理者能够利用这里呈现的结果，在选择数据基础设施时做出更好的决策。

# Databricks的SQL数据仓库提供了什么？

在我们深入探讨特定产品之前，让我们退后一步，看看今天提供的不同选项。Databricks目前提供[3种不同的仓库选项](https://www.databricks.com/product/pricing/databricks-sql)。

+   **SQL Classic** —— 最基本的仓库，运行在客户的云环境内。

+   **SQL Pro** —— 提升的性能，适合探索性数据科学，运行在客户的云环境内。

+   **SQL Serverless** —— 最佳性能，计算完全由Databricks管理。

从成本的角度来看，经典版和专业版都在用户的云环境内运行。这意味着您将为您的Databricks使用获得两个账单 —— 一个是纯粹的Databricks成本（DBU），另一个来自您的云提供商（例如AWS EC2账单）。

为了真正了解成本比较，让我们看一个在Small数据仓库上运行的示例成本细分的示例费用分解。

![](../Images/f6c8a672940d052d1ba0fa8858f22175.png)

作业计算成本比较，以及各种SQL Serverless选项。所显示的价格基于按需列表价格。[Spot prices will vary](https://aws.amazon.com/ec2/spot/pricing/)，并且是根据本出版物时的价格选择的。作者提供的图像。

在上表中，我们也看到了按需与spot成本的成本比较。您可以从表中看出，Serverless选项没有云组件，因为所有管理工作都由Databricks处理。

Serverless相比于专业版来说可能更具成本效益，如果您使用所有按需实例的话。但如果有廉价的spot节点可用，那么专业版可能会更便宜。总体来说，我认为Serverless的定价相当合理，因为它还包括云成本，尽管它仍然是一个“高端”价格。

我们还包括了等效的作业计算集群，这是全面管理的选择中最便宜的选项。如果成本是您关注的问题，您也可以在作业计算中运行SQL查询！

# Serverless的优缺点

Databricks的Serverless选项是一个完全管理的计算平台。这基本上与Snowflake的运行方式相同，其中所有计算细节对用户隐藏。在高层次上，这种方法有其利弊：

**优点：**

+   您不必考虑实例或配置。

+   启动时间比从头开始启动集群要少得多（根据我们的观察为5-10秒）。

**缺点：**

+   企业可能会对所有计算都在Databricks内部运行存在安全问题。

+   企业可能无法利用其云合同中对特定实例的特殊折扣

+   无法优化集群，因此你不知道 Databricks 选择的实例和配置是否实际上适合你的作业

+   计算是一个黑箱——用户不知道发生了什么或 Databricks 在幕后实施了哪些更改，这可能导致稳定性问题。

由于无服务器的固有黑箱特性，我们对人们仍然可以调整的各种参数及其对性能的影响感到好奇。因此，让我们深入探讨一下我们探索的内容：

# 实验设置

我们尝试采用“实用”方法来进行这项研究，并模拟当一家真实公司希望运行 SQL 仓库时可能采取的措施。由于[DBT](https://www.getdbt.com/)在现代数据堆栈中非常流行，我们决定检查两个参数进行扫描和评估：

+   **仓库大小** — [‘2X-Small’, ‘X-Small’, ‘Small’, ‘Medium’, ‘Large’, ‘X-Large’, ‘2X-Large’, ‘3X-Large’, ‘4X-Large’]

+   [**DBT 线程**](https://docs.getdbt.com/docs/running-a-dbt-project/using-threads) — [‘4’, ‘8’, ‘16’, ‘24’, ‘32’, ‘40’, ‘48’]

我们选择这两个参数的原因是它们都是任何工作负载的“通用”调优参数，并且它们都影响作业的计算方面。[DBT 线程](https://docs.getdbt.com/docs/running-a-dbt-project/using-threads)特别有效地调整作业在 DAG 中运行时的并行性。

我们选择的工作负载是流行的[TPC-DI](https://www.tpc.org/tpcdi/default5.asp)基准测试，规模因子为 1000。这个工作负载特别有趣，因为它实际上是一个完整的管道，模拟了更真实的数据工作负载。例如，下面是我们的 DBT DAG 的屏幕截图，你可以看到它相当复杂，改变 DBT 线程的数量可能会对其产生影响。

![](../Images/f1e1ab47791286c4933f311b55cee5aa.png)

来自我们 TPC-DI 基准测试的 DBT DAG，作者图片

> 顺便提一下，Databricks 有一个[出色的开源库](https://github.com/shannon-barrow/databricks-tpc-di)，可以帮助快速在 Databricks 中设置 TPC-DI 基准测试。（我们没有使用这个库，因为我们是使用 DBT 进行的）

要深入了解我们如何运行实验，我们使用了[Databricks Workflows](https://docs.databricks.com/en/workflows/index.html)并将 dbt 作为 dbt CLI 的“运行器”，所有作业都并行执行；由于 Databricks 方面的环境条件未知，不应存在变异。

每个作业启动一个新的 SQL 仓库，然后在同一 Unity Catalog 中的独特模式中运行并随后拆除。我们使用了[Elementary dbt 包](https://docs.elementary-data.com/quickstart)来收集执行结果，并在每次运行结束时运行 Python 笔记本，将这些指标收集到集中式模式中。

成本通过 Databricks 系统表提取，特别是那些用于计费使用的表。

> 自己尝试这个实验并克隆[Github 仓库](https://github.com/synccomputingcode/databricks-tpcdi)

# 结果

下面是成本和运行时间与仓库大小的图表。我们可以看到，运行时间在达到中等规模的仓库时停止扩展。比中等大一点的仓库对运行时间几乎没有影响（或许更糟）。这是一个典型的扩展趋势，显示了扩展集群规模不是无限的，它们总有一个点，增加更多计算资源的回报会递减。

> 对于计算机科学爱好者来说，这只是基本的计算机科学原理——[阿姆达尔定律](https://en.wikipedia.org/wiki/Amdahl%27s_law)

一个不寻常的观察是，中等仓库的表现优于接下来的三个尺寸（大到2xlarge）。我们重复了这个特定的数据点几次，并得到了一致的结果，所以这不是一个奇怪的偶然现象。由于无服务器的黑箱特性，我们不幸地不知道背后的情况，也无法给出解释。

![](../Images/21006a1473f2231a3634087b518fcb73.png)

各仓库大小的运行时间（分钟）。图片由作者提供

由于扩展在中等规模处停止，我们可以在下面的成本图中看到，成本在中等仓库大小后开始急剧上升，因为基本上你投入了更昂贵的机器，而运行时间保持不变。因此，你是在为额外的计算能力支付费用，但没有获得任何好处。

![](../Images/1128d7c301950a8939daf3a79ec097e5.png)

各仓库大小的成本（美元）。图片由作者提供

下面的图表显示了我们改变线程数和仓库大小时运行时间的相对变化。对于横轴零线以上的值，运行时间增加（这是不好的）。

![](../Images/585b9cb1320cf2ec770a9e4716c5f979.png)

线程增加时运行时间的百分比变化。图片由作者提供

这里的数据有些噪声，但根据仓库的大小有一些有趣的见解：

+   **2x-small** — 增加线程数通常会使任务运行时间更长。

+   **X-small to large** — 增加线程数通常能使任务运行速度提高约10%，尽管增益相对平稳，所以继续增加线程数没有价值。

+   **2x-large** — 实际上存在一个最佳线程数，即24，从清晰的抛物线可以看出。

+   **3x-large** — 在线程数为8时运行时间出现了非常不寻常的峰值，为什么？不清楚。

为了将所有内容整合到一个综合图中，我们可以看到下面的图表，绘制了成本与总任务时长的关系。不同的颜色代表不同的仓库大小，气泡的大小表示 DBT 线程的数量。

![](../Images/b8331017e39df72ff9b380a9c0dc67b5.png)

成本与任务时长的关系。气泡的大小表示线程数量。图片由作者提供

在上图中，我们看到较大的仓库通常会导致较短的持续时间，但成本更高。然而，我们确实发现了一些不寻常的点：

+   **中型是最佳的**——从纯成本和运行时间的角度来看，中型是最好的仓库选择

+   **DBT线程的影响**——对于较小的仓库，线程数量的变化似乎使持续时间变化了大约+/- 10%，但成本变化不大。对于较大的仓库，线程数量显著影响了成本和运行时间。

# 结论

总结一下，我们关于Databricks SQL无服务器 + DBT产品的前五个教训是：

1.  **经验法则是错误的**——我们不能仅仅依赖关于仓库大小或DBT线程数量的“经验法则”。虽然确实存在一些预期的趋势，但它们并不一致或可预测，完全依赖于你的工作负载和数据。

1.  **巨大差异——**对于完全相同的工作负载，成本范围从$5到$45，运行时间从2分钟到90分钟，所有这些都由于线程数量和仓库大小的不同组合。

1.  **无服务器扩展有极限——**无服务器仓库不会无限扩展，最终较大的仓库将无法提供任何加速，只会导致成本增加而没有任何好处。

1.  **中型很棒？**——我们发现**中型**无服务器SQL仓库在TPC-DI基准测试中在成本和作业持续时间方面超越了许多较大的仓库。我们不知道原因。

1.  **作业集群可能是最便宜的**——如果成本是一个问题，切换到仅使用标准作业计算和笔记本可能会便宜很多

这里报告的结果揭示了“无服务器”黑箱系统的性能可能会出现一些不寻常的异常。由于这一切都在Databrick的墙后，我们不知道发生了什么。也许所有这些都运行在巨大的Spark on Kubernetes集群上，也许他们在某些实例上与亚马逊有特殊的交易？无论如何，不可预测的性质使得控制成本和性能变得棘手。

因为每个工作负载在很多维度上都是独特的，我们不能依赖“经验法则”，或仅适用于当前状态的高成本实验。无服务器系统的混乱性质确实引发了这样一个问题：这些系统是否需要一个[闭环控制系统](https://medium.com/towards-data-science/why-your-data-pipelines-need-closed-loop-feedback-control-76e28e3565f)来加以控制？

作为一种反思——无服务器的商业模式确实引人注目。假设Databricks是一个理性的企业，并且不希望降低他们的收入，同时他们也希望降低成本，那么必须提出一个问题：“Databricks是否有动力去改善底层的计算？”

问题是这样的——如果他们让无服务器速度提高 2 倍，那么他们的无服务器收入会突然下降 50%——这对 Databricks 来说是非常糟糕的一天。如果他们能使其速度提高 2 倍，然后将 DBU 成本提高 2 倍以抵消速度提升，那么他们的收入将保持不变（实际上，这就是他们为 [Photon](https://synccomputing.com/databricks-photon-and-graviton-instances-worth-it/) 所做的）。

因此，Databricks 确实有动力在保持客户运行时间大致不变的情况下降低内部成本。虽然这对 Databricks 来说是好事，但很难将任何无服务器加速技术传递给用户，从而减少成本。

想了解如何改善你的 Databricks 管道？请联系 [Jeff Chou](https://www.linkedin.com/in/jeffchoumit/) 和 [Sync 团队](https://synccomputing.com/) 的其他成员。

# 资源

+   [自己尝试这个实验，并在此处克隆 Github 仓库](https://github.com/synccomputingcode/databricks-tpcdi)

# 相关内容

+   [为什么你的数据管道需要闭环反馈控制](https://medium.com/towards-data-science/why-your-data-pipelines-need-closed-loop-feedback-control-76e28e3565f)

+   [Databricks 集群是否值得使用 Photon 和 Graviton 实例？](https://synccomputing.com/databricks-photon-and-graviton-instances-worth-it/)

+   [Databricks 的自动扩展是否具有成本效益？](https://synccomputing.com/is-databrickss-autoscaling-cost-efficient/)

+   [介绍 Gradient — Databricks 优化变得简单](https://synccomputing.com/introducing-gradient-for-databricks/)
