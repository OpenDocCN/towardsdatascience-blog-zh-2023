# 快速评估你的 RAG，无需手动标记测试数据

> 原文：[`towardsdatascience.com/quickly-evaluate-your-rag-without-manually-labeling-test-data-43ade0ae187a?source=collection_archive---------6-----------------------#2023-12-21`](https://towardsdatascience.com/quickly-evaluate-your-rag-without-manually-labeling-test-data-43ade0ae187a?source=collection_archive---------6-----------------------#2023-12-21)

## 自动化评估过程，无需任何人工干预

[](https://ahmedbesbes.medium.com/?source=post_page-----43ade0ae187a--------------------------------)![Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----43ade0ae187a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----43ade0ae187a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----43ade0ae187a--------------------------------) [Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----43ade0ae187a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fadc8ea174c69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquickly-evaluate-your-rag-without-manually-labeling-test-data-43ade0ae187a&user=Ahmed+Besbes&userId=adc8ea174c69&source=post_page-adc8ea174c69----43ade0ae187a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----43ade0ae187a--------------------------------) ·12 分钟阅读·2023 年 12 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F43ade0ae187a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquickly-evaluate-your-rag-without-manually-labeling-test-data-43ade0ae187a&user=Ahmed+Besbes&userId=adc8ea174c69&source=-----43ade0ae187a---------------------clap_footer-----------)

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F43ade0ae187a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fquickly-evaluate-your-rag-without-manually-labeling-test-data-43ade0ae187a&source=-----43ade0ae187a---------------------bookmark_footer-----------)![](img/d058a92e9619f69812d1ccfc71bcb537.png)

用户生成的图像

今天的话题是如何在不手动标记测试数据的情况下评估你的 RAG。

测量你的 RAG 性能是你应该关注的事情，特别是当你在构建这些系统并将其投入生产时。

除了让你对应用程序的行为有一个大致的了解外，评估你的 RAG 还提供了定量反馈，这些反馈指导实验和参数的适当选择（如 LLMs、嵌入模型、块大小、Top K 等）

评估你的 RAG 对你的客户或利益相关者也很重要，因为他们***总是***期望性能指标来验证你的项目。

不再拗口，这个问题涵盖了以下内容：

1.  从你的 RAG 数据中自动生成合成测试集

1.  流行的 RAG 指标概述

1.  使用 Ragas 包在合成数据集上计算 RAG 指标

*附注：本问题的一些部分有点实际操作性质。它们包括了实现数据集生成和评估 RAG 所需的编码材料*…
