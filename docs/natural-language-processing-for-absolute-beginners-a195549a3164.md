# 自然语言处理初学者指南

> 原文：[`towardsdatascience.com/natural-language-processing-for-absolute-beginners-a195549a3164`](https://towardsdatascience.com/natural-language-processing-for-absolute-beginners-a195549a3164)

## 用 10 行 Python 代码解决复杂的 NLP 任务

[](https://dmitryelj.medium.com/?source=post_page-----a195549a3164--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----a195549a3164--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a195549a3164--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a195549a3164--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----a195549a3164--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a195549a3164--------------------------------) ·阅读时间 9 分钟·2023 年 9 月 2 日

--

![](img/bc90b1586db7d9db3689cfb34615b58a.png)

作者提供的图片（使用 Craiyon 生成）

NLP（自然语言处理）通常被认为是计算机科学中的复杂领域。像 SpaCy 或 NLTK 这样的框架体积庞大，通常需要一些学习。但借助开源的大型语言模型（LLMs）和现代 Python 库，许多任务可以更容易地解决。而且，甚至在几年前只有科学论文中才能找到的结果，现在也可以通过仅仅 10 行 Python 代码实现。

话不多说，我们开始吧。

## 1. 语言翻译

你是否曾想过 Google 翻译是如何工作的？Google [使用](https://blog.research.google/2020/06/recent-advances-in-google-translate.html)一个在大量文本上训练的深度学习模型。现在，借助 [Transformers 库](https://huggingface.co/docs/transformers/index)，不仅在 Google 实验室，还能在普通 PC 上完成这项工作。在此示例中，我将使用一个预训练的 [T5-base](https://huggingface.co/t5-base)（文本到文本转换器）模型。该模型最初在原始文本数据上训练，然后在诸如（“将英语翻译成德语：这所房子很棒”，“Das Haus ist Wunderbar”）的源-目标对上微调。在这里，“将英语翻译成德语”是一个前缀，告诉模型该做什么，而短语是模型应该学习的实际上下文。

> **重要警告**。大型语言模型的体积确实很大。此示例中使用的 *T5ForConditionalGeneration* 类将自动下载约 900 MB 大小的“t5-base”模型。在运行代码之前，请确保有足够的磁盘空间，并且流量没有限制。

可以在 Python 中使用预训练的 T5 模型：

```py
from transformers import T5Tokenizer, T5ForConditionalGeneration

preprocessed_text = "translate English to German: the weather is good"
tokenizer = T5Tokenizer.from_pretrained('t5-base',
                                        max_length=64,
                                        model_max_length=512,
                                        legacy=False)
tokens = tokenizer.encode(preprocessed_text,
                          return_tensors="pt",
                          max_length=512,
                          truncation=True)

model = T5ForConditionalGeneration.from_pretrained('t5-base')
outputs = model.generate(tokens, min_length=4, max_length=32)

print("Result:", tokenizer.decode(outputs[0], skip_special_tokens=True))

#> Result: Das Wetter ist gut.
```

在这里，一个 *T5Tokenizer* 类将源字符串转换为数字形式；这个过程叫做 *分词*。在我们的例子中，一个“*将英语翻译成德语：天气很好*”的文本将被转换为 *[13959, 1566, 12, 2968, 10, 8, 1969, 19, 207, 1]* 数组。“*生成*”方法实际执行任务，最后，分词器进行反向转换。作为输出，我们将得到结果“Das Wetter ist gut”。

我们能否使这段代码更简洁？实际上，我们可以。借助 [Transformer 的 *pipeline*](https://huggingface.co/docs/transformers/main_classes/pipelines) 类，我们可以创建一个抽象管道，只需 2 行 Python 代码即可完成这个任务：

```py
from transformers import pipeline

translator = pipeline("translation_en_to_de", model="t5-base")
print(translator("the weather is good"))

#> [{'translation_text': 'Das Wetter ist gut.'}] 
```

出于自学的目的，我通常更喜欢第一种方法，因为它更容易理解“幕后”的情况。但对于“生产”目的，第二种方法更具灵活性，它还允许在不更改代码的情况下使用不同的模型。

## 2\. 摘要生成

文本摘要的目标是将文档转换为简短版本，这显然如果手动完成会花费时间。令人惊讶的是，T5 模型也可以做到这一点；我们唯一需要更改的是前缀：

```py
body = '''Obviously, the lunar surface is covered with craters, left 
from previous collisions of meteorites with the Moon. Where does math go? 
While a meteorite collision is a random event, its frequency 
obeys probability theory laws. There is no atmosphere on the Moon's 
surface, no erosion, and no wind. Therefore the lunar surface is an 
ideal "book" in which the events of the last tens of thousands of 
years are recorded. By studying the Moon, we can calculate how often 
such objects fall on its surface.

A study of the lunar surface with high-resolution cameras is ongoing. 
It has been estimated that at least 220 new craters have formed on the 
Moon over the past 7 years. This check is also vital because 
these calculations can help assess the danger to the Earth.'''

preprocessed_text = f"summarize: {body}"

tokenizer = T5Tokenizer.from_pretrained('t5-base', 
                                        max_length=256,
                                        model_max_length=512,
                                        legacy=False)
tokens = tokenizer.encode(preprocessed_text,
                          return_tensors="pt",
                          max_length=256,
                          truncation=True)

model = T5ForConditionalGeneration.from_pretrained('t5-base')
outputs = model.generate(tokens,
                         min_length=4,
                         max_length=64)

print("Result:", tokenizer.decode(outputs[0], skip_special_tokens=True))

#> the lunar surface is an ideal "book" in which the events of the last 
#> tens of thousands of years are recorded. by studying the Moon, we can 
#> calculate how often such objects are falling on its surface.
```

正如我们所见，结果非常准确。

与第一个例子一样，使用管道可以为相同的任务生成更简短的代码：

```py
summarizer = pipeline("summarization", model="t5-base", tokenizer="t5-base")
summarizer(body, min_length=4, max_length=64)

#> [{'summary_text': 'the lunar surface is an ideal "book" in which the 
#>  events of the last tens of thousands of years are recorded . it has been
#>  estimated that at least 220 new craters have formed on the Moon over the 
#>  past 7 years .'}]
```

读者可能对使用 “t5-base” 模型可以完成的其他任务感兴趣。我们可以轻松地将它们全部打印出来：

```py
for prefix in model.config.task_specific_params:
    print(f"{prefix}: {model.config.task_specific_params[prefix]}")

#> summarization: 
#>   {'early_stopping': True, 'length_penalty': 2.0, 'max_length': 200, 'min_length': 30, 'no_repeat_ngram_size': 3, 'num_beams': 4, 'prefix': 'summarize: '}
#> translation_en_to_de: 
#>   {'early_stopping': True, 'max_length': 300, 'num_beams': 4, 'prefix': 'translate English to German: '}
#> translation_en_to_fr: 
#>   {'early_stopping': True, 'max_length': 300, 'num_beams': 4, 'prefix': 'translate English to French: '}
#> translation_en_to_ro: 
#>   {'early_stopping': True, 'max_length': 300, 'num_beams': 4, 'prefix': 'translate English to Romanian: '}
```

## 3\. 问答

大型语言模型还可以提供另一种有趣的功能，即在给定的上下文中回答问题。我将使用与之前示例相同的文本：

```py
body = '''Obviously, the lunar surface is covered with craters, left 
from previous collisions of meteorites with the Moon. Where does math go? 
While a meteorite collision is a random event, its frequency 
obeys probability theory laws. There is no atmosphere on the Moon's 
surface, no erosion, and no wind. Therefore the lunar surface is an 
ideal "book" in which the events of the last tens of thousands of 
years are recorded. By studying the Moon, we can calculate how often 
such objects fall on its surface.

A study of the lunar surface with high-resolution cameras is ongoing. 
It has been estimated that at least 220 new craters have formed on the 
Moon over the past 7 years. This check is also vital because 
these calculations can help assess the danger to the Earth.'''

question_answerer = pipeline("question-answering",
                             model='distilbert-base-cased-distilled-squad')

result = question_answerer(question="Which surface has collision with meteorites",
                           context=body)
print(f"Answer: '{result['answer']}', score: {round(result['score'], 4)}, start: {result['start']}, end: {result['end']}")

#> Answer: 'the Moon', score: 0.4401, start: 93, end: 101

result = question_answerer(question="How many craters were formed",
                           context=body)
print(f"Answer: '{result['answer']}', score: {round(result['score'], 4)}, start: {result['start']}, end: {result['end']}")

#> Answer: 'at least 220', score: 0.5302, start: 600, end: 612

result = question_answerer(question="Is there atmosphere on the moon",
                           context=body)
print(f"Answer: '{result['answer']}', score: {round(result['score'], 4)}, start: {result['start']}, end: {result['end']}")

#> Answer: 'There is no', score: 0.2468, start: 220, end: 231
```

在这种情况下，我使用了一个 [distilbert-base-cased-distilled-squad](https://huggingface.co/distilbert-base-cased-distilled-squad) 模型，模型大小为 261 MB。正如我们所见，该模型不仅提供了答案，还从原始文本中检索了位置，这对验证结果很有帮助。

## 4\. 语言生成

另一个有趣的过程是语言生成。对于这个例子，我将使用一个 [GPT-2 模型](https://huggingface.co/gpt2)。这显然不是我们今天拥有的最新 GPT 模型，但 GPT-2 是免费提供的，并且其文件大小为 548 MB，足够在普通 PC 上运行。

让我们看看它是如何工作的：

```py
from transformers import GPT2Config, GPT2Model,
                         TFGPT2LMHeadModel, GPT2Tokenizer

tokenizer = GPT2Tokenizer.from_pretrained("gpt2")
input_ids = tokenizer.encode("I am going to say",
                             return_tensors='tf')
model = TFGPT2LMHeadModel.from_pretrained("gpt2",
                                  pad_token_id=tokenizer.eos_token_id)
output = model.generate(input_ids,
                        max_length=128,
                        early_stopping=True,
                        do_sample=True,
                        top_k=20)
print("Output:", tokenizer.decode(output[0], skip_special_tokens=True))

#> Output: I am going to say something. It is very hard for people to 
#> believe me because I do not have to speak English. But the fact that 
#> you are writing a book on it is one thing. You have to have this 
#> knowledge which is not from what you read; it is from an educated...
```

同样，可以使用更简短的代码通过管道完成：

```py
from transformers import pipeline

generator = pipeline('text-generation', model='gpt2')
generator("I am going to say", max_length=128, num_return_sequences=1)

#> I am going to say this: I have had several bad experiences with the 
#> internet in the years since my first connection. While there ...
```

实际上，我在这些文本中看不到太多意义，但从语法角度来看，它们足够好，对于某种自动化或单元测试，它们可能是有用的。有趣的是，GPT-2 模型于 2019 年发布。为了好玩，我向 GPT-3.5（发布于 2022 年）提出了相同的问题，“请继续我将要说的短语”，并得到了以下答案：

```py
Am going to say that communication is a vital skill in today's interconnected
world. Whether it's expressing your thoughts and ideas, building relationships,
or resolving conflicts, effective communication plays a central role in almost
every aspect of our lives. It's not just about the words we use but also our
tone, body language, and the context in which we communicate.
```

这个结果要好得多；这三年取得了巨大的进步。但显然，即使 GPT-3.5 模型在今天发布于公共领域，也不可能在普通电脑上运行。

## 5\. 情感分析

之前的示例主要是为了娱乐，但情感分析对于商业来说更为重要。情感分析是分析给定文本中的情感并找出主观意见的过程。对于网络商店、流媒体平台和其他许多用户可以发布评论的服务，这可能特别重要。

对于这个测试，我将使用一个[distilbert-base-uncased-finetuned-sst-2-english](https://huggingface.co/distilbert-base-uncased-finetuned-sst-2-english)模型：

```py
from transformers import pipeline

sentiment_pipeline = pipeline("sentiment-analysis",
                         model='distilbert-base-uncased-finetuned-sst-2-english')
data = ["It was not bad",
        "I expected to love it but I was wrong"]
sentiment_pipeline(data)

#> [{'label': 'POSITIVE', 'score': 0.9995607733726501},
#>  {'label': 'NEGATIVE', 'score': 0.997614860534668}]
```

我故意尝试使用不易分析的短语，但模型给出了正确的答案。显然，自然语言是非常灵活的，仍然可能出现导致错误结果的文本。例如，这个模型对短语“我期待它很糟糕，但我错了”给出了错误的回答。同时，（体积大得多的）GPT-3.5 模型能够正确解析。另一方面，考虑到 DistilBERT 模型的大小仅为 268 MB 且可以免费使用（模型拥有 Apache 2.0 许可证），结果还是相当不错的。读者也可以尝试[其他开源模型](https://huggingface.co/models?pipeline_tag=text-classification)，并选择最适合他们需求的模型。

## 6\. 命名实体识别（NER）

自然语言处理的另一个有趣部分是“命名实体识别”。这是从非结构化文本中提取实体，如名称、地点、日期等的过程。对于这个测试，我将使用一个[bert-base-NER 模型](https://huggingface.co/dslim/bert-base-NER)（文件大小为 433 MB）。

让我们考虑一个例子：

```py
from transformers import pipeline

body = "Hi, my name is Dmitrii. I am in London, I work in Super Company, " \
       "I have a question about the hotel reservation."

ner = pipeline("ner",
               model="dslim/bert-base-NER",
               aggregation_strategy='average')
ner(body)

#> [{'entity_group': 'PER',
#>  'score': 0.7563127,
#>  'word': 'Dmitrii',
#>  'start': 15,
#>  'end': 22},
#> {'entity_group': 'LOC',
#>  'score': 0.99956125,
#>  'word': 'London',
#>  'start': 32,
#>  'end': 38},
#> {'entity_group': 'ORG',
#>  'score': 0.99759734,
#>  'word': 'Super Company',
#>  'start': 50,
#>  'end': 63}]
```

正如我们所见，该模型能够正确确定文本中提到的主要实体，如名称、地点和公司。

## 7\. 关键词提取

在最后一个例子中，我们测试了 NER，但不是所有的参数都从文本中提取出来了。一个单独的关键词提取算法对于同一任务也可能很有用。对于这个例子，我将使用[KeyBERT](https://maartengr.github.io/KeyBERT/index.html)：

```py
from keybert import KeyBERT

body = "Hi, my name is Dmitrii. I am in London, I work in Super Company, " \
       "I have a question about the hotel reservation."

kw_model = KeyBERT()
keywords = kw_model.extract_keywords(body,
                                     keyphrase_ngram_range=(1, 1),
                                     diversity=0.8,
                                     stop_words=None)
print(keywords)

#> [('reservation', 0.5935), ('hotel', 0.5729), ('london', 0.2705), 
#>  ('dmitrii', 0.2), ('company', 0.1817)]
```

正如我们所见，关键词提取作为 NER 的补充也很有用，它可以从相同的短语中提取一些额外的数据。

## 结论

在这篇文章中，我测试了不同的自然语言处理（NLP）算法。正如我在文章开头承诺的那样，通过现代库的帮助，复杂的任务可以在 10 行 Python 代码中解决。还要提到的是，这些代码都可以在本地运行，无需任何 API 订阅或密钥。最后但同样重要的是，我希望读者能看到 NLP 也可以很有趣。

感谢阅读。如果你喜欢这个故事，可以随时[订阅](https://medium.com/@dmitryelj/membership)Medium，你将会在我的新文章发布时收到通知，并且可以全面访问其他作者的成千上万篇故事。
