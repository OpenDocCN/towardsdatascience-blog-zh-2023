# 合成数据实用指南

> 原文：[https://towardsdatascience.com/the-synthetic-data-field-guide-f1fc59e2d178?source=collection_archive---------6-----------------------#2023-06-30](https://towardsdatascience.com/the-synthetic-data-field-guide-f1fc59e2d178?source=collection_archive---------6-----------------------#2023-06-30)

## 使数据更有用

## 关于各种虚假数据的指南

[](https://kozyrkov.medium.com/?source=post_page-----f1fc59e2d178--------------------------------)[![Cassie Kozyrkov](../Images/ad18dd12979a4a3ec130bdf8b889af23.png)](https://kozyrkov.medium.com/?source=post_page-----f1fc59e2d178--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f1fc59e2d178--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f1fc59e2d178--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----f1fc59e2d178--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fccb851bb5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-synthetic-data-field-guide-f1fc59e2d178&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=post_page-2fccb851bb5e----f1fc59e2d178---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1fc59e2d178--------------------------------) ·6分钟阅读·2023年6月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff1fc59e2d178&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-synthetic-data-field-guide-f1fc59e2d178&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=-----f1fc59e2d178---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff1fc59e2d178&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-synthetic-data-field-guide-f1fc59e2d178&source=-----f1fc59e2d178---------------------bookmark_footer-----------)

如果你想获取一些[数据](http://bit.ly/quaesita_data)，你有什么选择？最简单的回答是：你可以获得真实数据或虚假数据。

在[我之前的文章](https://bit.ly/quaesita_synthguide1)中，我们熟悉了合成数据的概念，并讨论了创建它的思路。我们比较了真实数据、噪声数据和手工制作的数据。让我们深入探讨比让人类挑选一个数字更复杂的合成数据种类……

*(注意：这篇文章中的链接会带你去同一作者的解释文章。)*

## 复制的数据

也许你测量了10,000个真实的人体身高，但你想要20,000个数据点。你可以采取的一个方法是假设你现有的数据集已经很好地代表了你的总体。（[*假设*](http://bit.ly/quaesita_saddest)总是有风险的，请谨慎操作。）然后你可以简单地复制数据集或复制其中的一部分，使用老式的复制粘贴。瞧！更多数据！但这些数据是*好*的和*有用*的吗？这总是取决于你需要它做什么。在大多数情况下，答案是否定的。不过，当然，仍然有...
