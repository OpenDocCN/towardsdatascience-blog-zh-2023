# 语言模型简史

> 原文：[https://towardsdatascience.com/a-brief-history-of-language-models-d9e4620e025b?source=collection_archive---------11-----------------------#2023-05-12](https://towardsdatascience.com/a-brief-history-of-language-models-d9e4620e025b?source=collection_archive---------11-----------------------#2023-05-12)

## 通向GPT的突破——为非专家解释

[](https://medium.com/@doriandrost?source=post_page-----d9e4620e025b--------------------------------)[![Dorian Drost](../Images/1795395ad0586eafd83d3e2f7b975ca8.png)](https://medium.com/@doriandrost?source=post_page-----d9e4620e025b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d9e4620e025b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d9e4620e025b--------------------------------) [Dorian Drost](https://medium.com/@doriandrost?source=post_page-----d9e4620e025b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1d49ea537d1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-history-of-language-models-d9e4620e025b&user=Dorian+Drost&userId=1d49ea537d1c&source=post_page-1d49ea537d1c----d9e4620e025b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9e4620e025b--------------------------------) ·9分钟阅读·2023年5月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd9e4620e025b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-history-of-language-models-d9e4620e025b&user=Dorian+Drost&userId=1d49ea537d1c&source=-----d9e4620e025b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd9e4620e025b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-brief-history-of-language-models-d9e4620e025b&source=-----d9e4620e025b---------------------bookmark_footer-----------)

今天大型语言模型如GPT在媒体上获得的压倒性关注，给人一种我们正身处持续革命中的印象。然而，即使是革命也是建立在其前身的成功基础上的，而GPT是数十年研究的结果。

在这篇文章中，我想概述一下在语言模型研究领域中一些重要的发展步骤，这些步骤最终导致了我们今天所拥有的大型语言模型。我将简要描述语言模型的一般概念，然后讨论在不同时间主导该领域的一些核心技术，这些技术通过克服其前身的障碍和困难，为今天的技术铺平了道路，其中（Chat-）GPT可能是最著名的代表。

## 什么是语言模型？

![](../Images/02c4b9a3bb243d65d9c3ef834667b71a.png)

将单词转化为语言模型所需的条件是什么？照片由[Glen Carrie](https://unsplash.com/fr/@glencarrie?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

语言模型是一种机器学习模型，给定一系列单词，预测下一个单词。就是这么简单！

主要思想是，这样的模型必须具有人类语言的某种表示。在某种程度上，它模拟了我们语言所依赖的规则。经过看过数百万行文本后，模型将表示这样一个事实：像动词、名词和代词这样的事物在语言中存在，并且它们在句子中有不同的功能。它可能还会得到一些来自单词含义的模式，比如“巧克力”经常出现在“甜的”、“糖”和“脂肪”等单词的语境中，但几乎不会与“割草机”或“线性回归”等单词一起出现。

正如提到的，它通过学习给定一系列单词预测下一个单词的方式得到这种表示。这是通过分析大量文本来推断在给定上下文中下一个可能的单词。让我们看看如何实现这一点。

## 初学者

![](../Images/12fe94508ad0570b055f733f591f30e4.png)

在我们可以考虑更复杂的技术之前，我们必须从简单的开始。照片由[Jon Cartagena](https://unsplash.com/@cartega?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

让我们从一个直觉上的第一个想法开始：给定大量的文本，我们可以计算在给定上下文中每个单词的频率。上下文仅仅是前面出现的单词。也就是说，例如，我们统计词语“like”在词语“I”之后出现的频率，统计它在词语“don’t”之后出现的频率，依此类推，对于所有在词语“like”之前出现过的单词。如果我们将这个频率除以前面的词语的频率，我们很容易得到概率P(“like” | “I”)，读作*给定词语“I”，单词“like”的概率：*

P(“like” | “I”) = count(“I like”) / count(“I”)

P(“like” | “don’t”) = count(“don’t like”) / count(“don’t”)

我们可以为我们在文本中找到的每对单词做到这一点。然而，存在一个明显的缺点：用于确定概率的上下文仅仅是一个单词。这意味着，如果我们想预测在单词“don’t”之后出现的内容，我们的模型不知道“don’t”之前是什么，因此无法区分“他们不”，“我不”或“我们不”。

要解决这个问题，我们可以扩展上下文。所以，不再计算 P(“like” | “don’t”)，而是计算 P(“like” | “I don’t”)、P(“like” | “they don’t”)、P(“like” | “we don’t”) 等等。我们甚至可以将上下文扩展到更多单词，这就是我们称之为 n-gram 模型，其中 n 表示要考虑的上下文单词数量。一个 n-gram 只是 n 个单词的序列，因此，“I like chocolate” 是一个 3-gram。

n 越大，模型在预测下一个单词时能考虑的上下文就越多。然而，n 越大，我们需要计算的不同概率就越多，因为例如 5-gram 比 2-gram 要多得多。不同的 n-gram 数量呈指数增长，并且很容易达到一种无法处理它们的内存或计算时间的程度。因此，n-gram 只允许我们非常有限的上下文，这对于许多任务来说是不够的。

## 循环神经网络

![](../Images/921cfd15d94a347c9a0366834c9368ca.png)

循环神经网络一遍又一遍地执行相同的步骤。照片由 [Önder Örtel](https://unsplash.com/@onderortel?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

循环神经网络（RNNs）引入了一种方法来解决 n-gram 模型在处理更大上下文时遇到的问题。在 RNN 中，输入序列逐个单词地被处理，生成所谓的 *hidden representation*。其主要思想是，这个隐藏表示包含到目前为止序列的所有相关信息，并且可以在下一步中用于预测下一个单词。

让我们举个例子：假设我们有这样一个句子

> The mouse eats the cheese

现在，RNN 逐个处理单词（首先是“mouse”，然后是“eats”，...），创建隐藏表示，并预测最可能出现的下一个单词。例如，当我们到达单词“the”时，模型的输入将包括当前单词（“the”）和一个隐藏表示向量，该向量包含了句子“the mouse eats”的相关信息。这些信息用于预测下一个单词（例如“cheese”）。请注意，模型看不到单词“the”、“mouse”和“eats”；它们被编码在隐藏表示中。

这是否比 n-gram 模型看到最后的 n 个词更好？嗯，这要看情况。隐藏表示的主要优点在于，它可以包含关于不同大小序列的信息，而不会呈指数增长。在一个 3-gram 模型中，模型准确地看到 3 个词。如果这不足以准确预测下一个词，它无能为力，因为没有更多的信息。另一方面，RNN 使用的隐藏表示包括整个序列。然而，它必须以某种方式将所有信息适配到这个固定大小的向量中，因此信息不会以逐字方式存储。如果序列变得更长，这可能会成为一个瓶颈，所有相关信息都必须经过这个固定大小的向量。

你可以这样理解它们之间的区别：n-gram 模型只看到有限的上下文，但它清晰地看到这个上下文（词本身），而 RNN 具有更大且更灵活的上下文，但它只看到一个模糊的图像（隐藏表示）。

不幸的是，RNN 还有另一个缺点：由于它们一个词一个词地处理序列，因此无法并行训练。为了处理位置 t 的词，你需要步骤 t-1 的隐藏表示，而步骤 t-1 的隐藏表示又需要步骤 t-2 的隐藏表示，以此类推。因此，无论是在训练还是推理过程中，计算都必须一个步骤一个步骤地进行。如果可以并行计算每个词所需的信息，不是更好吗？

## 注意力机制的救援：变换器

![](../Images/3e1c6ba208a156426fa9d180a1d6f62c.png)

注意力全在于击中正确的位置。照片来自[Afif Ramdhasuma](https://unsplash.com/@javaistan?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

变换器是一类解决 RNN 缺点的模型。它们避免了隐藏表示的瓶颈问题，并且允许并行训练。它们是如何做到的呢？

变换器模型的关键组件是注意力机制。请记住，在 RNN 中，存在一个包含到目前为止输入序列所有信息的隐藏表示。为了避免单一表示带来的瓶颈，注意力机制在每一步构建一个新的隐藏表示，该表示可以包含来自任何先前词的信息。这使得模型能够决定序列中的哪些部分对于预测下一个词是相关的，因此可以通过为这些部分分配更高的相关性来专注于它们，从而计算下一个词的概率。假设我们有一个句子

*当我前几天看到多萝西和稻草人时，我走到她面前说：“嗨*

我们希望预测下一个词。注意力机制使模型能够专注于与续写相关的词，并忽略那些不相关的部分。在这个例子中，代词“her”必须指代“Dorothy”（而不是“the scarecrow”），因此模型必须决定专注于“Dorothy”并忽略“the scarecrow”来预测下一个词。对于这个句子，更可能的续写是“Hi, Dorothy”而不是“Hi, scarecrow”或“Hi, together”。

RNN只会有一个隐藏表示向量，这个向量可能包含也可能不包含决定代词“her”指代谁所需的信息。相比之下，利用注意力机制，创建了一个新的隐藏表示，其中包含来自词“Dorothy”的大量信息，但来自其他当前不相关的词的信息较少。对于下一个词的预测，将再次计算新的隐藏表示，这可能看起来非常不同，因为现在模型可能希望更多地关注其他词，例如“scarecrow”。

注意力机制还有一个优点，即它允许训练的并行化。如前所述，在RNN中，你必须一个接一个地计算每个词的隐藏表示。在Transformer中，你在每一步计算隐藏表示，仅需单个词的表示。特别是，对于计算第t步的隐藏表示，你不需要第t-1步的隐藏表示。因此，你可以同时计算两者。

近年来模型规模的增加，使得模型每天的表现越来越好，这仅仅是因为技术上实现了这些模型的并行训练。使用循环神经网络，我们无法训练具有数百亿参数的模型，因此无法利用这些模型与自然语言交互的能力。Transformer的注意力机制可以看作是最后一个组成部分，它与大量的训练数据和足够的计算资源一起，为开发像GPT及其兄弟模型并启动人工智能和语言处理的持续革命提供了必要条件。

# 总结

那么，在这篇文章中我们看到了什么？我的目标是给你一个概述，介绍达到今天强大语言模型所必需的一些主要步骤。作为总结，以下是按顺序排列的重要步骤：

+   语言建模的关键方面是给定一段文本序列来预测下一个词。

+   n-gram模型只能表示有限的上下文。

+   循环神经网络具有更灵活的上下文，但它们的隐藏表示可能成为瓶颈，并且无法并行训练。

+   Transformer 通过引入注意力机制来避免瓶颈，这使得能够详细关注上下文的特定部分。最终，它们可以并行训练，这对于训练大型语言模型是必需的。

当然，为了达到我们今天拥有的模型，还需要许多其他技术。这一概述只是突出了一些非常重要的关键方面。你认为在迈向大型语言模型的过程中，还有哪些步骤是相关的？

# 进一步阅读

欲了解更多背景和技术细节，可以查看被称为语言建模圣经的书籍：

+   《语音与语言处理》，Dan Jurafsky 和 James H. Martin，第三版草稿。 [https://web.stanford.edu/~jurafsky/slp3/](https://web.stanford.edu/~jurafsky/slp3/)

以下论文介绍了迈向大型语言模型过程中的一些里程碑。

循环神经网络：

+   Elman, J. L. (1990). Finding structure in time. *认知科学*, *14*(2), 179–211.

+   Hochreiter, S., & Schmidhuber, J. (1997). Long short-term memory. *神经计算*, *9*(8), 1735–1780.

Transformer:

+   Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., … & Polosukhin, I. (2017). Attention is all you need. *神经信息处理系统进展*, *30*.

*喜欢这篇文章？* [*关注我*](https://medium.com/@doriandrost) *以便收到我未来的帖子通知。*
