# 自然语言基础——情感分析、机器翻译和命名实体识别的入门与语言模型实现

> 原文：[https://towardsdatascience.com/natural-language-fundamentals-intro-implementation-of-sentiment-analysis-machine-translation-c79662e52624?source=collection_archive---------17-----------------------#2023-03-20](https://towardsdatascience.com/natural-language-fundamentals-intro-implementation-of-sentiment-analysis-machine-translation-c79662e52624?source=collection_archive---------17-----------------------#2023-03-20)

## 自然语言领域中的多语言建模

[](https://medium.com/@fmnobar?source=post_page-----c79662e52624--------------------------------)[![Farzad Mahmoodinobar](../Images/2d75209693b712300e6f0796bd2487d0.png)](https://medium.com/@fmnobar?source=post_page-----c79662e52624--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c79662e52624--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c79662e52624--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----c79662e52624--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-language-fundamentals-intro-implementation-of-sentiment-analysis-machine-translation-c79662e52624&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----c79662e52624---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c79662e52624--------------------------------) ·9 分钟阅读·2023年3月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc79662e52624&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-language-fundamentals-intro-implementation-of-sentiment-analysis-machine-translation-c79662e52624&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----c79662e52624---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc79662e52624&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnatural-language-fundamentals-intro-implementation-of-sentiment-analysis-machine-translation-c79662e52624&source=-----c79662e52624---------------------bookmark_footer-----------)![](../Images/93e4900e458bc59ccf6ab8f6b69ece8c.png)

照片由 [RetroSupply](https://unsplash.com/@retrosupply) 提供，来自 [Unsplash](https://unsplash.com/photos/kZvmEpyfiJs)

我们人类使用词语、声音、手势和符号来以不同形式（如言语、书写和标志）传达复杂的概念和抽象信息。为了利用这些强大的计算机，我们必须找到让计算机理解人类交流和现有知识库的方法。因此，自然语言处理（NLP）、理解（NLU）和生成（NLG）这三个人工智能领域应运而生。这三个领域的边界有时不太清晰，而整体自然语言空间涵盖了当今计算机和数据科学世界的各种应用。最常见的应用包括（I）情感分析，（II）机器翻译和（III）命名实体识别（NER），我们将在本文中对这些进行定义和实现。
