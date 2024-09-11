# 训练LLMs的不同方式

> 原文：[https://towardsdatascience.com/different-ways-of-training-llms-c57885f388ed?source=collection_archive---------0-----------------------#2023-07-21](https://towardsdatascience.com/different-ways-of-training-llms-c57885f388ed?source=collection_archive---------0-----------------------#2023-07-21)

## 为什么提示不属于其中任何一种

[](https://medium.com/@doriandrost?source=post_page-----c57885f388ed--------------------------------)[![Dorian Drost](../Images/1795395ad0586eafd83d3e2f7b975ca8.png)](https://medium.com/@doriandrost?source=post_page-----c57885f388ed--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c57885f388ed--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c57885f388ed--------------------------------) [Dorian Drost](https://medium.com/@doriandrost?source=post_page-----c57885f388ed--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1d49ea537d1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdifferent-ways-of-training-llms-c57885f388ed&user=Dorian+Drost&userId=1d49ea537d1c&source=post_page-1d49ea537d1c----c57885f388ed---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c57885f388ed--------------------------------) ·10分钟阅读·2023年7月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc57885f388ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdifferent-ways-of-training-llms-c57885f388ed&user=Dorian+Drost&userId=1d49ea537d1c&source=-----c57885f388ed---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc57885f388ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdifferent-ways-of-training-llms-c57885f388ed&source=-----c57885f388ed---------------------bookmark_footer-----------)

在大型语言模型（LLMs）领域中，存在多种训练机制，具有不同的方式、要求和目标。由于它们服务于不同的目的，因此重要的是不要混淆它们，并了解它们适用的不同场景。

在本文中，我想概述一些最重要的训练机制，包括*预训练*、*微调*、*基于人类反馈的强化学习（RLHF）*和*适配器*。此外，我还将讨论*提示*的作用，尽管它本身不被视为学习机制，并阐明*提示调优*的概念，它在提示与实际训练之间架起了一座桥梁。

## 预训练

![](../Images/964977ee5f9e96d018d2ac954cd1baad.png)

“训练”。就像“训练”一样。我很抱歉……照片由 [Brian Suman](https://unsplash.com/ko/@briansuman?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

预训练是最基础的训练方式，相当于你可能知道的其他机器学习领域的训练。在这里，你从一个未经训练的模型（即权重随机初始化的模型）开始，并训练模型在给定一系列前序标记的情况下预测下一个标记。为此，从各种来源收集大量句子，并将其分成小块输入模型。

这里使用的训练模式称为 *自监督*。从被训练模型的角度来看，我们可以说这是一个监督学习方法，因为模型在做出预测后总是能得到正确的答案。例如，给定序列 *I like ice …* 模型可能预测 *cones* 作为下一个词，然后被告知答案是错误的，因为实际的下一个词是 *cream*。最终，可以计算损失，并调整模型权重，以便下次预测得更好。之所以称之为 *自* 监督（而不是简单地称为 *监督*），是因为不需要提前通过昂贵的程序来收集标签，而这些标签已经包含在数据中。给定句子 *I like ice cream*，我们可以自动将其分割为 *I like ice* 作为输入，*cream* 作为标签，这不需要人工干预。尽管这不是模型本身完成的，但仍由机器自动执行，因此在学习过程中，AI 自行监督的概念依然存在。

最终，通过大量文本的训练，模型学习了编码语言结构的一般规则（例如，它学习到 *I like* 可以被名词或分词跟随）以及文本中包含的知识。例如，它学习到，句子 *Joe Biden is …* 通常会跟随 *the president of the United States*，因此对这一知识有了表征。

这种预训练已经由其他人完成，你可以直接使用像 GPT 这样的模型。那么，为什么你还需要训练一个类似的模型呢？如果你处理的数据具有类似语言的特性，但不是普通语言本身，训练一个从头开始的模型可能是必要的。音乐记谱法可能是一个例子，它在某种程度上像语言一样有结构。存在某些规则和模式，关于哪些乐段可以跟随其他乐段，但一个训练在自然语言上的大型语言模型（LLM）不能处理这种数据，所以你需要训练一个新模型。然而，LLM 的架构可能适用，因为音乐记谱法和自然语言之间有许多相似之处。

## 微调

![](../Images/ab0f36248a06d12adc07fee4166b305c.png)

这些旋钮用于微调弦乐器。照片由 [Tony Woodhead](https://unsplash.com/pt-br/@portraitlondon?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

尽管一个预训练的 LLM 能够执行各种任务，这主要是由于它编码的知识，但它也存在两个主要的缺陷：输出结构和数据中缺失的知识。

正如你所知道的，LLM 总是根据前面的令牌序列预测下一个令牌。对于继续一个给定的故事，这可能没问题，但在其他情况下，这可能不是你想要的。如果你需要不同的输出结构，有两种主要方法可以实现这一目标。你可以通过设计提示，使得模型预测下一个令牌的固有能力完成你的任务（这称为*提示工程*），或者你可以改变最后一层的输出，使其反映你的任务，就像你在任何其他机器学习模型中一样。比如说分类任务，其中有*N*个类别。通过提示工程，你可以指示模型在给定输入后总是输出分类标签。通过微调，你可以将最后一层修改为*N*个输出神经元，并从激活度最高的神经元中得出预测的类别。

LLM 的另一个局限在于它所训练的数据。由于数据来源相当丰富，最著名的 LLM 编码了大量的常识。因此，它们可以告诉你，比如说，美国总统、贝多芬的主要作品、量子物理学的基本原理以及弗洛伊德的主要理论。然而，也有一些领域模型并不了解，如果你需要处理这些领域，微调可能对你有帮助。

微调的思想是利用一个已经预训练的模型，继续用不同的数据进行训练，并且在训练过程中只调整最后几层的权重。这仅需要初始训练所需资源的一小部分，因此可以更快地完成。另一方面，模型在预训练过程中学到的结构仍然编码在前几层中，并且可以加以利用。比如说，你想要教你的模型关于你喜欢的但不太知名的奇幻小说，这些小说并未包含在训练数据中。通过微调，你可以利用模型对自然语言的一般知识，让它理解新的奇幻小说领域。

## RLHF 微调

![](../Images/1514bb39ddf778332f53be9cf20e6c85.png)

RLHF 微调是关于最大化奖励的。照片由 [Alexander Grey](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

微调模型的一个特殊情况是基于人类反馈的强化学习（RLHF），这是GPT模型和像Chat-GPT这样的聊天机器人之间的主要区别之一。通过这种微调，模型被训练以产生人类在与模型对话时认为最有用的输出。

主要思想如下：给定一个任意提示，从模型中生成多个输出。人类根据这些输出的有用性或适当性对其进行排名。给定四个样本A、B、C和D，人类可能会决定C是最佳输出，B略差但与D相等，而A是最差的输出。这将导致排序C > B = D > A。接下来，这些数据用于训练一个奖励模型。这是一个全新的模型，它通过给予奖励来学习对LLM的输出进行评分，这些奖励反映了人类的偏好。一旦奖励模型训练完成，它就可以代替人类进行评分。现在，模型的输出由奖励模型进行评分，这个奖励作为反馈提供给LLM，然后LLM会根据最大化奖励进行调整；这个想法与GANs非常相似。

正如你所见，这种训练需要人工标注的数据，这需要相当大的努力。然而，所需的数据量是有限的，因为奖励模型的思想是从这些数据中进行概括，使其能够在学习其部分后独立对llm进行评分。RLHF常用于使LLM的输出更具对话性，或避免模型表现出不良行为，如模型变得刻薄、侵入或侮辱。

## 适配器

![](../Images/386b39fc4fdc975696588b89f402249a.png)

两种适配器可以插入到已经存在的网络中。图片来自 [https://arxiv.org/pdf/2304.01933.pdf](https://arxiv.org/pdf/2304.01933.pdf)。

在上述的微调过程中，我们调整了模型最后几层的一些参数，而之前几层的其他参数保持不变。然而，还有一种替代方案，通过减少所需训练的参数数量来提高效率，这就是所谓的*a*dapters*。

使用适配器意味着在已经训练好的模型上添加额外的层。在微调过程中，只有这些适配器被训练，而模型的其余参数完全不变。然而，这些层比模型自带的层要小得多，这使得调整它们变得更容易。此外，它们可以插入模型的不同位置，而不仅仅是最后的位置。在上面的图片中，你可以看到两个示例：一个是将适配器作为串行层添加，另一个是将其并行添加到已经存在的层中。

## 提示

![](../Images/699c09b7d2c2fb388ba8675664f2389f.png)

提示更多的是告诉模型要做什么，而不是如何去做。照片由 [Ian Taylor](https://unsplash.com/@carrier_lost?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你可能会想知道提示是否算作另一种训练模型的方法。提示是构建实际模型输入之前的指令，特别是如果你使用少量示例提示，你在提示中提供给大语言模型的示例，这与训练非常相似，训练也包含展示给模型的示例。然而，提示与训练模型有所不同。首先，从定义上讲，我们只在更新权重时才谈论训练，而提示过程中不会更新权重。创建提示时，你不会改变任何模型，也不会改变权重，不会产生新的模型，也不会改变模型中编码的知识或表示。提示应该被视为一种指导大语言模型的方法，告诉它你期望从它那里得到什么。以下提示作为一个例子：

```py
"""Classify a given text regarding its sentiment.

Text: I like ice cream.
Sentiment: negative

Text: I really hate the new AirPods.
Sentiment: positive

Text: Donald is the biggest jerk on earth. I hate him so much!
Sentiment: neutral

Text: {user_input}
Sentiment:"""
```

我指示模型进行情感分类，正如你可能已经注意到的，我给模型的示例全都是错误的！如果一个模型用这样的数据进行训练，它会混淆标签 *positive*（正面）、*negative*（负面）和 *neutral*（中性）。现在，如果我让模型对句子 *I like ice cream* 进行分类，而这个句子是我给出的示例之一，会发生什么？有趣的是，它将其分类为 *positive*（正面），这与提示相反，但在语义上是正确的。这是因为提示没有训练模型，也没有改变它所学到的表示。提示只是告诉模型我期望的结构，即我期望情感标签（可以是 *positive*、*negative* 或 *neutral*）跟在冒号后面。

## 提示调优

![](../Images/fe1aa433918ad848d9d51e23e7f84523.png)

提示调优也叫做软提示。软如美利奴羊毛……照片由 [Advocator SY](https://unsplash.com/@advocator_sy?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

尽管提示本身并没有训练大语言模型，但有一种机制叫做 *提示调优*（也叫 *软提示*），它与提示相关，可以视为一种训练。

在前面的例子中，我们将提示视为一种自然语言文本，这种文本提供给模型以告诉它要做什么，并且在实际输入之前。这就是说，模型输入变成了<prompt><instance>，例如，<*将以下内容标记为正面、负面或中性：> <我喜欢冰淇淋>。当我们自己创建提示时，我们称之为*硬提示*。在*软提示*中，格式<prompt><instance>将保持不变，但提示本身不是由我们设计的，而是通过数据学习的。具体来说，提示由一个向量空间中的参数组成，这些参数可以在训练过程中进行调优，以获得更小的损失，从而得到更好的回答。也就是说，在训练之后，提示将是那种字符序列，这种序列能为我们提供的数据得到最佳回答。然而，模型参数本身没有经过训练。

提示调优的一个大优点是，你可以为不同的任务训练多个提示，但仍然可以在相同的模型中使用它们。就像在硬提示中，你可能会构造一个用于文本摘要的提示，一个用于情感分析的提示，以及一个用于文本分类的提示，但在同一个模型中使用所有这些提示，你可以为这些目的调优三个提示，并且仍然使用相同的模型。如果你使用微调，恰恰相反，你最终会得到三个只服务于特定任务的模型。

## 总结

我们刚刚看到了各种不同的训练机制，因此让我们在最后做一个简短的总结。

+   预训练LLM意味着以自我监督的方式教它预测下一个标记。

+   微调是调整预训练LLM最后几层的权重，可以用来将模型适应特定的上下文。

+   RLHF的目标是调整模型的行为以匹配人类的期望，并且需要额外的标注工作。

+   适配器通过向预训练的LLM添加小层，提供了一种更高效的微调方式。

+   提示不被认为是训练，因为它不会改变模型的内部表示。

+   提示调优是一种调整生成提示的权重的技术，但不会影响模型的权重本身。

当然，还有许多其他的训练机制，每天都有新的机制被发明。LLM能做的远不止预测文本，教它们做到这些需要多种技能和技术，其中一些我刚刚向你介绍过。

## 进一步阅读

Instruct-GPT是RLHF最著名的例子之一：

+   [https://openai.com/research/instruction-following](https://openai.com/research/instruction-following)

+   Ouyang, L., Wu, J., Jiang, X., Almeida, D., Wainwright, C., Mishkin, P., … & Lowe, R. (2022). 训练语言模型以根据人类反馈遵循指令。*神经信息处理系统进展*, *35*, 27730–27744.

常见适配器形式的概述可以在LLM-Adapters项目中找到：

+   [https://github.com/AGI-Edgerunners/LLM-Adapters](https://github.com/AGI-Edgerunners/LLM-Adapters)

+   Hu, Z., Lan, Y., Wang, L., Xu, W., Lim, E. P., Lee, R. K. W., … & Poria, S. (2023). LLM-Adapters: 一种用于大规模语言模型参数高效微调的适配器家族。 *arXiv 预印本 arXiv:2304.01933*。

提示调优的探索可以在这里找到：

+   Lester, B., Al-Rfou, R., & Constant, N. (2021). 规模的力量在参数高效提示调优中的作用。 *arXiv 预印本 arXiv:2104.08691*。

一些关于提示调优的不错解释可以在这里找到：

+   [https://huggingface.co/docs/peft/conceptual_guides/prompting](https://huggingface.co/docs/peft/conceptual_guides/prompting)

+   [https://ai.googleblog.com/2022/02/guiding-frozen-language-models-with.html](https://ai.googleblog.com/2022/02/guiding-frozen-language-models-with.html)

*喜欢这篇文章吗？* [*关注我*](https://medium.com/@doriandrost) *以便获取我未来的帖子通知。*
