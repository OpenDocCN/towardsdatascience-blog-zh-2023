# 开源 LLMs 的历史：更好的基础模型（第二部分）

> 原文：[`towardsdatascience.com/the-history-of-open-source-llms-better-base-models-part-two-6ca51ae74ebe`](https://towardsdatascience.com/the-history-of-open-source-llms-better-base-models-part-two-6ca51ae74ebe)

## 如何让 LLaMA、MPT、Falcon 和 LLaMA-2 使开源 LLMs 声名鹊起…

[](https://wolfecameron.medium.com/?source=post_page-----6ca51ae74ebe--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6ca51ae74ebe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ca51ae74ebe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ca51ae74ebe--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6ca51ae74ebe--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ca51ae74ebe--------------------------------) ·阅读时间 16 分钟·2023 年 11 月 18 日

--

![](img/1cad7310b9d455e5c1b8cf3ad38f5a03.png)

(照片由 [Iñaki del Olmo](https://unsplash.com/@inakihxz?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，刊登在 [Unsplash](https://unsplash.com/photos/assorted-title-of-books-piled-in-the-shelves-NIJuEQw0RKg?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash))

对大型语言模型（LLMs）的开源研究极其宝贵，因为它旨在使这一强大而有影响力的技术民主化。尽管开源 LLMs 现在被广泛使用和研究，但这一领域的研究在最初遇到了一些难以克服的困难。具体来说，开源 LLMs 最初表现不佳，并遭到了大量批评。在本概述中，我们将探讨一系列研究，这些研究通过让高性能的预训练 LLMs 对所有人开放，改变了这种局面。由于预训练语言模型的成本极高，我们将研究的模型尤其具有影响力。在这些高性能基础模型创建并发布后，许多人能够以极低的额外成本使用这些模型进行研究。

> “考虑到训练方法的表面简单性，LLMs 的能力确实非常出色。” *— 摘自 [14]*

**当前系列。** 本概述是关于开源 LLM 历史的三部分系列中的第二部分。系列中的[第一部分](https://medium.com/towards-data-science/the-history-of-open-source-llms-early-days-part-one-d782bcd8f7e8)概述了创建开源 LLM 的初步尝试。在这里，我们将研究目前可用的最受欢迎的开源基础模型（即已经预训练但未经过微调或对齐的语言模型）。下一次，我们将探讨如何微调或对齐这些模型以创建各种有用的应用。

![](img/b08d1b5626668731078252e9ca6d9c37.png)

(来源于 [10, 12, 14, 15])

# 开源 LLM 的早期日子

在本系列的第一部分中，我们看到早期的开源 LLM 研究结果提出了几个重要的基础模型，例如 OPT 和 BLOOM。然而，这些模型普遍被认为相比于闭源的预训练模型（例如 GPT-3）表现较差。*我们该如何解决这个问题？* 首先，我们需要深入了解 LLM 的训练过程。

**训练流程。** LLM 的训练分为几个步骤，如下图所示。首先，我们在大量原始文本上对模型进行预训练。然后，我们使用如 SFT 和 RLHF 等技术进行对齐。最后，我们可以进行进一步的微调或上下文学习，将 LLM 专门化为特定任务。

![](img/6f6a2c9988d315b1a2c4cccd562fdd69.png)

(由作者创建)

最近，我们已经看到有力的实证证据表明，大多数语言模型的知识是在预训练过程中获得的。对齐过程只是教会模型如何正确地格式化或展示这些在预训练过程中获得的知识。正如 LIMA [3] 所提出的，这个观点被称为“表面对齐假说”。尽管这个假说似乎与本概述的主题并不完全相关，但我们从中学到了一些重要的东西——*经过不足预训练的模型不太可能通过微调或对齐“修复”*。

> “模型的知识和能力几乎完全在预训练期间获得，而对齐则教会它在与用户互动时应使用哪个子分布格式。” *— 引自 [3]*

**解决方案是什么？** 鉴于最初的开源 LLM 性能较差，很快就明确了社区需要从头开始重新创建更高质量的基础模型，才能取得任何进展。此外，这些模型还需要在更多的数据上进行预训练，以提高其性能。由于预训练成本极高（特别是当数据量很大时），这种努力并不简单。创建更好的开源基础模型必须由拥有足够资金的组织进行（例如，[Meta](https://ai.meta.com/) 或 [MosaicML](https://www.databricks.com/company/newsroom/press-releases/databricks-completes-acquisition-mosaicml)），这些组织能够支付训练这些模型的费用，并将其免费提供给社区中的其他人。

# 朝着更好的基础模型迈进

开源 LLM 的性能最初过于差劲，不足以支撑广泛使用和探索，但这个问题很快得到了解决。在这里，我们将回顾几种通过提供强大的预训练 LLM 来改变这一局面的模型。

## LLaMA：开源质量的飞跃

LLaMA [1] 是首批发布的高性能且开源的预训练 LLM 之一。然而，LLaMA 不仅仅是一个单一的模型，而是一组不同的 LLM，其规模从 70 亿到 650 亿参数不等。这些模型在性能和推理效率之间实现了不同的权衡。尽管 LLaMA 不能用于商业用途（即仅限于研究），但它仍然是一个具有深远影响的提案，推动了开源 LLM 研究的多个方向。

![](img/a63c11bbd093661d57e7102568ebf623.png)

（来自 [1]）

**数据。** 受到 Chinchilla [2] 的启发，LLaMA 模型在一个包含超过 1.4 万亿个文本标记的语料库上进行预训练。这个预训练数据集明显大于任何之前的开源 LLM。数据的来源和分布如上所示。有趣的是，LLaMA 完全使用公开可用的数据源进行预训练，这意味着任何拥有足够计算资源的人都可以复制整个预训练过程。

> “GPT-4 从各种许可的、创造的和公开可用的数据源中学习，这些数据源可能包括公开可用的个人信息。” *— 引自 GPT-4 博客*

鉴于许多专有的大型语言模型（LLMs）是使用内部数据进行训练的，而这些数据并不公开，因此这种特性尤为可取。简单来说，LLaMA 是在多个方面朝着更高透明度和开放性迈出的重要一步。

![](img/5f5d62aa5ade1ff73497a211e3c60034.png)

（来自 [1]）

**性能提升。** 与其前身相比，*LLaMA 在开源 LLM 的性能上有了巨大的飞跃*。尽管如此，其质量仍落后于顶级专有 LLM（例如，[ChatGPT](https://openai.com/blog/chatgpt) 或 [GPT-4](https://openai.com/research/gpt-4)），但我们应当记住 LLaMA 模型尚未进行对齐。值得注意的是，LLaMA-13B 的性能与 GPT-3 [3] 相当，而 LLaMA-65B 在多个情况下超越了 PaLM [4]，这表明 LLaMA 套件的性能与其他广泛使用的基础模型相当。详细的指标见上表。

![](img/7abf77a393f0ee4b2c1d597cfd0abff3.png)

（来自 [5, 6, 7, 8]）

**开源爆发。** LLaMA 提案中最有趣的方面之一是随之而来的开源 LLM 研究浪潮；见上文。在 LLaMA 模型的权重公开后，开源研究社区迅速开始发布各种不同的模型变体和软件包。这些进展包括从 LLaMA 的微调版本到一个用于在笔记本电脑上高效运行任何 LLaMA 模型的 C++ 库。这些进展真正展示了研究开放性的美好。*我们仅仅用了几周时间，就从通过 API 互动这些强大的模型，转而在我们的笔记本电脑上运行它们！*

## MPT：高质量、商业化和开源的 LLM

![](img/75283179a1c900a72ba34c79e7f90e3c.png)

（来自 [10]）

尽管 LLaMA 令人印象深刻，但该套件中的模型都无法用于商业应用——*它们仅从研究的角度具有价值*。幸运的是，LLaMA 的提案很快被 MosaicML 开发并发布的商业可用（即，按照 [Apache 2.0 许可](https://www.planetcrust.com/what-does-apache-2-0-license-mean) 发布）的 MPT 套件所跟进。首先发布的是 MPT-7B [9]，它引起了大量关注（即，它基本上是 LLaMA-7B 的商业可用替代品！）。实际上，在更大的 MPT-30B [10] 模型发布之前，MPT-7B 在 [HuggingFace](https://huggingface.co/mosaicml/mpt-7b) 上的下载量超过了 300 万次！

![](img/6667da3f4b5edca42345e89e4ce1ff88.png)

（来自 [9, 10]）

这两个模型之间的主要区别是：

1.  它们使用略有不同的数据混合进行预训练；见上文。

1.  MPT-30B 采用了更长的上下文长度，达到 8K 令牌。

然而，这些模型都表现出色，并可以用于商业应用，这使它们在人工智能社区中变得非常受欢迎。

![](img/f6ddad759e3ef2bc8b2c5ed5be03ed9b.png)

（来自 [9]）

**MPT 是否符合炒作？** 尽管 LLaMA 显著提升了开源 LLM 的最先进性能，但 MPT 套件与此性能不相上下。特别是，MPT-7B 在各种标准基准测试中与 LLaMA-7B 的表现相匹配；见上文。更进一步，MPT-30B 的表现趋向于与 GPT-3 相当。与同样大小的开源模型（例如 LLaMA-30B 和 Falcon-40B）相比，MPT-30B 的表现略逊一筹；见下文。然而，它在与编码相关的任务中优于这些模型，并且可以在单个 GPU 上运行（通过量化）。

![](img/acc9091d4b79f5f783ce5ece777d65cf.png)

（来自 [10]）

**MPT 变体。** 除了预训练的 MPT-7B 和 MPT-30B 模型，还发布了各种微调的 MPT 模型，例如 [instruct](https://huggingface.co/mosaicml/mpt-30b-instruct) 和 [chat](https://huggingface.co/mosaicml/mpt-30b-chat) 版本的 MPT 模型。此外，通过对具有 64K token 上下文长度的数据进行微调，还创建了 “StoryWriter” 版本的 MPT-7B。鉴于预训练 LLM 比微调要昂贵得多，因此可以以边际成本创建各种不同的微调 MPT 变体；见下文。

![](img/cadb7f5b56f274d1a89097a7a98f06f5.png)

（来自 [9]）

**但等一下…还有更多！** MPT 模型非常有用（尤其是对于从事商业应用的人），但这些模型还伴随着一整套软件（即，[LLM 工厂](https://github.com/mosaicml/llm-foundry)），由 MosaicML 发布。这些开源代码可以用于预训练和微调 MPT 模型，使 MPT 套件成为探索 LLM 特殊用例的极其宝贵的工具。

## Falcon：在开源性能方面达到新高度

![](img/ad0740575bb930c8ee0391e4195bac92.png)

（来自 [1]）

尽管在开源 LLM 领域取得了许多进展，但可用的模型在性能上仍然落后于专有 LLM 一段时间。Falcon 套件的提出 [11] 是开源替代方案首次真正与专有 LLM 的质量相匹敌。Falcon 提供了两种变体 — Falcon-7B 和 Falcon-40B。除了商业许可外，这些 Falcon 模型由于在大规模、自定义策划的语料库上进行预训练，表现非常出色。值得注意的是，Falcon-40B 的 instruct 变体在 [OpenLLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) 上连续几个月是表现最佳的模型（差距显著）。

> “挑战现有的数据质量和 LLM 信念，仅凭经过适当筛选和去重的网络数据训练的模型可以匹敌使用策划数据训练的模型。” *— 来自 [12]*

**从网络中策划数据。** Falcon 模型是在一个名为 RefinedWeb [12] 的大规模文本语料库上训练的，该语料库包含超过 5 万亿个标记的文本。实际上，仅有 1.5 万亿个标记和 1 万亿个标记的 RefinedWeb 被用于分别预训练 Falcon-7B 和 Falcon-40B。尽管大多数 LLM 是在公开策划的数据源上进行预训练的，但 Falcon 的作者选择仅使用来自网络的数据（即 [CommonCrawl](https://commoncrawl.org/)）构建自己的预训练数据集。为筛选这些数据，创建了一个新的流程，强调简单但有效的组件；见下文。

![](img/341b3a3c1bb2512198d79709973a53bf.png)

(来自 [12, 13])

RefinedWeb 语料库表明，可以从网络中高效策划出大量高质量的文本数据——*超出了以前探索的数据集的规模*。在应用过滤后，基于这些数据训练的模型甚至可以超越在策划数据源上训练的类似模型。

![](img/40ccf29d0c7358a4dd95dd7527665ac0.png)

(来自 [12])

用于训练 Falcon-7B 和 Falcon-40B 的确切数据集如上所示。值得注意的是，Falcon-7B 仅在英语数据上进行训练，而 Falcon-40B 的预训练集中则包含了各种欧洲语言的数据。

**新的 SOTA。** 目前尚未发布关于 Falcon LLM 的任何出版物。因此，对这些模型的唯一正式评估是通过 OpenLLM 排行榜进行的，其中 Falcon-40B 模型表现相当不错。特别是，[Falcon-40B-Instruct](https://huggingface.co/tiiuae/falcon-40b-instruct) 曾是领先的模型，显著超越其他模型；见下文。

![](img/61a93b643212a4d71d7eb1866dc9bb15.png)

(来自 Open LLM 排行榜)

从质量上看，一些从业者声称 Falcon-40B 似乎不如 LLaMA 基础模型。尽管了解这些评论是有用的，但这些证据是轶事性和主观的。在标准化的自然语言基准测试中，Falcon LLM 的表现非常出色，使其在开源模型中长时间保持最先进的性能。

## LLaMA-2: 当前的最先进技术

![](img/e4a98130bd933b03c24a04b294d44d6a.png)

(来自 [14])

尽管 Falcon-40B 曾是领先的开源 LLM，但最近发布的 LLaMA-2 模型套件已使该模型失去领先地位。与 LLAMA-1 类似，LLaMA-2 [14] 包含多个不同的 LLM，其规模从 70 亿到 700 亿参数不等，并仅使用公开数据进行预训练。LLAMA-2 模型既有预训练版本也有微调版本[6](https://cameronrwolfe.substack.com/p/the-history-of-open-source-llms-better#footnote-6-135439692)，但由于我们专注于开源基础模型，本文仅涵盖预训练模型。

> “已经有公开发布的预训练 LLMs（例如 BLOOM，其性能与封闭预训练的竞争对手如 GPT-3 和 Chinchilla 相当，但这些模型都不适合作为封闭产品 LLMs 的替代品，例如 ChatGPT、BARD 和 Claude。” *— 来自 [14]*

LLaMA-2 通过发布一套在大规模数据集上进行预训练的更高性能基础模型，继续缩小开源和闭源语言模型之间的性能差距。正如我们将看到的，这些模型仍未达到专有模型的质量，但比之前的任何开源模型都要接近得多。

![](img/b801cdd176fb57461ca9cb6f109e2535.png)

（来自 [14]）

**它有什么不同？** LLaMA-2 采用了一种与其前身非常相似的方法，除了几个小的（但影响深远的）区别。首先，LLaMA-2 模型在超过 40%更多的数据上进行预训练——总共 2 万亿个标记，而 LLaMA-1 为 1.4 万亿个标记。此外，LLaMA-2 模型使用稍长的上下文长度进行训练，较大的模型在其底层架构中使用了分组查询注意力（GQA）。有趣的是，[14]中的作者指出，LLaMA-2 的预训练设置采样了更多已知更具知识性的来源。这样的变化旨在强调事实来源、增加知识和减少幻觉。

![](img/d5d43f1550c7ee0b23a708140d768fdc.png)

（来自 [15]）

**什么是 GQA？** 如[15]所提出，GQA 是对多头自注意力的修改，可以提高 LLMs 的推理效率。典型的多头自注意力机制有`N`个查询、键和值头，总共创建`N`个自注意力头。在 GQA 中，我们将这些`N`个头分成组，其中键和值头在每组内共享；见上文。这种方法是在普通多头自注意力和多查询注意力之间的插值，后者在所有`N`个头中使用共享的键和值投影。[15]发现，GQA 在提高推理速度方面与多查询注意力相当，同时保持普通多头注意力的性能。

![](img/b5c3329f9503ea48e5ab203aa4479509.png)

（来自 [14]）

**LLaMA-2 确实很出色。** 与流行的开源模型（例如 MPT、Falcon 和 LLaMA-1）相比，LLaMA-2 基础 LLMs 表现非常好。事实上，LLaMA-2–70B 在所有考虑的任务中设立了开源 LLMs 的新最佳水平；见上文。然而，值得注意的是，LLaMA-2 因在编码任务上的（相对）较差表现而受到一些批评（例如，[HumanEval](https://arxiv.org/abs/2107.03374)）。

![](img/6db4644805a2095441473a7763fb7247.png)

（来自 [14]）

与专有模型相比，LLaMA-2 基础模型的表现较差；见上文。然而，我们应当记住，这一比较是在基础 LLM 与像 GPT-3.5 和 GPT-4 这样的对齐模型之间进行的。与其他流行的基础 LLM（例如，PaLM [4]）相比，LLaMA-2 的表现是有利的！

**商业许可证。** 尽管 LLaMA-1 仅可用于研究，LLaMA-2 则在[商业许可证](https://github.com/facebookresearch/llama/blob/main/LICENSE)下发布，这意味着— *与 MPT 和 Falcon 类似* — 这些模型可以用于商业应用。然而，LLaMA-2 的许可证并非标准的 Apache 2.0 许可证，它有一些需要从业者考虑的附加条件。最值得注意的是，任何由 LLaMA-2 驱动、每月活跃用户超过 7 亿的实体/应用程序必须从 Meta 处获得使用 LLaMA-2 的许可证。有关 LLaMA-2 许可证的更多信息，请参阅[这里](https://opensourceconnections.com/blog/2023/07/19/is-llama-2-open-source-no-and-perhaps-we-need-a-new-definition-of-open/)。

# 开源 LLM 的趋势

鉴于 LLaMA、MPT、Falcon 和 LLaMA-2 的表现远超其前身，我们可以合理地问：*是什么导致当前一代开源 LLM 表现如此出色？* 在这里，我们将快速查看这些模型的一些关键特性，这些特性在推动它们的卓越表现和迅速流行中尤为重要。特别是，这些模型 *i)* 在大量数据上进行了预训练，以及 *ii)* 强调推理效率。

## 更好的数据 = 更好的表现！

当前开源 LLM 与之前的 LLM 之间的关键区别在于用于预训练的数据集。尽管像 OPT 和 BLOOM 这样的模型分别在 1800 亿和 3410 亿个标记上进行训练，但当前开源模型是在显著更大的数据集上进行预训练的：

+   *LLaMA*: 1.4 万亿个标记

+   *MPT*: 1 万亿个标记

+   *Falcon*: 1–1.5 万亿个标记

+   *LLaMA-2*: 2 万亿个标记

当前的开源 LLM 将用于预训练的数据量提高了（几乎）一个数量级！事实上，这些预训练数据集的规模与用于专有 LLM 的数据集相似。例如，MassiveText（即用于训练 Gopher [13]和 Chinchilla [2]）包含大约 2.3 万亿个标记，尽管实际上仅使用了其中的一部分进行预训练；见下文。

![](img/5dad8fba9cf56bcd2c7a8db9376f26af.png)

（来源 [13]）

**规模并非一切！** 除了显著增加预训练数据的数量，当前的开源 LLM 还非常关注数据的组成和质量。例如，在用于训练 MPT 的数据集中，代码的比例得到了增加，从而使得生成的模型在基于编码的任务上表现更佳。此外，Falcon-40B 提出了一种全新的从网络构建高质量文本语料库的管道，而 LLaMA-2 则声称使用了更新的数据管道和混合方式进行预训练。总的来说，专注于预训练数据集的质量和组成似乎是近期开源 LLM 研究中的一种共同趋势。

> “我们进行了更全面的数据清理，更新了数据混合，训练了更多的总标记，增加了上下文长度，并使用了分组查询注意力（GQA）以提升我们较大模型的推理可扩展性。” *— 来源于 [14]*

## 优化以实现更快的推理

在决定使用开源或闭源 LLM 时，实践者必须考虑的不仅仅是性能。付费语言模型 API 可能在广泛的任务范围内取得令人印象深刻的表现，但它们往往无法在特定领域的数据上进行微调。另一方面，使用开源 LLM 构建应用时，一个主要考虑因素是部署模型的成本。鉴于托管 LLM 的难度，近期的开源模型往往经过优化，以实现快速和简便的推理。实际上，MPT-30B [10] 特别调整了尺寸，以便可以在单个 GPU 上托管！

![](img/bb4a8b793ab705597715a58ce57e7250.png)

（来源于 [15, 16, 17]）

**修改后的架构。** 除了比大多数专有模型稍小之外，当前的开源 LLM 还采用了多种架构技巧 —— 如上图所示 —— 来加速推理过程，例如：

+   低精度层归一化

+   快速注意力

+   多查询注意力

+   并行变换器

+   组查询注意力

此外，还采用了多种其他架构修改，例如 RoPE 嵌入、ALiBi、SwiGLU 激活函数等，以提升性能。当前的开源 LLM 对仅有解码器的变换器架构进行了简单的修改，以改善性能和推理速度。

# 结论

在本概述中，我们研究了开源 LLM 的演变，从初期的低质量模型（例如 BLOOM 和 OPT）到最近的强大基础模型（例如 LLaMA 和 MPT）。为了提升前辈模型的性能，这些最近的模型主要集中于策划更大、更高质量的预训练数据集，这导致了质量的显著提高。鉴于高质量的基础模型是任何 LLM 应用的前提，这些模型对开源 LLM 的流行程度产生了重大影响。如今，任何从业者都可以利用强大的基础 LLM，无论是用于研究目的还是商业应用，而无需投入大量资金从头开始预训练模型。

## 与我联系！

非常感谢阅读本文。我是 [Cameron R. Wolfe](https://cameronrwolfe.me/)，[Rebuy](https://www.rebuyengine.com/) 的 AI 总监。我研究深度学习的实证和理论基础。如果你喜欢这个概述，请订阅我的 [Deep (Learning) Focus 时事通讯](https://cameronrwolfe.substack.com/)，在这里我帮助读者通过从基础到高级的相关主题概述来理解 AI 研究。你也可以在 [X](https://twitter.com/cwolferesearch) 和 [LinkedIn](https://www.linkedin.com/in/cameron-r-wolfe-ph-d-04744a238/) 上关注我，或者查看我在 medium 上的 [其他文章](https://medium.com/@wolfecameron)！

## 参考书目

[1] Touvron, Hugo 等人。“Llama: 开放且高效的基础语言模型。” *arXiv 预印本 arXiv:2302.13971* (2023)。

[2] Hoffmann, Jordan 等人。“训练计算最优的大型语言模型。” *arXiv 预印本 arXiv:2203.15556* (2022)。

[3] Zhou, Chunting 等人。“Lima: 对齐的‘少即是多’。” *arXiv 预印本 arXiv:2305.11206* (2023)。

[4] Chowdhery, Aakanksha 等人。“Palm: 使用路径扩展语言建模。” *arXiv 预印本 arXiv:2204.02311* (2022)。

[5] Taori, Rohan 等人。“斯坦福 Alpaca: 一种遵循指令的 LLaMA 模型。” (2023)。

[6] Chiang, Wei-Lin 等人。“Vicuna: 一个以 90%* ChatGPT 质量给 GPT-4 留下深刻印象的开源聊天机器人。” (2023)。

[7] Geng, Xinyang 等人。“Koala: 一种用于学术研究的对话模型。” (2023)。

[8] Yuvanesh Anand, Zach Nussbaum, Brandon Duderstadt, Benjamin Schmidt 和 Andriy Mulyar。“GPT4All: 通过从 GPT-3.5-Turbo 大规模数据蒸馏训练助理风格聊天机器人。” 2023 年。

[9] “介绍 MPT-7B: 开源、可商业化 LLM 的新标准。” *MosaicML*, 2023 年 5 月 5 日, [www.mosaicml.com/blog/mpt-7b.](http://www.mosaicml.com/blog/mpt-7b.)

[10] “MPT-30B: 提高开源基础模型的标准。” *MosaicML*，2023 年 6 月 22 日，[www.mosaicml.com/blog/mpt-30b.](http://www.mosaicml.com/blog/mpt-30b.)

[11] “介绍 Falcon LLM”， *技术创新研究所*，2023 年 6 月 7 日，[`falconllm.tii.ae/.`](https://falconllm.tii.ae/.)

[12] Penedo, Guilherme 等人. “Falcon LLM 的 RefinedWeb 数据集：通过网络数据超越精选语料库，仅使用网络数据。” *arXiv 预印本 arXiv:2306.01116* (2023)。

[13] Rae, Jack W. 等人. “语言模型扩展：方法、分析与从训练 Gopher 中获得的见解。” *arXiv 预印本 arXiv:2112.11446* (2021)。

[14] Touvron, Hugo 等人. “Llama 2: 开放基础与微调聊天模型。” *arXiv 预印本 arXiv:2307.09288* (2023)。

[15] Ainslie, Joshua 等人. “GQA: 从多头检查点训练通用多查询变换器模型。” *arXiv 预印本 arXiv:2305.13245* (2023)。

[16] Vaswani, Ashish 等人. “注意力即你所需。” *《神经信息处理系统进展》* 30 (2017)。

[17] Dao, Tri 等人. “Flashattention: 快速且内存高效的精确注意力机制，具备 IO 感知功能。” *《神经信息处理系统进展》* 35 (2022): 16344–16359。

[18] Dao, Tri. “FlashAttention-2: 更快的注意力机制，具有更好的并行性和工作分区。” *arXiv 预印本 arXiv:2307.08691* (2023)。
