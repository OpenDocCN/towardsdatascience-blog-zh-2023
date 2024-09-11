# 线性代数的鸟瞰图：映射的度量——行列式

> 原文：[https://towardsdatascience.com/a-birds-eye-view-of-linear-algebra-the-measure-of-a-map-determinant-1e5fd752a3be?source=collection_archive---------3-----------------------#2023-11-05](https://towardsdatascience.com/a-birds-eye-view-of-linear-algebra-the-measure-of-a-map-determinant-1e5fd752a3be?source=collection_archive---------3-----------------------#2023-11-05)

[](https://medium.com/@rohitpandey576?source=post_page-----1e5fd752a3be--------------------------------)[![Rohit Pandey](../Images/af817d8f68f2984058f0afb8fd7ecbe9.png)](https://medium.com/@rohitpandey576?source=post_page-----1e5fd752a3be--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e5fd752a3be--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1e5fd752a3be--------------------------------) [Rohit Pandey](https://medium.com/@rohitpandey576?source=post_page-----1e5fd752a3be--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa743c5fec8cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-the-measure-of-a-map-determinant-1e5fd752a3be&user=Rohit+Pandey&userId=a743c5fec8cd&source=post_page-a743c5fec8cd----1e5fd752a3be---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e5fd752a3be--------------------------------) ·16 min read·2023年11月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1e5fd752a3be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-the-measure-of-a-map-determinant-1e5fd752a3be&user=Rohit+Pandey&userId=a743c5fec8cd&source=-----1e5fd752a3be---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1e5fd752a3be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-the-measure-of-a-map-determinant-1e5fd752a3be&source=-----1e5fd752a3be---------------------bookmark_footer-----------)![](../Images/b0a624404feecd67f0eeefedb248be8c.png)

图像由 midjourney 创建

这是关于线性代数的进行中的书籍《线性代数的鸟瞰图》的第二章。到目前为止的目录：

1.  第一章：[基础知识](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f)

1.  **第二章**： （当前）映射的度量——行列式

1.  第三章：[为什么矩阵乘法是这样？](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-why-is-matrix-multiplication-like-that-a4d94067651e)

1.  第4章: [方程组、线性回归和神经网络](https://medium.com/p/fe5b88a57f66)

1.  第5章: [秩的零空间及为何行秩 == 列秩](/a-birds-eye-view-of-linear-algebra-rank-nullity-and-why-row-rank-equals-column-rank-bc084e0e1075)

线性代数是多维度的工具。无论你在做什么，只要你扩展到*n*维度，线性代数就会进入视野。

在[前一章，](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f)我们描述了抽象线性映射。在本章中，我们将卷起袖子，开始处理矩阵。实际问题，如数值稳定性、有效算法等，将开始被探讨。

# I) 如何量化线性映射？

我们在[前一章](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f)中讨论了向量空间的概念（基本上是n维的数字集合——更一般来说是[域](https://en.wikipedia.org/wiki/Field_(mathematics))的集合）以及在这两个向量空间上操作的线性映射，将一个空间的对象映射到另一个空间。
