# Google 生成性 AI 转型

> 原文：[https://towardsdatascience.com/google-generative-ai-transformations-edb4164935cb?source=collection_archive---------10-----------------------#2023-06-12](https://towardsdatascience.com/google-generative-ai-transformations-edb4164935cb?source=collection_archive---------10-----------------------#2023-06-12)

## ETL 即将被转型

[](https://franklyai.medium.com/?source=post_page-----edb4164935cb--------------------------------)[![Frank Neugebauer](../Images/0da70d082d0f9c7ad8ccf574ed215df2.png)](https://franklyai.medium.com/?source=post_page-----edb4164935cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----edb4164935cb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----edb4164935cb--------------------------------) [Frank Neugebauer](https://franklyai.medium.com/?source=post_page-----edb4164935cb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F29e3b5503cd1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-generative-ai-transformations-edb4164935cb&user=Frank+Neugebauer&userId=29e3b5503cd1&source=post_page-29e3b5503cd1----edb4164935cb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----edb4164935cb--------------------------------) ·8 分钟阅读·2023年6月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fedb4164935cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-generative-ai-transformations-edb4164935cb&user=Frank+Neugebauer&userId=29e3b5503cd1&source=-----edb4164935cb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fedb4164935cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgoogle-generative-ai-transformations-edb4164935cb&source=-----edb4164935cb---------------------bookmark_footer-----------)![](../Images/dc5dd2d38e77ff28d92f5c611af6d217.png)

图片由 [Suzanne D. Williams](https://unsplash.com/fr/@scw1217?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 这是什么？

大型语言模型（LLMs）不仅可以提取信息和生成信息，还可以**转型**信息，使得提取、转型和加载（ETL）成为完全不同的工作。我将提供一个示例来说明这些观点，这也将展示 LLMs 如何以及为何应该用于许多相关任务，包括将非结构化文本转变为结构化文本。

谷歌最近在预览中公开了其大型语言模型（LLM）套件，并将部分产品品牌命名为“生成性 AI 工作室”（Generative AI Studio）。简而言之，Google Cloud Platform Console 中的 GenAI Studio 是 Google 的 LLM 的用户界面。不过，与 Google Bard（这是一个使用 LLM 的商业应用）不同，谷歌不会因为任何原因保存数据。请注意，谷歌还发布了一个 API，用于许多这里概述的功能。

# 使用 GenAI Studio

进入 GenAI Studio 非常简单——从 GCP 控制台开始，使用左侧的导航栏，将鼠标悬停在 Vertex AI 上，然后选择**概述**（**Overview**）下的**生成性 AI 工作室**（**GENERATIVE AI STUDIO**）。
