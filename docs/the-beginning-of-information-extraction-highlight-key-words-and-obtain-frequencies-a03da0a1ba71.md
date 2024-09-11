# 信息提取的开始：突出关键字并获取频率

> 原文：[`towardsdatascience.com/the-beginning-of-information-extraction-highlight-key-words-and-obtain-frequencies-a03da0a1ba71?source=collection_archive---------8-----------------------#2023-08-28`](https://towardsdatascience.com/the-beginning-of-information-extraction-highlight-key-words-and-obtain-frequencies-a03da0a1ba71?source=collection_archive---------8-----------------------#2023-08-28)

## 一种快速的方法，用于突出 PDF 文档中的感兴趣关键字并计算其频率。

[](https://ben-mccloskey20.medium.com/?source=post_page-----a03da0a1ba71--------------------------------)![Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----a03da0a1ba71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a03da0a1ba71--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a03da0a1ba71--------------------------------) [Benjamin McCloskey](https://ben-mccloskey20.medium.com/?source=post_page-----a03da0a1ba71--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F503796fc1483&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-beginning-of-information-extraction-highlight-key-words-and-obtain-frequencies-a03da0a1ba71&user=Benjamin+McCloskey&userId=503796fc1483&source=post_page-503796fc1483----a03da0a1ba71---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a03da0a1ba71--------------------------------) · 10 min 阅读 · 2023 年 8 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa03da0a1ba71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-beginning-of-information-extraction-highlight-key-words-and-obtain-frequencies-a03da0a1ba71&user=Benjamin+McCloskey&userId=503796fc1483&source=-----a03da0a1ba71---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa03da0a1ba71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-beginning-of-information-extraction-highlight-key-words-and-obtain-frequencies-a03da0a1ba71&source=-----a03da0a1ba71---------------------bookmark_footer-----------)![](img/47bf25395e329662fb5f9381919d3942.png)

图片由 [Judy Velazquez](https://unsplash.com/@roses_n_basil?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

随着可用信息量的日益增加，快速收集有关这些信息的相关统计数据对于关系映射和获得新的视角以避免数据冗余非常重要。今天我们将探讨 PDF 的文本提取，也称为信息提取，以及对不同语料库形成一些事实和观点的快速方法。今天的文章深入探讨了自然语言处理（NLP）领域，即计算机理解人类语言的能力。

# 信息提取

**信息提取（IE）**，如 Jurafsky 等人所定义，是*“将文本中嵌入的非结构化信息转化为结构化数据的过程。”* [1]。一种非常快速的信息提取方法不仅是搜索一个词是否出现在文本中，还包括计算该词出现的*频率*。这一点基于这样一种假设：**一个词在文本中出现的次数越多，越**…
