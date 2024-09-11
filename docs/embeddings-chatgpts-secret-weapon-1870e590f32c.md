# 嵌入技术：ChatGPT 的终极武器

> 原文：[https://towardsdatascience.com/embeddings-chatgpts-secret-weapon-1870e590f32c?source=collection_archive---------1-----------------------#2023-03-06](https://towardsdatascience.com/embeddings-chatgpts-secret-weapon-1870e590f32c?source=collection_archive---------1-----------------------#2023-03-06)

## 嵌入技术，以及它们如何帮助 ChatGPT 预测下一个词

[](https://emmaccode.medium.com/?source=post_page-----1870e590f32c--------------------------------)[![艾玛·布德罗](../Images/f7201d012b733643d6e97957f73fd1fa.png)](https://emmaccode.medium.com/?source=post_page-----1870e590f32c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1870e590f32c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1870e590f32c--------------------------------) [艾玛·布德罗](https://emmaccode.medium.com/?source=post_page-----1870e590f32c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fea170050148c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fembeddings-chatgpts-secret-weapon-1870e590f32c&user=Emma+Boudreau&userId=ea170050148c&source=post_page-ea170050148c----1870e590f32c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1870e590f32c--------------------------------) ·5分钟阅读·2023年3月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1870e590f32c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fembeddings-chatgpts-secret-weapon-1870e590f32c&user=Emma+Boudreau&userId=ea170050148c&source=-----1870e590f32c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1870e590f32c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fembeddings-chatgpts-secret-weapon-1870e590f32c&source=-----1870e590f32c---------------------bookmark_footer-----------)![](../Images/b73140f2b26fd1fb712cffe14b2f354e.png)

(图片由作者提供)

## 转换器和注意力机制

如果你最近经常浏览网络或阅读技术新闻，你可能在某个时刻听说过或读到过有关 ChatGPT 的信息。 ChatGPT 是 OpenAI 的新款 **语言 transformer** 模型，就这些模型而言，这是一个相当准确的模型，产生了一些非常引人注目的——有时甚至是病毒式的——结果。 在这个上下文中，**transformer** 指的是一种采用 **自注意力** 的机器学习模型。 **自注意力** 是一个数据科学术语，简单来说，这个模型尝试模拟人类的认知功能，或人类的认知注意力。 这个模型的 ***语言*** 部分也很重要，因为这描述了 transformer 希望预测的内容，即人类语言。这通常被称为 **自然语言处理**，或 **NLP**，尽管 NLP 通常指的是对语言数据进行处理，以将其转化为计算机或神经网络可以理解的数值权重。

Transformers 具有许多重要特性，这些特性使得在讨论像 ChatGPT 这样的语言 transformer 时，记住它们的定义非常重要。 transformers 和更广泛的 attention 模型通常具有的一些特性包括…
