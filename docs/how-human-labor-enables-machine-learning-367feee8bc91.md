# 人类劳动如何促进机器学习

> 原文：[`towardsdatascience.com/how-human-labor-enables-machine-learning-367feee8bc91?source=collection_archive---------2-----------------------#2023-10-31`](https://towardsdatascience.com/how-human-labor-enables-machine-learning-367feee8bc91?source=collection_archive---------2-----------------------#2023-10-31)

## 技术与人类活动之间的大部分分隔是人为的——人们是如何让我们的工作成为可能的？

[](https://medium.com/@s.kirmer?source=post_page-----367feee8bc91--------------------------------)![Stephanie Kirmer](https://medium.com/@s.kirmer?source=post_page-----367feee8bc91--------------------------------)[](https://towardsdatascience.com/?source=post_page-----367feee8bc91--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----367feee8bc91--------------------------------) [Stephanie Kirmer](https://medium.com/@s.kirmer?source=post_page-----367feee8bc91--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa8dc77209ef3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-human-labor-enables-machine-learning-367feee8bc91&user=Stephanie+Kirmer&userId=a8dc77209ef3&source=post_page-a8dc77209ef3----367feee8bc91---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----367feee8bc91--------------------------------) ·8 分钟阅读·2023 年 10 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F367feee8bc91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-human-labor-enables-machine-learning-367feee8bc91&user=Stephanie+Kirmer&userId=a8dc77209ef3&source=-----367feee8bc91---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F367feee8bc91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-human-labor-enables-machine-learning-367feee8bc91&source=-----367feee8bc91---------------------bookmark_footer-----------)![](img/312c447d8f15ac8994a2be525e99e76d.png)

图片由 [Dominik Scythe](https://unsplash.com/@drscythe?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们并没有足够谈论我们依赖多少人工工作来实现机器学习领域的激动人心的进展。事实是，技术和人工活动之间的划分是人为的。所有使模型产生的输入都是人类努力的结果，而所有输出以某种方式存在于影响人们。我今天使用这一专栏来谈论我们在某些特定领域忽视了人们对我们所做工作的重要性——不仅仅是那些编写代码的数据科学家。

> 技术与人工活动之间的划分是人为的，因为所有使模型产生的输入都是人类努力的结果，而所有输出以某种方式存在于影响人们。

# 生成数据

你几乎肯定已经知道这一点——LLMs 需要巨量的文本数据进行训练。我们通常以硬盘上的数百或数千 GB 的数据来考虑这个问题，但这有点抽象。一些报告指出，GPT-4 的训练数据大约有*1 万亿*个单词。每一个单词都是由一个人用他们自己的创造能力写出的。作为参考，《冰与火之歌》系列的第一本书大约有 292,727 个单词。因此，GPT-4 的训练数据大约是*3,416,152 本书的长度*。而这只是文本建模的一个例子——其他类型的模型，例如那些生成或分类多媒体的模型，也使用类似规模的海量数据。

当涉及到这些数据时，有几个方面需要考虑。首先，所有这些数据都是由人生成的，它不会凭空出现在我们的硬盘上。尊重和认可那些创造我们数据的人的工作是重要的，这是伦理问题，因为他们付出了努力，创造了我们正在受益的价值。但我们也有更自私的理由需要了解我们的数据来源。作为数据科学家，我们有责任了解我们向模型提供的材料作为示例，并深入理解它。如果我们忽视数据的来源，我们可能会在面对现实世界时，对我们的模型行为感到不愉快的惊讶。例如，在互联网论坛或社交媒体数据上训练 LLMs 会使这些模型面临复制这些空间最糟糕现象的风险，包括种族主义、仇恨言论等。在稍微不那么极端的例子中，我们知道[模型受其训练数据的影响](https://www.technologyreview.com/2023/08/07/1077324/ai-language-models-are-rife-with-political-biases/)。

> 如果我们忽视数据的来源，我们可能会在面对现实世界时，对我们的模型行为感到不愉快的惊讶。

# 标记数据

数据标注需要人工帮助。但是，标签到底是什么？从根本上说，标注数据意味着使用人类的洞察力为我们在数据中发现的内容分配值或判断。无论数据如何收集或创建，大多数机器学习用例都需要某种形式的标注。

这可能意味着简单地决定一个数据点是好是坏，确定词语是积极的还是消极的，创建衍生值，将记录划分为不同类别，确定哪些标签适用于图像或视频，或者其他无尽的任务。一个常见的例子是识别图像或其他多媒体中的文本，以改善字符识别模型。[如果你使用过验证码](https://www.hcaptcha.com/labeling)，我敢打赌这听起来很熟悉——你已经做过数据标注工作。

理论上，LLM 本身不需要标注，因为我们从这些文本已经由真实人类生成的事实中推断出人类的质量，因此这些文本在本质上必须尽可能类似于人类输出。基本上，因为是人类写的，那么它在定义上就是一个可接受的示例供模型学习和模仿。这就是我们使用语义嵌入的地方——模型学习人类生成文本中的语言模式如何工作，并将这些模式量化为数学表示。但是正如我之前所描述的，我们仍然在选择哪些文本进入模型的过程，我们有责任理解和评估这些文本。

# 教学模型

强化学习使用人工干预来调整任务——意味着我们在模型基本上掌握了返回连贯答案的方式后，稍微调整模型对提示的响应，无论是文本、图像、视频还是其他内容。在一些主要的自动化预训练或基础训练之后，[许多模型由人类进行微调，做出有时微妙的判断，判断模型是否达到了期望。](https://www.youtube.com/watch?v=bZQun8Y4L2A) 这是一项非常艰巨的任务，因为我们实际希望模型做到的细微差别可能非常复杂。这基本上是在大规模上以通过或不通过的方式进行 LLM 的校对。

正如我之前讨论的，许多现代模型寻求生成对人类用户最令人愉悦的内容——那些看起来正确且吸引人的东西。那么，有什么比让人类查看训练的中间阶段结果并决定结果是否符合描述，再告诉模型以便它做出更合适的选择，更好的训练方式呢？这不仅是最有效的方法，也可能是唯一有效的方法。

> 这基本上是在以通过或不通过的方式进行 LLM 的校对。

# 为什么这很重要

好吧，那又怎样呢？仅仅意识到真实的人们付出了大量的努力来实现我们的模型，这就够了吗？拍拍他们的背，表示感谢？不完全是，因为我们需要审视人类的影响对我们生成的结果意味着什么。作为数据科学家，我们需要对我们构建的东西与其所在世界之间的互动保持好奇心。

由于所有这些影响领域，人类的选择塑造了模型的能力和判断。我们将人类的偏见嵌入到模型中，因为人类创造、控制并判断所有相关材料。我们决定哪些文本将提供给模型进行训练，或者模型的哪个具体回应比另一个更差，而模型将这些选择固化为可以重复和再利用的数学表示。

这种偏见元素是不可避免的，但并不一定是坏事。试图创造一个完全摆脱人类影响的东西暗示人类的影响和人类本身是需要避免的问题，这在我看来是不公平的评估。同时，我们也应该现实地认识到人类偏见是我们模型的一部分，并抵制将模型视为超越我们人类缺陷的诱惑。例如，我们如何分配标签，导致我们有意识或无意识地赋予数据意义。无论是原创创意内容、数据标签，还是模型输出的判断，我们都在创建的数据中留下了我们思维过程和历史的痕迹。

> 试图创造一个完全摆脱人类影响的东西暗示人类的影响和人类本身是需要避免的问题，这在我看来是不公平的评估。

此外，在机器学习领域，人类的努力通常被视为服务于“真实”工作的，而不是自身具有意义。那些产生原创作品的人不再被视为独特的创造性个体，而只是被归入服务于模型的“内容生成者”。我们忽视了这些内容存在的本质人性和真实原因，即服务和赋能人类。与之前的观点一样，我们最终贬低了人们而崇拜技术，我认为这是愚蠢的。模型是人们的产物，旨在服务于人们，它们不是独立的目标。如果你构建了一个从未被使用和运行的模型，那还有什么意义呢？

# 数据是可再生资源吗？

另一个有趣的问题是：人类生成的纯净内容的风险成为模型能力的限制因素。也就是说，当我们的社会开始使用 LLM 生成数据，使用 Dall-E 生成图像，并且我们停止激励真实的人在没有这些技术的情况下进行创作时，我们需要训练新版本这些模型的万亿字词和山岳般的图像将被人为生成的内容所污染。那种内容，当然，来源于人类内容，但并不完全一样。我们尚未拥有很好的方法来区分由没有模型的人生成的内容，所以我们将难以判断我们未来模型的训练数据是否存在这种污染，以及污染的程度。

有人认为这其实不是什么大问题，训练模型时使用至少一部分人工内容不会有问题，但也有其他理论认为，当[我们开始以这种方式掠夺人工生成的内容时，训练的基本过程将以称为模型崩溃的形式被根本性改变](https://bdtechtalks.com/2023/06/19/chatgpt-model-collapse/)。在某些方面，这是模型影响了模型所依赖的世界的根本问题的一个例子，因此模型的行为定义上改变了模型本身。数据科学家对此也深有体会。这不仅仅适用于 LLM，任何模型都可能通过影响人们的行为来使自己失业，从而导致由于基础数据关系的变化而性能漂移。

> 你的模型影响了模型所依赖的世界，因此模型的行为定义上改变了模型本身。

即使我们没有在实际的人工数据上进行训练，还有许多学者在考虑我们的人的组成和创造过程是否会因接触到人工生成的内容而发生变化。如果你大量阅读 LLM 生成的文本，无论是在写作时获取模型的建议还是在互联网的一般环境中，[这是否会微妙地改变你的写作方式](https://arxiv.org/abs/2309.05196)？在社区层面上现在还为时尚早，但这是一个严重的问题。

人类影响是机器学习的一个事实——这是一个哲学问题。我们将机器学习视为一种纯科学事业，认为它作用于我们，这也是它对某些人来说似乎令人恐惧的原因之一。但实际上，正在创建的系统是人类干预和创造力的产物。创建和策划数据使得所有其他机器学习的可能性成为可能。在某种程度上，这应该让我们感到安慰，因为我们可以控制我们如何使用机器学习以及如何使用它。机器学习的过程是将数据片段之间的关系计算成数学表示，但数据是由人类产生的，并且在我们的控制之下。机器学习和人工智能不是某种外星的、抽象的力量——**它们只是我们。**

*查看更多我的作品请访问* [*www.stephaniekirmer.com*](http://www.stephaniekirmer.com/)*.*

上述文章和参考资料，方便访问：

+   [`www.youtube.com/watch?v=bZQun8Y4L2A`](https://www.youtube.com/watch?v=bZQun8Y4L2A) GPT 状况，微软 Build 大会 2023，Andrej Karpathy

+   [www.technologyreview.com%2F2023%2F08%2F07%2F1077324%2Fai-language-models-are-rife-with-political-biases%2F](https://www.technologyreview.com/2023/08/07/1077324/ai-language-models-are-rife-with-political-biases/) MIT 技术评论，[Melissa Heikkilä](https://www.technologyreview.com/author/melissa-heikkila/)，2023 年 8 月

+   [`www.hcaptcha.com/labeling`](https://www.hcaptcha.com/labeling)

+   [`files.eric.ed.gov/fulltext/EJ1390465.pdf`](https://files.eric.ed.gov/fulltext/EJ1390465.pdf)

+   [`arxiv.org/abs/2309.05196`](https://arxiv.org/abs/2309.05196)

+   [`bdtechtalks.com/2023/06/19/chatgpt-model-collapse/`](https://bdtechtalks.com/2023/06/19/chatgpt-model-collapse/)
