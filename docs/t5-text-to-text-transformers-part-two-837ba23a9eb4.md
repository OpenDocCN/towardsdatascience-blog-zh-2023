# T5：文本到文本的转换器（第二部分）

> 原文：[https://towardsdatascience.com/t5-text-to-text-transformers-part-two-837ba23a9eb4?source=collection_archive---------12-----------------------#2023-07-05](https://towardsdatascience.com/t5-text-to-text-transformers-part-two-837ba23a9eb4?source=collection_archive---------12-----------------------#2023-07-05)

## 大型语言模型的最佳迁移学习

[](https://wolfecameron.medium.com/?source=post_page-----837ba23a9eb4--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----837ba23a9eb4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----837ba23a9eb4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----837ba23a9eb4--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----837ba23a9eb4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ft5-text-to-text-transformers-part-two-837ba23a9eb4&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----837ba23a9eb4---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----837ba23a9eb4--------------------------------) · 14分钟阅读 · 2023年7月5日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F837ba23a9eb4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ft5-text-to-text-transformers-part-two-837ba23a9eb4&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----837ba23a9eb4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F837ba23a9eb4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ft5-text-to-text-transformers-part-two-837ba23a9eb4&source=-----837ba23a9eb4---------------------bookmark_footer-----------)![](../Images/16eb0b06a8e38a4121e6c959124618c5.png)

（照片由[Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/s/photos/text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

[BERT](https://cameronrwolfe.substack.com/p/language-understanding-with-bert) [5]的提出促进了自然语言处理（NLP）中迁移学习方法的普及。由于互联网上大量未标记文本的广泛存在，我们可以轻松地*(i)* 在大量原始文本上预训练大型转换器模型，并*(ii)* 微调这些模型以准确解决下游任务。这种方法非常有效，但其新兴的普及也导致了许多替代方法和修改方案的提出。随着这些新方法的出现，人们不禁会想：*在NLP中进行迁移学习的最佳实践是什么？*

这个问题是通过统一的文本到文本转换器（T5）模型的分析来回答的。T5在预训练和微调期间都将所有任务重新表述为文本到文本的格式，这意味着模型接收文本输入并生成文本输出。利用这种统一格式，T5能够分析各种不同的迁移学习设置，从而可以比较多种方法。在[之前的通讯](https://cameronrwolfe.substack.com/p/t5-text-to-text-transformers-part)中，我们了解了T5模型的格式、架构和总体方法。

在本期通讯中，我们将概述T5所进行的分析，包括不同预训练的实证比较……
