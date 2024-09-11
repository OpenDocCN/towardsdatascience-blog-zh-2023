# 为什么概率链结比模糊匹配或基于术语频率的方法更准确

> 原文：[`towardsdatascience.com/why-probabilistic-linkage-is-more-accurate-than-fuzzy-matching-or-term-frequency-based-approaches-15a28c733e73?source=collection_archive---------7-----------------------#2023-10-26`](https://towardsdatascience.com/why-probabilistic-linkage-is-more-accurate-than-fuzzy-matching-or-term-frequency-based-approaches-15a28c733e73?source=collection_archive---------7-----------------------#2023-10-26)

## 不同的记录链结方法如何有效利用记录中的信息进行预测？

[](https://robinlinacre.medium.com/?source=post_page-----15a28c733e73--------------------------------)![Robin Linacre](https://robinlinacre.medium.com/?source=post_page-----15a28c733e73--------------------------------)[](https://towardsdatascience.com/?source=post_page-----15a28c733e73--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----15a28c733e73--------------------------------) [Robin Linacre](https://robinlinacre.medium.com/?source=post_page-----15a28c733e73--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a6a0dc508d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-probabilistic-linkage-is-more-accurate-than-fuzzy-matching-or-term-frequency-based-approaches-15a28c733e73&user=Robin+Linacre&userId=8a6a0dc508d6&source=post_page-8a6a0dc508d6----15a28c733e73---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----15a28c733e73--------------------------------) ·4 min read·2023 年 10 月 26 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F15a28c733e73&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-probabilistic-linkage-is-more-accurate-than-fuzzy-matching-or-term-frequency-based-approaches-15a28c733e73&user=Robin+Linacre&userId=8a6a0dc508d6&source=-----15a28c733e73---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F15a28c733e73&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhy-probabilistic-linkage-is-more-accurate-than-fuzzy-matching-or-term-frequency-based-approaches-15a28c733e73&source=-----15a28c733e73---------------------bookmark_footer-----------)![](img/e0e3237d3ad7b6527286b4e82bd0913a.png)

从数据中提取信息。图像由作者使用 DALL·E 3 创建

一个普遍存在的数据质量问题是有多个不同的记录指向同一实体，但没有唯一的标识符将这些实体联系在一起。

在缺乏唯一标识符如社会安全号码的情况下，我们可以使用一组合在一起的非唯一变量，如姓名、性别和出生日期来识别个人。

为了在记录链接中获得最佳准确性，我们需要一个从输入数据中提取尽可能多信息的模型。

本文描述了在做出准确预测时最重要的三种信息类型，以及 Fellegi-Sunter 模型如何利用这三种信息，该模型用于[Splink](https://github.com/moj-analytical-services/splink)（一个用于数据链接和去重的免费 Python 包）。

它还描述了一些替代的记录链接方法如何丢弃部分信息，从而降低了准确性。

## 三种信息类型

大致上，有三类信息在预测一对记录是否匹配时是相关的：

1.  记录对的相似性

1.  整体数据集中的值的频率，以及更广泛地测量不同情景的普遍性

1.  整体数据集的数据质量

让我们逐一查看。

## 1\. 配对记录比较的相似性：模糊匹配

预测两个记录是否表示同一实体的最明显方法是测量列中是否包含相同或相似的信息。

每列的相似性可以使用模糊匹配函数如[Levenshtein](https://en.wikipedia.org/wiki/Levenshtein_distance)或[Jaro-Winker](https://en.wikipedia.org/wiki/Jaro%E2%80%93Winkler_distance)进行定量测量，或使用绝对或百分比差异等数值差异。

例如，`Hammond`与`Hamond`的 Jaro-Winkler 相似度为 0.97（1.0 是完美分数）。这可能是一个拼写错误。

这些度量可以分配权重，并求和以计算总相似性分数。

这种方法有时被称为模糊匹配，它是准确链接模型的重要组成部分。

然而，仅使用这种方法有一个主要缺陷：权重是任意的：

+   用户需要猜测不同领域的重要性。例如，应该给年龄的匹配分配什么权重？这与名字匹配相比如何？当信息不匹配时，我们应该如何决定惩罚性权重的大小？

+   预测的强度与每个模糊匹配度量之间的关系必须由用户猜测，而不是被估计。例如，如果名字的 Jaro-Winkler 相似度为 0.9 而不是完全匹配，我们的预测应该改变多少？如果 Jaro-Winkler 分数降至 0.8，预测的变化量是否相同？

## 2\. 整体数据集中值的频率，或更广泛地测量不同情景的普遍性

我们可以通过考虑整体数据集中值的频率（有时称为“术语频率”）来改进模糊匹配。

例如，`John`与`John`，以及`Joss`与`Joss`都是完全匹配，因此具有相同的相似性评分，但后者比前者更有力地证明匹配，因为`Joss`是一个不常见的名字。

`John`与`Joss`的相对术语频率提供了这些不同名字相对重要性的基于数据的估计，这可以用来指导权重设置。

这个概念可以扩展到包括那些不是完全匹配的类似记录。权重可以通过估计在数据集中观察到模糊匹配的常见程度来推导。例如，如果在 Jaro-Winkler 评分为 0.7 的情况下，即使在不匹配记录之间，看到模糊匹配非常常见，那么如果观察到这样的匹配，这并不能提供多少支持匹配的证据。在概率连接中，这些信息被捕捉在被称为`u`概率的参数中，详细描述见[这里](https://moj-analytical-services.github.io/splink/topic_guides/theory/fellegi_sunter.html#u-probability)。

## 3. 整体数据集的数据质量：衡量不匹配信息的重要性

我们已经看到，模糊匹配和基于术语频率的方法可以让我们对记录之间的相似性进行评分，甚至在某种程度上对不同列上的匹配重要性进行加权。

然而，这些技术都无法帮助量化非匹配与预测匹配概率之间的相对重要性。

概率方法通过估计数据质量明确地估计这些情景的相对重要性。在概率连接中，这些信息被捕捉在`m`概率中，详细定义见[这里](https://moj-analytical-services.github.io/splink/topic_guides/theory/fellegi_sunter.html#m-probability)。

例如，如果性别变量的数据质量极高，那么性别的不匹配将是强有力的证据，表明这两条记录不是真正的匹配。

相反，如果记录已经被观察了多年，那么年龄的不匹配不会是两条记录匹配的有力证据。

## 概率连接

概率模型的强大之处在于以其他模型无法实现的方式结合所有三种信息来源。

不仅所有这些信息都被纳入预测中，[Fellegi-Sunter 模型中的部分匹配权重](https://moj-analytical-services.github.io/splink/topic_guides/theory/fellegi_sunter.html#match-weights)使得不同类型信息的相对重要性可以从数据本身中估计出来，从而正确地加权以优化准确性。

相反，模糊匹配技术通常使用任意权重，不能充分融入所有三个来源的信息。术语频率方法缺乏利用数据质量信息来负面加权不匹配信息的能力，也没有适当加权模糊匹配的机制。

*作者是* [*Splink*](http://github.com/moj-analytical-services/splink)*的开发者，这是一个用于大规模概率链接的免费开源 Python 包。*
