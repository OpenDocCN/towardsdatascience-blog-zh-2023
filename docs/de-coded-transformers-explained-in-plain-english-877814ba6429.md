# 解码：用简单英语解释 Transformers

> 原文：[https://towardsdatascience.com/de-coded-transformers-explained-in-plain-english-877814ba6429?source=collection_archive---------0-----------------------#2023-10-09](https://towardsdatascience.com/de-coded-transformers-explained-in-plain-english-877814ba6429?source=collection_archive---------0-----------------------#2023-10-09)

## 无需代码、数学或提及 Keys、Queries 和 Values

[](https://medium.com/@chris.p.hughes10?source=post_page-----877814ba6429--------------------------------)[![Chris Hughes](../Images/87b16cd8677739b12294380fb00fde85.png)](https://medium.com/@chris.p.hughes10?source=post_page-----877814ba6429--------------------------------)[](https://towardsdatascience.com/?source=post_page-----877814ba6429--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----877814ba6429--------------------------------) [Chris Hughes](https://medium.com/@chris.p.hughes10?source=post_page-----877814ba6429--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff13df9df155e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fde-coded-transformers-explained-in-plain-english-877814ba6429&user=Chris+Hughes&userId=f13df9df155e&source=post_page-f13df9df155e----877814ba6429---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----877814ba6429--------------------------------) ·15分钟阅读·2023年10月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F877814ba6429&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fde-coded-transformers-explained-in-plain-english-877814ba6429&user=Chris+Hughes&userId=f13df9df155e&source=-----877814ba6429---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F877814ba6429&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fde-coded-transformers-explained-in-plain-english-877814ba6429&source=-----877814ba6429---------------------bookmark_footer-----------)

自从[2017年引入](https://arxiv.org/abs/1706.03762)以来，transformers 已成为机器学习领域的显著力量，彻底改变了[主要翻译](https://blog.research.google/2020/06/recent-advances-in-google-translate.html)和[自动补全](https://github.blog/2023-05-17-how-github-copilot-is-getting-better-at-understanding-your-code/)服务的能力。

最近，随着大型语言模型如OpenAI的[ChatGPT](https://openai.com/blog/chatgpt)、[GPT-4](https://cdn.openai.com/papers/gpt-4.pdf#:~:text=This%20technical%20report%20presents%20GPT-4%2C%20a%20large%20multimodal,as%20dialogue%20systems%2C%20text%20summarization%2C%20and%20machine%20translation.)和Meta的[LLama](https://ai.meta.com/blog/large-language-model-llama-meta-ai/)的出现，变换器的受欢迎程度更高了。这些模型获得了极大的关注和兴奋，都建立在变换器架构的基础上。通过利用变换器的力量，这些模型在自然语言理解和生成方面取得了显著突破，向公众展示了这些成就。

尽管有很多[优质资源](http://jalammar.github.io/illustrated-transformer/)详细讲解了变换器的工作原理，但我发现自己在理解其数学机制的同时，很难直观地解释变换器是如何工作的。在进行许多访谈、与同事讨论以及在相关主题上做简短讲座之后，似乎很多人都面临这个问题！

在这篇博客文章中，我将旨在提供一个高级解释，讲解变换器是如何工作的，而不依赖于代码或数学。我的目标是避免令人困惑的技术术语和与以前架构的比较。虽然我会尽量保持简单，但由于变换器相当复杂，这不会很容易，但我希望它能提供更好的直观理解，了解它们的作用及其工作方式。

# 什么是变换器？

变换器是一种神经网络架构，特别适用于处理序列输入的任务。在这个上下文中，最常见的序列例子可能是句子，我们可以把它看作是一个有序的单词集合。

这些模型的目标是为序列中的每个元素创建一个数值表示；封装关于元素及其邻近上下文的关键信息。生成的数值表示可以传递给下游网络，这些网络可以利用这些信息来执行各种任务，包括生成和分类。

通过创建如此丰富的表示，这些模型使下游网络能够更好地理解输入序列中的潜在模式和关系，从而增强它们生成连贯且具有上下文相关性的输出的能力。

变换器的主要优势在于其处理序列中的长距离依赖的能力，以及高度的效率；能够并行处理序列。这对于机器翻译、情感分析和文本生成等任务尤其有用。

![](../Images/4d9874a6d0be28e2def6d15c3e1efe41.png)

图片由 [Azure OpenAI Service DALL-E model](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models#dall-e-preview) 生成，提示为：“*绿色和黑色的矩阵代码，形状像 Optimus Prime*”

# **Transformer 中包含什么？**

要将输入传递给 transformer，我们必须首先将其转换为一系列标记；一组整数表示我们的输入。

由于 transformers 最初应用于 NLP 领域，我们首先考虑这一场景。将句子转换为一系列标记的最简单方法是定义一个 *词汇表*，它充当查找表，将单词映射到整数；我们可以保留特定的数字来表示词汇表中未包含的任何单词，以便始终分配一个整数值。

![](../Images/271353078c1b211d9723ec89e696c761.png)

实际上，这是一种简单的文本编码方式，因为像 *cat* 和 *cats* 这样的单词被视为完全不同的标记，尽管它们是相同动物的单数和复数描述！为了克服这一点，已经制定了不同的标记化策略——如 [byte-pair encoding](https://huggingface.co/learn/nlp-course/chapter6/5?fw=pt)——它们在索引单词之前会将其拆分成更小的块。此外，通常还需要添加特殊标记，以表示句子的开始和结束，从而为模型提供额外的上下文。

让我们考虑以下示例，以更好地理解标记化过程。

*“你好，今天在 Drosval 的天气不错吗？”*

Drosval 是 GPT-4 使用以下提示生成的名称：*“你能创建一个虚构的地名，它听起来像是属于* [*David Gemmell 的 Drenai 宇宙吗？*](https://davidgemmell.fandom.com/wiki/Drenai_series)*”*；这个名字被故意选择，因为它不应该出现在任何训练模型的词汇中。

使用来自 [transformers library](https://huggingface.co/docs/transformers/index) 的 `[bert-base-uncased](https://huggingface.co/bert-base-uncased)` 标记化器，这被转换为以下标记序列：

![](../Images/3f08e4204076bf092f9c2c3042ed9645.png)

代表每个单词的整数将根据特定的模型训练和标记化策略而变化。解码后，我们可以看到每个标记代表的单词：

![](../Images/572e2fc47ce9691a370aefd1e24dfcf1.png)

有趣的是，我们可以看到这与我们的输入并不相同。添加了特殊标记，我们的缩写被拆分为多个标记，我们的虚构地名由不同的“块”表示。由于我们使用了‘uncased’模型，我们也丢失了所有的大写上下文。

然而，尽管我们用句子作为例子，变压器模型并不限于文本输入；这种架构在[视觉任务上也展示了良好的结果](https://arxiv.org/abs/2010.11929v2)。为了将图像转换为序列，ViT的作者将图像切割成不重叠的16x16像素块，并将这些块串联成一个长向量，然后输入到模型中。如果我们在推荐系统中使用变压器，一种方法可能是将用户浏览过的最后*n*个项目的ID作为输入传递到我们的网络中。如果我们能够为我们的领域创建有意义的输入标记表示，我们可以将其输入到变压器网络中。

## 嵌入我们的标记

一旦我们得到一个表示输入的整数序列，我们可以将它们转换为[词嵌入](https://huggingface.co/blog/getting-started-with-embeddings)。词嵌入是一种表示信息的方式，能够被机器学习算法轻松处理；其目的是通过将信息表示为数字序列，以压缩的格式捕捉编码的标记的意义。最初，词嵌入被初始化为随机数字序列，有意义的表示在训练过程中被学习。然而，这些词嵌入有一个固有的限制：它们不考虑标记出现的上下文。这有两个方面。

根据任务的不同，当我们嵌入标记时，我们可能还希望保留标记的顺序；这在NLP等领域尤为重要，否则我们基本上会得到一个[*词袋模型*方法](https://machinelearningmastery.com/gentle-introduction-bag-words-model/)。为了解决这个问题，我们对词嵌入应用*位置编码*。虽然有[多种创建位置嵌入的方法](https://arxiv.org/abs/2104.09864)，但主要思想是我们有*另一*组嵌入，表示输入序列中每个标记的位置，这些位置嵌入与我们的标记嵌入结合。

![](../Images/dc7ababa63186505d97d97bb6099fbdd.png)

另一个问题是标记的含义可以根据周围的标记而变化。考虑以下句子：

*天黑了，谁关掉了灯？*

*哇，这个包裹真的很轻！*

在这里，*light* 这个词在两个不同的上下文中使用，其含义完全不同！然而，根据分词策略的不同，词嵌入可能是相同的。在变压器模型中，这由其*注意力*机制处理。

# 从概念上讲，什么是注意力机制？

也许变换器架构中最重要的机制被称为*注意力*，它使网络能够理解输入序列中哪些部分对于给定任务最为相关。对于序列中的每个标记，注意力机制确定哪些其他标记在给定上下文中对理解当前标记是重要的。在我们探讨变换器中如何实现这一机制之前，让我们从简单开始，试图从概念上理解注意力机制的目标，以建立我们的直觉。

理解注意力的一种方式是将其视为一种方法，用来将每个标记嵌入替换为包含其邻近标记信息的嵌入；而不是在不考虑上下文的情况下对每个标记使用相同的嵌入。如果我们知道哪些标记与当前标记相关，那么捕捉这种上下文的一种方式是创建一个加权平均——或者更一般地说，是一个线性组合——这些嵌入。

![](../Images/566be9fdc10f7026db59b5958d5d66a1.png)

让我们考虑一个简单的例子，看看这对于我们之前看到的一个句子会是什么样子。在应用注意力之前，序列中的嵌入没有邻居的上下文。因此，我们可以将*light*一词的嵌入可视化为以下线性组合。

![](../Images/37c2bd186bb6520960ceafb770f8a5fb.png)

在这里，我们可以看到我们的权重只是单位矩阵。应用我们的注意力机制后，我们希望学习一个权重矩阵，以便我们能够以类似于以下的方式表达我们的*light*嵌入。

![](../Images/ab0c5b6d7a3fb8fdfcd507c33fbd0fb2.png)

这次，较大的权重被赋予对应于我们选择的标记最相关部分的嵌入；这应该确保最重要的上下文被捕捉到新的嵌入向量中。

含有当前上下文信息的嵌入有时被称为*上下文化嵌入*，这就是我们最终试图创建的东西。

现在我们对注意力机制的目标有了一个高层次的理解，让我们探讨一下它在下一部分是如何实际实现的。

# **注意力是如何计算的？**

注意力有多种类型，主要区别在于用于执行线性组合的权重计算方式。这里，我们将考虑[缩放点积注意力](https://paperswithcode.com/method/scaled)，如在[原始论文](https://arxiv.org/abs/1706.03762)中介绍的，这是最常见的方法。在这一部分中，假设我们所有的嵌入都已经进行位置编码。

记住，我们的目标是通过对原始嵌入的线性组合来创建上下文化嵌入，让我们从简单开始，假设我们可以将所有必要的信息编码到学习到的嵌入向量中，我们只需要计算权重。

为了计算权重，我们必须首先确定哪些 tokens 彼此相关。为此，我们需要建立两个嵌入之间相似性的概念。表示这种相似性的一种方法是使用点积，我们希望学习嵌入，使得较高的分数表示两个词更相似。

![](../Images/fc0f44b7a748f5b568ba290ebbd34af0.png)

由于我们需要计算每个 token 与序列中其他每个 token 的相关性，我们可以将其概括为矩阵乘法，这为我们提供了权重矩阵，这些矩阵通常被称为*注意力分数*。为了确保我们的权重总和为 1，我们还应用了 [SoftMax 函数](https://en.wikipedia.org/wiki/Softmax_function)。然而，由于矩阵乘法可能产生任意大的数字，这可能导致 SoftMax 函数对较大的注意力分数返回非常小的梯度，这可能在训练过程中导致 [梯度消失问题](https://en.wikipedia.org/wiki/Vanishing_gradient_problem)。为了应对这一问题，在应用 SoftMax 之前，注意力分数会乘以一个缩放因子。

![](../Images/f368c6edaece31c4813beea23a5cf557.png)

现在，为了得到我们的上下文化嵌入矩阵，我们可以将注意力分数与原始嵌入矩阵相乘；这相当于对我们的嵌入进行线性组合。

![](../Images/df5e39b1e5c61b24645c7de35f229e91.png)

简化的注意力计算：假设嵌入是位置编码的

虽然模型可能能够学习到足够复杂的嵌入来生成注意力分数和后续的上下文化嵌入，但我们试图将大量信息压缩到通常非常小的嵌入维度中。

因此，为了让模型学习任务稍微简单一些，我们引入一些可学习的参数！我们不直接使用嵌入矩阵，而是通过三个独立的线性层（矩阵乘法）来处理它；这应该使模型能够“关注”嵌入的不同部分。如下图所示：

![](../Images/691be809db9692bf8adc0deb68ef4c1f.png)

缩放的点积自注意力：假设嵌入是位置编码的

从图像中，我们可以看到线性投影被标记为Q、K和V。在原始论文中，这些投影被称为 *查询、键和值*，显然受到信息检索的启发。就个人而言，我发现这个类比并没有帮助我理解，因此我通常不会关注这一点；我在这里遵循文献中的术语，以保持一致，并明确这些线性层是不同的。

现在我们了解了这个过程如何运作，我们可以把注意力计算视作一个有三个输入的单一块，这些输入将被传递到Q、K和V。

![](../Images/07ffd6affcf03eca718d7d59446b70e2.png)

当我们将相同的嵌入矩阵传递给Q、K和V时，这被称为 *自注意力*。

# **什么是多头注意力？**

在实践中，我们通常并行使用多个自注意力块，以使变换器能够同时关注输入序列的不同部分——这被称为 *多头注意力*。

多头注意力的想法相当简单，多个独立的自注意力块的输出被连接在一起，然后通过一个线性层。这个线性层使模型能够学习如何结合来自每个注意力头的上下文信息。

在实践中，每个自注意力块中使用的隐藏维度大小通常选择为原始嵌入大小除以注意力头的数量；以保持嵌入矩阵的形状。

![](../Images/04f88198fb56767adc1f4dd86c41a10b.png)

# **变换器还包含什么？**

尽管引入变换器的论文（现在臭名昭著）被命名为[*Attention is all you need*](https://arxiv.org/abs/1706.03762)，但这有些令人困惑，因为变换器的组件不仅仅是注意力！

一个变换器块还包含以下内容：

+   **前馈神经网络**（FFN）：一个两层神经网络，独立应用于批次和序列中的每个令牌嵌入。FFN块的目的是将额外的可学习参数引入变换器，这些参数是[确保上下文嵌入是独特且分散的](https://arxiv.org/pdf/2305.13297.pdf)。原始论文使用了一个[GeLU激活函数](https://paperswithcode.com/method/gelu)，但FFN的组件可以根据架构的不同而有所变化。

+   [**层归一化**](https://arxiv.org/abs/1607.06450)：有助于稳定深度神经网络的训练，包括变换器。它对每个序列的激活进行归一化，防止它们在训练过程中变得过大或过小，这可能导致梯度相关问题，如[梯度消失或梯度爆炸](https://en.wikipedia.org/wiki/Vanishing_gradient_problem)。这种稳定性对于有效训练非常深的变换器模型至关重要。

+   **跳跃连接**：如在[ResNet架构](https://arxiv.org/abs/1512.03385)中，残差连接用于缓解梯度消失问题并提高训练稳定性。

尽管自引入以来，转换器架构保持了相当稳定，但层归一化块的位置可能会根据转换器架构而有所不同。原始架构，现在称为*后层归一化*，如下所示：

![](../Images/00b8b1ab46c026969fcf9a2a9acffa19.png)

在最近的架构中，如下图所示，最常见的放置位置是*预层归一化*，它将归一化块放在自注意力和FFN块之前，位于跳跃连接中。

![](../Images/92c7363ca0353c06a66962158f54fc5f.png)

# **不同类型的Transformer有哪些？**

虽然现在有许多不同的转换器架构，但大多数可以归纳为三种主要类型。

## 编码器架构

编码器模型旨在生成可以用于下游任务（如分类或命名实体识别）的上下文嵌入，因为注意力机制能够覆盖整个输入序列；这就是本文迄今为止探索的架构类型。最受欢迎的编码器-only转换器家族是[BERT](https://arxiv.org/abs/1810.04805)及其变体。

在通过一个或多个转换器块处理数据后，我们得到了一个复杂的上下文化嵌入矩阵，表示序列中每个标记的嵌入。然而，为了用于下游任务，如分类，我们只需要做一个预测。传统上，取第一个标记，并通过分类头；分类头通常包含Dropout和线性层。这些层的输出可以通过SoftMax函数转换为类别概率。下面展示了这可能的样子。

![](../Images/32ef39197e778affc8d0afc72b0ee8ab.png)

## 解码器架构

几乎与编码器架构相同，关键区别在于解码器架构使用了*掩蔽（或因果）* *自注意力层*，使得注意力机制只能关注输入序列的当前和先前元素；这意味着生成的上下文嵌入仅考虑先前的上下文。流行的解码器-only模型包括[GPT家族](https://arxiv.org/abs/2005.14165)。

![](../Images/99336020869964d867175ac62d505d56.png)

这通常是通过用二进制下三角矩阵掩蔽注意力分数，并用负无穷替换未掩蔽的元素来实现的；当通过以下SoftMax操作时，这将确保这些位置的注意力分数为零。我们可以更新之前的自注意力图以包括如下内容。

![](../Images/03f498f51e541ef9e56d9d13593b1b3a.png)

掩码自注意力计算：假设位置编码嵌入

由于它们只能从当前位置及之前的位置进行注意力计算，解码器架构通常用于自回归任务，如序列生成。然而，在使用上下文嵌入生成序列时，与使用编码器相比，有一些额外的考虑因素。下方展示了一个例子。

![](../Images/9a0bda9202f68abe52c409227bb1c833.png)

我们可以注意到，尽管解码器为输入序列中的每个 token 生成了一个上下文嵌入，但我们通常使用与最终 token 对应的嵌入作为生成序列时输入到后续层的内容。

此外，在对 logits 应用 SoftMax 函数后，如果没有进行过滤，我们将会得到模型词汇表中每个 token 的概率分布；这可能非常庞大！通常，我们希望使用各种过滤策略减少潜在选项的数量，一些常见的方法包括：

+   **温度调整：** 温度是一个在 SoftMax 操作中应用的参数，影响生成文本的随机性。它通过改变输出单词的概率分布来决定模型输出的创造性或专注性。较高的温度会使分布变平，生成的输出更具多样性。

+   **Top-P 采样：** 这种方法根据给定的概率阈值筛选下一个 token 的潜在候选数量，并基于超过该阈值的候选重新分配概率分布。

+   **Top-K 采样：** 这种方法将潜在候选数量限制为基于其 logit 或概率评分（取决于实现）的 *K* 个最可能的 token。

这些方法的更多细节 [可以在这里找到](https://peterchng.com/blog/2023/05/02/token-selection-strategies-top-k-top-p-and-temperature/)。

一旦我们调整或减少了对下一个 token 潜在候选的概率分布，我们可以从中进行采样以获得预测结果——这只是从一个 [多项分布](https://en.wikipedia.org/wiki/Multinomial_distribution) 进行采样。预测的 token 然后附加到输入序列中，并反馈到模型中，直到生成所需数量的 tokens，或模型生成一个 *停止* token；一个特殊的 token，用于标记序列的结束。

## 编码器-解码器架构

最初，变换器被提出作为一种机器翻译架构，使用了编码器和解码器来实现这一目标；先使用编码器创建中间表示，再使用解码器翻译成所需的输出格式。虽然编码器-解码器变换器已经变得不那么常见，[诸如 T5](https://arxiv.org/abs/1910.10683) 的架构展示了如何将任务如问答、摘要和分类框架化为序列到序列的问题，并利用这种方法进行解决。

编码器-解码器架构的关键区别在于，解码器使用*编码器-解码器注意力*，在其注意力计算过程中使用编码器的输出（作为 K 和 V）和解码器块的输入（作为 Q）。这与自注意力形成对比，在自注意力中，相同的输入嵌入矩阵用于所有输入。除此之外，总体生成过程与仅使用解码器架构非常相似。

我们可以通过下图来可视化编码器-解码器架构。在这里，为了简化图示，我选择了描绘原始论文中所见的*后层归一化*变种，其中层归一化层位于注意力块之后。

![](../Images/ea8723275525bea7281566f1b01e2f0a.png)

# 结论

希望这提供了对变换器工作原理的直观了解，帮助将一些细节以某种易于消化的方式分解，并且作为解开现代变换器架构神秘面纱的良好起点！

[*Chris Hughes*](https://www.linkedin.com/in/chris-hughes1/) *在 LinkedIn 上*

除非另有说明，所有图像均由作者创建。

# 参考文献

+   [[1706.03762] 注意力即全部所需 (arxiv.org)](https://arxiv.org/abs/1706.03762)

+   [Google 翻译的最新进展 — Google Research 博客](https://blog.research.google/2020/06/recent-advances-in-google-translate.html)

+   [GitHub Copilot 如何更好地理解你的代码 — GitHub 博客](https://github.blog/2023-05-17-how-github-copilot-is-getting-better-at-understanding-your-code/)

+   [介绍 ChatGPT (openai.com)](https://openai.com/blog/chatgpt)

+   [gpt-4.pdf (openai.com)](https://cdn.openai.com/papers/gpt-4.pdf#:~:text=This%20technical%20report%20presents%20GPT-4%2C%20a%20large%20multimodal,as%20dialogue%20systems%2C%20text%20summarization%2C%20and%20machine%20translation.)

+   [介绍 LLaMA：一个基础的、65 亿参数的语言模型 (meta.com)](https://ai.meta.com/blog/large-language-model-llama-meta-ai/)

+   [图解变换器 — Jay Alammar — 一次一个概念地可视化机器学习。 (jalammar.github.io)](http://jalammar.github.io/illustrated-transformer/)

+   [字节对编码标记化 — Hugging Face NLP 课程](https://huggingface.co/learn/nlp-course/chapter6/5?fw=pt)

+   [德内系列 | 大卫·杰梅尔维基 | Fandom](https://davidgemmell.fandom.com/wiki/Drenai_series)

+   [bert-base-uncased · Hugging Face](https://huggingface.co/bert-base-uncased)

+   [🤗 Transformers (huggingface.co)](https://huggingface.co/docs/transformers/index)

+   [[2010.11929v2] 一张图片值 16x16 个词：大规模图像识别的变换器 (arxiv.org)](https://arxiv.org/abs/2010.11929v2)

+   [开始使用嵌入 (huggingface.co)](https://huggingface.co/blog/getting-started-with-embeddings)

+   [温和介绍词袋模型 — MachineLearningMastery.com](https://machinelearningmastery.com/gentle-introduction-bag-words-model/)

+   [[2104.09864] RoFormer: 带有旋转位置嵌入的增强型变换器 (arxiv.org)](https://arxiv.org/abs/2104.09864)

+   [缩放点积注意力解释 | Papers With Code](https://paperswithcode.com/method/scaled)

+   [Softmax函数 — Wikipedia](https://en.wikipedia.org/wiki/Softmax_function)

+   [[1607.06450] 层归一化 (arxiv.org)](https://arxiv.org/abs/1607.06450)

+   [消失梯度问题 — Wikipedia](https://en.wikipedia.org/wiki/Vanishing_gradient_problem)

+   [[1512.03385] 深度残差学习用于图像识别 (arxiv.org)](https://arxiv.org/abs/1512.03385)

+   [[1810.04805] BERT: 深度双向变换器的预训练用于语言理解 (arxiv.org)](https://arxiv.org/abs/1810.04805)

+   [[2005.14165] 语言模型是少量样本学习者 (arxiv.org)](https://arxiv.org/abs/2005.14165)

+   [令牌选择策略：Top-K、Top-P 和温度 (peterchng.com)](https://peterchng.com/blog/2023/05/02/token-selection-strategies-top-k-top-p-and-temperature/)

+   [多项分布 — Wikipedia](https://en.wikipedia.org/wiki/Multinomial_distribution)

+   [[1910.10683] 探索统一的文本到文本变换器的迁移学习极限 (arxiv.org)](https://arxiv.org/abs/1910.10683)
