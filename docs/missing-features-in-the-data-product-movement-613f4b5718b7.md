# 您的数据产品中的缺失特性

> 原文：[`towardsdatascience.com/missing-features-in-the-data-product-movement-613f4b5718b7?source=collection_archive---------14-----------------------#2023-02-15`](https://towardsdatascience.com/missing-features-in-the-data-product-movement-613f4b5718b7?source=collection_archive---------14-----------------------#2023-02-15)

## 参与度、愉悦感和信任作为成果

[](https://medium.com/@cisenbe?source=post_page-----613f4b5718b7--------------------------------)![查德·艾森伯格](https://medium.com/@cisenbe?source=post_page-----613f4b5718b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----613f4b5718b7--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----613f4b5718b7--------------------------------) [查德·艾森伯格](https://medium.com/@cisenbe?source=post_page-----613f4b5718b7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb9113837f160&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmissing-features-in-the-data-product-movement-613f4b5718b7&user=Chad+Isenberg&userId=b9113837f160&source=post_page-b9113837f160----613f4b5718b7---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----613f4b5718b7--------------------------------) ·7 分钟阅读·2023 年 2 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F613f4b5718b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmissing-features-in-the-data-product-movement-613f4b5718b7&user=Chad+Isenberg&userId=b9113837f160&source=-----613f4b5718b7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F613f4b5718b7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmissing-features-in-the-data-product-movement-613f4b5718b7&source=-----613f4b5718b7---------------------bookmark_footer-----------)

我在 Zendesk 领导一个每月的数据讨论小组，我很幸运能够听到来自聪明、多样化和才华横溢的人的各种想法和观点。我们讨论的主题范围从[技术](https://medium.com/airbnb-engineering/how-airbnb-achieved-metric-consistency-at-scale-f23cc53dea70)到[程序性](https://benn.substack.com/p/how-dbt-fails)，并且我们定期将这些与我们的工作流联系起来。在线讨论小组、本地聚会和会议都非常棒，但我觉得将您组织中的人员聚在一起讨论行业趋势会有更多价值，因为这可以为您的团队及其项目提供方向。

我们一直在回到的一个主题是团队的价值以及如何衡量它。在[早期的文章](https://medium.com/towards-data-science/data-teams-as-support-teams-2bb1f1ed31b)中，我讨论了数据团队如何创造价值，但这不是唯一的方法。无论你是在提供业务支持、通过反向 ETL 实现自动化，还是为高级分析和机器学习用例提供数据，总会存在一个挥之不去的问题：我们是否节省或创造了比花费更多的价值？

正如我们的一位与会者所说，我们的客户经常根据“氛围”来衡量我们的影响力。这不应被视为贬低，而是对围绕影响力和价值创建明确指标的困难的一种让步。此外，这个想法中还隐含了分析输出中存在主观且或许无法量化的价值，我们稍后会讨论这个问题。

为数据产品建立归因模型是一个臭名昭著的难题，我不会尝试在这里解决所有元素。虽然我相信我们可以从一个广泛的归因框架中受益，但鉴于数据降低成本和增加收入的方式众多，我不知道这是否甚至可能。相反，我想关注几个我认为被忽视的具体组件，特别是：

1.  数据的心理社会价值

1.  客户的愉悦和满意

1.  数据质量作为核心交付物

# 我们是拉拉队员吗？

第一个观察点更不涉及我们应如何“做数据产品”，而是关于基本要求。在讨论中，我提出了一个观点：我们不一定知道数据驱动型公司成功的原因，但[研究表明它们确实成功](https://cisr.mit.edu/publication/2022_0101_Dashboarding_WeillWoerner)。有人回应道：也许这些仪表盘“没有做任何事情”，至少在推动特定、明确的行动方面没有发挥作用。如果它们的价值在于激励和对齐我们的客户，而不是（或除了）可操作的见解，那会怎样呢？

![](img/5ae353fad8599c59240cb1b3370b4419.png)

由[Rojan Maharjan](https://unsplash.com/@kathmandude_?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我的职业生涯的某个阶段，我曾担任一家批发公司的 BI 开发人员，我们有一份报告向销售团队提供销售指标的汇总。这份报告在整个月的使用情况还算合理，但在月末使用量激增，一些用户每小时刷新好几次。我记得对这种趋势感到非常困惑。

我相信这种行为背后有几个操作性用例，但事后看来，我认为报告的最有价值的输出之一是激励。看到他们的货物几乎实时发出使我们的销售人员感到兴奋；它给了他们额外的“动力”，让他们在争分夺秒地完成或超越配额时进行电话和达成交易。这份报告正在做非常实际的工作，推动业务流程，即使这不是我们通常期望的方式。

还值得注意的是，许多这些数据文物本质上具有社交属性。一个仪表板是一个共享对象，可以作为同事之间对话的共同纽带。即使底层数据并不出色或特别有洞察力，由此引发的对话和共享叙事也可能是有价值的。对齐是组织的强大工具，而数据可以推动对齐。

作为“客观”的人，许多数据专业人员可能不愿意接受“灵感”是有效或有价值的输出；然而，如果我们不利用数据来激励自己，我们就会错过一些东西。我最初也抵触这个想法，但反思我的职业生涯，我有很多类似于上述的故事和轶事，我不禁觉得这种定性影响是显著的。也许我从这次经历中得到的最重要的教训是，仅仅因为我无法感知到生成价值的机制，并不意味着它就不存在。

最后，数据向我们展示一些隐藏的见解或反对意见的叙事现在已经成了一个陈词滥调，但当数据确认我们的信念并让我们安心我们走在正确的轨道上时，确实会发生一些强大的事情。数据常常支持现状而不是颠覆它，这应该被庆祝，而不是害怕。

# 面包与马戏团

![](img/d11108b1ff07cfd487f4590e494790ba.png)

图片由 [Mathew Schwartz](https://unsplash.com/@cadop?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

同样，我认为考虑我们的客户是否喜欢我们的数据产品是很重要的。这可能包括为分析师和数据科学家提供符合人体工程学的命名规范和易于使用的表格，也包括为业务用户提供美观的仪表板。

这是什么样的呢？我们是否应该在数据产品中构建反馈机制？我们是否应该进行调查？我会提醒我们不要过于强硬；与所有内部产品一样，我们有可能与实际价值链（即我们如何获得报酬）保持脆弱的关系。

不过，我认为在数据产品生命周期中提供轻量级系统以驱动反馈仍然有空间。哪些功能受到喜爱？哪些功能被讨厌？何时数据产品可以进入“维护模式”，何时我们应该结束？根据我的经验，这些问题几乎总是以临时的方式提出，到产品生命周期的那个阶段，甚至识别活跃用户也可能被已经被遗忘的过程自动化所复杂化。

# 质量作为可交付物

在我们结束每月会议时，团队中的数据工程经理之一，[Niral Patel](https://www.linkedin.com/in/niralpatel/)提出了一个中肯的问题：为什么质量总是被当作事后的思考，而它却是满足我们客户需求的关键因素？

在某种程度上，这涉及到资源问题。数据团队经常是“临时拼凑”的，要求承担远超其能力的工作。他们陷入了操作困境和不明确的需求中。在时间紧迫时，交付一个不完整的东西总比什么都不交付要好。

但就像标准化测试中因错误答案而受罚一样，数据质量差会破坏信任。如果你的团队持续产生不准确、过时且不一致的仪表板，你的客户将失去对你的信任。这种损害不仅限于你和你的团队；你还在助长一种需要多年努力和大量人力才能修复的不信任文化。

我们有责任向利益相关者推广数据质量作为我们产品的关键特性，并需要让他们参与权衡取舍。由于数据质量有许多维度，我们的客户必须参与优先级排序：我们应如何处理异常数据量？遇到模式变化时我们应该怎么做？准时交付不完整的数据与迟交完整的数据，哪个更重要？虽然“差”数据质量有一定的感觉，但在交付“好”数据时的权衡管理要复杂得多且具有上下文敏感性。

在数据治理领域正进行着重要的工作，现在是时候将这些应用到我们的数据产品中，从一开始就如此，并且每次都如此。质量是一个基本特性，与我们产品的设计、接口和用例同样重要。

# 我们所需的指标

虽然有产品和服务可以提供我们使用数据，但数据本身是不够的；我们需要指标和相关结果。我认为以下指标（及相关的监控/可观察性）是成功数据产品的关键组件：

1.  一种理想的使用情况测量方法，能够区分自动化与临时使用

1.  一种理想的特征级满意度测量方法

1.  一种理想的数据质量测量方法，涵盖所有关键方面

这些组件远远不够，但我相信它们对于数据团队充分判断他们的工作是否产生了影响是必要的。是的，美元或 FTE 等价物归因是金标准，但在无法获得这些的情况下，知道我们向投入、满意的用户提供了高质量的数据是一个很好的替代指标。这些指标还允许我们捕捉数据产品的那些意外好处；也许我们的用户将输出应用于完全不同的过程。

Niral 在后续谈话中提出的一个重要观点是，数据产品应该具有可操作性；自动化或仪表板应推动某些产生价值的行动。区分创建数据产品的主要目标是心理社会影响，还是将心理社会影响视为一种价值维度是至关重要的。我不会建议在规划阶段仅以提高士气或团队凝聚力为唯一目标来制作产品，因为这种价值主张充其量也只是勉强成立。相反，如果我们的产品未能推动预期的行动但仍然非常受欢迎，我们应该有框架来调查原因。本质上，我们不希望仅仅因为它没有“按预期工作”就抛弃一些有价值的东西。

总结一位参与者的精彩见解：数据团队为我们利益相关者的工作场所福祉做出了贡献；如果提供数据提取或仪表板使用户感到愉快，那就是有用的。这肯定不是数据团队的唯一价值主张，但也不是我们应该忽视的东西。尽管倾向于依赖客观性并忽视我们工作中的人文元素（“数据可以‘克服’的‘弱点’”）可能很诱人，但现实是我们是社会性生物，工作在复杂的组织中。我们有工具来穿透主观性，但大多数工作会经过其他人的手；承认和认识到这个人文元素的价值大于忽视它。

我刚完成这篇文章的初稿几分钟后，[Benn Stancil](https://medium.com/u/b3c63c3caf8e?source=post_page-----613f4b5718b7--------------------------------) 发布了一篇关于[他对放弃数据客观性的 discomfort](https://benn.substack.com/p/the-case-for-being-biased)的智能文章。如果我不把你引导到他的方向，我会感到遗憾。
