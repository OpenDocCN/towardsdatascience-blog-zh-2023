# 时间序列模型中的文本数据预处理

> 原文：[`towardsdatascience.com/text-data-pre-processing-for-time-series-models-162c0d01f5c5`](https://towardsdatascience.com/text-data-pre-processing-for-time-series-models-162c0d01f5c5)

## 你是否曾考虑过如何将文本数据中的情感用作时间序列模型中的回归器？

[](https://petrkorab.medium.com/?source=post_page-----162c0d01f5c5--------------------------------)![Petr Korab](https://petrkorab.medium.com/?source=post_page-----162c0d01f5c5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----162c0d01f5c5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----162c0d01f5c5--------------------------------) [Petr Korab](https://petrkorab.medium.com/?source=post_page-----162c0d01f5c5--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----162c0d01f5c5--------------------------------) ·6 分钟阅读·2023 年 2 月 9 日

--

![](img/c572f7ea6033e1d39e2accc7b85121c6.png)

照片由 Kaleidico 在 Unsplash 提供

# 介绍

文本数据提供了可以量化、聚合并用作时间序列模型中的变量的定性信息。从 NLP 的早期开始，简单的文本数据表示方法，如[独热编码](https://machinelearningmastery.com/how-to-one-hot-encode-sequence-data-in-python/)的类别变量和词[ n-gram](https://www.analyticsvidhya.com/blog/2021/09/what-are-n-grams-and-how-to-implement-them-in-python/)，已经被使用。随着时间的推移，包括[词袋模型](https://machinelearningmastery.com/gentle-introduction-bag-words-model/)在内的更复杂的方法，开始用于表示文本数据以供机器学习算法使用。基于 Harris [1] 和 Firth [2] 提出的分布假设，现代模型如 Word-to-Vec [3] 和 [4]、GloVe [5] 和 ELMo [6] 在其神经网络架构中使用词的向量表示。由于计算机将文本处理为向量，因此它可以作为时间序列计量经济模型中的变量使用。

> 通过这种方式，我们可以利用文本中的定性信息，扩展定量时间序列模型的可能性。

在本文中，你将深入了解：

+   如何将文本中的定性信息用于定量建模

+   如何清洗和表示时间序列模型中的文本数据

+   如何高效处理**100 万行文本数据**

+   Python 中的端到端编码示例。

在我们最近的会议论文中，我们制定了一个文本数据预处理的结构性计划，可能用于以下领域：（1）利用社交网络的情感预测汇率，（2）使用公开新闻数据预测农业价格，（3）在各个领域进行需求预测。

# **1\. 文本数据表示的结构计划**

让我们从计划开始。开始时，我们有**定性原始文本**数据，随着时间的推移进行收集。最后，我们得到的是具有时间变化的数值向量（=**定量数据**）。这个图表更清楚地说明了我们将如何进行：

![](img/cf08c69096f31e5002ac150c93450ef3.png)

图 1\. 文本数据表示的结构计划。来源：Poměnková 等人，提交至[MAREW 2023](https://www.marew.cz/)。

# 2\. Python 中的经验示例

让我们通过[**新闻类别数据集**](https://www.kaggle.com/datasets/rmisra/news-category-dataset)展示编码，该数据集由 Rishabh Misra 编制[8]，[9]，并根据[署名 4.0 国际](https://creativecommons.org/licenses/by/4.0/)许可证发布。数据包含 2012 年至 2022 年间在 huffpost.com 上发布的新闻标题，已扩展到 100 万行的数据集。

> 主要目标是从新闻标题中构建按月频率的时间序列，反映公众情绪。

数据集包含 100 万个标题。由于其规模，我使用了[Polars](https://pypi.org/project/polars/)库，这使得数据框操作更加高效。与主流的 Pandas 相比，它处理大型数据文件的效率更高。此外，代码在 Google Colab 上运行，使用了 GPU 硬件加速器。

Python 代码在[这里](https://github.com/PetrKorab/Text-Data-Pre-processing-for-Time-series-Models)，数据如下：

![](img/25d4f8392aa4cf56a94555b83f266278.png)

图 2\. 新闻类别数据集

# 2.1\. 文本数据预处理

文本数据预处理的目的是去除所有可能偏倚分析或导致结果不准确解释的冗余信息。我们将移除**标点符号**、**数字**、**多余的空格**、英语**停用词**（最常见的、信息量低或为零的词），并将文本**转为小写**。

> 可能最简单且最有效的文本数据清理方法是使用[cleantext](https://pypi.org/project/cleantext/)库。

首先，定义一个清理函数以执行清理操作：

```py
def preprocess(text):
    output = clean(str(text), punct=True,
                              extra_spaces=True,
                              stopwords=True,
                              lowercase=True,
                              numbers = True)
    return output
```

接下来，我们使用 Polars 清理 1 百万数据集：

```py
data_clean = data.with_columns([
    pl.col("headline").apply(preprocess)
])
```

清理后的数据集包含最大信息量的文本，便于进一步处理。任何不必要的字符串和数字都会降低最终经验建模的准确性。

# 2.2\. 文本数据表示

**数据表示**涉及在计算机中表示数据的方法。由于计算机处理的是数字，我们选择了一个适当的模型来向量化文本数据集。

在我们的项目中，我们正在构建情感的时间序列。对于这种用例，预训练的情感分类器**VADER *(情感词典和情感推理器)* **是一个不错的选择。阅读我的[上一篇文章](https://medium.com/towards-data-science/the-most-favorable-pre-trained-sentiment-classifiers-in-python-9107c06442c6)以了解更多关于该分类器的信息以及其他一些替代方案。

使用[vaderSentiment](https://pypi.org/project/vaderSentiment/)库进行分类的代码如下。首先，创建分类函数：

```py
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer

# calculate the compound score
def sentiment_vader(sentence):

    # create a SentimentIntensityAnalyzer object
    sid_obj = SentimentIntensityAnalyzer()

    sentiment_dict = sid_obj.polarity_scores(sentence)

    # create overall (compound) indicator
    compound = sentiment_dict['compound']

    return compound
```

接下来，应用时间序列数据集的函数：

```py
# apply the function with Polars

sentiment = data_clean.with_columns([
    pl.col("headline").apply(sentiment_vader)
])
```

结果如下：

![](img/ba7ed9741a751c82c61badb4b7430180.png)

图 3\. 情感评估

*标题* 列包含情感，范围为[-1:1]，反映每行标题中的主要情感内容。

# 2.3\. 时间序列表示

时间序列文本数据表示的下一步是扩展数据矩阵的时间维度。可以通过 (a) 沿时间轴聚合数据和 (b) 选择实现时间序列文本数据表示的方法来实现。在我们的数据中，我们将做前者，并按月频率聚合每行的情感。

这段代码对情感进行平均聚合，并准备每月的时间序列：

```py
# aggregate over months

timeseries = (sentiment.lazy()
    .groupby("date_monthly")
    .agg(
        [
            pl.avg("headline")
        ]
    ).sort("date_monthly")
).collect()
```

# 2.4\. 定量建模

最后的步骤是使用时间序列进行建模。以我们的最近会议论文为例，我们类似地从前五名经济学期刊发表的研究文章标题中提取了情感。然后，我们使用 5 年窗口的滚动时间变动相关性，并观察情感与 GDP 及其他全球经济指标的关系（见图 2）。

我们假设情感与宏观经济环境在经济急剧衰退和通货膨胀冲击期间相关。结果支持这种考虑，除了一个特定的期刊，关于 1970 年代的石油冲击，这导致了急剧的衰退伴随着巨大的通货膨胀峰值。

![](img/58d0b0e9bbd828241d5a69f71b6583a7.png)

图 4\. 情感与 GDP 的滚动相关性。来源：Poměnková et al.，提交至[MAREW 2023](https://www.marew.cz/)。

# 结论

在这篇文章中，我们从 100 万行文本数据中构建了每月的情感时间序列。关键点包括：

+   **定性信息可以扩展定量时间序列模型的能力。**

+   **Polars 库使得即使在 Python 语言中，也能实现大规模文本数据预处理的可行性。**

+   **诸如 Google Colab 的云服务使得处理大规模文本数据集的速度更快。**

本教程的完整代码在我的 [GitHub](https://github.com/PetrKorab/Text-Data-Pre-processing-for-Time-series-Models) 上。推荐阅读 [*Python 中最受欢迎的预训练情感分类器*](https://medium.com/towards-data-science/the-most-favorable-pre-trained-sentiment-classifiers-in-python-9107c06442c6)*。*

*你喜欢这篇文章吗？你可以邀请我* [*喝咖啡*](https://www.buymeacoffee.com/petrkorab) *来支持我的写作。你也可以订阅我的* [*邮件列表*](https://medium.com/subscribe/@petrkorab) *以便接收我的新文章通知。谢谢！*

# **参考文献**

[1] Z. Harris. 1954\. 分布结构。*Word*，第 10 卷，第 23 期，第 146–162 页。

[2] J. R. Firth. 1957\. 语言学理论的概述 1930–1955\. 收录于《语言学分析研究》，第 1–32 页\. 牛津：语言学会。重印于 F.R. Palmer (编)，《J.R. Firth 1952–1959 选集》，伦敦：Longman 1968。

[3] Mikolov, T., Chen, K., Corrado, G. S., Dean, J. 2013b. 向量空间中词表示的高效估计。计算与语言：国际学习表示会议。

[4] T. Mikolov, I. Sutskever, K. Chen, G. S. Corrado 和 J. Dean. 2013\. 词汇和短语的分布式表示及其组合性。*神经信息处理系统进展*，第 26 卷 (NIPS 2013)。

[5] M. E. Peters, M. Neumann, M. Iyyer, M. Gardner, C. Clark, K. Lee 和 L. Zettlemoyer, L. 2018\. 2018 年北美计算语言学协会会议：人类语言技术会议论文集，第 1 卷。

[6] J. Pennington, R. Socher 和 C. D. Manning. 2014\. GloVe：全局词向量表示。发表于 2014 年自然语言处理实证方法会议（EMNLP）论文集。

[7] Poměnková, J., Koráb, P., Štrba, D. 时间序列建模的文本数据预处理。提交至 [MAREW 2023](https://www.marew.cz/)。

[8] Misra, Rishabh. “新闻类别数据集。” arXiv 预印本 arXiv:2209.11429 (2022)。

[9] Misra, Rishabh 和 Jigyasa Grover. “为 ML 雕刻数据：机器学习的第一幕。” ISBN 9798585463570 (2021)。
