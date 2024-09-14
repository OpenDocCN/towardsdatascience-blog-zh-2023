# 你应该知道的关于评估大型语言模型的一切

> 原文：[`towardsdatascience.com/everything-you-should-know-about-evaluating-large-language-models-dce69ef8b2d2`](https://towardsdatascience.com/everything-you-should-know-about-evaluating-large-language-models-dce69ef8b2d2)

## 开放语言模型

## 从困惑度到衡量一般智能

[](https://donatoriccio.medium.com/?source=post_page-----dce69ef8b2d2--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----dce69ef8b2d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dce69ef8b2d2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dce69ef8b2d2--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----dce69ef8b2d2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dce69ef8b2d2--------------------------------) ·10 分钟阅读·2023 年 8 月 28 日

--

![](img/1557949e969c2f9d2a1f22fa5916c168.png)

图片由作者使用稳定扩散生成。

随着开源语言模型的越来越普及，容易在众多选项中迷失方向。

我们如何确定它们的表现并进行比较？我们如何自信地说一个模型比另一个更好？

本文通过提供训练和评估指标、一般和特定基准，来帮助清晰了解你模型的表现。

如果你错过了，请查看《开放语言模型》系列中的第一篇文章：

[](/a-gentle-introduction-to-open-source-large-language-models-3643f5ca774?source=post_page-----dce69ef8b2d2--------------------------------) ## 开源大型语言模型的温和介绍

### 为什么大家都在谈论美洲驼、羊驼、猎鹰和其他动物

[towardsdatascience.com

# 困惑度

语言模型在词汇表上定义概率分布，以选择序列中最可能的下一个词。给定一段文本，语言模型为语言中的每个词分配一个概率，选择最可能的词。

**困惑度** 衡量语言模型在给定序列中预测下一个词的能力。作为一种训练指标，它显示了模型对训练集的学习效果。

我们不会深入探讨数学细节，但直观上，**最小化困惑度意味着最大化预测概率**。

换句话说，最好的模型是当看到新文本时不会感到*惊讶*的，因为它早已预测了序列中接下来会出现的词。

虽然困惑度很有帮助，但它没有考虑单词背后的意义或使用上下文，并且受到我们如何对数据进行分词的影响 — 不同的语言模型采用不同的词汇和分词技术可能会产生不同的困惑度分数，使得直接比较变得不那么有意义。

**困惑度是一个有用但有限的指标**。我们主要用它来跟踪模型训练过程中的进展或比较同一模型的不同版本。例如，在应用量化技术后 — 一种减少模型计算需求的技术 — 我们通常使用困惑度来评估模型质量的变化。

困惑度只是方程式的一部分 — 它提供了有价值的见解，但并不能讲述全部故事。¹

![](img/b8db6afe466fbec575a24d6a365f89c3.png)

大型语言模型的一些任务。图片由作者提供。

# BLEU 和 ROUGE

如果你对自然语言处理感兴趣，你可能听说过**ROUGE 和 BLEU 分数**。

它们在 2000 年代初期引入，用于机器翻译，量化机器文本与人类参考文本的接近程度。

**BLEU** 分数是人类参考文本中的单词数除以总单词数。类似于精确度分数，它的值介于零和一之间，值越接近一表示文本越相似。

**ROUGE** 基于类似的原理，但由于它通过多个方面（如 n-grams（ROUGE-N）、最长公共子序列（ROUGE-L）和跳跃二元组（ROUGE-S））分析重叠，因此略为复杂。

对于大型语言模型，BLEU 和 ROUGE 用于评估输出与被认为正确的人类解决方案的接近程度。但它们并不适用于所有生成任务。正如你所想，生成参考文本可能既昂贵又耗时，对于某些领域或语言甚至不可行。

有时，概括或翻译文本并没有唯一的正确方式。这些分数只能考虑一些有效的选项。

此外，**它们不考虑上下文** — 对于新闻文章有效的文本可能不适合社交媒体帖子，而正式场合适用的内容可能不适合休闲场合。

# 基准测试的需求

开源模型通常较小，并经过微调以更专注于特定任务。

Meta 的创始人**马克·扎克伯格**认为，我们将根据不同的需求与不同的 AI 实体互动，而不是依赖通用的 AI 助手。² 要真正了解哪种模型最适合特定任务，我们需要一种比较它们的方法。

**特定基准**评估语言模型的某个特定方面。例如，如果你想评估你的模型回答的真实性或量化模型在微调后的任务表现，请使用特定基准。

其中四个用于[*Hugging’s Face OpenLLM Leaderboard*](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)。

**抽象推理语料库（ARC）**是一个**抽象推理测试**。它适用于人类和人工智能，并试图测量一种*类似人类的流动智力*。给定一个输入网格，用户需要选择正确的输出。

![](img/14d0a1ee6042b9276a4dc0697f9b298e.png)

ARC 测试接口。语言模型可以通过 JSON 文件与其交互。³

**HellaSwag**是一个测试，用户需要为给定的上下文选择最佳结尾，这一任务称为**常识推理**。虽然对人类来说很容易，但许多 LLM 在这个测试中表现不佳。唯一能够达到接近人类水平表现的是 GPT-4。

![](img/3d60eefdd92c36f9dbc64f2eeaa7b4a3.png)

HellaSwag 中的一个示例测试。⁴

**大规模多任务语言理解（MMLU）**测量文本模型在 57 个任务上的多任务准确性，包括数学、美国历史、计算机科学、法律等。测试看起来像是不同问题的选择题，并评估**对世界和常识的理解**。

![](img/2eea9088510f8fe88d7605b22647c90d.png)

测量大规模多任务语言理解。⁵

**TruthfulQA**包括两个任务：生成和多项选择。生成任务要求模型产生真实和有信息的答案，而多项选择任务要求模型选择或分配真实和虚假答案选项的概率。该基准测试涵盖 57 个主题，并使用各种指标来测量模型**识别虚假信息的能力**。有趣的是，论文显示较大的模型不太真实。⁶

![](img/f0f66f040f86a5563b17fec762908a3b.png)

这些模型的衡量标准是它们识别虚假信息的能力。⁶

# 测量代码生成能力

当 ChatGPT 刚推出时，要求它写一些代码可能是我们都尝试的第一件事。编码能力是 LLM 可以提供给我们的最有用且节省时间的技能之一。

在开源领域，有许多专门从事代码生成的模型，如**Wizard Coder**或最新的**Code LLama**。

为了展示其新**Code Llama**模型令人印象深刻的编码能力，他们选择了两个特定于代码的基准测试：[**HumanEval**](https://github.com/openai/human-eval)和**Mostly Basic Python Programming (**[**MBPP**](https://github.com/google-research/google-research/tree/master/mbpp)**)**，并辅以人工评估。

在第一个基准测试中，模型需要从文档字符串生成代码，而在第二个基准测试中，模型从文本提示开始。

每个提示都会附带一个或多个单元测试，以评估输出的正确性。

![](img/66183ff25f6b5e4d7f8aeeb2daf4674f.png)

MBPP 条目。 [来源。](https://github.com/google-research/google-research/blob/master/mbpp/sanitized-mbpp.json)

在收集到模型生成的**k**个样本后，计算**pass@k 指标**。如果至少一个样本通过了单元测试，则认为解决方案正确。例如，**pass@1 分数为 67.0 意味着模型在第一次尝试中可以解决 67%的问题。**

计算此指标时，你可以使用任何**k**的值。但实际上，我们关注的是**pass@1**。如果你必须不断尝试才能获得正确的解决方案，你如何相信那个模型？

**Code LLama**的评估结果如下。

![](img/1041feff23bbd2b453ee4f3ef6d97865.png)

Code Llama 评估。⁷

他们的结果显示，GPT-4 是最好的模型，能够在**HumanEval**中第一次尝试解决 67%的任务。然而，**Code Llama 是最好的开源代码专用模型，只有 34B 参数。**

# 测量通用智能

评估系统必须覆盖众多场景，尤其是针对大型通用语言模型，因为它们在处理多样任务时表现出令人印象深刻的泛化能力。

对于经典的机器学习模型，你习惯使用*测试集*来评估模型，而 LLMs 使*零样本学习*和*少样本学习*成为可能——LLM 可以学习执行没有明确训练过的任务。在这种情况下，使用测试集或单一指标来基准 LLM 的能力是不够的。

**通用基准**是各种场景和任务的广泛测试集合。它们像是对模型的终极测试，旨在评估智能的每个方面。

一些评估体系如**语言模型的整体评估（HELM），旨在根据七个关键指标评估**模型：准确性、校准和不确定性、稳健性、公平性、偏见和刻板印象、毒性和效率，计算涉及 16 种场景。

![](img/48761bd94d5f661f7a1a6fee5d8953c7.png)

语言模型的整体评估。⁸

**SuperGLUE**，于 2019 年推出，是通用语言理解评估（GLUE）测试的高级版本。GLUE 基准包括九项与句子或句子对语言理解相关的任务，所有任务都建立在现有数据集上。**SuperGLUE**提供了一组更具挑战性的任务和一个公开的排行榜。

**BIG-bench**，来自谷歌，扩展了 GLUE 和 SuperGLUE，包含了更多自然语言理解任务的广泛集合。这是一个庞大的协作项目，由来自 132 个机构的 444 位作者贡献。它基于[超过 200 个任务](https://github.com/google/BIG-bench/blob/main/bigbench/benchmark_tasks/keywords_to_tasks.md#summary-table)评估 LLMs 的准确性、流畅性、创造力和泛化能力！

由于运行 BIG-bench 可能非常耗时，作者还提供了一个包含 24 个任务的轻量版，称为 BIG-bench lite。他们的 GitHub 仓库开放供贡献和新想法。

![](img/fe51824a9ddcda3ce58ea568cafbf8a1.png)

BIG-bench 任务的多样性和规模。⁹

评估语言模型的另一种方法是手动 **人工评估**。顾名思义，它通过要求人工评审员对 LLM 的输出进行评分或比较来衡量大型语言模型的质量和性能，类似于 **Chatbot Arena**。这是一个使用 Elo 评分系统（类似于国际象棋）对大型语言模型（LLMs）进行基准测试的平台，用户可以并排与两个匿名 LLM 进行对话，并投票选择他们认为最好的一个。然后，这些投票用于计算 ELO 评分，并在排行榜上对 LLM 进行排名。

你可以访问 [他们的网站](https://chat.lmsys.org/) 并与不同的 LLM 进行对话。

![](img/5416b49cc003e19491c9faffc0905bc4.png)

Chatbot Arena [排行榜](https://chat.lmsys.org/)

# 研究案例：Llama 2

**Llama 2** 是 Llama 的继任者。它于 2023 年 7 月发布，具有 7B、13B、34B 和 70B 的不同规模，包括一个微调版本，称为 **Llama 2 Chat**。

论文中，我们可以找到两种主要的评估程序：**综合评估**和**安全评估**。

![](img/94b3490d50e78389ac1abdfb88e555e0.png)

综合评估。Llama 2 是对 Llama 的改进，得分优于 MPT 和 Falcon。¹⁰

作者工作的评估标准表明，他们优先考虑了两个主要目标。

首先，**将 Llama 2 与第一个版本以及开源竞争对手进行比较。** 为了实现这一目标，他们使用了全面的通用评估，其中模型在五个维度上进行评估：**代码、常识推理、世界知识、阅读理解**和**数学**。每个维度是多个基准测试的平均值。

结果还通过 **MMLU、BBH（BigBench Hard）和 AGI Eval 基准测试** 进行补充，这些基准测试在单独的列中显示。

作者工作的第二个目标显而易见，即 **展示他们的微调方法使模型更真实且有害性更低。**

![](img/dfa33942088c45fa0d30d7d2f2ac30b6.png)![](img/4f049abf17b90b2e6a3eabecc334c478.png)

微调前的 LLM（左）和微调后的 LLM（右）¹⁰

安全评估旨在使用 **TruthfulQA** 和 **ToxiGen** 基准测试来评估 **真实性** 和 **有害性**。

结果表明，得益于微调过程，**Llama 2 比其他模型的有害性更低，但比 ChatGPT 的真实性略逊。**

# 结论

语言模型具有多方面和灵活的特性。

开源模型提供了量身定制的解决方案，专业化可能是未来的发展方向。

在比较模型时，请寻找与您的需求相关的基准测试。

最好的模型不一定是困惑度最低或 BLEU 分数最高的模型，而是那个真正为您的生活增添价值的模型。

*如果您喜欢这篇文章，请加入* [***Text Generation***](https://textgeneration.substack.com/) *——我们的新闻通讯每周发布两篇文章，提供有关生成式 AI 和大型语言模型的最新见解。*

*此外，您还可以在* [***LinkedIn***](https://www.linkedin.com/in/driccio/)***上找到我。***

# 参考资料

¹ M. Shoeybi 和 R. Caruana，[语言模型评估超越困惑度](https://arxiv.org/abs/2106.00085) (2023), arXiv.org

² Lex Friedman Podcast，[马克·扎克伯格：人工智能的未来](https://www.youtube.com/watch?v=Ff4fRgnuFgQ&t=2723s) (2023), YouTube.

³ Xu, Y., Li, W., Vaezipoor, P., Sanner, S., & Khalil, E. B., [LLMs 与抽象与推理语料库：成功、失败以及基于对象的表示的重要性。](https://arxiv.org/abs/2305.18354) (2023), arXiv.org

⁴ Zellers, R. 等人，[HellaSwag: 机器真的能完成你的句子吗？](https://arxiv.org/abs/1905.07830) (2022), arXiv.org

⁵ Hendrycks, D., Burns, C., Basart, S., Zou, A., Mazeika, M., Song, D., & Steinhardt, J. [测量大规模多任务语言理解。](https://arxiv.org/abs/2009.03300) (2021), arXiv.org

⁶ Lin, S., Hilton, J., & Evans, O. (2021). [TruthfulQA: 测量模型如何模仿人类的虚假信息。](https://arxiv.org/abs/2109.07958) (2022), arXiv.org

⁷ [介绍 Code Llama，一种最先进的大型语言模型用于编码](https://ai.meta.com/blog/code-llama-large-language-model-coding/)。 (2023), meta.com

⁸ Liang, P. 等人 (2022)。[语言模型的整体评估](https://arxiv.org/abs/2211.09110)。 (2022), arXiv.org

⁹ Srivastava, A. 等人。[超越模仿游戏：量化和外推语言模型的能力。](https://arxiv.org/abs/2206.04615) (2022), arXiv.org

¹⁰ Touvron, H. 等人 (2023)。[Llama 2：开放基础和微调的聊天模型](https://arxiv.org/abs/2307.09288)。 (2023), arXiv.org
