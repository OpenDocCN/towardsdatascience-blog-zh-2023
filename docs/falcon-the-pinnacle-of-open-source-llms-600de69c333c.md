# 鹰：开源大型语言模型的巅峰

> 原文：[`towardsdatascience.com/falcon-the-pinnacle-of-open-source-llms-600de69c333c`](https://towardsdatascience.com/falcon-the-pinnacle-of-open-source-llms-600de69c333c)

## 开源 LLMs 与专有 LLMs 之间的差距持续缩小…

[](https://wolfecameron.medium.com/?source=post_page-----600de69c333c--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----600de69c333c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----600de69c333c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----600de69c333c--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----600de69c333c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----600de69c333c--------------------------------) ·阅读时间 14 分钟·2023 年 10 月 24 日

--

![](img/4a5506c6e87e01bb8145fcb5b5d29b13.png)

（照片由 [Alan Mersom](https://unsplash.com/@merse?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/aerial-photography-of-bird-NzkjR1pw0Lc?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)）

最近，开源大型语言模型（LLMs）的研究主要集中在两个方面：模仿学习和预训练开源基础模型。虽然这两种方法都是可行的，但创建高质量的开源基础模型尤其令人振奋，因为这些模型可以进一步微调（成本较低）并用于各种不同的下游应用。最初尝试创建这些模型的结果并不理想。尽管后来的模型（例如，LLaMA 和 MPT-7B）表现更佳，但这些模型在质量上仍难以匹敌其专有对手（例如，GPT-3.5 或 GPT-4），直到最近才有所改进。

随着 Falcon-7B 和 Falcon-40B LLMs [1] 的发布，我们*第一次*看到开始与最受欢迎的付费模型质量相媲美的开源基础 LLMs。这些模型在通过新颖的数据管道获得的大规模文本语料库上进行训练，取得了（以相当大的优势）新的开源 LLMs 的最先进性能，并且可以自由用于商业应用。更棒的是，Falcon 模型对其底层的 Transformer 架构进行了若干修改，这些修改显著加快了推理速度，甚至可以提高预训练的效率。

![](img/c9e99a0dc680837e42c6f23d65dcf34e.png)

（来自 [1, 2]）

**大局观。** 创建一个 LLM 的过程包含几个步骤；见下文。这个过程的第一步（即，获得预训练基础模型）被广泛认为是最昂贵的，无论是在金钱还是时间上。

![](img/1827c9fb6ce35c89268c94f997d784e3.png)

创建和完善 LLM 的多步骤过程（来自 [16, 17]）

这些模型之前被保留在专有 API 后面，但开源 LLM 的进步使高性能基础 LLM 更加公开。Falcon 是这个类别中的另一个模型，相比其他开源替代品，它达到了前所未有的性能水平。

# 使用 Web 数据进行 LLM 预训练

![](img/fdc2d6a1345e149a2f4922d4b913e892.png)

（来自 [3]）

当我们探讨预训练与微调（即，SFT 和 RLHF）语言模型之间的主要差异时，会发现预训练比微调要困难得多（且更昂贵）；见上文。预训练有两个根本性特征使其如此困难：

1.  模型是从头开始训练的，因此需要更多的训练迭代次数。

1.  预训练数据集必须大且多样化（即，提供尽可能多的“覆盖”），以便生成的 LLM 拥有较大的知识基础。

简而言之，预训练数据集非常庞大（例如，Chinchilla [6] 在 1.4 万亿文本令牌上训练），这意味着预训练的范围不可避免地很大。*我们必须进行大量的训练迭代才能遍历所有这些数据！*

**创建预训练数据集。** 然而，数据集的大小不仅是使预训练成为如此庞大任务的原因。仅仅策划数据集就是一个复杂的过程，涉及到检索数据和执行整个过滤（例如，基于数据质量、污染数据、PII 等）和去重步骤的管道。已经提出并探索了各种不同的处理步骤来策划 LLM 预训练数据；见 [这里](https://wandb.ai/wandb_gen/llm-data-processing/reports/Processing-Data-for-Large-Language-Models--VmlldzozMDg4MTM2)。

尽管最初可能认为这些处理步骤可以简化或避免，但 LLM 研究一次又一次地向我们展示了模型训练数据的质量是极其重要的。例如，我们可以看到 LIMA [7] 或 Galactica [8]，这两个模型都在较小（但高质量）的文本语料库上训练，结果与在更大规模的噪声数据集上训练的相同模型的性能相匹配或超越。

![](img/8ee5c6fb504267cab8295ea5e9d225a3.png)

（来自 [4, 5]）

**当前的 LLM。** 由于数据质量对模型质量的影响，大多数 LLM 使用的预训练数据来自高度策划的来源，例如筛选过的文本内容、书籍、代码或技术报告；见上文 [4, 5]。实际上，许多策划过的预训练数据公共来源在线上随处可见（例如，[the Pile](https://huggingface.co/datasets/EleutherAI/pile) 或 [C4](https://huggingface.co/datasets/c4)），并且已经被现有模型广泛使用。

> “策展被认为是产生高性能模型所必需的……然而，随着模型需要在万亿个标记上进行预训练，尚不清楚策展是否具有可扩展性，以及我们是否会很快耗尽独特的高质量数据。” *— 来源于 [2]*

然而，使用策展数据源是否真的具有可扩展性仍然值得怀疑——*随着预训练数据集规模的扩大，细粒度的过滤和策展变得越来越困难*。因此，对于较大的模型和数据集，广泛的策展可能变得不再那么必要，特别是考虑到为 LLM 预训练找到足够的数据变得越来越[困难](https://epochai.org/blog/will-we-run-out-of-ml-data-evidence-from-projecting-dataset)。

## RefinedWeb: 可扩展的网页文本策展

![](img/e14fa28eebf85cd442c6f973f4bbdfea.png)

（来自 [2]）

鉴于这些局限性，Falcon [1]的作者探讨了可扩展和高效的数据策展方法，这些方法可以推广到大量数据。用于创建[RefinedWeb](https://huggingface.co/datasets/tiiuae/falcon-refinedweb)预训练数据集的完整数据策展流程，Falcon-7B [10]和 Falcon-40B [11]即在该数据集上进行预训练，详细描述见[2]。RefinedWeb 由经过简化过滤流程的网页数据组成，可以用于训练比在策展数据源上训练的类似模型性能更优的模型；见上文。这一发现表明，大规模训练语料库可以有效地从专门从互联网获得的数据中创建（而不是策展数据源）。

> “挑战了对数据质量和 LLMs 的现有信念，单独使用经过适当过滤和去重的网络数据训练的模型可以达到与训练在策展数据上的模型相当的性能。” *— 来源于 [2]*

**简化的策展流程**。用于 Falcon 的预训练数据集基于[Common Crawl](https://commoncrawl.org/)。与之前的工作相比，[2]中的作者通过强调规模来区分他们的数据策展流程，*旨在从网络中生成一个包含 3-6 万亿个数据标记的预训练语料库*。这远远超过了之前工作的数据集——即使是 MassiveText（即用于预训练 Gopher [9]和 Chinchilla [6]的语料库）也仅包含 2.3 万亿个标记的文本。而且，现有模型在预训练过程中仅使用了这些数据的一个子集。

![](img/a24030f337f8654514e93e3a46049486.png)

（来自 [2]）

[2] 中构建的语料库包含大约 5 万亿个仅含英语的数据标记，完全从网络上获取；见上文。尽管数据规模庞大，作者在构建过程中采用了严格的去重策略，这一策略以较高的速度去除准确的和 [模糊重复](https://a-laymans-guide-to-fuzzy-document-deduplication-a3b3cf9a05a7)。在这种去重之外，只进行了最小的额外过滤。实际上，除了通过 [FastText 分类器](https://ai.facebook.com/blog/introducing-many-to-many-multilingual-machine-translation/) 进行语言识别外，没有进行其他基于机器学习的过滤。

![](img/31969e7e6acb9ce3ab8fbd6f47f59b30.png)

（来自 [9]）

除了过滤非英语文档，[2] 中的作者还使用了几个简单的启发式方法来过滤不需要的内容，例如：

+   从与黑名单关联的 URL 中过滤内容

+   使用 [trafilatura](https://trafilatura.readthedocs.io/en/latest/) 从网页中提取内容

+   定义简单规则来识别和过滤 PII

此外，采用了 MassiveText [9] 过滤管道中的几个步骤；见上文。RefinedWeb 的完整策划管道移除了原本从 CommonCrawl 获得的近 90% 数据，但在完全过滤后的语料库中仍保留了超过 5 万亿个标记的文本数据；见下文。此外，这个数据集是“多模态友好的”，因为它包含了指向图片的链接和替代文本。

![](img/da2f19496120c661797566a662f0ffd2.png)

（来自 [2]）

**开源预训练数据。** 生成的 RefinedWeb 数据集非常庞大，表明互联网上有足够的数据可以进行前所未有规模的 LLM 预训练。换句话说，*我们并没有（至少暂时没有）数据短缺*。然而，[2] 中的作者仅公开了该语料库中一个小的 600B 标记子集（即 12% 的数据）。尽管其规模较小，但这个公开的 RefinedWeb 子集仍然是任何从事 LLM 工作的实践者有用的预训练数据来源。事实上，在 [2] 中，基于这些数据训练的小型 Falcon 变体相比于在策划语料库上训练的模型表现更为优越；详见下文。

![](img/37ba0534f99ae86567287b7b74d09c5c.png)

（来自 [2]）

# Falcon 系列 LLM

![](img/0a83f0e07ea8dde2417e6773023d7c8d.png)

（来自 TII Falcon 网页 [1]）

Falcon 系列 LLM [1]，包括 Falcon-7B [10] 和 Falcon-40B [11]，在开源模型中实现了最先进的性能。此外，这些模型的指令调优变体，如 [Falcon-40B-Instruct](https://huggingface.co/tiiuae/falcon-40b-instruct)（即 HuggingFace 的 [Open LLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) 上的顶级模型），在公共基准测试中表现更佳。正如我们将看到的，这些模型具有几个关键特性（例如，数据、性能和推理速度），使其独特且实际有用。

**商业许可证。** 起初，Falcon 模型以一种特殊的许可证发布，该许可证要求在模型（或其任何衍生物）用于商业应用时支付版税。然而，在这一初始发布之后不久，该许可证被修改为普通的 [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) 许可证，这意味着 Falcon 基础模型现在可以在商业应用中免费使用！与其他开源和可商业使用的 LLMs（即使是 MPT-30B）相比，Falcon-40B 实现了独特的卓越性能。

## Falcon-7B 和 40B 数据集

![](img/2e7873095bb9719b857400439db4124d.png)

（来自 [10, 11]）

如前所述，数据集 [2] 被用于对开源 Falcon-7B/40B 模型进行预训练。然而，这些模型仅在完整的 5 万亿标记语料库的一个子集上进行预训练（即 Falcon-7B 为 1.5 万亿标记，Falcon-40B 为 1 万亿标记）。然而，这个语料库的子集通过额外的、精心挑选的数据（如书籍、代码和技术内容）进行了增强；见上文。由于其更大的规模，Falcon-40B 的训练数据量少于 Falcon-7B。尽管如此，较大的模型仍然表现更好，训练时间超过两个月，而 Falcon-7B 仅需两周。

**多语言 LLMs。** RefinedWeb 语料库仅包含英文数据，而 Falcon-7B 仅使用英文文本进行训练。然而，Falcon-40B 的预训练集中增加了 RefinedWeb-Europe 语料库，该语料库包含来自各种常见欧洲语言的文本数据。尽管这些数据仅占预训练语料库的 7%，但它将少量多语言数据注入到模型的知识库中，使其在需要基本多语言理解的公共基准测试中表现更佳。

## Falcon 架构

Falcon-7B 和 Falcon-40B 模型都使用了修改过的解码器-only transformer 架构。对该架构的修改包括：

+   Flash Attention

+   RoPE 嵌入

+   Multi-Query Attention

+   并行注意力和前馈层

这些修改（其中一些与 MPT 套件的 LLMs 共享）极大地提高了 Falcon 的推理速度。实际上，Falcon-40B 的推理速度是 GPT-3 的 `5X`。此外，由于这些修改，预训练 Falcon-40B 的成本较低；例如，Falcon-40B 需要 75% 的 GPT-3 [4] 计算预算，40% 的 Chinchilla [6] 计算预算和 80% 的 PaLM-62B [5] 计算预算。Falcon-7B 和 Falcon-40B 的训练序列长度为 2K 标记，相较于最近的 LLMs（如 [StoryWriter](https://www.mosaicml.com/blog/mpt-7b) 和 [Claude](https://www.anthropic.com/index/100k-context-windows)）可以说较小。

![](img/3f28168ffe93c805f9bbc83ced80fccc.png)

（来自 [10, 11]）

Falcon-7B/40B 共享相同的模型架构，但 40B 变体稍微更深（即 60 层对比 32 层）且具有更高维度的隐藏层；见上文。使用 Falcon-40B 需要约 90Gb 的内存，这比类似的模型（如 LLaMA-65B）有更低的开销。然而，Falcon-40B 仍不能像 MPT-30B 一样在单个 GPU 上托管。鉴于其较小的规模，Falcon-7B 仅需约 15Gb 的内存，使其在推理和微调中更具可及性。

**RoPE 嵌入。** 正如我们在之前的概述中看到的，自注意力操作（该操作在语言模型的解码器单一转换器架构的每一层中实现）并不会自然地考虑序列中每个标记的位置。因此，我们必须为每个标记注入位置信息（例如，通过加性位置嵌入）到此操作中；见下文。

![](img/6ec75d8d03d246b5921c190064e5acbf.png)

转换器的加性位置嵌入（由作者创建）

提出了几种位置嵌入变体——这些变体可能在训练过程中学习每个嵌入，也可能不学习，包括绝对嵌入和相对嵌入。然而，旋转位置嵌入（RoPE）[15]是一个将每个标记的绝对位置（即序列中的全局位置）和相对位置（即基于标记之间的距离定义位置）结合到自注意力中的替代方案，如下所示：

1.  用[旋转矩阵](https://en.wikipedia.org/wiki/Rotation_matrix)编码绝对位置

1.  将相对位置信息直接添加到自注意力操作中

这种方法被发现能够在绝对位置和相对位置信息之间取得平衡，这对需要较长序列长度的任务尤其有利。因此，RoPE 嵌入最近获得了广泛关注，导致其被用于如 PaLM [5]等模型中。有关更详细的概述，请查看 RoPE 嵌入的[这里](https://blog.eleuther.ai/rotary-embeddings/)。

**多查询注意力。** Falcon 模型用一种称为多查询注意力的替代结构替换了典型的多头自注意力操作。多查询注意力仅在每层的注意力头之间共享键和值向量（如下图红色高亮部分）。所有头部共享相同的键的投影矩阵和相同的值的投影层，而不是为每个头部执行单独的投影。尽管这一变化并未加快训练速度，但显著提高了最终 LLM 的推理速度。

![](img/039a767ac21dfcdc2004b48b3619e68c.png)

LLM 中的多查询注意力（来自[18]）

**并行注意力层。** 最后，Falcon 模型在其架构的每一层结构中做出了一项基本变化。与“串行”解码器仅限变体的层不同，Falcon 模型的每一层都并行执行自注意力和前馈变换，然后进行单层归一化操作。这个公式与标准的变换器块之间的差异如下所示。有趣的是，这种并行公式并没有降低模型的性能。然而，由于变换器层的两个主要操作是并行进行的，因此它可能在推理速度上带来好处。

![](img/86b1c65401579465e9f86b1d664926c1.png)

解码器仅限的变体层（由作者创建）

## 开源语言模型的新标准！

在撰写时，关于 Falcon 模型的手稿尚未发布。然而，Falcon-7B 和 Falcon-40B（以及它们的指令调优变体）已通过 [Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) 进行评估，该榜单包括多个基准测试，例如：

+   ARC [[链接](https://huggingface.co/datasets/ai2_arc)]

+   HellaSwag [[链接](https://huggingface.co/datasets/hellaswag)]

+   MMLU [[链接](https://huggingface.co/datasets/lukaemon/mmlu)]

+   TruthfulQA [[链接](https://huggingface.co/datasets/truthful_qa)]

通过该榜单进行的评估是不完整和初步的。然而，这些评估虽然在合理范围内捕捉了模型性能，但清楚地显示了 Falcon-40B 是当前开源语言模型的最先进技术；见下文。

![](img/6ff26ede567d2242b1804df2484e9bae.png)

（来自 Open LLM Leaderboard）

Falcon-40B 的指令变体（即 [Falcon-40B-Instruct](https://huggingface.co/tiiuae/falcon-40b-instruct)），它在来自 [Baize](https://github.com/project-baize/baize-chatbot) 的数据混合上进行了指令调优，远远超越了各种其他开源模型。此外，预训练的 Falcon-40B 模型表现也相当不错，甚至优于像 LLaMA-65B 和 MPT-30B 这样的著名基础模型。进一步说，Falcon-40B 也可用于商业用途，而榜单上的许多可比模型（例如 LLaMA [13]、[Guanaco](https://huggingface.co/timdettmers/guanaco-65b) [14] 和 [Lazarus](https://huggingface.co/CalderaAI/30B-Lazarus)）仅可用于研究目的。

**Falcon 的实际使用。** 鉴于其性能非常出色，与其他语言模型相比托管起来轻量（由于改进的推理速度），并且可以在商业应用中自由使用，Falcon LLMs 是任何从事 LLM 工作的实践者的重要开源工具。幸运的是，已经撰写了几篇详细的概述，概述了在实践中微调和托管/部署这些模型的有用框架。

+   在 AWS Sagemaker 上部署 Falcon-40B [[link](https://aws.amazon.com/blogs/machine-learning/deploy-falcon-40b-with-large-model-inference-dlcs-on-amazon-sagemaker/)]

+   使用 Falcon 进行推理和参数高效微调 [[link](https://huggingface.co/blog/falcon)]

+   使用 PyTorch Lightning 对 Falcon-40B 进行微调 [[link](https://lightning.ai/blog/falcon-a-guide-to-finetune-and-inference/)]

鉴于 Falcon 使用 AWS 进行训练，目前有相当数量的解释性文章介绍如何在类似硬件上部署和训练这些模型。这些文章为任何希望在自己的用例中利用 Falcon 的人提供了一个良好的起点。

# 最终思考

Falcon 的发布是开源 LLM 研究和应用的重大突破。当我们审视这些模型的独特贡献时，我们立即看到一些关键组件，这些组件导致了成功：

1.  大规模预训练数据的独特混合

1.  针对效率优化的架构

RefinedWeb 数据集显示，文本语料库可以在比以前探索的更大规模上创建。为此，我们只需从网络上下载大量数据，并采用严格的去重规则以及更简单高效的过滤启发式方法。然后，通过将这些庞大的数据源与少量精选文本相结合，我们可以预训练一个性能极佳的开源 LLM。最后，Falcon 模型的修改架构使得训练和推理更加高效，结果是一个表现出色且在部署时能快速生成文本的模型。

## 与我联系！

非常感谢阅读这篇文章。我是 [Cameron R. Wolfe](https://cameronrwolfe.me/)，[Rebuy](https://www.rebuyengine.com/) 的 AI 总监。我研究深度学习的经验和理论基础。如果你喜欢这篇概述，请订阅我的 [Deep (Learning) Focus newsletter](https://cameronrwolfe.substack.com/)，我通过从头开始概述相关主题帮助读者理解 AI 研究。你还可以在 [X](https://twitter.com/cwolferesearch) 和 [LinkedIn](https://www.linkedin.com/in/cameron-r-wolfe-ph-d-04744a238/) 上关注我，或者查看我在 medium 上的 [其他著作](https://medium.com/@wolfecameron)！

## 参考文献

[1] “Introducing Falcon LLM”, *技术创新研究所*, 2023 年 6 月 7 日, [`falconllm.tii.ae/.`](https://falconllm.tii.ae/.)

[2] Penedo, Guilherme 等. “The RefinedWeb dataset for Falcon LLM: outperforming curated corpora with web data, and web data only.” *arXiv preprint arXiv:2306.01116* (2023).

[3] “Introducing MPT-7B: A New Standard for Open-Source, Commercially Usable Llms.” *MosaicML*, 2023 年 5 月 5 日, [www.mosaicml.com/blog/mpt-7b.](http://www.mosaicml.com/blog/mpt-7b.)

[4] Brown, Tom 等. “Language models are few-shot learners.” *Advances in neural information processing systems* 33 (2020): 1877–1901.

[5] Chowdhery, Aakanksha 等. “Palm: 利用路径扩展语言建模。” *arXiv 预印本 arXiv:2204.02311* (2022)。

[6] Hoffmann, Jordan 等. “训练计算最优的大型语言模型。” *arXiv 预印本 arXiv:2203.15556* (2022)。

[7] Zhou, Chunting 等. “Lima: 对齐的‘少即是多’。” *arXiv 预印本 arXiv:2305.11206* (2023)。

[8] Taylor, Ross 等. “Galactica: 一种用于科学的大型语言模型。” *arXiv 预印本 arXiv:2211.09085* (2022)。

[9] Rae, Jack W. 等. “扩展语言模型：方法、分析与训练 gopher 的见解。” *arXiv 预印本 arXiv:2112.11446* (2021)。

[10] “Falcon-7B”，*技术创新研究所*，HuggingFace 页面，[`huggingface.co/tiiuae/falcon-7b.`](https://huggingface.co/tiiuae/falcon-7b.)。

[11] “Falcon-40B”，*技术创新研究所*，HuggingFace 页面，[`huggingface.co/tiiuae/falcon-40b`](https://huggingface.co/tiiuae/falcon-40b)。

[12] Gao, Leo 等. “The pile: 一个 800GB 的多样文本数据集用于语言建模。” *arXiv 预印本 arXiv:2101.00027* (2020)。

[13] Touvron, Hugo 等. “Llama: 开放且高效的基础语言模型。” *arXiv 预印本 arXiv:2302.13971* (2023)。

[14] Dettmers, Tim 等. “Qlora: 量化语言模型的高效微调。” *arXiv 预印本 arXiv:2305.14314* (2023)。

[15] Su, Jianlin 等. “Roformer: 增强的带有旋转位置嵌入的变换器。” *arXiv 预印本 arXiv:2104.09864* (2021)。

[16] Ouyang, Long 等. “训练语言模型以遵循指令并获得人类反馈。” *神经信息处理系统进展* 35 (2022): 27730–27744。

[17] Glaese, Amelia 等. “通过有针对性的人类判断提高对话代理的对齐。” *arXiv 预印本 arXiv:2209.14375* (2022)。

[18] Vaswani, Ashish 等. “注意力即你所需。” *神经信息处理系统进展* 30 (2017)。
