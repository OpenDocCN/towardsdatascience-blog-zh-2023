# 如何使用 Python、NLTK 和一些简单统计方法进行语言检测

> 原文：[https://towardsdatascience.com/how-to-do-language-detection-using-python-nltk-and-some-easy-statistics-6cec9a02148?source=collection_archive---------6-----------------------#2023-01-11](https://towardsdatascience.com/how-to-do-language-detection-using-python-nltk-and-some-easy-statistics-6cec9a02148?source=collection_archive---------6-----------------------#2023-01-11)

## 这是一个你每天使用的技术的实际介绍。

[](https://katherineamunro.medium.com/?source=post_page-----6cec9a02148--------------------------------)[![凯瑟琳·穆恩罗](../Images/8013140495c7b9bd25ef08d712f097bf.png)](https://katherineamunro.medium.com/?source=post_page-----6cec9a02148--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6cec9a02148--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6cec9a02148--------------------------------) [凯瑟琳·穆恩罗](https://katherineamunro.medium.com/?source=post_page-----6cec9a02148--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb84716d39740&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-do-language-detection-using-python-nltk-and-some-easy-statistics-6cec9a02148&user=Katherine+Munro&userId=b84716d39740&source=post_page-b84716d39740----6cec9a02148---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6cec9a02148--------------------------------) ·12 min read·2023年1月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6cec9a02148&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-do-language-detection-using-python-nltk-and-some-easy-statistics-6cec9a02148&user=Katherine+Munro&userId=b84716d39740&source=-----6cec9a02148---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6cec9a02148&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-do-language-detection-using-python-nltk-and-some-easy-statistics-6cec9a02148&source=-----6cec9a02148---------------------bookmark_footer-----------)![](../Images/645352f0855da593110591ce2a9202a4.png)

图片由 [埃蒂安·吉拉尔德](https://unsplash.com/@etiennegirardet) 提供，来源于 [Unsplash](https://unsplash.com)

你是否想过谷歌翻译的“检测语言”功能是如何工作的？当然你没有想过，你有更重要的事情要做。但我去找了一下，却找不到答案（尽管[我实际上已经写了一本书](https://www.amazon.com/Handbook-Data-Science-AI-Analytics/dp/1569908869)关于自然语言处理（NLP））。这是谷歌的秘密武器。所以今天，我将向你展示一个非常简单的方法来进行语言检测，使用一种被高度低估的NLP工具和一些非常简单的数学。你很快就可以把它添加到你的GitHub作品集中。

## **什么是语言检测，为什么要使用它？**

语言检测只是意味着识别输入文本的语言。这是自然语言处理中许多任务的第一步，包括你每天使用的许多任务：

+   拼写和语法纠正（比如微软Word，谷歌文档等）

+   下一个词预测（你的手机一直在做这件事！）

+   机器翻译（例如谷歌翻译的“检测语言”选项）

## **我们如何检测一种语言？**
