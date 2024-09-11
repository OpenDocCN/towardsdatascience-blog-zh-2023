# 如何在 Spacy 3.5 中创建自定义 NER

> 原文：[`towardsdatascience.com/how-to-create-a-custom-ner-in-spacy-3-5-c9942aab3c91?source=collection_archive---------6-----------------------#2023-04-25`](https://towardsdatascience.com/how-to-create-a-custom-ner-in-spacy-3-5-c9942aab3c91?source=collection_archive---------6-----------------------#2023-04-25)

## 自然语言处理

## 关于如何从文本中提取自定义实体的快速教程

[](https://alod83.medium.com/?source=post_page-----c9942aab3c91--------------------------------)![Angelica Lo Duca](https://alod83.medium.com/?source=post_page-----c9942aab3c91--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c9942aab3c91--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c9942aab3c91--------------------------------) [Angelica Lo Duca](https://alod83.medium.com/?source=post_page-----c9942aab3c91--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8bc34d63aee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-custom-ner-in-spacy-3-5-c9942aab3c91&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=post_page-f8bc34d63aee----c9942aab3c91---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c9942aab3c91--------------------------------) · 5 分钟阅读 · 2023 年 4 月 25 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc9942aab3c91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-custom-ner-in-spacy-3-5-c9942aab3c91&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=-----c9942aab3c91---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc9942aab3c91&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-custom-ner-in-spacy-3-5-c9942aab3c91&source=-----c9942aab3c91---------------------bookmark_footer-----------)![](img/82aa2292c77d5ad1b40112b7c88ed4f5.png)

图片由 [Max Chen](https://unsplash.com/ja/@maxchen2k?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你是否厌倦了使用不完全适合你特定需求的通用命名实体识别（NER）模型？不要再找了！本文将引导你创建一个在 Spacy 3.5 中自定义的 NER 模型。

通过一些调整和训练数据，你可以获得一个能够准确识别特定领域或用例的模型。告别千篇一律的 NER 模型，迎接定制的精准度。让我们深入了解吧！

我们将讨论：

+   对 spaCy 及其竞争者的快速介绍

+   问题设定

+   生成一个训练集

+   生成和训练模型

+   测试你的模型。

# spaCy 及其竞争对手的简要介绍

如果这是你第一次听说 spaCy，请知道它是一个流行的开源库，用于 Python 中的自然语言处理（NLP）。它提供了高效和快速的 NLP 功能，如分词、词性标注、实体识别、依存分析等。SpaCy 的主要优势在于其...
