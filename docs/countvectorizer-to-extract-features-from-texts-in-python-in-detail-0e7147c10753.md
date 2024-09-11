# CountVectorizer 在 Python 中提取文本特征的详细介绍

> 原文：[https://towardsdatascience.com/countvectorizer-to-extract-features-from-texts-in-python-in-detail-0e7147c10753?source=collection_archive---------6-----------------------#2023-10-21](https://towardsdatascience.com/countvectorizer-to-extract-features-from-texts-in-python-in-detail-0e7147c10753?source=collection_archive---------6-----------------------#2023-10-21)

![](../Images/07e11611df2a6a84e10f07ef5553f473.png)

图片由 [Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 你需要了解的关于如何在 Sklearn 中有效使用 CountVectorizer 的所有知识

[](https://rashida00.medium.com/?source=post_page-----0e7147c10753--------------------------------)[![Rashida Nasrin Sucky](../Images/42bd057e8eca255907c43c29a498f2ca.png)](https://rashida00.medium.com/?source=post_page-----0e7147c10753--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0e7147c10753--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----0e7147c10753--------------------------------) [Rashida Nasrin Sucky](https://rashida00.medium.com/?source=post_page-----0e7147c10753--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a36b941a136&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcountvectorizer-to-extract-features-from-texts-in-python-in-detail-0e7147c10753&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=post_page-8a36b941a136----0e7147c10753---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0e7147c10753--------------------------------) ·7 min read·2023年10月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0e7147c10753&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcountvectorizer-to-extract-features-from-texts-in-python-in-detail-0e7147c10753&user=Rashida+Nasrin+Sucky&userId=8a36b941a136&source=-----0e7147c10753---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0e7147c10753&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcountvectorizer-to-extract-features-from-texts-in-python-in-detail-0e7147c10753&source=-----0e7147c10753---------------------bookmark_footer-----------)

任何自然语言处理（NLP）项目最基本的数据处理是将文本数据转换为数字数据。只要数据是文本形式，我们就无法对其进行任何计算操作。

此文本到数字数据转换有多种方法可用。本教程将解释 scikit-learn 库中最基本的向量化器之一，即 CountVectorizer 方法。

这个方法非常简单。它将每个单词的出现频率作为数值。通过例子可以更清楚地理解。

在以下代码块中：

+   我们将导入 CountVectorizer 方法。

+   调用该方法。

+   将文本数据适配到 CountVectorizer 方法，并将其转换为数组。

```py
import pandas as pd 
from sklearn.feature_extraction.text import CountVectorizer 

#This is the text to be vectorized
text = ["Hello Everyone! This is Lilly. My aunt's name is also Lilly. I love my aunt.\
        I am trying to learn how to use count vectorizer."]

cv= CountVectorizer() 
count_matrix = cv.fit_transform(text)…
```
