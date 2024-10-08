# T5: 文本到文本的变换器（第二部分）

> 原文：[`towardsdatascience.com/t5-text-to-text-transformers-part-two-837ba23a9eb4`](https://towardsdatascience.com/t5-text-to-text-transformers-part-two-837ba23a9eb4)

## 大型语言模型的最佳迁移学习

[](https://wolfecameron.medium.com/?source=post_page-----837ba23a9eb4--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----837ba23a9eb4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----837ba23a9eb4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----837ba23a9eb4--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----837ba23a9eb4--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----837ba23a9eb4--------------------------------) ·阅读时间 14 分钟·2023 年 7 月 5 日

--

![](img/16eb0b06a8e38a4121e6c959124618c5.png)

（照片来源于[Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)于[Unsplash](https://unsplash.com/s/photos/text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

[BERT](https://cameronrwolfe.substack.com/p/language-understanding-with-bert) [5] 的提出促使了自然语言处理（NLP）领域中迁移学习方法的普及。由于互联网上大量未标记文本的广泛存在，我们可以轻松地*(i)* 在大量原始文本上预训练大型变换器模型，以及*(ii)* 微调这些模型以准确解决下游任务。这种方法非常有效，但其新兴的受欢迎程度导致许多替代方法和改进被提出。随着这些新方法的出现，人们不禁开始想：*NLP 中迁移学习的最佳实践是什么？*

这个问题通过对统一的文本到文本变换器（T5）模型的分析得到了回答。T5 在预训练和微调期间都将所有任务重新格式化为文本到文本的格式，这意味着模型接收文本输入并生成文本输出。利用这种统一格式，T5 可以分析各种不同的迁移学习设置，从而允许比较多种方法。在[之前的通讯](https://cameronrwolfe.substack.com/p/t5-text-to-text-transformers-part)中，我们了解了 T5 模型的格式、架构和整体方法。

在本期通讯中，我们将概述 T5 进行的分析，包括不同预训练目标、架构、模型/数据规模和 NLP 领域迁移学习的训练方法的实证比较。在[1]中，这些选项被逐一研究，以确定它们对 T5 性能的影响。通过研究这些分析，我们得出了一套最佳实践，这些实践（当结合在一起时）产生了最先进的 T5 框架，能够以极高的准确性解决语言理解任务。

![](img/f405573770f7dcd828722a57a401b9aa.png)

（来自 [1]）

# 基础知识

我们已经覆盖了 T5 架构的动机和基础知识。请查看链接中的帖子 [这里](https://cameronrwolfe.substack.com/p/t5-text-to-text-transformers-part)。我们也可以快速回顾这些思想。 [BERT](https://cameronrwolfe.substack.com/p/language-understanding-with-bert) [5] 的提案普及了 [迁移学习](https://cameronrwolfe.substack.com/i/108182616/what-is-transfer-learning) 范式（即，在某些独立数据集上预训练模型，然后在目标数据集上进行微调）用于 NLP。然而，BERT 的有效性使许多研究人员将重点放在这个话题上，并提出各种修改和改进。T5 的想法是 *(i)* 将所有语言任务转换为统一的文本到文本格式（见下图），并 *(ii)* 研究各种不同的 NLP 迁移学习设置，以推断出最有效的技术。

![](img/8c1579aef03f0f44be25910121043e47.png)

（来自 [1]）

## 语言建模 vs. 去噪

初期的 NLP 迁移学习方法利用了 [因果语言建模](https://cameronrwolfe.substack.com/i/85568430/language-modeling) 目标 [6] 进行预训练。然而，随后显示去噪（也称为 [掩码语言建模](https://cameronrwolfe.substack.com/i/76273144/training-bert)，或 MLM）目标表现更好 [5]。给定一组要作为输入传递给某些模型的文本标记，MLM 通过以下方式操作：

1.  随机（均匀）选择 15% 的标记

1.  用一个 `[MASK]` 标记替换 90% 的选择标记

1.  用一个随机标记替换 10% 的选择标记

1.  训练模型预测/分类每个 `[MASK]` 标记

被均匀选择的标记的百分比称为“损坏率”。在 T5 中，我们将看到这种去噪目标的几个不同变体，但基本想法保持不变。

> *“我们所有的目标都接收来自我们未标记文本数据集的一个标记化文本片段的标记 ID 序列。标记序列被处理以生成一个（损坏的）输入序列和相应的目标。然后，模型像往常一样通过最大似然法来预测目标序列。”* — 来自 [1]

## 基准测试和评估

T5 尝试推导出 NLP 中迁移学习的最佳实践集。然而，为了确定哪些技术效果最佳，T5 在各种任务和自然语言基准上进行了评估。所有这些任务都是使用 T5 的 [文本到文本格式](https://cameronrwolfe.substack.com/i/108182616/text-to-text-framework)解决的。有关这些任务的完整描述，请参见 [1] 的第 2.3 节。下面提供了简要总结。

+   [GLUE](https://gluebenchmark.com/) 和 [SuperGLUE](https://super.gluebenchmark.com/) [7, 8]：这两个基准测试包含许多不同的任务，如 [句子接受度判断](https://nyu-mll.github.io/CoLA/)、[情感分析](https://huggingface.co/datasets/sst2)、[释义](https://paperswithcode.com/dataset/mrpc)、[句子相似度](https://paperswithcode.com/sota/semantic-textual-similarity-on-sts-benchmark)、[自然语言推断（NLI）](https://cims.nyu.edu/~sbowman/multinli/)、[共指消解](https://paperswithcode.com/sota/natural-language-inference-on-wnli)、[句子完成](https://paperswithcode.com/dataset/copa)、[词义消歧](https://pilehvar.github.io/wic/)和 [问答](https://sheng-z.github.io/ReCoRD-explorer/)。SuperGLUE 是一个改进且更具挑战性的基准测试，其结构类似于 GLUE。

+   [CNN + Daily Mail 抽象总结](https://huggingface.co/datasets/cnn_dailymail) [9]：将新闻文章与一段简短的总结文本配对，捕捉文章的主要亮点。

+   [SQuAD](https://rajpurkar.github.io/SQuAD-explorer/) [10]：一个关于维基百科文章的问答数据集，其中每个问题的答案是相关文章中的一段文本。

+   几个翻译数据集（例如，英语到德语、法语和罗马尼亚语）。

值得注意的是，GLUE 和 SuperGLUE 基准测试中的所有任务都由 T5 连接在一起，并且在所有任务上同时进行微调。

## 其他重要观点

+   不同类型的 Transformer 架构 [[link](https://cameronrwolfe.substack.com/i/108182616/different-transformer-architectures)]

+   语言建模基础 [[link](https://cameronrwolfe.substack.com/p/language-models-gpt-and-gpt-2)]

+   自注意力 [[link](https://twitter.com/cwolferesearch/status/1641932082283700226?s=20)]

# 我们从 T5 中学到了什么？

如前所述，T5 的实验尝试发现 NLP 中迁移学习的最佳实践。为此，首先提出一种基线方法，然后逐一更改该基线的几个方面（例如，模型架构/大小、数据集和预训练目标），以查看哪些效果最佳。这种方法类似于 [坐标下降](https://en.wikipedia.org/wiki/Coordinate_descent)策略。我们将首先描述基线技术，然后解释 T5 在测试各种迁移学习设置后的发现。

## T5 基线模型

![](img/1164abd82c0351276b47aebc6c0ff224.png)

（来自 [11]）

**模型**。T5 基础架构使用标准的[编码器-解码器变换器架构](https://newsletter.artofsaience.com/p/vision-transformers-from-idea-to)；见上文。编码器和解码器的结构与[BERTBase](https://huggingface.co/bert-base-uncased)类似。尽管许多现代 NLP 方法使用“单堆栈”变换器架构（例如，[仅编码器架构](https://cameronrwolfe.substack.com/i/76273144/transformer-encoders)用于 BERT 或[仅解码器架构](https://cameronrwolfe.substack.com/i/85568430/decoder-only-transformers)用于大多数语言模型），T5 选择避免这些架构。有趣的是，[1]中的作者发现编码器-解码器架构在生成和分类任务中都取得了令人印象深刻的结果。[1]中没有考虑仅编码器模型，因为它们专门用于标记/跨度预测，不能很好地解决生成任务。

![](img/c6a34aec0e8b8a501f645c36308a5e95.png)

（来自 [1]）

与编码器-解码器架构相比，仅解码器模型受到限制，因为它们仅使用[因果（或掩码）自注意力](https://cameronrwolfe.substack.com/i/108182616/different-transformer-architectures)；见上文。掩码自注意力在计算序列中任何给定标记的表示时只考虑前面的标记。然而，有些情况下我们希望对初始范围或文本前缀进行完全可见的注意力，然后基于这个前缀生成输出（例如，翻译任务）。仅解码器模型无法处理这些情况，因为它们对整个输入进行因果自注意力。

**训练 T5**。T5 模型在[C4 语料库](https://cameronrwolfe.substack.com/i/108182616/how-is-t-studied)上进行了 34B 标记的预训练。作为对比，BERT 在 137B 标记上进行训练，而 RoBERTa 在 2.2T 标记上进行训练[5, 12]。受 BERT 中的 MLM 目标启发，T5 使用略微修改的去噪目标进行预训练：

1.  随机选择输入序列中的 15%标记

1.  用单一标记替换所有连续选择的标记范围

    “哨兵”标记

1.  给每个哨兵标记一个在当前输入序列中唯一的 ID

1.  使用所有选择的标记构造目标，用哨兵标记分隔

虽然这个任务看起来有点复杂，但我们可以在下面看到它在短输入序列上的工作示例。

![](img/fc6d45fea45d7059ee02c3f765550acc.png)

（来自 [1]）

通过用单一的哨兵标记替换整个掩码标记的范围，我们降低了预训练的计算成本，因为我们通常在较短的输入和目标序列上进行操作。

**微调。** 在完成预训练后，T5 会在每个下游任务上单独进行微调，然后再进行评估。由于 T5 使用文本到文本的格式，预训练和微调都使用相同的 [最大似然目标](https://cameronrwolfe.substack.com/i/85568430/language-modeling)！换句话说，我们只需将正确答案表述为文本序列（在预训练和微调过程中），然后训练模型输出正确的文本序列。

**基线表现如何？** 如下表所示，基线 T5 模型的表现与之前的模型（如 BERT）相似，尽管这些模型并不可直接比较（即，基线 T5 模型使用的计算量是 BERTBase 的 25%）。此外，我们看到预训练在大多数任务上提供了巨大的好处。这个规则的例外是翻译任务，在这些任务中，预训练与未预训练的表现相似。

![](img/bd2737cdb0b6a47d562d90be471046cc.png)

（来自 [1]）

## 寻找更好的方法…

在测试基线架构和训练方法后，[1] 的作者逐次修改这种方法的一个方面，例如底层架构、预训练目标或微调策略。通过测试这些不同的迁移学习变体，我们可以找到在不同语言理解任务中 consistently 表现最佳的方法。

![](img/4501632abb16a5694ee835b61fef66df.png)

（来自 [1]）

**架构。** 为了研究架构选择对迁移学习结果的影响，我们可以测试不同的 [变体变换器架构](https://cameronrwolfe.substack.com/i/108182616/different-transformer-architectures)。在 [1] 中测试的架构包括普通的编码器-解码器架构、仅解码器架构以及一个前缀语言模型，它在序列中对固定前缀执行完全可见的注意力，然后使用因果自注意力生成输出；见上文。这些架构之间的主要区别在于它们的自注意力机制中使用的掩蔽类型。

![](img/3374e6d2feb1f559969604c21e1685c6.png)

（来自 [1]）

当测试几种不同的架构（使用因果语言建模和去噪目标进行预训练）时，我们发现编码器-解码器变换器架构（具有去噪目标）表现最佳，因此在剩下的实验中采用了这种架构。相对于其他模型，这种编码器-解码器变体总共有 2P 个参数，但计算成本与具有 P 个参数的仅解码器模型相同。为了将总参数数量减少到 P，我们可以在编码器和解码器之间共享参数，发现这种方法表现非常好。

**预训练目标。** 最初，T5 使用三种不同类型的预训练目标进行训练。第一种是 BERT 风格的 MLM 目标。其他目标是一个去洗牌策略（即模型尝试将打乱的句子恢复到正确的顺序）和一个基于前缀的语言建模目标。在后者中，文本被分成两个跨度，第一个跨度作为输入传递给编码器，第二个跨度由解码器预测（即回忆一下我们使用的是编码器-解码器转换器）。下文比较了这些目标训练的模型的表现，我们可以看到去噪目标明显优于其他策略。

![](img/66118100ba35a924d0d85038d805a7a4.png)

(来自 [1])

从这里开始，[1]中的作者测试了对 BERT 风格 MLM 目标的几种修改，如下表所示。

![](img/37c7525e991bc22e87aec755725e910b.png)

(来自 [1])

这些变体的表现趋于相似；见下文。然而，通过选择替换整个损坏令牌跨度为单个哨兵令牌的预训练目标，并且仅尝试预测目标中的损坏令牌，我们可以最小化预训练的计算成本。因此，遮蔽整个连续令牌跨度的基线策略是高效的，因为它产生了更短的目标序列。

![](img/2e84b4d88b27544df2239fd4afbf1c40.png)

(来自 [1])

[1] 的作者测试了不同的损坏率，发现损坏率对结果没有显著影响，并且 15%的设置效果良好。还发现一种替代的预训练目标，即明确选择令牌跨度进行损坏（即基线方法选择令牌时采用均匀分布而不是跨度，然后将连续的令牌组合在一起），其表现与基线方法相似。下图展示了[1]中测试的不同预训练目标的示意图。

![](img/2d0823916554a44745ff5a6c11a8f1cd.png)

(来自 [1])

研究了许多不同的策略，但这里的主要结论是 *(i)* 去噪目标效果最好，*(ii)* 去噪目标的变体表现相似，以及 *(iii)* 最小化目标长度的策略在计算上最为高效。

**数据和模型规模。** 最后，研究了规模对 T5 质量的影响。首先，T5 使用几个不同的数据集进行预训练，包括一个未过滤的数据集，一个新闻特定数据集，一个模仿 [GPT-2 的 WebText 语料库](https://cameronrwolfe.substack.com/i/85568430/language-models-are-unsupervised-multitask-learners-gpt)的数据集，以及几个维基百科语料库的变体。T5 在每个数据集上进行预训练后的表现如下面所示。

![](img/6c5ee2dd55d190a687d0503720236cbb.png)

(来自 [1])

我们在这里看到 *(i)* 不过滤预训练语料库是极其有害的，*(ii)* 在特定领域语料库上进行预训练在某些情况下是有帮助的。例如，在基于新闻的语料库上进行预训练在 [ReCoRD](https://sheng-z.github.io/ReCoRD-explorer/) 这个基于新闻文章的阅读理解数据集上表现最佳。

> “这些发现背后的主要教训是，在领域内未标记的数据上进行预训练可以提高下游任务的性能。这并不令人意外，但如果我们的目标是预训练一个能够快速适应来自任意领域的语言任务的模型，这种情况就显得不那么令人满意。” *— 来源 [1]*

进一步说，T5 使用不同大小的 C4 语料库的截断版本进行预训练。从这些实验中，我们了解到更多的数据（并不令人意外）更好。在预训练期间多次循环通过较小版本的数据集会导致过拟合，并损害下游性能；见下文。

![](img/dbdc4a5171a4c55177aef1a419b10f0a.png)

（来源 [1]）

为了扩展 T5 模型，作者测试了以下修改：

1.  `4X` 更多训练迭代（或 4`X` 更大的批次大小）

1.  `2X` 更多训练迭代和 2`X` 更大的模型

1.  `4X` 更大的模型

1.  训练一个由 4 个编码器-解码器变换器组成的集合

这里，为了简化，预训练和微调步骤都增加了。这些实验的结果如下所示。

![](img/fd40ae8bbd39863009892cf3d60815b2.png)

（来源 [1]）

这些结果大致符合我们的预期。增加训练时间（或批次大小）可以提高性能。将此与更大的模型结合起来，相比单独增加训练迭代或批次大小，能带来进一步的好处。换句话说，*增加预训练数据量和模型大小在提升性能方面是互补的*。

> “机器学习研究的痛苦教训认为，可以利用额外计算的通用方法最终会胜过依赖人类专业知识的方法” *— 来源 [1]*

**其他内容。** T5 也使用不同的多任务训练策略进行了微调。总体而言，这些模型的表现略逊于那些针对每个任务单独微调的模型。然而，确实存在策略来最小化任务特定微调和多任务学习之间的性能差距。有关更多信息，请查看 [此处](https://www.ruder.io/multi-task/) 的概述。

许多深度神经网络的微调方法仅训练模型参数的子集（例如，“冻结”早期层并仅微调模型中的最后几层）。[1] 的作者尝试了几种这种微调 T5 的技术（例如，通过 [适配器层](https://medium.com/dair-ai/adapters-a-compact-and-extensible-transfer-learning-method-for-nlp-6d18c2399f62) 或逐步解冻 [6]），但这些方法被端到端微调完整模型所超越；见下文。

![](img/1440ddf98e99d73602f1670d655f3113.png)

（来自[1]）

# T5: 把一切整合在一起！

现在我们已经回顾了[1]中的整个实验分析，我们对 NLP 中的迁移学习不同选项以及什么效果最好有了更清晰的认识！下面，我们将讨论这一分析的主要要点，这些要点构成了 T5 所使用的官方迁移学习框架。与各种替代方案相比，这种方法被发现表现相当不错。

**基线设置。** 首先，让我们回顾一下 T5 的基线架构。它是一个编码器-解码器变换器，通过统一的[文本到文本格式](https://cameronrwolfe.substack.com/i/108182616/text-to-text-framework)进行训练。在进行去噪预训练后，模型会在每个下游任务上单独进行微调，然后再进行评估。值得注意的是，最终的 T5 模型在 GLUE 和 SuperGLUE 基准测试中对每个任务进行了单独的微调，因为对所有任务同时训练的效果稍微差一点（假设我们采取必要的步骤以避免过拟合）。

**预训练。** 最终的 T5 方法并不是均匀选择标记，而是进行跨度破坏（即一次选择整个标记跨度进行破坏），平均跨度长度为三。然而，15%的标记仍然会被选择进行破坏。这个目标略好于基线，并产生更短的目标序列长度。此外，T5 将无监督预训练更新与多任务监督更新混合使用。无监督更新与监督更新的比例取决于所使用的模型大小（即，大型模型需要更多无监督更新以避免过拟合）。

**训练量。** 额外的预训练对 T5 的性能有帮助。具体来说，增加批量大小和训练迭代次数都能提升 T5 的性能。因此，最终的 T5 模型在总共预训练了 1T 标记。这比基线的 34B 标记要大得多，但仍远远低于在 2.2T 标记上进行预训练的 RoBERTa[12]。预训练是在通用的过滤 C4 数据集上进行的，因为特定任务的预训练在不同任务中没有一致的好处。

**模型规模。** 使用更大的模型是有帮助的，但有时较小的模型可能更合适（例如，当你在推理时计算资源有限）。因此，T5 发布了五种不同规模的模型，参数从 220M 到 11B 不等。因此，T5 实际上是一套不同的模型！我们可以通过链接[这里](https://huggingface.co/docs/transformers/model_doc/t5)访问这些模型。

# 结论

非常感谢阅读这篇文章。我是 [Cameron R. Wolfe](https://cameronrwolfe.me/)， [Rebuy](https://www.rebuyengine.com/) 的 AI 总监。我研究深度学习的实证和理论基础。你还可以查看我在 medium 上的 [其他文章](https://medium.com/@wolfecameron)！如果你喜欢这篇文章，请在 [twitter](https://twitter.com/cwolferesearch) 上关注我，或订阅我的 [Deep (Learning) Focus 新闻通讯](https://cameronrwolfe.substack.com/)，在这里我通过对流行论文的易懂概述帮助读者建立对 AI 研究主题的深入理解。

# 参考文献

[1] Raffel, Colin, 等. “探索统一文本到文本转换器的迁移学习极限。” *机器学习研究杂志* 21.1 (2020): 5485–5551。

[2] Liu, Peter J., 等. “通过总结长序列生成维基百科。” *arXiv 预印本 arXiv:1801.10198* (2018)。

[3] Liu, Peter J., Yu-An Chung 和 Jie Ren. “Summae: 使用长度无关的自编码器进行零样本抽象文本总结。” *arXiv 预印本 arXiv:1910.00998* (2019)。

[4] Song, Kaitao, 等. “Mass: 用于语言生成的掩码序列到序列预训练。” *arXiv 预印本 arXiv:1905.02450* (2019)。

[5] Devlin, Jacob, 等. “Bert: 用于语言理解的深度双向转换器预训练。” *arXiv 预印本 arXiv:1810.04805* (2018)。

[6] Howard, Jeremy 和 Sebastian Ruder. “通用语言模型微调用于文本分类。” *arXiv 预印本 arXiv:1801.06146* (2018)。

[7] Wang, Alex, 等. “GLUE: 一个多任务基准和自然语言理解分析平台。” *arXiv 预印本 arXiv:1804.07461* (2018)。

[8] Wang, Alex, 等. “Superglue: 一个更具粘性的通用语言理解系统基准。” *神经信息处理系统进展* 32 (2019)。

[9] Hermann, Karl Moritz, 等. “教机器阅读和理解。” *神经信息处理系统进展* 28 (2015)。

[10] Rajpurkar, Pranav, 等. “Squad: 100,000+ 个用于机器文本理解的问题。” *arXiv 预印本 arXiv:1606.05250* (2016)。

[11] Vaswani, Ashish, 等. “注意力机制就是你所需要的一切。” *神经信息处理系统进展* 30 (2017)。

[12] Liu, Yinhan, 等. “Roberta: 一种稳健优化的 BERT 预训练方法。” *arXiv 预印本 arXiv:1907.11692* (2019)。
