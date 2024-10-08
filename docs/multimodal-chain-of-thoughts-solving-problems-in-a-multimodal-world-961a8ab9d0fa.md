# 多模态思维链：在多模态世界中解决问题

> 原文：[`towardsdatascience.com/multimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa`](https://towardsdatascience.com/multimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa)

## NLP | 多模态性 | 思维链 |

## 世界不仅仅是文字：如何将思维链扩展到图像和文字？

[](https://salvatore-raieli.medium.com/?source=post_page-----961a8ab9d0fa--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----961a8ab9d0fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----961a8ab9d0fa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----961a8ab9d0fa--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----961a8ab9d0fa--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----961a8ab9d0fa--------------------------------) ·14 分钟阅读·2023 年 3 月 13 日

--

![](img/868bf8d7fafd276e2175a475fa6cc822.png)

图片由 [Giulio Magnifico](https://unsplash.com/it/@giuliomagnifico) 提供，来源于 Unsplash

有时得出答案并不容易，特别是当问题需要推理时。模型并不总是在其参数中隐藏答案，但可以通过正确的上下文和方法得出答案。什么是思维链？为什么这种方法使得解决多步骤推理任务成为可能？它可以扩展到多模态问题（即包含图像和文字的问题）吗？只有大型模型才能做到这一点吗？

**本文讨论了如何回答这些问题。**

# 思维链（CoT）：它是什么？

![](img/118e0e262bad7b95096ebc2f40b9c948.png)

图片由 [Todd Cravens](https://unsplash.com/it/@toddcravens) 提供，来源于 [Unsplash](https://unsplash.com/)

近年来，我们见证了模型参数数量的增长（超过 1000 亿个参数）。这受到扩展法则的推动：随着参数数量的增加，误差减少。

[](/unsupervised-data-pruning-less-data-to-learn-better-30cd2bfbd855?source=post_page-----961a8ab9d0fa--------------------------------) ## 无监督数据剪枝：用更少的数据更好地学习

### 更多的数据并不总是意味着更准确的模型，但如何选择你的数据呢？

towardsdatascience.com

虽然这对于如[情感分析](https://en.wikipedia.org/wiki/Sentiment_analysis)和机器翻译等任务是正确的（即使在[零-shot](https://en.wikipedia.org/wiki/Zero-shot_learning)或[少-shot 学习](https://paperswithcode.com/task/few-shot-learning)的情况下），即使是拥有数十亿参数的模型在需要多步推理的任务中（例如数学问题或常识推理）也会遇到困难。

> 如何让模型在这些任务中取得成功？

大型模型可以针对特定任务进行微调，这也是最初尝试的系统。[正如这个想法的作者解释的那样](https://arxiv.org/pdf/2006.06609.pdf)，如果你问一个模型鲸鱼是否有肚脐，模型将错误地回答“没有”。这是因为模型的参数中没有存储这条信息。作者建议可以通过提供隐性知识的提示来帮助模型：“鲸鱼是哺乳动物”。

![](img/80dc45de359daf253e1afdba6a7637c1.png)

来源：[这里](https://arxiv.org/pdf/2006.06609.pdf)

提供隐性知识的想法为系统通过与用户交互来改进自己铺平了道路。用户可以识别错误并向模型提供信息，允许其自我纠正。或者更准确地说，正如作者所定义的：

> 这可以视为一种“单次学习”的形式，可以在不进行进一步训练的情况下即时改进模型，这与大多数当前依赖于数据收集和重新训练来修正模型错误的工作方式不同。

从概念上讲，这个想法是一个模型可以通过利用中间步骤来解决其确切答案并不直接知道的问题。

[正如谷歌所指出的](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)，提示使得上下文少样本学习成为可能。换句话说，除了对特定任务进行微调外，可以通过一些输入-输出示例来提示语言模型。这种方法被证明非常有效，尤其在问答系统中。此外，正如在上下文学习中所示，它对于[大型模型特别有效](https://arxiv.org/abs/2005.14165)。

![](img/51a81dd2261eeecbc3a6dfdb574ecda2.png)

“对于所有 42 个准确度基准的整体表现，虽然零-shot 性能随着模型规模的增加而稳步提升，但少-shot 性能则更快增加，证明了更大的模型在上下文学习方面更为高效。” 来源：[这里](https://arxiv.org/pdf/2005.14165.pdf)

谷歌随后提出，通过仅包含几个思维链的示例，可以让模型解决多步推理问题。为了更好地理解，以下是经典提示和思维链提示之间的变化示例：

![](img/fe105e20281bcdedc89613b6e0cc665f.png)

“思维链提示使大型语言模型能够处理复杂的算术、常识和符号推理任务。思维链推理过程被突出强调。” 来源：[这里](https://arxiv.org/pdf/2201.11903.pdf)

这种方法的优点是它既不需要改变语言模型的权重，也不需要大规模的训练数据集。

> 简而言之，我们可以说，这个理念是将复杂的问题分解为一系列可以单独解决的中间步骤。

这可能看起来是小事，但实际上意味着这种方法可以应用于任何你可以用语言解决的问题。

谷歌的作者表示这是模型的一个突现特性，它在达到一定的模型容量时（他们估计约 100 B 参数）出现。作者评估了增加模型以解决数学问题：

![](img/e080fc3f70908bcd1f8de1eb59a93f03.png)

“: 思维链提示使大型语言模型能够解决具有挑战性的数学问题”。来源：[这里](https://arxiv.org/pdf/2201.11903.pdf)

此外，作者指出，模型的改进并非来自增加参数，而是通过使用“思维链提示，增加模型规模会带来显著优于标准提示的大模型性能提升。”

这对于[常识推理](https://en.wikipedia.org/wiki/Commonsense_reasoning)（“在假设一般背景知识的情况下推理关于物理和人类互动”）也是成立的。

![](img/6eaadf276534702b66a03cafc02d8ab7.png)

“: 算术、常识和符号推理基准的输入、思维链、输出三元组示例。” 来源：[这里](https://arxiv.org/pdf/2201.11903.pdf)

在这种情况下，模型也显示了相同的行为：“性能随着模型规模的扩大而提高，采用思维链提示也带来了额外的小幅改进。” 最大的改进出现在运动理解领域（令人惊讶）。

总的来说，我们已经看到 CoT 有两种技术，微调或使用提示（上下文学习）。关于第二种范式，我们可以进一步细分为：

+   **零样本 CoT**。Kojima 证明了语言模型在零样本 CoT（仅仅添加“让我们一步步来思考”）方面表现不错，这足以显著提高零样本 LLM 的复杂推理能力。

+   **少样本 CoT**。少量的逐步推理示例用于模型推理中的条件设置。每个示例同时提供一个问题和一个解释模型如何得出最终答案的推理链（这些示例可以是手工制作的或使用自动生成）。

![](img/124ec295b0a57cc2601f62677c11d3ee.png)

来源：[这里](https://arxiv.org/pdf/2205.11916.pdf)

少样本 CoT 已被证明更有效且结果更好（前提是示例编写得很好）。因此，大多数后续研究都集中在这种方法上。

![](img/4f31f9d790f23ee524f7a0b7b3087c5b.png)

“典型的 CoT 技术（FT: 微调；KD: 知识蒸馏）。第一部分：上下文学习技术；第二部分：微调技术。根据我们所知，我们的工作是首次研究不同模态下的 CoT 推理。此外，我们专注于 1B 模型，不依赖于 LLMs 的输出。” 来源：[这里](https://arxiv.org/pdf/2302.00923.pdf)

# 多模态链式思维具有挑战性

![](img/acf1b79a008b36938e2f394e6876ae94.png)

图片来源于 [airfocus](https://unsplash.com/fr/@airfocus) 在 Unsplash 上

正如我们所见，链式思维（CoT）在需要复杂推理的问题上证明了非常有用。许多问题不仅是文本的，还有多模态的。例如，解决一个问题我们可能需要查看图片。正如我们所说，CoT 仅适用于可以用文本形式表达的问题。**我们如何处理多模态问题？**

> 想象一下阅读一本没有图形或表格的教科书。我们通过联合建模不同的数据模态（如视觉、语言和音频）来极大地增强知识获取能力。 ([来源](https://arxiv.org/pdf/2302.00923.pdf))

最近一篇文章正好提出了这个问题，并试图将 CoT 扩展到多模态问题中：

[](https://arxiv.org/abs/2302.00923?source=post_page-----961a8ab9d0fa--------------------------------) [## 多模态链式思维推理在语言模型中的应用]

### 大型语言模型（LLMs）通过利用链式思维在复杂推理上表现出色…

[arxiv.org](https://arxiv.org/abs/2302.00923?source=post_page-----961a8ab9d0fa--------------------------------)

如前所述，[参数少于 100 亿的模型往往会产生不合逻辑的 CoT](https://arxiv.org/pdf/2206.07682.pdf)，从而导致错误的答案。一个 [多模态模型](https://en.wikipedia.org/wiki/Multimodal_learning) 不仅要处理文本输入，还要处理其他模态。这使得创建一个参数少于 100 亿的模型变得困难。

另一方面，META 的 LLaMA 显示出，参数少于 100 亿的模型可以达到与更大模型相当的结果。

[](https://medium.com/mlearning-ai/metas-llama-a-small-language-model-beating-giants-5065948e0b7f?source=post_page-----961a8ab9d0fa--------------------------------) [## META 的 LLaMA：一个击败巨头的小型语言模型]

### META 开源模型将帮助我们理解语言模型的偏见如何产生

medium.com](https://medium.com/mlearning-ai/metas-llama-a-small-language-model-beating-giants-5065948e0b7f?source=post_page-----961a8ab9d0fa--------------------------------)

此外，正如其他研究所示，文本模型在训练过程中没有看到图片，因此没有关于视觉元素或如何利用视觉特征的信息。

在多模态环境中进行 CoT 推理要求模型考虑不同的模态：给定不同模态的输入，模型将一个多步骤的问题分解为一系列中间步骤，然后推断出答案。

![](img/3d2df89b5ca5751d1b77424fdce30e20.png)

多模态 CoT 任务示例。 来源: [这里](https://arxiv.org/pdf/2302.00923.pdf)

> 执行多模态 CoT 的最直接方法是将不同模态的输入转换为一种模态，并提示大型语言模型执行 CoT。 ([source](https://arxiv.org/pdf/2302.00923.pdf))

例如，可以取一张图像，并将其作为输入用于字幕生成模型。一旦获得字幕，就可以将其与文本提示结合起来，然后提供给大型语言模型。

然而，这种方法有一个严重的缺陷，即字幕与视觉特征相比丢失了大量信息，因此不同模态间的信息协同作用丧失了。

此外，之前的研究表明，预训练的单模态模型的跨模态对齐并不容易。例如，在 BLIP-2 中，为了使视觉变换器和语言模型能够互相交流，他们需要在两者之间增加一个额外的变换器。

[## BLIP-2: when ChatGPT meets images](https://levelup.gitconnected.com/blip-2-when-chatgpt-meets-images-463582b541e0?source=post_page-----961a8ab9d0fa--------------------------------)

### BLIP-2 是一种新的视觉语言模型，能够进行关于图像的对话。

[levelup.gitconnected.com](https://levelup.gitconnected.com/blip-2-when-chatgpt-meets-images-463582b541e0?source=post_page-----961a8ab9d0fa--------------------------------)

考虑到这些挑战，作者决定研究是否可以训练一个具有 10 亿参数的多模态 CoT 模型。

> 这项工作集中在 10 亿模型上，因为它们可以用消费级 GPU（例如 32G 内存）进行微调和部署。在这一部分，我们将研究为什么 10 亿模型在 CoT 推理中失败，并研究如何设计有效的方法以克服这一挑战。 ([source](https://arxiv.org/pdf/2302.00923.pdf))

# 为什么小模型在 CoT 中失败，如何设计以克服这一挑战？

![](img/0d7373870be1ed4a86f40f8f61dfe6d2.png)

图片由 [Jason Leung](https://unsplash.com/fr/@ninjason) 提供，来源于 Unsplash

实际上，训练小模型进行推理的方法已经被尝试过。然而，之前的尝试中使用了一个大型模型作为教师和一个小型模型作为学生。

例如，作者为教师模型提供了一个提示，并使用了“让我们一步一步思考”的方法来获得解释推理的答案。然后，将提示加上演示提供给较小的模型。

![](img/84f4aedada0c72c493bcc5a5224755ac.png)

“我们考虑一种由多个阶段组成的方法。首先，使用多步骤推理提示一个大型教师模型回答问题，而不依赖于正确的示例。也就是说，教师采用零-shot 链式思维推理生成输出。然后，我们使用生成的推理样本（包括问题和教师输出）对更小的学生模型进行微调。” 图像来源 ([here](https://arxiv.org/pdf/2212.10071.pdf))

然而，这种方法仍然需要使用大型语言模型及其所有缺点。

作者决定探索小型模型可以针对多模态-CoT 进行微调的可能性。简而言之，融合多模态特征允许模型架构更加灵活地调整（与提示相关）。然而，主要问题仍然存在：“*关键挑战在于，参数少于 100 亿的语言模型往往生成幻觉理由，从而误导答案推断*。”

> 首先，为什么小模型在 CoT 推理中会出现幻觉？

作者提出了相同的问题：调查为什么 1-B 模型在 CoT 推理中失败。一旦理解了这一点，研究有效的方法。

作者首先对文本-only 基线模型进行了 CoT 推理的微调。在这种情况下，问题被建模为文本生成问题。基线包括问题（Q）、上下文（C）和多个选项（O），模型必须预测答案（A）。作者将基线与在答案前预测理由（R）（QCM→RA）以及理由用于解释答案（QCM→AR）进行了比较。

![](img/2146d70a184c982d35f2f28141176e52.png)

[(source)](https://arxiv.org/pdf/2302.00923.pdf)

结果令人惊讶，如果模型首先预测理由，准确率下降超过 10%：“*结果表明，理由可能不一定有助于预测正确答案*。”换句话说，似乎推理反而对答案有害。

> 但为什么呢？

为了理解这一点，作者决定将问题分成两个阶段。首先，生成理由，然后利用这些理由回答问题。模型在生成高质量理由方面成功了（[RougeL](https://en.wikipedia.org/wiki/ROUGE_(metric))是一种用于自动摘要和机器翻译的度量），但同时，似乎对准确性推断（问题的答案）产生了不利影响。

![](img/51cbf7e909c1943b99abc033957ab8db.png)

[(source)](https://arxiv.org/pdf/2302.00923.pdf)

理由并没有帮助提高答案的准确性。因此，作者选择了 50 个随机错误案例并手动检查。发现模型在生成理由时经常出现幻觉，因为缺乏对视觉内容的参考。

![](img/c06139df299f4b78e12e963813f31e7a.png)

[(source)](https://arxiv.org/pdf/2302.00923.pdf)

这是最常见的错误，超过 60%的错误归因于这一因素。

![](img/b3633d5eba9fad4d0720544d6c9f3718.png)

[(来源)](https://arxiv.org/pdf/2302.00923.pdf)

**那么为什么不提供关于图像内部内容的信息呢？** 作者使用了一个管道来生成标题并将其提供给模型（将标题附加到输入中）。然而，这导致边际准确度的增加（0.59 个百分点，在表 3 中）。

作者随后测试了另一种方法，将图像作为输入传递给[DETR](https://github.com/facebookresearch/detr)模型，目的是提取视觉特征。他们将这些视觉特征与编码后的语言表示结合在一起。换句话说，文本由 LM 编码器编码，图像由视觉模型编码。这两个输出结合在一起，成为 LM 解码器的输入。

结果显示（见表 3），这不仅改善了理由生成，还提高了回答的准确性。换句话说，拥有更好的理由“幻觉现象得到了缓解。”视觉特征对更好的回答有益，但这些有用的信息可能在生成标题的过程中丢失了。

> 理解了模型为什么会出现幻觉之后，我们可以使用什么框架来进行高效的多模态-CoT？

作者建议将语言（文本）和视觉（图像）模态结合到一个两阶段框架中：首先生成理由，然后生成回答。

模型架构在两个步骤中是相同的；然而，输入和输出有所变化。在第一步中，模型接受语言和视觉输入以生成理由。在第二步中，提供了原始语言输入，并将其附加到第一阶段生成的理由中。这经过第二模型的编码器，然后添加视觉特征并使用解码器得到最终答案。

![](img/8d57433fa30ff894cf4d070962bf2bad.png)

“我们多模态-CoT 框架的概述。Multimodal-CoT 由两个阶段组成：（i）理由生成和（ii）答案推断。两个阶段共享相同的模型架构，但在输入和输出上有所不同。在第一阶段，我们将语言和视觉输入提供给模型以生成理由。在第二阶段，我们将原始语言输入与第一阶段生成的理由附加在一起。然后，我们将更新后的语言输入与原始视觉输入一起提供给模型以推断答案。” [(来源)](https://arxiv.org/pdf/2302.00923.pdf)

# 小模型能有竞争力吗？

![](img/535bf546836d7acf7591f1219306871d.png)

照片由[Steven Lelham](https://unsplash.com/it/@slelham)拍摄，来自 Unsplash

我们已经了解了为什么小模型在 CoT 过程中会产生幻觉，如何解决这个问题，现在还需要了解这种方法是否在与大型模型和其他方法相比时具有竞争力。

作者决定使用 ScienceQA 基准：

> ScienceQA 是首个大规模的多模态科学问题数据集，注释了详细的讲解和解释。它包含 21,000 个多模态选择题，涵盖了 3 个学科、26 个主题、127 个类别和 379 项技能的丰富领域多样性。 [(来源)](https://arxiv.org/pdf/2302.00923.pdf)

为了使用视觉特征，他们需要一个使用编码器-解码器的模型，因此选择了[T5](https://huggingface.co/docs/transformers/model_doc/t5)。此外，为了更好地研究该方法是否对其他模型具有通用性，他们还选择了[FLAN-T5](https://huggingface.co/docs/transformers/model_doc/flan-t5)。他们还决定将其与多个模型和人类进行比较。

结果表明，他们的方法优于 GPT-3.5，并且在各种问题类别中也超越了人类（无论是平均水平还是在不同类别中）。UnifiedQA 和 GPT-3.5 使用了字幕，结果表明视觉特征更为有效。

![](img/1929fc5a46b1dfcc2564668c653efea7.png)

[(来源)](https://arxiv.org/pdf/2302.00923.pdf)

消融研究表明，使用两阶段方法能够充分发挥视觉特征的优势。

![](img/45f9f2d4bfd4427b0d97bd7931d2b47f.png)

[(来源)](https://arxiv.org/pdf/2302.00923.pdf)

此外，作者指出，多模态性能提升收敛性。实际上，这种两阶段模型从训练开始时就能实现更高的准确性。

![](img/c2ea5f39f79dcf7d6ba95d44b25c8a1b.png)

“No-CoT 基准和 MultimodalCoT 变体在各个时期的准确性曲线。” [(来源)](https://arxiv.org/pdf/2302.00923.pdf)

作者表示，该方法具有广泛的模型通用性来提取视觉特征，他们选择了 DETR，因为它提供了最佳的准确性。

![](img/eb8903811f64cc692608c9018b9d2eb9.png)

[(来源)](https://arxiv.org/pdf/2302.00923.pdf)

选择的文本模型也是具有通用性的。也就是说，该方法即使在使用不同的语言模型时也能有效。

![](img/f7ae7075d0f2ac3fe50e42b801924860.png)

[(来源)](https://arxiv.org/pdf/2302.00923.pdf)

然后，作者检查了 50 个答案正确的例子和 50 个答案错误的例子，以更好地理解机制。结果表明，CoT 并不总是对答案有利，但模型非常稳健，在某些情况下即使推理错误也能正确回答。此外，当答案错误时，大多数错误是由于常识性错误。

![](img/c2c13be5e6be851b4efd753c8fd8cc4e.png)

[(来源)](https://arxiv.org/pdf/2302.00923.pdf)

当问题需要常识知识时，该模型在大多数情况下会出现常识错误：例如，理解地图或计算图像中的数字，或使用字母表。错误示例：

![](img/9d0c25aae5e3728a937c8fac70f1ea22.png)

[(source)](https://arxiv.org/pdf/2302.00923.pdf)

作者表示，这些结果为模型的未来修改提供了线索：

> 可以通过以下方式改进 MultimodalCoT：（i）结合更有信息量的视觉特征，改善语言-视觉交互，以便理解地图和计数；（ii）注入常识知识；（iii）应用过滤机制，例如，只使用有效的 CoT 来推断答案，并去除无关的 CoT。[(source)](https://arxiv.org/pdf/2302.00923.pdf)

作者已将模型、代码和数据集上传到 GitHub，供希望测试或了解更多的人使用：

[](https://github.com/amazon-science/mm-cot?source=post_page-----961a8ab9d0fa--------------------------------) [## GitHub - amazon-science/mm-cot: “Multimodal Chain-of-Thought Reasoning”的官方实现…]

### “想象一下学习一本没有图表的教科书。”Multimodal-CoT 在解耦的方式中融入了视觉特征…

github.com](https://github.com/amazon-science/mm-cot?source=post_page-----961a8ab9d0fa--------------------------------)

# **告别思考**

本研究中的作者正式研究了多模态 CoT。他们分析了为什么小模型在 CoT 期间会产生幻觉，并展示了小模型在多模态 CoT 中能够超越大型模型（甚至超越人类表现）的能力。关键是能够最佳地结合文本和视觉模态。

这是通过使用两阶段方法实现的，第一阶段使用视觉特征创建推理，然后利用这一最佳推理来获得答案。作者进行的分析给出了如何获得更好模型的建议。

简而言之，这篇论文的结果表明，即使是一个小模型也能解决复杂问题。此外，为模型提供正确的多模态特征是至关重要的。无需一个拥有数十亿参数的大型语言模型，因为对视觉特征有了解的小模型在图像描述方面表现得更好。

# 如果你觉得这些内容有趣：

你可以查看我的其他文章，也可以[**订阅**](https://salvatore-raieli.medium.com/subscribe)以便在我发布新文章时获得通知，你还可以在[**LinkedIn**](https://www.linkedin.com/in/salvatore-raieli/)**上联系我**。

这是我的 GitHub 仓库的链接，我计划在这里收集与机器学习、人工智能等相关的代码和许多资源。

[](https://github.com/SalvatoreRa/tutorial?source=post_page-----961a8ab9d0fa--------------------------------) [## GitHub - SalvatoreRa/tutorial: 机器学习、人工智能、数据科学教程…]

### 机器学习、人工智能、数据科学的教程，包含数学解释和可重用的代码（使用 Python…）

[GitHub 教程](https://github.com/SalvatoreRa/tutorial?source=post_page-----961a8ab9d0fa--------------------------------)

或许你对我最近的一篇文章感兴趣：

[](https://pub.towardsai.net/pca-bioinformaticians-favorite-tool-can-be-misleading-fe139262a576?source=post_page-----961a8ab9d0fa--------------------------------) [## PCA：生物信息学家最喜爱的工具可能会产生误导

### 一项新研究评估了一个最常用的技术可能带来的问题

[PCA：生物信息学家最喜爱的工具可能会产生误导](https://pub.towardsai.net/pca-bioinformaticians-favorite-tool-can-be-misleading-fe139262a576?source=post_page-----961a8ab9d0fa--------------------------------) [](https://levelup.gitconnected.com/stable-diffusion-and-the-brain-how-ai-can-read-our-minds-45398b395ea9?source=post_page-----961a8ab9d0fa--------------------------------) [## 稳定扩散与大脑：AI 如何读取我们的思想

### 研究人员能够利用 fMRI 数据重建图像。

[稳定扩散与大脑：AI 如何读取我们的思想](https://levelup.gitconnected.com/stable-diffusion-and-the-brain-how-ai-can-read-our-minds-45398b395ea9?source=post_page-----961a8ab9d0fa--------------------------------) [](https://levelup.gitconnected.com/microsoft-biogpt-towards-the-chatgpt-of-life-science-56e251536af6?source=post_page-----961a8ab9d0fa--------------------------------) [## 微软 BioGPT：走向生命科学领域的 ChatGPT

### BioGPT 在不同的生物医学 NLP 任务中达到了 SOTA

[微软 BioGPT：走向生命科学领域的 ChatGPT](https://levelup.gitconnected.com/microsoft-biogpt-towards-the-chatgpt-of-life-science-56e251536af6?source=post_page-----961a8ab9d0fa--------------------------------) [](https://levelup.gitconnected.com/stable-diffusion-to-fill-gaps-in-medical-image-data-b78a2a7d6c9d?source=post_page-----961a8ab9d0fa--------------------------------) [## 稳定扩散填补医学图像数据中的空白

### 一项新研究显示，稳定扩散可以帮助医学图像分析和罕见疾病。怎么做？

[稳定扩散填补医学图像数据中的空白](https://levelup.gitconnected.com/stable-diffusion-to-fill-gaps-in-medical-image-data-b78a2a7d6c9d?source=post_page-----961a8ab9d0fa--------------------------------)
