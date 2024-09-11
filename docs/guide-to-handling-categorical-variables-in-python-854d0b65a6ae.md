# 处理Python中的分类变量指南

> 原文：[https://towardsdatascience.com/guide-to-handling-categorical-variables-in-python-854d0b65a6ae?source=collection_archive---------10-----------------------#2023-06-16](https://towardsdatascience.com/guide-to-handling-categorical-variables-in-python-854d0b65a6ae?source=collection_archive---------10-----------------------#2023-06-16)

## *关于如何处理机器学习和数据科学中分类变量的指南*

[](https://medium.com/@theDrewDag?source=post_page-----854d0b65a6ae--------------------------------)[![安德烈亚·达戈斯蒂诺](../Images/58c7c218815f25278aae59cea44d8771.png)](https://medium.com/@theDrewDag?source=post_page-----854d0b65a6ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----854d0b65a6ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----854d0b65a6ae--------------------------------) [安德烈亚·达戈斯蒂诺](https://medium.com/@theDrewDag?source=post_page-----854d0b65a6ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fguide-to-handling-categorical-variables-in-python-854d0b65a6ae&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----854d0b65a6ae---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----854d0b65a6ae--------------------------------) ·13分钟阅读·2023年6月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F854d0b65a6ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fguide-to-handling-categorical-variables-in-python-854d0b65a6ae&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----854d0b65a6ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F854d0b65a6ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fguide-to-handling-categorical-variables-in-python-854d0b65a6ae&source=-----854d0b65a6ae---------------------bookmark_footer-----------)![](../Images/e99d8b89a83f713a851916579c9a484c.png)

照片由[托马斯·哈斯](https://unsplash.com/@thomashaas?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)拍摄 / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

在数据科学或机器学习项目中处理分类变量并非易事。这类工作需要对应用领域有深入的了解，以及对各种可用方法有广泛的理解。

因此，本文将重点解释以下概念

+   **什么是分类变量**以及如何将它们划分为不同的类型。

+   如何根据其类型将它们转换为**数值**。

+   主要使用**Sklearn**的工具和技术来管理这些变量。

对分类变量的正确处理可以大大提高我们预测模型或分析的结果。实际上，大多数与学习和理解数据相关的信息可能都包含在现有的分类变量中。

只需考虑表格数据，根据变量`gender`或某种`color`进行拆分。这些拆分依据类别数量，可以揭示组之间的显著差异，并且可以为分析师或学习算法提供信息。
