# Behind the Millions: Estimating the Scale of Large Language Models

> 原文：[https://towardsdatascience.com/behind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b?source=collection_archive---------0-----------------------#2023-03-31](https://towardsdatascience.com/behind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b?source=collection_archive---------0-----------------------#2023-03-31)

## Discussing LLMs like ChatGPT, the underlying costs, and inference optimization approaches

[](https://medium.com/@andimid?source=post_page-----97bd7287fb6b--------------------------------)[![Dmytro Nikolaiev (Dimid)](../Images/4121156b9c08ed20e7aa620712a391d9.png)](https://medium.com/@andimid?source=post_page-----97bd7287fb6b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----97bd7287fb6b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----97bd7287fb6b--------------------------------) [Dmytro Nikolaiev (Dimid)](https://medium.com/@andimid?source=post_page-----97bd7287fb6b--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F97b5279dad26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbehind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b&user=Dmytro+Nikolaiev+%28Dimid%29&userId=97b5279dad26&source=post_page-97b5279dad26----97bd7287fb6b---------------------post_header-----------) Published in [Towards Data Science](https://towardsdatascience.com/?source=post_page-----97bd7287fb6b--------------------------------) ·13 min read·Mar 31, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F97bd7287fb6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbehind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b&user=Dmytro+Nikolaiev+%28Dimid%29&userId=97b5279dad26&source=-----97bd7287fb6b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F97bd7287fb6b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbehind-the-millions-estimating-the-scale-of-large-language-models-97bd7287fb6b&source=-----97bd7287fb6b---------------------bookmark_footer-----------)![](../Images/216ddba9f024c656c8b7a89b5e2e8a50.png)

Photo by [Pixabay](https://www.pexels.com/photo/hard-cash-on-a-briefcase-259027/)

> 感谢[Regan Yue](https://medium.com/u/84ea27feb7de?source=post_page-----97bd7287fb6b--------------------------------)，你可以在[mp.weixin.qq.com](https://mp.weixin.qq.com/s/ek9Z9E-T04BBP7g7rl2Eqw)、[juejin.cn](https://juejin.cn/post/7225058326575693884)、[segmentfault.com](https://segmentfault.com/a/1190000043715456)和[xie.infoq.cn](https://xie.infoq.cn/article/e6d832cc0db3203e661647960)阅读这篇文章的中文版本！

在近期过去，机器学习被视为一种复杂的、小众的技术，只有少数人能够理解。然而，随着机器学习应用变得越来越强大，公众的兴趣激增，围绕人工智能的内容也变得极其丰富。这一高潮发生在[2022年11月，当我们看到ChatGPT](https://openai.com/blog/chatgpt)，并在[2023年3月GPT-4发布时](https://openai.com/research/gpt-4)，即使是最怀疑的人也对现代神经网络的能力感到惊讶。

![](../Images/c89106d2f2cccd701ed6952812eb5b11.png)

询问ChatGPT关于其能力的问题。图片由作者使用[ChatGPT](https://chat.openai.com/chat)创建

尽管这些内容中有些无疑是有价值的，但其中大量内容传播了**恐惧**和**错误信息**，例如*传播机器人将取代所有人类工作*或*发现神秘的方法通过神经网络赚取巨额财富*。因此，澄清有关机器学习和**大型语言模型**的误解，并提供有用的信息以帮助人们更好地理解这些技术，变得越来越重要。

本文旨在讨论现代机器学习中一个常被忽视或误解的关键方面——训练大型语言模型的成本。同时，我们将简要了解什么是LLM以及一些可能的优化推理技术。通过提供详细的示例，我希望能让你相信这些技术*并非凭空而来*。通过了解**数据的规模和底层计算**，你将更好地理解这些强大的工具。

我主要依赖于最近的[LLaMA论文，由Meta AI发布](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/)，因为它清楚地说明了团队用于训练这些模型的数据量和计算资源。本文将分为以下几个部分：

1.  首先，我们将简要了解**现代大型语言模型是什么**；

1.  接着，我们讨论**训练这些模型的成本**；

1.  最后，我们将简要考虑一些**优化语言模型推理的流行技术**。

请继续关注，我们将深入探讨大型语言模型的世界，你会发现一切既*非常简单*又*非常复杂*。

# 大型语言模型简介

在我们深入探讨训练大型语言模型（LLMs）相关的成本之前，让我们先简要定义一下什么是语言模型。

![](../Images/30e42d2c00e8ab3c7dd6aade7470950b.png)

2018-2019年发布的几种语言模型的参数计数。现代LLM通常有数十亿到数百亿的参数。图1来自[DistilBERT论文](https://arxiv.org/pdf/1910.01108.pdf)

简单来说，语言模型是一种旨在理解或生成自然语言的机器学习算法。最近，**生成模型**变得越来越受欢迎 —— 由OpenAI开发的**GPT模型系列**：ChatGPT、GPT-4等（代表*生成预训练变换器*，基于[*变换器*架构](https://huggingface.co/course/chapter1/4)）。

虽然不太流行，但仍然重要的例子包括[GPT-3 (175B)](https://en.wikipedia.org/wiki/GPT-3)、[BLOOM (176B)](https://bigscience.huggingface.co/blog/bloom)、[Gopher (280B)](https://www.deepmind.com/blog/language-modelling-at-scale-gopher-ethical-considerations-and-retrieval)、[Chinchilla (70B)](https://arxiv.org/abs/2203.15556)和[LLaMA (65B)](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/)，其中*B*指*十亿*个参数，尽管这些模型中许多也有较小的版本。

> 关于ChatGPT特别是GPT-4的参数数量一无所知，但看起来这些数量大致相同。

![](../Images/a2016210338f77ae9153992dbf03dd4d.png)

一些流行的LLM架构。图像由作者提供

这些模型通过使用大量的文本数据进行“训练”，使它们能够学习自然语言的复杂模式和结构。然而，它们在训练过程中解决的任务非常简单：它们只是预测序列中的下一个词（或*标记*）。

你可能听说过这样一种模型叫做**自回归**，这意味着它*使用过去的输出作为未来预测的输入*，并*一步一步生成输出*。这可以在ChatGPT的示例中看到：

![](../Images/66aef20a3c0122577e3ba5851f4df307.png)

GhatGPT生成了一个响应。Gif由作者使用[ChatGPT](https://chat.openai.com/chat)创建

你可以注意到模型生成答案是*逐渐的*和*分块的*，这些块有时少于一个词。这些块称为*标记*，[它们在NLP中非常有用](https://neptune.ai/blog/tokenization-in-nlp)，尽管现在对我们来说不那么重要。

在每个时间步骤，模型将之前的输出连接到当前输入，并继续生成。它一直这样做，直到达到特殊的*序列结束（EOS）*标记。省略提示任务并将单词作为标记，为简便起见，过程可以如下所示。

![](../Images/daabb0414aba3ccf9732c3402f6831b7.png)

为自回归模型生成文本的示意图。图像由作者提供

这个简单的机制加上**大量**的数据（*比任何人几辈子能读的还多*）使得模型能够生成连贯且上下文适宜的文本，模拟人类的写作。

> 注意，这里我们仅讨论生成模型。如果还有其他模型家族呢？
> 
> 原因很简单——文本生成任务是最**困难**的任务之一，同时也是最**令人印象深刻**的任务之一。[ChatGPT在仅仅5天内获得了100万用户](https://twitter.com/gdb/status/1599683104142430208)——比之前任何其他应用都要快，并且[以相同的势头继续发展](https://www.reuters.com/technology/chatgpt-sets-record-fastest-growing-user-base-analyst-note-2023-02-01/)。
> 
> 所谓的[**编码器**](https://huggingface.co/course/chapter1/5)（**BERT**模型家族）可能不那么令人兴奋，但它们也能以人类水平的表现解决各种问题，并帮助你完成诸如[文本分类](https://paperswithcode.com/task/text-classification)或[命名实体识别（NER）](https://paperswithcode.com/task/named-entity-recognition-ner)等任务。

我不会提供LLMs可以做的具体例子——互联网已经充满了这些例子。最好的方法是[亲自尝试ChatGPT](https://chat.openai.com/chat)，但你也可以找到很多令人兴奋的资源，比如[Awesome ChatGPT prompts repo](https://github.com/f/awesome-chatgpt-prompts)。尽管其能力令人印象深刻，但当前的大型语言模型仍有一些限制。最流行且显著的包括：

1.  **偏见和静态性**：由于LLMs是在来自各种来源的数据上训练的，它们不可避免地学习并复制了这些来源中的偏见。它们在某种意义上也具有静态性，即无法适应新数据或实时更新其知识，除非重新训练。

1.  **理解和虚假信息**：尽管LLMs可以生成类似人类的文本，但它们可能并不总是完全理解输入的上下文。此外，生成输出文本的自回归方式并不禁止模型生成虚假或无意义的信息。

1.  **资源密集型**：训练LLMs需要大量的计算资源，这转化为高成本和能源消耗。这个因素可能限制了LLMs对较小组织或个人研究人员的可及性。

这些及其他缺陷是研究社区的活跃话题。值得注意的是，该领域发展如此之快，以至于几个月内无法预测哪些限制会被克服——但毫无疑问，新的限制将会出现。

> 一个可能的例子是早期模型简单地增加了参数数量，但现在认为训练较小的模型更长时间，并提供更多的数据更为有效。这减少了模型的大小及其在推理过程中进一步使用的成本。
> 
> 通过这种方式，LLaMA的发布解放了爱好者，这些模型已经可以在[计算机](https://mobile.twitter.com/ggerganov/status/1640022482307502085)、[树莓派](https://twitter.com/miolini/status/1634982361757790209)甚至[手机](https://twitter.com/thiteanish/status/1635188333705043969)上本地运行！

在了解了LLM的整体概况后，让我们进入本文的主要部分——估算训练大型语言模型的成本。

# 估算机器学习模型的一般成本及LLM的特殊成本

要估算训练大型语言模型的成本，必须考虑任何机器学习算法包含的三个关键因素：

+   **数据**，

+   **计算资源**，以及

+   **架构**（或算法本身）。

让我们深入探讨这些方面，以更好地理解它们对训练成本的影响。

## 数据

LLM需要大量的数据来学习自然语言的模式和结构。估算数据的成本可能具有挑战性，因为公司通常会使用通过业务操作积累的数据以及开源数据集。

此外，数据需要被*清洗*、*标注*、*组织*和*高效存储*，考虑到LLM的规模。数据管理和处理成本可以迅速累积，特别是当考虑到这些任务所需的基础设施、工具和数据工程师时。

以一个具体例子为例，已知LLaMA使用了包含**1.4万亿个标记**的训练数据集，总大小为**4.6TB**！

![](../Images/76fbf5879560b4e3d2a8131bb616e1eb.png)

LLaMA模型的训练数据集，来源于[LLaMA论文](https://arxiv.org/pdf/2302.13971.pdf)

较小的模型（7B和13B）在1T标记上进行训练，而较大的模型（33B和65B）则使用了1.4T标记的完整数据集。

![](../Images/343af4446e0e34340c69378c96587c8b.png)

LLaMA模型的训练损失图，来源于[LLaMA论文](https://arxiv.org/pdf/2302.13971.pdf)

我认为现在你明白了，当人们称这些数据集为**庞大**时，并没有夸大其词，也明白了为什么在十年前技术上无法实现这些。但计算资源的情况更为有趣。

## 计算

实际的训练过程占据了LLM预算的一大部分。训练大型语言模型是资源密集型的，需要在强大的图形处理单元（GPU）上进行，因为GPU具有显著的并行处理能力。[NVIDIA每年发布新GPU](https://www.crn.com/news/components-peripherals/8-big-announcements-at-nvidia-s-gtc-2023-from-generative-ai-services-to-new-gpus)，其成本高达数十万美元。

训练这些模型的云计算服务成本可能非常高，达到**数百万美元**，尤其是考虑到需要通过各种配置进行迭代。

回到 LLaMA 论文，作者报告他们用**两千个每个 80 GB RAM 的 GPU 训练最大的 65B 模型 21 天**。

![](../Images/9fc18370b8aa038ee24375c5a4ac1612.png)

训练 LLaMA 模型所需的计算资源。图片来源于 [LLaMA 论文](https://arxiv.org/pdf/2302.13971.pdf)

[NVIDIA A100 GPU](https://www.nvidia.com/en-us/data-center/a100/) 是现代神经网络训练的热门选择。[Google Cloud Platform 提供这种 GPU](https://cloud.google.com/compute/gpus-pricing)，**每小时 $3.93**。

![](../Images/3ff8ab50ec05b9fa428a6f46aad9f892.png)

NVIDIA A100 GPU 的价格。截图来源于 [公共 GCP 定价页面](https://cloud.google.com/compute/gpus-pricing)

那么让我们做一些快速计算：

> 2048 GPUs x $3.93 GPU 每小时 x 24 小时 x 21 天 =
> 
> 405 万美元

四百万美元是并非每个研究人员都能承担的预算，对吧？而且这是单次运行！再举一个例子，[这篇文章估算了训练 GPT-3 的成本](https://lambdalabs.com/blog/demystifying-gpt-3)，作者得出了**355 GPU 年和 460 万美元**。

> 你可能听说过“神经网络在 GPU 上训练得非常快”，但没有人说明相对于什么。
> 
> 考虑到**巨大的**计算量，它们的训练速度确实很快，没有这些 GPU，它们可能要训练几十年。因此，21 天对 LLM 来说确实很快。

## 架构（和基础设施）

最先进的 LLM 的开发还依赖于技术娴熟的研究人员和工程师来开发架构并正确配置训练过程。架构是模型的基础，决定了它如何学习和生成文本。

设计、实施和控制这些架构需要计算机科学各个领域的专业知识。负责发布和交付前沿成果的工程师和研究人员的薪资可以达到**数十万美元**。值得注意的是，LLM 开发所需的技能集可能与“经典”机器学习工程师的技能集差异很大。

![](../Images/dd4a832be476af47f2a09f4ac0602d69.png)

机器学习系统基础设施。图 1 来源于 [Hidden Technical Debt in Machine Learning Systems paper](https://proceedings.neurips.cc/paper_files/paper/2015/file/86df7dcfd896fcaf2674f757a2463eba-Paper.pdf)

我认为现在你不会怀疑训练 LLM 是一个*非常困难*和*资源密集型*的工程问题了。

现在让我们简要讨论一些使 LLM 推理过程更高效、成本更低的方法。

# 优化语言模型以进行推理

## 我们真的需要优化吗？

推理是指使用训练好的语言模型生成预测或回应的过程，通常作为 API 或 web 服务。鉴于 LLM 的资源密集型特性，优化它们以实现高效推理至关重要。

> 例如，GPT-3模型有1750亿个参数，占**700 GB**的float32数字。大致相同量的内存也会被激活占用，请记住我们讨论的是RAM。
> 
> 为了进行预测而不使用任何优化技术，我们将需要16个每个有80 GB内存的A100 GPU！

一些流行的技术可以帮助减少内存需求和模型延迟，包括*模型并行*、*量化*等。

## 模型并行

[并行性](https://colossalai.org/docs/concepts/paradigms_of_parallelism/)是一种将单个模型的计算分布到多个GPU上的技术，可以在训练和推理过程中使用。

将模型的层或参数拆分到多个设备上可以显著提高整体推理速度，并且在实践中非常常见。

## 量化

[量化](https://huggingface.co/docs/optimum/concept_guides/quantization)涉及减少模型数值值（如权重）的精度。通过将浮点数转换为低精度整数，量化可以在不显著降低模型性能的情况下显著节省内存和加快计算速度。

产生的简单想法是使用*float16*数字代替*float32*，将内存减少一半。事实证明，模型权重甚至可以几乎无准确性损失地转换为*int8*，因为它们在数轴上相互接近。

## 其他技术

寻找优化LLM的方法是一个活跃的研究领域，其他技术包括：

+   [知识蒸馏](https://neptune.ai/blog/knowledge-distillation)——训练一个较小的*学生*模型来模仿较大*教师*模型的行为。

+   [参数剪枝](https://analyticsindiamag.com/a-beginners-guide-to-neural-network-pruning/)——从模型中移除冗余或不重要的参数，以减少模型的大小和计算需求；

+   并使用像[ORT (ONNX Runtime)](https://onnxruntime.ai/)这样的框架，通过*算子融合*和*常量折叠*等技术优化计算图。

总体而言，优化大型语言模型以进行推理是其部署的关键方面。通过应用各种优化技术，开发人员可以确保他们的LLM不仅强大和准确，而且具有成本效益和可扩展性。

# 为什么OpenAI开放ChatGPT访问？

考虑到上述所有因素，人们可能会想知道为什么OpenAI决定开放ChatGPT的访问权，考虑到训练和推理相关的高成本。虽然我们无法确定公司的确切动机，但我们可以分析这个决定背后的好处和潜在的战略原因。

首先，OpenAI通过让最先进的LLM更广泛地为公众所用，从而获得了*显著的知名度*（参见[AI革命更多是用户体验革命](https://medium.com/@kozyrkov/whats-different-about-today-s-ai-380569e3b0cd)）。通过展示大型语言模型的实际应用，该公司吸引了投资者、客户和科技界的广泛关注。此外，这使得OpenAI能够**收集大量反馈和数据**以改进他们的模型。

其次，OpenAI的使命围绕着AI的创建和进步。通过开放ChatGPT的访问，公司可以说在更接近实现其使命和为不可避免的变化做准备。提供强大AI工具的访问促进了创新，推动了AI研究领域的发展。这一进展可能会导致更高效的模型、更广泛的应用和各种挑战的创新解决方案。值得注意的是，**ChatGPT和GPT-4的架构是封闭的**，但这是另一个话题。

虽然训练和维护大型语言模型的成本无疑是巨大的，但对某些组织来说，开放这些工具的好处和战略优势可能会超过费用。以OpenAI为例，开放ChatGPT的访问不仅提高了他们的知名度，证明了他们在AI领域的领先地位，还使他们能够收集更多数据来训练更强大的模型。这一战略使他们能够推进使命，并在某种程度上对AI和LLM技术的发展做出贡献。

![](../Images/80511d030bfdd16042515b6d604833f2.png)

询问ChatGPT为何OpenAI提供免费访问ChatGPT。图像由作者使用[ChatGPT](https://chat.openai.com/chat)创建

# 结论

如我们所见，训练大型语言模型的成本受到各种因素的影响，包括*不仅*是昂贵的计算资源，还有大数据管理以及开发前沿架构所需的专业知识。

> 现代的LLM拥有**数十亿**个参数，训练数据量达到**万亿**个标记，且花费**数百万**美元。

我希望你现在能更好地理解训练和推理大型语言模型的规模，以及它们的局限性和陷阱。

自几年前以来，自然语言处理领域一直在经历其[ImageNet时刻](https://thegradient.pub/nlp-imagenet/)，现在轮到**生成模型**了。生成语言模型的广泛应用和采纳有可能*彻底改变各个行业和我们生活的各个方面*。虽然很难预测这些变化会如何展开，但我们可以确定LLM将对世界产生一些影响。

就个人而言，我喜欢最近训练“更聪明”的模型，而不仅仅是“更大”的模型的趋势。通过探索更优雅的方式来开发和部署大型语言模型，我们可以推动人工智能和自然语言处理的边界，为该领域开辟创新解决方案和更加光明的未来。

# 资源

这里是我关于大型语言模型的其他文章，它们可能对你有用。我已经涵盖了：

+   [提示工程最佳实践](/summarising-best-practices-for-prompt-engineering-c5e86c483af4)：如何应用提示工程技术与大型语言模型有效互动，以及如何使用 OpenAI API 和 Streamlit 构建本地大型语言模型应用程序；

+   [使用 ChatGPT 进行调试](/using-chatgpt-for-efficient-debugging-fc9e065b7856#da94-27cac6b3f550)：如何使用大型语言模型进行调试和代码生成。

如果你对大型语言模型感兴趣并想了解更多，这里有一些可以帮助你的资源：

+   [插图化 Transformer](https://jalammar.github.io/illustrated-transformer/) 是对 Transformer 架构的极好介绍，该架构由 Jay Alammar 引发了自然语言处理领域的重大变革；

+   [GPT-3的工作原理——可视化和动画](https://jalammar.github.io/how-gpt3-works-visualizations-animations/) 是同一作者对自回归解码过程的可视化展示；

+   [用 60 行 NumPy 实现 GPT](https://jaykmody.com/blog/gpt-from-scratch/) 是 Jay Mody 撰写的一篇精彩文章，作者在其中构建了自己的简单 GPT；

+   [让我们构建 GPT：从头开始，用代码详细说明](https://www.youtube.com/watch?v=kCc8FmEb1nY) 是著名的 Andrej Karpathy 制作的精彩视频，他曾在 [特斯拉自动驾驶](https://www.tesla.com/autopilot)担任 AI 高级总监；

+   要深入了解该领域，请查看 [Awesome-LLM GitHub 仓库](https://github.com/Hannibal046/Awesome-LLM) 以获取更详细的资源列表。可以查看 [Chinchilla](https://arxiv.org/abs/2203.15556) 和 [LLaMA](https://arxiv.org/abs/2302.13971) 作为近年来最具影响力的论文之一。

# 感谢阅读！

+   希望这些资料对你有帮助。[在 Medium 上关注我](https://medium.com/@andimid)以获取更多类似的文章。

+   如果你有任何问题或评论，我很乐意收到任何反馈。可以在评论中问我，或通过 [LinkedIn](https://www.linkedin.com/in/andimid/) 或 [Twitter](https://twitter.com/dimid_ml) 与我联系。

+   支持我作为作者并访问成千上万的其他 Medium 文章，请通过 [我的推荐链接](https://medium.com/@andimid/membership) 获取 Medium 会员（对你没有额外费用）。
