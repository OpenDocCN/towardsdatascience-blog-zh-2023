# 回归基础，第二部分：梯度下降

> 原文：[https://towardsdatascience.com/back-to-basics-part-dos-linear-regression-cost-function-and-gradient-descent-e3d7d05c56fd?source=collection_archive---------1-----------------------#2023-02-04](https://towardsdatascience.com/back-to-basics-part-dos-linear-regression-cost-function-and-gradient-descent-e3d7d05c56fd?source=collection_archive---------1-----------------------#2023-02-04)

[](https://medium.com/@shreya.rao?source=post_page-----e3d7d05c56fd--------------------------------)[![Shreya Rao](../Images/03f13be6f5f67783d32f0798f09a4f86.png)](https://medium.com/@shreya.rao?source=post_page-----e3d7d05c56fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e3d7d05c56fd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e3d7d05c56fd--------------------------------) [Shreya Rao](https://medium.com/@shreya.rao?source=post_page-----e3d7d05c56fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F99b63de2f2c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fback-to-basics-part-dos-linear-regression-cost-function-and-gradient-descent-e3d7d05c56fd&user=Shreya+Rao&userId=99b63de2f2c3&source=post_page-99b63de2f2c3----e3d7d05c56fd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e3d7d05c56fd--------------------------------) ·11分钟阅读·2023年2月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe3d7d05c56fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fback-to-basics-part-dos-linear-regression-cost-function-and-gradient-descent-e3d7d05c56fd&user=Shreya+Rao&userId=99b63de2f2c3&source=-----e3d7d05c56fd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe3d7d05c56fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fback-to-basics-part-dos-linear-regression-cost-function-and-gradient-descent-e3d7d05c56fd&source=-----e3d7d05c56fd---------------------bookmark_footer-----------)

欢迎来到我们***回归基础***系列的第二部分。在[第一部分](https://medium.com/towards-data-science/back-to-basics-part-uno-linear-regression-cost-function-and-gradient-descent-590dcb3eee46)，我们讲解了如何使用线性回归和成本函数为我们的房价数据找到最佳拟合线。然而，我们也发现测试多个*截距*值可能是繁琐且低效的。在第二部分中，我们将深入探讨梯度下降，这是一种强大的技术，可以帮助我们找到完美的*截距*并优化我们的模型。我们将探讨其背后的数学原理，并了解它如何应用于我们的线性回归问题。

梯度下降是一种强大的优化算法，***旨在快速而高效地找到曲线的最小点。*** 直观地理解这个过程的最佳方式是想象你站在山顶，山谷中有一个装满金币的宝箱在等着你。

![](../Images/31950f4c1265a42f3cdc94e121c9c121.png)

然而，谷底的确切位置是未知的，因为外面非常黑暗，你什么也看不见。此外，你还希望在其他人之前到达谷底（因为你想要所有的宝藏）。梯度下降帮助你导航地形，并*高效且迅速*地达到这个*最优*点。在每个点上，它会告诉你需要迈多少步以及朝哪个方向前进。

同样，通过使用…，梯度下降可以应用于我们的线性回归问题。
