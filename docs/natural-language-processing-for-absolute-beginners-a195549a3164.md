# 自然语言处理入门

> 原文：[https://towardsdatascience.com/natural-language-processing-for-absolute-beginners-a195549a3164?source=collection_archive---------3-----------------------#2023-09-02](https://towardsdatascience.com/natural-language-processing-for-absolute-beginners-a195549a3164?source=collection_archive---------3-----------------------#2023-09-02)

## 在 10 行 Python 代码中解决复杂的 NLP 任务

[](https://dmitryelj.medium.com/?source=post_page-----a195549a3164--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----a195549a3164--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a195549a3164--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a195549a3164--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----a195549a3164--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-language-processing-for-absolute-beginners-a195549a3164&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----a195549a3164---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a195549a3164--------------------------------) ·9 分钟阅读·2023年9月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa195549a3164&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-language-processing-for-absolute-beginners-a195549a3164&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----a195549a3164---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa195549a3164&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-language-processing-for-absolute-beginners-a195549a3164&source=-----a195549a3164---------------------bookmark_footer-----------)![](../Images/bc90b1586db7d9db3689cfb34615b58a.png)

作者提供的图片（使用 Craiyon 生成）

NLP（自然语言处理）确实是计算机科学中的一个复杂领域。像 SpaCy 或 NLTK 这样的框架庞大且通常需要一些学习。但是借助开源的大型语言模型（LLMs）和现代 Python 库，许多任务可以更轻松地解决。更进一步的是，仅仅几年前只能在科学论文中看到的结果，现在可以用 10 行 Python 代码实现。

言归正传，我们开始吧。

## 1\. 语言翻译

你是否曾经想过 Google Translate 是如何工作的？Google [正在使用](https://blog.research.google/2020/06/recent-advances-in-google-translate.html)一个在大量文本上训练的深度学习模型。现在，在 [Transformers 库](https://huggingface.co/docs/transformers/index) 的帮助下，这不仅可以在 Google Labs 中完成，也可以在普通 PC 上进行。在这个示例中，我将使用一个预训练的 [T5-base](https://huggingface.co/t5-base)（文本到文本转换器）模型。这个模型首先在原始文本数据上进行训练，然后在源-目标对上进行微调，比如（“translate English to German: the house is wonderful”，“Das Haus ist Wunderbar”）。在这里，“translate English to German” 是一个前缀，它“告诉”模型需要做什么，而这些短语则是模型应该学习的实际上下文。
