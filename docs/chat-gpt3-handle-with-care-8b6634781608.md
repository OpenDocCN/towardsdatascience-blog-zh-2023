# ChatGPT — 小心使用

> 原文：[`towardsdatascience.com/chat-gpt3-handle-with-care-8b6634781608`](https://towardsdatascience.com/chat-gpt3-handle-with-care-8b6634781608)

## 了解 ChatGPT 的实际能力和限制对于充分利用这一技术至关重要。香港大学人工智能研究中心的最新研究论文权衡了 OpenAi 算法的局限性和优势。

[](https://misclassified.medium.com/?source=post_page-----8b6634781608--------------------------------)![Giovanni Bruner](https://misclassified.medium.com/?source=post_page-----8b6634781608--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b6634781608--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b6634781608--------------------------------) [Giovanni Bruner](https://misclassified.medium.com/?source=post_page-----8b6634781608--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b6634781608--------------------------------) ·阅读时间 6 分钟·2023 年 3 月 13 日

--

![](img/ffc519eec3fa34c4ea5ccb1ca9d769bb.png)

图片由 [Possessed Photography](https://unsplash.com/@possessedphotography?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

首先，出现了语言模型。直观上很简单：一个词序列中的下一个词可以用概率分布建模，并且严重依赖于前面的词。词汇是有限的语料库的一部分（英语词汇中有 170,000 个标记）。每个词的含义是有限的。词序列遵循缓慢变化的内部元数据集：语法。这是一个可预测的结构。你可以期待一个动词后面跟着名词，而不是另一个动词。语法和意义，作为限制，限制了下一个词预测中的随机性。这无疑比预测一千家公司第二天的股票价格要容易得多。此外，语言模型本质上是自回归的，下一个词的预测依赖于前面的词，而且需要考虑的潜在不可观测的变量也不多。

正因为如此，语言模型适合使用预训练模型和**迁移学习**。这是解锁新 AI 革命的关键特性。迁移学习意味着你可以使用别人预训练的模型，比如在 20 GB 的维基百科文章上训练的模型，而无需用自己的数据重新训练，只需进行少量调整以适应你的问题。

这怎么可能呢？好吧，你的语言问题不太可能需要使用与维基百科上完全不同的语法和词汇。迁移学习在人们开始争论第二次人工智能寒冬的时候，开启了新的人工智能夏季。

预训练模型变得更大、更快，随着参数数量和使用的数据量的增加，性能也得到了提升。经验发现，语言模型的性能会随着模型大小的增加而提升，直到达到计算能力的上限。计算机芯片已尽可能地强大。为了让语言模型持续增长，必须发生一些事情。高效地在多台机器上并行训练的方式是显而易见的。变压器的出现。

![](img/d6f7b0f4625c887b78cb0cf6d929582d.png)

照片由[Aditya Vyas](https://unsplash.com/@aditya1702?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

由 Google Brain 团队发布的变压器语言模型，在传统语言序列模型上进行了一系列令人印象深刻的创新改进。其核心是广泛使用多头自注意力机制和设计为在多个并行 GPU 上运行的模型架构。注意力机制大大改善了语言模型中预测下一个词的任务，以比传统递归神经网络更高效的方式传播序列中所有词汇的信息。

变压器使得大规模、非常大规模的语言模型的预训练成为可能。从 2019 年的 GPT 2 的 15 亿参数，到 2020 年的 GPT3 的 1750 亿参数。铺平了 2022 年 ChatGPT 的惊人发布以及大型语言模型（LLM）时代的道路。

## ChatGPT 在许多方面表现出色，但请注意幻觉。

这虽然是一个相当长的引言，但在放置背景时非常重要。语言模型不是黑魔法，也不是人工通用智能。它们不会在短时间内超越人类。它们是极其有用的工具，在预测序列中的下一个词方面表现出色。像所有人类发明的工具一样，如果我们不阅读说明书和细则，它们可能会造成伤害。

词汇是有限的，具有有限的意义，它们以可预测的方式组合，遵循语法结构。然而，信息，即意义的结合方式，是无限的，不一定是可预测的。像 ChatGPT 这样的超大语言模型可以生成全新的信息，完全是虚构的。这反过来又生成了未经验证的事实的新叙述。它们具有参数化的记忆，没有访问外部知识库的能力。我们已经看到它们在概率方面表现良好，但它们没有内部机制来分辨真相与谎言。简而言之，它们可能会出现幻觉。

请查看下面。我在这里假装自己是一位著名的数据科学家，询问 ChatGPT 关于我自己的问题。

![](img/57af5cbc1bdab36c6b3ebe9cb7aaf809.png)

作者提供的图片

ChatGPT 没错。可能我在妈妈面前很有名，但仅此而已。但接着我感到被冒犯了，完全编造了关于自己的额外信息。

![](img/71f18f6a68766699eb6b9f439b8f8ab1.png)

作者提供的图片

当然，我不是 Kaggle 大师（我希望我能成为），但 AI 对我表示歉意。

ChatGPT 的突然流行在这一领域是前所未有的，这要归功于一个可以交互的界面，保留了积累的知识。对话界面使用了带有人工反馈的强化学习（RCHF）。问题在于，积累的知识可能基于显然不真实的后续问题和纠正。

Yejin Bang、Pascale Fung 及其博士团队几周前发布了一个广泛的框架，用于定量评估像 ChatGPT 这样的模型在公开可用数据集上的表现。作为一个零样本学习者——一个无需专门调整即可回答任何问题的模型——ChatGPT 在大多数任务中被评为最先进。其在问答、情感分析和虚假信息检测方面有了大幅提升。

![](img/d5cc89a7d05f863ef5f43b25544e463a.png)

图片来自论文： [`arxiv.org/pdf/2302.04023.pdf`](https://arxiv.org/pdf/2302.04023.pdf)

关于推理，这是一个最具争议的特性之一，研究人员发现 ChatGPT 在演绎推理方面表现非常好，但在归纳推理和解决数学问题方面表现非常差。

演绎推理是从一般前提出发得出具体结论的过程，当前提包含足够的信息来引导你找到解决方案时效果良好。研究发现该算法在这些推理任务中表现优越。

![](img/000b77ef38e431bf1ea48196cca9645d.png)

图片来源于 Pascale Fung 的 YouTube 视频：[`www.youtube.com/watch?v=ORoTJZcLXek`](https://www.youtube.com/watch?v=ORoTJZcLXek)

归纳是逆向过程。它是从数据中提取信息以推断出一个普遍结论。演绎思维则是在你需要检验一个理论时所遵循的智力过程，而归纳思维则是帮助你形成一个理论的过程。换句话说，给定大量详细前提，你可以期望 ChatGPT 提出一些样本数据，但不要期望它根据一些样本数据提出一个通用规则。

实际上，ChatGPT 目前还无法像人类那样形成对世界的概念。

## 参考文献

**Yejin Bang 等**，《ChatGPT 在推理、幻想和互动方面的多任务、多语言、多模态评估》，[`arxiv.org/pdf/2302.04023.pdf`](https://arxiv.org/pdf/2302.04023.pdf)

**Pascale Fung**，ChatGPT: Prof. Pascale Fung 所讲的《ChatGPT 能做什么和不能做什么》，[`www.youtube.com/watch?v=ORoTJZcLXek`](https://www.youtube.com/watch?v=ORoTJZcLXek)

**阿希什·瓦斯瓦尼**等人，注意力机制就是你所需要的，[`arxiv.org/pdf/1706.03762.pdf`](https://arxiv.org/pdf/1706.03762.pdf)
