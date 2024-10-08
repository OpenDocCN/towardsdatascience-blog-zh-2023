# Hugging Face 简介及 6 种 NLP 任务实现

> 原文：[`towardsdatascience.com/implement-nlp-tasks-using-hugging-face-77dfdcad65fd`](https://towardsdatascience.com/implement-nlp-tasks-using-hugging-face-77dfdcad65fd)

## Hugging Face 用于 NLP 任务的入门教程

[](https://medium.com/@fmnobar?source=post_page-----77dfdcad65fd--------------------------------)![Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----77dfdcad65fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----77dfdcad65fd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----77dfdcad65fd--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----77dfdcad65fd--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----77dfdcad65fd--------------------------------) ·阅读时间 12 分钟·2023 年 4 月 18 日

--

![](img/8a227cd780125742369e775c5e6ccf84.png)

照片由 [Duy Pham](https://unsplash.com/@miinyuii?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/Cecb0_8Hx-o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[Hugging Face](https://huggingface.co/) 是一个开源 AI 社区，由机器学习从业者创建，专注于自然语言处理（NLP）、计算机视觉以及音频/语音处理任务。无论你是已经在这些领域工作还是希望未来进入这些领域，你都会从学习如何使用 Hugging Face 的工具和模型中受益。

在这篇文章中，我们将介绍利用 Hugging Face 上的预训练模型完成的六种最常用的 NLP 任务，如下所示：

1.  文本生成（即语言建模）

1.  问答系统

1.  情感分析

1.  文本分类

1.  文本摘要

1.  机器翻译

在开始任务之前，让我们花一分钟时间讨论“训练”和“推断”这两个机器学习中的重要概念，以澄清我们今天将要做的工作。

让我们开始吧！

[](https://medium.com/@fmnobar/membership?source=post_page-----77dfdcad65fd--------------------------------) [## 使用我的推荐链接加入 Medium

### 阅读 Farzad 的每一个故事（以及 Medium 上其他作者的故事）。你的会员费直接支持 Farzad 和其他…

medium.com](https://medium.com/@fmnobar/membership?source=post_page-----77dfdcad65fd--------------------------------)

# 机器学习中的训练与推断

训练是将大量数据输入机器学习模型的过程。在这个过程中，模型通过优化目标函数从提供的数据中“学习”，因此这个过程被称为“训练”。一旦我们有了训练好的模型，我们可以使用它来对模型之前未见过的新数据进行预测。这个过程被称为“推理”。简而言之，训练是模型的学习过程，而推理是模型进行预测的过程（即我们实际使用模型时）。

现在我们理解了训练和推理之间的区别后，我们可以更具体地定义我们今天要做的工作。在这篇文章中，我们将使用各种预训练模型进行推理。换句话说，我们不会在这里经历训练任何新模型的昂贵过程。另一方面，我们将利用 Hugging Face Hub 中众多现有的预训练模型，并使用这些模型进行推理（即进行预测）。

# 1\. 文本生成（又称语言建模）

我决定从这个任务开始，考虑到最近对生成性人工智能（如 ChatGPT）的兴趣激增。这个任务通常被称为语言建模，模型执行的任务是预测文本中缺失的部分（这可以是一个词、token 或更大的文本字符串）。最近引起了很多兴趣的是，模型可以生成文本，而不一定需要之前见过这样的提示。

让我们看看它在实践中的效果！

## 1.1\. 文本生成 — 实现

为了实现文本生成，我们将从 `transformers` 库中导入 `pipeline`，使用其中一个 GPT 模型，并按照以下步骤进行。我还在代码中添加了注释，以便您更容易地跟随这些步骤：

1.  导入库

1.  指定要用于此特定任务的预训练模型的名称

1.  指定将由模型完成的句子

1.  创建一个 `pipeline` 的实例作为 `generator`

1.  执行文本生成并将结果存储为`output`

1.  返回结果

下面的代码块按照这些步骤进行。

```py
# Import libraries
from transformers import pipeline

# Specify the model
model = "gpt2"

# Specify the task
task = "text-generation"

# Instantiate pipeline
generator = pipeline(model = model, task = task, max_new_tokens = 30)

# Specify input text
input_text = "If you are interested in learing more about data science, I can teach you how to"

# Perform text generation and store the results
output = generator(input_text)

# Return the results
output
```

结果：

![](img/5de8984992318dd87aacdbbaf11cd913.png)

文本生成结果

我们可以从结果中看到，模型根据我们提供的输入文本生成了额外的文本，考虑到它所训练的数据和我们提供的句子。请注意，我将输出的长度限制为 `max_new_tokens` 的 30 个 token，以防生成过长的响应。生成的文本听起来合理且与上下文相关。

但如果我们想向模型提出一个问题呢？模型能否回答问题，而不仅仅是完成一个不完整的句子？让我们接下来探索一下。

# 2\. 问答

问答，如其名所示，是一个模型回答用户提出的问题的任务。通常有两种类型的问答任务：

1.  **抽象式（即上下文相关）：** 用户在问题/提示中向模型描述一个情况，并要求模型根据提供的信息生成回应。在这种情况下，模型从提示中挑选相关的信息部分，并返回结果。

1.  **抽取式（即上下文无关）：** 用户向模型提问，而不提供任何上下文。

让我们看看如何实现问答系统。

## 2.1\. 问答系统 — 实现

实现过程与语言建模任务类似。我们将使用两个不同的模型来比较结果。

我们从`distilbert-base-cased-distilled-squad`开始 ([link](https://huggingface.co/distilbert-base-cased-distilled-squad))。

```py
# Specify model
model = 'distilbert-base-cased-distilled-squad'

# Instantiate pipeline
answerer = pipeline(model = model, task="question-answering")

# Specify question and context
question = "What does NLP stand for?"
context = "Today we are talking about machine learning and specifically the natural language processing, which enables computers to understand, process and generate languages"

# Generate predictions
preds = answerer(
    question = question,
    context = context,
)

# Return results
print(
    f"score: {round(preds['score'], 4)}, start: {preds['start']}, end: {preds['end']}, answer: {preds['answer']}"
)
```

结果：

![](img/df755d3f89fe5dac99a188cf302b4bcb.png)

问答结果 (distilbert-base-cased-distilled-squad `Model`)

我们可以看到总结，模型能够确定哪个部分的上下文与回答问题相关。

让我们使用另一个模型`deepset/roberta-base-squad2` ([link](https://huggingface.co/deepset/roberta-base-squad2))来实现相同的问题。

```py
# Specify model
model = "deepset/roberta-base-squad2"

# Specify task
task = "question-answering"

# Instantiate pipeline
answerer = pipeline(task = task, model = model, tokenizer = model)

# Specify input
qa_input = {
    'question': 'What does NLP stand for?',
    'context': 'Today we are talking about machine learning and specifically the natural language processing, which enables computers to understand, process and generate languages'
}

# Generate predictions
output = answerer(qa_input)

# Return results
output
```

结果：

![](img/0e158182b62c0f846aae0b86dd9e56a8.png)

问答结果 (`deepset/roberta-base-squad2 Model`)

正如我们在上面的例子中看到的，第二个模型也能够识别出 NLP 代表自然语言处理，鉴于我们提供的上下文。

让我们继续我们的 NLP 任务之旅，接下来看看情感分析。

# 3\. 情感分析

情感分析是将文本的情感分类为积极、消极或中性的过程。情感分析在不同的行业中有广泛的应用，比如通过产品评论监控客户情绪，甚至在政治中，比如在选举年评估公众对某一主题的兴趣。本文的重点是使用 Hugging Face 完成各种任务，因此我们不会深入探讨每个主题，但如果您有兴趣深入了解情感分析，可以参考这篇文章：

[](/sentiment-analysis-intro-and-implementation-ddf648f79327?source=post_page-----77dfdcad65fd--------------------------------) ## 情感分析 — 介绍与实现

### 使用 NLTK、scikit-learn 和 TextBlob 进行情感分析

towardsdatascience.com

## 3.1\. 情感分析 — 实现

为了实现情感分析，我们将再次依赖`transformers`库中的`pipeline`，并按照以下步骤操作。我还在代码中添加了注释，以便您更容易跟随步骤。

1.  导入库

1.  指定用于特定任务（例如情感分析）的预训练模型名称。

1.  指定任务（即情感分析）

1.  指定将进行情感分析的句子

1.  创建一个`pipeline`实例作为`analyzer`

1.  执行情感分析并将结果保存为`output`

1.  返回结果

```py
# Specify pre-trained model to use
model = 'distilbert-base-uncased-finetuned-sst-2-english'

# Specify task
task = 'sentiment-analysis'

# Text to be analyzed
input_text = 'Performing NLP tasks using HuggingFace pipeline is super easy!'

# Instantiate pipeline
analyzer = pipeline(task, model = model)

# Store the output of the analysis
output = analyzer(input_text)

# Return output
output
```

结果：

![](img/0e5bf1b80072f1bb437635989b5c8451.png)

情感分析结果

结果表明，句子的情感是积极的，得分约为 85%。这句句子听起来相当积极，所以到目前为止我喜欢这些结果。可以自由地重复这一过程，测试其他句子！

让我们转向另一种文本分类类型。

# 4\. 文本分类

情感分析，正如我们刚刚讨论的，可以被视为文本分类的一个特殊案例，其中类别（或类）仅为正面、负面或中性。文本分类则更为通用，它可以将传入的文本（例如句子、段落或文档）分类到预定义的类别中。让我们在实践中深入了解一下这意味着什么。

## 4.1\. 文本分类 — 实现

我们将使用相同的`pipeline`包，并采取与情感分析非常相似的步骤，如下所示：

```py
# Specify model
model = 'facebook/bart-large-mnli'

# Specify Task
task = 'zero-shot-classification'

# Specify input text
input_text = 'This is a tutorial about using pre-trained models through HuggingFace'

# Identify the classes/categories/labels
labels = ['business', 'sports', 'education', 'politics', 'music']

# Instantiate pipeline
classifier = pipeline(task, model = model)

# Store the output of the analysis
output = classifier(input_text, candidate_labels = labels)

# Return output
output
```

结果：

![](img/bd38a8780d752b47257237e4dea5e5c7.png)

文本分类结果

结果相当有趣！分数对应于每个标签，按从大到小排序以便阅读。例如，结果表明我们的句子被标记为“教育”，得分约为 40%，其次是“商业”，得分约为 22%，而“音乐”、“体育”和“政治”标签的分数都很低，这总体上对我来说是合理的。

让我们继续进行下一个任务，即摘要。

# 5\. 文本摘要

文本摘要的任务是自动总结文本输入，同时仍然传达传入文本的主要点或要旨。需要这种摘要模型的商业直觉示例是那些人类阅读传入的文本通信（例如客户邮件）的情况，使用摘要模型可以节省人力时间。例如，这些人工代表可以阅读客户邮件的摘要，而不是全部邮件，从而通过节省人力时间和成本提高操作效率。

让我们看看如何实现文本摘要。

## 5.1\. 文本摘要 — 实现

与其他任务类似，我们将使用`pipeline`进行摘要任务。对于这个具体任务，我们将首先使用来自 Google 的名为[T5](https://github.com/google-research/text-to-text-transfer-transformer)的文本到文本预训练模型，来总结我们刚刚阅读的关于“文本摘要”的描述。然后，我们将使用 Google 的不同模型重复相同的练习，看看结果的变化。让我们看看如何实现这一点。

```py
# Specify model and tokenizer
model = "t5-base"
tokenizer = "t5-base"

# Specify task
task = "summarization"

# Specify input text
input_text = "Text summarization is the task of automatically summarizing textual input, while still conveying the main points and gist of the incoming text. One example of the business intuition behind the need for such summarization models is the situations where humans read incoming text communications (e.g. customer emails) and using a summarization model can save human time. "

# Instantiate pipeline
summarizer = pipeline(task = task, model = model, tokenizer = tokenizer, framework = "tf")

# Summarize and store results
output = summarizer(input_text)

# Return output
output
```

结果：

![](img/732cdbd9d24b13790eec08fb126dbb1c.png)

文本摘要结果（T5 模型）

正如你在结果中看到的，T5 模型接受了相对较长的输入文本，并返回了一个简洁的摘要，概述了它认为输入文本的主要点。我喜欢这个摘要，因为它解释了什么是文本摘要以及它能提供什么好处——这是一个很好的摘要！

让我们尝试另一个来自 Google 的模型，[Pegasus](https://huggingface.co/google/pegasus-cnn_dailymail)，看看使用不同模型时结果如何变化。

```py
# Specify model
model = 'google/pegasus-cnn_dailymail'

# Specify task
task = 'summarization'

# Specify inpute text
input_text = "Text summarization is the task of automatically summarizing textual input, while still conveying the main points and gist of the incoming text. One example of the business intuition behind the need for such summarization models is the situations where humans read incoming text communications (e.g. customer emails) and using a summarization model can save human time. "

# Instantiate pipeline
summarizer = pipeline(task = task, model = model)

# Summarize and store results
output = summarizer(input_text, max_length = 75, min_length = 25)

# Return output
output
```

结果：

![](img/8aebfbfd0c88e203b79cc25583fce843.png)

文本摘要结果（Pegasus 模型）

正如预期的那样，两种模型的结果有所不同，因为它们各自使用了特定的数据和训练目标，但都在某种程度上完成了任务。我个人更喜欢 T5 模型的结果，因为它更简洁地陈述了输入文本的要点。

最后，我们将要讨论的任务是机器翻译。

# 6\. 机器翻译

机器翻译是生成输入文本在目标语言中翻译的任务。这类似于 Google Translate 或其他类似翻译引擎提供的服务。使用 Hugging Face 进行机器翻译的好处之一是我们可以选择用于翻译的模型，这可能为我们寻找的特定语言提供更准确的翻译。

让我们来看看 Hugging Face 中机器翻译的实现。

## 6.1\. 机器翻译 — 实现

为了生成翻译，我们将使用两个最常见的预训练模型将同一句话从英语翻译成法语。每个模型的实现略有不同，但整体过程与我们到目前为止实现的其他任务相同。

## 6.1.1\. T5

[T5](https://huggingface.co/docs/transformers/model_doc/t5)是 Google 开发的一个编码器-解码器预训练模型，在包括机器翻译在内的多个任务上表现良好。为了提示 T5 执行诸如从语言 X 到语言 Y 的翻译等任务，我们会在每个任务的输入句子前添加一个字符串（称为“前缀”）：`"translate X to Y: sentence_to_be_translated"`。

实际上，这在实践中更容易理解，所以我们直接使用 T5 将一个句子从英语翻译成法语，看看效果如何。

```py
# Specify prefix
original_language = 'English'
target_language = 'French'
prefix = f"translate {original_language} to {target_language}: "

# Specify input text
input_text = f"{prefix}This is a post on Medium about various NLP tasks using Hugging Face."

# Specify model
model = "t5-base"

# Specify task
task = "translation"

# Instantiate pipeline
translator = pipeline(task = task, model = model)

# Perform translation and store the output
output = translator(input_text)

# Return output
output
```

结果：

![](img/ff09beef413b5d75dcf118b24cfc6259.png)

机器翻译结果（T5 模型）

我在 Google Translate 上查找了这个翻译，看起来这是一个不错的翻译！我对这种验证方法有些担忧，因为 T5 也是由 Google 开发的。我不知道 Google Translate 使用的是哪个具体模型，但我们将在下一节中使用 mBART 运行相同的翻译任务，看看结果有多大差异。

## 6.1.2\. mBART

[mBART](https://huggingface.co/docs/transformers/model_doc/mbart)是由 Meta 开发的多语言编码器-解码器预训练模型，主要用于机器翻译任务。mBART 与 T5 不同，它不需要提示中的前缀，但我们需要向模型提供原始语言和目标语言的识别。

让我们在 mBART 中实现相同的任务。

```py
# Import packages
from transformers import MBartForConditionalGeneration, MBart50TokenizerFast

# Specify input text
input_text = "This is a post on Medium about various NLP tasks using Hugging Face."

# Specify model
model_name = "facebook/mbart-large-50-many-to-many-mmt"

# Instantiate model and tokenizer
model = MBartForConditionalGeneration.from_pretrained(model_name)
tokenizer = MBart50TokenizerFast.from_pretrained(model_name)

# Specify source language
tokenizer.src_lang = "en_XX"

# Encode input text
encoded_en = tokenizer(input_text, return_tensors="pt")

# Perform translation to the target language
generated_tokens = model.generate(**encoded_en, forced_bos_token_id=tokenizer.lang_code_to_id["fr_XX"])

# Decode the translation and store the output
output = tokenizer.batch_decode(generated_tokens, skip_special_tokens=True)

# Return output
output
```

![](img/01ceddbf75c2b303d82c706352e78920.png)

机器翻译结果（mBART 模型）

结果与 T5 生成的结果非常相似，唯一的区别是“poste”被替换为“post”。尽管两者之间存在差异，但这次练习的主要目的是展示这些预训练模型如何生成机器翻译，我们已经使用这两个模型完成了这一目标。

# 结论

在这篇文章中，我们介绍了 Hugging Face，这是一个开源 AI 社区，被许多从事自然语言处理、计算机视觉和音频/语音处理任务的机器学习从业者使用。然后，我们演示了如何在 Hugging Face 平台上实现这些预训练模型，以完成下游 NLP 任务，如文本生成、问答、情感分析、文本分类、文本摘要和机器翻译。

# 感谢阅读！

如果你觉得这篇文章对你有帮助，请[关注我在 Medium](https://medium.com/@fmnobar)并订阅以接收我最新的帖子！

*(除非另有说明，所有图片均由作者提供。)*
