# 感知机：第一个神经网络模型

> 原文：[https://towardsdatascience.com/perceptrons-the-first-neural-network-model-8b3ee4513757?source=collection_archive---------3-----------------------#2023-03-28](https://towardsdatascience.com/perceptrons-the-first-neural-network-model-8b3ee4513757?source=collection_archive---------3-----------------------#2023-03-28)

## 概述和在 Python 中的实现

[](https://medium.com/@roiyeho?source=post_page-----8b3ee4513757--------------------------------)[![Dr. Roi Yehoshua](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----8b3ee4513757--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b3ee4513757--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8b3ee4513757--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----8b3ee4513757--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperceptrons-the-first-neural-network-model-8b3ee4513757&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----8b3ee4513757---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b3ee4513757--------------------------------) ·14 分钟阅读·2023年3月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8b3ee4513757&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperceptrons-the-first-neural-network-model-8b3ee4513757&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----8b3ee4513757---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8b3ee4513757&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperceptrons-the-first-neural-network-model-8b3ee4513757&source=-----8b3ee4513757---------------------bookmark_footer-----------)![](../Images/878b3bf6ae98c62e8e147e6ad2892349.png)

照片由 [Hal Gatewood](https://unsplash.com/ja/@halacious?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/OgvqXGL7XO4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

感知机是最早的神经网络（NNs）计算模型之一，它们构成了我们今天更复杂和更深层次网络的基础。理解感知机模型及其理论将为你理解神经网络中的许多关键概念提供良好的基础。

# 背景：生物神经网络

生物神经网络（例如我们大脑中的神经网络）由大量称为**神经元**的神经细胞组成。

每个神经元通过称为**树突**的纤维接收来自其邻近神经元的电信号（冲动）。当其接收信号的总和超过某个阈值时，神经元通过称为**轴突**的长纤维发出自己的信号，这些轴突与其他神经元的树突相连接。

两个神经元之间的连接被称为**突触**。平均而言，每个神经元连接约7,000个突触，这显示了我们大脑网络的高度连接性。当我们学习两个概念之间的新关联时，代表这些概念的神经元之间的突触强度会得到增强。这一现象被称为**赫布法则**（1949），其内容是“同时活动的细胞…
