# 探索什么让AI伦理工具包运转起来

> 原文：[https://towardsdatascience.com/exploring-what-makes-an-ai-ethics-toolkit-tick-d3a1404e8e2e?source=collection_archive---------6-----------------------#2023-09-22](https://towardsdatascience.com/exploring-what-makes-an-ai-ethics-toolkit-tick-d3a1404e8e2e?source=collection_archive---------6-----------------------#2023-09-22)

## *AI伦理工具包随处可见，但我们真的理解它们吗？*

[](https://medium.com/@malaksadekIC?source=post_page-----d3a1404e8e2e--------------------------------)[![Malak Sadek](../Images/e787445ba9d880bd50a6b96c0a06dc33.png)](https://medium.com/@malaksadekIC?source=post_page-----d3a1404e8e2e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d3a1404e8e2e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d3a1404e8e2e--------------------------------) [Malak Sadek](https://medium.com/@malaksadekIC?source=post_page-----d3a1404e8e2e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb25d31cb7e6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-what-makes-an-ai-ethics-toolkit-tick-d3a1404e8e2e&user=Malak+Sadek&userId=b25d31cb7e6b&source=post_page-b25d31cb7e6b----d3a1404e8e2e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d3a1404e8e2e--------------------------------) ·8分钟阅读·2023年9月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd3a1404e8e2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-what-makes-an-ai-ethics-toolkit-tick-d3a1404e8e2e&user=Malak+Sadek&userId=b25d31cb7e6b&source=-----d3a1404e8e2e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd3a1404e8e2e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-what-makes-an-ai-ethics-toolkit-tick-d3a1404e8e2e&source=-----d3a1404e8e2e---------------------bookmark_footer-----------)![](../Images/13404b719ff8c5436ad10af50ea9acdf.png)

图片由[Todd Quackenbush](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) — 是时候拆解AI伦理工具包了

# 介绍

随着AI系统在具有重要影响的应用中的使用持续增加，专家们呼吁在设计这些系统时采取更多参与性和价值意识的做法。增加利益相关者参与可以为AI系统设计带来许多好处，包括使其更加包容、对抗现有偏见和提供问责制。对此，AI伦理领域近年来产生了大量工具包。这些工具包来自不同的来源，如大学、公司和监管机构，采用了几种不同的技术和框架，并针对的数据科学家、AI工程师、设计师和公众等不同受众（Wong等人，2023）。

许多AI伦理工具包仅关注与AI创建相关的技术方面，主要面向技术从业者（例如，[FAIR自我评估工具](https://satifyd.dans.knaw.nl/)）。然而，也有一些工具包主张并专注于利益相关者的参与，特别是AI创建团队之外的利益相关者，如最终用户和领域专家（例如，[AI与设计工具包](https://aixdesign.co/projects/toolkit)）。那些关注利益相关者参与的工具包通常通过提供画布或卡片组等资源，使利益相关者，尤其是非技术背景的人，能够参与设计活动。这种参与可以导致从头脑风暴不同决策的后果到与AI用户建立同理心的各种结果。

尽管存在大量工具包，但尚未有人真正尝试理解它们的工作原理或其潜在策略。这使得尽管这些工具包被广泛使用，但是否对工具包用户及其使用工具包产生的结果有积极影响仍不明确。因此，我想了解一个工具包的具体运作方式；结果证明这是一次非常有教育意义的经历。

![](../Images/7c710f455221529a41c12bf18cfa6090.png)

由Nadia Piet创建的‘AI与设计’工具包的一部分，来自[https://aixdesign.co/shop](https://aixdesign.co/shop)。

# 我做了什么

我举办了一个设计研讨会，9位参与者以不同的角色与人工智能合作。研讨会包括一个创意活动，旨在头脑风暴针对一个虚构的对话式人工智能的应用场景和设计特性，以帮助人们自我管理糖尿病。它包含了一个由个人设计师而非公司或大学制作的[AI伦理工具包](https://www.ethicsfordesigners.com/tools-1/moral-agent)。我故意选择了这个工具包，以深入探讨一个不依赖于大量资金的小型工具包的基本运作机制。

这个研讨会在Miro上进行，持续了两个小时。工具包的工作方式是由一副卡片组成。这些卡片每张都有一个值（例如隐私、自主、安全等）和一些How Might We?问题，这些问题作为提示，激发出不同的想法，以便在生产的技术中尊重给定的价值。我在Miro板上布置了这些卡片，并留出了便签供人们进行头脑风暴，然后我给每个人分配了两个需要单独关注的价值。

一个小时后，我们大家聚集在一起，我翻转了卡片，这样人们就无法再看到卡片的值，只能看到人们写在便签上的想法。我让每个人展示他们写下的想法，其他人则需要猜测他们希望尊重的价值。

![](../Images/dad810ea93de8966692c6298c91f3c05.png)

来自研讨会Miro板的截图，显示了“诚实”卡片以及参与者围绕它进行头脑风暴的想法。

# 我学到了什么

## 将价值观作为跳板来建立同理心和更广泛的考虑

该工具包旨在使用他们提供的价值列表作为构思和头脑风暴的跳板。3位参与者提到这是一个非常有用的方法：

*“看到不同的价值观如何适用于开发对话代理的过程非常有趣。”*

*“从单一价值观的角度思考设计。”*

*“查看价值卡片并围绕这些卡片产生想法。”*

+   相较于专注于技术可行性，更加关注用户的价值观和不同想法的重要性。参与者似乎喜欢/更愿意这种方法，因为它提供了一个与关注技术细节和忽视如安全和公平等价值观的方式不同的变革。一位参与者表示，他们认为这些价值观在考虑系统及其建设和技术背景时可能不会出现，或在考虑什么是最简单或最快的做法时。他们说，尽管人们在设计时应考虑这些价值观，但现实中并非总是如此，因为其他优先事项会遮盖这些价值观。**总之，** **突出价值观帮助参与者进行头脑风暴并考虑通常被忽视的非技术方面。**

+   这个练习让参与者希望了解更多关于糖尿病患者生活及其经历的信息，以理解如何以最定制和具体的方式支持他们，并提出实际相关的解决方案，而不是基于假设。这种欲望的触发值是‘宁静’（导致希望了解造成压力和焦虑的情况）和‘安全’（导致希望了解他们面临的生命威胁情况）。**总之，关注价值观增强了参与者对目标用户的（i）同理心和（ii）了解如何最好地支持他们的好奇心/愿望。**

![](../Images/7fe3ccee1fbbec41902af12949e6f368.png)

照片由[Miltiadis Fragkidis](https://unsplash.com/@_miltiadis_?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) — 与利益相关者的价值观合作可以促进更深层次的同理心和理解

## 使用游戏化来增加参与度并改善结果

参与者在猜测值时非常享受游戏化的元素：

*“我喜欢猜测创意相关的值——这让我比起只是阅读/讨论这些创意时更能投入其中。”*

*“我觉得猜测值的部分最有用，它让我真正理解了不同的价值观以及它们之间的相互关系。”*

+   在猜测值时，参与者觉得几个创意可能对应不同的值，不同值下的创意有时指的是相同的概念或建议。讨论也突出了几个值之间过于相似或有大量重叠，导致很难区分它们。— 很多值被认为过于相似或重叠，参与者难以猜测或区分它们。不同值之间有很多关联，有些值可以导致或促进其他值，或体现类似的现象/情感。在猜测过程中，“公平”被误认为“包容性”；“社区”、“自由”和“自主”无法区分；“掌握”被混淆为“勇气”和“好奇心”；“包容性”被误认为“无障碍”和“尊重”。**总之，游戏化使参与者能够真正理解创意和价值观之间的相互关系。**

![](../Images/1747a2b765c2c6909c7b8a523112394b.png)

照片由[Erik Mclean](https://unsplash.com/@introspectivedsgn?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) — 游戏化具有许多好处。在AI协作设计的背景下，它可以帮助参与者掌握相互关系，并增加参与感。

## 未来改进和想法

+   三位参与者指出，他们希望能够获得更多的背景信息和提示，以便更好地与对话式人工智能的目标利益相关者产生共鸣。对于诸如“安全”和“宁静”这样的具体值，参与者希望了解更多关于用户的体验，以便更好地为他们提供针对性的解决方案，特别是解决他们的需求。一位参与者觉得最初的提示缺乏来自他们设计对象（糖尿病患者）的反馈，因此难以为他们真正头脑风暴出创意。他们希望能获得更多关于用户场景的定制/具体信息，包括他们生活中的片段描述或实际的背景信息，以便能更好地定制生成的创意。

+   一项建议是进行这样的工作坊，首先专注于抽象的高层次内容，不带太多事前信息，然后再去收集用户信息以提供背景。因为如果你先收集用户信息，你可能会错过一些观点或价值观，并过于关注你所收集的内容。之后，你可以将这些信息整合在一起，再次进行头脑风暴，探讨场景和技术方面，同时受益于两种知识来源。

+   两位参与者指出，如果他们没有带有定义的值列表，他们可能会在想出描述这些值的确切词语时遇到困难，可能会想到类似的词汇，但不是确切的词语。如果没有提供值的描述，他们也会难以理解这些值的含义。

# 这如何能帮助你

对AI伦理工具包的有效性的实验让我对使用游戏化和基于价值的练习的力量有了更深刻的理解。以下是我将这些见解提炼成的六点，这些可以帮助你与利益相关者一起使用AI伦理工具包进行设计工作坊，以便进行头脑风暴或构思AI设计：

+   合作思考AI伦理的主要点之一是让AI更具人性化。因此，**包括大量的背景（最好是第一手）信息关于你的目标用户和你为谁设计**是非常重要的。记住，许多技术从业者可能不习惯于建立同理心的练习和构思，所以通过真正明确他们试图帮助的人来帮助他们实现这一点。

+   考虑你的工作坊是否需要**许多活动从高层次的相关性或重要性逐步深入到细节**。混合这两者可能会让参与者感到困惑，尤其是当你要求他们对可能无法在同一层面进行比较的观点进行排名或评分时。

+   如果你正在处理值，请**确保你清楚定义每个值的含义**。这很重要，因为不同的值可能根据询问的人不同而含义不同，如果没有清晰定义，它们之间可能会有很多冲突和重叠。

+   **将值融入你的AI设计活动**可以帮助参与者集思广益，并考虑**常被忽视的非技术性方面**，帮助他们**与目标用户建立同理心**，并增加他们的**好奇心和更好地支持这些用户的愿望**。

+   **结合游戏化技术**可以帮助**提高参与者的投入感和享受感**，在AI伦理工作坊中，这也可以**帮助他们更深入地把握观点之间的联系**。

# 我的角色

我的博士项目旨在利用设计领域的工具和技术，使 AI 系统的设计变得更具可访问性和包容性。我正在致力于创建一个参与式过程及其支持工具包，以系统性地在整个 AI 生命周期中涉及人们——重点关注价值敏感性。

你可以在[帝国理工学院官网](https://www.imperial.ac.uk/design-engineering/study/phd/malak/)查看我项目的官方页面。你也可以查看我撰写的另一篇文章，解释了我[博士项目的详细信息](https://medium.com/@malaksadekIC/introducing-my-phd-project-to-make-ai-design-more-inclusive-80d0edf70378)。

我创建了这个 Medium 账号，以便在我进行博士项目的过程中发布有趣的发现，希望能够以一种让任何人都能理解的方式传播关于 AI 系统的新闻和信息。

# 参考文献

Richmond Y. Wong、Michael A. Madaio 和 Nick Merrill. 2023\. 《像工具包一样看待：工具包如何构想 AI 伦理的工作》。Proc. ACM Hum.-Comput. Interact. 7, CSCW1, Article 145 (2023年4月)，共27页。 [https://doi.org/10.1145/3579621](https://doi.org/10.1145/3579621)
