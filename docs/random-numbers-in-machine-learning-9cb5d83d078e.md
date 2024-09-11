# 机器学习中的随机数

> 原文：[`towardsdatascience.com/random-numbers-in-machine-learning-9cb5d83d078e?source=collection_archive---------4-----------------------#2023-10-20`](https://towardsdatascience.com/random-numbers-in-machine-learning-9cb5d83d078e?source=collection_archive---------4-----------------------#2023-10-20)

## 关于伪随机数、种子和可重复性

[](https://medium.com/@caroline.arnold_63207?source=post_page-----9cb5d83d078e--------------------------------)![Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----9cb5d83d078e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9cb5d83d078e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cb5d83d078e--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----9cb5d83d078e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9367198e7a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandom-numbers-in-machine-learning-9cb5d83d078e&user=Caroline+Arnold&userId=9367198e7a3c&source=post_page-9367198e7a3c----9cb5d83d078e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9cb5d83d078e--------------------------------) · 8 min read · 2023 年 10 月 20 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9cb5d83d078e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandom-numbers-in-machine-learning-9cb5d83d078e&user=Caroline+Arnold&userId=9367198e7a3c&source=-----9cb5d83d078e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9cb5d83d078e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandom-numbers-in-machine-learning-9cb5d83d078e&source=-----9cb5d83d078e---------------------bookmark_footer-----------)![](img/cede17b2b0c90191d192b48d7de7c688.png)

图片由 [Riho Kroll](https://unsplash.com/@rihok?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

机器学习依赖于统计学，而随机数对数据处理和模型训练流程中许多步骤的性能至关重要。现代机器学习框架提供了实现底层随机性的抽象和函数，而对于我们这些数据科学家和机器学习工程师来说，随机数生成的细节往往仍然不够明确。

在这篇文章中，我希望能够阐明机器学习中的随机数。你将阅读到以下内容：

+   随机数在机器学习中的 3 个示例

+   生成（伪）随机数

+   通过设定种子来固定随机数

+   可重复的机器学习：scikit-learn、tensorflow 和 pytorch 所需的代码行。

在本文结束时，你将了解在机器学习管道中使用随机数会发生什么，并学习确保机器学习算法可重复性的必要代码行。

## 随机数在机器学习中的 3 个示例
