# 在 R 中理解正则表达式的技巧

> 原文：[`towardsdatascience.com/tips-to-understand-regular-expressions-in-r-5d25be06f2a8?source=collection_archive---------13-----------------------#2023-01-18`](https://towardsdatascience.com/tips-to-understand-regular-expressions-in-r-5d25be06f2a8?source=collection_archive---------13-----------------------#2023-01-18)

## 通过 stringR 增强对正则表达式的理解

[](https://gustavorsantos.medium.com/?source=post_page-----5d25be06f2a8--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----5d25be06f2a8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d25be06f2a8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d25be06f2a8--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----5d25be06f2a8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftips-to-understand-regular-expressions-in-r-5d25be06f2a8&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----5d25be06f2a8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d25be06f2a8--------------------------------) ·8 分钟阅读·2023 年 1 月 18 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5d25be06f2a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftips-to-understand-regular-expressions-in-r-5d25be06f2a8&user=Gustavo+Santos&userId=4429d99b1245&source=-----5d25be06f2a8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5d25be06f2a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftips-to-understand-regular-expressions-in-r-5d25be06f2a8&source=-----5d25be06f2a8---------------------bookmark_footer-----------)![](img/ca3fa7161dcaa5cd562cdf85952ca062.png)

图片由 [Jason Leung](https://unsplash.com/ko/@ninjason?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/0sBTrm726C8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 简介

当一个项目涉及文本分析时，如情感分析、文本挖掘或任何其他需要分析文本信息的任务，通常会需要在某个阶段对文本进行解析。这意味着你需要提取文本的一部分或在文本中查找给定的模式，以便能够提取出一些见解。

如果我们处理的是简单的模式，比如一个单词或一个数字，那么在 R 中处理起来很简单。

假设你有以下文本，并且你想找到单词`random`。我们可以使用许多简单的函数来执行这个任务。但首先加载`library(stringr)`。接下来看看它的三个函数。

```py
text <- "This is a random text. If you want to try to find a pattern here, 
let's say the numbers 1 or 2 or 3, you can use stringR."

# Find out if the word "random" is present
str_find(text, pattern='random')
[1] TRUE

# Extract the word "random"
str_extract(text, pattern= 'random')
[1] "random"

# Find where the pattern is located in the text
str_locate(text, 'random')
     start end
[1,]    11  16
```

现在，让我们想象一下我们想要找到另一种模式，一种非常特定的模式，比如*任何*…
