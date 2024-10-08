# 通过示例了解精益数据管道

> 原文：[`towardsdatascience.com/a-lean-data-pipeline-by-example-e08bfce58133`](https://towardsdatascience.com/a-lean-data-pipeline-by-example-e08bfce58133)

## 强大的云数据平台，如 Snowflake 和 Databricks，改变了我们对标准形式和数据仓库的思考方式

[](https://timburnsowlmtn.medium.com/?source=post_page-----e08bfce58133--------------------------------)![Tim Burns](https://timburnsowlmtn.medium.com/?source=post_page-----e08bfce58133--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e08bfce58133--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e08bfce58133--------------------------------) [Tim Burns](https://timburnsowlmtn.medium.com/?source=post_page-----e08bfce58133--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e08bfce58133--------------------------------) ·7 分钟阅读·2023 年 4 月 18 日

--

![](img/c75ed1033a8be71c7cc1007e08423f67.png)

图片由[David Di Veroli](https://unsplash.com/@daviddiveroli?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/-m1duEoiJng?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上

# 介绍

在最小化云数据管道成本时，首先想到的是寻找最便宜的 ETL 解决方案。例如，Databricks 是否比 Snowflake 便宜？我应该使用云服务还是在内部集群上本地运行 Spark？

更好的成本最小化方法是利用精益原则。精益创业原则之一是通过小步骤进行验证[[1](https://theleanstartup.com/)]。对于云数据管道来说，关注价值尤为重要，因为每一次规范化和维度都需要花费金钱。此外，50 年的数据管道经验提供了大量我们*可以*做的转换，但只有对客户的分析价值才能定义我们*应该*做的事情。

在这篇文章中，我描述了如何通过专注于发展业务价值来进行成本最小化，即仅在需要回答特定业务问题时才转换数据。这样，你可以根据独特的业务案例而不是成本最小化的练习来选择大数据平台。只要你了解在最小化数据转换中的权衡，如果你的业务用例不需要，就最好避免进行转换。

# 一个（过于）简单的问题

因为我想说明一个想法，而不是建立实际的数据管道，所以我将以一个玩具问题来演示我们如何实现一个多阶段管道，然后假设一个转换管道是不必要的。

玩具业务用例是，我想使用在另类电台中播放的顶级艺术家来构建一个个人音乐播放列表。因此，我现在有一个商业问题需要提问，这将指导任何后端开发活动并限制其范围。

> 自 2020 年 3 月以来，另类电台的前 10 名艺术家有哪些？

ChatGPT 对这个问题的回应是：“对不起，作为一个语言模型，……” 然后将我指向 Billboard Top 100 的另类电台榜单，这同样没有帮助。我想知道电台上播放的内容，并使用准确的数据为我提供个人播放列表的新想法。

# 准确的数据，真实的模型，真实的答案

公共 API 端点用于下载播放列表数据是丰富的。开始于

+   [`rapidapi.com/blog/top-free-music-data-apis`](https://rapidapi.com/blog/top-free-music-data-apis/)

作为数据工程师，我们确定数据源，并专注于将播放列表数据干净地下载到我们喜欢的云提供商的数据湖中。这个过程中的每一步都必须保持数据的清洁。也就是说，每一步都应确保数据完整且无重复。像 dbt 这样的现代工具具有内置功能，可以使这一过程变得简单[[8](https://docs.getdbt.com/docs/introduction)]。

播放列表通常具有基于播出日期的模式以及“艺术家、专辑、歌曲和节目”等属性。节目是另一个基于时间的实体，跟踪每位 DJ 在电台的表现。API 生产者可能会反映他们的数据库，最佳实践是将数据按原样同步到你的数据湖中。关键是我们从 API 接收到的数据足够用于分析。

一个[简单的 Python 代码](https://github.com/timowlmtn/bigdataplatforms/blob/master/src/kexp/layers/raw_data_api/ApiReader.py)可以将原始数据从网络同步到数据湖。因此，回答问题的第一步是展示单次播放的数据是怎样的。

![](img/0be65f0313f49fff152f8168244c2370.png)

JSON 输出示例快照（来源 [KEXP API](https://api.kexp.org/v2/)）

关键策略是同步来自 API 的所有数据，并确保我们能够定期同步数据。

# 让你的数据转换选择具有目的性

一旦数据同步到数据湖中，它将会有类似于源头的数据输出。数据湖会以原始形式捕获所有数据，以便在需要时随时使用。数据湖需要一个严格的结构和治理，以保持数据的清晰界限和安全性[[9](https://www.mckinsey.com/capabilities/mckinsey-digital/our-insights/a-smarter-way-to-jump-into-data-lakes#/)]。

当数据在数据湖中时，你准备好建立一个数据仓库（或者如果你愿意，可以是数据湖仓）。数据仓库有丰富的理论，数据质量至关重要。如果你回答了问题，但问题本身是错的，那比不回答问题更糟。因此，完全理解你*能*做什么与知道你*应该*做什么一样重要。

我喜欢将对历史数据仓库的全面理解视为我数据管道中的“未来防护知识”。我可能不需要每一个转换，但如果当前数据无法解决新的客户问题，我需要有一个预想的转换。

## 规范化和清理

标准范式 (1NF、2NF 和 3NF) 是最常见的数据转换的基础 [[2](https://dl.acm.org/doi/10.1145/362384.362685)]。

**第一范式 (1NF)** 规定每一列应只有单一值，因此列中不应有数组或用逗号分隔的列表。保持所有列值为原子的原因很充分，因为在同一查询中进行聚合和展开会导致笛卡尔连接。然而，现代的数据仓库/湖泊架构支持复杂类型，如列表和映射。复杂结构对于维护数据血缘关系和在管道下游提供灵活性非常有益，并且受到像 Snowflake 和 Databricks 这样的商业应用的良好支持。

**第二范式 (2NF)** 规定将 1NF 的数据根据一组属性去重。在大数据世界中，我们不能依赖系统强制唯一性，因为分布式数据处理的原因。数据在多台机器上的列中处理，我们希望避免全局操作，因为这些操作在时间和金钱上都是昂贵的。在现代大数据世界中，我们按处理时间收费，因此管理效率至关重要。

**第三范式 (3NF)** 规定将 2NF 的数据移入一个单独的表中，每个表都有一组特定于其领域的数据，并且一个外键将特定表中的数据与原始数据相关联。这有助于性能，因为 3NF 中的数据只需要在一个地方查询，外键可以用于生成报告。

## Kimball 数据仓库模型

Kimball 数据仓库（维度模型）是《Kimball 数据仓库工具包》中描述的星型模式 [[3](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/)]。它扩展了 3NF 的思想，并以度量或衡量指标作为数据的核心分析组件，以维度作为这些度量相关实体的属性集合。它通常被认为是数据分析的黄金标准。

Kimball 数据仓库是为关系型数据库设计的，因此实施 Kimball 模型的所有方面可能是可选的。例如，在分布式 Spark 环境中，顺序替代键可能会成为需要避免的障碍 [4]。如果你可以在没有替代键的情况下回答所有客户的问题，那么你就不需要它们。

# 使用分析 GUI 验证小增量

“建造它，他们就会来”并不是数据产品的一个好模型。大多数数据项目的潜在来源和分析用例的数量庞大，很容易陷入专注于后端而排除客户的陷阱。

回到成本控制：每次转换和每次管道执行都会花费金钱。高效的数据管道尽可能少做。如果我们能够回答业务问题：停止。展示给客户。获取他们的反馈。回答下一组问题。

一个好的分析图形用户界面（GUI）非常有价值，因为它让公众可以访问数据。此外，它让客户可以询问有关数据的问题。客户想要的解决方案将会是关于引入新数据的。

我使用了 Tableau，并为简单应用发布了一个具体答案。

![](https://public.tableau.com/views/KexpTopArtists/Artist?:language=en-US&:display_count=n&:origin=viz_share_link)

自 2020 年 3 月以来，替代电台上的十位艺术家是谁？（来源 [作者的公开表格](https://public.tableau.com/app/profile/timothy.burns)）

在这个例子中，我不需要进行任何转换来实现预期的用例。当你能够回答业务问题时，停止数据转换，然后专注于下一个业务问题是合适的。

此外，通过像 Tableau 这样的图形用户界面（GUI）为客户提供对数据的完全访问权限，然后他们可以自己回答以下一些问题。关键思想是，你发布数据，让客户可以*自行探索并提供反馈*。

当你不能用现有数据回答问题时，获取更多数据——迭代——重复。

## 基于分析的数据产品的增量好处

分析驱动的数据产品的最强有力的论据是，用户在每次迭代中都能感受到价值。较不明显的好处是将关注点与数据湖分开。数据湖可以作为无损数据存储与生成用例并行增长。交付用例的工程师可以随着他们满足更复杂的问题，拉取越来越多的数据。

例如，在玩具问题的用例中，播放列表数据可以持续更新，同时我们从如 SeatGeek [[10](https://platform.seatgeek.com/)] 等来源加载票务数据。即使我们不立即使用它，更多的数据也有助于建立价值。利用数据湖与大数据平台结合的力量，即使是在玩具问题中也会显现出来。

# 结论

数据云改变了我们收集和使用数据的方式[5]。我们应该专注于将数据导入云端，并利用 Snowflake、Redshift 或 Databricks 等工具查询数据。在这种情况下，我们可以根据我们能想象或通过机器学习等技术建立的关系，发掘出更多的价值。

在构建数据产品时，至关重要的是通过分析用例拉动后端功能，仅构建所需的数据结构，最小化成本，并确保您的数据产品提供客户价值。

# 参考文献

1.  [E. Ries (2011) 精益创业](https://theleanstartup.com/)

1.  E.F. Codd, (1970) [大规模共享数据银行的关系模型](https://dl.acm.org/doi/10.1145/362384.362685)

1.  Kimball, Ross (2013) [数据仓库工具包](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/books/data-warehouse-dw-toolkit/)

1.  M. Karasou (2019) 向 Spark 数据框添加顺序 ID

1.  [F. Slootman, S. Hamm (2020) 数据云的崛起](https://www.snowflake.com/wp-content/uploads/2020/11/Rise-of-the-Data-Cloud-The-Big-Idea.pdf)

1.  [AWS (2023) 什么是 Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/welcome.html)

1.  [A. Behm 等人 (2022) Photon: 一个快速的湖仓系统查询引擎](https://cs.stanford.edu/~matei/papers/2022/sigmod_photon.pdf)

1.  [dbt Labs (2023) 什么是 dbt?](https://docs.getdbt.com/docs/introduction)

1.  [M. Hagstroem 等人 (2017) 更聪明地跳入数据湖](https://www.mckinsey.com/capabilities/mckinsey-digital/our-insights/a-smarter-way-to-jump-into-data-lakes#/)

1.  Seatgeek (2023) [客户 API](https://platform.seatgeek.com/)
