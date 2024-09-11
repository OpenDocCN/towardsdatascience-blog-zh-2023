# 寻找改进的措辞

> 原文：[https://towardsdatascience.com/finding-improved-rephrasings-b5fb002ac811?source=collection_archive---------15-----------------------#2023-04-19](https://towardsdatascience.com/finding-improved-rephrasings-b5fb002ac811?source=collection_archive---------15-----------------------#2023-04-19)

## 使用带有机器学习元素的 Trie

[](https://jagota-arun.medium.com/?source=post_page-----b5fb002ac811--------------------------------)[![Arun Jagota](../Images/3c3eb142f671b5fb933c2826d8ed78d9.png)](https://jagota-arun.medium.com/?source=post_page-----b5fb002ac811--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5fb002ac811--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b5fb002ac811--------------------------------) [Arun Jagota](https://jagota-arun.medium.com/?source=post_page-----b5fb002ac811--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fef9ed921edad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-improved-rephrasings-b5fb002ac811&user=Arun+Jagota&userId=ef9ed921edad&source=post_page-ef9ed921edad----b5fb002ac811---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5fb002ac811--------------------------------) ·19分钟阅读·2023年4月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb5fb002ac811&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-improved-rephrasings-b5fb002ac811&user=Arun+Jagota&userId=ef9ed921edad&source=-----b5fb002ac811---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb5fb002ac811&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-improved-rephrasings-b5fb002ac811&source=-----b5fb002ac811---------------------bookmark_footer-----------)![](../Images/b2d3d765f44d8d3e011a1660ef32353e.png)

图片由 [Nile](https://pixabay.com/users/nile-598962/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=623167) 提供，来源于 [Pixabay](https://pixabay.com/)

良好的表达需要努力。幸运的是，现代自然语言处理技术的化身，如 ChatGPT，非常有帮助。

在这篇文章中，我们将使用经过适当增强的 Trie 模型来解决这个问题。这个 Trie 将以无监督的方式自动检测在语料库中反复出现的短词序列。随后，它还将从标记的数据集（包括“尴尬措辞”和“改进措辞”对）中学习常见的“尴尬措辞”模式。

对于小而微妙的措辞问题变体，我们将展示通过对学习到的 Trie 进行适当的模糊搜索，可以直接找到改进的措辞。

对于更复杂的问题版本，我们描述了一种使用 Tries 作为特征提取器的变体。这些可以集成到任何最先进的神经语言模型中。

对于这种用途，Trie 需要经过特定的预处理和预训练。我们也会描述这两者。

这种方法——本身——是…

+   易于理解，无需任何神经网络或 NLP 背景知识。

+   作为概念验证实现起来很简单。

+   本身有效于检测简短的尴尬短语并建议重新措辞。
