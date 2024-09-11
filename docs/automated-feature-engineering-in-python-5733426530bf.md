# Python中的自动特征工程

> 原文：[https://towardsdatascience.com/automated-feature-engineering-in-python-5733426530bf?source=collection_archive---------2-----------------------#2023-05-02](https://towardsdatascience.com/automated-feature-engineering-in-python-5733426530bf?source=collection_archive---------2-----------------------#2023-05-02)

## 机器学习 | Python | 数据科学

## 使用Upgini增强数据集的特征指南

[](https://david-farrugia.medium.com/?source=post_page-----5733426530bf--------------------------------)[![David Farrugia](../Images/082ed61e24c7c26a4ae1c77343a87824.png)](https://david-farrugia.medium.com/?source=post_page-----5733426530bf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5733426530bf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5733426530bf--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----5733426530bf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916826092a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomated-feature-engineering-in-python-5733426530bf&user=David+Farrugia&userId=3916826092a6&source=post_page-3916826092a6----5733426530bf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5733426530bf--------------------------------) ·5分钟阅读·2023年5月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5733426530bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomated-feature-engineering-in-python-5733426530bf&user=David+Farrugia&userId=3916826092a6&source=-----5733426530bf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5733426530bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomated-feature-engineering-in-python-5733426530bf&source=-----5733426530bf---------------------bookmark_footer-----------)![](../Images/9bef5b09a779382f78f011904ab77b6f.png)

图片来源：[Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据科学家或机器学习专业人士最重要的技能之一是能够从任何给定的数据集中提取更深层次和更有意义的特征。这个概念，更常被称为特征工程，可能是建模机器学习算法时最强大的技术之一。

从数据中学习涉及大量的工程工作。尽管现代高级工具如sklearn已经将大多数复杂性抽象化，但仍然需要充分理解数据，并将其塑造成你想要解决的问题。

提取更好的特征有助于为模型提供关于业务领域及其影响因素的额外（且可能更强的）潜在关系。

不用多说，特征工程是非常耗时和繁琐的。它需要大量的创造力、技术专长，并且在大多数情况下，还需要反复试验。

我最近遇到了一款新工具，Upgini。符合当前大型语言模型的趋势……
