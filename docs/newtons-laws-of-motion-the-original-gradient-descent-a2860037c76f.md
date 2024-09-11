# 牛顿运动定律：最初的梯度下降

> 原文：[https://towardsdatascience.com/newtons-laws-of-motion-the-original-gradient-descent-a2860037c76f?source=collection_archive---------4-----------------------#2023-12-27](https://towardsdatascience.com/newtons-laws-of-motion-the-original-gradient-descent-a2860037c76f?source=collection_archive---------4-----------------------#2023-12-27)

## 探索梯度下降和牛顿运动方程的共享语言

[](https://medium.com/@rodrigopesilva?source=post_page-----a2860037c76f--------------------------------)[![Rodrigo Silva](../Images/d260f05ed9887c5072e0590db1481be2.png)](https://medium.com/@rodrigopesilva?source=post_page-----a2860037c76f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a2860037c76f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a2860037c76f--------------------------------) [Rodrigo Silva](https://medium.com/@rodrigopesilva?source=post_page-----a2860037c76f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F222e82adf972&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnewtons-laws-of-motion-the-original-gradient-descent-a2860037c76f&user=Rodrigo+Silva&userId=222e82adf972&source=post_page-222e82adf972----a2860037c76f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a2860037c76f--------------------------------) ·7 min read·2023年12月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa2860037c76f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnewtons-laws-of-motion-the-original-gradient-descent-a2860037c76f&user=Rodrigo+Silva&userId=222e82adf972&source=-----a2860037c76f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa2860037c76f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnewtons-laws-of-motion-the-original-gradient-descent-a2860037c76f&source=-----a2860037c76f---------------------bookmark_footer-----------)![](../Images/0510803a4ad720b3e2ff6ee725c64c85.png)

照片由 [Luddmyla .](https://unsplash.com/@luddmyla?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来自 [Unsplash](https://unsplash.com/photos/man-playing-skateboard-while-making-tricks-pKSLMEwRpqI?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

我记得在工程学院读本科时，作为物理学学生，我上了第一门机器学习课程。换句话说，我是个局外人。当教授通过梯度下降解释反向传播算法时，我脑海里有个模糊的问题：“梯度下降是一个随机算法吗？”在举手向教授提问之前，陌生的环境让我犹豫了一下；我稍微缩了回来。突然，答案闪现在我脑海里。

这是我想到的。

# 梯度下降

要说清楚什么是梯度下降，我们首先需要定义训练神经网络的问题，我们可以通过概述机器如何学习来做到这一点。

## 神经网络训练概述

在所有监督学习的神经网络任务中，我们有一个预测值和真实值。预测值与真实值之间的差异越大，说明我们的神经网络在预测值方面的表现越差。因此，我们创建了一个称为*损失函数*的函数，通常表示为*L*，它量化了实际值与预测值之间的差异。训练神经网络的任务就是更新权重和偏差（简称参数）以最小化损失函数。这就是训练神经网络的大致情况，而“学习”只是更新参数以最佳适应实际数据，即最小化损失函数。

## 通过梯度下降优化

梯度下降是一种用于计算这些新参数的优化技术。由于我们的任务是选择参数以最小化损失函数，我们需要一个选择标准。我们试图最小化的损失函数是神经网络输出的函数，因此在数学上我们将其表示为*L = L*(*y_nn, y*)。但神经网络输出*y_nn*也依赖于其参数，所以*y_nn* = *y_nn*(**θ**)，其中**θ**是一个包含我们神经网络所有参数的向量。换句话说，损失函数本身是神经网络参数的一个函数。

借鉴了一些向量微积分的概念，我们知道要最小化一个函数，你需要逆着它的*梯度*方向前进，因为梯度指向函数最快增长的方向。为了获得一些直觉，我们来看一下图1中L(**θ**)可能的样子。

![](../Images/1139deaa99d1bfa730554f63b8062840.png)

图1：显示*L(w1,w2)作为w1和w2函数的表面。图像由作者提供。*

在这里，我们对训练神经网络时什么是期望的、什么是不期望的有了清晰的直觉：我们希望损失函数的值更小，所以如果我们从使损失函数落在黄色/橙色区域的参数w1和w2开始，我们希望沿着紫色区域的方向滑动下降到表面。

这种“滑下”运动是通过梯度下降方法实现的。如果我们处在表面上最亮的区域，梯度将继续指向上方，因为这是增加最快的方向。然后，沿相反方向（因此是梯度*下降*）会产生一个向着最大减少区域的运动。

为了看到这一点，我们可以绘制梯度下降向量，如图2所示。在这个图中，我们有一个等高线图，显示了与图1相同的区域和函数，但损失函数的值现在编码为颜色：越亮，值越大。

![](../Images/3fea759395a7eec55f659093c188b1dd.png)

图2：显示指向梯度下降方向的向量的等高线图。图片由作者提供。

我们可以看到，如果我们选择一个位于黄色/橙色区域的点，梯度下降向量会指向最快到达紫色区域的方向。

> 一个很好的免责声明是，通常一个神经网络可能包含任意多的参数（GPT-3有超过1000亿个参数！），这意味着这些漂亮的可视化在实际应用中完全不实用，神经网络中的参数优化通常是一个非常高维的问题。

从数学上讲，梯度下降算法可以表示为

![](../Images/b58f4f03cc362ecb6bedab4bc1bc52ce.png)

在这里，**θ**_(n+1) 是更新后的参数（即图1的表面滑下来的结果）；**θ**_(n) 是我们开始时的参数；ρ 被称为学习率（梯度下降指向的方向上的步长）；∇L 是在初始点 **θ**_(n) 计算的损失函数的梯度。使这里的名字为*下降*的是前面的负号。

数学在这里非常关键，因为我们会看到牛顿的运动第二定律与梯度下降方程具有相同的数学公式。

# 牛顿第二定律

牛顿的运动第二定律可能是经典力学中最重要的概念之一，因为它说明了力、质量和加速度是如何联系在一起的。每个人都知道牛顿第二定律的高中公式：

![](../Images/c327ee2b0739daab2e55cac453b94ff7.png)

其中，F 是力，m 是质量，a 是加速度。然而，牛顿原始的公式是基于一个更深层的量：*动量*。动量是物体的质量与速度的乘积：

![](../Images/86c23dd0fcfa9576402ad0d87154229e.png)

并且可以解释为物体的*运动量*。牛顿第二定律背后的思想是，要改变一个物体的动量，你需要以某种方式干扰它，这种干扰被称为*力*。因此，牛顿第二定律的简洁公式是

![](../Images/f9989f6a13514f256555efe79f7a782f.png)

这种公式适用于你能想到的每一种力，但我们希望在讨论中有更多的结构，为了获得结构，我们需要限制我们的可能性范围。让我们讨论保守力和势能。

## 保守力和势能

保守力是一种不耗散能量的力。这意味着，当我们处于仅涉及保守力的系统中时，总能量是常量。这听起来很严格，但实际上，自然界中最基本的力量都是保守的，如重力和电力。

对于每个保守力，我们关联一个称为*势能*的量。这个势能通过公式与力相关联。

![](../Images/be34c31af4e79f240bf4dd5ea17c8609.png)

在一维中。如果我们仔细查看最后两个公式，就会得到保守场的第二运动定律：

![](../Images/75d544970a15fd52099e4d73e83d767a.png)

由于导数处理起来有点复杂，并且在计算机科学中我们反正将导数近似为有限差分，因此让我们用Δ替换d：

![](../Images/c567d46cecf9295ec5d559c1857105e6.png)

我们知道Δ意味着“取更新值并减去当前值”。因此，我们可以将上述公式重新写成

![](../Images/31faf4864f2ad24e1dcab43280c27134.png)

这已经看起来很像上面几行中的梯度下降公式。为了使其更类似，我们只需在三维中查看它，梯度自然会出现：

![](../Images/061d25617148bb0cda8e33dc704ee48e.png)

我们可以清楚地看到梯度下降与上述公式之间的对应关系，这完全源于牛顿物理学。一个物体的动量（如果你愿意，也可以理解为速度）总是指向势能减少最快的方向，步长由Δt给出。

# 结束语和要点总结

因此，我们可以将牛顿公式中的势能与机器学习中的损失函数相关联。动量向量类似于我们试图优化的参数向量，时间步长常数即学习率，即我们朝着损失函数最小值移动的速度。因此，类似的数学公式表明这些概念是联系在一起的，并且提供了一种很好的统一视角。

如果你想知道，我一开始的问题的答案是“不是”。梯度下降算法中没有随机性，因为它复制了自然每天所做的事情：粒子的物理轨迹总是试图在周围找到最低可能的势能。如果你让一个球从某个高度掉落，它总会有相同的轨迹，没有随机性。当你看到有人在滑板上滑下陡峭的坡道时，请记住：那实际上是自然在应用梯度下降算法。

我们看待问题的方式可能会影响其解决方案。在这篇文章中，我没有展示任何关于计算机科学或物理的新内容（实际上，这里的物理知识已有约400年历史），但改变视角和将（表面上）不相关的概念结合在一起，可能会创造出新的联系和对某一主题的直觉。

# 参考文献

[1] Robert Kwiatkowski, [梯度下降算法——深度探讨](/gradient-descent-algorithm-a-deep-dive-cf04e8115f21)，2021年。

[2] Nivaldo A. Lemos, 《解析力学》，剑桥大学出版社，2018年。
