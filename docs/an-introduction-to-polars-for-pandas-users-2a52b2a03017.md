# Pandas 用户的 Polars 简介

> 原文：[https://towardsdatascience.com/an-introduction-to-polars-for-pandas-users-2a52b2a03017?source=collection_archive---------0-----------------------#2023-03-05](https://towardsdatascience.com/an-introduction-to-polars-for-pandas-users-2a52b2a03017?source=collection_archive---------0-----------------------#2023-03-05)

## 演示如何使用新的极快 DataFrame 库与表格数据进行交互

[](https://dkhundley.medium.com/?source=post_page-----2a52b2a03017--------------------------------)[![David Hundley](../Images/1779ef96ec3d338f8fe4a9567ba7b194.png)](https://dkhundley.medium.com/?source=post_page-----2a52b2a03017--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2a52b2a03017--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2a52b2a03017--------------------------------) [David Hundley](https://dkhundley.medium.com/?source=post_page-----2a52b2a03017--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F82498630db6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-polars-for-pandas-users-2a52b2a03017&user=David+Hundley&userId=82498630db6&source=post_page-82498630db6----2a52b2a03017---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2a52b2a03017--------------------------------) ·17 分钟阅读·2023年3月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2a52b2a03017&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-polars-for-pandas-users-2a52b2a03017&user=David+Hundley&userId=82498630db6&source=-----2a52b2a03017---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2a52b2a03017&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-polars-for-pandas-users-2a52b2a03017&source=-----2a52b2a03017---------------------bookmark_footer-----------)![](../Images/703b01459d9d9be0836f9fc0c45e973d.png)

标题卡由作者创建

如果你和我一样，可能会听到很多关于这个新的 [Polars](https://www.pola.rs) 库的宣传，但不确定它是什么或如何入门。如果你完全陌生于它，理解 Polars 最简单的方式就是它是一个非常快速的替代品，用于传统的 Pandas DataFrame 库。我们将专注于本篇文章中的 Polars 的 Python 实现，但要注意，它也被编写为与日益流行的 Rust 语言兼容。

在继续之前，让我首先表达我对任何新软件的谨慎乐观。总是存在这样一个大问题：“这会变得主流吗？”我不幸地看到过太多次，一个非常酷的软件在初期获得了大量宣传，但后来却逐渐消失。关于Polars，我认为现在还远远不能对其长期前景做出判断，但我会在这篇文章的最后对我个人对Polars的看法进行评估。

这份入门指南专门为已经熟悉Pandas库的人编写，我将直接比较/对比Polars和Pandas的语法及性能。如果你想要...
