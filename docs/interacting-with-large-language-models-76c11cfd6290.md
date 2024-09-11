# 与大型语言模型的互动

> 原文：[`towardsdatascience.com/interacting-with-large-language-models-76c11cfd6290?source=collection_archive---------14-----------------------#2023-05-24`](https://towardsdatascience.com/interacting-with-large-language-models-76c11cfd6290?source=collection_archive---------14-----------------------#2023-05-24)

## 丰富提示以引导模型 — 为非专家解释

[](https://medium.com/@doriandrost?source=post_page-----76c11cfd6290--------------------------------)![Dorian Drost](https://medium.com/@doriandrost?source=post_page-----76c11cfd6290--------------------------------)[](https://towardsdatascience.com/?source=post_page-----76c11cfd6290--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----76c11cfd6290--------------------------------) [Dorian Drost](https://medium.com/@doriandrost?source=post_page-----76c11cfd6290--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1d49ea537d1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finteracting-with-large-language-models-76c11cfd6290&user=Dorian+Drost&userId=1d49ea537d1c&source=post_page-1d49ea537d1c----76c11cfd6290---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----76c11cfd6290--------------------------------) · 10 min 阅读 · 2023 年 5 月 24 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F76c11cfd6290&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finteracting-with-large-language-models-76c11cfd6290&user=Dorian+Drost&userId=1d49ea537d1c&source=-----76c11cfd6290---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F76c11cfd6290&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finteracting-with-large-language-models-76c11cfd6290&source=-----76c11cfd6290---------------------bookmark_footer-----------)

像 GPT 这样的语言模型在解决复杂任务如翻译、推理或回答问题方面展现了惊人的能力。然而，为了达到这一点，这些模型必须以正确的方式进行查询；这是一种称为提示的技术。了解如何与语言模型互动对于最大化其效果至关重要，而编写合适的提示则可能是一项巨大的挑战。

在这篇文章中，我想概述不同的提示技术，这些技术使语言模型能够更准确地解决任务。我将首先解释语言模型的基本功能，然后介绍一些最常用的提示技术，即零-shot 和 few-shot 提示。之后，我将解释思维链提示，一种增强语言模型能力的机制，并展示如何将这些与使用外部工具相结合，以解决更复杂的任务。

# 语言模型与提示

![](img/1bd2dcf3272b3f133eea6f460b661a3f.png)

提示是对语言模型的指令。照片由 [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

语言模型的基本原理可以用一句话来解释：语言模型是一个根据上下文预测下一个词的模型。也就是说，给定一个句子如“*猴子吃了*”，语言模型计算不同词的后续概率。在这个例子中，“*香蕉*”的概率可能很高，“*苹果*”也可能相当高，但“*钢琴*”的概率则相对较低。因此，模型可能会将句子完成为“*猴子吃了香蕉*”。

有不同种类的模型以不同的方式估计这些概率，每种都有其独特的优点和缺点。如果你想了解一些重要的技术，可以查看 [这篇概述](https://medium.com/towards-data-science/a-brief-history-of-language-models-d9e4620e025b)。要理解本文，你只需知道语言模型总是根据一系列词预测下一个词。

为了与语言模型交互，你使用提示。提示就是你给模型继续的文本。因此，在上述例子中，“*猴子吃了香蕉*”是提示。提示不仅限于单句，它可以更长，包括多句或段落。由于技术原因，提示长度需要有一个限制，通常在几百或几千个词的范围内。根据你的提示，你可以让模型完成不同的任务，如回答问题、翻译成其他语言等等。让我们探索一些创建有意义提示的方法，这些提示告诉模型你对它的期望。

# Zero-shot 提示

![](img/53abcb53e017487806ff1f06f59a0cc2.png)

我们从简单的开始。照片由 [Jan Kahánek](https://unsplash.com/@honza_kahanek?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

使用大型语言模型的最简单方法可能是零-shot 提示。主要思想是构造一个提示，使得完成提示能够给出所需的答案。一个简单的例子：

> 美国总统是 …

当语言模型通过预测下一个词来完成这个句子时，它可能会创建句子“*美国总统是乔·拜登*”，并通过这样做回答了问题。

然而，这种方法也有其局限性，因为有时很难以这种结构清晰而明确地制定问题。例如，如果我们拿这个句子

> 乔·拜登是 …

我们可能期望模型将句子完成为“*乔·拜登是美国总统*”。然而，这个句子还有许多其他合理的延续方式，比如“*乔·拜登是民主党的一名政治家*”。这是一个真实的陈述，根据我们的提示，无法确定哪一个答案更符合我们的期望。那么，我们如何才能更清晰地制定提示呢？

# 少量示例提示

![](img/d46379676c2396ac401866e877395697.png)

更多的示例可以丰富提示。照片由 [ron dyar](https://unsplash.com/@prolabprints?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在提示中，我们可以给出一些我们期望的示例，这是一种叫做少量示例提示的技术。考虑以下示例：

> 贾斯廷·特鲁多：加拿大
> 
> 瑞希·苏纳克：英国
> 
> 奥拉夫·舒尔茨：德国
> 
> 乔·拜登：

显然，这个提示中编码的模式是政治家与他们所领导国家之间的联系。给定这个提示，我们期望模型继续回答为“*乔·拜登：美利坚合众国*”。在另一个使用相同格式的提示中，我们可能会关注不同的方面：

> 贾斯廷·特鲁多：51
> 
> 瑞希·苏纳克：43
> 
> 奥拉夫·舒尔茨：64
> 
> 乔·拜登：

这里我们讨论政治家的年龄，因此期望的延续是“*乔·拜登：80*”。

如前所述，这叫做少量示例提示，因为你提供了一些示例（shots），告诉模型该做什么。这有助于更详细地描述期望的任务，从而导致语言模型提供更精确的答案。

# 思考链

![](img/d16f316f99f2629b2267e1746e25c75d.png)

这是一个链条，因为这是一个接一个的思考。好吧，我想你应该在没有我解释的情况下明白这一点。照片由 [Karine Avetisyan](https://unsplash.com/@kar111?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

到目前为止，我们的提示仅包括我们希望模型回答的任务。然而，我们还可以使用提示给模型额外的指导，突出重要的方面或限制其回答。例如，为了避免错误答案，我们可以从

> 回答以下问题。如果你没有所需的信息，请回答‘我不知道’。

随后是少量示例。这使得模型承认它不知道答案，而不是编造错误的答案。

提示的另一个有用扩展被称为思维链提示。主要思想是鼓励模型明确地表达它在完成任务时必须执行的中间步骤。我们通过简单地提供包含这些步骤或推理痕迹的少量示例来实现这一点，这被称为思考。正如你可能猜到的，这个想法受到人类在将任务拆分为子任务并对其进行推理时的思维方式的启发。假设你有如下任务：

> 问：莎莉买了 18 个苹果来做苹果派。她烤了 2 个饼，还剩下 4 个苹果。每个饼里有多少个苹果？

你会怎么做才能得出答案？我敢打赌你首先算出她使用了 14 个苹果（18 个买的减去 4 个剩下的），然后把 14 个苹果除以两个饼，得到每个饼 7 个苹果。对你来说这可能听起来很简单，但你是否曾教过孩子解决这样的任务？如果你只是给它任务和答案（7），它不会理解你是如何得出这个答案的。然而，如果你将你所做的步骤表达出来，孩子可以理解并将这些步骤转移到其他任务中，只要它能够进行必要的算术运算。同样，在提示语言模型时，你可以在少量示例中包含思考。对于上述任务，一个示例可能如下所示：

> 问：莎莉买了 18 个苹果来做苹果派。她烤了 2 个饼，还剩下 4 个苹果。每个饼里有多少个苹果？
> 
> 思考：如果她剩下 4 个苹果，她已经使用了 18–4=14 个苹果。如果她把 14 个苹果分配到两个饼里，每个饼里有 14/2=7 个苹果。
> 
> 答：每个饼包含 7 个苹果。

如果所有示例都像这样，模型不会直接生成答案，而是会先生成思考，这有两个大的优点：

首先，这会导致更好的答案。由于模型被迫一步一步地进行，它可以为每个子任务利用更多资源，而不是一次性解决整个任务。当你进行复杂计算时，你也会记录下中间结果，不是吗？

其次，它有助于理解模型的预测和它所犯的错误。如果你只是得到一个错误的答案，很难找出模型出现问题的原因。然而，这些思考可以给你对模型推理过程的有意义的洞察，使你能够更详细地理解失败的原因。假设模型产生了以下思考：

> 如果她剩下 4 个苹果，她已经使用了 18–4=14 个苹果。如果她把 14 个苹果分配到两个饼里，每个饼里有 14/2=9 个苹果。

在这个示例中，你可以发现，模型执行了正确的步骤，但只是最后一步计算错误。在其他情况下，你可能会看到模型没有理解任务，这可能会鼓励你重新表述任务。

# 添加动作

![](img/f74eef02a2714f4f7e1cc7404e5d7e30.png)

更多工具允许更多操作。照片来自 [Eka Cahyadi Hasti](https://unsplash.com/@ekacahyadihasti?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

通过思考链，我们赋予了模型将复杂任务拆分为更简单的子任务，并逐步解决这些子任务的能力。然而，这些子任务必须由模型自己完成，依赖于它已经掌握的技能。如果模型无法解决任何一个任务，整个思考链将结束，即使链条本身是正确的。这极大地限制了它能够完成的任务。你可以将其与人类解决任务时仅凭记忆中的知识进行比较。上面的例子你可以通过使用你记住的基本数学技能来解决，但现实世界中的大多数问题并不那么简单。假设你想回答以下问题：

> 谁更年长？安德鲁·斯科特还是本尼迪克特·康伯巴奇？

确定回答问题所需的步骤是相当简单的。首先，你需要研究安德鲁·斯科特和本尼迪克特·康伯巴奇的出生日期，然后检查哪个日期更早。然而，你不能仅凭思考来做到这一点，因为你不记得他们的生日（除非你是地球上最大的福尔摩斯粉丝）。为了实际执行这些任务，你需要帮助，例如通过咨询维基百科。

相比之下，如果你为语言模型提供额外的工具，它会变得更加熟练。[像 Langchain 这样的框架可以做到这一点](https://python.langchain.com/en/latest/modules/agents/tools.html)。假设你提供了维基百科 API，那么你可以构建一些利用该 API 的少量示例：

> 问：谁更年长？安德鲁·斯科特还是本尼迪克特·康伯巴奇？
> 
> 思考：我需要找出安德鲁·斯科特的出生日期。
> 
> 行动：query_wikipedia(search_term=’Andrew Scott’)
> 
> 观察：“安德鲁·斯科特（Andrew Scott）（1976 年 10 月 21 日出生）是一位爱尔兰演员*……”*
> 
> 思考：我知道安德鲁·斯科特的出生日期了。现在我需要找出本尼迪克特·康伯巴奇的出生日期。
> 
> 行动：query_wikipedia(search_term=’Benedict Cumberbatch’)
> 
> 观察：“本尼迪克特·提莫西·卡尔顿·康伯巴奇 CBE（Benedict Timothy Carlton Cumberbatch CBE，1976 年 7 月 19 日出生）是一位英国演员……”
> 
> 思考：我知道安德鲁·斯科特（Andrew Scott）出生于 1976 年 10 月 21 日，而本尼迪克特·康伯巴奇（Benedict Cumberbatch）出生于 1976 年 7 月 19 日。康伯巴奇的出生日期更早。因此，他更年长。
> 
> 答：本尼迪克特·康伯巴奇比安德鲁·斯科特年长。

正如你所看到的，思维总是紧跟着行动。这个行动被执行后导致一个观察，这个观察影响下一个思维。从这样的例子中，模型学会执行导致所需结果的行动，这些行动是解决初始任务的途径。你为模型提供的行动可以包括从 API 调用中检索数据到对数据库进行更改，或打开你远程控制的窗户。模型甚至可以请求另一种 AI 来完成一些子任务。正如你所见，这大大扩展了模型能够解决的任务，因为它开启了超越语言本身范围的子任务解决新途径。

# 摘要

在这篇文章中，我解释了如何使用提示来制定语言模型的任务，甚至赋予它更多的技能和能力，而无需重新训练模型。导致更复杂和强大提示的主要思想可以总结如下：

+   简单的零样本提示仅为语言模型制定任务。

+   少样本示例可以更详细地指定任务，并向模型提供解决任务的示例。

+   鼓励模型产生思维链可以让其解决复杂任务，并且以一种直观的方式让模型自我解释。

+   添加动作为模型提供了解决其自身无法解决的子任务的工具，从而允许处理更复杂的任务。

然而，为语言模型配备工具不必是终点。一个自然的下一步可能是促进模型的自动化，使其通过选择相关任务来追求目标，正如 [Baby AGI](https://www.kdnuggets.com/2023/04/baby-agi-birth-fully-autonomous-ai.html) 所做的那样。你能想到其他什么下一步？

# 进一步阅读

以下论文介绍了本文中突出显示的主要概念。

Few-shot 提示是 GPT3 模型的核心基础之一：

+   Brown, T., Mann, B., Ryder, N., Subbiah, M., Kaplan, J. D., Dhariwal, P., … & Amodei, D. (2020). 语言模型是少样本学习者。*神经信息处理系统进展*, *33*, 1877–1901。

本文介绍了思维链推理：

+   Wei, J., Wang, X., Schuurmans, D., Bosma, M., Chi, E., Le, Q., & Zhou, D. (2022). 思维链提示在大型语言模型中引发推理。*arXiv 预印本 arXiv:2201.11903*。

ReAct 是一种将思维链推理与行动相结合的突出方法：

+   Yao, S., Zhao, J., Yu, D., Du, N., Shafran, I., Narasimhan, K., & Cao, Y. (2022). ReAct：在语言模型中协同推理与行动。*arXiv 预印本 arXiv:2210.03629*。

*喜欢这篇文章吗？* [*关注我*](https://medium.com/@doriandrost) *以便获取我未来的帖子。*
