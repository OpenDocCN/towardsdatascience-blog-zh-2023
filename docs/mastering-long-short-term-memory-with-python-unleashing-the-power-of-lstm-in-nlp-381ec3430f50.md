# 使用 Python 掌握长短期记忆（LSTM）：释放 LSTM 在自然语言处理中的威力

> 原文：[`towardsdatascience.com/mastering-long-short-term-memory-with-python-unleashing-the-power-of-lstm-in-nlp-381ec3430f50?source=collection_archive---------7-----------------------#2023-11-28`](https://towardsdatascience.com/mastering-long-short-term-memory-with-python-unleashing-the-power-of-lstm-in-nlp-381ec3430f50?source=collection_archive---------7-----------------------#2023-11-28)

## 一份全面的指南，用于理解和实现用于自然语言处理的 LSTM 层，使用 Python 语言

[](https://eligijus-bujokas.medium.com/?source=post_page-----381ec3430f50--------------------------------)![Eligijus Bujokas](https://eligijus-bujokas.medium.com/?source=post_page-----381ec3430f50--------------------------------)[](https://towardsdatascience.com/?source=post_page-----381ec3430f50--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----381ec3430f50--------------------------------) [Eligijus Bujokas](https://eligijus-bujokas.medium.com/?source=post_page-----381ec3430f50--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd61597e07b4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-long-short-term-memory-with-python-unleashing-the-power-of-lstm-in-nlp-381ec3430f50&user=Eligijus+Bujokas&userId=d61597e07b4d&source=post_page-d61597e07b4d----381ec3430f50---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----381ec3430f50--------------------------------) ·17 分钟阅读·2023 年 11 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F381ec3430f50&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-long-short-term-memory-with-python-unleashing-the-power-of-lstm-in-nlp-381ec3430f50&user=Eligijus+Bujokas&userId=d61597e07b4d&source=-----381ec3430f50---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F381ec3430f50&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-long-short-term-memory-with-python-unleashing-the-power-of-lstm-in-nlp-381ec3430f50&source=-----381ec3430f50---------------------bookmark_footer-----------)![](img/8590a68892231bb5a03f41691fc9f697.png)

图片由 [Sven Brandsma](https://unsplash.com/@seffen99?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这项工作是我关于 [RNN 和 NLP 的 Python 文章](https://medium.com/towards-data-science/mastering-nlp-in-depth-python-coding-for-deep-learning-models-a15055e989bf) 的延续。深度学习网络中简单递归层的自然进展是具有**长短期记忆**（简称 **LSTM**）层的深度学习网络。

与 RNN 和 NLP 一样，我将尽量详细解释 LSTM 层，并从零开始编写该层的前向传播代码。

所有代码可以在这里查看：[`github.com/Eligijus112/NLP-python`](https://github.com/Eligijus112/NLP-python)

我们将使用与上一篇文章中相同的数据集¹：

```py
# Data wrangling
import pandas as pd

# Reading the data 
d = pd.read_csv('input/Tweets.csv', header=None)

# Adding the columns 
d.columns = ['INDEX', 'GAME', "SENTIMENT", 'TEXT']

# Leaving only the positive and the negative sentiments 
d = d[d['SENTIMENT'].isin(['Positive', 'Negative'])]

# Encoding the sentiments that the negative will be 1 and the positive 0
d['SENTIMENT'] = d['SENTIMENT'].apply(lambda x: 0 if x == 'Positive' else 1)

# Dropping missing values
d = d.dropna()
```
