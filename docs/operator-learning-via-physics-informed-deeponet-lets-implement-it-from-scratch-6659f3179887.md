# 通过物理信息 DeepONet 进行算子学习：从头实现

> 原文：[`towardsdatascience.com/operator-learning-via-physics-informed-deeponet-lets-implement-it-from-scratch-6659f3179887?source=collection_archive---------2-----------------------#2023-07-07`](https://towardsdatascience.com/operator-learning-via-physics-informed-deeponet-lets-implement-it-from-scratch-6659f3179887?source=collection_archive---------2-----------------------#2023-07-07)

## 深入探讨 DeepONets、物理信息神经网络及物理信息 DeepONets

[](https://shuaiguo.medium.com/?source=post_page-----6659f3179887--------------------------------)![Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----6659f3179887--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6659f3179887--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6659f3179887--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----6659f3179887--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foperator-learning-via-physics-informed-deeponet-lets-implement-it-from-scratch-6659f3179887&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----6659f3179887---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6659f3179887--------------------------------) · 23 分钟阅读 · 2023 年 7 月 7 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6659f3179887&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foperator-learning-via-physics-informed-deeponet-lets-implement-it-from-scratch-6659f3179887&source=-----6659f3179887---------------------bookmark_footer-----------)![](img/e58768fda61ec2f49710623f6f30cdc2.png)

图 1. ODE/PDE 被广泛用于描述系统过程。在许多场景中，这些 ODE/PDE 接受一个函数（例如，强迫函数 u(t)）作为输入，并输出另一个函数（例如，s(t)）。传统上，数值求解器用于连接输入和输出。近年来，**神经算子** 被开发出来以更高的效率解决相同的问题。（图片由作者提供）

常微分方程（ODEs）和偏微分方程（PDEs）是科学和工程许多学科的基础，从物理学和生物学到经济学和气候科学。它们是描述物理系统和过程的基本工具，捕捉了量随时间和空间的连续变化。

然而，这些方程的一个独特特征是它们不仅接受单一值作为输入，还接受函数。例如，考虑预测地震对建筑物振动的情况。地面的震动，随时间变化，可以表示为一个作为输入的函数，作用于描述建筑物运动的微分方程中。类似地，在音乐厅中声波传播的情况下，乐器产生的声波可以是一个输入函数，具有随时间变化的音量和音调。这些变化的输入函数从根本上影响了结果输出函数——建筑物的振动和音乐厅中的声学场。
