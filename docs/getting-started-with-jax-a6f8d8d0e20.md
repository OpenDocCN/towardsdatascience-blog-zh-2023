# 开始使用JAX

> 原文：[https://towardsdatascience.com/getting-started-with-jax-a6f8d8d0e20?source=collection_archive---------15-----------------------#2023-07-07](https://towardsdatascience.com/getting-started-with-jax-a6f8d8d0e20?source=collection_archive---------15-----------------------#2023-07-07)

## 驱动高性能数值计算和机器学习研究的未来

[](https://pierpaoloippolito28.medium.com/?source=post_page-----a6f8d8d0e20--------------------------------)[![Pier Paolo Ippolito](../Images/981abb84149adab275473b76bdbde66f.png)](https://pierpaoloippolito28.medium.com/?source=post_page-----a6f8d8d0e20--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a6f8d8d0e20--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a6f8d8d0e20--------------------------------) [Pier Paolo Ippolito](https://pierpaoloippolito28.medium.com/?source=post_page-----a6f8d8d0e20--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb8391a6a5f1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-jax-a6f8d8d0e20&user=Pier+Paolo+Ippolito&userId=b8391a6a5f1a&source=post_page-b8391a6a5f1a----a6f8d8d0e20---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a6f8d8d0e20--------------------------------) ·5分钟阅读·2023年7月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa6f8d8d0e20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-jax-a6f8d8d0e20&user=Pier+Paolo+Ippolito&userId=b8391a6a5f1a&source=-----a6f8d8d0e20---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa6f8d8d0e20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-jax-a6f8d8d0e20&source=-----a6f8d8d0e20---------------------bookmark_footer-----------)![](../Images/17e806ec59ab7110eb1eb8acbdca9117.png)

图片由 [Lance Asper](https://unsplash.com/@lance_asper?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

JAX是Google开发的一个Python库，用于在任何类型的设备（CPU、GPU、TPU等）上进行高性能数值计算。JAX的主要应用之一是机器学习和深度学习的研究开发，尽管该库主要设计用于提供执行通用科学计算任务（高维矩阵运算等）的所有必要功能。

考虑到特别关注高性能计算，JAX 被设计得非常快速，基于 XLA（加速线性代数）构建。XLA 实际上是一个编译器，旨在加速线性代数操作，并且也可以在 TensorFlow 和 Pytorch 等其他框架的背后工作。此外，JAX 数组被设计成遵循与 Numpy 相同的原则，使得将旧的 Numpy 代码迁移到 JAX 非常容易，并通过 GPUs 和 TPUs 利用性能提升。

JAX 的一些主要特点包括：

+   **即时编译（JIT）**：JIT 和加速硬件使 JAX 比普通的 Numpy 快得多。使用 *jit()* 函数可以实现编译和缓存…
