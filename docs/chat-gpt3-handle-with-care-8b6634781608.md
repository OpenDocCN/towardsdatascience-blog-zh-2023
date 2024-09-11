# ChatGPT — 小心使用

> 原文：[`towardsdatascience.com/chat-gpt3-handle-with-care-8b6634781608?source=collection_archive---------15-----------------------#2023-03-13`](https://towardsdatascience.com/chat-gpt3-handle-with-care-8b6634781608?source=collection_archive---------15-----------------------#2023-03-13)

## 超越炒作 — 理解 ChatGPT 能做什么以及不能做什么，对于充分利用这项技术至关重要。香港大学人工智能研究中心最近的一篇研究论文评估了 OpenAI 算法的局限性和优势。

[](https://misclassified.medium.com/?source=post_page-----8b6634781608--------------------------------)![Giovanni Bruner](https://misclassified.medium.com/?source=post_page-----8b6634781608--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b6634781608--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b6634781608--------------------------------) [Giovanni Bruner](https://misclassified.medium.com/?source=post_page-----8b6634781608--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F14f159c7c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchat-gpt3-handle-with-care-8b6634781608&user=Giovanni+Bruner&userId=14f159c7c0&source=post_page-14f159c7c0----8b6634781608---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b6634781608--------------------------------) ·6 分钟阅读·2023 年 3 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8b6634781608&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchat-gpt3-handle-with-care-8b6634781608&user=Giovanni+Bruner&userId=14f159c7c0&source=-----8b6634781608---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8b6634781608&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fchat-gpt3-handle-with-care-8b6634781608&source=-----8b6634781608---------------------bookmark_footer-----------)![](img/ffc519eec3fa34c4ea5ccb1ca9d769bb.png)

图片由 [Possessed Photography](https://unsplash.com/@possessedphotography?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

首先，语言模型出现了。直觉上很简单：一个词序列中的下一个词可以通过概率分布来建模，并且严重依赖于前面的词。词汇是一个有限的语料库的一部分（英语词汇表中有 170,000 个词元）。每个词都有有限的含义。一个词序列遵循一个缓慢变化的内部元数据集：语法。这是一种可预测的结构。你可以预期动词后面跟着名词，而不是另一个动词。语法和意义作为约束，限制了下一个词预测的随机性。这可以说比预测一千家公司下一天的股价更简单。而且，语言模型本质上是自回归的，下一个词的预测依赖于前面的词，并且需要考虑的潜在、不可观测的变量并不多。
