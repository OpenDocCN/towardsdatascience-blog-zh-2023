# 揭开旋转矩阵的神秘面纱

> 原文：[https://towardsdatascience.com/demystifying-rotation-matrix-6c8885c691d6?source=collection_archive---------7-----------------------#2023-11-01](https://towardsdatascience.com/demystifying-rotation-matrix-6c8885c691d6?source=collection_archive---------7-----------------------#2023-11-01)

## 如何在 R² 中旋转向量

[](https://medium.com/@smertatli?source=post_page-----6c8885c691d6--------------------------------)[![Mert Atli](../Images/2ce3ad8e4e3533aa889e1972ca52b02b.png)](https://medium.com/@smertatli?source=post_page-----6c8885c691d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6c8885c691d6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6c8885c691d6--------------------------------) [Mert Atli](https://medium.com/@smertatli?source=post_page-----6c8885c691d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff38e5ad771a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-rotation-matrix-6c8885c691d6&user=Mert+Atli&userId=f38e5ad771a7&source=post_page-f38e5ad771a7----6c8885c691d6---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c8885c691d6--------------------------------) ·6分钟阅读·2023年11月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6c8885c691d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-rotation-matrix-6c8885c691d6&user=Mert+Atli&userId=f38e5ad771a7&source=-----6c8885c691d6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6c8885c691d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdemystifying-rotation-matrix-6c8885c691d6&source=-----6c8885c691d6---------------------bookmark_footer-----------)

旋转矩阵在线性代数的世界里就像一个神奇的工具，旨在精准而轻松地旋转空间中的向量。想象你有一个向量，一支小箭头指向空间中的某处，你想围绕某个点旋转它，就像在钥匙圈上旋转钥匙一样。这正是旋转矩阵帮助你完成的任务。

为了了解旋转矩阵如何产生，让我们从 R² 中的一个向量开始，并尝试沿水平轴旋转它。

# 在 R² 中旋转向量

下图展示了一个在 R² 中的向量 **v**，它与水平轴形成了角度 **a**。假设我们想要沿水平轴逆时针旋转‘b 度’，这就是 **v’**。

![](../Images/19c48b2f1c07578044de81caa85bd4ae.png)

从 v 通过旋转到 v’

**正如我们所见，旋转只改变了 v 的方向，并保持长度（即‘大小’）不变。**

在 R² 中，我们可以将向量 v 表示为有序元组 (m, n)，其中第一个元素位于水平轴上，第二个元素位于垂直轴上。根据三角学，我们知道**v**=(m, n) 的坐标可以表示为 (||v||.cos(a), ||v||.sin(a))。
