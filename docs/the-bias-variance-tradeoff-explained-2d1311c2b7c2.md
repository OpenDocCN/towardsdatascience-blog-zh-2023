# 偏差-方差权衡，解读

> 原文：[`towardsdatascience.com/the-bias-variance-tradeoff-explained-2d1311c2b7c2?source=collection_archive---------5-----------------------#2023-02-15`](https://towardsdatascience.com/the-bias-variance-tradeoff-explained-2d1311c2b7c2?source=collection_archive---------5-----------------------#2023-02-15)

## 不拘一格的解密者

## 偏差-方差权衡，第三部分，共三部分

[](https://kozyrkov.medium.com/?source=post_page-----2d1311c2b7c2--------------------------------)![Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----2d1311c2b7c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d1311c2b7c2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d1311c2b7c2--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----2d1311c2b7c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fccb851bb5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-bias-variance-tradeoff-explained-2d1311c2b7c2&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=post_page-2fccb851bb5e----2d1311c2b7c2---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d1311c2b7c2--------------------------------) ·4 分钟阅读·2023 年 2 月 15 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2d1311c2b7c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-bias-variance-tradeoff-explained-2d1311c2b7c2&source=-----2d1311c2b7c2---------------------bookmark_footer-----------)

在本系列的[第一部分](http://bit.ly/quaesita_bivar1)和[第二部分](http://bit.ly/quaesita_bivar2)中，我们覆盖了许多内容。[第一部分](http://bit.ly/quaesita_bivar1)是开胃菜，我们讲解了你在理解偏差-方差权衡过程中需要知道的一些基础知识。[第二部分](http://bit.ly/quaesita_bivar2)是丰盛的主菜，我们详细讨论了过拟合、欠拟合和正则化等概念。

吃蔬菜是个很好的主意，所以在继续阅读本部分之前，请务必回顾一下之前的文章，因为第三部分是甜点：你通过跟随逻辑所获得的总结。

![](img/2d5041225c864fe2f6084d3983c5220e.png)

我们的甜点将以简洁的方式呈现。图像由作者提供。

# 偏差-方差权衡简述

偏差-方差权衡的核心思想是：

+   当你在[模型性能评分](http://bit.ly/mfml_039)上获得优异成绩时，无法判断你是否[过拟合](http://bit.ly/mfml_049)或[欠拟合](http://bit.ly/mfml_050)，或者过得很好。

+   训练性能和实际性能（[你关心的那个](http://bit.ly/quaesita_parrot)）并不相同。

+   训练性能是指模型在学习过的旧数据上的表现，而你真正关心的是模型在输入全新数据时的表现。

+   当你**增加**…
