# 超越温暖的拥抱：深入了解 Hugging Face

> 原文：[`towardsdatascience.com/beyond-the-warm-embrace-a-deeper-look-at-hugging-face-96b3497788e7`](https://towardsdatascience.com/beyond-the-warm-embrace-a-deeper-look-at-hugging-face-96b3497788e7)

## 微调语言模型以进行命名实体识别

[](https://medium.com/@raicik.zach?source=post_page-----96b3497788e7--------------------------------)![Zachary Raicik](https://medium.com/@raicik.zach?source=post_page-----96b3497788e7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----96b3497788e7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----96b3497788e7--------------------------------) [Zachary Raicik](https://medium.com/@raicik.zach?source=post_page-----96b3497788e7--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----96b3497788e7--------------------------------) ·阅读时间 9 分钟·2023 年 11 月 3 日

--

![](img/ea850ddee92eada7c048b2f96261e036.png)

图片由[Choong Deng Xiang](https://unsplash.com/@dengxiangs?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供

Hugging Face 是一个提供各种自然语言处理（NLP）和自然语言理解（NLU）任务工具和预训练模型的平台。在我们之前的文章[温暖的拥抱：探索 Hugging Face](https://medium.com/@raicik.zach/a-warm-embrace-exploring-hugging-face-210c7f4a9078)中，我们深入探讨了这个平台及其开源库，该库实现了许多最先进的变换器架构。本文通过提供对特定任务的 Hugging Face 工具的统一视图，增强了 Hugging Face 文档，帮助新兴的数据科学家更好地理解如何将多种 Hugging Face 能力结合起来，以微调现有的语言模型进行命名实体识别（“NER”）。

# 相关背景

在本节中，我们简要介绍了构建模型所必需的两个基础概念。提醒一下，我们在[温暖的拥抱：探索 Hugging Face](https://medium.com/@raicik.zach/a-warm-embrace-exploring-hugging-face-210c7f4a9078)中涵盖了 Hugging Face 的基础知识。

+   命名实体识别

+   模型微调

在以下各节中，假设你对模型开发及相关概念有一定了解——然而，如果有任何不清楚的地方，随时可以联系我们！

## 命名实体识别

命名实体识别（“NER”）是自然语言处理中的一个常见任务，其目的是识别和分类相关信息或实体，将其归入多个预定义（命名）类别之一。NER 模型可以针对各种实体进行训练。一些最常见的实体包括：

+   姓名

+   组织

+   日期

+   地点与位置

在下面的图片中，我手动标记了一个示例句子中的几个不同的命名实体。在机器学习和 NLP 的上下文中，NER 是通过模型自动化这一分类过程的过程。

![](img/5c1045e5fc606e6d1f3ba22cc4e0cc77.png)

图片来源：作者

NER 模型可以支持多种任务，包括但不限于信息检索、内容摘要、内容推荐和机器翻译。

## 模型微调

在最高层次上，微调模型是指根据新的数据集调整现有模型权重，通过替换模型的一些或全部层并重新训练来实现。根据你的任务和数据集，你可能只需重新训练最后一层、模型的某些层，或整个模型。

![](img/04f1b91b24dd65f050e51fabed9ee704.png)

图片来源：作者；灵感来自于 [`d2l.ai/chapter_computer-vision/fine-tuning.html`](http://d2l.ai/chapter_computer-vision/fine-tuning.html)

有人可能会问——为什么不从头开始训练自己的模型呢？

+   创建新模型通常需要大量的计算资源和时间。利用预训练模型可以让我们利用在广泛数据上训练的模型能力，而无需进行巨大的计算和时间投入。

+   由于预训练模型通常是在大型和全面的数据集上训练的，微调模型可以让你在较小的数据集上获得良好的表现，从而最小化过拟合风险并改善泛化能力等好处。

# 开发我们的 NER 模型

我们将使用的数据集称为 [Broad Twitter Corpus](https://huggingface.co/datasets/GateNLP/broad_twitter_corpus)（根据 *Creative Commons Attribution 4.0 International* 许可证可用于商业用途）。该数据集本身只是一个巨大的推文集合。这些推文都被标注了命名实体标签。更重要的是，根据 [白皮书](https://aclanthology.org/C16-1111.pdf)，这些推文是**手动**标注的。在这个数据集上构建 NER 模型不仅可以让我们在未来自动标注实体，还能支持我们在前面部分描述的一些下游使用场景。

我们将遵循的流程的大致概述如下。

1.  设置我们的环境

1.  加载 [Broad Twitter 数据集](https://huggingface.co/datasets/GateNLP/broad_twitter_corpus)

1.  加载预训练的 [BERT](https://huggingface.co/bert-base-cased) 模型

1.  重新标记 Broad Twitter 令牌

1.  微调预训练的 BERT 模型

## 环境设置

为了简便，我在 Google Colab [notebook](https://colab.research.google.com/drive/1JfuURjpI_lB12fiR2IlZYs9NdEKIGQR9?usp=sharing) 中完成了这项工作。Google Colab 提供免费的 GPU 访问，方法是转到运行时 -> 更改运行时类型，并选择 T4 GPU。此外，这段代码可以在许多不同的环境中运行——不仅仅是 Colab。

我们需要做的第一件事是安装所需的 Hugging Face 包。我从 Hugging Face 文档中复制了每个包的简要描述，文档链接在每个包名旁边。

+   [**Datasets**](https://huggingface.co/docs/datasets/index)**: “**Datasets 是一个库，用于轻松访问和共享用于音频、计算机视觉和自然语言处理（NLP）任务的数据集**”

+   [**Transformers**](https://huggingface.co/docs/transformers/index)**: “**Transformers 提供了 API 和工具，以轻松下载和训练最先进的预训练模型**”

+   [**Evaluate**](https://huggingface.co/docs/evaluate/index)**: “**一个用于轻松评估机器学习模型和数据集的库**”

这些包可以使用 PIP 安装。

```py
%%capture ##cell magic command that captures and discards all the stdout and stderr from the cell in which it's used
!pip install datasets
!pip install transformers
!pip install evaluate
```

## 加载 Broad Twitter 数据集

Hugging Face 的数据集库使得使用两行 Python 代码加载数据集变得非常简单。

```py
from datasets import load_dataset

twitter = load_dataset("GateNLP/broad_twitter_corpus")
```

当你加载一个数据集时，它会加载数据集中包含的所有相关数据拆分。打印数据集将显示可用的拆分、每个拆分的行数以及每行的特征。下面的单元格展示了 `print(twitter)` 的结果。

```py
DatasetDict({
    train: Dataset({
        features: ['id', 'tokens', 'ner_tags'],
        num_rows: 5342
    })
    validation: Dataset({
        features: ['id', 'tokens', 'ner_tags'],
        num_rows: 2002
    })
    test: Dataset({
        features: ['id', 'tokens', 'ner_tags'],
        num_rows: 2002
    })
})
```

## 加载 BERT

在加载用于标记分类的 BERT 时，我们需要指定标签的数量。该数据集已经使用了[IOB2](https://en.wikipedia.org/wiki/Inside%E2%80%93outside%E2%80%93beginning_(tagging))标签。根据 HF 数据集页面，数字标签和字符串标签之间的映射如下。

```py
labels = {
    0:'O',
    1:'B-PER',
    2:'I-PER',
    3:'B-ORG',
    4:'I-ORG',
    5:'B-LOC',
    6:'I-LOC',
  }
```

如果你对 IOB2 标签不熟悉，可能会想知道 `B-PER` 和 `I-PER` 之间有什么区别？`B-ORG` 和 `I-ORG` 之间的区别呢？`B-LOC` 和 `I-LOC` 之间呢？答案在于标记化。具体来说，一些实体可能跨越多个标记。我们使用这些实体前缀来告诉模型实体的开始位置以及它的延续范围。标签前的 `B` 前缀表示该标记是该类型块的开始，`I` 前缀表示该标记在一个块内部。`O` 标签表示该标记在任何块之外。

现在我们知道我们有 7 个标签，我们可以使用以下代码加载 BERT。

```py
from transformers import AutoModelForTokenClassification

bert_ner = AutoModelForTokenClassification.from_pretrained("bert-base-cased", num_labels=len(labels))
```

最后，我们需要告诉模型如何在 ID 和标签之间进行映射。

```py
bert_ner.config.id2label = labels
bert_ner.config.label2id = {v: k for k, v in labels.items()}
```

我们已经加载了 BERT，但我们是否需要对 BERT 进行微调以适应我们的 NER 任务？当 BERT 被 Google 推出时，它被认为是具有突破性的。我们不能直接使用它进行 NER 而无需微调吗？我们可以在下面测试它在没有任何微调的情况下对标记的分类能力。我们将使用之前的示例句子，该句子与推文（短文本）非常相似。

```py
from transformers import pipeline, AutoTokenizer
from evaluate import evaluator

tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")
pipe = pipeline('token-classification', model='bert-base-cased', tokenizer=tokenizer, device=device_id)
pipe(["Zachary Raicik works for Corvus and lives in San Diego"], aggregation_strategy="average")
```

结果可能会让你惊讶。事实证明，在没有任何微调的情况下，BERT 在我们的任务中表现得并不好。在下面的代码块中，你可以看到 BERT 将 `Zachary Raicik works for Corvus and lives in San` 标记为一个实体，而将 `Diego` 标记为另一个实体。

```py
[[{'entity_group': 'LABEL_0',
   'score': 0.66933894,
   'word': 'Zachary Raicik works for Corvus and lives in San',
   'start': 0,
   'end': 48},
  {'entity_group': 'LABEL_1',
   'score': 0.5502231,
   'word': 'Diego',
   'start': 49,
   'end': 54}]]
```

## 重新标记 Broad Twitter 标记

当我们下载了 Broad Twitter 数据集时，它附带了一组预定义的标记。然而，数据集中的标记与 BERT 标记器生成的标记可能不匹配。BERT 可能将某些标记拆分为子词标记。因此，我们需要构建一个函数，将提供的标签重新分配给 BERT 生成的子词标记。

```py
def tokenize_and_tag(row):
  tokens, ner_tags = row["tokens"], row["ner_tags"]

  sub_tokens, labels = [], []
  for token, tag in zip(tokens, ner_tags):
      token_sub_tokens = tokenizer.tokenize(token)
      sub_tokens.extend(token_sub_tokens)
      labels.extend([tag] * len(token_sub_tokens))

  sub_tokens = ['[CLS]'] + sub_tokens + ['[SEP]']
  labels = [-100] + labels + [-100]

  padding_length = tokenizer.model_max_length - len(sub_tokens)
  sub_tokens = sub_tokens + ['[PAD]'] * padding_length
  labels = labels + [-100] * padding_length

  input_ids = tokenizer.convert_tokens_to_ids(sub_tokens)
  attention_mask = [1 if token != '[PAD]' else 0 for token in sub_tokens]
  token_type_ids = [0] * tokenizer.model_max_length  

  row["bert_tokens"] = sub_tokens
  row["input_ids"] = input_ids
  row["attention_mask"] = attention_mask
  row["token_type_ids"] = token_type_ids
  row["labels"] = labels

  return row
```

Hugging Face 期望训练时包含`input_ids`、`attention_mask`、`token_type_ids`和`labels`字段。有关如何处理数据以供`Transformers`库使用的更多信息，请参见[这里](https://huggingface.co/docs/transformers/v4.34.1/en/preprocessing#natural-language-processing)。

我们可以使用这个函数重新标记我们的数据集。

```py
train_twitter = twitter['train'].map(tokenize_and_tag)
test_twitter = twitter['test'].map(tokenize_and_tag)
validation_twitter =twitter['validation'].map(tokenize_and_tag)
```

## 微调预训练 BERT 模型

目前，我们的数据集已经准备好，我们也准备开始训练了。

```py
import numpy as np #Used to mask predictions and labels
from transformers import TrainingArguments, Trainer #Training 
from evaluate import load #Used to load required performance metrics during training 
```

在微调预训练模型时，你可以选择重新训练你想要的任意多的层。由于这只是一个示例练习，我们将只微调 BERT 的最后一层。

```py
# Freeze ALL model parameters
for param in bert_ner.parameters():
    param.requires_grad = False

# Unfreeze the last 5 layers
for param in bert_ner.bert.encoder.layer[-1:].parameters():
    param.requires_grad = True
```

我们使用`TrainingArguments`来指定学习率、训练轮次等参数。

```py
training_args = TrainingArguments(
    evaluation_strategy="epoch",
    output_dir="twitter-training",
    learning_rate=5e-05, 
    num_train_epochs=5.0,
    per_device_train_batch_size=8,
    per_device_eval_batch_size=8,
)
```

损失函数旨在优化训练过程中的模型权重，而不是用于解释。因此，我们将包括额外的指标来理解模型的表现。

在命名实体识别（NER）任务中，类别不平衡是比较常见的。因此，准确率等指标可能不够合适。在这种情况下，类别不平衡并不严重（它在数据集描述中有所显示）。然而，为了完整性，我们将在训练过程中使用加权 F1 分数来评估我们的模型。加权 F1 分数在计算某一标签的 F1 分数时，会考虑该标签的真实实例数量。这意味着每个类别的贡献按其规模成比例地影响平均值。

```py
metric = evaluate.load("f1") # Use evaluate.combine if you want multiple metrics

def compute_metrics(eval_pred):
    logits, labels = eval_pred

    predictions = np.argmax(logits, axis=-1)

    valid_mask = np.array(labels) != -100 
    valid_labels = labels[valid_mask]

    valid_predictions = predictions[valid_mask]

    return metric.compute(predictions=valid_predictions, references=valid_labels,average='weighted')
```

我们已经具备了训练所需的所有组件。

```py
trainer = Trainer(
    model=bert_ner,
    args=training_args,
    train_dataset=train_twitter,
    eval_dataset=validation_twitter,
    compute_metrics=compute_metrics
)

trainer.train()
trainer.save_model("twitter-training-mdl")
```

一旦训练过程完成，你应该能看到类似这样的视图。

![](img/1d4be0d8640699f2865348c4d9a4922c.png)

作者提供的图片

在实际情况下，你可能希望增加训练轮次，并花更多时间选择适合任务的参数。例如

+   **学习率优化：** 学习率控制模型权重的更新幅度。在我们的示例中使用了`5e-05`，但可能不同的学习率更适合这个任务。

+   **权重衰减：** 这是一种正则化技术，旨在抑制大的权重。通常，它会导致模型更简单，并有助于防止过拟合。

# 将所有内容整合在一起

让我们重新审视一下之前的句子。

```py
from transformers import pipeline
from evaluate import evaluator

pipe = pipeline('token-classification', model='twitter-training-mdl', tokenizer=tokenizer, device=device_id)

pipe(["Zachary Raicik works for Corvus and lives in San Diego"], aggregation_strategy="average")
```

结果表明，我们微调后的模型相比未经微调的 BERT 要好得多。

```py
[[{'entity_group': 'PER',
   'score': 0.8900693,
   'word': 'Zachary Raicik',
   'start': 0,
   'end': 14},
  {'entity_group': 'ORG',
   'score': 0.534402,
   'word': 'Corvus',
   'start': 25,
   'end': 31},
  {'entity_group': 'LOC',
   'score': 0.7905616,
   'word': 'San Diego',
   'start': 45,
   'end': 54}]]
```

在这篇文章中，我们介绍了如何为特定任务微调现有模型的基本知识。在我们的案例中，我们使用了 BERT 来构建一个命名实体识别模型。这个过程可以应用于任何数量的任务，使用不同的数据集或模型。虽然这篇文章为 Hugging Face 能做的事情提供了强有力的介绍，但我们仅仅触及了表面。例如，我们甚至没有将我们的模型传递给 Hugging Face 维护的强大评估库。在未来的文章中，我们将介绍如何使用这些附加工具来微调 NER 模型以及其他用例。
