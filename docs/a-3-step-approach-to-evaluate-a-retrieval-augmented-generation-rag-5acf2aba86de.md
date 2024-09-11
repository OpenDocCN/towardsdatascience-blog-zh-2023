# 评估检索增强生成（RAG）的三步法

> 原文：[`towardsdatascience.com/a-3-step-approach-to-evaluate-a-retrieval-augmented-generation-rag-5acf2aba86de?source=collection_archive---------2-----------------------#2023-11-23`](https://towardsdatascience.com/a-3-step-approach-to-evaluate-a-retrieval-augmented-generation-rag-5acf2aba86de?source=collection_archive---------2-----------------------#2023-11-23)

## 停止随意选择 RAG 的参数

[](https://ahmedbesbes.medium.com/?source=post_page-----5acf2aba86de--------------------------------)![Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----5acf2aba86de--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5acf2aba86de--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5acf2aba86de--------------------------------) [Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----5acf2aba86de--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fadc8ea174c69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-3-step-approach-to-evaluate-a-retrieval-augmented-generation-rag-5acf2aba86de&user=Ahmed+Besbes&userId=adc8ea174c69&source=post_page-adc8ea174c69----5acf2aba86de---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5acf2aba86de--------------------------------) ·9 分钟阅读·2023 年 11 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5acf2aba86de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-3-step-approach-to-evaluate-a-retrieval-augmented-generation-rag-5acf2aba86de&user=Ahmed+Besbes&userId=adc8ea174c69&source=-----5acf2aba86de---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5acf2aba86de&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-3-step-approach-to-evaluate-a-retrieval-augmented-generation-rag-5acf2aba86de&source=-----5acf2aba86de---------------------bookmark_footer-----------)![](img/3398735846db1df7203b650519c9909b.png)

图片由 [Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

调整你的 RAG 以获得最佳性能需要时间，因为这取决于各种相互依赖的参数：**块大小、重叠、检索的前 K 篇文档、嵌入模型、LLM 等。**

最佳组合通常取决于你的数据和用例：你不能仅仅套用上一个项目中的设置并期望获得相同的结果。

大多数人没有正确处理这个问题，而是几乎随意地选择参数。虽然有些人对这种方法感到满意，但我决定从数字上解决这个问题。

**这就是评估你的 RAG 的关键所在。**

> 在***这篇文章中，我将向你展示一个快速的三步法，你可以用它来高效且迅速地评估你的 RAGs，适用于两个任务。***

1.  **检索**

1.  **生成**

通过掌握这个评估流程，你可以迭代，进行多次实验，使用指标进行比较，并希望找到最佳配置。

让我们看看这如何运作 👇。

*附注：在每个部分中，提供了代码片段以帮助你开始实施这些想法。*
