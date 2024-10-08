# 自然语言基础——情感分析、机器翻译和命名实体识别的介绍与语言模型实现

> 原文：[`towardsdatascience.com/natural-language-fundamentals-intro-implementation-of-sentiment-analysis-machine-translation-c79662e52624`](https://towardsdatascience.com/natural-language-fundamentals-intro-implementation-of-sentiment-analysis-machine-translation-c79662e52624)

## 自然语言领域的多语言建模

[](https://medium.com/@fmnobar?source=post_page-----c79662e52624--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----c79662e52624--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c79662e52624--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c79662e52624--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----c79662e52624--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c79662e52624--------------------------------) ·阅读时间 9 分钟·2023 年 3 月 20 日

--

![](img/93e4900e458bc59ccf6ab8f6b69ece8c.png)

图片由 [RetroSupply](https://unsplash.com/@retrosupply) 提供，来自 [Unsplash](https://unsplash.com/photos/kZvmEpyfiJs)

我们人类使用语言、声音、手势和符号，以不同的形式（如演讲、写作和标志）传达复杂的概念和抽象。随着计算机的出现，为了利用这些强大的机器，我们不得不想出让计算机理解人类交流和现有知识库的方法。因此，自然语言处理（NLP）、理解（NLU）和生成（NLG）这三个人工智能的分支应运而生。这三个领域的界限有时并不清晰，而整体自然语言空间涵盖了当今计算机和数据科学世界中的各种应用。最常见的应用包括（I）情感分析，（II）机器翻译和（III）命名实体识别（NER），我们将在本文中对这些应用进行定义和实现。

[](https://medium.com/@fmnobar/membership?source=post_page-----c79662e52624--------------------------------) [## 通过我的推荐链接加入 Medium

### 阅读 Farzad（以及 Medium 上其他作者）的每一个故事。您的会员费用直接支持 Farzad 和其他人……

medium.com](https://medium.com/@fmnobar/membership?source=post_page-----c79662e52624--------------------------------)

为了实现这三项任务，我们将利用现有的“预训练语言模型”。因此，让我们首先了解什么是语言建模。我建议浏览一下“语言建模”部分，但如果你主要对应用和/或实现感兴趣，可以跳过这部分。

# 语言建模

语言建模包括使用统计和概率的方法，结合机器学习架构（如特别是深度神经网络）来确定一串文字中词语序列出现的可能性，例如一个句子。基于计算出的概率，可以做出某些决策——例如，一个模型可以生成对用户提供的提示的字符串/响应（如 ChatGPT），执行文本分类以确定某个词是否为名词、动词等。由于如今我们周围有大量的文本数据，这些语言模型通常在大量文本数据上进行训练。因此，这些模型也被称为大语言模型。此时你可能会想这与我们的帖子有何关联——我们正要到达这里。这些预训练的语言模型可以进一步训练（即微调）以执行特定任务，如情感分析、机器翻译和命名实体识别，我们今天将详细探讨这些任务。深入了解语言模型的架构和训练策略超出了本文的意图，但如果你对这一主题感兴趣，可以访问以下帖子：

[## 预训练模型在自然语言处理中的应用](https://medium.com/@fmnobar/intro-to-pre-trained-models-in-nlp-6bf7490a49fa?source=post_page-----c79662e52624--------------------------------)

### 预训练模型基础及其下游任务。

[medium.com](https://medium.com/@fmnobar/intro-to-pre-trained-models-in-nlp-6bf7490a49fa?source=post_page-----c79662e52624--------------------------------)

既然我们对自然语言领域和语言建模有所了解，接下来就进入使用这些模型的有趣部分吧！

# 1. 情感分析

识别文本情感的任务，例如判断它是积极的、消极的还是中立的，称为情感分析。它被用于社交媒体监控、客户反馈分析和产品评论分析等应用中。可以想象，这对许多公司来说非常有用。例如，一家大型在线零售公司无法分配足够的人力资源来手动阅读关于各种产品的所有评论。相反，他们可以对评论运行情感分析模型，并分析结果，从而节省时间和成本。接下来，让我们看看如何实现这一点。

## 1.1. 情感分析 — 实现

在这个示例中，我们首先从 Transformers 库中加载一个预训练的语言模型。接着，我们使用模型从输入句子中生成情感分析。最后，我们在两个不同的句子上测试它，一个是积极的，另一个是消极的，以验证模型的性能。以下是我们将使用的两个句子：

+   `I loved this movie!`，我们期望模型将其分类为“积极”情感

+   `I did not like this movie.`，我们期望模型将其分类为“消极”情感

让我们看看它是如何工作的！

```py
# Import libraries
from transformers import pipeline

# Load the pre-trained model
nlp = pipeline('sentiment-analysis', model='distilbert-base-uncased-finetuned-sst-2-english')

# Define the function to perform sentiment analysis
def sentiment_analyzer(input_text):
    # Perform sentiment analysis
    result = nlp(input_text)[0]

    # Return results
    return f"'{input_text}' has a {result['label']} sentiment, with a score of {round(result['score'], 4)}!\n"

# Define example sentences
sentence_1 = "I loved this movie!"
sentence_2 = "I did not like this movie."
sentence_list = [sentence_1, sentence_2]

# Analyze the sentiment of each sentence
for sentence in sentence_list:
    print(sentiment_analyzer(sentence))
```

结果：

![](img/a7b2a502990181b233f5a8c4829fa741.png)

结果看起来非常好，如我们所期待的。如果你对情感分析有更深入的兴趣，以下帖子适合你：

## 情感分析——简介与实现

### 使用 NLTK、scikit-learn 和 TextBlob 进行情感分析

towardsdatascience.com

接下来，让我们讨论机器翻译。

# 2. 机器翻译

使用计算机将文本从一种语言翻译到另一种语言的任务称为机器翻译。对大多数用户来说，最著名的例子是 Google 翻译——Google 翻译所做的就是机器翻译！应用非常广泛。例如，人们可以阅读和理解其他语言的信息。

## 2.1. 机器翻译——实现

要实现机器翻译，我们将使用由[Facebook AI](https://arxiv.org/pdf/2008.00401.pdf)开发的 mBART-50，它来自[Transformers](https://huggingface.co/docs/transformers/index)库，这是一个用于机器翻译的预训练语言模型。步骤与我们在情感分析中做的非常相似，具体如下：

1.  按如下方式安装 Transformers：`pip install transformers`

1.  导入库

1.  加载预训练模型

1.  通过模型运行示例句子并返回结果

mBART-50 的有趣之处在于它是一个多语言机器翻译模型，这意味着它可以进行不同语言之间的翻译。让我们测试一下这个功能，将一个句子翻译成 6 种不同的语言，使用一个模型！

> **提示：** 我强调了这个语言模型的多语言特性，因为历史上，神经机器翻译模型只能从一种特定语言翻译到另一种特定语言。例如，我们需要为从英语到法语的翻译训练一个特定的模型，并为从英语到德语的翻译训练另一个模型。另一方面，这些多语言机器翻译模型可以使用一个模型在多种语言之间进行翻译，这非常令人印象深刻！

在下面的代码块中，我们导入库，加载模型，然后创建 `translator` 函数，该函数接受三个参数：（1）一个句子（`source_sentence`），（2）提供的句子的语言（`source_language`）以及（3）我们希望翻译成的语言（`target_language`）。然后 `translator` 按照指示返回翻译结果。

```py
# Import library
from transformers import MBartForConditionalGeneration, MBart50TokenizerFast

# Load model and tokenizer
model = MBartForConditionalGeneration.from_pretrained("facebook/mbart-large-50-many-to-many-mmt")
tokenizer = MBart50TokenizerFast.from_pretrained("facebook/mbart-large-50-many-to-many-mmt")

def translator(source_sentence, source_language, target_language):
    # Encode sentence
    tokenizer.src_lang = source_language
    input_ids = tokenizer(source_sentence, return_tensors="pt").input_ids

    # Translate sentence
    output_ids = model.generate(input_ids, forced_bos_token_id=tokenizer.lang_code_to_id[target_language])
    translation = tokenizer.batch_decode(output_ids, skip_special_tokens=True)[0]

    # return translation
    return translation
```

接下来，让我们通过将 `Multilingual machine translation is impressive!` 翻译成法语、西班牙语、意大利语、德语、简体中文和日语来测试我们的函数。

```py
# Define sentence to be translated
original_sentence = 'Multilingual machine translation is impressive!'

# Define source language
english = "en_XX"

# Define target languages
french = "fr_XX"
spanish = "es_XX"
italian = "it_IT"
german = "de_DE"
simplified_chinese = "zh_CN"
japanese = "ja_XX"

# Create a list of target languages
target_list = [french, spanish, italian, german, simplified_chinese, japanese]

# Create a prompt list of lists
prompt_list = []

for target in target_list:
    prompt_list.append([original_sentence, english, target])

# Create translations
print(f"Generating machine translations for: \n'{original_sentence}'\n")

for i in enumerate(prompt_list):
    translation = translator(source_sentence=i[1][0], source_language=i[1][1], target_language=i[1][2])
    print(f"{i[1][2]}:")
    print(f"{translation}\n")
```

结果：

![](img/33ff48d10d8bc93efc50209423b753b2.png)

我们可以在结果中看到目标语言的翻译！我个人不讲这些语言，但我使用 Google 翻译验证了这些翻译，结果似乎是准确的！

最后但同样重要的是，让我们看一下命名实体识别。

# 3\. 命名实体识别（NER）

识别和分类文本中的命名实体，并将其分类到预定义类别中的任务称为命名实体识别，简称 NER。这些类别的一些示例包括：人名、地点、日期、组织、数字等。你可能会想，为什么我们需要这样的模型。NER 在自然语言领域有许多应用。例如，Visa、美国运通、亚马逊等可以使用 NER 识别和屏蔽客户通信中的敏感信息，以保护客户的敏感信息，如出生日期和信用卡信息。另一种应用是社交媒体公司（如 Meta）识别评论/帖子中的地点和个人姓名，并将其用于内容推荐。

现在我们了解了 NER 的基本概念，让我们实现它并查看结果。

## 3.1\. NER — 实现

在这个例子中，我们将使用 [spaCy](https://spacy.io) 的预训练模型进行命名实体识别（NER）。实现过程非常简单。我们将按照以下步骤进行：

1.  安装并下载所需的数据

1.  导入库

1.  加载预训练任务，包括 NER

1.  通过模型运行一个示例句子并返回结果

如果你需要安装 spaCy 并下载数据，你可以使用以下命令 ([source](https://spacy.io/usage/models))：

```py
pip3 install spacy
python -m spacy download en_core_web_sm
```

我通过命令行界面完成了上述安装。操作步骤很简单：（I）打开终端，然后（II）运行上述两行命令。如果你需要更详细的命令行教程，请随时查看以下内容：

[](/command-line-interface-cli-tutorial-how-advanced-users-interact-with-computers-28cf88f81ce?source=post_page-----c79662e52624--------------------------------) ## 命令行界面（CLI）教程 — 高级用户如何与计算机互动

### CLI 入门以提高与计算机互动的生产力

towardsdatascience.com

接下来，让我们对以下句子实施和应用命名实体识别（NER）：`Farzad wrote this Medium article in March 2023, using an Apple laptop, on a Jupyter notebook!`

```py
# Import library
import spacy

# Load English tokenizer, tagger, parser and NER
nlp = spacy.load("en_core_web_sm")

# Define example sentence
sentence = "Farzad wrote this Medium article in March 2023, using an Apple laptop, on a Jupyter notebook!"

# Apply NER
doc = nlp(sentence)

# Analyze syntax
print(f"Noun phrases:{[chunk.text for chunk in doc.noun_chunks]}\n")
print("Verbs:", [token.lemma_ for token in doc if token.pos_ == "VERB"])
print("")

# Find named entities, phrases and concepts
for entity in doc.ents:
    print(entity.text, entity.label_)
```

结果：

![](img/24a127a3d560d0fab77ace3d9ff9e0d7.png)

这非常有趣！让我们来谈谈结果。第一行识别了名词——我个人不完全同意所有这些，但仍然，这非常令人印象深刻！第二行正确地识别了“write”和“use”作为动词，第三块识别了“Medium”作为地点，“March 2023”作为日期，“Apple”作为组织（这一点很有趣，因为 apple 也可能是水果的名称，但模型根据句子的上下文识别了公司名称）和“Jupyter”作为一个人（这一点需要一些改进）。有方法进一步训练这些预训练模型，以确保 NER 在每个使用场景中更准确，但我们想要表达的重点是展示这些预训练语言模型如何以合理的准确性完成复杂任务，如 NER。

# 结论

在这篇文章中，我们简要地介绍了自然语言处理（NLP）、理解（NLU）和生成（NLG）的世界，并通过引入和实施自然语言领域中的一些常见任务，使用语言建模来理解它们的重要性。然后，我们介绍了（I）情感分析，（II）机器翻译和（III）命名实体识别（NER）的语言模型实现，并查看了这些强大的预训练语言模型在多种语言中的令人印象深刻的结果。

# 感谢阅读！

如果你觉得这篇文章对你有帮助，请[关注我在 Medium](https://medium.com/@fmnobar)并订阅以接收我最新的文章！

*(所有图片，除非另有说明，均由作者提供。)*
