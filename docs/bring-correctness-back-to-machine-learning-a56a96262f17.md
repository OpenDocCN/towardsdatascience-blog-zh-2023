# 将正确性带回机器学习

> 原文：[`towardsdatascience.com/bring-correctness-back-to-machine-learning-a56a96262f17`](https://towardsdatascience.com/bring-correctness-back-to-machine-learning-a56a96262f17)

## 我们是否在错误的假设上构建我们的领域？

[](https://medium.com/@mattiadigangi?source=post_page-----a56a96262f17--------------------------------)![Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----a56a96262f17--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a56a96262f17--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a56a96262f17--------------------------------) [Mattia Di Gangi](https://medium.com/@mattiadigangi?source=post_page-----a56a96262f17--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a56a96262f17--------------------------------) ·9 分钟阅读·2023 年 10 月 13 日

--

![](img/f4abe69e4718d076a4a8eb6168a88bd6.png)

由 [Andrea De Santis](https://unsplash.com/@santesson89?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍：什么是正确性？

研究论文仍然是传达机器学习领域新发现的主要方式。然而，论文的结果不能被复现的情况相当频繁，而原因却不清楚。

在这里，我想提出我对研究论文作为沟通工具的利弊的看法。我将介绍我对科学及其在发展集体人类知识中的作用的看法。

最近的一篇论文发现了研究出版过程中的漏洞。我觉得这篇论文非常有说服力，我将向你展示其主要主张，以提高对**代码正确性**在传播机器学习知识中作用的认识。

这个问题不仅限于机器学习，但在这个领域，许多研究人员缺乏强大的工程技能——并且通常试图逃避工程工作——这导致了可靠、正确、可用、有效的软件的问题。

你是否从事机器学习工作，并阅读研究论文寻找新想法？那么，这篇文章将帮助你以原则性的方法对你阅读的内容进行更批判的分析。

你是研究人员和研究论文的作者吗？希望你会对这个话题感兴趣，阅读引用的论文，并参与讨论。

现在，足够的介绍，让我们深入讨论吧！

# 科学即知识

根据所有可以衡量的指标，科学作为职业比以往任何时候都[更受欢迎](https://www.theatlantic.com/science/archive/2018/11/diminishing-returns-science/575665/)。这些指标包括科学家（和博士）的数量、可用资金、资助申请等。所有科学家中增长的一部分是机器学习领域的研究人员，理论的和应用的都有。

科学家的工作是什么？就是在某一研究领域发现新知识，从而扩展人类已知的知识范围，通过扩展或推翻现有知识。

在许多情况下，科学家在现有知识的基础上进一步推进。有时，先前的科学证据可能被证明是错误的或具有误导性的。例如，选择的样本可能不能代表整个群体，因此结果可能不具有普遍性。

另一个原因可能是研究是在特定条件下进行的，而其结果被推广到不同的条件。例如，在机器学习中，一个方法可能在训练集人为缩小的情况下表现优于当前最先进的方法…但在正常的数据条件下，它的表现不如基线。

传播科学发现的主要工具，虽然被[一些](https://www.theatlantic.com/science/archive/2018/04/the-scientific-paper-is-obsolete/556676/)人认为过时，但毫无疑问还是经过同行评审的研究论文。通过研究论文，科学家们以结构化和有组织的方式描述他们的发现。他们描述发现的领域、他们解决的问题、知识领域中的漏洞、他们的假设以及旨在为假设提供证据或证伪的实验。

根据描述，其他科学家决定论文是否值得信赖以及发现是否值得发表，这一过程称为同行评审。需要注意的是，这是一个棘手且不完美的系统，同行评审常常未能发表值得的研究，或者相反，允许发表质量较差的论文。没有任何系统是完美的，这也是游戏的一部分。这个游戏是科学传播的主要过程，通常决定了研究资金的未来和研究人员的职业生涯。

现在，可以提出一些关于过程基本原理的问题：“我们如何相信论文中的声明？”，“一篇论文是否足以将一项知识视为已获得？”，“如果一篇新论文与之前发表的论文结果相矛盾怎么办？”

为了回答这些问题，我们需要介绍一些概念。

# 可重复性、可靠性、正确性

机器学习会议越来越重视可重复性的问题，他们这样做完全正确。其他科学领域，特别是心理学和医学，在经历了著名的“[复制危机](https://en.wikipedia.org/wiki/Replication_crisis)”之后，遭遇了严重的可信度问题：论文结果无法被不同的独立研究者复制，这对该领域的整个工作产生了大量质疑。然而，这个问题远远超出了这两个提到的领域。

![](img/fd4fe1c8821c677ee944d85f1dbf1e02.png)

图片来源：[Julia Koblitz](https://unsplash.com/@jkoblitz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

虽然极为重要，但可重复性只是产生新科学知识过程的一部分。机器学习会议通常要求审稿人也评估实验的“严谨性”。

让我们尝试用更明确的术语来定义它们：

**可重复性**是指通过遵循论文中解释的协议，另一组能够重复一项科学研究的可能性。这意味着论文中描述了所有相关细节，也许所使用的软件已发布（我们在这里讨论的是机器学习），并且可以获取相同的训练数据。

如果独立研究者遵循所描述的方法论，但他们的结果与论文中描述的结果有显著差异，则该研究是不可重复的。

**严谨性**是对协议正确性的判断。实验是否与要证明的假设一致？结果是否确实显示了作者所声称的内容？实验是否存在偏差或不完整性？

实际上，严谨性分数反映了从科学角度看研究的技术正确性。

这两个方面在机器学习会议中广泛讨论，但还有第三个方面虽然讨论较少但仍然重要。

**代码正确性**是一个是/否问题：代码是否实际实现了论文中描述的方法论？

代码正确性通常被认为是理所当然的，因此不会被检查或强制执行。

# 建立正确的知识

可重复性对建立知识至关重要。让我再重复一遍：无法被独立复制的结果不代表科学知识。它们不一定是由于欺诈（尽管有时确实如此），但可能是因为一些被作者认为是“次要”的细节没有描述，而这些细节在所提议的方法中或比提议的方法更为重要，以获得声称的结果。

另一方面，正确性是建立正确知识的基础。如果我们阅读一篇论文并发现有趣的结果，然后尝试复制这些结果并获得完全相同的结果，我们可以对自己新获得的领域知识感到满意。

如果参考实现隐藏了一些“bug”，并且在某些基本方面没有遵循所描述的思想，会怎么样呢？

![](img/7c3d1d4670393089d3812a727f31e779.png)

图片由 [Dmitry Bukhantsov](https://unsplash.com/@bdv91?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这种情况下，我们是在错误的事实基础上建立知识。积累许多错误的事实，我们就不知道什么是重要的，什么是不重要的。这对我们这些想要在某个领域成为“专家”的人来说，确实很可怕，不是吗？

这个问题在论文 [When Good and Reproducible Results are a Giant with Feet of Clay: The Importance of Software Quality in NLP](https://arxiv.org/abs/2303.16166) 中由 Papi 和同事们 [1] 讨论，其中以一些广泛使用的 Conformer 实现 [2] 为例。这些实现通常在常见的开源框架中加速了许多组的研究，这些组不需要自己实现 Conformer 和训练方法。问题是，所有被检验的实现中都包含三类 bugs，当推断批量大小大于一时，这些 bugs 会影响结果。注意，在正常情况下，推断批量大小应影响推断过程中的资源使用，从而影响推断速度，但不应影响输出。

关于这些 bug 的详细信息，我强烈推荐阅读这篇论文。在这里，我只是想强调，差异通常足够小，以至于不会注意到实现中存在问题，这也可能是它们被忽视了这么久的原因。然而，在一种情况下，当批量大小非常大时，降级是巨大的。

有些人可能会疑惑为什么我们要关心 BLEU 或 WER 的小数点后几位，但我认为这种观点忽视了问题的关键。本文仅以**流行的** Conformer 实现为例，这些实现被全球数百或数千名研究人员和实践者使用。然而，它发现了所有这些实现中的 bug，其中一个在保持高效实现的同时并不容易解决。

记住，“bug”只是软件行业对软件错误的友好称呼。一个可爱的“bug”总是软件需求与实际行为之间的差异。

想象一下，每周提出的数百个深度学习网络的所有私人实现会发生什么。在许多情况下，这些网络由小组或单个开发者开发，没有人审查他们的代码。实现与论文中的描述是否匹配，留给作者自己判断，也不作为科学同行评审讨论的重点。

那么问题是：在这些假设下，我们如何可靠地信任我们阅读的论文中的声明？

不幸的是，这个问题没有明确的答案。[1]的作者提出了一些受软件开发如何解决正确性问题启发的解决方案。我认为这朝着正确的方向发展，但不幸的是，太多人过于看重感知到的额外工作（而不是错误软件的成本），我不认为这种方法会很快被广泛采纳。

# 结论

良好的科学话题无疑是一个艰难的领域。研究工作涉及许多不同的方面，研究人员最不希望的就是为了同样数量的出版物而增加更多的工作。

然而，当我们依赖不正确的软件时，我们正在构建错误的知识，并可能基于这些错误的知识走上错误的方向。试想一下，如果你使用一个你认为可靠的软件工具工作了一年或两年，然后有人发现了一个 bug，当它被修复时，你的新模型的性能突然下降。我认为没有人愿意陷入这种情况。我们从最佳工程实践中知道，错误应该在早期发现，以减少其成本。

像论文中提出的那种机器学习测试工具可以帮助研究人员以简单的方式生成更正确的软件，我真的很期待它的发布。如果是开源的，整个社区都可以贡献力量，整体提升编码标准。

你怎么看？你有什么解决方案来确保 ML 研究的正确性吗？在评论中告诉我！

*如果你读到这里，非常感谢你的时间！我知道你的时间有限且宝贵，但你仍然决定花时间阅读我的想法，非常感谢！*

# 更多我的文章

[## 阅读和撰写 ML 研究论文的技巧](https://towardsdatascience.com/tips-for-reading-and-writing-an-ml-research-paper-a505863055cf?source=post_page-----a56a96262f17--------------------------------) [## Tips for Reading and Writing an ML Research Paper](https://towardsdatascience.com/tips-for-reading-and-writing-an-ml-research-paper-a505863055cf?source=post_page-----a56a96262f17--------------------------------)

### 从数十次同行评审中获得的经验教训

[## 无需多言：自动化开发环境和构建](https://towardsdatascience.com/tips-for-reading-and-writing-an-ml-research-paper-a505863055cf?source=post_page-----a56a96262f17--------------------------------) [## Without Further Ado: Automate Dev Environments and Build](https://towardsdatascience.com/without-further-ado-automate-dev-environments-and-build-f2f9bcaaae1e?source=post_page-----a56a96262f17--------------------------------)

### 通过环境和构建自动化，使您的软件易于使用，从而给您的同事开发者带来快乐。通过…

[## 3 个常见的 bug 来源及如何避免](https://towardsdatascience.com/without-further-ado-automate-dev-environments-and-build-f2f9bcaaae1e?source=post_page-----a56a96262f17--------------------------------) [## 3 Common Bug Sources and How to Avoid Them](https://towardsdatascience.com/3-common-bug-sources-and-how-to-avoid-them-182f9974d2ab?source=post_page-----a56a96262f17--------------------------------)

### 一些编码模式更容易隐藏 bug。编写高质量代码并了解我们的大脑如何工作可以帮助…

[通向数据科学](https://towardsdatascience.com/3-common-bug-sources-and-how-to-avoid-them-182f9974d2ab?source=post_page-----a56a96262f17--------------------------------) [## 语音增强介绍：第一部分 — 概念和任务定义

### 介绍改善降级语音质量的概念、方法和算法…

通向数据科学](https://towardsdatascience.com/introduction-to-speech-enhancement-part-1-df6098b47b91?source=post_page-----a56a96262f17--------------------------------)

# Medium 会员

你喜欢我的写作吗？你是否考虑订阅 Medium 会员以无限访问文章？

如果你通过这个链接订阅，你将通过你的订阅支持我，而对你来说没有额外费用 [`medium.com/@mattiadigangi/membership`](https://medium.com/@mattiadigangi/membership)

# 参考文献

[1] Papi, S 等。“当良好且可重复的结果成为泥足巨人：软件质量在自然语言处理中的重要性” arxiv.org/2303.16166

[2] Gulati, Anmol 等。“[Conformer：用于语音识别的卷积增强变换器](https://arxiv.org/abs/2005.08100)” *Proc. Interspeech 2020*（2020 年）：5036–5040。
