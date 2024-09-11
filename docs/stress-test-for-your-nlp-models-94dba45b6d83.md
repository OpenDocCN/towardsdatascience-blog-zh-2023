# 给你的 NLP 模型做压力测试

> 原文：[https://towardsdatascience.com/stress-test-for-your-nlp-models-94dba45b6d83?source=collection_archive---------14-----------------------#2023-01-30](https://towardsdatascience.com/stress-test-for-your-nlp-models-94dba45b6d83?source=collection_archive---------14-----------------------#2023-01-30)

## 指标并未显示 NLP 模型的实际表现。了解如何彻底测试你的模型并修复标注伪影。

[](https://prosperous-silver-alpaca.medium.com/?source=post_page-----94dba45b6d83--------------------------------)[![Alexander Biryukov](../Images/ad35544b7be340297c1676df19436416.png)](https://prosperous-silver-alpaca.medium.com/?source=post_page-----94dba45b6d83--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94dba45b6d83--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----94dba45b6d83--------------------------------) [Alexander Biryukov](https://prosperous-silver-alpaca.medium.com/?source=post_page-----94dba45b6d83--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6325a625339&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstress-test-for-your-nlp-models-94dba45b6d83&user=Alexander+Biryukov&userId=a6325a625339&source=post_page-a6325a625339----94dba45b6d83---------------------post_header-----------) 在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----94dba45b6d83--------------------------------) 发布 · 9分钟阅读 · 2023年1月30日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F94dba45b6d83&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstress-test-for-your-nlp-models-94dba45b6d83&user=Alexander+Biryukov&userId=a6325a625339&source=-----94dba45b6d83---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F94dba45b6d83&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstress-test-for-your-nlp-models-94dba45b6d83&source=-----94dba45b6d83---------------------bookmark_footer-----------)

数据集伪影是自然语言处理（NLP）中影响模型在实际世界中表现的问题之一。尽管预训练模型在基准数据集上表现良好，但在其他设置中表现却很差。 *这些失败是由于数据集伪影或标注伪影* —— *即语言模型在训练过程中学到的虚假关联*[1]*。

![](../Images/63a1dfef8ca9af14ff29d43f75aaff89.png)

图片来源：[Nathan Dumlao](https://unsplash.com/ko/@nate_dumlao?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

结果显示，由于数据集的特殊性和标注者带来的偏差，你的模型在训练过程中可能会吸收大量的伪影。伪影为什么不好？基本上，它们为你的模型提供了记住一些实际上是错误的因果关系的捷径，而不是学习正确的“推理”。例如，如果一个数据集中包含许多男性角色是医生的例子，那么在进行推断时，模型会给男性成为医生的概率更高而非女性。也有可能构造一些对抗性示例，使模型表现出令人惊讶的低性能。

作为数据科学家的你的工作是尽可能消除这些伪影，并提高整体…
