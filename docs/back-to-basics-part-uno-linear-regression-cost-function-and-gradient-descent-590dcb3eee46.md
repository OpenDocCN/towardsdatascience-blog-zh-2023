# 基础回顾，第一部分：线性回归与成本函数

> 原文：[`towardsdatascience.com/back-to-basics-part-uno-linear-regression-cost-function-and-gradient-descent-590dcb3eee46?source=collection_archive---------3-----------------------#2023-02-03`](https://towardsdatascience.com/back-to-basics-part-uno-linear-regression-cost-function-and-gradient-descent-590dcb3eee46?source=collection_archive---------3-----------------------#2023-02-03)

## 一份关于机器学习基本概念的插图指南

[](https://medium.com/@shreya.rao?source=post_page-----590dcb3eee46--------------------------------)![Shreya Rao](https://medium.com/@shreya.rao?source=post_page-----590dcb3eee46--------------------------------)[](https://towardsdatascience.com/?source=post_page-----590dcb3eee46--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----590dcb3eee46--------------------------------) [Shreya Rao](https://medium.com/@shreya.rao?source=post_page-----590dcb3eee46--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F99b63de2f2c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fback-to-basics-part-uno-linear-regression-cost-function-and-gradient-descent-590dcb3eee46&user=Shreya+Rao&userId=99b63de2f2c3&source=post_page-99b63de2f2c3----590dcb3eee46---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----590dcb3eee46--------------------------------) ·7 分钟阅读·2023 年 2 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F590dcb3eee46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fback-to-basics-part-uno-linear-regression-cost-function-and-gradient-descent-590dcb3eee46&user=Shreya+Rao&userId=99b63de2f2c3&source=-----590dcb3eee46---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F590dcb3eee46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fback-to-basics-part-uno-linear-regression-cost-function-and-gradient-descent-590dcb3eee46&source=-----590dcb3eee46---------------------bookmark_footer-----------)

今天，我们将深入探讨机器学习中的三个关键概念：线性回归、成本函数和梯度下降。这些概念构成了许多机器学习算法的基础。起初，我决定不写这方面的文章，因为这些主题已经被广泛讨论。然而，我改变了主意，因为理解这些概念对理解更高级的话题，如神经网络（我计划在不久的将来进行探讨），是至关重要的。此外，本系列将分为两部分，以便更好地管理和组织，便于理解。

所以请舒适地坐好，拿一杯咖啡，准备开始一段神奇的机器学习之旅。

和任何机器学习问题一样，我们从一个具体的问题开始。在这种情况下，我们的朋友马克考虑出售他的 2400 平方英尺的房子，并求助于我们，以确定最合适的挂牌价格。

![](img/dcfbc5c2f2237a4bf6a0c9d903f45f91.png)

直观地，我们开始通过查看朋友邻里的类似房屋来进行分析。经过一些挖掘，我们找到了三个附近房屋的列表，并查看它们的售价。当然，典型的……
