# 初学者的统计独立性

> 原文：[https://towardsdatascience.com/statistical-independence-for-beginners-69534ce04641?source=collection_archive---------10-----------------------#2023-03-21](https://towardsdatascience.com/statistical-independence-for-beginners-69534ce04641?source=collection_archive---------10-----------------------#2023-03-21)

## 使用 R 和 Excel 函数的直观解释

[](https://medium.com/@jaekim8080?source=post_page-----69534ce04641--------------------------------)[![Jae Kim](../Images/34716958ecfe8c0540f5cf5c1640d587.png)](https://medium.com/@jaekim8080?source=post_page-----69534ce04641--------------------------------)[](https://towardsdatascience.com/?source=post_page-----69534ce04641--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----69534ce04641--------------------------------) [Jae Kim](https://medium.com/@jaekim8080?source=post_page-----69534ce04641--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a7641c3f8c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstatistical-independence-for-beginners-69534ce04641&user=Jae+Kim&userId=3a7641c3f8c1&source=post_page-3a7641c3f8c1----69534ce04641---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----69534ce04641--------------------------------) ·5分钟阅读·2023年3月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F69534ce04641&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstatistical-independence-for-beginners-69534ce04641&user=Jae+Kim&userId=3a7641c3f8c1&source=-----69534ce04641---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F69534ce04641&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstatistical-independence-for-beginners-69534ce04641&source=-----69534ce04641---------------------bookmark_footer-----------)![](../Images/56e9420c704c0951aa30f3ab824e4e53.png)

图片由 [Naser Tamimi](https://unsplash.com/ja/@tamiminaser?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

统计独立性是统计学中的一个基本概念。它是许多（监督学习）机器学习算法的基本假设的一部分，例如逻辑回归模型和朴素贝叶斯分类器。它还与人工智能中的关键方法密切相关，如最大熵和神经网络。例如，参见[这篇文章](https://torstenvolk.medium.com/the-next-frontier-in-ai-replacing-statistical-independence-with-human-intuition-a39789f8c737)以获取进一步的见解。

在这篇文章中，我用直观的解释、示例和统计独立性检验的资源（R 代码和 Excel 函数）来解释其定义。

# **Probabilities**

为了简单起见，考虑两个事件 A 和 B，并定义以下概率：

**Prob(A)**：（边际）事件 A 的概率

**Prob(B)**：（边际）事件 B 的概率

**Prob(A ∩ B)**：事件 A 和 B 的联合概率，即 A 和 B 同时发生的概率；

**Prob(A|B)**：在给定 B 的情况下，事件 A 的条件概率，即在事件 B 已经发生的情况下 A 的概率；

**Prob(B|A)**：在给定 A 的情况下，事件 B 的条件概率。
