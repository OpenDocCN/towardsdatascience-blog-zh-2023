# 利用物理信息神经网络和符号回归发现微分方程

> 原文：[https://towardsdatascience.com/discovering-differential-equations-with-physics-informed-neural-networks-and-symbolic-regression-c28d279c0b4d?source=collection_archive---------0-----------------------#2023-07-28](https://towardsdatascience.com/discovering-differential-equations-with-physics-informed-neural-networks-and-symbolic-regression-c28d279c0b4d?source=collection_archive---------0-----------------------#2023-07-28)

## 一个逐步实现代码的案例研究

[](https://shuaiguo.medium.com/?source=post_page-----c28d279c0b4d--------------------------------)[![Shuai Guo](../Images/d673c066f8006079be5bf92757e73a59.png)](https://shuaiguo.medium.com/?source=post_page-----c28d279c0b4d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c28d279c0b4d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c28d279c0b4d--------------------------------) [帅果](https://shuaiguo.medium.com/?source=post_page-----c28d279c0b4d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiscovering-differential-equations-with-physics-informed-neural-networks-and-symbolic-regression-c28d279c0b4d&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----c28d279c0b4d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c28d279c0b4d--------------------------------) ·25分钟阅读·2023年7月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc28d279c0b4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiscovering-differential-equations-with-physics-informed-neural-networks-and-symbolic-regression-c28d279c0b4d&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----c28d279c0b4d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc28d279c0b4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdiscovering-differential-equations-with-physics-informed-neural-networks-and-symbolic-regression-c28d279c0b4d&source=-----c28d279c0b4d---------------------bookmark_footer-----------)![](../Images/feb76e9bafbe81909b3d783c4332a1eb.png)

图片由 [Steven Coffey](https://unsplash.com/@steeeve?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

微分方程作为一个强大的框架，用于捕捉和理解物理系统的动态行为。通过描述变量如何相互变化，它们提供了对系统动态的洞察，并使我们能够预测系统的未来行为。

然而，我们在许多现实世界系统中面临的一个共同挑战是，它们的控制微分方程通常是*部分已知*的，未知的方面以几种方式表现出来：

+   微分方程的**参数**是未知的。一个例子是风工程，其中流体动力学的控制方程是明确的，但与湍流相关的系数则高度不确定。

+   微分方程的**函数形式**是未知的。例如，在化学工程中，由于速率决定步骤和反应路径的不确定性，速率方程的确切函数形式可能没有完全理解。

+   **函数形式**和**参数**都是未知的。一个典型的例子是电池状态建模，其中常用的等效电路…
