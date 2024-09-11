# 如何从零构建一个 LLM

> 原文：[`towardsdatascience.com/how-to-build-an-llm-from-scratch-8c477768f1f9?source=collection_archive---------1-----------------------#2023-09-21`](https://towardsdatascience.com/how-to-build-an-llm-from-scratch-8c477768f1f9?source=collection_archive---------1-----------------------#2023-09-21)

## **数据整理、变换器、规模化训练与模型评估**

[](https://shawhin.medium.com/?source=post_page-----8c477768f1f9--------------------------------)![Shaw Talebi](https://shawhin.medium.com/?source=post_page-----8c477768f1f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8c477768f1f9--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----8c477768f1f9--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----8c477768f1f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-llm-from-scratch-8c477768f1f9&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----8c477768f1f9---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----8c477768f1f9--------------------------------) ·16 分钟阅读·2023 年 9 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8c477768f1f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-llm-from-scratch-8c477768f1f9&user=Shaw+Talebi&userId=f3998e1cd186&source=-----8c477768f1f9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8c477768f1f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-an-llm-from-scratch-8c477768f1f9&source=-----8c477768f1f9---------------------bookmark_footer-----------)

这是关于[实际使用大型语言模型](https://medium.com/towards-data-science/a-practical-introduction-to-llms-65194dda1148)（LLMs）系列文章中的第 6 篇。之前的文章探讨了如何通过[提示工程](https://medium.com/towards-data-science/prompt-engineering-how-to-trick-ai-into-solving-your-problems-7ce1ed3b553f)和[微调](https://medium.com/towards-data-science/fine-tuning-large-language-models-llms-23473d763b91)来利用预训练的 LLMs。虽然这些方法可以处理绝大多数 LLM 的使用案例，但在某些情况下，从零开始构建 LLM 也是有意义的。在本文中，我们将回顾基于 GPT-3、Llama、Falcon 等模型开发基础 LLM 的关键方面。

![](img/9e8157ee792c3765304fb0a7c4d15f82.png)

照片由[Frames For Your Heart](https://unsplash.com/@framesforyourheart?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上拍摄。

历史上（即不到 1 年前），训练大规模语言模型（10b+参数）是 AI 研究人员专属的神秘活动。然而，在 ChatGPT 后的所有 AI 和 LLM 热情中，现在我们有了一个环境，企业和其他组织有兴趣从头开始开发他们自己的定制 LLM [1]。尽管对于超过 99%的 LLM 应用来说这并不是必需的（依我看），但了解开发这些大规模模型的要求以及在何时构建它们仍然是有益的。
