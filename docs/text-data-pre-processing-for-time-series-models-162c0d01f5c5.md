# 时间序列模型中的文本数据预处理

> 原文：[https://towardsdatascience.com/text-data-pre-processing-for-time-series-models-162c0d01f5c5?source=collection_archive---------10-----------------------#2023-02-09](https://towardsdatascience.com/text-data-pre-processing-for-time-series-models-162c0d01f5c5?source=collection_archive---------10-----------------------#2023-02-09)

## 你是否曾考虑过如何将文本数据中的情感用作时间序列模型中的回归变量？

[](https://petrkorab.medium.com/?source=post_page-----162c0d01f5c5--------------------------------)[![Petr Korab](../Images/9f3afb4b8985584981220e30f18e3b69.png)](https://petrkorab.medium.com/?source=post_page-----162c0d01f5c5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----162c0d01f5c5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----162c0d01f5c5--------------------------------) [Petr Korab](https://petrkorab.medium.com/?source=post_page-----162c0d01f5c5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F13a053cbaad9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-data-pre-processing-for-time-series-models-162c0d01f5c5&user=Petr+Korab&userId=13a053cbaad9&source=post_page-13a053cbaad9----162c0d01f5c5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----162c0d01f5c5--------------------------------) ·6分钟阅读·2023年2月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F162c0d01f5c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-data-pre-processing-for-time-series-models-162c0d01f5c5&user=Petr+Korab&userId=13a053cbaad9&source=-----162c0d01f5c5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F162c0d01f5c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftext-data-pre-processing-for-time-series-models-162c0d01f5c5&source=-----162c0d01f5c5---------------------bookmark_footer-----------)![](../Images/c572f7ea6033e1d39e2accc7b85121c6.png)

由Kaleidico在Unsplash拍摄

# 介绍

文本数据提供了可以量化、汇总并作为时间序列模型中的变量使用的定性信息。文本数据表示的简单方法，如[独热编码](https://machinelearningmastery.com/how-to-one-hot-encode-sequence-data-in-python/)分类变量和词[ n-gram](https://www.analyticsvidhya.com/blog/2021/09/what-are-n-grams-and-how-to-implement-them-in-python/)，自自然语言处理（NLP）早期开始就已被使用。随着时间的推移，更多复杂的方法，包括[词袋模型](https://machinelearningmastery.com/gentle-introduction-bag-words-model/)，也开始用于表示机器学习算法中的文本数据。基于哈里斯[1]和弗斯[2]提出的分布假设，现代模型如Word-to-Vec [3]和[4]、GloVe [5]和ELMo [6]在其神经网络架构中使用词的向量表示。由于计算机以向量形式处理文本，因此它可以作为时间序列计量经济学模型中的变量。

> 通过这种方式，我们可以利用文本中的定性信息，扩展定量时间序列模型的可能性。

在这篇文章中，你将学习更多内容：

+   如何将文本中的定性信息用于定量分析…
