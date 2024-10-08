# 一步步解决 4 个实际问题的指南：使用变压器和 Hugging Face

> 原文：[`towardsdatascience.com/4-real-life-problems-solved-using-transformers-and-hugging-face-a-complete-guide-e45fe698cc4d`](https://towardsdatascience.com/4-real-life-problems-solved-using-transformers-and-hugging-face-a-complete-guide-e45fe698cc4d)

## 了解变压器并利用它们的力量在 Python 中解决实际问题

[](https://zoumanakeita.medium.com/?source=post_page-----e45fe698cc4d--------------------------------)![Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----e45fe698cc4d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e45fe698cc4d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e45fe698cc4d--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----e45fe698cc4d--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e45fe698cc4d--------------------------------) ·阅读时间 11 分钟·2023 年 1 月 31 日

--

![](img/429c05a1b2db762b98d5755b81d78326.png)

图片来源：[Aditya Vyas](https://unsplash.com/@aditya1702) 在 [Unsplash](https://unsplash.com/photos/B9MULm2UZIk)

# 介绍

在自然语言处理（NLP）领域，研究人员在过去几十年里做出了显著贡献，带来了各种领域的创新进展。以下是 NLP 在实践中的一些例子：

+   **Siri**，由苹果公司开发的个人助理，可以帮助用户完成设置闹钟、发送短信和回答问题等任务。

+   在**医疗领域**，NLP 正被用来加速药物发现过程。

+   此外，NLP 还被用于通过**翻译**来弥合语言障碍。

本文的目的是讨论变压器，这是一种在自然语言处理领域中极为强大的模型。它将首先强调变压器相对于循环神经网络的优势，以加深你对该模型的理解。接着，将提供使用 Huggingface 变压器在实际场景中的应用实例。

# 循环网络——变压器之前的辉煌时代

在深入了解变压器的基本概念之前，了解循环模型及其局限性是非常重要的。

循环网络使用编码器-解码器结构，通常用于处理按特定顺序输入和输出序列的任务。循环网络的一些最常见的应用包括机器翻译和时间序列数据建模。

## 循环网络的挑战

例如，让我们以一句法语句子为例，将其翻译成英语。编码器接收原始法语句子作为输入，解码器生成翻译输出。

![](img/8c8639426bdc40dfafc77c5b86aa8163.png)

递归网络的简单示意图（作者提供）

+   编码器逐词处理输入的法语句子，而解码器以相同的顺序生成单词嵌入，这使得训练非常耗时。

+   当前词的隐藏状态依赖于前面词的隐藏状态，这使得无法进行并行计算，无论计算能力多么强大。

+   序列到序列的神经网络在网络过大时容易遇到梯度爆炸的问题，导致性能不佳。

+   另一种递归网络——长短期记忆（LSTM）网络，被开发出来解决梯度消失的问题，但它们的速度甚至比传统序列模型还慢。

> 结合递归网络的优点并实现并行计算的模型难道不是很有用吗？
> 
> **这就是变换器派上用场的地方。**

# 在自然语言处理（NLP）中，变换器是什么？

2017 年，Google Brain 在其著名的研究论文《Attention is all you need》中介绍了变换器，这是一种新型强大的神经网络架构。它基于注意力机制，而不是递归网络中的序列计算。

# 变换器的主要组件是什么？

像递归网络一样，变换器也由两个主要组件组成：编码器和解码器，每个组件都包含一个自注意力机制。以下部分提供了对变换器每个组件主要元素的总体理解。

![](img/d25a9de3087b7b52d0c3fcd3f74227cd.png)

一般 [变换器架构](https://arxiv.org/abs/1706.03762)（作者改编）

## 输入句子预处理阶段

这一部分包括两个主要步骤：（1）创建输入句子的嵌入，和（2）计算输入句子中每个单词的位置信息。这些计算在输入句子（编码器块之前）和输出句子（解码器块之前）中以相同的方式进行。

**输入数据的嵌入**

在创建输入数据的嵌入之前，我们首先对其进行分词，然后为每个单词创建嵌入，而不考虑它们在句子中的关系。

**位置编码**

分词过程消除了输入句子中存在的任何连接感。位置编码旨在通过为每个单词创建上下文向量来恢复原始的循环特性。

## 编码器块

通过前面的步骤，我们为每个单词得到两个向量的组合：（1）嵌入向量和（2）上下文向量。这些向量被结合起来，为每个单词创建一个单一向量，然后送到编码器中。

**多头注意力**

如前所述，所有的关系感都丧失了。注意力层的目的是识别输入句子中不同单词之间的上下文连接。这个步骤最终会为每个单词创建一个注意力向量。

**位置相关前馈网络（FFN）**

在这一步中，应用前馈神经网络于每个注意力向量，将其转换为可以由解码器中的下一个多头注意力层使用的格式。

## 解码器块

解码器块由三个主要层组成：一个被遮蔽的多头注意力层、一个多头注意力层和一个位置相关的前馈网络。后两个层与编码器中的层类似。

解码器在训练过程中使用，它接受两个输入：正在翻译的输入句子的注意力向量和对应的英文目标句子。

> **那么，被遮蔽的多头注意力层负责什么呢？**

在生成下一个英文单词时，网络可以使用来自法语单词的所有单词。然而，当处理目标序列中的某个单词（英文翻译）时，网络只需访问之前的单词，因为让下一个单词可用会导致网络“作弊”，而不去真正努力学习。这就是被遮蔽的多头注意力层发挥作用的地方。它通过将这些下一个单词转换为零来遮蔽它们，以便注意力网络不能使用这些单词。

被遮蔽的多头注意力层的结果通过其余的层，以生成概率分数来预测下一个单词。

# 自然语言处理中的迁移学习

训练从头开始的深度神经网络（如变换器）不是一件容易的事，并且可能会面临以下挑战：（1）寻找目标问题所需的数据量可能非常耗时，以及（2）获得必要的计算资源，如 GPU，以训练如此深的网络可能会非常昂贵。

想象一下从头开始构建一个模型，以将***曼丁哥语翻译成沃洛夫语***，这两种语言都是低资源语言***。*** 收集与这些语言相关的数据是昂贵的。与其经历所有这些挑战，不如重新使用经过预训练的深度神经网络作为训练新模型的起点。

这些模型已经在一个巨大的数据语料库上进行了训练，由他人（道德人士、组织等）提供，并在语言翻译任务上（如法语到英语）评估效果非常好。

> 但是，你所说的重新使用深度神经网络是什么意思？

重新使用模型涉及选择一个与您的用例相似的预训练模型，细化目标任务的输入输出对数据，并使用您的数据对预训练模型的高层进行再训练。

变换器的引入导致了最先进的迁移学习模型的发展，如：

+   BERT，全称是 **B**idirectional **E**ncoder **R**epresentations from **T**ransformers，由 Google 研究人员于 2018 年开发。它有助于解决最常见的语言任务，如命名实体识别、情感分析、问答、文本总结等。

+   GPT3（生成预训练变换器 3），由 OpenAI 研究人员提出。它是一个多层 transformer，主要用于生成各种类型的文本。GPT 模型能够生成类似人类的文本响应来回答给定的问题。

# Hugging Face Transformers 介绍

Hugging Face 是一个人工智能社区和机器学习平台，由 Julien Chaumond、Clément Delangue 和 Thomas Wolf 于 2016 年创建。它的目标是通过为数据科学家、人工智能从业者和工程师提供对 20000 多个基于最先进的 transformer 架构的预训练模型的即时访问，来实现自然语言处理的民主化。这些模型可以应用于：

+   100 多种语言的文本，用于执行分类、信息提取、问答、生成、生成和翻译等任务。

+   语音，用于对象音频分类和语音识别等任务。

+   用于对象检测、图像分类和分割的视觉。

Hugging Face transformers 还提供了近 2000 个数据集和分层 API，使程序员可以使用三种最流行的深度学习库：Pytorch、Tensorflow 和 Jax，轻松与这些模型进行交互。

Hugging Face transformers 的其他组件是 [Pipelines](https://huggingface.co/docs/transformers/main_classes/pipelines)。这些对象抽象了库中的代码复杂性，使得使用所有这些模型进行推理变得简单。

# Hugging Face 教程

现在你对 transformers 和 Hugging Face 平台有了更好的理解，我们将带你了解以下实际场景：序列分类、语言翻译、文本生成、问答、命名实体识别和文本总结。

## 前提条件

第一步是按照如下步骤安装 transformers 库：

```py
pip install transformers
```

我们将使用来自 [Kaggle](https://www.kaggle.com/datasets/szymonjanowski/internet-articles-data-with-users-engagement) 的互联网新闻和消费者互动数据集。该数据集由 [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/) 免费提供，旨在预测文章在发布前的受欢迎程度。

为了简化教程，本教程将仅使用数据中的三个示例，分析基于描述列。

```py
import pandas as pd

# Create wrapper to properly format the text
from textwrap import TextWrapper

# Wrap text to 80 characters.
wrapper = TextWrapper(width=80)

# Load the data
news_data = pd.read_csv("consumer_engagement_data.csv")

# Choose candidate descriptions
description_76 = news_data.iloc[76]["description"]
description_118 = news_data.iloc[118]["description"]
description_178 = news_data.iloc[178]["description"]

english_texts = [description_76, description_118, description_178]

for english_text in english_texts:
    print(wrapper.fill(english_text))
    print("\n")
```

![](img/8bd3cd5bb79bd7e0bd60b35fd7b967ea.png)

英文原新闻（图片由作者提供）

## 语言翻译

MarianMT 是一个高效的机器翻译框架。它使用了[MarianNMT](https://marian-nmt.github.io/)引擎，该引擎完全由微软和许多学术机构（如爱丁堡大学和波兹南的亚当·密茨凯维奇大学）用 C++开发。目前，同一引擎也用于[微软翻译器](http://translator.microsoft.com/)服务。

[赫尔辛基大学的 NLP 小组](https://blogs.helsinki.fi/language-technology/)在 Hugging Face Transformers 上开源了多个翻译模型，它们的格式都是`Helsinki-NLP/opus-mt-{src}-{tgt}`，其中`{src}`和`{tgt}`分别对应源语言和目标语言。

所以，在我们的案例中，源语言是英语（en），目标语言是法语（fr）。

MarianMT 是那些使用 Marian 在[Opus](https://opus.nlpl.eu/)上收集的平行数据上进行过训练的模型之一。

+   MarianMT 除了 Transformers 外还需要`sentencepiece`：

```py
pip install sentencepiece
from transformers import MarianTokenizer, MarianMTModel
```

+   选择预训练模型，获取分词器并加载预训练模型。

```py
# Get the name of the model
trans_model_name = 'Helsinki-NLP/opus-mt-en-fr'
```

```py
# Get the tokenizer
trans_model_tkn = MarianTokenizer.from_pretrained(trans_model_name)
```

```py
# Instanciate the model
trans_model = MarianMTModel.from_pretrained(trans_model_name)
```

+   使用以下函数在每个源（英语）文本前添加特殊标记`>>{tgt}<<`。

+   使用以下函数实现批量翻译逻辑，批量是要翻译的文本列表。

![](img/f9f8f7fef13b97168fc274c8e6575fca.png)

翻译文本

## 零样本分类

大多数情况下，训练机器学习模型需要事先知道所有候选标签/目标，这意味着如果你的训练标签是**科学**、**政治**或**教育**，你将无法预测**医疗**标签，除非重新训练你的模型，考虑到该标签和相应的输入数据。

这种强大的方法使得我们可以在大约 15 种语言中预测文本的目标，而无需看到任何候选标签。我们只需从中心加载模型即可使用它。

这里的目标是尝试对每个先前描述的类别进行分类，无论是***技术***、***政治***、***安全***还是***金融***。

+   导入管道模块

```py
from transformers import pipeline
```

+   定义候选标签。这些标签对应于我们想要预测的内容：***技术***、***政治***、***商业***或***金融***。

```py
candidate_labels = ["tech", "politics", "business", "finance"]
```

+   使用多语言选项定义分类器

```py
my_classifier = pipeline("zero-shot-classification",       
                         model='joeddav/xlm-roberta-large-xnli')
```

+   使用这个辅助函数实现预测逻辑。

+   对第一个和最后一个描述进行预测

```py
#For the first description
prediction_desc_76 = run_predictions(english_texts[0])

print(wrapper.fill(prediction_desc_76["Text"]))
```

![](img/b623f00081e60981cd526c2504213bf0.png)

原始文本描述

```py
print(prediction_desc_76["Result"])
```

![](img/793ab28e5f86f849926116e2cb691e7f.png)

预测为主要涉及金融的文本（作者提供的图像）

这个结果显示文本总体上是关于金融的，比例为 81%。

对最后一个描述，我们得到以下结果：

```py
#For the last description
prediction_desc_178 = run_predictions(english_texts[-1])

print(wrapper.fill(prediction_desc_178["Text"]))
```

![](img/7bc527410c6eaba16da38414e18b7c31.png)

```py
print(prediction_desc_178["Result"])
```

![](img/a8f6409eb75d5aa89b802f3ebb35ddd4.png)

预测为主要涉及技术的文本（作者提供的图像）

之前的结果显示文本总体上关于技术，可信度为 95%。

## 情感分类

大多数进行情感分类的模型需要适当的训练。Hugging Face 的 pipeline 模块通过指定在中心可用的特定模型的名称，使情感分析预测变得简单。

+   加载 pipeline 模块

```py
from transformers import pipeline
```

+   选择要执行的任务并加载相应的模型。在这里，我们想要执行情感分类，使用 distill BERT base 模型。

```py
distil_bert_model = pipeline(task = "sentiment-analysis", 
                          model="distilbert-base-uncased-finetuned-sst-2-english")
```

+   模型已经准备好了！让我们分析最后两句话背后的潜在情感。

```py
# Run the predictions
distil_bert_model(english_texts[1:])
```

![](img/b745e22c0ab4a2fdf757d446d8dc82e3.png)

情感评分（作者提供的图片）

该模型预测第一段文本的情感为负面，置信度为 96%，第二段则预测为正面，置信度为 52%。

## 问答

想象一下处理一份比苹果公司报告长得多的报告。你唯一感兴趣的只是提到事件的日期。与其阅读整个报告来找到关键信息，我们可以使用 Hugging Face 的问答模型，它将提供我们感兴趣的答案。

这可以通过向模型提供适当的上下文（苹果公司的报告）以及我们感兴趣的问题来实现。

+   从 transformers 导入问答类和 tokenizer

```py
from transformers import AutoModelForQuestionAnswering, AutoTokenizer
```

+   使用模型名称及其 tokenizer 实例化模型。

```py
model_name = "deepset/roberta-base-squad2"
QA_model = pipeline('question-answering', model=model_name, 
                     tokenizer=model_name)
```

+   通过提出问题并指定上下文来请求模型。

```py
QA_input = {
           'question': 'when is Apple hosting an event?',
           'context': english_texts[-1]
           }
```

+   获取模型结果

```py
model_response = QA_model(QA_input)
pd.DataFrame([model_response])
```

![](img/15915fd60f52c1f919d5e604f43d34c0.png)

问答结果（作者提供的图片）

模型回答说苹果公司的事件是在 9 月 10 日，置信度为 97%。它甚至通过提供开始和结束位置来指定答案在文本中的位置。

# 结论

在本文中，我们探讨了自然语言技术从递归网络到 transformers 的演变，以及 Hugging Face 如何通过其平台使 NLP 使用变得民主化。

如果你仍然犹豫是否使用 transformers，我们认为是时候尝试一下，给你的业务案例增添价值。

如果你喜欢阅读我的故事并希望支持我的写作，可以考虑[成为 Medium 会员](https://zoumanakeita.medium.com/membership)。通过每月 $5 的承诺，你可以无限制地访问 Medium 上的故事。

欢迎在[Medium](https://zoumanakeita.medium.com/)、[Twitter](https://twitter.com/zoumana_keita_) 和[YouTube](https://www.youtube.com/channel/UC9xKdy8cz6ZuJU5FTNtM_pQ)上关注我，或者在[LinkedIn](https://www.linkedin.com/in/zoumana-keita/)上打个招呼。讨论 AI、ML、数据科学、NLP 和 MLOps 话题总是很愉快！

文章你应该考虑使用 Datatable 而不是 Pandas 来处理大数据吗？可能是一个好的下一步。
