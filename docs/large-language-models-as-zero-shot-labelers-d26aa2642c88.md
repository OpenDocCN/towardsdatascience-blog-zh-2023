# 大型语言模型作为零-shot 标注者

> 原文：[`towardsdatascience.com/large-language-models-as-zero-shot-labelers-d26aa2642c88`](https://towardsdatascience.com/large-language-models-as-zero-shot-labelers-d26aa2642c88)

## 使用 LLMs 为监督模型获取标签

[](https://medium.com/@devins?source=post_page-----d26aa2642c88--------------------------------)![Devin Soni](https://medium.com/@devins?source=post_page-----d26aa2642c88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d26aa2642c88--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d26aa2642c88--------------------------------) [Devin Soni](https://medium.com/@devins?source=post_page-----d26aa2642c88--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d26aa2642c88--------------------------------) ·5 分钟阅读·2023 年 3 月 20 日

--

![](img/4517fd2ff5ae2bea5a595bff399475b2.png)

图片由 [h heyerlein](https://unsplash.com/es/@heyerlein?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

数据标注是构建监督式机器学习模型中的一个关键步骤，因为标签的数量和质量通常是决定模型性能的主要因素。

然而，数据标注可能非常耗时且昂贵，特别是对于涉及领域知识或阅读大量数据的复杂任务。

近年来，大型语言模型（LLMs）作为获取文本数据标签的强大解决方案出现。通过零-shot 学习，我们可以仅使用 LLM 的输出来获取未标记数据的标签，而不必让人类来获取这些标签。这可以显著降低获取标签的成本，并使过程变得更加可扩展。

在这篇文章中，我们将进一步探讨零-shot 学习的概念以及 LLMs 如何用于这一目的。

# 什么是零-shot 学习？

![](img/e6b0b7b78d31241482486c84fdd7e723.png)

图片由 [Alex Knight](https://unsplash.com/@agk42?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

零-shot 学习（ZSL）是机器学习中的一种问题设置，其中模型被要求解决一个它没有经过训练的预测任务。这通常涉及将数据识别或分类为训练时未明确见过的概念。

在传统的监督学习中，这是不可能的，因为模型只能对其训练过的任务（即有标签的任务）进行预测。然而，在 ZSL 范式中，模型可以推广到任意未见的任务，并在合理的水平上表现。请注意，在大多数情况下，针对特定任务训练的监督模型仍将优于使用 ZSL 的模型，因此 ZSL 更常在监督标签尚不可用时使用。

ZSL 最有前景的应用之一是在数据标记中，它可以显著降低获取标签的成本。如果一个模型能够自动将数据分类到类别中，而无需在该任务上进行训练，它可以用于为下游监督模型生成标签。这些标签可以用于引导一个监督模型，这种方式类似于主动学习或人机交互式机器学习。

# 大语言模型的零样本学习

![](img/a125bdc1b8fe07f54ee9edc4871db98a.png)

由[Arseny Togulev](https://unsplash.com/@tetrakiss?utm_source=medium&utm_medium=referral)拍摄，图片来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

像 GPT-3 这样的 LLM 是 ZSL 的强大工具，因为它们的稳健预训练过程使它们对自然语言有全面的理解，这种理解并不依赖于某个特定监督任务的标签。

## 嵌入搜索

LLM 的上下文嵌入能够捕捉给定文本中的语义概念，这使得它们在 ZSL 中非常有用。

像[句子转换器](https://github.com/UKPLab/sentence-transformers)这样的库提供了经过训练的 LLM，这些 LLM 使得语义相似的文本具有彼此距离很小的嵌入。

[](https://github.com/UKPLab/sentence-transformers?source=post_page-----d26aa2642c88--------------------------------) [## GitHub - UKPLab/sentence-transformers: 多语言句子与图像嵌入与 BERT

### 这个框架提供了一种计算句子、段落和图像的稠密向量表示的简单方法……

github.com](https://github.com/UKPLab/sentence-transformers?source=post_page-----d26aa2642c88--------------------------------)

如果我们有一些标记数据的嵌入，我们可以使用最近邻搜索来找到具有相似嵌入的未标记数据。

如果两个文本在嵌入空间中非常接近，那么它们很可能具有相同的标签。

## 上下文学习

[上下文学习](http://ai.stanford.edu/blog/understanding-incontext/)是 LLM 的一种新兴能力，它使它们能够仅通过查看输入-输出对来学习解决新任务。模型无需更新参数即可学习任意新任务。

[](http://ai.stanford.edu/blog/understanding-incontext/?source=post_page-----d26aa2642c88--------------------------------) [## 上下文学习如何运作？理解与传统学习的差异框架…

### 在这篇文章中，我们提供了一个关于大语言模型如 GPT-3 的上下文学习的贝叶斯推理框架。

ai.stanford.edu](http://ai.stanford.edu/blog/understanding-incontext/?source=post_page-----d26aa2642c88--------------------------------)

我们可以利用这种能力，通过仅提供几个输入-输出对来获得标签，并让模型为未标记的数据点提供标签。

在零样本学习的背景下，这意味着我们可以提供一些手工制作的文本示例及其相关的监督标签，让模型实时学习标注功能。

![](img/ad50a16d50b0f93dee71ca00397fbfb1.png)

在这个简单的案例中，我们训练 ChatGPT 通过上下文学习来分类一个句子是否与青蛙相关。

## 生成模型

最近，生成 LLMs 中的对齐方法，如[RLHF（来自人类反馈的强化学习）](https://huggingface.co/blog/rlhf)的进展，使得只需要求模型为你标注数据成为可能。

[](https://huggingface.co/blog/rlhf?source=post_page-----d26aa2642c88--------------------------------) [## 说明来自人类反馈的强化学习（RLHF）

### 本文已翻译成中文简体。对翻译成其他语言感兴趣？请联系 nathan…

huggingface.co](https://huggingface.co/blog/rlhf?source=post_page-----d26aa2642c88--------------------------------)

如 ChatGPT 这样的模型能够通过简单地（用语言）回复所需标签来为输入数据提供标签。它们通过在如此大量的数据上进行预训练获得的丰富世界知识，使这些模型能够仅凭语义理解来解决新任务。

这个过程可以通过使用开源模型如[FLAN-T5](https://huggingface.co/google/flan-t5-xxl)来自动化，只需要求模型仅用你的标签集中的项回应（例如，“回应‘是’或‘否’”），然后在询问模型标签后检查哪个选项的输出概率最高。

![](img/1548eb1c4ec958355a2c58b1d51dd1d7.png)

ChatGPT 不仅能提供标签，还能解释其获得该标签的逻辑。

# 结论

数据标注是监督机器学习中的关键步骤，但获取大量标注数据可能很昂贵。

使用零样本学习和 LLMs，我们可以显著降低标签获取的成本。

LLMs 在大量数据上进行预训练，编码了对世界信息的语义理解，使它们在任意未见任务上表现出色。这些模型可以高准确度地自动标注数据，使我们可以以低成本启动监督模型。
