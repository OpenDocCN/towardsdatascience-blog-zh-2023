# 民主化 AI：MosaicML 对开源 LLM 运动的影响

> 原文：[`towardsdatascience.com/democratizing-ai-mosaicmls-impact-on-the-open-source-llm-movement-7972ff12dd92`](https://towardsdatascience.com/democratizing-ai-mosaicmls-impact-on-the-open-source-llm-movement-7972ff12dd92)

## 高质量基础模型如何为整个行业开启新可能性……

[](https://wolfecameron.medium.com/?source=post_page-----7972ff12dd92--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----7972ff12dd92--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7972ff12dd92--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7972ff12dd92--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----7972ff12dd92--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7972ff12dd92--------------------------------) ·13 min 阅读·2023 年 10 月 15 日

--

![](img/4fb50fe15a17b036375d4670a4939ea5.png)

（照片由[Raimond Klavins](https://unsplash.com/@raimondklavins?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于[Unsplash](https://unsplash.com/photos/Ql6JhGdbQg0?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)）

最近，我们回顾了许多关于创建开源大型语言模型（LLM）的当前研究。在所有这些工作中，模型是通过一个包含几个简单组件的共同框架创建的；见下文。

![](img/56f9033518d72f0d72b0228371d7398c.png)

创建和优化大型语言模型（来自[12, 13]）的多步骤过程

尽管这个框架有几个步骤，但第一步可以说是最重要的。通过广泛的高质量预训练创建一个更强大的基础模型，可以在通过监督微调（SFT）和从人类反馈中进行强化学习（RLHF）时实现更好的结果。然后，下游应用由于使用了改进的模型而表现得更好。预训练（基础）模型是任何 LLM 应用程序的共同起点。

直到最近，开源基础模型要么与其专有对手相比表现不佳，要么只能用于研究。然而，这种情况随着 MosaicML 发布的 MPT-7B 和 MPT-30B [1, 2]的出现发生了变化。这些开源基础模型达到了令人印象深刻的性能水平，商业使用免费，并且配备了用于训练、微调和评估 LLM 的完整高效软件套件。这些开源工具使得可以以显著降低的成本探索多种专业应用场景，从而成为 AI 从业者的强大资源。

# 更快的 LLM 和更长的上下文长度

MPT-7B/30B 模型基于典型的 仅解码器变换器 架构。然而，进行了一些关键修改，包括：

+   [ALiBi](https://docs.mosaicml.com/projects/composer/en/stable/method_cards/alibi.html) [6]（取代了普通的位置嵌入）

+   [低精度层归一化](https://docs.mosaicml.com/projects/composer/en/latest/method_cards/low_precision_layernorm.html)

+   [Flash Attention](https://github.com/HazyResearch/flash-attention) [7]

在本节中，我们将深入了解这些组件，每个组件的工作原理，以及它们对 LLM 的影响。要全面理解本节的细节，可能需要回顾以下概念：

+   自注意力 [[link](https://twitter.com/cwolferesearch/status/1641932082283700226?s=20)]

+   因果自注意力（由仅解码器 LLM 使用） [[link](https://twitter.com/cwolferesearch/status/1644773244786941952?s=20)]

## ALiBi 实现了上下文长度的外推

![](img/80ebdbd2cbc254be8fafdafed13cdd8d.png)

在 LLM 中嵌入一个令牌序列（由作者创建）

在一个普通的变换器架构中，我们通过首先 [令牌化](https://vaclavkosar.com/ml/Tokenization-in-Machine-Learning-Explained) 原始文本并查找每个令牌的嵌入（令牌化器词汇表中的每个令牌都有一个唯一的嵌入）来创建一个输入令牌序列。然后，我们将位置嵌入添加到每个令牌嵌入中，从而将位置信息注入到序列中每个令牌的嵌入中；见上文。这是必要的，因为自注意力操作对序列中每个令牌的位置是无感知的。尽管位置嵌入工作良好，但有一个大问题：*它们难以推广到比训练期间见过的更长的序列*。

![](img/2f1ed9718460486ca898dc3ca48bc9dc.png)

（来自 [6]）

**解决方案。** 带有线性偏差的注意力**（**ALiBi）[6]通过完全去除位置嵌入来解决这个问题。相反，位置信息通过在自注意力操作中对键-查询注意力分数添加一个加性惩罚注入到变换器中；见上文。我们应当回顾，自注意力计算序列中每对令牌之间的注意力分数。ALiBi 通过为这个分数添加一个与令牌对之间的距离成比例的静态、非学习的偏差（或惩罚）来操作；见下文。

![](img/1ba2295d8ea2102c90b334b95463b5c8.png)

计算特定令牌对的键-查询注意力分数（由作者创建）

这种方法之所以有影响，是因为它依赖于令牌之间的成对距离，而不是序列中令牌的绝对位置。这一量度不那么依赖于基础序列的长度，并允许 ALiBi 对比训练期间看到的序列更长的序列进行更好的泛化；见下文。正如我们将看到的，使用 ALiBi 的 MPT 模型可以训练以支持比大多数开源替代方案更大的上下文长度*甚至可以推断到长度为 84K 标记的序列*！

![](img/21d505c2e4b85a7d291630146e65b4af.png)

(来自 [6])

## 更快的推理

由于使用低精度层归一化和 FlashAttention [7]，MPT 模型具有非常快的训练和推理速度（即，比使用标准[HuggingFace 推理管道](https://huggingface.co/blog/inference-update)的同等规模的 LLaMA 模型快`1.5-2X`）。更进一步，这些模型的权重可以迁移到像[FasterTransformer](https://github.com/NVIDIA/FasterTransformer)或[ONNX](https://github.com/onnx/models)这样的优化模块，以实现更快的推理。

**低精度层归一化**。简单来说，低精度层归一化以 16 位精度执行[LayerNorm](https://pytorch.org/docs/stable/generated/torch.nn.LayerNorm.html)模块的操作。尽管这种方法在某些情况下可能导致损失峰值，但它改善了硬件利用率，从而加速了训练和推理。使用低精度层归一化对模型的最终性能也几乎没有影响。

**Flash attention**。在其经典形式中，自注意力是一个`O(N²)`操作，其中`N`是输入序列的长度。为了提高该操作的效率，已经提出了许多近似注意力变体，例如：

+   Reformer [[link](https://arxiv.org/abs/2001.04451)]

+   SMYRF [[link](https://arxiv.org/abs/2010.05315)]

+   Performer [[link](https://arxiv.org/abs/2009.14794)]

大多数这些技术的目标是推导出一种“线性”注意力变体——一种具有`O(N)`复杂度的类似/近似操作。尽管这些变体在理论上减少了[FLOPs](https://stackoverflow.com/questions/58498651/what-is-flops-in-field-of-deep-learning)，*许多在实际场景中并没有实现任何墙钟速度的提升*！Flash attention 通过以 IO 感知的方式重新构建注意力操作来解决这个问题；见下文。

![](img/95ca281c05794f507e75428c5cd1c621.png)

(来自 [7])

FlashAttention 的硬件实现细节超出了本文的范围。然而，结果高效的注意力实现带来了各种积极的好处。例如，FlashAttention 可以：

+   将 BERT-large [10]的训练时间提高 15%

+   将 GPT-2 的训练速度提高`3X` [11]

+   为 LLMs 启用更长的上下文长度（由于更好的内存效率）

关于 FlashAttention 的更多细节，请查看 [这里](https://shreyansh26.github.io/post/2023-03-26_flash-attention/)。

# MPT-7B：一个商业可用的 LLaMA-7B

![](img/fcb2e1d8d8a273e732c0af9d250ef5c7.png)

训练 MPT-7B 模型及其各种衍生模型的总计算成本（来自 [1]）

在 [1] 中提出的 MPT-7B 是一个开源、商业可用的语言基础模型，性能广泛匹配类似规模的开源基础模型，如 LLaMA-7B [3]（它是*不可*商业使用的！）。根据 Chinchilla [4] 的经验教训，MPT-7B 在一个大的语料库上进行预训练——总计一万亿个标记——这些文本是多样的、公开可用的。用于训练、微调和评估 MPT-7B 的代码完全开源，*使得这个模型成为实践者们调整自己专门化大语言模型以解决各种不同下游应用的一个很好的资源或起点*。

## 创建基础模型

由于其修改后的架构，MPT-7B 具有几个理想的属性，例如能够泛化到更长的上下文长度和更快的推理速度。此外，我们在 [1] 中看到，这种修改后的架构消除了 MPT-7B 预训练过程中的损失峰值，使得模型可以在没有任何人工干预的情况下进行预训练（假设任何硬件故障都在大语言模型的训练代码中自动处理）！

![](img/719031a87595e7c842ff731839f2a5bb.png)

MPT-7B 在其预训练过程中仅经历硬件故障，这些故障可以自动解决（来自 [1]）

**训练过程。** 尽管大多数大语言模型使用 [AdamW 优化器](https://pytorch.org/docs/stable/generated/torch.optim.AdamW.html) 进行训练，MPT 采用了 Lion 优化器 [8]，这提高了训练过程的稳定性。整个训练框架基于 PyTorch 的 [完全分片数据并行 (FSDP)](https://github.com/huggingface/blog/blob/main/pytorch-fsdp.md) 包，不使用管道或张量并行。简单来说，MPT-7B 的训练框架是 [完全开源的](https://github.com/mosaicml/llm-foundry)，使用了流行/常见的组件，但进行了几个有用的修改，以提高训练的稳定性。

![](img/b83525c36783bedc660d813bd8750ac2.png)

(来自 [1])

**数据。** 用于训练 MPT-7B 的文本语料库是由公开数据集（主要是英语数据）定制混合而成；见上文。在 [1] 中，我们看到用于训练 MPT-7B 的数据量非常大 — 总计 1T 个标记。作为对比，开源模型如 [Pythia](https://huggingface.co/EleutherAI/pythia-1.4b) 和 [StableLM](https://github.com/Stability-AI/StableLM) 分别在 300B 和 800B 个标记上进行预训练。有趣的是，我们看到 [1] 的作者采用了一种非常特定的分词器 — [GPT-NeoX-20B](https://huggingface.co/docs/transformers/model_doc/gpt_neox) BPE 分词器 — 进行模型训练。这个分词器是受欢迎的，因为它在一个大规模、多样化的数据集上进行训练，并且比其他流行的分词器更一致地处理空格。

> “这个分词器具有许多令人满意的特性，其中大多数与代码分词相关：在包括代码的数据混合上训练，应用一致的空格分隔（不像 GPT2 分词器根据前缀空格的存在不一致地进行分词），并且包含了对重复空格字符的处理。” *— 来源于 [1]*

作为从业者，我们应该始终关注模型所使用的分词器。这个选择 — *尽管通常被忽视或忽略* — 会对我们的结果产生极大影响。例如，基于代码的语言模型需要一个以特定方式处理空格的分词器，而多语言模型则有各种独特的分词考虑因素。

## 它的表现如何？

![](img/4262851d604e3809968c0c19f5bcf82f.png)

（来源于 [1]）

MPT-7B 在标准基准测试中与各种开源模型进行比较（例如，LLaMA，[StableLM](https://github.com/Stability-AI/StableLM)，[Pythia](https://github.com/EleutherAI/pythia)，[GPT-NeoX](https://huggingface.co/docs/transformers/model_doc/gpt_neox)，[OPT](https://cameronrwolfe.substack.com/p/understanding-the-open-pre-trained-transformers-opt-library-193a29c14a15) 和 [GPT-J](https://huggingface.co/docs/transformers/model_doc/gptj)）。如上所示，LLaMA-7B 相较于开源替代方案取得了显著的改进，而 MPT-7B 的表现与 LLaMA 相匹配或超过其表现。近期的开源大型语言模型比其前身要好得多！*LLaMA-7B 和 MPT-7B 相比其他开源模型都是极其高效的基础模型*。然而，MPT-7B 可以用于商业用途，而 LLaMA 仅能用于研究。

## MPT-7B 的衍生模型

除了发布 MPT-7B 基础模型外，[1] 的作者还利用 MPT 的开源训练代码来微调多个不同的基础模型衍生版本（见下文）。与从头开始预训练一个大型语言模型相比，微调的成本非常低（即，*时间和成本减少 10–100 倍，甚至更多*）。因此，开发 MPT-7B 的大部分时间和精力都投入到了创建基础模型上，该模型作为微调下述模型的起点。

![](img/08fa6f43cdadaf4d961b5d44c3852f25.png)

（来自 [1]）

**MPT-StoryWriter-65K（商业版）** 是 MPT-7B 的一个版本，经过了在非常长上下文长度的数据上进行微调。特别是，文献中的作者 [1] 利用了包含虚构书籍摘录的 [books3 dataset](https://huggingface.co/datasets/the_pile_books3)，以创建一个用于微调的数据集（即仅使用下一个令牌预测目标），上下文长度为 65K 令牌。由于使用了 ALiBi [6] 和 FlashAttention [7]，MPT-StoryWriter-65K 可以有效地在如此大的输入上进行训练，能够处理《了不起的盖茨比》的全部内容（68K 令牌）以编写后记（见上文），甚至可以推广到处理长度达 84K 令牌的序列。

> “我们期望 LLMs 将输入视为需要遵循的指令。指令微调是训练 LLMs 以这种方式执行指令跟随的过程。通过减少对巧妙提示工程的依赖，指令微调使 LLMs 更加易于访问、直观和立即可用。” *——来自 [1]*

**MPT-7B-Instruct（商业版）** 和 **MPT-7B-Chat（非商业版）** 是 MPT-7B 的指令调整版本。指令版是在 [Dolly-15K](https://huggingface.co/datasets/databricks/databricks-dolly-15k) 和 [Helpful and Harmless dataset](https://huggingface.co/datasets/Anthropic/hh-rlhf) 数据上进行微调的，而聊天模型则使用了来自 [ShareGPT](https://sharegpt.com/)、[HC3](https://huggingface.co/datasets/Hello-SimpleAI/HC3)、[Alpaca](https://huggingface.co/datasets/tatsu-lab/alpaca) 和 [Evol-Instruct](https://huggingface.co/datasets/WizardLM/WizardLM_evol_instruct_70k) 等来源的数据进行训练。如上文所述，指令调整是指在预训练语言模型的基础上，对其风格或行为进行修改，使其更加直观和易于访问，通常着重于指令跟随或问题解决。

# MPT-30B：一个开源的 GPT-3 替代品

![](img/108528023f4e40073f14767ea75f97f7.png)

MPT-30B 在所有性能类别上都优于 MPT-7B（来自 [2]）

在其提出后不久，MPT-7B 模型在 AI 研究界获得了显著认可 —— *它甚至在 HuggingFace 上累计下载量超过了 300 万次*！MPT-7B 的成功并不令人意外，因为它为极受欢迎的 LLaMA-7B 模型提供了一个商业上可用的替代品。借此势头，MosaicML 的研究人员推出了一个稍大的模型，称为 MPT-30B [2]，该模型被发现能与 GPT-3 [9] 的表现相匹敌或超过。因此，MPT-30B 的提出延续了为任何人提供强大基础 LLM 的商业可用版本的趋势。

## 深入了解 MPT-30B

MPT-30B 共享与 MPT-7B 相同的修改过的解码器架构，使用了 FlashAttention 和低精度层归一化，以提高效率。总体而言，这些模型非常相似，除了 MPT-30B 更大一些。值得注意的是，MPT-30B 的大小选择得非常具体。这个大小的模型可以在单个 GPU 上使用 8 位或 16 位精度进行部署，而像 [Falcon-40B](https://falconllm.tii.ae/) 这样的替代品稍微大了一些，无法以这种方式部署。

![](img/5bf95c22273ced86d9ebbdda2e35b4c5.png)

（来自 [2]）

**有什么不同？** MPT-30B 与 MPT-7B 主要有两方面的不同：

+   预训练数据混合

+   上下文长度

MPT-30B 的预训练数据集类似于 MPT-7B，但数据的混合略有不同；见上文。此外，MPT-30B 部分使用 8K 上下文长度进行训练，而大多数其他开源模型（例如，LLaMA、Falcon 和 MPT-7B）使用较短的 2K 令牌上下文长度进行训练。更具体地说，我们看到 MPT-30B 使用一种训练课程，模型首先使用 2K 上下文长度进行训练，然后在训练后期切换到 8K 上下文长度。在第二阶段，数据集中代码的比例增加了 `2.5X`，使得最终模型在编程能力上比其他开源 LLM 更强。

**模型变体。** 除了 MPT-30B 基础模型外，[2] 中的作者还发布了 MPT-30B 的聊天和指令变体。这些模型遵循类似于 MPT-7B-Instruct 和 MPT-7B-Chat 的训练策略。然而，这些模型用于指令调优的数据显著增加。有趣的是，发现 MPT-30B-Chat 在编程技能上表现出色！

## 它表现得好吗？

![](img/7f9063398a8d60ba7f0ffd8a308921f9.png)

（来自 [2]）

除了在各种类别上优于 MPT-7B 外，MPT-30B 的表现与顶级开源替代品如 LLaMA-30B 和 Falcon-40B 相当；见上文。总体而言，我们发现 MPT-30B 在解决基于文本的任务时落后于 Falcon 和 LLaMA，但在编程相关问题上通常优于这些模型（可能是因为预训练数据集中代码的比例较高！）。值得注意的是，我们发现 MPT-30B 在各种上下文学习任务上优于 GPT-3；见下文。

![](img/6d2fb7533c554c9ac72d5323b2c9b96f.png)

（来自 [2]）

结合这些结果来看，像 MPT-30B 这样的模型可能为开源 LLM 应用奠定了基础，能够与专有系统的质量相媲美。我们所需要的只是足够的精细调整和微调！

# 最终备注

> “你可以训练、微调和部署你自己的私人 MPT 模型，可以从我们的检查点之一开始，或从头开始训练” *— 来自 [2]*

MosaicML 提供的基础模型是开源 LLM 社区的一大进步，因为它们提供了与 LLaMA 和 GPT-3 等流行基础模型相当的商用 LLM。然而，这一开源产品不仅仅限于 MPT 模型本身——它还包括一个 [用于训练 LLM 的开源代码库](https://github.com/mosaicml/llm-foundry)，各种 [在线演示](https://huggingface.co/mosaicml) 等。

![](img/d1e644e4c8b767e0c765327d15d87d49.png)

(来自 [2])

MPT-7B 和 30B 模型配备了一个完整的开源工具生态系统，可用于创建专业化/个性化的 LLMs。鉴于创建基础模型是任何基于 LLM 系统中最昂贵的部分（见上文），这些工具显著降低了使用 LLM 的门槛，并提供了一个解决各种下游应用的起点。记住，当我们有一个特定的任务需要解决时，[微调是极其有效的](https://magazine.sebastianraschka.com/i/125373356/finetuning-task-specific-llms-for-your-business-needs)（即，仅通过提示一个更通用的 LLM 难以超越）！

## 与我联系！

非常感谢您阅读这篇文章。我是 [Cameron R. Wolfe](https://cameronrwolfe.me/)，[Rebuy](https://www.rebuyengine.com/) 的 AI 总监。我研究深度学习的经验和理论基础。如果您喜欢这个概述，请订阅我的 [Deep (Learning) Focus 新闻通讯](https://cameronrwolfe.substack.com/)，在其中我通过从基础上概述相关主题帮助读者理解 AI 研究。您还可以在 [X](https://twitter.com/cwolferesearch) 和 [LinkedIn](https://www.linkedin.com/in/cameron-r-wolfe-ph-d-04744a238/) 上关注我，或者查看我在 medium 上的 [其他文章](https://medium.com/@wolfecameron)！

## 参考文献

[1] “介绍 MPT-7B: 开源商用 LLM 的新标准。” *MosaicML*，2023 年 5 月 5 日，[www.mosaicml.com/blog/mpt-7b.](http://www.mosaicml.com/blog/mpt-7b.)

[2] “MPT-30B: 提升开源基础模型的标准。” *MosaicML*，2023 年 6 月 22 日，[www.mosaicml.com/blog/mpt-30b.](http://www.mosaicml.com/blog/mpt-30b.)

[3] Touvron, Hugo, 等。“Llama: 开放且高效的基础语言模型。” *arXiv 预印本 arXiv:2302.13971* (2023)。

[4] Hoffmann, Jordan, 等。“训练计算最优的大型语言模型。” *arXiv 预印本 arXiv:2203.15556* (2022)。

[5] Zhang, Susan, 等。“OPT: 开放的预训练变换器语言模型。” *arXiv 预印本 arXiv:2205.01068* (2022)。

[6] Press, Ofir, Noah A. Smith, 和 Mike Lewis。“训练短期，测试长期：具有线性偏置的注意力机制实现输入长度外推。” *arXiv 预印本 arXiv:2108.12409* (2021)。

[7] Dao, Tri, 等。“Flashattention: 快速且内存高效的准确注意力机制，具有 IO 感知能力。” *神经信息处理系统进展* 35 (2022): 16344–16359。

[8] 陈向宁等人。“优化算法的符号发现。” *arXiv 预印本 arXiv:2302.06675* (2023)。

[9] 布朗汤姆等人。“语言模型是少样本学习者。” *神经信息处理系统进展* 33 (2020): 1877–1901。

[10] 德夫林雅各布等人。“Bert: 深度双向变换器的预训练以实现语言理解。” *arXiv 预印本 arXiv:1810.04805* (2018)。

[11] 拉德福德亚历克等人。“语言模型是无监督的多任务学习者。”

[12] 欧阳龙等人。“通过人类反馈训练语言模型以遵循指令。” *神经信息处理系统进展* 35 (2022): 27730–27744。

[13] 格莱斯艾米莉亚等人。“通过有针对性的人工评判改进对话代理的对齐。” *arXiv 预印本 arXiv:2209.14375* (2022)。
