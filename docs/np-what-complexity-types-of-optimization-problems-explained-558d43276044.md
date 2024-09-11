# NP-什么？优化问题的复杂性类型解释

> 原文：[`towardsdatascience.com/np-what-complexity-types-of-optimization-problems-explained-558d43276044?source=collection_archive---------2-----------------------#2023-08-17`](https://towardsdatascience.com/np-what-complexity-types-of-optimization-problems-explained-558d43276044?source=collection_archive---------2-----------------------#2023-08-17)

![](img/d71f4632f6eb7e92629cf29ff7352ffd.png)

复杂的建筑。图片由作者通过 [Midjourney](https://www.midjourney.com/) 创作。 

## 计算机科学中一个核心问题的介绍

[](https://hennie-de-harder.medium.com/?source=post_page-----558d43276044--------------------------------)![Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----558d43276044--------------------------------)[](https://towardsdatascience.com/?source=post_page-----558d43276044--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----558d43276044--------------------------------) [Hennie de Harder](https://hennie-de-harder.medium.com/?source=post_page-----558d43276044--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffb96be98b7b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnp-what-complexity-types-of-optimization-problems-explained-558d43276044&user=Hennie+de+Harder&userId=fb96be98b7b9&source=post_page-fb96be98b7b9----558d43276044---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----558d43276044--------------------------------) ·11 分钟阅读·2023 年 8 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F558d43276044&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnp-what-complexity-types-of-optimization-problems-explained-558d43276044&user=Hennie+de+Harder&userId=fb96be98b7b9&source=-----558d43276044---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F558d43276044&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnp-what-complexity-types-of-optimization-problems-explained-558d43276044&source=-----558d43276044---------------------bookmark_footer-----------)

**为什么** **最短路径问题** **容易解决，而** [**旅行商问题**](https://medium.com/towards-data-science/local-search-with-simulated-annealing-from-scratch-9f8dcb6c2e06) **则不然？这背后的数学思想是什么？如何确定一个问题在其规模增加时是否会需要无法管理的步骤？在这篇文章中，你将学习这个话题的基础知识。如果你想深入了解这个话题，我在文章末尾附上了一个关于这一主题的千年奖难题的简短说明。**

在我们开始讨论 NP 难度之前，你应该了解时间复杂度的基础知识。如果你已经熟悉时间复杂度、大 O 符号和最坏情况分析，你可以跳过以下部分。

# 时间复杂度

当我们使用计算机和编写程序时，我们经常遇到可以用不同方式解决的问题。我们需要考虑的一个重要问题是这些解决方案的效率如何。时间复杂度帮助我们理解随着问题规模的增加，算法运行的速度如何。

*大 O 符号* 可以与给算法贴上一个简单的标签进行比较，这个标签告诉我们算法完成所需的时间基于我们处理的项目数量…。
