# 嘿 GPU，我的矩阵怎么了？

> 原文：[https://towardsdatascience.com/hey-gpu-whats-up-with-my-matrix-cb7f6d7ae7d6?source=collection_archive---------16-----------------------#2023-06-13](https://towardsdatascience.com/hey-gpu-whats-up-with-my-matrix-cb7f6d7ae7d6?source=collection_archive---------16-----------------------#2023-06-13)

## 一份温和的指南，帮助你理解 GPU 如何执行矩阵乘法

[](https://thushv89.medium.com/?source=post_page-----cb7f6d7ae7d6--------------------------------)[![Thushan Ganegedara](../Images/3fabfa37132f7d3a9e7679c3b8d7e061.png)](https://thushv89.medium.com/?source=post_page-----cb7f6d7ae7d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cb7f6d7ae7d6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cb7f6d7ae7d6--------------------------------) [Thushan Ganegedara](https://thushv89.medium.com/?source=post_page-----cb7f6d7ae7d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6f0b045d5681&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhey-gpu-whats-up-with-my-matrix-cb7f6d7ae7d6&user=Thushan+Ganegedara&userId=6f0b045d5681&source=post_page-6f0b045d5681----cb7f6d7ae7d6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cb7f6d7ae7d6--------------------------------) ·8分钟阅读·2023年6月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcb7f6d7ae7d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhey-gpu-whats-up-with-my-matrix-cb7f6d7ae7d6&user=Thushan+Ganegedara&userId=6f0b045d5681&source=-----cb7f6d7ae7d6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcb7f6d7ae7d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhey-gpu-whats-up-with-my-matrix-cb7f6d7ae7d6&source=-----cb7f6d7ae7d6---------------------bookmark_footer-----------)![](../Images/a911c1d35115175bbfe3894ea74e8d00.png)

照片由 [Thomas Foster](https://unsplash.com/@thomasfos?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/vWgoeEYdtIY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

矩阵乘法；深度神经网络和现代语言理解巨头的圣杯。作为机器学习工程师或数据科学家，我们的手指总是迅速地敲打 `tf.matmul` 或 `torch.matmul`，而且从不回头。但别告诉我你从未有过对矩阵进入 GPU 后发生了什么的短暂迷恋！如果你有，那你来对地方了。跟随我一起探索 GPU 内部迷人的复杂性。

我会向你解释这些计算强者如何处理数据。你将了解到 GPU 在面对矩阵时所做的三件鲜为人知的令人印象深刻的事情。到这篇博客文章结束时，你将对 GPU 内部的矩阵乘法有一个很好的理解。

# GEMM: 对于 GPU 来说，真正的瑰宝💎

GEMM 或广义矩阵乘法是当 GPU 执行矩阵乘法时执行的内核。

`C = a (A.B) + b C`

在这里，`a` 和 `b` 是标量，`A` 是一个 `MxK` 矩阵，`B` 是一个 `KxN` 矩阵，因此 `C` 是一个 `MxN` 矩阵。就是这么简单！你可能会想知道为什么会有那项附加的加法。事实证明，这是一种神经网络中相当常见的模式……
