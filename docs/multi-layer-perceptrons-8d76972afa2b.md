# 多层感知器的解释与说明

> 原文：[https://towardsdatascience.com/multi-layer-perceptrons-8d76972afa2b?source=collection_archive---------0-----------------------#2023-04-02](https://towardsdatascience.com/multi-layer-perceptrons-8d76972afa2b?source=collection_archive---------0-----------------------#2023-04-02)

## 了解第一个完全功能的神经网络模型

[](https://medium.com/@roiyeho?source=post_page-----8d76972afa2b--------------------------------)[![Roi Yehoshua 博士](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----8d76972afa2b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8d76972afa2b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8d76972afa2b--------------------------------) [Roi Yehoshua 博士](https://medium.com/@roiyeho?source=post_page-----8d76972afa2b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-layer-perceptrons-8d76972afa2b&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----8d76972afa2b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d76972afa2b--------------------------------) ·13分钟阅读·2023年4月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8d76972afa2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-layer-perceptrons-8d76972afa2b&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----8d76972afa2b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8d76972afa2b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-layer-perceptrons-8d76972afa2b&source=-----8d76972afa2b---------------------bookmark_footer-----------)

在 [上一篇文章](https://medium.com/@roiyeho/perceptrons-the-first-neural-network-model-8b3ee4513757) 中，我们讨论了感知器作为最早期的神经网络模型之一。正如我们所看到的，单层感知器在计算能力上有限，因为它们只能解决线性可分的问题。

在本文中，我们将讨论多层感知器（MLP），它们是由多层感知器组成的网络，比单层感知器强大得多。我们将了解这些网络如何运作以及如何利用它们解决诸如图像分类等复杂任务。

# 定义与符号

**多层感知机**（MLP）是一个具有至少三层的神经网络：一个输入层、一个隐藏层和一个输出层。每一层都作用于其前一层的输出：

![](../Images/a1eb1fbb0ed55fbf175252ae54f14c61.png)

MLP 架构

我们将使用以下符号：

+   *aᵢˡ* 是层 *l* 中神经元 *i* 的激活（输出）

+   *wᵢⱼˡ* 是从层 *l*-1 中神经元 *j* 到层 *l* 中神经元 *i* 的连接权重

+   *bᵢˡ* 是层 *l* 中神经元 *i* 的偏置项

输入层和输出层之间的中间层称为 **隐藏层**……
