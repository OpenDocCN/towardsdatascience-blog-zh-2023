# 深度学习中的神经网络优化

> 原文：[https://towardsdatascience.com/optimisation-algorithms-neural-networks-101-256e16a88412?source=collection_archive---------4-----------------------#2023-11-24](https://towardsdatascience.com/optimisation-algorithms-neural-networks-101-256e16a88412?source=collection_archive---------4-----------------------#2023-11-24)

## 如何在“基础”梯度下降算法之外提升训练效果

[](https://medium.com/@egorhowell?source=post_page-----256e16a88412--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----256e16a88412--------------------------------)[](https://towardsdatascience.com/?source=post_page-----256e16a88412--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----256e16a88412--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----256e16a88412--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimisation-algorithms-neural-networks-101-256e16a88412&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----256e16a88412---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----256e16a88412--------------------------------) ·8分钟阅读·2023年11月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F256e16a88412&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimisation-algorithms-neural-networks-101-256e16a88412&user=Egor+Howell&userId=1cac491223b2&source=-----256e16a88412---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F256e16a88412&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimisation-algorithms-neural-networks-101-256e16a88412&source=-----256e16a88412---------------------bookmark_footer-----------)![](../Images/f4296e3c0bd744ca991919ad968f9e98.png)

[https://www.flaticon.com/free-icons/neural-network](https://www.flaticon.com/free-icons/neural-network).神经网络图标。神经网络图标由andinur创建 — Flaticon.

# 背景

在我上一篇文章中，我们讨论了如何通过[***超参数调整***](/optimise-your-hyperparameter-tuning-with-hyperopt-861573239eb5)提高神经网络的性能：

[](/hyperparameter-tuning-neural-networks-101-ca1102891b27?source=post_page-----256e16a88412--------------------------------) [## 超参数调整：神经网络基础

### 如何通过调整超参数来提升神经网络的“学习”和“训练”

towardsdatascience.com](/hyperparameter-tuning-neural-networks-101-ca1102891b27?source=post_page-----256e16a88412--------------------------------)

这是一个过程，通过该过程可以“调优”最佳超参数，如学习率和隐藏层数量，以找到最适合我们网络的超参数，从而提升其性能。

不幸的是，对于大型深度神经网络（[***深度学习***](https://en.wikipedia.org/wiki/Deep_learning)）的调优过程非常缓慢。改进的方法之一是使用比传统的“原生”梯度下降方法更*快速的优化器*。在这篇文章中，我们将深入探讨最流行的优化器和[***梯度下降***](/why-gradient-descent-is-so-common-in-data-science-def3e6515c5c)的变体，这些可以提高训练速度以及收敛速度，并在PyTorch中进行比较！

# 回顾：梯度下降

在深入之前，我们快速复习一下梯度下降及其背后的理论。
