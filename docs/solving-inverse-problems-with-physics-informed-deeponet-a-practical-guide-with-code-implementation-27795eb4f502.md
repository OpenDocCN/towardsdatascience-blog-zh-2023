# 解决逆问题的物理信息深度ONet：带有代码实现的实用指南

> 原文：[https://towardsdatascience.com/solving-inverse-problems-with-physics-informed-deeponet-a-practical-guide-with-code-implementation-27795eb4f502?source=collection_archive---------1-----------------------#2023-07-17](https://towardsdatascience.com/solving-inverse-problems-with-physics-informed-deeponet-a-practical-guide-with-code-implementation-27795eb4f502?source=collection_archive---------1-----------------------#2023-07-17)

## 两个案例研究，涉及参数估计和输入函数校准

[](https://shuaiguo.medium.com/?source=post_page-----27795eb4f502--------------------------------)[![Shuai Guo](../Images/d673c066f8006079be5bf92757e73a59.png)](https://shuaiguo.medium.com/?source=post_page-----27795eb4f502--------------------------------)[](https://towardsdatascience.com/?source=post_page-----27795eb4f502--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----27795eb4f502--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----27795eb4f502--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-inverse-problems-with-physics-informed-deeponet-a-practical-guide-with-code-implementation-27795eb4f502&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----27795eb4f502---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----27795eb4f502--------------------------------) ·21 min read·2023年7月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F27795eb4f502&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-inverse-problems-with-physics-informed-deeponet-a-practical-guide-with-code-implementation-27795eb4f502&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----27795eb4f502---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F27795eb4f502&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-inverse-problems-with-physics-informed-deeponet-a-practical-guide-with-code-implementation-27795eb4f502&source=-----27795eb4f502---------------------bookmark_footer-----------)![](../Images/375cf738f91896940d75caed2c8b983d.png)

图片由 [愚木混株 cdd20](https://unsplash.com/@cdd20?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在我的[上一篇博客](https://medium.com/towards-data-science/operator-learning-via-physics-informed-deeponet-lets-implement-it-from-scratch-6659f3179887)中，我们深入探讨了物理信息化DeepONet (PI-DeepONet)的概念，并探讨了为什么它特别适用于算子学习，即学习从输入函数到输出函数的映射。我们还将理论转化为代码，实施了一个可以准确解决普通微分方程 (ODE) 的PI-DeepONet，即使面对未见过的输入强迫轮廓。

![](../Images/7ac9405ffbb94748bfce6bc08ba4c3b5.png)

图1\. 算子将一个函数转换为另一个函数，这是在现实世界动态系统中经常遇到的概念。**算子学习**本质上涉及训练一个神经网络模型来逼近这个潜在的算子。实现这一目标的一个有前途的方法是**DeepONet**。（图像作者提供）

使用PI-DeepONet解决这些***正向***问题的能力无疑是有价值的。但这就是PI-DeepONet能做的全部吗？嗯，绝对不是！

在计算科学和工程中，我们经常遇到的另一个重要问题类别是所谓的***逆问题***。本质上，这类问题**逆转了信息流动的方向，从输出到输入**：输入是未知的，输出是可观测的，任务是从观测到的输出中估计未知的输入。
