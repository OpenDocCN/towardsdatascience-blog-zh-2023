# 线性代数的鸟瞰视角：为什么矩阵乘法是这样的？

> 原文：[https://towardsdatascience.com/a-birds-eye-view-of-linear-algebra-why-is-matrix-multiplication-like-that-a4d94067651e?source=collection_archive---------3-----------------------#2023-11-23](https://towardsdatascience.com/a-birds-eye-view-of-linear-algebra-why-is-matrix-multiplication-like-that-a4d94067651e?source=collection_archive---------3-----------------------#2023-11-23)

## 第一个矩阵的列为什么要与第二个矩阵的行匹配？为什么不让两个矩阵的行匹配呢？

[](https://medium.com/@rohitpandey576?source=post_page-----a4d94067651e--------------------------------)[![Rohit Pandey](../Images/af817d8f68f2984058f0afb8fd7ecbe9.png)](https://medium.com/@rohitpandey576?source=post_page-----a4d94067651e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a4d94067651e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a4d94067651e--------------------------------) [Rohit Pandey](https://medium.com/@rohitpandey576?source=post_page-----a4d94067651e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa743c5fec8cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-why-is-matrix-multiplication-like-that-a4d94067651e&user=Rohit+Pandey&userId=a743c5fec8cd&source=post_page-a743c5fec8cd----a4d94067651e---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4d94067651e--------------------------------) ·18 min read·2023年11月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa4d94067651e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-why-is-matrix-multiplication-like-that-a4d94067651e&user=Rohit+Pandey&userId=a743c5fec8cd&source=-----a4d94067651e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa4d94067651e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-why-is-matrix-multiplication-like-that-a4d94067651e&source=-----a4d94067651e---------------------bookmark_footer-----------)![](../Images/2512b5389933ce8260d05fe8308ab411.png)

由 midjourney 创建的图片

这是正在进行中的线性代数书籍的第三章，“线性代数的鸟瞰视角”。到目前为止的目录如下：

1.  [第一章：基础知识](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f)

1.  第二章：[地图的度量——行列式](https://medium.com/p/1e5fd752a3be)

1.  **第三章：**（当前）为什么矩阵乘法是这样的？

1.  第4章: [方程组、线性回归和神经网络](https://medium.com/p/fe5b88a57f66)

1.  第5章: [秩和为什么行秩 == 列秩](/a-birds-eye-view-of-linear-algebra-rank-nullity-and-why-row-rank-equals-column-rank-bc084e0e1075)

在这里，我们将描述可以对两个矩阵执行的操作，但要记住它们只是线性映射的表示。

# I) 为什么要关注矩阵乘法？

几乎任何信息都可以嵌入到向量空间中。图像、视频、语言、语音、生物识别信息以及你能想象的其他所有东西。而所有的机器学习和人工智能应用（如最近的聊天机器人、文本到图像等）都基于这些向量嵌入。由于线性代数是处理这些问题的科学...
