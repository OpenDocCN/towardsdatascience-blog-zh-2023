# 线性代数的全景视角：方程组、线性回归和神经网络

> 原文：[`towardsdatascience.com/a-birds-eye-view-of-linear-algebra-systems-of-equations-linear-regression-and-neural-networks-fe5b88a57f66?source=collection_archive---------1-----------------------#2023-12-28`](https://towardsdatascience.com/a-birds-eye-view-of-linear-algebra-systems-of-equations-linear-regression-and-neural-networks-fe5b88a57f66?source=collection_archive---------1-----------------------#2023-12-28)

## 谦逊的矩阵乘法及其逆几乎是许多简单机器学习模型中发生的**最终**过程

[](https://medium.com/@rohitpandey576?source=post_page-----fe5b88a57f66--------------------------------)![Rohit Pandey](https://medium.com/@rohitpandey576?source=post_page-----fe5b88a57f66--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe5b88a57f66--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe5b88a57f66--------------------------------) [Rohit Pandey](https://medium.com/@rohitpandey576?source=post_page-----fe5b88a57f66--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa743c5fec8cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-systems-of-equations-linear-regression-and-neural-networks-fe5b88a57f66&user=Rohit+Pandey&userId=a743c5fec8cd&source=post_page-a743c5fec8cd----fe5b88a57f66---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe5b88a57f66--------------------------------) · 18 分钟阅读 · 2023 年 12 月 28 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe5b88a57f66&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-systems-of-equations-linear-regression-and-neural-networks-fe5b88a57f66&user=Rohit+Pandey&userId=a743c5fec8cd&source=-----fe5b88a57f66---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe5b88a57f66&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-birds-eye-view-of-linear-algebra-systems-of-equations-linear-regression-and-neural-networks-fe5b88a57f66&source=-----fe5b88a57f66---------------------bookmark_footer-----------)![](img/3c6e139e72e16ccb0e834afec721d153.png)

图像来源于 midjourney

这是正在进行中的线性代数书籍的第四章，“线性代数的全景视角”。到目前为止的目录：

1.  [第一章：基础](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f)

1.  第二章: [映射的度量——行列式](https://medium.com/p/1e5fd752a3be)

1.  第三章: [为什么矩阵乘法是这样的？](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-why-is-matrix-multiplication-like-that-a4d94067651e)

1.  第四章（当前章节）：方程组、线性回归和神经网络

1.  第五章: 秩-零度及为何行秩等于列秩

除非另有说明，本文博客中的所有图片均由作者提供。

# I) 引言

现代 AI 模型利用高维向量空间来编码信息。并且***我们***用来推理高维空间及其之间映射的工具就是线性代数。

在这个领域中，矩阵乘法（及其逆矩阵）实际上是构建许多简单机器学习模型所需的全部工具。这就是为什么花时间来…
