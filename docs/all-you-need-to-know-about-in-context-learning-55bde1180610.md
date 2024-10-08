# 了解上下文学习的所有信息

> 原文：[`towardsdatascience.com/all-you-need-to-know-about-in-context-learning-55bde1180610`](https://towardsdatascience.com/all-you-need-to-know-about-in-context-learning-55bde1180610)

## | 上下文学习 | 大型语言模型 | LLMs

## 什么是大型语言模型（Large Language Models），它们是如何运作的，使其如此强大的原因是什么？

[](https://salvatore-raieli.medium.com/?source=post_page-----55bde1180610--------------------------------)![Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----55bde1180610--------------------------------)[](https://towardsdatascience.com/?source=post_page-----55bde1180610--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----55bde1180610--------------------------------) [Salvatore Raieli](https://salvatore-raieli.medium.com/?source=post_page-----55bde1180610--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----55bde1180610--------------------------------) ·阅读时间 19 分钟·2023 年 7 月 25 日

--

![](img/7f1b46371aebe6ca8e4684f7f9be78fa.png)

图片来源：[🇸🇮 Janko Ferlič](https://unsplash.com/ko/@itfeelslikefilm?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> “对我来说，上下文是关键——从中产生了对一切的理解。” — **肯尼斯·诺兰**

上下文学习（ICL）是最令人惊讶的模型技能之一。观察到 GPT-3 时引起了作者的注意。**ICL 到底是什么？更重要的是，是什么促使了它的产生？**

本文分为不同的部分，每部分将回答这些问题：

+   什么是上下文学习（ICL）？为什么这很有趣？它有什么用处？

+   ICL 的奥秘：它是如何工作的？是训练数据？是提示？还是架构？

+   ICL 的未来是什么？还存在哪些挑战？

查看文章末尾的参考文献列表，我也提供了一些建议，以便深入探讨这些主题。

# 什么是上下文学习（ICL）？

![](img/4b4d54bbb67cef241a1909f3d3526037.png)

图片来源：[Dmitry Ratushny](https://unsplash.com/@ratushny?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> “我的语言的限制意味着我的世界的限制。” — **路德维希·维特根斯坦**

在[大型语言模型](https://en.wikipedia.org/wiki/Large_language_mode)（LLMs）发布之前，人工智能模型仅限于其训练数据。换句话说，[LLMs](https://en.wikipedia.org/wiki/Large_language_model)只能解决为其训练设计的任务。

另一方面，[GPT-3](https://arxiv.org/abs/2005.14165)和今天的[LLMs](https://en.wikipedia.org/wiki/Large_language_mode)展示了一种新能力：通过在输入（提示）中提供新的示例来学习新技能和解决新任务。此外，在这种情况下，我们并没有训练模型；没有[梯度更新](https://en.wikipedia.org/wiki/Gradient_descent)或模型参数的变化。这项技能被称为[上下文学习（ICL）](https://en.wikipedia.org/wiki/Prompt_engineering)。

![](img/b4e3700d502d9da8a95015beac0410c8.png)

图片来源：[这里](https://arxiv.org/abs/2005.14165)

更具体来说，与模型交互的方式是提供自然语言指令作为提示。虽然这可能看起来有限，但可以在提示中输入一定数量的[令牌](https://learn.microsoft.com/en-us/semantic-kernel/prompt-engineering/tokens)（[上下文窗口](https://www.linkedin.com/pulse/whats-context-window-anyway-caitie-doogan-phd/)）。此外，尽管处于这个文本模板中，它也可以允许模型[解决数学题](https://arxiv.org/abs/2212.10535)。事实上，在提示中，我们可以插入单词纠正、算术练习、翻译、编程等示例。

![](img/668ff5c0b2b29f0af73c35b6890623ca.png)

图片来源：[这里](https://arxiv.org/abs/2301.00234)

现在，我们可以给出[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)的正式定义：

> 上下文学习是一种范式，它允许语言模型仅通过几个示例进行任务学习（[来源](https://arxiv.org/abs/2301.00234)）。

简单来说，通过给模型一组展示任务的输入输出对，模型阅读训练示例以找出输入和输出的分布，成功映射输入和输出，并生成适当的响应。

[如本研究所示](https://arxiv.org/abs/2211.09066)，这一简单的想法有助于模型更轻松地执行某些任务。通过用明确的指令解释如何执行任务，可以使模型更好地理解并更轻松地执行这些任务。使用这些少量示例相较于使用更多标记数据训练模型具有竞争力（[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)）。

这促使了各种策略的出现，以利用[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)（提示工程），因为改变提示能够比[为特定任务进行微调](https://arxiv.org/abs/2211.09066)获得更好的性能。

![](img/8c175a1a8e3657a5f80c1019545eaedb.png)

图片来源：[这里](https://arxiv.org/abs/2211.09066)

[](/speak-to-me-how-many-words-a-model-is-reading-331e3af86d27?source=post_page-----55bde1180610--------------------------------) [## 告诉我：模型正在阅读多少单词]

### 为什么以及如何克服大型语言模型的内在限制

在如何阅读模型的单词数量

[这种行为似乎也仅在规模扩大时出现](https://arxiv.org/abs/2206.07682)，即，[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)似乎仅在[参数数量达到一定水平](https://en.wikipedia.org/wiki/Neural_scaling_law)时出现。实际上，对于某些能力，模型的表现似乎在达到某个参数数量之前是随机的，然后突然有所改善。

![](img/fd41f594bcec315652fa7c8f69a46357.png)

图片来源：[这里](https://arxiv.org/abs/2206.07682)

## AI 中的新兴能力：我们在追逐一个神话吗？

### 从新兴属性的角度看大型语言模型

在 AI 中的新兴能力：我们在追逐一个神话吗？

简而言之，行为之所以被研究和学习，是因为[它有明显的优势](https://arxiv.org/abs/2301.00234)：

+   示例以自然语言编写，因此与模板的通信是可解释且易于理解的。整合人类知识要容易得多，因为你只需更改提示即可。

+   另外，[上下文学习](https://en.wikipedia.org/wiki/Prompt_engineering)记忆了人类的学习方式，因为它回忆了通过[类比](https://en.wikipedia.org/wiki/Analogy)的学习过程。

+   它是无训练的，我们不需要训练模型（与[监督学习](https://en.wikipedia.org/wiki/Supervised_learning)不同）。这意味着它在[计算成本](https://en.wikipedia.org/wiki/Computational_complexity)上要便宜得多。这非常高效，因为技能可以立即获得。

+   这也意味着模型可以作为服务使用，因此可以部署用于许多任务。实际上，这些任务可以由日常用户教授。

+   [ICL](https://en.wikipedia.org/wiki/Prompt_engineering)使模型能够[泛化](https://developers.google.com/machine-learning/crash-course/generalization/video-lecture)，允许模型学习示例中的潜在模式和规则，然后将其应用于新情况。此外，它还为模型提供了多样性，因为它可以应用于许多不同类型的技能。

> 看起来很惊人，但它是如何工作的？

# ICL 的谜团：它是如何工作的？

![](img/c9a184a494833d37feeb23b3928d720c.png)

图片由[𝓴𝓘𝓡𝓚 𝕝𝔸𝕀](https://unsplash.com/@kirklai?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> “通常，如果你想对这些模型进行微调，你需要收集特定领域的数据并进行一些复杂的工程工作。但现在我们只需输入五个示例，它就能完成我们想要的结果。所以，*上下文学习*是一种异常高效的学习现象，需要加以理解，”Akyürek 说。 ([source](https://news.mit.edu/2023/large-language-models-in-context-learning-0207))

尽管*[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)*似乎几乎是神奇的，但它也有其局限性。例如，[GPT-3](https://en.wikipedia.org/wiki/GPT-3)展示了看似惊人的推理能力。然而，一些需要推理的数据集，例如 Winograd 数据集，并没有显示*[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)*的效果：

> Winograd 方案是一对仅在一两个词上有所不同的句子，这些句子中包含了一个模糊性，该模糊性在两个句子中以相反的方式得到解决，并且需要使用世界知识和推理来解决。 ([source](https://cs.nyu.edu/~davise/papers/WinogradSchemas/WS.html))

事实上，使用几个示例进行提示并没有带来改进。

![](img/77e88cac98ec8e0f298a32e423cc5387.png)

图片来源：[这里](https://arxiv.org/abs/2005.14165)

这些事实和其他看似矛盾的行为使研究人员提出了问题：**ICL 的起源是什么？为什么它比微调更有效？通过改变提示，ICL 能否得到改善？**

同时，必须记住大多数技能是在预训练期间学到的。训练一个*[LLM](https://en.wikipedia.org/wiki/Large_language_model)*的第一步需要大量的文本，通常通过简单地要求模型预测给定序列前部分的单词来进行。这一步是最昂贵、最耗时和最资源密集的。在*[alignment](https://jasonwei20.github.io/files/FLAN%20talk%20external.pdf)*（从 GPT 3.5 到*[ChatGPT](https://openai.com/blog/chatgpt)）期间，模型只是提高了利用这些知识的能力和如何与人类互动。

[](/the-infinite-babel-library-of-llms-90e203b2f6b0?source=post_page-----55bde1180610--------------------------------) ## 大语言模型的无限巴别图书馆

### 开源、数据和注意力：大语言模型未来的变化

[towardsdatascience.com

## 利用你所看到的：预训练对 ICL 的影响

在预训练期间，[LLMs](https://en.wikipedia.org/wiki/Large_language_model) 因此接触到了大量的文本：来自维基百科、书籍（小说和非小说）、科学文章、推文、Reddit 帖子、博客帖子、互联网数据等。

[在 2022 年一篇文章中](https://arxiv.org/abs/2212.10559)，提出了[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)可以被视为一种隐式的[微调](https://en.wikipedia.org/wiki/Fine-tuning_(deep_learning))。主要区别在于，虽然[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)仅通过前向计算产生，而微调还包含一个反向传播步骤（在该步骤中参数被更新）。**这确认了** [**ICL**](https://en.wikipedia.org/wiki/Prompt_engineering) **必须在预训练期间产生，但它如何影响预训练？**

[正如一篇文章所示](https://aclanthology.org/2022.naacl-main.380/)，预训练数据集对模型开发[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)至关重要。根据作者的说法，源领域比[语料库](https://en.wikipedia.org/wiki/Text_corpus)的大小更为重要。此外，将多个[语料库](https://en.wikipedia.org/wiki/Text_corpus)结合起来可以促使[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)的出现（如果两个语料库单独无法产生 ICL，将它们结合起来可以产生[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)）。另一个重要因素是[语料库](https://en.wikipedia.org/wiki/Text_corpus)的领域相关性：仅在新闻[语料库](https://en.wikipedia.org/wiki/Text_corpus)上进行训练只能在与新闻相关的下游任务中实现相对的上下文学习能力。最后，作者指出，尽管[困惑度](https://en.wikipedia.org/wiki/Perplexity)（跟踪[LLMs](https://en.wikipedia.org/wiki/Large_language_model)的最常用指标之一）和[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)通常存在相关性，但困惑度本身并不能反映模型在[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)方面的能力（比较两个[LLMs](https://en.wikipedia.org/wiki/Large_language_model)时，困惑度最低的模型不一定是[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)最高的模型）。

[这在进一步确认](https://arxiv.org/abs/2205.05055)了数据集必须在稀有类别中存在多个实例以允许 ICL。根据作者的说法，训练数据示例应以簇的形式出现（即，每个类别应有多个示例），并且应具有一定的类别多样性。

![](img/fe8746fa845491858ad4fcabf22a1b63.png)

图片来源：[这里](https://arxiv.org/abs/2205.05055)

[另一项研究指出](https://arxiv.org/abs/2303.07895)在上下文学习出现时，预训练分布（训练数据）是一种隐式混合。预训练示例是从任务混合中提取的，而示例和任务之间的关联是潜在的。因此，一旦模型训练完成，它能够自行揭示示例中的潜在任务。例如，一系列包含正面和负面内容的推文代表了情感分析的潜在任务。

简而言之，这些文章声称，[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)会出现如果数据集多样化，它们呈现了多样的类别数量（但同时每个类别有多个示例），涵盖多个领域，并且最好这些示例代表一个[自然语言处理](https://en.wikipedia.org/wiki/Natural_language_processing)的潜在任务。由于[LLMs](https://en.wikipedia.org/wiki/Large_language_model)通常使用大量文本进行训练，因此满足这些前提。

[## 说一次！重复词汇无助于 AI](https://say-once-repeating-words-is-not-helping-ai-58f38035f66e?source=post_page-----55bde1180610--------------------------------)

### 重复标记如何以及为何对 LLM 造成伤害？这是个问题吗？

[towardsdatascience.com](https://say-once-repeating-words-is-not-helping-ai-58f38035f66e?source=post_page-----55bde1180610--------------------------------)

## 你如何使用你学到的知识：你能回忆起你学到的内容吗？

一些研究人员尝试[开发框架](https://arxiv.org/abs/2111.02080)以理解[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)在预训练过程中如何出现。根据作者的说法，[LLM](https://en.wikipedia.org/wiki/Large_language_model)使用[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)来“定位”执行任务所需的概念。这个想法是，在训练过程中模型获得了潜在概念，然后在[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)过程中再次找到它们。为了再次找到这些概念，[LLM](https://en.wikipedia.org/wiki/Large_language_model)可以使用提示的全部或部分组件：格式、输入、输出和输入-输出映射。

正如[作者在博客文章中解释的](https://ai.stanford.edu/blog/understanding-incontext/)，模型在训练过程中学习了多个概念，然后模型使用训练示例来理解提示中的任务需要进行[情感分析](https://en.wikipedia.org/wiki/Sentiment_analysis)或[主题分类](https://developers.google.com/machine-learning/guides/text-classification?hl=it)，此时将映射应用于测试输入：

> 在这篇论文中，我们研究了当预训练文档具有长程连贯性时，如何出现上下文学习。在这种情况下，语言模型必须推断出潜在的文档级概念，以在预训练期间生成连贯的下一个标记。在测试时，上下文学习发生在语言模型还推断出提示中示例之间的共享潜在概念时。（[来源](https://arxiv.org/abs/2111.02080)）

![](img/39f467017195bea30d100863084d5721.png)

图片来源：[这里](https://arxiv.org/abs/2111.02080)

**是的，但什么是概念？** 对于作者来说，概念是“*一个包含各种文档级统计信息的潜在变量*”。因此，某个话题的概念（例如新闻）是单词的分布（使用了哪些单词）、格式（如何书写）、文章与话题之间的关系等。

提供给模型的文本不是随机的单词，而是文本具有其自身的[内部连贯性](https://en.wikipedia.org/wiki/Coherence_(linguistics))。换句话说，类似的文本具有相似的语义信息（相同的话题）和格式（不同的编程文档解释和代码片段）。通过学习根据这些前例预测一个单词，[LLM](https://en.wikipedia.org/wiki/Large_language_model)也建模内部一致性，并使其能够推断出提示中的潜在概念。

作者的话：

> **1\. 预训练**：为了在预训练期间预测下一个标记，语言模型必须利用前面的句子中的证据来推断（“定位”）文档的潜在概念。
> 
> **2\. 上下文学习**：如果语言模型还通过提示中的上下文示例推断出*提示概念*（提示中示例共享的潜在概念），那么就会发生上下文学习！（[来源](https://ai.stanford.edu/blog/understanding-incontext/)）

对于作者来说，这种“定位”过程可以视为[贝叶斯推断](https://en.wikipedia.org/wiki/Bayesian_inference)，其中[LLMs](https://en.wikipedia.org/wiki/Large_language_model)推断提示中的概念（所有示例在输入提示中共享的概念）。一旦他推断出概念，他就可以产生正确的答案。

公式中：

![](img/8718900b9ea981fa05ea2b6b7117f0bf.png)

图片来源：[这里](https://arxiv.org/abs/2111.02080)

## 请礼貌提问：提示的效果

在近期的工作中，[Min 等人](https://arxiv.org/abs/2202.12837)定义了 ICL 提示的特征以及提示的各种组件如何影响模型在 ICL 中的表现。

![](img/3dda25ecd69fc81d7b1be593488a2917.png)

图片来源：[这里](https://arxiv.org/abs/2202.12837)

认为演示作为输入输出对（(x1, y1)…(xk, yk)）有四个正式方面：

+   **输入-标签映射**。一个输入 x 与其标签 y 正确配对。

+   **输入文本的分布**，即从中提取输入 x 的分布。

+   **标签空间**是 y 输出的空间。

+   **格式**，特别是输入-输出对的配对

对于[作者们](https://arxiv.org/abs/2202.12837)而言，格式、输入和标签空间的分布非常重要。相比之下，输入-标签映射对[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)几乎没有影响。[根据斯坦福人工智能实验室](https://ai.stanford.edu/blog/understanding-incontext/)的说法，这源于模型在预训练期间已经接触过输入-输出匹配，因此在演示时不需要输入-标签映射。相反，其他元素是必需的，以便能够定位其已学习的概念（简而言之，执行[贝叶斯推断](https://en.wikipedia.org/wiki/Bayesian_inference)）。

![](img/6b897bdf4114db6d6f1cb57c2d7ca69d.png)

图片来源：[这里](https://arxiv.org/abs/2202.12837)

[另一篇论文指出](https://arxiv.org/abs/2205.12685)，实际上输入-标签映射是重要的，[根据另一篇论文](https://arxiv.org/abs/2303.03846)，虽然确实重要，但如果模型足够大，它可以自行学习映射。

对于其他作者而言，演示的多样性、简洁性以及结构上的相似性都是重要的。对于另一篇论文，[演示的顺序很重要](https://aclanthology.org/2022.acl-long.556/)。然而，[刘等人](https://aclanthology.org/2022.deelio-1.10.pdf)显示，示例的选择对[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)有很大影响。因此，应选择接近[嵌入空间](https://developers.google.com/machine-learning/crash-course/embeddings/video-lecture)的示例。事实上，一种有效的技术是提供一个问题以进行嵌入，并寻找在[嵌入](https://en.wikipedia.org/wiki/Embedding)中距离较近的示例。

![](img/1192be99f7221b72a5ae571369b3d2a8.png)

图片来源：[这里](https://aclanthology.org/2022.deelio-1.10.pdf)

[](https://levelup.gitconnected.com/the-ai-college-student-goes-back-to-the-bench-daa6d9bdfb14?source=post_page-----55bde1180610--------------------------------) [## 人工智能大学生回到基础]

### 大型语言模型如何解决大学考试及其重要性

[levelup.gitconnected.com](https://levelup.gitconnected.com/the-ai-college-student-goes-back-to-the-bench-daa6d9bdfb14?source=post_page-----55bde1180610--------------------------------)

## 对注意力机制的深入探讨

我们到目前为止已经看到训练数据集和提示的作用，现在是时候更仔细地研究架构的效果了。一个[LLM](https://en.wikipedia.org/wiki/Large_language_model)是一个变换器，而[变换器](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))主要基于[多头自注意力](https://en.wikipedia.org/wiki/Attention_(machine_learning))。由于[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)是[LLMs](https://en.wikipedia.org/wiki/Large_language_model)最有趣的行为之一，许多作者专注于尝试找到[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)发生的机制性答案：

> 如果我们可以理解导致变换器模型产生其输出的内部结构，那么我们可能能够更系统地解决当前的安全问题，并预测未来更强大的模型中的安全问题。 ([source](https://arxiv.org/abs/2209.11895))

![](img/470625884f8f80cbbb190a076b5eeb81.png)

图片来源：[这里](https://arxiv.org/abs/1706.03762)

[Anthropic 的研究人员识别了他们称为**归纳头**的电路](https://arxiv.org/abs/2209.11895)。归纳头是一个由[两个注意力头](https://en.wikipedia.org/wiki/Attention_(machine_learning))在不同层中协作以复制或完成模式的电路。基本上，第一个注意力头将信息从前一个标记复制到下一个标记。第二个注意力头则拥有关于当前标记之前发生的事情的信息。然后，这个机制可以搜索当前标记 A 所在的序列，并查看下一个标记 B，因此一旦看到 A，模式更有可能生成输出 B。

![](img/1c3b8ddbce7d102d33057ae158f67070.png)

图片来源：[这里](https://arxiv.org/abs/2209.11895)

[对于作者们](https://arxiv.org/abs/2209.11895)来说，这并不是一种简单的复制机制。实际上，在[归纳推理](https://en.wikipedia.org/wiki/Inductive_reasoning)中，我们可以推断出 A 后面跟着 B，如果在上下文中我们看到 A 最有可能跟着 B。对作者们而言，这些归纳头使这种推理机制得以具体化，这种机制不是基于训练数据，而是基于上下文：[A][B]…[A]→[B]

[对于 Anthropic](https://arxiv.org/abs/2209.11895)，这些归纳头在[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)中扮演了重要角色。事实上，它们能够学习和重复任意序列，可以被视为一种简化的少样本学习形式。在[大型模型](https://en.wikipedia.org/wiki/Large_language_model)中，这种效果会被放大，因为它们可以处理抽象表示。因此：“*进行序列复制的相同头部也承担了类比序列复制或上下文最近邻的更广泛角色*”。

现在，这一机制也很有趣，因为它还促进了另一种序列完成：[A*][B*] … [A] → [B]。在这种情况下，A*和 B*不是相同的令牌 A 和 B，而是在[嵌入空间](https://en.wikipedia.org/wiki/Embedding)中相似的令牌（例如，不同语言中的相同单词）。

这些引导头似乎在[LLM](https://en.wikipedia.org/wiki/Large_language_model)提高其 ICL 技能时出现。同时，在小型[LMs](https://en.wikipedia.org/wiki/Large_language_model)中，可以观察到这种关系与[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)（对它们来说，引导头是[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)的驱动因素）。此外，这些引导头的逆向工程可以完成，这似乎是理解它们如何形成以及如何影响[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)的一个有前景的研究方向。

![](img/89d1bccf2e12244a91bab946d0f5db4d.png)

图片来源：[这里](https://arxiv.org/abs/2209.11895)

每个案例中的[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)都是通过注意力机制关联并产生的。不过，这有一个二次（计算术语中）的成本。一些具有简化形式的[注意力](https://en.wikipedia.org/wiki/Attention_(machine_learning))（线性或对数）模型被测试过；然而，这导致了表现力的下降，并影响了模型的[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)能力。因此，虽然寻求[多头自注意力](https://en.wikipedia.org/wiki/Attention_(machine_learning))的替代方案，但作者们确保他们提出的模型能够进行[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)。

[](https://levelup.gitconnected.com/welcome-back-80s-transformers-could-be-blown-away-by-convolution-21ff15f6d1cc?source=post_page-----55bde1180610--------------------------------) [## 欢迎回到 80 年代：卷积可能会超越变换器

### Hyena 模型展示了卷积如何比自注意力更快

levelup.gitconnected.com](https://levelup.gitconnected.com/welcome-back-80s-transformers-could-be-blown-away-by-convolution-21ff15f6d1cc?source=post_page-----55bde1180610--------------------------------)

## 学会学习上下文

显然，由于[变换器](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))是通过[梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)训练的，因此后者与[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)之间存在关系。以[线性回归](https://en.wikipedia.org/wiki/Linear_regression)为起点，[Akyürek 建议](https://arxiv.org/abs/2211.15661)变换器隐式地将[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)视为一个优化问题。

[Oswald](https://arxiv.org/abs/2212.07677)展示了[变换器层](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model))理论上可以在上下文数据上实现[梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)。根据[Oswald](https://arxiv.org/abs/2212.07677)的说法，上下文学习在某些情况下模仿[梯度下降](https://en.wikipedia.org/wiki/Gradient_descent)。[这篇论文](https://arxiv.org/abs/2212.07677)展示了[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)和梯度下降之间的关系。

![](img/128ac013ae9e45af341a022694e50030.png)

图片来源：[这里](https://arxiv.org/abs/2212.07677)

上述结果，以及[Akyürek 的结果](https://arxiv.org/abs/2211.15661)，意味着进行上下文学习的模型不仅仅是匹配先前的模式，而是还学会了如何执行其他任务（这是对诱导头所说内容的扩展）。实际上，[Akyürek](https://arxiv.org/abs/2211.15661)提供了包含合成数据的提示，以防模型已经见过这些数据。

[Akyürek 的假设](https://arxiv.org/abs/2211.15661)是模型在内部执行某种机器学习算法（这在某种程度上呼应但扩展了模型进行贝叶斯推断的想法）。在文章中，他们表示模型在其隐藏状态中实现了一个线性模型，并且这一点在训练过程中得到了学习。

> “在这种情况下，我们尝试恢复线性模型的实际解决方案，我们能够证明参数被写在隐藏状态中。这意味着线性模型某处确实存在，”Akyürek 说。（[来源](https://news.mit.edu/2023/large-language-models-in-context-learning-0207)）

[在一个有趣的实验中](https://ai.googleblog.com/2023/05/larger-language-models-do-in-context.html)，Google 测试了通过[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)模型是否能够覆盖先前的[先验知识](https://en.wikipedia.org/wiki/Prior_knowledge_for_pattern_recognition)，对作者来说，这也是广泛的[LLMs](https://en.wikipedia.org/wiki/Large_language_model)的一个新兴特性例子。

在其中一个实验中，作者进行了常规的[ICL](https://en.wikipedia.org/wiki/Prompt_engineering)、翻转的 ICL（标签被翻转）和语义不相关的标签 ICL（SUL-ICL），其中标签是没有语义相关性的词汇。

![](img/2807e4985bd1bae2dd2215dc62c8d9df.png)

图片来源：[这里](https://arxiv.org/abs/2303.03846)

这篇文章展示了一些有趣的现象。当标签被翻转（但真实评价保持不变）时，如果模型能够覆盖其先验知识，则应出现性能下降。结果是小模型的性能保持平稳，而[大模型](https://en.wikipedia.org/wiki/Large_language_model)则出现了下降。

> 这些结果表明，当矛盾的输入-标签映射在语境中出现时，大型模型可以覆盖预训练中的先验知识。小模型无法做到这一点，使这一能力成为模型规模的突现现象。([source](https://ai.googleblog.com/2023/05/larger-language-models-do-in-context.html))

![](img/15b8229c8301c802ce1b0c2a46c4b578.png)

图片来源：[这里](https://arxiv.org/abs/2303.03846)

其次，当在语义无关的标签（例如“foo/bar”而不是“负面/正面”用于情感分析）的演示中提供输入-标签映射时，模型也可以学习。从[先验知识](https://en.wikipedia.org/wiki/Prior_knowledge_for_pattern_recognition)中仅依赖的模型应该会有性能下降，因为它无法利用标签的语义意义进行预测。事实上，小模型的预测能力下降，而[LLM](https://en.wikipedia.org/wiki/Large_language_model)则没有。对作者而言，这意味着虽然小模型依赖于[先验知识](https://en.wikipedia.org/wiki/Prior_knowledge_for_pattern_recognition)，但“*大型模型在语境中有能力学习输入-标签映射，当标签的语义性质被移除时。*”

![](img/d166a16fd271cee5b30b5c7c55da4a41.png)

图片来源：[这里](https://arxiv.org/abs/2303.03846)

[作者们还研究了](https://arxiv.org/abs/2303.03846)指令调整对 ICL 的影响。在指令调整过程中，模型会接收到包含问题和答案的指令。因此，这个过程涉及自然语言标签，作者们想知道这是否会影响[LLM](https://en.wikipedia.org/wiki/Large_language_model)学习输入-标签映射或利用语义先验知识的能力。实验表明：“指令调整提高了学习输入-标签映射的能力，它增强了对语义先验知识的使用。”

因此，这些结果表明，不仅是架构、数据量和提示影响 ICL，[对于 Google 来说，参数的数量本身](https://ai.googleblog.com/2023/05/larger-language-models-do-in-context.html)也很重要：

> 这些结果强调了语言模型的语境学习行为如何根据语言模型的规模而变化，以及更大的语言模型具有将输入映射到多种类型标签的突现能力，这是一种真正的符号推理形式，其中输入-标签映射可以为任意符号学习。 ([source](https://arxiv.org/pdf/2303.03846.pdf))

[](https://salvatore-raieli.medium.com/scaling-isnt-everything-how-bigger-models-fail-harder-d64589be4f04?source=post_page-----55bde1180610--------------------------------) [## 扩展并非一切：更大模型如何更容易失败

### 大型语言模型是否真的理解编程语言？

[salvatore-raieli.medium.com](https://salvatore-raieli.medium.com/scaling-isnt-everything-how-bigger-models-fail-harder-d64589be4f04?source=post_page-----55bde1180610--------------------------------)

# 结论、挑战和前景

![](img/986b2920355032f6fc74247aeef7d0a9.png)

[Nadine Shaabana](https://unsplash.com/@nadineshaabana?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上传的照片

> “将文本与上下文分开，剩下的只是骗局。” ― **斯图尔特·斯塔福德**
> 
> “单词本身从来没有好坏之分，是上下文让它们如此。” ― **阿比吉特·纳斯卡尔**

在-context 学习是 [LLMs](https://en.wikipedia.org/wiki/Large_language_model) 最有趣且难以捉摸的行为之一。GPT-3 发布时首次受到赞赏，它让社区对其潜在应用充满兴奋。

ICL 简单来说就是从类比中学习的能力。只需在演示中提供几个例子，模型就能做出预测。这使模型具备前所未有的多功能性和无限应用的发展可能。

尽管如此，我们仍然不完全理解它在训练过程中如何产生。我们已经看到了训练数据、提示或注意力本身的重要性。今天，随着希望用新架构取代基于注意力的模型的想法，我们需要了解如何保留 ICL。

ICL 研究非常活跃，一些研究方向包括：

+   **新预训练策略**，如前所述，如果一种训练策略传统上提高了性能（困惑度下降），并不意味着它提高了模型的 ICL 技能。因此，正在寻找专注于提高模型 ICL 技能的策略。

+   **ICL 能力蒸馏**，ICL 似乎随着模型的规模而出现，但如果能够将这些技能蒸馏到较小的模型中，我们将节省计算成本、内存和基础设施。因此，蒸馏对于具有 ICL 的较小模型似乎很有前景。[初步研究显示前景光明](https://arxiv.org/abs/2212.08410)。

+   **ICL 鲁棒性**。正如我们所见，ICL 技能并不稳定，演示格式的排列和变化会影响 ICL。[有一项研究表明](https://arxiv.org/abs/2209.07661)，提高鲁棒性会以降低准确度为代价，因此我们需要深入研究 ICL 的工作机制。更好的理论理解可以帮助开发更鲁棒的 ICL。

+   **ICL 的效率和可扩展性。** ICL 需要在演示中使用不同的示例。理论上，更多的示例可以改善 ICL。增加示例的数量有计算成本，这来自于计算注意力（效率）。另一个挑战是，你不能添加超过上下文窗口允许的示例（可扩展性）。正如我们在之前的文章中看到的，研究在如何扩展上下文窗口（以及使用了哪些策略）方面非常活跃，尽管尚不清楚模型是否能够利用它。此外，在某些情况下，出现了逆向扩展，其中模型不是遵循上下文中的指令，而是重复了记忆的数据。

另一个研究方向是开发可以通过调整提示格式来提高 ICL（In-context Learning，情境学习）的技术。随着时间的推移，已经提出了几种有趣的方法（包括思维链（COT）、自一致性 COT、思维树等）。这些方法在改善数学练习和其他推理问题的模型性能方面已显示出成功。所有这些都是通过简单地修改提示实现的。在本文中，我关注了模型的更机械化的方面、训练数据、提示以及 ICL 是如何出现的。在下一篇文章中，我将详细讨论这些方法。

## 大家怎么看？在评论中告诉我吧。

# 如果你觉得这很有趣：

*你可以查看我的其他文章，也可以* [***订阅***](https://salvatore-raieli.medium.com/subscribe) *以便在我发布文章时收到通知，你也可以* [***成为 Medium 会员***](https://medium.com/@salvatore-raieli/membership) *来访问所有故事（这是平台的会员链接，我从中获得少量收入，但对你没有费用），你还可以在*[***LinkedIn***](https://www.linkedin.com/in/salvatore-raieli/)***上与我联系。***

*这里是我的 GitHub 仓库的链接，我计划在这里收集与机器学习、人工智能等相关的代码和许多资源。*

[## GitHub - SalvatoreRa/tutorial: 机器学习、人工智能、数据科学的教程…](https://github.com/SalvatoreRa/tutorial?source=post_page-----55bde1180610--------------------------------)

### 关于机器学习、人工智能、数据科学的教程，包括数学解释和可重复使用的代码（用 Python 编写…）。

[github.com](https://github.com/SalvatoreRa/tutorial?source=post_page-----55bde1180610--------------------------------)

# 参考文献

这是我撰写本文时参考的主要文献列表，仅引用了文章的第一个名字。如果你想深入了解这个话题，我也推荐这些文献。

1.  Brown, 2020, 语言模型是少样本学习者，[链接](https://arxiv.org/abs/2005.14165)

1.  Dong, 2022, 关于情境学习的调查，[链接](https://arxiv.org/abs/2301.00234)

1.  赵，《大语言模型调查》，[链接](https://arxiv.org/abs/2303.18223)

1.  谢，2022 年，《上下文学习是如何工作的？理解与传统监督学习的差异的框架》，[链接](https://ai.stanford.edu/blog/understanding-incontext/)

1.  魏，2022 年，《大语言模型的突现能力》，[链接](https://arxiv.org/abs/2206.07682)

1.  周，2022 年，《通过上下文学习教授算法推理》，[链接](https://arxiv.org/abs/2211.09066)

1.  [Vinita Silaparasetty](https://medium.com/u/8f22c49c614?source=post_page-----55bde1180610--------------------------------)，《什么是提示工程？》，[链接](https://medium.com/@vinitasilaparasetty/what-is-prompt-engineering-8221e0aa619d)

1.  [Fareed Khan](https://medium.com/u/b856005e5ecd?source=post_page-----55bde1180610--------------------------------)，《提示工程完全指南》，[链接](https://medium.com/@fareedkhandev/prompt-engineering-complete-guide-2968776f0431)

1.  [Paul DelSignore](https://medium.com/u/6202cb40e768?source=post_page-----55bde1180610--------------------------------)，《提示工程的黑暗面》，[链接](https://medium.com/the-generator/the-dark-side-of-prompt-engineering-33b8087ffd59)

1.  [Babar M Bhatti](https://medium.com/u/10dee34829b?source=post_page-----55bde1180610--------------------------------)，《为 LLMs 制作有效提示的艺术与科学》，[链接](https://thebabar.medium.com/the-art-and-science-of-crafting-effective-prompts-for-llms-e04447e8f96a)

1.  戴，2022 年，《为什么 GPT 能在上下文中学习？语言模型隐式地执行梯度下降作为元优化器》，[链接](https://arxiv.org/abs/2212.10559)

1.  辛，2022 年，《大规模语言模型预训练语料对上下文学习的影响》，[链接](https://aclanthology.org/2022.naacl-main.380/)

1.  [Cameron R. Wolfe, Ph.D.](https://medium.com/u/28aa6026c553?source=post_page-----55bde1180610--------------------------------)，《语言模型的扩展法则与 GPT-3》，链接

1.  谢，2021 年，《将上下文学习解释为隐式贝叶斯推断》，[链接](https://arxiv.org/abs/2111.02080)

1.  Huszár，2022 年，《大语言模型中的隐式贝叶斯推断》，[链接](https://www.inference.vc/implicit-bayesian-inference-in-sequence-models/)

1.  闵，2022 年，《重新思考示范的作用：什么使得上下文学习有效？》，[链接](https://arxiv.org/abs/2202.12837)

1.  陈，2022 年，《数据分布属性驱动变压器中的突现上下文学习》，[链接](https://arxiv.org/abs/2205.05055)

1.  刘，2023 年，《什么样的上下文示例对 GPT-3 有效？》，[链接](https://aclanthology.org/2022.deelio-1.10/)

1.  [Priyanka](https://medium.com/u/29db232e8826?source=post_page-----55bde1180610--------------------------------)，《语言模型的困惑度》，[链接](https://medium.com/@priyankads/perplexity-of-language-models-41160427ed72)

1.  奥尔松，2022 年，《上下文学习与归纳头》，[链接]

1.  Wies, 2023，《上下文学习的可学习性》，[链接](https://arxiv.org/abs/2303.07895)

1.  Akyürek, 2022，《什么是上下文学习的学习算法？线性模型的研究》，[链接](https://arxiv.org/abs/2211.15661)

1.  Oswald, 2022，《变压器通过梯度下降在上下文中学习》，[链接](https://arxiv.org/abs/2212.07677)

1.  Wei, 2023，《更大的语言模型在上下文学习中的表现不同》，[链接](https://arxiv.org/abs/2303.03846)

1.  Google 博客，《更大的语言模型在上下文学习中的表现不同》，[链接](https://ai.googleblog.com/2023/05/larger-language-models-do-in-context.html)

1.  Zewe，《解决机器学习的谜团》，[链接](https://news.mit.edu/2023/large-language-models-in-context-learning-0207)

1.  Kaddour, 2023，《大型语言模型的挑战与应用》，[链接](https://arxiv.org/abs/2307.10169)

1.  Magister, 2022，《教小型语言模型推理》，[链接](https://arxiv.org/abs/2212.08410)

1.  Chen, 2022，《上下文学习中敏感性与准确性之间的关系》，[链接](https://arxiv.org/abs/2209.07661)
