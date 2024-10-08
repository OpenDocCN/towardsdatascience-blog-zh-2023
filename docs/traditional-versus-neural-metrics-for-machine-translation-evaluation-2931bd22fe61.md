# 传统指标与神经指标在机器翻译评估中的比较

> 原文：[`towardsdatascience.com/traditional-versus-neural-metrics-for-machine-translation-evaluation-2931bd22fe61`](https://towardsdatascience.com/traditional-versus-neural-metrics-for-machine-translation-evaluation-2931bd22fe61)

## 自 2010 年以来新增的 100 多种指标

[](https://medium.com/@bnjmn_marie?source=post_page-----2931bd22fe61--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----2931bd22fe61--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2931bd22fe61--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2931bd22fe61--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----2931bd22fe61--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----2931bd22fe61--------------------------------) ·15 分钟阅读·2023 年 3 月 9 日

--

![](img/6177bf23f91d84b81a888a899af3c75e.png)

图片来源于[Pixabay](https://pixabay.com/photos/tunnel-light-grim-dark-art-6786462/)

使用自动指标进行评估的优势在于其**速度更快、可重复性更强且成本更低**，相比于由人工进行的评估。

这一点在机器翻译的评估中尤为明显。对于人工评估，我们理想情况下需要专家翻译人员

对于许多语言对而言，这些专家**极其稀少且难以聘请**。

大规模且快速的人工评估，如机器翻译这一动态研究领域所需的评估新系统，通常是不切实际的。

因此，机器翻译的自动评估已经成为一个**非常活跃且富有成效的研究领域**，已经有超过 20 年的历史。

尽管 BLEU 仍然是使用最广泛的评估指标，但有无数更好的替代方案。

[](/bleu-a-misunderstood-metric-from-another-age-d434e18f1b37?source=post_page-----2931bd22fe61--------------------------------) ## BLEU：一种被误解的古老指标

### 但在 AI 研究中仍然在用

towardsdatascience.com

自 2010 年以来，已经提出了 100 多种自动指标以改进机器翻译评估。

在这篇文章中，我介绍了最受欢迎的指标，这些指标作为 BLEU 的替代方案或补充方案。我将它们分为两类：传统指标和神经指标，每类都有不同的优势。

# 机器翻译的自动评估指标

大多数机器翻译的自动指标只需要：

+   机器翻译系统生成的**翻译假设**用于评估

+   至少需要一个**参考翻译**由人工生成

+   （很少）**机器翻译系统翻译的源文本**

这是一个法语到英语翻译的例子：

+   来源句子：

*Le chat dort dans la cuisine donc tu devrais cuisiner ailleurs.*

+   翻译假设（由机器翻译生成）：

*The cat sleeps in the kitchen so cook somewhere else.*

+   参考翻译：

*The cat is sleeping in the kitchen, so you should cook somewhere else.*

翻译假设和参考翻译都是相同源文本的翻译。

自动评价指标的目标是生成一个可以被解释为翻译假设和参考翻译之间距离的分数。距离越小，系统生成的翻译就越接近人类翻译质量。

指标返回的绝对分数**通常不能单独解释**。它几乎总是用来**排名机器翻译系统**。得分更高的系统就是更好的系统。

在我的一项研究中（[Marie et al., 2021](https://aclanthology.org/2021.acl-long.566.pdf)），我展示了几乎 99% 的机器翻译研究论文依赖于自动评价指标 BLEU 来评估翻译质量和排名系统，而在过去 12 年中，**已经提出了 100 多种其他指标**。*注意：我只查看了 2010 年以来 ACL 发布的研究论文。可能还有更多指标被提出用于评估机器翻译。*

这是一个不完整的 106 种指标的列表，提出于 2010 年到 2020 年（点击指标名称获取来源）：

[名词短语分块](https://aclanthology.org/P10-1012.pdf)，[SemPOS 精炼](https://aclanthology.org/P10-2016.pdf)，[mNCD](https://aclanthology.org/P10-2015.pdf)，[RIBES](https://aclanthology.org/D10-1092.pdf)，[扩展 METEOR](https://aclanthology.org/N10-1031.pdf)，[Badger 2.0, ATEC 2.1, DCU-LFG, LRKB4, LRHB4, I-letter-BLEU, I-letter-recall, SVM-RANK,TERp, IQmt-DR, BEwT-E, Bkars, SEPIA](https://aclanthology.org/W10-1703.pdf)，[MEANT](https://aclanthology.org/P11-1023.pdf)，[AM-FM](https://aclanthology.org/P11-2027.pdf)。 [AMBER, F15, MTeRater, MP4IBM1, ParseConf, ROSE, TINE](https://aclanthology.org/W11-2103.pdf)，[TESLA-CELAB](https://aclanthology.org/P12-1097.pdf)，[PORT](https://aclanthology.org/P12-1098.pdf)，[词汇衔接](https://aclanthology.org/D12-1097.pdf)，[pFSM, pPDA](https://aclanthology.org/D12-1090.pdf)，[HyTER](https://aclanthology.org/N12-1017.pdf)，[SAGAN-STS, SIMPBLEU, SPEDE, TerrorCAT, BLOCKERRCATS, XENERRCATS, PosF, TESLA](https://aclanthology.org/W12-3102.pdf)，[LEPOR, ACTa, DEPREF, UMEANT, LogRefSS](https://aclanthology.org/W13-2202v2.pdf)，[基于话语的](https://aclanthology.org/P14-1065.pdf)，[XMEANT](https://aclanthology.org/P14-2124.pdf)，[BEER](https://aclanthology.org/D14-1025.pdf)，[SKL](https://aclanthology.org/D14-1027.pdf)，[AL-BLEU](https://aclanthology.org/D14-1026.pdf)，[LBLEU](https://aclanthology.org/D14-1140.pdf)，[APAC, RED-*, DiscoTK-*, ELEXR, LAYERED, Parmesan, tBLEU, UPC-IPA, UPC-STOUT, VERTa-*](https://aclanthology.org/W14-3336.pdf)，[pairwise neural](https://aclanthology.org/P15-1078.pdf)，[基于神经表示](https://aclanthology.org/P15-2025.pdf)，[ReVal](https://aclanthology.org/D15-1124.pdf)，[BS, LeBLEU, chrF, DPMF, Dreem, Ratatouille, UoW-LSTM, UPF-Colbat, USAAR-ZWICKEL](https://aclanthology.org/W15-3031.pdf)，[CharacTER, DepCheck, MPEDA, DTED](https://aclanthology.org/W16-2302.pdf)，[意义特征](https://aclanthology.org/E17-1020.pdf)，[BLEU2VEC_Sep, Ngram2vec, MEANT 2.0, UHH_TSKM, AutoDA, TreeAggreg, BLEND](https://aclanthology.org/W17-4755.pdf)，[HyTERA](https://aclanthology.org/N18-2077.pdf)，[RUSE, ITER, YiSi](https://aclanthology.org/W18-6450.pdf)，[BERTr](https://aclanthology.org/P19-1269.pdf)，[EED, WMDO, PReP](https://aclanthology.org/W19-5302.pdf)，[跨语言相似性+目标语言模型](https://aclanthology.org/2020.acl-main.151.pdf)，[XLM+TLM](https://aclanthology.org/2020.acl-main.327.pdf)，[Prism](https://aclanthology.org/2020.emnlp-main.8.pdf)，[COMET](https://aclanthology.org/2020.emnlp-main.213.pdf)，[PARBLEU, PARCHRF, MEE, BLEURT, BAQ-*, OPEN-KIWI-*, BERT, mBERT, EQ-*](https://aclanthology.org/2020.wmt-1.77.pdf)

这些度量中的大多数已被证明比 BLEU 更好，但从未被使用。事实上，只有 2 个（1.8%），即 RIBES 和 chrF，被用于两篇以上的研究论文中（在我检查的 700 多篇论文中）。自 2010 年以来，**最常用的度量是 2010 年之前提出的度量**（BLEU、TER 和 METEOR）：

![](img/e33b3b596dc9e5171fb2919ae936d6d2.png)

表格由[Marie et al., 2021](https://aclanthology.org/2021.acl-long.566.pdf)提供

大多数 2016 年后创建的度量是神经度量。它们依赖于神经网络，最新的甚至依赖于非常流行的预训练语言模型。

相比之下，早期发布的传统度量可以更简单且成本更低。由于各种原因，它们仍然极其受欢迎，并且这种受欢迎程度似乎没有下降，至少在研究中是这样。

在接下来的章节中，我将介绍几个根据其受欢迎程度、原创性或与人工评估相关性选择的度量。

# 传统度量

机器翻译评估的传统度量可以被视为基于两个字符串包含的字符**之间的距离**来评估的度量。

这两个字符串分别是翻译假设和参考翻译。注意：*通常，传统度量不会利用系统翻译的源文本。*

WER（字错误率）曾是这些度量中使用最广泛的，并且是 BLEU 的前身，直到 BLEU 在 2000 年代初期取代了它。

优势：

+   **计算成本低**：大多数传统度量依赖于字符和/或标记级别上运行的字符串匹配算法的效率。有些度量确实需要对标记进行一些移动，这可能更昂贵，特别是对于长翻译。然而，它们的计算易于并行化，并且不需要 GPU。

+   **可解释**：小段的分数通常可以手动轻松计算，从而促进分析。*注意：“可解释”并不意味着“可解读”，即我们可以确切解释度量分数的计算方法，但分数本身无法解释，因为它通常无法告诉我们翻译质量。*

+   **语言无关**：除了一些特定度量外，相同的度量算法可以独立于翻译语言进行应用。

缺点：

+   **与人工评估的相关性差**：这是它们相对于神经度量的主要缺点。为了获得对翻译质量的最佳估计，不应使用传统度量。

+   **需要特定预处理**：除了一个度量（chrF）外，我在本文中介绍的所有传统度量都需要被评估的段落及其参考翻译进行分词。分词器不包含在度量中，即需要用户使用外部工具进行。获得的分数因此依赖于特定的分词，可能无法重复。

## BLEU

这是最受欢迎的度量标准。几乎 99%的机器翻译研究出版物都使用它。

我已经在我的上一篇文章中介绍过 BLEU。

BLEU 是一个存在许多明显缺陷的度量标准。

[](https://medium.com/@bnjmn_marie/12-critical-flaws-of-bleu-1d790ccbe1b1?source=post_page-----2931bd22fe61--------------------------------) [## 12 BLEU 的关键缺陷

### 根据 37 项研究结果，为什么你不应该相信 BLEU，这些研究已发表超过 20 年。

medium.com](https://medium.com/@bnjmn_marie/12-critical-flaws-of-bleu-1d790ccbe1b1?source=post_page-----2931bd22fe61--------------------------------)

我在关于 BLEU 的两篇文章中没有讨论 BLEU 的众多变体。

在阅读研究论文时，你可能会发现度量标准标记为 BLEU-1、BLEU-2、BLEU-3 等。连字符后的数字*通常*表示用于计算分数的 n-grams 的最大长度。

例如，BLEU-4 是通过考虑{1,2,3,4}-grams 的令牌来计算的 BLEU。换句话说，BLEU-4 是大多数机器翻译论文中计算的典型 BLEU，如[Papineni 等人（2002）](https://aclanthology.org/P02-1040.pdf)最初提出的。

BLEU 是一个需要大量统计数据才能准确的度量标准。它在短文本上表现不佳，甚至可能在计算与参考翻译中的任何 4-gram 不匹配的翻译时产生错误。

由于在某些应用或分析中可能需要按句子级别评估翻译质量，因此可以使用一种变体，称为句子 BLEU、sBLEU，或有时称为 BLEU+1。它避免了计算错误。BLEU+1 有很多变体。最流行的变体由[Chen 和 Cherry（2014）](https://aclanthology.org/W14-3346.pdf)描述。

正如我们将看到的神经度量标准，BLEU+1 有许多更好的替代方案，不应使用。

## chrF(++)

chrF（[Popović，2015](https://aclanthology.org/W15-3049.pdf)）是机器翻译评估中第二受欢迎的度量标准。

自 2015 年以来，它逐渐在机器翻译出版物中得到越来越多的应用。

已经证明，它与人类判断的相关性优于 BLEU。

此外，chrF 是与分词无关的。这是我知道的唯一具有这一特性的度量标准。由于它不需要任何外部工具进行自定义分词，因此它是确保评估可重复性最佳的度量标准之一。

chrF 完全依赖于字符。空格默认被忽略。

chrF++（[Popović，2017](https://aclanthology.org/W17-4770.pdf)）是 chrF 的一个变体，与人工评估的相关性更高，但代价是失去了分词独立性。确实，chrF++利用空格来考虑词序，因此与人工评估的相关性更好。

我在为会议和期刊审阅机器翻译论文时强烈推荐使用 chrF，以使评估更具可重复性，但不推荐 chrF++，因为它依赖于分词。

*注意：阅读使用 chrF 的研究工作时要小心。作者常常将 chrF 和 chrF++混淆。他们也可能在使用 chrF++时引用 chrF 的论文，反之亦然。*

[Maja Popović的 chrF 原始实现](https://github.com/m-popovic/chrF)可以在 github 上找到。

你还可以在 [SacreBLEU](https://github.com/mjpost/sacrebleu)（Apache 2.0 许可证）中找到一个实现。

## RIBES

RIBES ([Isozaki et al., 2010](https://aclanthology.org/D10-1092.pdf)) 在研究社区中被定期使用。

该指标是为具有非常不同句子结构的“远程语言对”设计的。

例如，将英语翻译成日语需要显著的词序调整，因为在日语中动词位于句子的末尾，而在英语中动词通常放在补语之前。

RIBES 的作者发现 2010 年时可用的指标对错误的词序惩罚不足，因此提出了这一新指标。

[RIBES 的实现可以在 Github 上找到](https://github.com/nttcslab-nlp/RIBES)（GNU 通用公共许可证 V2.0）。

## METEOR

METEOR ([Banerjee and Lavie, 2005](https://aclanthology.org/W05-0909.pdf)) 最早在 2005 年提出，旨在纠正当时可用的传统指标中的几个缺陷。

例如，BLEU 只计算准确的词汇匹配。由于 BLEU 不会奖励与参考翻译不完全相同但具有类似意义的词，因此它过于严格。因而，BLEU 对许多有效的翻译视而不见。

METEOR 通过引入更多的匹配灵活性部分修正了这一缺陷。同义词、词干甚至释义都被接受为有效的翻译，从而有效提高了指标的召回率。该指标还实现了加权机制，例如，更注重准确匹配而不是词干匹配。

该指标通过召回率和精确率的调和平均数来计算，特别之处在于召回率的权重高于精确率。

METEOR 与人类评估的相关性优于 BLEU，并且在 2015 年之前已经经过多次改进。它至今仍然被定期使用。

[METEOR 有一个由 CMU 维护的官方网页](https://www.cs.cmu.edu/~alavie/METEOR/)，提供了该指标的原始实现（许可证未知）。

## TER

TER ([Snover et al., 2006](https://aclanthology.org/2006.amta-papers.25.pdf)) 主要用于评估人类翻译者后编辑翻译所需的努力。

> **定义**
> 
> 机器翻译中的后编辑是将机器翻译输出修正为可接受的翻译的过程。机器翻译加上后编辑是翻译行业中一种标准流程，用于减少翻译成本。

有两个知名的变体：TERp ([Snover 等人, 2009](https://web.jhu.edu/HLTCOE/Publications/terplusdorr.pdf)) 和 HTER ([Snover 等人, 2009](https://web.jhu.edu/HLTCOE/Publications/terplusdorr.pdf), [Specia 和 Farzindar, 2010](https://aclanthology.org/2010.jec-1.5.pdf))。

TERp 是在 TER 的基础上增加了一个同义句数据库，以提高度量的召回率及其与人工评估的相关性。如果翻译假设中的一个标记或其同义句出现在参考翻译中，则算作匹配。

HTER，即“人工 TER”，是计算机器翻译假设与其人工后编辑之间的标准 TER。它可以用来评估后编辑特定翻译的成本。

## CharacTER

该度量的名称已经给出了一些关于其工作原理的提示：这是在字符级别应用的 TER 度量。移位操作在词级别进行。

获得的编辑距离也按翻译假设的长度进行标准化。

CharacTER ([Wang 等人, 2016](https://aclanthology.org/W16-2342.pdf)) 在传统度量中与人工评估的相关性最高。

尽管如此，它的使用仍然少于其他度量。我最近找不到使用它的论文。

[其作者对 CharacTER 的实现](https://github.com/rwth-i6/CharacTER)可以在 Github 上找到（未知许可证）。

# 神经度量

神经度量采用了与传统度量截然不同的方法。

他们使用**神经网络**来估计翻译质量评分。

据我所知，2015 年提出的 ReVal 是第一个旨在计算翻译质量评分的神经度量。

自 ReVal 以来，新的神经度量定期被提出用于评估机器翻译。

机器翻译评估的研究工作现在几乎**完全专注于神经度量**。

尽管如此，正如我们将看到的，尽管神经度量具有优势，但其普及度仍远远不如传统度量。尽管神经度量已经存在近 8 年，但传统度量仍然被研究界压倒性地偏好（在机器翻译行业的情况可能不同）。

优势：

+   **与人工评估的良好相关性**：神经度量是机器翻译评估的最先进技术。

+   **无需预处理**：这主要适用于最近的神经度量，如 COMET 和 BLEURT。预处理，如标记化，由度量内部和透明地完成，即用户无需关心。

+   **更好的召回率**：得益于嵌入的利用，神经指标即使在翻译与参考不完全匹配时也能给出奖励。例如，与参考中某个词意义相似的词更可能被指标奖励，这与只能奖励精确匹配的传统指标形成对比。

+   **可训练**：这可能既是优点也是缺点。大多数神经指标必须经过训练。如果你有适用于特定用例的训练数据，这是一个优势。你可以微调指标以最好地与人类判断相关。然而，如果没有特定的训练数据，与人类评估的相关性将远非最佳。

缺点：

+   **高计算成本**：神经指标不需要 GPU，但如果有 GPU 的话会更快。然而，即使有 GPU，它们也明显比传统指标慢。一些依赖大型语言模型的指标，如 BLEURT 和 COMET，也需要大量内存。它们的高计算成本还使得统计显著性测试极为昂贵。

+   **无法解释**：理解神经指标为何产生特定评分几乎是不可能的，因为其背后的神经模型通常利用了数百万或数十亿个参数。提高神经模型的可解释性是一个非常活跃的研究领域。

+   **难以维护**：如果没有得到适当维护，较老的神经指标实现将不再有效。这主要是由于 nVidia CUDA 以及（py）Torch 和 Tensorflow 等框架的变化。当前使用的神经指标在 10 年后可能无法使用。

+   **不可重复**：神经指标通常比传统指标有更多的超参数。这些在使用它们的科学出版物中大多未被详细说明。因此，重复特定数据集的特定评分通常是不可能的。

## ReVal

据我所知，ReVal ([Gupta et al., 2015](https://aclanthology.org/D15-1124.pdf)) 是第一个提出用于评估机器翻译质量的神经指标。

ReVal 相比传统指标有显著改进，与人类评估的相关性显著更好。

该指标基于 LSTM，虽然非常简单，但据我所知，尚未在机器翻译研究中使用过。

现在已被更新的指标所超越。

如果你有兴趣了解它的工作原理，你仍然可以在[Github 上找到 ReVal 的原始实现](https://github.com/rohitguptacs/ReVal)（GNU 通用公共许可证 V2.0）。

## YiSi

YiSi ([Chi-kiu Lo, 2019](https://aclanthology.org/W19-5358.pdf)) 是一个非常多功能的指标。它主要利用了嵌入模型，但可以通过各种资源如语义解析器、大型语言模型（BERT），甚至是源文本和源语言的特征来增强。

使用所有这些选项可能会使其相当复杂，并将其范围缩小到少数语言对。此外，使用所有这些选项时，与人工判断的相关性提升并不明显。

尽管如此，使用仅原始嵌入模型的指标与人工评估表现出非常好的相关性。

![](img/67b29d9e097f29153a18e57206fddddb.png)

图来源于[Chi-kiu Lo, 2019](https://aclanthology.org/W19-5358.pdf)

作者展示了在评估英语翻译时，YiSi 显著优于传统指标。

YiSi 的原始实现[公开在 Github 上](https://github.com/chikiulo/yisi)（MIT 许可证）。

## BERTScore

BERTScore ([Zhang et al., 2020](https://arxiv.org/pdf/1904.09675.pdf)) 利用 BERT 对评估句子中每个标记的上下文嵌入，并将其与参考的标记嵌入进行比较。

它的工作方式如下所示：

![](img/38fa77a7febfd7d0a00bdce488c3aec6.png)

图来源于[Zhang et al., 2020](https://arxiv.org/pdf/1904.09675.pdf)

它是最早采用大型语言模型进行评估的指标之一。它不是专门为机器翻译提出的，而是为任何语言生成任务设计的。

BERTScore 是机器翻译评估中使用最广泛的神经指标。

[BERTScore 实现](https://github.com/Tiiiger/bert_score)可在 Github 上获得（MIT 许可证）。

## BLEURT

BLEURT ([Sellam et al., 2020](https://aclanthology.org/2020.acl-main.704.pdf)) 是另一个依赖于 BERT 的指标，但可以专门针对机器翻译评估进行训练。

更确切地说，它是一个在合成数据上微调的 BERT 模型，这些数据是来自维基百科的句子与其不同类型的随机扰动：*注意：这一步被作者混乱地称为“预训练”（见论文中的注释 3），但实际上是在 BERT 的原始预训练之后进行的。*

+   被遮蔽的词（如原始 BERT 中）

+   被丢弃的词

+   回译（即机器翻译系统生成的句子）

每对句子在训练过程中都会通过多个损失进行评估。其中一些损失是通过评估指标计算的：

![](img/75a0959127e110f106e5646f4a15184c.png)

表格来源于[Sellam et al., 2020](https://aclanthology.org/2020.acl-main.704.pdf)

最后，在第二阶段，BLEURT 会在翻译和由人工提供的评分上进行微调。

直观地说，由于使用了可能类似于机器翻译错误或输出的合成数据，BLEURT 在质量和领域漂移方面比 BERTScore 更具鲁棒性。

此外，由于 BLEURT 利用了作为“预训练信号”的指标组合，它在直观上比这些指标中的每一个都要好，包括 BERTScore。

然而，训练 BLEURT 的成本非常高。 我只知道 Google 发布的 BLEURT 检查点。 *注意：如果你知道其他模型，请在评论中告诉我。*

第一个版本仅对英语进行训练，但较新的版本，称为 BLEURT-20，现在包括 19 种其他语言。[这两个 BLEURT 版本可以在同一仓库中获取](https://github.com/google-research/BLEURT)。

## Prism

在提出 Prism 的工作中，[Thompson 和 Post（2019）](http://aclanthology.lst.uni-saarland.de/2020.emnlp-main.8.pdf)直观地认为机器翻译和释义评估是非常相似的任务。他们唯一的区别在于源语言不同。

确实，通过释义，目标是生成一个新的句子 A’，给定句子 A，其中 A 和 A’具有相同的意义。评估 A 和 A’的相似度与评估翻译假设与给定参考翻译的接近程度是相同的。换句话说，翻译假设是否是参考翻译的一个好的释义。

Prism 是通过多语言神经机器翻译框架在大型多语言平行数据集上训练的神经度量标准。

然后，在推理时，训练好的模型被用作零-shot 释义器，以评分源文本（翻译假设）和目标文本（参考翻译）之间的相似度，这两者都在同一语言中。

这种方法的主要优点是 Prism 不需要任何人工评估训练数据，也不需要任何释义训练数据。唯一的要求是拥有你计划评估的语言的平行数据。

尽管 Prism 是原创的、方便训练，并且似乎优于大多数其他度量标准（包括 BLEURT），但我没有找到使用它的机器翻译研究出版物。

[Prism 的原始实现可以在 Github 上公开获取](https://github.com/thompsonb/prism)（MIT 许可证）。

## COMET

COMET（[Rei 等, 2020](https://aclanthology.org/2020.emnlp-main.213.pdf)）是一种基于大型语言模型的监督方法。作者选择了 XLM-RoBERTa，但提到像 BERT 这样的其他模型也可以与他们的方法一起使用。

与大多数其他度量标准不同，COMET 利用了源句子。因此，大型语言模型在一个三元组{翻译源句子、翻译假设、参考翻译}上进行微调。

![](img/b4b14895c18748a1c6b68369be04ea0c.png)

图由[Rei 等, 2020](https://aclanthology.org/2020.emnlp-main.213.pdf)提供

该度量标准使用人工评分（与 BLEURT 使用的相同评分）进行训练。

COMET 比 BLEURT 更容易训练，因为它不需要生成和评分合成数据。

COMET 有许多版本，包括[COMETHINO](https://aclanthology.org/2022.eamt-1.9.pdf)这种记忆占用更小的蒸馏模型。

[COMET 的发布实现](https://github.com/Unbabel/COMET)（Apache 许可证 2.0）还包括一个高效执行统计显著性测试的工具。

# 结论和建议

机器翻译评估是一个**非常活跃的研究领域**。神经指标**每年都在变得更好、更高效**。

然而，传统指标如 BLEU 仍然是机器翻译从业者的最爱，主要是由于习惯。

在 2022 年，[机器翻译会议（WMT22）发布了一份根据与人工评估相关性的评价指标排名](https://www.statmt.org/wmt22/pdf/2022.wmt-1.2.pdf)，其中包括了我在本文中介绍的指标：

![](img/b80680afbb0d0928893a0e512bfe22c8.png)

表格由[Freitag 等（2022）](https://www.statmt.org/wmt22/pdf/2022.wmt-1.2.pdf)提供

COMET 和 BLEURT 排名靠前，而 BLEU 排在底部。有趣的是，你还可以在这个表格中看到一些我在本文中没有提及的指标。其中一些，如 MetricX XXL，是没有文档记录的。

尽管有无数更好的替代方案，BLEU 仍然是迄今为止使用最广泛的指标，至少在机器翻译研究中如此。

## **个人推荐：**

当我为会议和期刊审阅科学论文时，我总是建议那些仅使用 BLEU 进行机器翻译评估的作者：

+   添加至少**一个神经指标**的结果，例如 COMET 或 BLEURT，前提是这些指标涵盖了语言对。

+   添加**chrF**（不是 chrF++）的结果。虽然 chrF 不是最先进的技术，但它明显优于 BLEU，生成的评分易于复现，并且可以用于诊断目的。
