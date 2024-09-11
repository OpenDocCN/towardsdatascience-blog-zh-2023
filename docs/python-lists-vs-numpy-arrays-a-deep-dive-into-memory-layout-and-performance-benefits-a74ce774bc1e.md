# Python 列表与 NumPy 数组：深入探讨内存布局与性能优势

> 原文：[https://towardsdatascience.com/python-lists-vs-numpy-arrays-a-deep-dive-into-memory-layout-and-performance-benefits-a74ce774bc1e?source=collection_archive---------11-----------------------#2023-07-14](https://towardsdatascience.com/python-lists-vs-numpy-arrays-a-deep-dive-into-memory-layout-and-performance-benefits-a74ce774bc1e?source=collection_archive---------11-----------------------#2023-07-14)

## [快速计算](https://medium.com/@qtalen/list/fast-computing-2a37a7e82be5)

## 探索分配差异和效率提升

[](https://qtalen.medium.com/?source=post_page-----a74ce774bc1e--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----a74ce774bc1e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a74ce774bc1e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a74ce774bc1e--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----a74ce774bc1e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-lists-vs-numpy-arrays-a-deep-dive-into-memory-layout-and-performance-benefits-a74ce774bc1e&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----a74ce774bc1e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a74ce774bc1e--------------------------------) ·9分钟阅读·2023年7月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa74ce774bc1e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-lists-vs-numpy-arrays-a-deep-dive-into-memory-layout-and-performance-benefits-a74ce774bc1e&user=Peng+Qian&userId=8e2fe735546d&source=-----a74ce774bc1e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa74ce774bc1e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-lists-vs-numpy-arrays-a-deep-dive-into-memory-layout-and-performance-benefits-a74ce774bc1e&source=-----a74ce774bc1e---------------------bookmark_footer-----------)![](../Images/9c59152b04be4c0785841172da617a50.png)

NumPy 数组中的数据被紧凑地排列，就像书架上的书一样。照片由[Eliabe Costa](https://unsplash.com/@eliabevces?utm_source=medium&utm_medium=referral)拍摄，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供。

在本文中，我们将深入探讨本地 Python 列表和 NumPy 数组之间的内存设计差异，揭示为什么 NumPy 在许多情况下能提供更好的性能。

我们将比较数据结构、内存分配和访问方法，展示 NumPy 数组的强大功能。

# 介绍

想象一下你正在准备去图书馆找一本书。现在，你发现图书馆有两个书架：

第一个书架上摆满了各种精美的盒子，有些里面放着光盘，有些放着图片，还有些放着书籍。盒子上仅贴有物品的名称。

这代表了本地 Python 列表，其中每个元素都有其内存空间和类型信息。

然而，这种方法有一个问题：盒子中有许多空闲的空间，浪费了书架空间。而且，当你想找一本特定的书时，你必须检查每一个盒子，这会浪费额外的时间。

现在让我们看一下第二个书架。这次没有盒子；书籍、光盘和图片都…
