# 深度学习中的梯度消失与爆炸问题

> 原文：[https://towardsdatascience.com/vanishing-exploding-gradient-problem-neural-networks-101-c8f48ec6a80b?source=collection_archive---------5-----------------------#2023-12-08](https://towardsdatascience.com/vanishing-exploding-gradient-problem-neural-networks-101-c8f48ec6a80b?source=collection_archive---------5-----------------------#2023-12-08)

## 如何确保你的神经网络不会“死亡”或“爆炸”

[](https://medium.com/@egorhowell?source=post_page-----c8f48ec6a80b--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----c8f48ec6a80b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c8f48ec6a80b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c8f48ec6a80b--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----c8f48ec6a80b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvanishing-exploding-gradient-problem-neural-networks-101-c8f48ec6a80b&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----c8f48ec6a80b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c8f48ec6a80b--------------------------------) ·9 min read·2023年12月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc8f48ec6a80b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvanishing-exploding-gradient-problem-neural-networks-101-c8f48ec6a80b&user=Egor+Howell&userId=1cac491223b2&source=-----c8f48ec6a80b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc8f48ec6a80b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvanishing-exploding-gradient-problem-neural-networks-101-c8f48ec6a80b&source=-----c8f48ec6a80b---------------------bookmark_footer-----------)![](../Images/8ec02d8429b5e34801dd4ccb016d5e74.png)

[https://www.flaticon.com/free-icons/neural-network](https://www.flaticon.com/free-icons/neural-network). title=”neural network icons.” 神经网络图标由 Paul J. 创建 — Flaticon。

# 什么是梯度消失与爆炸？

在我之前的一篇文章中，我们解释了神经网络如何通过反向传播算法进行学习。主要思想是我们从输出层开始，将错误“传播”到输入层，并在此过程中根据损失函数更新权重。如果你对这个概念不熟悉，我强烈推荐你查看那篇文章：

[](/forward-pass-backpropagation-neural-networks-101-3a75996ada3b?source=post_page-----c8f48ec6a80b--------------------------------) [## 前向传播与反向传播：神经网络基础 101

### 解释神经网络如何通过手动操作和使用 PyTorch 代码“训练”和“学习”数据中的模式。

[towardsdatascience.com](/forward-pass-backpropagation-neural-networks-101-3a75996ada3b?source=post_page-----c8f48ec6a80b--------------------------------)

权重是通过相对于损失函数的偏导数进行更新的。问题在于，随着我们接近网络的更低层，这些梯度变得越来越小。这导致在训练网络时，较低层的权重几乎没有变化。这被称为*梯度消失问题*。

相反的情况也可能发生，即梯度在层间继续增大。这就是*梯度爆炸问题*，主要出现在[***递归神经网络***](https://en.wikipedia.org/wiki/Recurrent_neural_network)中……
