# 如何为任何团队规模构建数据科学战略

> 原文：[`towardsdatascience.com/build-a-data-science-strategy-for-anyteams-of-any-size-with-this-one-article-consultants-dont-371048bf36ed`](https://towardsdatascience.com/build-a-data-science-strategy-for-anyteams-of-any-size-with-this-one-article-consultants-dont-371048bf36ed)

## 创建一个快速变动且对变化具有弹性的文化和实践

[](https://medium.com/@seaneaster?source=post_page-----371048bf36ed--------------------------------)![肖恩·伊斯特](https://medium.com/@seaneaster?source=post_page-----371048bf36ed--------------------------------)[](https://towardsdatascience.com/?source=post_page-----371048bf36ed--------------------------------)![走向数据科学](https://towardsdatascience.com/?source=post_page-----371048bf36ed--------------------------------) [肖恩·伊斯特](https://medium.com/@seaneaster?source=post_page-----371048bf36ed--------------------------------)

·发表于 [走向数据科学](https://towardsdatascience.com/?source=post_page-----371048bf36ed--------------------------------) ·20 分钟阅读·2023 年 9 月 11 日

--

![](img/64ec3d72d1bbc5bce8a418ad238fc293.png)

照片由 [Maarten van den Heuvel](https://unsplash.com/@mvdheuvel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/photos/_pc8aMbI9UQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 创建一个快速变动且对变化具有弹性的文化和实践

如果你是一个数据科学领导者，被要求在“建立我们的数据科学战略”时拥有很大的自由和很少的方向，这篇文章会对你有所帮助。我们将讨论：

+   我们所说的战略是什么：仅仅是一个计划？一个路线图？还是更多或更少的东西？在本节中，我们将具体化并采用一个工作定义，以了解我们在构建战略时实际构建了什么。

+   这个概念如何在实际的组织背景下应用于数据科学团队？在这里，我们将探讨我们对战略的概念如何适用于数据科学，并具体说明我们的战略应用于哪些方面。

+   如何实际制定该战略。

在整个过程中，我们将大量借鉴研发战略的方法，因为它与数据科学面临的关键挑战类似：创新的使命，以及追求发现所带来的不确定性。当我们结束时，你将获得一个明确的战略定义，以及一个适用于任何规模组织的有用的制定过程。

# 什么是战略？

如果你像我一样，没有高级 MBA 学位，也从未参加过商业战略研讨会，你可能会对有人要求你制定“数据科学战略”时究竟想要什么感到困惑。你可能会发现最初的搜索并没有多大帮助。像[三 C 模型](https://www.waca.associates/en/web-analytics-dictionary/3cs-model/)（顾客、竞争者、公司）这样的经典强大框架，在决定公司应该竞争的领域时非常合理。将其应用于一个职能或团队时，你会发现自己感觉在拉伸这些概念的承受范围。

如果你像我一样，阅读像[*战略领主*](https://amzn.to/432dMFX)和[*麦肯锡方法*](https://amzn.to/3phxC2h)这样的书籍会让你陷入一个相当深入的阅读漩涡中。（附属链接。）前者是一本令人愉快的商业历史著作，后者是从成功的咨询公司经验中提炼出的有用技巧合集。两者都没有提供快速的答案。阅读《战略领主》的一个非常有益的副作用是了解到数据科学家们并不孤单：“[我]很容易把战略与战略规划混为一谈，但这也是危险的。[…] 即使今天，拥有计划的公司仍然远多于拥有战略的公司。刮掉大多数计划，你会发现某种版本的‘我们将继续做我们一直在做的事，但明年我们会做得更多和/或更好’。这种定义混淆在我的经验中也有所体现，几次对战略的请求实际上简化为‘你接下来几个月的计划是什么？’”

我们将在本文其余部分采用的一个非常有用的战略定义，来源于 Gary Pisano 的[这篇关于研发战略的工作论文](https://www.hbs.edu/ris/Publication%20Files/12-095_fb1bdf97-e0ec-4a82-b7c0-42279dd4d00e.pdf)：“战略不过是一种*对行为模式的承诺*，旨在帮助赢得竞争。”这个定义的美妙之处在于它可以适用于组织的任何层级和目的。所有类型和规模的团队都参与组织的竞争努力，所有团队都可以定义并声明他们用来集中这些努力的行为模式。

> 战略不过是一种*对行为模式的承诺*，旨在帮助赢得竞争。
> 
> —Gary Pisano

Pisano 提出了一个好的战略的三个要求：一致性、一致性和对齐。战略应该帮助我们做出一致的决策，这些决策累积起来有助于实现预期目标；应该帮助组织的各个角落将其分散的战术决策协调一致；并且应该使地方行动与更大的集体努力保持一致。

最终，它们都建立在核心假设上，即关于在竞争中提供优势的赌注。皮萨诺的有用例子是苹果，其战略“开发易于使用、外观美观的产品，并与消费者数字世界中的更广泛设备系统无缝集成”建立在一个核心假设上：“客户将愿意为具有这些属性的产品支付显著更高的价格。”

本质上，根据这个定义，所有策略都是包装决策逻辑的赌注：它们为所有各方提供了确定哪些行动有助于集体努力的方法。

我们将采用这个策略定义，并努力定义我们自己的核心战略假设，关于数据科学如何为我们的组织增值，以及我们将在追求该价值的过程中坚持的模式。此外，我们将假设我们的母公司已经制定了自己的战略，这一输入在我们应用第三个对齐测试时将是至关重要的。在确定了我们最终战略的形式之后，我们将把注意力转向限定其范围。

# 我们所说的数据科学是什么意思，这个战略概念如何适用？

为了提醒我的朋友们我有多么有趣，我给几个人发了相同的短信，“你听到‘数据科学策略’时会想到什么？”答案从对数据基础设施和 MLOps 的深思熟虑，到对问题模糊性的健康反应（我觉得被看到了），再到丰富多彩的“胡说八道”和“我的理想工作”。

尽管样本较小，但来自这一群体的多样化回应——包括初创公司和大型公司的资深产品经理、数据科学负责人和顾问——表明了这个术语的定义可能有多么模糊。更糟糕的是，数据科学家还面临第二层次的困惑：所谓的“数据科学”在实践中往往取决于公司招聘所需的技能，并用流行的标题装点。

为了在我们的分析中固定这些自由度，我们将首先采用一个共同的数据科学定义来进行本文的其余部分：致力于通过建模组织的可用数据来创造价值和竞争优势的职能。这可以采取几种典型形式：

+   构建优化客户决策的机器学习模型以用于生产

+   建立帮助各级员工完成工作的模型，可能应用于客户互动的人工环节应用。

+   建立可解释的模型以辅助商业决策

注意，我们排除了 BI 和分析，仅仅是为了聚焦，而不是因为它们不如建模工作有价值。你的分析部门和数据科学部门应当顺利合作。（我曾在[这里](https://medium.com/towards-data-science/analytics-and-data-science-are-better-together-f5190a7e91b9)写过这方面的内容。）

一些人，比如我的朋友和谷歌产品经理[卡罗尔·斯科达斯·沃尔波特](https://www.linkedin.com/in/carolwalport/)，会建议数据科学战略包括“如何使数据和基础设施处于足够好的状态，以进行分析或机器学习。我会说这是如何使团队完成所有工作的。”我们将有意排除这些更广泛的数据战略项目（抱歉，卡罗尔）。不过，我们将讨论如何应对数据和基础设施限制，以及如何发展数据科学战略以积极指导更广泛的数据战略。

现在我们有了界限：我们正在构建一套核心战略假设，关于机器学习和/或人工智能如何为拥有自己定义战略或目标的组织增加最大价值，以及团队在追求这一价值时将承诺的一系列模式。我们该如何开始？

# 建立我们的战略核心假设：从一个赢得 AI 的心态开始

经验丰富的机器学习产品经理、工程师和数据科学家常常会提到，机器学习产品与传统软件不同。一个组织必须考虑模型错误、数据漂移、模型监控和重新调整的风险——这也是现代 MLOps 的出现原因。而且，很容易在工程中犯错，使 ML 应用陷入技术债务的沼泽中。（请参阅[“机器学习：技术债务的高利贷”](https://research.google/pubs/pub43146/)以获取有关此主题的精彩阅读。）那么，面对所有这些成本，我们为什么要这么做？

从根本上说，我们考虑 AI 解决方案是因为复杂的模型已经证明能够检测到有价值的模式。这些模式可以是从暗示新分段的客户偏好的聚类到神经网络发现以优化预测的潜在表示。任何给定的机器学习构建都依赖于一个假设或预期，即模型可以检测到可以改进过程、发现可操作的发现或改进有价值的预测的模式。

在定义任何规模的数据科学团队的核心战略假设时，我们可以从这个麦肯锡的示例描述开始，描述 AI 驱动的公司如何以不同的方式思考。来自[“赢得 AI 是一种心态”](https://www.mckinsey.com/business-functions/quantumblack/our-insights/winning-with-ai-is-a-state-of-mind)：

> *如果我们选择了正确的用例，并以正确的方式进行，我们将不断了解我们的客户及其需求，并不断改进我们服务他们的方式。*

这是构建数据科学战略时非常有用的视角：它让我们专注于最大化学习，我们需要做的只是确定我们组织对“正确”的定义。但对我们来说，“正确”的用例是什么？

在这里，皮萨诺再次提供了帮助，定义了 R&D 战略的四个要素，这些要素很好地适用于数据科学：

+   架构：我们数据科学职能的组织结构（集中式、分布式）和地理结构。

+   过程：管理我们工作的正式和非正式方式。

+   人员：从我们希望吸引的技能组合到我们的人才价值主张的一切。

+   组合：我们如何在项目类型之间分配资源，以及“排序、优先级和选择项目的标准”。

我们将从最后一个概念开始，重点定义我们组织的理想项目组合，即我们可以说服自己能带来最大价值的组合。鉴于组织之间的巨大差异，我们将从每个组织面临的一个挑战开始：风险。

# 定义你的目标组合：根据你的战略确定风险水平和管理。

建模工作具有不确定的结果。“机器学习可以做得更好”是我们经常根据历史和直觉提出的论点，且这通常被证明是正确的。但我们从一开始就不知道它的效果如何，直到我们通过实际构建证明机器学习能解决问题。了解任何给定用例的答案可能需要不同程度的努力，从而产生不同程度的成本。这种答案的不确定性也可能不同，取决于我们的模型被应用的广泛程度以及我们对数据的理解程度。

一位朋友和医疗分析产品负责人，[约翰·梅纳德](https://www.linkedin.com/in/johnathan-menard/)将风险定义为数据科学战略中的一个明确部分：“你如何维护一个小规模和大规模的投注管道，同时保持健康的期望？当数据无法支持项目时，你的策略是什么？如果项目未能满足要求，你会如何调整交付内容？”

对于组织来说，明确和具体地了解他们能够承担的资源水平及其时间长度是明智的。以下是对任何个人建模工作提出的一些有用问题：

+   成功的估计可能性：这个模型用例成功的概率是多少？

+   预期回报范围：如果成功，这个项目是否能在一个可以大规模带来巨大回报的过程上带来微小的改进？一个突破是否能让你与竞争对手区分开来？

+   发现失败的预期时间：需要多长时间才能了解一个项目的假设价值主张是否会实现？在了解到这个项目不会成功之前，你能花费的最少资源是多少？

希望这些原则是简单明了的，并且都是共识中的好事。理想的项目可能会成功，带来巨大的投资回报，如果失败，则应尽早失败。这种完美的三位一体往往难以实现。关键在于做出适合你组织的权衡。

一个早期阶段的初创公司，专注于利用人工智能颠覆特定领域，可能会有投资者、领导层和员工接受公司作为对特定方法的单一大型投资。或者，它可能更倾向于那些能够快速进入生产并允许快速调整的小型项目。相反，如果我们在一家大型、成熟的公司和监管严格的行业中，并且利益相关者对机器学习持怀疑态度，我们可能会选择将投资组合偏向于低工作量的项目，这些项目提供渐进的价值并快速失败。这可以帮助建立初步信任，使利益相关者适应数据科学项目固有的不确定性，并使团队围绕更雄心勃勃的项目达成一致。成功的早期小型项目还可以加强对同一问题领域的更大项目的支持。

以下是如何定义目标投资组合的一些示例，包括项目范围、持续时间和预期回报：

+   “由于我们在集体数据科学旅程中仍处于早期阶段，我们专注于小型、低工作量和快速失败的用例，这样可以在不冒大量人员时间风险的情况下发现机会。”

+   “我们已经确定了一个包含三个大型机器学习项目的投资组合，每一个项目都有可能释放巨大的价值。”

+   “我们的目标是平衡小型、中型和大型项目，并与相应的回报水平相匹配。这使我们能够在追求具有颠覆性潜力的同时，频繁获得胜利。”

作为应用于我们完整投资组合的最终原则，目标是一个具有非相关成功的项目集合。也就是说，我们希望看到我们的投资组合，并感知项目将独立成功或失败。如果多个项目依赖于共同的假设，如果我们感到它们如此紧密相关以至于它们会一起成功或失败，那么我们应该重新考虑选择。

当我们完成以下任务时，我们就完成了这个阶段：

+   调查了我们的数据科学和机器学习机会

+   按投资、回报和成功可能性进行了绘制

+   选择了一个与我们的目标和风险承受能力一致的粗略优先级列表

现在我们已经确定了目标投资组合，我们将转向确保我们的流程使我们能够快速识别、范围定义和交付有价值的项目。

# 将你的投资组合集中在你的团队独特的解决方案上

建设还是购买的问题是一个长期存在的问题，并且通常涉及复杂的组织动态。寻找 AI 解决方案的供应商和初创公司不乏其人。许多是“江湖术士”；许多是有效的。许多内部技术和数据科学团队将前者视为笑话，将后者视为竞争对手，并且将区分两者所花费的时间视为巨大的时间浪费。这是有道理的，因为检查供应商的时间并不能提升建模者的技能，如果组织不奖励他们的努力，那么这就是数据科学家付出的成本，而没有职业上的回报。这种人际关系复杂性加剧了本已复杂的商业案例：所有典型的软件解决方案关注点仍然存在。你仍然需要担心供应商锁定和云集成等问题。然而，我们都应该愿意购买那些提供更高投资回报率的供应商产品，如果你考虑到内部团队相对于现成解决方案的独特优势，你可以排除干扰。

特别是，你的内部团队通常可以访问你组织的大部分（可能是全部）专有数据。这意味着内部团队可能会比单一用途的供应商解决方案更深入地理解这些数据，并更容易将其与其他来源进行丰富。只要有足够的时间和计算资源，一个有能力的内部团队可能会比单一用途的供应商解决方案更胜一筹。（这里面有个 PAC 理论的笑话。）但这值得吗？

标准的投资回报率和替代方案分析在这里是关键，重点是你内部市场的时间。假设我们正在优化一个电子商务网站上的广告投放。我们已经将供应商名单缩小到一个领先者，该供应商使用的是一种多臂赌博方法，这是在撰写本文时领先的市场优化供应商中常见的方法。我们估计供应商集成的时间为一个月。或者，我们可以建立自己的 MAB，并估计需要六个月。我们是否期望自己构建的 MAB 能够超越供应商的 MAB，并且值得为了这个延迟而付出？

这要看情况。使用汤普森采样进行多臂赌博问题（MAB）可以为预期的遗憾提供对数界限，这是一种行话，意味着它在探索选项时不会留下一大堆未利用的价值。无论是由你的内部团队还是供应商实施，这一说法仍然可以证明是正确的。相反，你的内部团队更接近你的数据，将这种用例带到内部相当于一个赌注，即你会在数据中找到足够丰富的信号来超越供应商产品。也许你的团队可以注入现成解决方案所没有的领域知识，从而提供有价值的优势。最后，考虑你的内部团队的机会成本：是否有其他高价值的项目他们可以从事？如果有，一个选项是测试供应商，处理其他项目，并在获得可衡量的供应商结果后重新评估。

当我们完成以下任务时，这一阶段就结束了：

+   回顾之前步骤中的机会，并对每个机会回答，“我们能买到这个吗？”

+   对于每个可购买的解决方案，回答我们是否有独特的已知或假设的内部优势。

+   对于每个需要做出真实权衡的领域，进行权衡分析。

确定了我们内部团队的战略竞争优势后，我们现在将考虑我们的内部流程、工具和数据能力。

# 在你的知识工厂工具和数据供应链周围建立流程。

我与许多经验丰富的数据科学家讨论过时间投入的问题，每个人都提到发现、处理、清理和移动（到适当的计算环境）数据占据了他们大部分工作时间。正如另一组麦肯锡作者[在 AutoML 和 AI 人才战略上写道](https://www.mckinsey.com/business-functions/quantumblack/our-insights/rethinking-ai-talent-strategy-as-automated-machine-learning-comes-of-age)：“许多组织发现，数据科学家花费 60%到 80%的时间来准备建模数据。一旦初步模型构建完成，根据一些分析，只有 4%的时间用于测试和调优代码。”这并不是大多数人进入这一领域的原因。在我们大多数人的心中，这是一种为了构建有影响力的模型的乐趣而付出的代价。因此，我们常常谈论数据科学家成功所需的“基础”。根据我的经验，这种框架很快会阻碍我们，我将挑战我们将自己视为一个模型工厂，受到工具和复杂且常常有问题的数据供应链的限制。

坦白说：当讨论平台时，我从未相信这些“基础”论点。

“数据和机器学习平台是成功的机器学习所依赖的基础，”这是无数幻灯片和白皮书中的**明确声明**。 “如果没有强大的基础，”某些顾问**父爱般地**总结道，“一切都会崩溃。”

然而，这里有个问题：很少有事情会因为没有机器学习而“崩溃”。如果你的房子建在不好的基础上，车库可能会塌陷在自己身上，甚至你也会受害。如果你在没有完善的数据和机器学习平台的情况下开始机器学习项目，那么你的模型构建将会……需要更长时间。而且没有那种新奇的机器学习模型，你的业务很可能会继续以原有方式运行，尽管缺少了机器学习原本旨在提供的某种竞争优势。但在平庸中持续并非末日。

这就是这个陈词滥调让我感到困惑的地方。它试图吓唬高管们投入平台建设——值得强调的是有价值的建设——好像没有它们世界就会末日，但其实不会。我们大声喊着天空要塌下来，然后当一个利益相关者遇到他们习惯的老雨时，我们失去了信誉。

尽管如此，我敢打赌，拥有强大机器学习能力的公司将超越那些没有这种能力的竞争者——我明白作为建模负责人我的职业生涯正是这样一种赌注——而现代数据和 MLOps 能力可以大大缩短 AI 能力的市场时间。请参见麦肯锡论文中的摘录 “[像科技本地人一样扩展 AI: CEO 的角色](https://www.mckinsey.com/business-functions/quantumblack/our-insights/scaling-ai-like-a-tech-native-the-ceos-role)”，强调由我加的：

> *我们经常听到高管们说，将 AI 解决方案* ***从构想到实施需要九个月到一年以上****，使得很难跟上市场动态的变化。即便经过多年的投资，领导者们经常告诉我们他们的组织并没有加快速度。相比之下，* ***采用 MLOps 的公司可以在仅仅两到 12 周内将构思转变为实际解决方案*** *而不会增加人员或技术债务，减少了实现价值的时间，并释放团队以更快地扩展 AI。*

你的数据科学战略需要考虑你的组织和工具约束，并采用在这些约束下能产生可操作模型或知识单元的模式。也就是说，建模项目应该总是包含：

1.  清晰地了解最小可行建模数据。你的数据科学团队应该知道源数据的位置，并且对数据需要如何转化有一个大致的了解。

1.  实现价值的直接和现实路径。你将如何让一个具有足够性能的模型投入使用，或者以其他方式应用模型结果？

早期阶段的公司或团队如果在架构和工具方面拥有完全的自由，将有利于采用现代 MLOps 实践，这将使得快速原型设计、部署和监控模型以评估其在实际世界中影响变得更加容易。与传统遗留技术并行工作或在其中工作的团队可能会发现这些技术并未考虑 ML 集成，并且部署是一个庞大的、沉重的过程。受严格监管行业中的公司将发现许多应用程序需要高度的可解释性和风险控制。

这些挑战没有不可克服的。我们只需在时间表的影响上保持原则性和聪明，并将其纳入我们的决策中。

当我们完成这个阶段时，我们会有：

+   调查我们计划中的用例，以确定每个用例获取数据的路径以便开始。

+   确定每个用例的实现价值路径，如果它能够成功的话。

+   将这一点纳入我们的预期投资中，并从第一步开始进行调整。

+   根据我们发现的任何变化调整了我们的优先级。

在完善了我们关于数据科学部署的想法后，我们将考虑工作模型以确保一致性。

# 架构与组织：构建一个能够持续成功的组织结构。

Pisano 将架构定义为“围绕研发在组织和地理上的结构所做的一系列决策。” 设计这点包括对如何将我们的数据科学家与业务部门整合做出深思熟虑的决定。他们是完全集中并有正式的接收流程？向不同的业务单位汇报？集中并嵌入其中？汇报结构和决策权可能不在你的掌控之下，特别是当你被要求为有明确汇报线的部门建立战略时。但如果这些点正在讨论中，这里有一些最大化数据科学成果价值的考虑因素。

**你的数据科学家会得到良好的支持并被适当衡量吗？** 考虑一下初级数据科学人才的来源。数据科学家来自各种定量背景，通常具有理论和实践技能的混合。一个典型的硕士毕业生在这些形成阶段中建立了技能和理解，并向领域专家展示了这些理解。这通常不包括大量的与非专家沟通技术发现的培训。

与他们在商业环境中的经历相比，他们可能对领域的了解较少，并且是少数拥有方法知识的人之一。他们将被要求应用少数人理解的技术。他们的项目必然比标准软件构建包含更多的不确定性。他们的成功将依赖于更多因素，许多因素超出数据科学家的控制范围，他们在阐述要求以最大化成功机会方面经验有限。将所有这些因素综合考虑，我们开始看到一种被投入深水区的情况。

这可能导致其他职能领导在首次领导数据科学团队时面临挑战。这一教训来自麦肯锡的“[为现代时代建立研发战略](https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights/building-an-r-and-d-strategy-for-modern-times#)”，也适用于我们的领域：

> *组织倾向于青睐那些有近期回报的“安全”项目——比如那些源于客户需求的项目——这些项目在许多情况下只是维持现有市场份额。例如，一家消费品公司将研发预算划分给其业务单位，业务单位的领导者则用这些资金来达成短期目标，而不是公司的长期差异化和增长目标。*

在我们的领域，这通常表现为初级数据科学家被他们的非技术主管要求编写能够回答当天问题的任何 SQL 查询。这通常是有帮助的，但通常不是企业通过招聘精明建模师来驱动的价值。

当你有曾经管理过数据科学（DS）或机器学习（ML）项目的领导时，这个问题会更容易解决。无论职能如何，成功的关键在于拥有能够倾听问题、规划分析和建模方法解决问题，并管理风险和不确定性的人。许多早期职业的数据科学家在这种情况下表现出色。根据我的经验，他们是沟通能力和处理模糊性的天赋者。我有幸不小心聘用了一些这样的人——嗨，志宇！依赖你的能力来筛选这些人才，并为之竞争，可能会带来风险。

这一切似乎都支持将数据科学职能集中化。这是一种方法，也引出了我们下一个重要的问题。

**你的数据科学家是否足够接近业务，以关注正确的问题？** 与直接向业务团队汇报的超本地团队相比，中央数据科学职能组可能会较少接触到你希望解决的业务问题。大型、单一的职能团队通过正式的流程，可能很难获得所需的业务输入，主要是因为许多利益相关者不知道自己要提出什么问题。如果你听过数据科学团队产生“没有人要求的科学项目”的恐怖故事，这通常是一个根本原因。而且，再次提醒，不要刻板印象：这很少是因为数据科学团队有过于学术的思维方式，更常见的是因为两个不同职能不知如何用共同语言交流。

这留给我们什么选项？这也是我经验中嵌入式模型有效的一个原因。在这种模型中，你的数据科学团队可以访问你们经常讨论业务问题的所有论坛。他们负责利用这个机会理解业务团队希望解决的问题，并提出可以增加价值的方法。他们向数据科学领导汇报，后者确保他们的工作方法论是正确的，支持他们获得项目成功所需的资源，并指导和辅导他们的成长。

有时数据科学项目失败是因为方法论不佳；它们常常失败是因为预测特征不够有用。知道这两者之间的区别对于非定量职能的人来说可能非常困难。

当我们完成这一步时，我们需要：

+   清晰地定义数据科学家或团队的工作范围

+   定义的参与模式

正如所有实际决策中一样，到处都有权衡，没有万能的解决方案。完全自主的本地团队将最大限度地关注不同的本地结果。集中式职能将最小化重复，但增加了偏离实际、有影响的结果的风险。

# 退后一步，进行沟通和整体迭代

让我们回顾一下我们迄今为止取得的成就：

1.  定义了一个战略假设，即我们将如何通过数据科学和机器学习增加价值的大赌注。

1.  确定一个目标投资组合，该投资组合与我们组织的风险承受能力相一致，考虑到你的流程和技术限制，并将我们的团队集中于那些无法通过购买解决的问题上。

1.  根据数据访问和它们如何创造价值，筛选我们的使用案例。

1.  可能，开发了支持数据科学家的报告结构项目采购方法，并将他们的才能集中于他们独特的优势。

更直白地说，我们已经列出了找到正确使用案例的标准，并筛选了我们的使用案例机会，以找到第一个正确的集合。

接下来的任务是：

1.  退后一步，整体查看所有内容。作为一个整体来看，这是否合理？

1.  传达这一策略，以及从中衍生出的初步计划。

1.  传达潜在利益相关者如何参与你的职能团队。

1.  迭代：每当导致策略的假设或情况发生变化时，重新审视你的策略，并承诺定期检查情况的变化。

![](img/64ec3d72d1bbc5bce8a418ad238fc293.png)

总结来说，这个过程需要相当大的努力。但是，它带来了巨大的回报。这一策略将明确表达你想要承担的风险、如何管理这些风险，以及它们如何支持你的目标结果（如果成功的话）。目的的明确对齐，以及保持活动与这一目的的一致性，对于一个职能团队来说是非常赋能的。实现这一点，结果将随之而来。

# 参考文献

+   Brenna 等人，[“为现代时代构建研发战略”](https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights/building-an-r-and-d-strategy-for-modern-times#)

+   Corbo 等人，[“像技术原生公司一样扩展 AI：CEO 的角色”](https://www.mckinsey.com/capabilities/quantumblack/our-insights/scaling-ai-like-a-tech-native-the-ceos-role)

+   Kiechel, Walter. [*战略之王：新企业世界的秘密知识史*](https://amzn.to/432dMFX)（附属链接。）

+   Meakin 等人，[“用 AI 获胜是一种心态”](https://www.mckinsey.com/capabilities/quantumblack/our-insights/winning-with-ai-is-a-state-of-mind)

+   Pisano, Gary P. “[制定研发战略](https://www.hbs.edu/ris/Publication%20Files/12-095_fb1bdf97-e0ec-4a82-b7c0-42279dd4d00e.pdf)”

+   Rasiel, Ethan. [*麦肯锡方法*](https://amzn.to/3phxC2h)（附属链接。）

+   Scully 等人，[“机器学习：技术债务的高利息信用卡”](https://research.google/pubs/pub43146/)
