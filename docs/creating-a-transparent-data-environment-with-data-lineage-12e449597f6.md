# 创建一个透明的数据环境与数据血统

> 原文：[`towardsdatascience.com/creating-a-transparent-data-environment-with-data-lineage-12e449597f6`](https://towardsdatascience.com/creating-a-transparent-data-environment-with-data-lineage-12e449597f6)

## 列级血统在你的技术栈中的好处

[](https://madison-schott.medium.com/?source=post_page-----12e449597f6--------------------------------)![Madison Schott](https://madison-schott.medium.com/?source=post_page-----12e449597f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----12e449597f6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----12e449597f6--------------------------------) [Madison Schott](https://madison-schott.medium.com/?source=post_page-----12e449597f6--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----12e449597f6--------------------------------) ·5 分钟阅读·2023 年 4 月 11 日

--

![](img/36bd79f779c717a98d9e4ec2b4b15c40.png)

图片由 [Bozhin Karaivanov](https://unsplash.com/@bkaraivanov?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/string?orientation=landscape&utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当我编写数据转换时，我经常会注意到不一致的地方。也许数据源中的某一列没有遵循我制定的命名规则，或者时间戳没有被转换成正确的数据类型。我会在发现这些问题时立即进行更改，而不是等到后续再去修复。

然而，第二天，我发现生产数据管道出现故障。由于我昨天做的那个“简单”的名称更改，几个下游的数据模型出现了问题。现在，我需要急忙更改所有下游数据模型中对该列的引用。

不幸的是，如果你没有一个能够显示数据流向的工具，这种情况可能很常见。你被迫被动等待，直到某些东西出故障才知道需要哪些更改。幸运的是，数据血统工具可以帮助我们避免这种情况，并对依赖关系采取主动措施。

# 什么是数据血统？

[数据血统](https://www.y42.com/blog/data-lineage/?utm_source=medium&utm_medium=blog&utm_campaign=lineage_transparency_article) 展示了数据在通过管道移动时所遵循的路径。它突出了数据的来源、如何转换以及包含在哪些可视化中。

数据血统有助于了解特定模型中使用了哪些数据源，以及在这个过程中是否引用了其他模型。你可以利用它快速发现依赖关系。它将透明度融入你的技术栈中，这是生成高质量数据的**关键**特性。

# 如何利用数据血缘建立透明度

当正确使用时，数据血缘有助于在你的业务中建立透明度。以下是我推荐使用数据血缘工具的三大理由。

# 数据血缘准确显示生产模型中使用了哪些数据源。

数据血缘工具非常适合显示哪些数据模型依赖于哪些数据源。 [列级血缘工具](https://www.y42.com/product/catalog-lineage/?utm_source=medium&utm_medium=blog&utm_campaign=lineage_transparency_article) 更加强大，因为它们显示了上游的确切列如何生成下游数据模型中的列。如果列在不同的数据源和模型中被重命名，这一点尤其有用。

列级血缘工具在数据工程师进行影响源数据摄取的更改时特别有用。他们可以参考下游数据模型的血缘，查看哪些模式更改会对业务产生重大影响。这将使他们能够与分析工程师合作，最小化业务用户的停机时间。

分析工程师可以优先处理最重要的数据源和模型的文档和测试。如果没有血缘工具，你只能猜测哪些会有最大影响。有了这些工具，你可以专注于驱动最多业务决策的数据。

血缘工具对业务团队也很有帮助，因为它们允许他们查看数据团队最常使用哪些数据源。我看到许多过时的 Google Sheets 堆积在数据环境中，因为没有人知道它们是否重要。能够看到从源到洞察的表的血缘将使业务用户能够权衡它们的重要性，同时保持他们的数据资产清洁和最新。

# 数据血缘使你能够看到分析师在仪表板和报告中使用的模型。

数据血缘工具让你洞察哪些数据源和数据模型被用来支持业务使用的关键仪表板和报告。这将帮助分析师和数据工程师更好地了解每天用于做出关键业务决策的模型。当每个人都了解最关键的数据源时，像模式更改、数据类型转换和重命名这样的事情可以变得更加战略性。这帮助每个人了解完整的影响和潜在的停机时间。

当数据和工程团队了解数据管道变更对业务的影响时，他们更有可能进行有效沟通并积极寻求解决方案。例如，如果数据工程师在没有数据血缘工具的情况下更改了源模式，他们可能不知道这会破坏营销投资回报率仪表板。然而，如果工程师有血缘工具来查看哪些内容依赖于这个源，他们可以通知业务团队停机时间。

开放的沟通有助于建立数据团队与业务团队之间的信任，这对于数据驱动的组织至关重要。当两个团队互相信任时，可以做出更好的决策。业务将对数据堆栈充满信心，并能够轻松验证所见的结果。此外，数据团队可以自信地对流水线进行更改，知道不会破坏任何东西，从而推动业务前进。

# 数据 lineage 揭示了数据瓶颈。

数据 lineage 工具通过引起对数据从源头到最终产品的流动过程中的瓶颈的关注来优化你的数据流水线。你不再需要 *猜测* 某些转换为什么要花费如此长的时间，甚至为什么它们会耗费你这么多钱，你可以看到数据的确切流动情况。

例如，你可能会注意到两个数据模型在数据量和查询复杂性面前耗时异常长。你查看这些模型的 lineage，发现它们引用了相同的源数据集。在查看该源数据后，你可以立即识别出问题所在。

Lineage 工具允许你理解不同部分之间的依赖关系及其如何改进。查看你的数据如何流经不同的模型可以让你轻松识别可以优化的领域。这使你能够创建一个更快、更便宜且更可靠的数据流水线。

# 数据 lineage 作为一种主动工具

当你更新其中一个源数据模型时，你应始终首先参考你的数据 lineage 工具。查看哪些下游数据模型正在使用该源。来自该源的哪些列正在被使用？这时，一个 [列级 lineage 工具](https://www.y42.com/?utm_source=medium&utm_medium=blog&utm_campaign=lineage_transparency_article) 就派上用场了，比如 [Y42](https://www.y42.com/?utm_source=medium&utm_medium=blog&utm_campaign=lineage_transparency_article) 提供的工具。一旦你了解了数据源的变化如何影响下游模型和仪表盘，你就可以自信地进行更改。

当我重命名数据源中的一列时，我总是使用一个 lineage 工具来查看这一变化如何向下游传播。现在，我再也不会遇到模型计划运行时出现意外的生产流水线失败。

数据 lineage 工具之所以强大，是因为它们为你的数据堆栈、数据团队以及与业务利益相关者的协作带来了透明度。进行更改不再是一个猜测游戏，因为你可以提前测量其即时影响。
