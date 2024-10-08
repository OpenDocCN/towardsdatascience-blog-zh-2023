# Google Med-PaLM：AI 临床医生

> 原文：[`towardsdatascience.com/google-med-palm-the-ai-clinician-a4482143d60e`](https://towardsdatascience.com/google-med-palm-the-ai-clinician-a4482143d60e)

## 人工智能 | 医学 | 自然语言处理 |

## 谷歌的新模型经过训练以回答医学问题。怎么做到的？

[](https://salvatore-raieli.medium.com/?source=post_page-----a4482143d60e--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----a4482143d60e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4482143d60e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4482143d60e--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----a4482143d60e--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4482143d60e--------------------------------) ·14 min read·2023 年 3 月 17 日

--

![](img/343cb55205bd7b26fac1907a4976f5de.png)

作者使用 OpenAI DALL-E 制作的图片

医学还基于患者和医生之间的互动。此外，尽管患者接受了不同的检查和影像技术，但总是会有书面报告。那么，为什么医疗和健康领域的 AI 模型未能充分利用语言呢？

# 医学的基础模型？

![](img/9568bb410aa816f7d631aa907308b6bc.png)

图片由 [Myriam Zilles](https://unsplash.com/it/@myriamzilles) 在 Unsplash 上拍摄

近年来的趋势是尝试限制大型 [语言模型](https://en.wikipedia.org/wiki/Language_model) (LMs)，然后 [微调](https://en.wikipedia.org/wiki/Fine-tuning_(machine_learning)) 以满足所需的应用。类似的方法可以应用于大量医学文本，使模型能够学习有用的表征。对医学主题有良好理解的模型可能对无数应用有帮助（例如，患者分诊、知识检索、关键发现的总结、诊断辅助等）。

问题在于医学领域是一个特殊领域。与其他领域相比，这里有不同的问题，甚至更大的安全问题。正如我们所见，像 ChatGPT 这样的模型也可能产生幻觉，并且可能传播错误信息。

[](https://medium.com/data-driven-fiction/everything-but-everything-you-need-to-know-about-chatgpt-546af7153ee2?source=post_page-----a4482143d60e--------------------------------) [## 关于 ChatGPT 的一切你需要知道的内容

### 了解已知信息、最新消息、其影响以及正在变化的情况。所有这些都在一篇文章中。

[medium.com](https://medium.com/data-driven-fiction/everything-but-everything-you-need-to-know-about-chatgpt-546af7153ee2?source=post_page-----a4482143d60e--------------------------------)

谷歌的一项新研究专注于利用广泛的语言模型编码临床知识，并评估其在医学中的潜力。他们决定从一个具体任务开始：[医学问答](https://en.wikipedia.org/wiki/Question_answering)。这是因为它是一个基本但也困难的任务：模型必须提供高质量的医学问题答案。为此，模型还必须理解医学背景，找到相关信息，并依据专家的问题进行推理。

[](https://arxiv.org/abs/2212.13138?source=post_page-----a4482143d60e--------------------------------) [## 大型语言模型编码临床知识

### 大型语言模型（LLMs）在自然语言理解方面展示了令人印象深刻的能力…

[arxiv.org](https://arxiv.org/abs/2212.13138?source=post_page-----a4482143d60e--------------------------------)

事实上，为医学创建语言模型（LM）的想法并不新鲜。事实上，多年来已经有过尝试。这是因为 LM 可以通过大量文本（通常是书籍或维基百科等通用文本）以[无监督的方式](https://en.wikipedia.org/wiki/Unsupervised_learning)进行训练。

模型在没有具体任务的情况下进行训练，但正如扩展法则所示，LM 能够展现出使其能够适应特定任务的突现行为，而无需[梯度更新](https://en.wikipedia.org/wiki/Gradient_descent)。一个例子是在上下文[少样本学习](https://paperswithcode.com/task/few-shot-learning)中，模型能够“快速概括到未见任务，甚至表现出明显的推理能力，配合适当的提示策略。”此外，模型隐式地充当知识库，但也有放大训练数据集中存在的偏见的缺点。

无论如何，已经尝试了几种方法，因为有数百万篇医学文章和无数医学数据可以利用。早期模型基于[BERT](https://en.wikipedia.org/wiki/BERT_(language_model))（sciBERT、BioBERT、PubMedBERT、DARE、ScholarBERT）。此外，也尝试过基于 GPT 架构的模型，如最近的 BioGPT。

[](https://levelup.gitconnected.com/microsoft-biogpt-towards-the-chatgpt-of-life-science-56e251536af6?source=post_page-----a4482143d60e--------------------------------) [## 微软 BioGPT：迈向生命科学的 ChatGPT？

### BioGPT 在不同生物医学自然语言处理任务中实现了**SOTA**。

[levelup.gitconnected.com](https://levelup.gitconnected.com/microsoft-biogpt-towards-the-chatgpt-of-life-science-56e251536af6?source=post_page-----a4482143d60e--------------------------------)

> 那么这项研究带来了什么新的东西呢？

+   一个允许更好评估语言模型在医学问答中的新数据集。

+   在医学问答基准上达到最先进的结果。

+   指令提示调整以提高对医学领域的对齐。

+   对语言模型在医学领域的局限性的深入分析。

![](img/8af892e30991b128b683835200dd4ad1.png)

“我们贡献的概述：我们策划了 MultiMedQA，一个涵盖医学考试、医学研究和消费者医学问题的医学问答基准。我们在 MultiMedQA 上评估了 PaLM 及其指令调整变体 Flan-PaLM。通过组合提示策略，Flan-PaLM 在 MedQA（USMLE）、MedMCQA、PubMedQA 和 MMLU 临床主题上超越了 SOTA 性能。特别是，它在 MedQA（USMLE）上的表现比之前的 SOTA 提高了超过 17%。接下来，我们提出了指令提示调整以进一步将 Flan-PaLM 与医学领域对齐，生成了 Med-PaLM。在我们的人类评估框架下，Med-PaLM 对消费者医学问题的回答与临床医生生成的回答相比表现良好，展示了指令提示调整的有效性”。图像来源：[这里](https://arxiv.org/pdf/2212.13138.pdf)

# 如何评估医学中的语言模型？

![](img/ff3b511131ef7bb5e3cf91e9635d4a60.png)

图片来源：[在线营销](https://unsplash.com/it/@impulsq) 在 Unsplash

首先，我们需要一个好的数据集。作者指出，尽管有几个用于研究的数据集，但每个数据集都关注于特定的方面或任务：医学考试问题、医学考试问题和医学信息需求的有用答案。

> 我们承认医学知识在数量和质量上都是巨大的。现有基准固有地有限，只提供了医学知识空间的部分覆盖。然而，将多个不同的数据集合并用于医学问答，使得对 LLM 知识的评估比选择题准确性或诸如 BLEU 这样的自然语言生成指标更为深入。([来源](https://arxiv.org/pdf/2212.13138.pdf))

作者换句话说，一个数据集是不够的，因为该领域相当广泛。此外，评估诸如[BLEU](https://en.wikipedia.org/wiki/BLEU)（或其他指标）这样的指标并不能展示模型理解领域的能力。

新构建的数据集要求模型能够回答选择题、开放性问题（长形式）、封闭领域（答案必须在参考文本中找到）和开放领域问题（特定来源中信息有限）。

总结来说，作者通过结合已经使用的数据集和策划的常见健康查询数据集，构建了一个新的基准。整个数据集均为英文，并涵盖了医学考试、医学搜索，甚至是消费者查询。还有标签和元数据。

![](img/0780b401e6f6868091afb988526d523c.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

鉴于领域的复杂性，他们通过临床医生编写的答案丰富了数据集。此外：

> 其次，鉴于医疗领域的安全关键要求，我们认为有必要超越使用 BLEU 等指标的自动化长答案生成质量测量，转向涉及更细致的人类评估框架，如本研究中提出的框架。

一个多项选择题的示例：

![](img/81a27b472eb3a865a3a25eac54617e9a.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

以及长答案问题：

![](img/98590bf8dee606642f7030f06489c12f.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

作者们随后定义了一个框架，以便临床医生能够衡量模型的稳健性。的确，尽管使用指标很有用，但它忽略了许多重要细节，在医学背景下可能会产生误导。

作者们使用了来自英国、美国和印度的临床医生的焦点小组和访谈来定义评估轴。此外，他们强调了“与科学共识的一致性、伤害的可能性和概率、答案的完整性和缺失情况以及偏见的可能性。”

![](img/ce46d4c18f0d48e4c1d8781455911f0d.png)

“总结了临床医生在我们的消费者医疗问题回答数据集中评估答案的不同方面。这些包括与科学共识的一致性、伤害的可能性和概率、理解证据、推理和检索能力、不适当、错误或缺失内容的存在以及答案中的偏见可能性。我们使用了一组临床医生来评估模型和人工生成答案在这些方面的质量。” ([source](https://arxiv.org/pdf/2212.13138.pdf))

表格总结了表单呈现的问题种类。正如作者所述，伤害和偏见都是复杂的概念，没有单一答案。例如，伤害可以在不同层面上定义：“身体健康、心理健康、道德、财务等。”因此，作者创建了一个包含不同问题的表单，并将其提供给不同国家（美国、英国和印度）的临床医生。

另一方面，并非每个人都有医学知识，因此作者决定评估答案对消费者的帮助性和实用性。他们创建了一个表单，并由没有医学背景的人进行评分：

> 这项工作旨在评估答案如何应对问题背后的感知意图，以及答案的帮助性和可操作性如何。 ([source](https://arxiv.org/pdf/2212.13138.pdf))

![](img/c0a52b11b34f755350c77c59073dc84d.png)

“总结了普通用户在我们的消费者医学问答数据集中评估答案实用性的不同轴向。我们使用了 5 位非专家普通用户来评估模型和人工生成答案在这些轴向上的质量。” ([source](https://arxiv.org/pdf/2212.13138.pdf))

# 哪种模型？

![](img/eb4355ee3e3daf25ecac6bb4d5625809.png)

图片由 [Kimon Maritz](https://unsplash.com/it/@kimonmaritz) 提供，来源于 Unsplash

作者开始使用 Pathways 语言模型（PaLM）和 Flan-PaLM 家族。

[PaLM](https://arxiv.org/abs/2204.02311) 是“一种密集激活的解码器-only 变换器语言模型，使用 Pathways 进行训练。” 该模型使用了包括互联网数据、维基百科、源代码、社交媒体对话和书籍在内的大规模语料库进行训练。PaLM 最大版本包含 540 B 的参数，并在多个基准测试中达到了最先进的水平。

[Flan-PaLM](https://arxiv.org/abs/2210.11416) 是 PaLM 的指令调整版。Flan-PaLM 使用了不同的数据集进行指令调整。如上所示，使用思维链使模型能够更好地泛化。

[](/multimodal-chain-of-thoughts-solving-problems-in-a-multimodal-world-961a8ab9d0fa?source=post_page-----a4482143d60e--------------------------------) ## 多模态思维链：在多模态世界中解决问题

### 世界不仅仅是文本：如何将思维链扩展到图像和文本？

towardsdatascience.com

一旦我们拥有模型，主要的问题就是如何将其适应医学领域：

> 然而，考虑到医学领域的安全关键性质，有必要将模型适应并对齐领域特定的数据。 ([source](https://arxiv.org/pdf/2212.13138.pdf))

作者决定使用提示和提示调整作为策略。正如所示，语言模型是少量示例学习者（需要少量示例）以进行上下文学习。换句话说，通过一些精心挑选的提示示例，模型可以在没有任何梯度更新或微调的情况下学习新任务。作者使用了三种提示策略：

+   **少量提示**。通过基于文本的描述（输入-输出对）描述任务的少量示例。最佳演示是在与合格临床医生一致的情况下完成的（针对每个数据集）。

+   **思维链提示**。在提示中添加一系列中间推理步骤以得出最终答案（这种方法模仿了人类在解决问题时的推理过程）。这些提示也与临床医生一起创建。

+   **自一致性提示。** 提高模型在多项选择题中表现的一种策略是从模型中采样多个解码输出（最终答案将是获得多数票的那个）。‘其背后的想法是，在复杂领域中，可能有多条路径可以达到正确答案。

如前所述，提示方法使得相对低成本地改进模型成为可能（微调如此大型的模型计算成本高）。但是提示对许多任务来说是不够的，它们将从微调中受益。

![](img/225390265af7e9c7bec1662b4451a6c6.png)

由[Anton Shuvalov](https://unsplash.com/it/@a8ka)在 Unsplash 拍摄

> 如何对 540B 模型进行微调？

**答案：提示微调。** 简而言之，提示微调是一些提示（由人类或 AI 模型生成的向量）用于引导模型完成预期任务。提示有两种类型，一种是由人类编码的（硬提示），另一种是通过反向传播学习到的（软提示）。这将可学习的参数缩小到仅表示少量标记（其余模型被冻结）。

该研究的作者使用了软提示和相关任务特定的人类设计提示（硬提示）。

> 我们将这种提示微调的方法称为“指令提示微调”。指令提示微调因此可以被视为一种轻量级的方法（数据高效、参数高效、训练和推理过程中计算高效）来训练模型以遵循一个或多个领域的指令。 ([source](https://arxiv.org/pdf/2212.13138.pdf))

然后，他们在一小部分示例上使用了指令提示微调，将 Flan-PaLM 适配到医学领域。由于这些示例较少，它们必须是“医学理解、临床知识回忆和医学知识推理方面的良好示例，不容易导致患者伤害。” **换句话说，这些示例特别与医学专家（来自不同学科的临床医生）合作精心策划。**

![](img/4c33efd486cdae4b521426e2d6d28d82.png)

医疗领域的指令提示微调 ([source](https://arxiv.org/pdf/2212.13138.pdf))

# Med-PaLM：它是否比其他模型更好？

![](img/725e4eb56ffff52d1ce66c18399679d4.png)

图像由[Steven Lelham](https://unsplash.com/it/@slelham)在 Unsplash 拍摄

模型已达到并超越了现有技术水平：

+   **MedQA，** 涵盖一般医学知识的多项选择题（美国医学执照考试）。

+   **MedMCQA，** 来自印度的医学入学考试问题（多项选择题）。

+   **PubMedQA，** 生物医学科学文献。

+   **MMLU，** 多项选择题涵盖临床知识、医学和生物学的各种主题。相关主题

![](img/bc23ead57fefeff0dfbf0671815c9764.png)

左侧：SOTA LLMs 在 MMLU 临床主题上的比较。右侧：PaLM 和 Flan-PaLM 模型在不同模型尺寸变体上的表现总结。改编自[这里](https://arxiv.org/pdf/2212.13138.pdf)。

结果在不同临床主题类别中也保持一致，显示出 Flan-PaLM 在所有类别中都达到了 SOTA 水平。

![](img/5a1a8442e839c0b0339d2f7ccbfcfa8b.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

作者们还决定评估模型在不同模型尺寸下的表现，使用了 MultiMedQA 中的医学问答数据集。这表明，当使用少量示例提示时，扩展模型规模可以提升模型性能。同时，结果还表明，指令微调相比基线提高了性能。

![](img/c312603e59ab7c5c6e127119c7ea695d.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

此外，作者提到了两个有趣的因素：

+   首先，思维链（CoT）提示在这种情况下并没有带来改进（这实际上令人惊讶）。

+   自一致性（SC）在多项选择题表现上带来了显著改善（这是预期中的结果）。

![](img/75aae3ea568125c02926e621d456bc9a.png)

左侧：Flan-PaLM 模型在少量示例和思维链（CoT）提示下的表现总结。右侧：Flan-PaLM 模型在有无自一致性提示（SC）下的表现总结。改编自[这里](https://arxiv.org/pdf/2212.13138.pdf)。

该模型也能够生成解释，说明它为何选择了特定的响应：

![](img/cf5f5abfc0a4a51b49f327f99544bcc6.png)

“Flan-PaLM 540B 模型生成的示例解释，以支持其多项选择答案”（[source](https://arxiv.org/pdf/2212.13138.pdf)）

语言模型（LMs）可能会产生幻觉，在医学背景下，这可能是灾难性的。因此，作者调查了 LLM 不确定性与陈述准确性之间的关系。换句话说，他们使用了模型置信度，发现更高的置信度对应着更高的准确性。

![](img/b60eaf440b2032888b2bfb8bd37898f2.png)

Flan-PaLM 540B 模型的推迟行为分析（[source](https://arxiv.org/pdf/2212.13138.pdf)）

# 该模型是否能说服临床医生？

![](img/07d5920df08e760c201adcc2ff243f3d.png)

图片由[Sander Sammy](https://unsplash.com/it/@sammywilliams)提供，来自 unsplash

> 最佳准确性是否足以在临床中使用该模型？

指标很重要，但可能会产生误导。特别是在像医学这样敏感的领域中，需要的不仅仅是基准测试的结果。

作者选择了 100 个可能代表真实消费者询问的问题。随后，他们使用 Flan-PaLM 和 Med-PaLM（两个 540B 模型）来预测回答，并将其提交给了一个由 9 位临床医生（来自美国、英国和印度）组成的评审小组。

尽管临床医生在 92%的问题上达成了科学共识，但 Flan-PaLM 在 61%的情况下达成了一致。

> 这表明，单独的通用指令微调不足以产生科学和临床上可靠的答案。然而，我们观察到 92.9%的 Med-PaLM 答案被评定为符合科学共识，展示了指令提示微调作为对齐技术在产生科学基础答案方面的优势。 ([source](https://arxiv.org/pdf/2212.13138.pdf))

![](img/8a8188548cf9d4e6e71bf935f7890619.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

模型是在以前发布的文章和书籍上进行训练的，因此作者指出这可能是失败的原因之一，未来应继续探索学习。

然后，作者询问临床医生模型在理解、知识检索和医学知识推理能力方面是否存在错误。在这里，Med-PaLM 再次表现优于 Flan-PaLM。

![](img/ca4d8f18fc551235e6c9c0cb1f9657f7.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

作者询问答案是否包含错误或缺失内容（生成答案的完整性和正确性）。也就是说，模型是否遗漏了应有的信息，或者答案中是否有不应包含的信息。Med-PaLM 的答案在 15%的情况下显示了重要信息的遗漏（相比之下，Flan-PaLM 为 47%）。令人惊讶的是，Med-PaLM 的错误率高于 Flan-PaLM（18%对 16%）。作者对此结果的解释如下：

> 指令提示微调使 Med-PaLM 模型生成的答案显著比 Flan-PaLM 模型更详细，从而减少了重要信息的遗漏。然而，较长的答案也增加了引入不正确内容的风险。 ([source](https://arxiv.org/pdf/2212.13138.pdf))

![](img/0534f0ca83ee534748e1450b035d43bc.png)

作者还探讨了生成响应可能带来的潜在危害的严重性和可能性。换句话说，他们询问这些响应是否可能导致临床医生或消费者/患者采取可能造成健康相关伤害的行动。尽管定义是相对的，评分在这种情况下是主观的，但与基线模型相比，指令微调产生了更安全的响应。

![](img/cc23eb96fb08305a170cab55a911dd7a.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

此外，作者分析了模型在医疗保健中放大偏见的潜在风险。该模型可能反映或放大训练数据中存在的反映健康结果和护理获取差异的模式。结果显示，新方法显著降低了偏见风险。

![](img/7fcc6f131454969819dde4f393a0c267.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

最后，作者分析了非专家如何评估答案。

> 尽管 Flan-PaLM 的回答被评估为在 60.6%的情况下有帮助，但 Med-PaLM 的回答这一数字提高到了 80.3%。 ([source](https://arxiv.org/pdf/2212.13138.pdf))

但仍然不如临床医生。通过询问非专家消费者回答是否直接回答了用户的问题，得到了类似的结果。

![](img/d750ef52e72706b4d54390645c6ae773.png)

([source](https://arxiv.org/pdf/2212.13138.pdf))

# 模型的局限性

![](img/5e0fc29daf1d272b0aaaa2d0a5d6ef9a.png)

由 [Joshua Hoehne](https://unsplash.com/it/@mrthetrain) 在 unsplash 拍摄

作者指出了研究的若干限制和未来方向：

+   **他们提出的数据集（MultiMedQA）并不全面**（尽管包含了多个来源）。例如，它在生物学方面存在不足。此外，模型还应包括更贴近现实世界的问题和答案（多项选择题容易填补，但与现实世界相距甚远）。

+   与专家的性能评估显示**该模型尚不具备临床医生的水平**。

+   **相同的人类评估方法可以改进。** 当然，它是有限的，专家的数量应该增加。共识的概念是与上下文和时间相关的。此外，科学共识往往没有考虑到少数群体，因此可能本身就是偏见的来源。更不用说它受到了临床医生自身背景的影响。

+   **偏见和伤害的分析是有限的**，考虑到这是探索性工作。另一方面，医疗领域是一个极其敏感的领域，这些都是不能忽视的伦理问题。因此，分析应该扩展到包括患者。此外，缺乏类似任务的特定基准数据集。

# 最后的思考

![](img/1b50ca35b6b4941334b3a4bf4ed4faee.png)

由 [Amine rock hoovr](https://unsplash.com/it/@hoovr01) 在 Unsplash 拍摄

这篇论文展示了如何通过指令提示调优来改善模型在医疗问答等复杂领域的表现。然而，这种行为随着模型规模的增加而出现。此外，与其他模型相比，该模型实现了最先进的技术水平。

PaLM 的 540 B 版本单独仍然能够取得显著的成果。可能训练数据包含了多个医学来源，模型在参数中存储了这些信息。

与人类专家的评估显示，仅仅扩大规模并不够。即使是 Med-PaLM 自身也可能产生不完整或错误的回答。

无论如何，目前仍然为时尚早，尚无法在医疗保健中使用这样的模型。首先，需要更多的研究以确保模型的安全性。虽然目前很难假设使用它来治疗疾病，但可以考虑作为一种向患者提供有关疾病和药物信息的方法。

另一方面，医生也有偏见，而语言模型可能是高效的助手。未来，语言模型还可能在减轻偏见和提供更广泛的治疗选择方面发挥作用。

最后，谷歌发布了 PaLM 的 API，旨在用于原型设计和构建生成型 AI 应用程序（[更多信息](https://developers.googleblog.com/2023/03/announcing-palm-api-and-makersuite.html)）

# 如果你觉得这很有趣：

你可以查看我的其他文章，也可以[**订阅**](https://salvatore-raieli.medium.com/subscribe)以便在我发布文章时收到通知，还可以在[**LinkedIn**](https://www.linkedin.com/in/salvatore-raieli/)**上联系我**。

这里是我的 GitHub 库链接，我计划在这里收集与机器学习、人工智能等相关的代码和资源。

[](https://github.com/SalvatoreRa/tutorial?source=post_page-----a4482143d60e--------------------------------) [## GitHub - SalvatoreRa/tutorial: 机器学习、人工智能、数据科学的教程…

### 机器学习、人工智能、数据科学的教程，包含数学解释和可重用的代码（Python…

github.com](https://github.com/SalvatoreRa/tutorial?source=post_page-----a4482143d60e--------------------------------)

或者你可能对我最近的文章感兴趣：

[](https://pub.towardsai.net/pca-bioinformaticians-favorite-tool-can-be-misleading-fe139262a576?source=post_page-----a4482143d60e--------------------------------) [## PCA：生物信息学家的最爱工具可能具有误导性

### 一项新的研究评估了一个最常用的技术可能存在的问题

pub.towardsai.net](https://pub.towardsai.net/pca-bioinformaticians-favorite-tool-can-be-misleading-fe139262a576?source=post_page-----a4482143d60e--------------------------------) [](https://levelup.gitconnected.com/stable-diffusion-and-the-brain-how-ai-can-read-our-minds-45398b395ea9?source=post_page-----a4482143d60e--------------------------------) [## 稳定扩散与大脑：AI 如何读取我们的思想

### 研究人员能够使用 fMRI 数据重建图像

levelup.gitconnected.com](https://levelup.gitconnected.com/stable-diffusion-and-the-brain-how-ai-can-read-our-minds-45398b395ea9?source=post_page-----a4482143d60e--------------------------------) [](https://levelup.gitconnected.com/stable-diffusion-to-fill-gaps-in-medical-image-data-b78a2a7d6c9d?source=post_page-----a4482143d60e--------------------------------) [## 稳定扩散填补医疗图像数据的空白

### 一项新的研究表明，稳定扩散可能有助于医学图像分析和稀有疾病。如何？

[levelup.gitconnected.com](https://levelup.gitconnected.com/stable-diffusion-to-fill-gaps-in-medical-image-data-b78a2a7d6c9d?source=post_page-----a4482143d60e--------------------------------) [为什么我们有庞大的语言模型和小型视觉变换器？](https://levelup.gitconnected.com/stable-diffusion-to-fill-gaps-in-medical-image-data-b78a2a7d6c9d?source=post_page-----a4482143d60e--------------------------------)

### Google ViT-22 为新一代大型变换器铺平了道路，并有望革新计算机视觉

towardsdatascience.com [为什么我们有庞大的语言模型和小型视觉变换器？](https://levelup.gitconnected.com/stable-diffusion-to-fill-gaps-in-medical-image-data-b78a2a7d6c9d?source=post_page-----a4482143d60e--------------------------------)
