# 加载大型语言模型的介绍

> 原文：[`towardsdatascience.com/an-introduction-to-loading-large-language-models-e7488a7352ed?source=collection_archive---------9-----------------------#2023-10-12`](https://towardsdatascience.com/an-introduction-to-loading-large-language-models-e7488a7352ed?source=collection_archive---------9-----------------------#2023-10-12)

## 掌握 Megamodels：加载 Llama2 和 HuggingFace 的大型语言模型的入门指南

![Amber Teng](https://medium.com/@angelamarieteng?source=post_page-----e7488a7352ed--------------------------------) [Amber Teng](https://medium.com/@angelamarieteng?source=post_page-----e7488a7352ed--------------------------------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7488a7352ed--------------------------------)

·

[跟进](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2a58d8e73e5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-loading-large-language-models-e7488a7352ed&user=Amber+Teng&userId=2a58d8e73e5a&source=post_page-2a58d8e73e5a----e7488a7352ed---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7488a7352ed--------------------------------) ·15 分钟阅读·2023 年 10 月 12 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe7488a7352ed&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-loading-large-language-models-e7488a7352ed&source=-----e7488a7352ed---------------------bookmark_footer-----------)![](img/ab1b2fdbc7396ec29bedfbd8c64ed35f.png)

[Possessed Photography](https://unsplash.com/@possessedphotography?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 的照片在 [Unsplash](https://unsplash.com/photos/jIBMSMs4_kA?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 上

在人工智能巨头的时代，经过数十亿个参数和数 TB 数据训练的模型主导了领域，自然语言处理变得更加容易接触——不仅仅是工程师、数据科学家和机器学习研究人员，还有爱好者、商人和学生。我们正站在技术革命的十字路口——由庞大的语言模型驱动。

这场革命不仅影响了我们中的少数人，*而是所有人*。因此，掌握这些大型语言模型（LLMs）的理解及其能力变得越来越重要，同时也需要了解如何使用这些 LLM。*那么，为什么工程师需要理解如何加载这些 LLM？*

这些新的大型语言模型（LLMs）几乎渗透到今天科技领域的各个方面——数据科学家和自然语言处理（NLP）工程师越来越被要求将 LLM 驱动的解决方案集成到他们的产品和系统中，无论是在学术界还是工业界。显然，对 LLM 的基本理解对于做出明智的决策至关重要……
