# GPT 模型如何运作

> 原文：[`towardsdatascience.com/how-gpt-models-work-b5f4517d5b5?source=collection_archive---------0-----------------------#2023-05-20`](https://towardsdatascience.com/how-gpt-models-work-b5f4517d5b5?source=collection_archive---------0-----------------------#2023-05-20)

## 学习 OpenAI GPT 模型背后的核心概念

[](https://medium.com/@bea_684?source=post_page-----b5f4517d5b5--------------------------------)![Beatriz Stollnitz](https://medium.com/@bea_684?source=post_page-----b5f4517d5b5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5f4517d5b5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5f4517d5b5--------------------------------) [Beatriz Stollnitz](https://medium.com/@bea_684?source=post_page-----b5f4517d5b5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1c8863892480&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-gpt-models-work-b5f4517d5b5&user=Beatriz+Stollnitz&userId=1c8863892480&source=post_page-1c8863892480----b5f4517d5b5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5f4517d5b5--------------------------------) · 14 分钟阅读 · 2023 年 5 月 20 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb5f4517d5b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-gpt-models-work-b5f4517d5b5&user=Beatriz+Stollnitz&userId=1c8863892480&source=-----b5f4517d5b5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb5f4517d5b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-gpt-models-work-b5f4517d5b5&source=-----b5f4517d5b5---------------------bookmark_footer-----------)![](img/3aacc19848583f3211780a4e9883415e.png)

图片由 [KOMMERS](https://unsplash.com/@kommers?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## **介绍**

我在 2021 年首次使用 GPT 模型编写了几行代码，那一刻我意识到文本生成技术已达到一个**拐点**。在此之前，我在研究生期间从零开始编写过语言模型，并且有使用其他文本生成系统的经验，因此我知道让它们产生有用结果是多么困难。我很幸运能在 Azure OpenAI Service 发布的公告中获得 GPT-3 的早期访问权限，并在其发布前进行试用。我让 GPT-3 总结了一份长文档，并尝试了少量示例提示。我看到结果比以前的模型要先进得多，这让我对这项技术感到兴奋，并渴望了解其实现方式。现在，后续的 GPT-3.5、ChatGPT 和 GPT-4 模型正在迅速获得广泛采用，领域内更多的人也对它们的工作原理感到好奇。虽然它们的内部工作细节是专有且复杂的，但所有 GPT 模型共享一些基础的理念，这些理念并不难以理解。我为这篇文章设定的目标是解释语言模型的核心概念，尤其是 GPT 模型的核心概念，解释内容针对数据科学家和机器学习…
