# 应用超参数调整到神经网络

> 原文：[`towardsdatascience.com/hyperparameter-tuning-neural-networks-101-ca1102891b27?source=collection_archive---------6-----------------------#2023-11-18`](https://towardsdatascience.com/hyperparameter-tuning-neural-networks-101-ca1102891b27?source=collection_archive---------6-----------------------#2023-11-18)

## 如何通过 Python 示例改进神经网络的“学习”过程

[](https://medium.com/@egorhowell?source=post_page-----ca1102891b27--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----ca1102891b27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ca1102891b27--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ca1102891b27--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----ca1102891b27--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperparameter-tuning-neural-networks-101-ca1102891b27&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----ca1102891b27---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ca1102891b27--------------------------------) ·9 分钟阅读·2023 年 11 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fca1102891b27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperparameter-tuning-neural-networks-101-ca1102891b27&user=Egor+Howell&userId=1cac491223b2&source=-----ca1102891b27---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fca1102891b27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperparameter-tuning-neural-networks-101-ca1102891b27&source=-----ca1102891b27---------------------bookmark_footer-----------)![](img/6b66a379b90bb74a1caf0ca7ed45e511.png)

神经网络图标由 Vectors Tank — Flaticon 创建。神经网络图标。[`www.flaticon.com/free-icons/neural`](https://www.flaticon.com/free-icons/neural-network)

# 背景

在我之前的帖子中，我们讨论了如何从数据中预测和学习[***神经网络***](https://medium.com/gitconnected/intro-perceptron-architecture-neural-networks-101-2a487062810c)。有两个过程负责此操作：***前向传播***和反向传播，也称为[***反向传播***](https://en.wikipedia.org/wiki/Backpropagation)。您可以在此处了解更多信息：

[](/forward-pass-backpropagation-neural-networks-101-3a75996ada3b?source=post_page-----ca1102891b27--------------------------------) ## 前向传播和反向传播：神经网络基础

### 解释神经网络如何通过手工和使用 PyTorch 中的代码来“训练”和“学习”数据中的模式。

[towardsdatascience.com

本文将深入探讨如何优化这个“学习”和“训练”过程，以提高我们模型的性能。我们将涵盖的领域包括计算改进和***超参数调优***，以及如何在 PyTorch 中实现它！

但在谈论所有这些好东西之前，让我们快速回顾一下神经网络吧！

# 快速回顾：什么是神经网络？

神经网络是大型数学表达式，试图找到能够将一组输入映射到相应输出的“正确”函数。一个…
