# 《混沌数据工程宣言》

> 原文：[https://towardsdatascience.com/the-chaos-data-engineering-manifesto-5dc09a182e85?source=collection_archive---------3-----------------------#2023-02-24](https://towardsdatascience.com/the-chaos-data-engineering-manifesto-5dc09a182e85?source=collection_archive---------3-----------------------#2023-02-24)

## 我们可以从软件工程师那里学到的另一个教训是：破坏现有系统，以便让它变得更加可靠。

[](https://medium.com/@shane.murray5?source=post_page-----5dc09a182e85--------------------------------)[![shane murray](../Images/8bb1f3acf15dc26273097e12d03dd616.png)](https://medium.com/@shane.murray5?source=post_page-----5dc09a182e85--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5dc09a182e85--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5dc09a182e85--------------------------------) [shane murray](https://medium.com/@shane.murray5?source=post_page-----5dc09a182e85--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8aa0d9ae3ebd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-chaos-data-engineering-manifesto-5dc09a182e85&user=shane+murray&userId=8aa0d9ae3ebd&source=post_page-8aa0d9ae3ebd----5dc09a182e85---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dc09a182e85--------------------------------) ·12 min read·2023年2月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5dc09a182e85&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-chaos-data-engineering-manifesto-5dc09a182e85&user=shane+murray&userId=8aa0d9ae3ebd&source=-----5dc09a182e85---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5dc09a182e85&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-chaos-data-engineering-manifesto-5dc09a182e85&source=-----5dc09a182e85---------------------bookmark_footer-----------)![](../Images/a73fda73f6bec9620f90b89108400b0f.png)

*照片由* [*Soheb Zaidi*](https://unsplash.com/@msohebzaidi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *提供，刊登于* [*Unsplash*](https://unsplash.com/s/photos/chaos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在*《纽约时报》*作为“指挥室”使用的昏暗凌乱的办公室里已是午夜时分。

强大的流量冲击是不可避免的。在每次重大选举期间，流量会达到高峰，然后猛烈冲击我们不堪重负的系统，随后才会退去，让我们能够评估损害。

我们已经在云环境中待了多年，这有帮助。我们的主要系统可以扩展——我们的文章总是能被服务——但后端服务中的集成点最终会在疯狂的流量压力下崩溃。

然而，2020年的这个夜晚与2014年、2016年和2018年的类似选举之夜有所不同。因为这次流量激增是模拟的，而不是实际发生的选举。

# 推至失败的边缘

模拟与否，这*确实*是生产环境，所以风险很高。当我们的系统J-Kidd——它将广告定位参数带到前端——崩溃时，压抑的恐惧感蔓延开来。这就像是所有的韧带都从以其名字命名的传球控卫的膝盖上被撕扯下来一样。哎呀。

![](../Images/83a91cb62d835846ae9285688a44eac9.png)

*通过* [*维基共享资源*](https://www.google.com/url?sa=i&url=https%3A%2F%2Fcommons.wikimedia.org%2Fwiki%2FFile%3AJason_Kidd_Mike_Woodson.jpg&psig=AOvVaw2eZ0VfIHJlUc8ya-YFm_Rg&ust=1677332195040000&cd=vfe&ved=0CA8QjRxqFwoTCMDoy9Gjrv0CFQAAAAAdAAAAABAD)*。 版权归 KeithAllisonPhoto.com 所有。* 本文件采用[创意共享](https://en.wikipedia.org/wiki/en:Creative_Commons) [署名-相同方式共享 2.0 通用](https://creativecommons.org/licenses/by-sa/2.0/deed.en) 许可协议授权。

J-Kidd并不是唯一一个被列入禁用名单的系统。这次演练的目的就是将我们的系统推向极限，直到它们失败。我们成功了。或者说，从你的角度来看，我们失败了。

第二天，团队进行了调整。我们拆分了系统，实施了安全机制，然后回到赛场迎接第二场比赛。因此，2020年的选举是我记得的第一次，值班工程师们没有绷紧神经，紧握键盘……至少不是因为系统可靠性的问题。

# 预先检讨和混沌工程

我们把这次演练称为“预先检讨”。其概念根源可以追溯到由站点可靠性工程师提出的[混沌工程](https://principlesofchaos.org/)。

对于不熟悉的人来说，混沌工程是一种有纪律的方法论，旨在有意地在系统内引入故障点，以更好地理解其阈值并提高弹性。

这一方法在很大程度上是通过[Netflix的猩猩军团](https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116)的成功而广泛流行起来的，该程序套件会通过删除服务器、区域以及引入其他故障点来自动引入混沌。所有这些都是为了可靠性和弹性。

尽管这一理念在数据工程中并非[完全](https://lakefs.io/blog/chaos-data-engineering/) [陌生](https://www.linkedin.com/posts/eczachly_dataengineering-activity-7033975323838877696-s4Qj?utm_source=share&utm_medium=member_desktop) [的](https://medium.com/geekculture/why-you-should-occasionally-kill-your-data-stack-613143c986ea)，但它确实可以被描述为一种极为罕见的实践。

任何理智的数据工程师都不会看着自己的待办事项清单、团队中未填补的职位、管道的复杂性，然后说：“这需要更难一点。我们引入一些混乱吧。” 这可能是问题的一部分。

数据团队需要超越提供数据质量快照的传统思维，开始考虑如何构建和维护可扩展的可靠数据系统。

我们不能忽视数据在关键操作中的日益重要角色。仅今年，我们就目睹了一次文件删除和一个不同步的遗留数据库[如何使超过4000个航班停飞](https://www.nbcnews.com/news/us-news/us-flights-grounded-faa-outage-rcna65243)。

当然，你不能直接将软件工程概念复制粘贴到数据工程手册中。数据是不同的。DataOps对DevOps方法论的调整就如同[数据可观察性](https://www.montecarlodata.com/blog-what-is-data-observability/)对可观察性的调整。

那么，请把这个宣言视为如何将混乱工程的验证概念应用于数据这个特殊领域的提案。

# 数据混乱工程的5条法则

混乱工程的原则和经验教训是定义数据混乱工程学科轮廓的良好起点。我们的第一条法则结合了两个最重要的方面。

## 第一条法则：对生产保持偏见，但要最小化爆炸半径

在站点可靠性工程师中有一句格言，对于每一位曾经历过相同SQL查询在测试和生产环境中返回不同结果的数据工程师来说，这句话一定会引起共鸣：“没有什么像生产环境那样运作，除了生产环境。”

我会补充一句：“生产数据也是如此。” 数据过于创造性和流动，难以被人类预见。[合成数据](https://medium.com/cord-tech/the-advantages-and-disadvantages-of-synthetic-training-data-b3acbb68e116)已经取得了很大进展，别误解我的意思，它可以是拼图中的一部分，但不太可能模拟关键的边缘情况。

像我一样，单单想到将故障点引入生产系统可能会让你感到不安。这是令人恐惧的。一些数据工程师正当地会怀疑：“在一个如此多工具抽象了基础设施的现代数据栈中，这真的有必要吗？”

我也担心如此。正如开头的轶事和J-Kidd的韧带断裂所示，云的弹性并不是万能的。

实际上，正是这种抽象性和不透明性——加上多个集成点——使得对现代数据堆栈进行压力测试变得如此重要。虽然本地数据库可能限制更多，但数据团队倾向于在日常操作中更频繁地遇到其阈值，因此对其有更好的理解。

让我们暂时跳过哲学上的异议，深入实际。数据是不同的。将假数据引入系统不会有帮助，因为输入改变了输出。这也会变得非常混乱。

这就是法律的第二部分发挥作用的地方：最小化爆炸半径。有一个混乱的光谱和可以使用的工具：

+   仅用文字来说，“假设这失败了，我们该怎么办？”

+   生产中的合成数据

+   像数据差异化这样的技术允许你在生产数据上测试 SQL 代码片段

+   像 LakeFS 这样的解决方案允许你通过创建“混乱分支”或生产环境的完整快照，在更大规模上实现这一点，你可以使用生产数据，但完全隔离。

+   在生产环境中进行操作，并练习你的数据回填技能。毕竟，没有什么能像生产环境那样模拟生产环境。

从较少混乱的场景开始可能是个好主意，并将帮助你理解如何在生产环境中最小化爆炸半径。

深入研究真实生产事件也是一个很好的起点。每个人真的理解发生了什么吗？生产事件是你已经支付的混乱实验，因此确保你从中获得最多的价值。

减少爆炸半径还可能包括诸如备份相关系统或拥有数据可观察性或数据质量监控解决方案的策略，以协助检测和解决数据事件。

## 第二法则：理解没有一个完美的时机（在合理范围内）

另一个混乱工程原则是观察和理解“稳定状态行为”。

这一原则有其智慧，但同样重要的是要理解，数据工程领域还未完全准备好按照“5 9s”或 99.999% 的正常运行时间标准进行衡量。

数据系统始终处于变化中，并且有更广泛的“稳定状态行为”。会有诱惑推迟引入混乱，直到你达到了所谓的“准备”点。好吧，你[无法通过架构应对糟糕的数据](https://www.montecarlodata.com/blog-you-cant-out-architect-bad-data/)，也没有人真正准备好面对混乱。

硅谷的“快速失败”陈词在这里适用。或者[引用 Reid Hoffman](https://twitter.com/reidhoffman/status/847142924240379904?lang=en)，如果你对第一次预演/火灾演习/引入混乱事件的结果不感到尴尬，那你引入它的时机太晚了。

在处理真实数据事件时引入假数据事件可能看起来很傻，但最终这可以帮助你更好地了解你在什么地方用创可贴解决了可能需要重构的更大问题。

## 第三法则：制定假设并识别系统、代码和数据层级的变量

混沌工程鼓励形成关于系统如何反应的假设，以了解需要监控的阈值。它也鼓励利用或模拟过去的真实世界事件或可能发生的事件。

我们将在下一节中深入探讨这些细节，但这里的重要修改是确保这些涵盖系统、代码和数据层级。每个层级的变量都可能产生数据事件，以下是一些快速示例：

+   **系统**：你的数据仓库中没有设置正确的权限。

+   **代码**：一个糟糕的左连接。

+   **数据**：第三方发送给你带有一堆NULLS的垃圾列。

模拟增加的流量水平和关闭服务器会影响数据系统，这些都是重要的测试，但也不要忽视数据系统可能出现的独特和有趣的故障方式。

## 第四法则：所有人都在一个房间里（或者至少是Zoom通话）

这个法则基于我的同事、站点可靠性工程师和混沌实践者[Tim Tischler](https://www.linkedin.com/in/timtischler)的经验。

“混沌工程不仅关乎系统，也关乎人。这两者一起演变，无法分开。这些练习的一半价值来自于把所有工程师放在一个房间里，问‘如果我们做X或者做Y，会发生什么？’你一定会得到不同的答案。一旦你模拟了事件并看到了结果，现在每个人的思维地图都一致了。这是非常宝贵的，”他说。

此外，数据系统和职责的相互依赖性即使在最好的团队中也会造成所有权模糊。常常在这些重叠和责任空白的地方发生并被忽视，例如数据工程师、分析工程师和数据分析师互相指责的地方。

在许多组织中，创建数据的产品工程师和管理数据的数据工程师被团队结构分隔开。他们还常常拥有不同的工具和相同系统与数据的模型。尤其是当数据来自内部构建的系统时，欢迎把这些产品工程师也拉进来。

良好的事件管理和分类通常涉及多个团队，把每个人都放在一个房间里可以让练习更高效。

我也从个人经验中补充，这些练习可以很有趣（以同样奇怪的方式，就像把所有筹码押在红色上很有趣）。我建议数据团队在下一次离线会议中考虑进行一次混沌数据工程演习或预先检讨活动。相比于逃脱房间，这更是一种实际的团队凝聚力练习。

## 第五法则：暂时不要自动化

像 Netflix 的 Simian Army 这样的真正成熟的混沌工程程序是自动化的，甚至是非计划的。虽然这可能创造出更准确的模拟，但现实情况是，目前数据工程还没有自动化工具。如果有的话，我不确定自己是否足够勇敢去使用它们。

到目前为止，Netflix 的一位原始混沌工程师 [描述过](https://medium.com/@njones_18523/chaos-engineering-traps-e3486c526059) 他们并不总是使用自动化，因为混沌可能会比他们能解决的问题更多（特别是在与运行系统的人员合作时），在合理的时间内解决这些问题。

鉴于数据工程目前的可靠性演变以及潜在的意外大范围影响，我建议数据团队更倾向于安排有计划、仔细管理的事件。

# 数据混沌工程变量，也就是要破坏的内容

如果不提供更多具体的测试示例或从中获得的价值，建议数据工程师彻底改革他们的流程并引入更高的风险有些不够实际。

以下是一些建议。

## 系统级别

在这个级别，你应该测试你 IT 和数据系统之间的集成点。这包括从摄取和存储到编排和可视化的数据管道。你可以通过以下方式制造一些混乱：

+   **在 Postgres 中制造问题**- 你可能会发现你需要将操作系统与分析系统解耦。

+   **终止 Airflow 作业或 Spark 集群**- 你可能会发现你需要警报来通知团队这些失败发生时，并 [自愈管道设计](https://www.startdataengineering.com/post/design-patterns/#32-behavioral)。

+   **改变 Spark 核心执行器的数量**- [Zach Wilson 指出](https://www.linkedin.com/posts/eczachly_dataengineering-activity-7023802704107888640-kAtW?utm_source=share&utm_medium=member_desktop) 增加更多的 Spark 执行器核心可能会负面影响可靠性而不是提升性能。对此进行实验。

+   **调整 Snowflake 中的自动挂起策略**- 很多团队正在努力优化他们的数据云成本，很容易削减过度而导致可靠性问题。例如，如果你将自动挂起设置得比查询负载中的任何间隙都要严格，数据仓库可能会陷入持续的自动挂起和自动恢复状态。这可能帮助你发现你需要按数据仓库大小重新组织和分类数据工作负载。或者，可能对谁有权限和权威进行这些类型的更改要更加审慎。

+   **取消发布仪表盘**- 我称之为尖叫测试。如果在取消发布仪表盘几周后没有人提及，可能可以考虑淘汰它。这也可以帮助测试你在数据工程和分析团队中的检测和沟通技能。

+   **模拟流处理管道的连接失败**- Mercari数据团队[迅速巧妙地解决了这个问题](https://www.montecarlodata.com/blog-mercari-data-reliability-engineering/)，得益于数据监控警报。他们暂时计划模拟类似问题，以训练他们的检测、处理和解决流程。

“我们的想法是通过角色扮演特定数据集的警报，并逐步演示事件处理、根本原因分析和与业务沟通的过程，”Mercari数据可靠性工程团队的成员Daniel说道。“通过实际演练，我们可以更好地了解恢复计划，如何优先确保最重要的表格首先恢复，以及如何与跨越四个时区的团队沟通。”

## 代码层

![](../Images/2b7956036c7bc4e6b3db04b443f0fa7a.png)

*当你移除那段代码块时，会发生什么？一切都会崩溃吗？照片由* [*Michał Parzuchowski*](https://unsplash.com/@mparzuchowski?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *拍摄，发布于* [*Unsplash*](https://unsplash.com/s/photos/jenga?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这个层级是ETL或ELT过程中的T（转换）。当你的dbt模型或SQL查询出现问题时会发生什么？是时候了解一下了！

+   **存档一个dbt模型**- 类似于仪表板的极限测试或积木游戏。这个模型真的需要吗？还是当你移除一个块时结构仍然稳定？这有助于评估你的测试和监控；是否存在未知的依赖关系；以及所有权的划分。

+   **调整关键指标的计算方式**- 当组织内存在多个“真实”版本时，相互冲突的趋势不可避免地会削弱对数据的信任。关键指标的变更是否会流动到所有下游报告中？是否存在不规范的定义？这有助于评估定义关键指标的组织流程，以及你是否需要投资于一个[语义层](https://www.getdbt.com/product/semantic-layer/)。

+   **让其他领域解读你的模型或SQL代码**- 当然，某个工程师或团队可能理解特定领域中的模型，但当其他人需要理解时会发生什么？是否有良好的文档？其他人能否协助处理事件？

## 数据层

软件工程师在代码和系统层面也处理可靠性问题，而只有数据工程师必须在数据层面解决潜在问题。我提到过数据是不同的吗？

这个层级不能被忽视，因为你的Airflow任务可能运行正常，你的数据差异可能检查通过，但运行的数据可能完全是垃圾。垃圾进，垃圾出（也许系统崩溃）

+   **更改架构**—软件工程师经常无意中提交更改数据输出方式的代码，从而破坏下游数据管道。你可能需要解耦系统，并且对于某些操作管道，需要一个[数据契约](https://www.montecarlodata.com/blog-data-contracts/)架构（或其他更好的工程协调流程）。

+   **生成错误数据**—这可能会暴露出你的模型对数据源表中的波动、空值等不够稳健。

+   **复制数据**—数据的剧增是否暴露出导致作业失败的瓶颈？你是否需要对某些管道进行微批处理？你的管道在需要时是否具备幂等性？

# 我们在谈论训练

我们以一个控球后卫开始了这个宣言，也许我们用[这句臭名昭著的2002年名言](https://www.facebook.com/SportsCenter/videos/allen-iversons-practice-rant/470685314188618/)来自由后卫艾伦·艾弗森作为对那些不相信“训练如同比赛”的警告，是合适的：

> *“我们在这里谈论的是训练。我是说，听着，我们在谈论训练。不是比赛！不是比赛！不是比赛！我们在谈论训练。不是比赛；不是我为之拼命并每场比赛都像是最后一场的比赛，不是比赛，我们在谈论的是训练，伙计。我的意思是，这有多荒谬？我们在谈论的是训练。*
> 
> *我知道我应该在这里，我知道我应该以身作则，我知道的。我并不是把它当作无关紧要的事情来推开。我知道这很重要。我知道的。我确实知道。但是我们在谈论的是训练，伙计。我们在谈论什么？训练？我们在谈论训练，伙计！*
> 
> *我们在谈论的是训练！我们在谈论的是训练……我们不是在谈论比赛！我们在谈论训练，伙计！”*

那一年，76人队在东部会议首轮季后赛中以五场比赛输给了波士顿凯尔特人队。艾弗森在系列赛中的投篮命中率为38.1%。

[关注我](/@shane.murray5) 在 Medium 上获取更多关于数据领导力、数据科学应用及相关主题的故事。 [订阅](/subscribe/@shane.murray5) 以便将我的故事发送到你的邮箱。
