# 实用介绍 Transformer 模型：BERT

> 原文：[https://towardsdatascience.com/practical-introduction-to-transformer-models-bert-4715ed0deede?source=collection_archive---------6-----------------------#2023-07-17](https://towardsdatascience.com/practical-introduction-to-transformer-models-bert-4715ed0deede?source=collection_archive---------6-----------------------#2023-07-17)

![](../Images/15139494860ad96a134be1b20fb42fa3.png)

图片来源于 [Alex Padurariu](https://unsplash.com/@alexpadurariu?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## [动手教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 动手教程：如何使用 BERT 构建你的第一个情感分析模型

[](https://medium.com/@shashank.kapadia?source=post_page-----4715ed0deede--------------------------------)[![Shashank Kapadia](../Images/347e4cb92a7d27f032c5761e4526f2fa.png)](https://medium.com/@shashank.kapadia?source=post_page-----4715ed0deede--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4715ed0deede--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4715ed0deede--------------------------------) [Shashank Kapadia](https://medium.com/@shashank.kapadia?source=post_page-----4715ed0deede--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcc7314ace45c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-introduction-to-transformer-models-bert-4715ed0deede&user=Shashank+Kapadia&userId=cc7314ace45c&source=post_page-cc7314ace45c----4715ed0deede---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4715ed0deede--------------------------------) ·7分钟阅读·2023年7月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4715ed0deede&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-introduction-to-transformer-models-bert-4715ed0deede&user=Shashank+Kapadia&userId=cc7314ace45c&source=-----4715ed0deede---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4715ed0deede&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-introduction-to-transformer-models-bert-4715ed0deede&source=-----4715ed0deede---------------------bookmark_footer-----------)

*前言：本文总结了关于给定主题的信息。它不应被视为原创研究。本文中包含的信息和代码可能受到我过去从各种在线文章、研究论文、书籍和开源代码中阅读或看到的内容的影响。*

# 目录

+   BERT 简介

+   预训练和微调

+   实践操作：使用 BERT 进行情感分析

+   解释结果

+   结束语

在 NLP 中，变换器模型架构是一项革命性的进展，大大增强了理解和生成文本信息的能力。

在本教程中，我们将深入探讨 BERT，这是一种著名的基于变换器的模型，并提供一个实际的示例来微调基础 BERT 模型以进行情感分析。

# BERT 介绍

BERT 由 Google 研究人员于 2018 年推出，是一种强大的语言模型，使用变换器架构。与早期的 LSTM 和 GRU 等单向或顺序双向模型架构相比，BERT 同时考虑过去和未来的上下文。这得益于创新的“注意力机制”，它使模型在生成表示时能够权衡句子中单词的重要性。

BERT 模型在以下两个 NLP 任务上进行了预训练：

+   掩码语言模型（MLM）

+   下一句预测（NSP）

并且通常被用作各种下游 NLP 任务的基础模型，例如我们在本教程中将涵盖的情感分析。

# 预训练和微调

BERT 的强大之处在于它的两步过程：

+   **预训练**是 BERT 在大量数据上进行训练的阶段。因此，它学会了预测句子中的掩码词（MLM 任务）和预测一个句子是否跟随另一个句子（NSP 任务）。这一阶段的输出是一个具有通用“语言理解”能力的预训练 NLP 模型。

+   **微调**是在特定任务上进一步训练预训练的 BERT 模型。模型以预训练的参数进行初始化，并在下游任务上对整个模型进行训练，使 BERT 能够根据当前任务的具体情况微调其语言理解能力。

# 实践操作：使用 BERT 进行情感分析

完整代码可以作为 [GitHub 上的 Jupyter Notebook](https://github.com/kapadias/medium-articles/blob/master/natural-language-processing/transformers-series/sentiment_analysis_bert.ipynb)获得

在这个动手练习中，我们将使用 IMDB 电影评论数据集[4] [(许可证：Apache 2.0)](https://github.com/huggingface/datasets/blob/main/LICENSE)来训练情感分析模型，该数据集标记了评论是积极还是消极。我们还将使用 Hugging Face 的 transformers 库加载模型。

让我们加载所有库

```py
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.metrics import confusion_matrix, roc_curve, auc
from datasets import load_dataset
from transformers import AutoTokenizer, AutoModelForSequenceClassification, TrainingArguments, Trainer

# Variables to set the number of epochs and samples
num_epochs = 10
num_samples = 100  # set this to -1 to use all data
```

首先，我们需要加载数据集和模型分词器。

```py
# Step 1: Load dataset and model tokenizer
dataset = load_dataset('imdb')
tokenizer = AutoTokenizer.from_pretrained('bert-base-uncased')
```

接下来，我们将创建一个图表，以查看积极和消极类别的分布。

```py
# Data Exploration
train_df = pd.DataFrame(dataset["train"])
sns.countplot(x='label', data=train_df)
plt.title('Class distribution')
plt.show()
```

![](../Images/a751c4f02714c162279b8e2dbac1071c.png)

图 1\. 训练数据集的类别分布

接下来，我们通过对文本进行分词来预处理我们的数据集。我们使用 BERT 的分词器，将文本转换为与 BERT 词汇表对应的标记。

```py
# Step 2: Preprocess the dataset
def tokenize_function(examples):
    return tokenizer(examples["text"], padding="max_length", truncation=True)

tokenized_datasets = dataset.map(tokenize_function, batched=True)
```

![](../Images/ae0915081c37ac5286fdd5ded5e033d4.png)

之后，我们准备训练和评估数据集。请记住，如果你想使用所有数据，可以将`num_samples`变量设置为`-1`。

```py
if num_samples == -1:
    small_train_dataset = tokenized_datasets["train"].shuffle(seed=42)
    small_eval_dataset = tokenized_datasets["test"].shuffle(seed=42)
else:
    small_train_dataset = tokenized_datasets["train"].shuffle(seed=42).select(range(num_samples)) 
    small_eval_dataset = tokenized_datasets["test"].shuffle(seed=42).select(range(num_samples)) 
```

接下来，我们加载预训练的BERT模型。我们将使用`AutoModelForSequenceClassification`类，这是一种针对分类任务设计的BERT模型。

> 在本教程中，我们使用‘[bert-base-uncased](https://huggingface.co/bert-base-uncased)’版本的BERT，它是基于小写英文文本进行训练的。

```py
# Step 3: Load pre-trained model
model = AutoModelForSequenceClassification.from_pretrained('bert-base-uncased', num_labels=2)
```

现在，我们准备定义我们的训练参数并创建一个`Trainer`实例来训练我们的模型。

```py
# Step 4: Define training arguments
training_args = TrainingArguments("test_trainer", evaluation_strategy="epoch", no_cuda=True, num_train_epochs=num_epochs)

# Step 5: Create Trainer instance and train
trainer = Trainer(
    model=model, args=training_args, train_dataset=small_train_dataset, eval_dataset=small_eval_dataset
)

trainer.train()
```

# 结果解释

训练完模型后，让我们来评估一下。我们将计算混淆矩阵和ROC曲线，以了解我们的模型表现如何。

```py
# Step 6: Evaluation
predictions = trainer.predict(small_eval_dataset)

# Confusion matrix
cm = confusion_matrix(small_eval_dataset['label'], predictions.predictions.argmax(-1))
sns.heatmap(cm, annot=True, fmt='d')
plt.title('Confusion Matrix')
plt.show()

# ROC Curve
fpr, tpr, _ = roc_curve(small_eval_dataset['label'], predictions.predictions[:, 1])
roc_auc = auc(fpr, tpr)

plt.figure(figsize=(1.618 * 5, 5))
plt.plot(fpr, tpr, color='darkorange', lw=2, label='ROC curve (area = %0.2f)' % roc_auc)
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
plt.xlim([0.0, 1.0])
plt.ylim([0.0, 1.05])
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Receiver operating characteristic')
plt.legend(loc="lower right")
plt.show()
```

![](../Images/cb104a044e899ad272b1ce2d8f888394.png)

图 2\. 混淆矩阵

![](../Images/aa9c8ed466a7fc3bef2b259d818a359d.png)

图 3\. ROC曲线

混淆矩阵详细地分解了我们的预测与实际标签的匹配情况，而ROC曲线则展示了在不同阈值设置下，真正例率（敏感度）与假正例率（1 — 特异度）之间的权衡。

最后，为了查看我们的模型的实际效果，让我们用它来推断样本文本的情感。

```py
# Step 7: Inference on a new sample
sample_text = "This is a fantastic movie. I really enjoyed it."
sample_inputs = tokenizer(sample_text, padding="max_length", truncation=True, max_length=512, return_tensors="pt")

# Move inputs to device (if GPU available)
sample_inputs.to(training_args.device)

# Make prediction
predictions = model(**sample_inputs)
predicted_class = predictions.logits.argmax(-1).item()

if predicted_class == 1:
    print("Positive sentiment")
else:
    print("Negative sentiment")
```

![](../Images/84a2556951138b36dc6bda8b93d1a4a9.png)

# 结语

通过对IMDb电影评论的情感分析示例进行讲解，我希望你能清晰地理解如何将BERT应用于现实世界的自然语言处理问题。我在这里包含的Python代码可以进行调整和扩展，以应对不同的任务和数据集，为更复杂和准确的语言模型铺平道路。

# 参考文献

[1] Devlin, J., Chang, M. W., Lee, K., & Toutanova, K. (2018). BERT：用于语言理解的深度双向变换器的预训练。arXiv预印本 arXiv:1810.04805

[2] Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., … & Polosukhin, I. (2017). 注意力机制即一切。在《神经信息处理系统进展》中（第5998–6008页）。

[3] Wolf, T., Debut, L., Sanh, V., Chaumond, J., Delangue, C., Moi, A., … & Rush, A. M. (2019). Huggingface的变换器：最先进的自然语言处理。ArXiv, abs/1910.03771。

[4] Lhoest, Q., Villanova del Moral, A., Jernite, Y., Thakur, A., von Platen, P., Patil, S., Chaumond, J., Drame, M., Plu, J., Tunstall, L., Davison, J., Šaško, M., Chhablani, G., Malik, B., Brandeis, S., Le Scao, T., Sanh, V., Xu, C., Patry, N., McMillan-Major, A., Schmid, P., Gugger, S., Delangue, C., Matussière, T., Debut, L., Bekman, S., Cistac, P., Goehringer, T., Mustar, V., Lagunas, F., Rush, A., & Wolf, T. (2021). 数据集：自然语言处理的社区库。收录于 2021 年自然语言处理实证方法会议：系统演示 (第 175–184 页)。在线和多米尼加共和国蓬塔卡纳：计算语言学协会。取自 [https://aclanthology.org/2021.emnlp-demo.21](https://aclanthology.org/2021.emnlp-demo.21)

感谢阅读。*如果您有任何反馈，请随时通过评论此帖子、在* [*LinkedIn*](https://www.linkedin.com/in/shashankkapadia/)*上给我留言，或发送电子邮件 (smhkapadia[at]gmail.com)*

*如果您喜欢这篇文章，请访问我的其他文章*

[](/domain-adaption-fine-tune-pre-trained-nlp-models-a06659ca6668?source=post_page-----4715ed0deede--------------------------------) [## 领域适应：微调预训练的 NLP 模型

### 逐步指南：为任何领域微调预训练的 NLP 模型

towardsdatascience.com](/domain-adaption-fine-tune-pre-trained-nlp-models-a06659ca6668?source=post_page-----4715ed0deede--------------------------------) [](https://medium.com/aimonks/the-evolution-of-natural-language-processing-56ce27916e10?source=post_page-----4715ed0deede--------------------------------) [## 自然语言处理的演变

### 语言模型发展的历史视角

medium.com](https://medium.com/aimonks/the-evolution-of-natural-language-processing-56ce27916e10?source=post_page-----4715ed0deede--------------------------------) [](/recommendation-system-in-python-lightfm-61c85010ce17?source=post_page-----4715ed0deede--------------------------------) [## Python 中的推荐系统：LightFM

### 使用 LightFM 在 Python 中构建推荐系统的逐步指南

towardsdatascience.com](/recommendation-system-in-python-lightfm-61c85010ce17?source=post_page-----4715ed0deede--------------------------------) [](/evaluate-topic-model-in-python-latent-dirichlet-allocation-lda-7d57484bb5d0?source=post_page-----4715ed0deede--------------------------------) [## 评估主题模型：潜在狄利克雷分配 (LDA)

### 构建可解释的主题模型的逐步指南

towardsdatascience.com](/evaluate-topic-model-in-python-latent-dirichlet-allocation-lda-7d57484bb5d0?source=post_page-----4715ed0deede--------------------------------)
