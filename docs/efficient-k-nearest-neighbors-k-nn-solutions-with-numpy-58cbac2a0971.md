# 使用 NumPy 实现高效的 k-最近邻 (k-NN) 解决方案

> 原文：[https://towardsdatascience.com/efficient-k-nearest-neighbors-k-nn-solutions-with-numpy-58cbac2a0971?source=collection_archive---------10-----------------------#2023-07-20](https://towardsdatascience.com/efficient-k-nearest-neighbors-k-nn-solutions-with-numpy-58cbac2a0971?source=collection_archive---------10-----------------------#2023-07-20)

## [快速计算](https://medium.com/@qtalen/list/fast-computing-2a37a7e82be5)

## 利用 NumPy 的广播、花式索引和排序进行高效计算

[](https://qtalen.medium.com/?source=post_page-----58cbac2a0971--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----58cbac2a0971--------------------------------)[](https://towardsdatascience.com/?source=post_page-----58cbac2a0971--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----58cbac2a0971--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----58cbac2a0971--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-k-nearest-neighbors-k-nn-solutions-with-numpy-58cbac2a0971&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----58cbac2a0971---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----58cbac2a0971--------------------------------) · 9分钟阅读 · 2023年7月20日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F58cbac2a0971&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-k-nearest-neighbors-k-nn-solutions-with-numpy-58cbac2a0971&user=Peng+Qian&userId=8e2fe735546d&source=-----58cbac2a0971---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F58cbac2a0971&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fefficient-k-nearest-neighbors-k-nn-solutions-with-numpy-58cbac2a0971&source=-----58cbac2a0971---------------------bookmark_footer-----------)![](../Images/bf4eee960dbacc8212da2d156f4d85d0.png)

图片来源：作者创建，[Canva](https://www.canva.com/)

# 引言

我有一个朋友是城市规划师。一天，他被要求重新评估城市中数千个加油站的位置适宜性，需要找到每个加油站最近的 k 个加油站的位置。

我们如何在短时间内找到最近的 k 个站点？这是 k-最近邻问题的一个实际应用场景。

因此，他找到了我，希望我能提供一个高性能的解决方案。

因此，我写下了这篇文章，它将指导你高效地解决使用 NumPy 的 k-近邻问题。通过与 Python 迭代解决方案的比较，我们将展示 NumPy 的强大性能。

在本文中，我们将**深入探讨**如何利用高级 NumPy 功能，如广播、花式索引和排序，来实现高性能的 k-近邻算法。

阅读本文后，你将能够：

+   了解 k-近邻问题及其实际应用……
