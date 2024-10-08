# GPT 与人类心理学

> 原文：[`towardsdatascience.com/gpt-and-human-psychology-94a21ba6d20e`](https://towardsdatascience.com/gpt-and-human-psychology-94a21ba6d20e)

## 人类思维和推理的类比。

[](https://medium.com/@maartengrootendorst?source=post_page-----94a21ba6d20e--------------------------------)![Maarten Grootendorst](https://medium.com/@maartengrootendorst?source=post_page-----94a21ba6d20e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94a21ba6d20e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----94a21ba6d20e--------------------------------) [Maarten Grootendorst](https://medium.com/@maartengrootendorst?source=post_page-----94a21ba6d20e--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94a21ba6d20e--------------------------------) ·13 分钟阅读·2023 年 6 月 29 日

--

随着生成文本模型（如 ChatGPT、GPT-4 等）的出现，人工智能的状态发生了巨大的变化。

这些**GPT**（生成式预训练变换器）模型似乎降低了那些没有技术背景的人进入人工智能领域的门槛。任何人只需开始向模型提问，就能得到令人惊讶的准确回答。

至少，大多数时候是这样……

当它无法生成正确的输出时，并不意味着它无法做到这一点。我们通常只需改变我们提问的方式，即**提示**，以引导模型得到正确的答案。

这通常被称为**提示工程**。

许多提示工程中的技术试图模仿人类思考的方式。要求模型*“大声思考”*或*“一步步思考”*是让模型模仿我们思维方式的很好的例子。

GPT 模型与人类心理学之间的这些类比很重要，因为它们帮助我们理解如何改进 GPT 模型的输出。这显示了它们可能缺失的能力。

这并不意味着我在倡导任何 GPT 模型作为通用智能，但有趣的是，看看我们如何以及为什么试图让 GPT 模型像人类一样**“思考”**。

你在这里看到的许多类比在下面的视频中也有讨论。Andrej Karpathy 从心理学的角度分享了对大型语言模型的惊人见解，绝对值得观看！

一部优秀的视频，描述了 GPT 的状态，并使用有趣的心理学类比。

作为数据科学家和心理学家，这个话题对我来说非常重要。看到这些模型的行为、我们希望它们如何表现，以及我们如何推动这些模型像我们一样表现，真的很有趣。

有许多主题中 GPT 模型和人类心理学的类比提供了有趣的见解，这将在本文中讨论：

![](img/929894205929dc3715d028f98052f80e.png)

本文将讨论的内容概述。

**免责声明**：在讨论 GPT 模型与人类心理学的类比时，存在一定的风险，即人工智能的人性化。换句话说，就是将这些 GPT 模型人性化。这绝对**不是**我的意图。此帖子不是关于存在风险或一般智能，而仅仅是一个有趣的练习，指出我们与 GPT 模型之间的相似之处。如果有的话，请随意带着一定的怀疑态度看待！

# 提示

提示就是我们对 GPT 模型的要求，例如：“创建一个 10 本书名的列表”。

当我们尝试不同的问题以期提高模型的表现时，我们就应用了**提示工程**。

在心理学中，有许多不同形式的提示个体表现出某些行为，这通常用于[应用行为分析](https://www.researchgate.net/profile/Traci-Sitzmann/publication/41087516_Sometimes_You_Need_a_Reminder_The_Effects_of_Prompting_Self-Regulation_on_Regulatory_Processes_Learning_and_Attrition/links/6065bd8892851c91b194ea94/Sometimes-You-Need-a-Reminder-The-Effects-of-Prompting-Self-Regulation-on-Regulatory-Processes-Learning-and-Attrition.pdf) (ABA)中学习新行为。

![](img/1a71ff8d3399351f75c033ba0422c5e4.png)

GPT 模型和心理学中提示的工作方式有明显的不同。在心理学中，提示是为了学习新的行为，即个体之前无法做到的事。而对于 GPT 模型来说，则是展示以前未见过的行为。

主要的区别在于个体学到的是全新的东西，并且在某种程度上作为个体发生了变化。相比之下，GPT 模型已经能够展示这种行为，只是由于其情况，即提示，没有展示这种行为。即使你成功地从模型中引导出“适当”的行为，模型本身并没有发生变化。

![](img/29138489ded82ec80a2d8fd1e616762a.png)

在 GPT 模型中，提示也明显简单了很多。许多提示技巧都非常直白（例如，*“你是一个科学家。总结一下这篇文章。”*）。

# 模仿行为

GPT 模型是模仿者。它和类似的模型在大量文本数据上进行训练，并尽可能地复制这些数据。

![](img/bd2b22ccc5cca8ae8f6b2a741d2cc1c5.png)

这意味着，当你问模型一个问题时，它会尝试生成一系列与训练中看到的内容最匹配的单词。随着训练数据的增加，这些单词序列变得越来越连贯。

![](img/3b7cd57db52de629400359f56f7f2aa7.png)

然而，这样的模型没有真正理解其模仿行为的固有能力。正如本文中的许多内容一样，GPT 模型是否真正具备推理能力无疑是一个讨论的话题，并且常常引发[热烈的讨论](https://twitter.com/Grady_Booch/status/1673797840605433856)。

尽管我们具备模仿行为的固有能力，但这要复杂得多，并且有基于[社会构建](http://ndl.ethernet.edu.et/bitstream/123456789/17733/1/18.pdf.pdf#page=70)和[生物学](https://www.sciencedirect.com/science/article/pii/S0960982210002332)的基础。我们往往在某种程度上理解模仿的行为，并且可以很容易地概括它。

# 身份

我们对自己有预设的观念，了解我们的经历如何塑造了我们，以及我们对世界的看法。我们有**身份**。

GPT 模型没有身份。它对我们生活的世界有很多知识，并且知道我们可能偏好的答案，但它没有“自我”意识。

![](img/caddbd7b77e5ec18fbfc9b681601f0b2.png)

它不一定像我们一样被引导向某些观点。从身份的角度来看，它是一个[**白板**](https://en.wikipedia.org/wiki/Tabula_rasa)。这意味着，由于 GPT 模型对世界有很多知识，它具备模仿你要求的身份的能力。

但一如既往，这只是模仿行为。

它确实有一个主要的优势。我们可以要求模型扮演科学家、作家、编辑等角色，它会尽力配合。通过使其模仿特定身份，它的输出将更适应任务。

![](img/c595f18483ba15eb217cea33288537de.png)

# 能力

这是一个有趣的主题。评估大语言模型的测试有很多来源，如[Hugging Face 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)或使用[Elo 评分](https://lmsys.org/blog/2023-05-25-leaderboard/)来挑战大语言模型。

这些是评估这些模型能力的重要测试。然而，我认为某个模型的优点，你可能不一定同意。

![](img/5243fcd374aa42d1e5b8a90ff05d9623.png)

这与模型本身有关。即使我们告诉它这些测试的得分，它仍然不知道自己的优点和**相对**缺点在哪里。例如，GPT-4 通过了我们通常认为是一个大优点的[律师资格考试](https://www.iit.edu/news/gpt-4-passes-bar-exam)。然而，该模型可能并未意识到，仅仅通过律师资格考试并不是它在充满经验丰富的律师的房间里所拥有的优点。

换句话说，一个人的能力被认为是优点还是缺点很大程度上依赖于情况的背景。这同样适用于我们自己的能力。我可能认为自己在大型语言模型方面很熟练，但如果你把我放在 Andrew Ng、Sebastian Raschka 等人周围，我对大型语言模型的知识突然不再是之前的强项。

这很重要，因为模型并不**本能**地知道什么是优点或缺点，**所以你需要告诉它**。

![](img/6756c8a666c6d0a16d0508443a047107.png)

例如，如果你觉得模型在解决数学方程时表现不佳，你可以告诉它不要自己进行任何计算，而是使用[Wolfram 插件](https://www.wolfram.com/wolfram-plugin-chatgpt/)。

相比之下，尽管我们声称对自己的优点和缺点有一定的认识，但这些往往是主观的，并且容易受到严重偏见的影响。

# 工具

如前所述，GPT 模型不知道它在特定情况下擅长或不擅长什么。你可以通过在提示中添加对情况的解释来帮助它理解情况。通过描述情况，模型会更倾向于生成更准确的回答。

这并不总是能使它在各种任务中都具备能力。就像人类一样，解释情况有帮助，但并不能克服所有的弱点。

![](img/7ec23d327dc1b836617fcd4782fcee8d.png)

相反，当我们面对当前无法完成的事情时，我们通常依靠**工具**来克服这些困难。我们在做复杂的方程时使用计算器，或者用汽车进行更快的交通。

这种对外部工具的依赖不是 GPT 模型自动进行的。你需要告诉模型在你确信它无法完成某个任务时使用特定的外部工具。

![](img/60801c7cd87bdc6cf5c4407f06f31e34.png)

这里重要的是，我们每天依赖大量的工具，比如手机、钥匙、眼镜等。给 GPT 模型相同的能力可以极大地帮助它的表现。这些外部工具类似于 OpenAI 提供的[插件](https://openai.com/blog/chatgpt-plugins)。

一个主要的缺点是这些模型不会自动使用工具。只有在你告诉模型这是一个可能性时，它才会访问插件。

# 内部对话

我们通常有一个内心的声音，在解决困难问题时与之对话。“如果我这样做，那么结果会是这样，但如果我那样做，可能会给我更好的解决方案”。

GPT 模型不会自动表现出这种行为。当你问它一个问题时，它只是生成一系列最符合逻辑的词汇。确实，它会计算这些词汇，但它不会利用这些词汇来创建内部对话。

![](img/38255518f654e653abaf59bb454e5956.png)

事实证明，通过让模型“思考大声”即说“让我们一步步来思考”，往往可以显著提高它给出的答案。这被称为 [链式思维](https://arxiv.org/pdf/2201.11903.pdf?trk=public_post_comment-text)，试图模拟人类推理者的思维过程。这不一定意味着模型在“推理”，但看到这如何改善其表现还是很有趣的。

作为一个小小的附加好处，模型不会在内部进行这个独白，因此跟随模型的思考过程可以提供对其行为的惊人洞察。

![](img/9deb38a8b47760da0645c6e3123a7f99.png)

这种“内心声音”相比于我们实际的运作方式要简单得多。我们在与自己进行“对话”时更加动态，这些“对话”方式可以是象征性的、运动性的，甚至是情感性的。例如，许多运动员会在脑海中想象自己在擅长的运动中表现的情景，以此来训练。这被称为 [**心理意象**](https://cnbc.cmu.edu/~tai/readings/nature/kosslyn_imagery.pdf)**。**

这些对话允许我们进行头脑风暴。我们用这个来提出新想法、解决问题以及理解问题出现的背景。相比之下，GPT 模型则必须通过非常具体的指令来明确地进行头脑风暴。

我们可以进一步将这与我们的 [系统 1 和系统 2](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=94a9488e9a647841ff237d1d44803ba23c40ee5e) 思维过程相关联。**系统 1** 思维是一个自动的、直观的、几乎瞬时的过程。我们在这里几乎没有控制权。相反，**系统 2** 是一个有意识的、缓慢的、逻辑的、费力的过程。

通过赋予 GPT 模型自我反思的能力，我们实际上是在尝试模仿这种系统 2 的思维方式。模型需要更多时间来生成答案，并仔细审查，而不是快速生成回应。

粗略地说，你可以认为在没有任何提示工程的情况下，我们启用了它的系统 1 思维过程，而在没有具体指令和类似链式思维的过程的情况下，我们启用了它的系统 2 思维方式。

如果你想了解更多关于我们系统 1 和系统 2 思维的内容，有一本很棒的书叫做 [《思考，快与慢》](https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow) 非常值得阅读！

# 记忆

Andrej Karpathy 在他在文章开头提到的视频中，对人类记忆能力与 GPT 模型的记忆能力进行了很好的比较。

我们的记忆非常复杂，包括长期记忆、工作记忆、短期记忆、感觉记忆等等。

![](img/495deb1447672a27bb9b440ff9633357.png)

我们可以非常粗略地将 GPT 模型的记忆视为四个组件，并将其与我们自己的记忆系统进行比较：

+   长期记忆

+   工作记忆

+   感觉记忆

+   外部记忆

GPT 模型的**长期记忆**可以被视为它在训练过程中学到的内容。这些信息在模型中以一定程度上表示，它可以在任何时候完美地再现。这种长期记忆会伴随模型的整个存在。相比之下，我们的长期记忆可能会随着时间的推移而衰退，通常被称为衰退理论。

> GPT 模型的长期记忆是完美的，不会随时间衰退。

GPT 模型的**工作记忆**是你给它的提示中所有适合的内容。模型可以完美地使用所有这些信息来进行计算并给出响应。这与我们的工作记忆是一个很好的类比，因为它是一种具有有限容量的记忆，用于暂时保存信息。例如，GPT 模型在给出响应后会“忘记”它的提示。之所以似乎记住对话，是因为除了提示外，对话历史也被添加到提示中。

![](img/137be71d91ee8e6c2289146814a41e41.png)

> GPT 模型在处理新信息时是健忘的。

**感觉记忆**与我们如何持有来自感官的信息有关，如视觉、听觉和触觉信息。我们使用这些信息并将其传递到我们的短期或工作记忆中进行处理。这类似于多模态 GPT 模型，这些模型处理文本、图像甚至声音。

然而，更合适的说法可能是 GPT 模型具有多模态的工作记忆和长期记忆，而非感觉记忆。这些模型将多模态数据与其不同形式的“记忆”紧密结合。因此，正如我们之前所见，它似乎更像是**模仿**感觉记忆。

> GPT 模型通过多模态训练程序模仿感觉记忆。

最后，当你给 GPT 模型**外部记忆**时，它会变得更强。这指的是一个信息数据库，它可以随时访问，比如几本关于物理的书。相比之下，我们的外部记忆使用来自环境的线索来帮助我们记住某些想法和感觉。从某种程度上说，它是访问外部信息与记住内部信息的区别。

**注意：** 我没有提到短期记忆。关于短期记忆和工作记忆之间是否真的相同，有很多讨论。一个常提到的区别是，工作记忆不仅仅是短期储存信息，还具有操控信息的能力。此外，它与 GPT 模型有更好的类比，所以这里我们稍微挑选一下。

# 自主性

正如我们在本文中看到的，如果我们希望 GPT 模型做某事，**我们应该告诉它**。

这点很重要，因为它涉及自主感。默认情况下，我们有一定程度的自主性。如果我决定去拿一杯饮料，我可以。

![](img/f777ff6330aacc674a08abb39d2826e5.png)

对于 GPT 模型而言，这有所不同，因为它默认没有自主性。它无法在没有必要的工具和环境的情况下独立操作。

我们可以通过让 GPT 模型创建多个任务以执行，从而赋予它自主性，以实现某个最终目标。对于每个任务，它会写下完成步骤，进行反思，如果有工具，它会执行这些步骤。

> [AutoGPT](https://autogpt.net/)是赋予 GPT 模型自主性的一个很好的例子。

因此，模型的能力在很大程度上依赖于其环境，甚至可能比我们的环境对我们的影响还要大。这是相当有影响力的，考虑到环境对我们的影响。

这也意味着，尽管 GPT 模型可以展示令人印象深刻的复杂自主行为，但它是固定的。它不能决定使用我们从未告诉它存在的工具。对于我们来说，我们对新工具和未知工具的适应性更强。

# 幻觉

GPT 模型的一个常见问题是它们能够自信地说出一些根本不真实也不受其训练数据支持的内容。

例如，当你要求 GPT 模型生成事实信息，如 2019 年苹果公司的收入时，它可能生成完全错误的信息。

这被称为[**幻觉**](https://arxiv.org/pdf/2202.03629.pdf)。

这个术语源自人类心理学中的**幻觉**，即我们相信我们看到的东西是真实的，而实际上并非如此。这里的主要区别在于，人类的幻觉基于感知，而模型则“幻觉”出错误的事实。

![](img/4ae754138139f02584f22a4dbfab3148.png)

将其与[**虚假记忆**](https://blogs.brown.edu/recoveredmemory/files/2015/05/Loftus_Pickrell_PA_95.pdf)进行比较可能更为恰当。人类记忆与实际发生情况不同的倾向。这类似于 GPT 模型试图重现实际上从未发生过的事情。

![](img/6e14407eee4c4f117cfc854bb92a2161.png)

有趣的是，我们更容易通过暗示、预设、框架等生成虚假记忆。这似乎更接近于 GPT 模型如何“幻觉”，因为它收到的提示具有很大的影响力。

我们的记忆也可能受到来自他人的提示/短语的影响。例如，通过问一个人“这辆车是什么红色的？”我们实际上是在隐含地提供一个所谓的“事实”，即这辆车是红色的，即使它实际上并不是。这可能会产生虚假的记忆，这被称为[**预设**](https://plato.stanford.edu/entries/presupposition/)。

# 谢谢阅读！

如果你和我一样，对 AI、数据科学或心理学充满热情，请随时在[**LinkedIn**](https://www.linkedin.com/in/mgrootendorst/)上添加我，在[**Twitter**](https://twitter.com/MaartenGr)上关注我，或订阅我的[**Newsletter**](http://maartengrootendorst.substack.com/)。你还可以在我的[**个人网站**](https://maartengrootendorst.com/)上找到一些我的内容。

*所有未注明来源的图片均由作者创作*
