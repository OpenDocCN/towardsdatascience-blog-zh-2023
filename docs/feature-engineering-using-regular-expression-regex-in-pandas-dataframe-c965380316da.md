# 使用正则表达式（RegEx）进行 Pandas DataFrame 的特征工程

> 原文：[`towardsdatascience.com/feature-engineering-using-regular-expression-regex-in-pandas-dataframe-c965380316da?source=collection_archive---------8-----------------------#2023-01-06`](https://towardsdatascience.com/feature-engineering-using-regular-expression-regex-in-pandas-dataframe-c965380316da?source=collection_archive---------8-----------------------#2023-01-06)

## 了解如何使用正则表达式轻松处理字符串列

[](https://weimenglee.medium.com/?source=post_page-----c965380316da--------------------------------)![Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----c965380316da--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c965380316da--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c965380316da--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----c965380316da--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-engineering-using-regular-expression-regex-in-pandas-dataframe-c965380316da&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----c965380316da---------------------post_header-----------) 发表在 [数据科学的探索](https://towardsdatascience.com/?source=post_page-----c965380316da--------------------------------) · 13 分钟阅读 · 2023 年 1 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc965380316da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-engineering-using-regular-expression-regex-in-pandas-dataframe-c965380316da&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----c965380316da---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc965380316da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-engineering-using-regular-expression-regex-in-pandas-dataframe-c965380316da&source=-----c965380316da---------------------bookmark_footer-----------)![](img/30db292baa4a61d31d6260220a331a59.png)

图片由 [Clark Van Der Beken](https://unsplash.com/@snapsbyclark?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

在 Pandas 中操作字符串列是数据工程师最常执行的操作之一。大多数情况下，你会做诸如分割列、从列中提取关键信息等操作。这项任务通常被称为*特征工程*。在本文中，我将带你了解一些你可以利用*正则表达式*的特征工程技术。

**正则表达式**（通常称为**regex**或**regexp**）是一系列字符，用于指定文本中的搜索模式。正则表达式非常强大，但对初学者来说也可能令人望而生畏。因此，在本文中，我将提供一个温和的正则表达式入门介绍，以帮助你入门。

# 我们的数据集

一如既往，我最喜欢用来说明本文概念的数据集是 Titanic 数据集。它包含一些非常适合进行特征工程的字符串列。

> ***数据来源***：本文的数据来源于*[`www.kaggle.com/datasets/tedllh/titanic-train`](https://www.kaggle.com/datasets/tedllh/titanic-train)*。
> 
> ***许可 —*** *数据库内容*…
