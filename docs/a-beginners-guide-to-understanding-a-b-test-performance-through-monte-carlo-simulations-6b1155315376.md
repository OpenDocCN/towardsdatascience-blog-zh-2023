# 《初学者理解 A/B 测试性能的蒙特卡罗模拟指南》

> 原文：[https://towardsdatascience.com/a-beginners-guide-to-understanding-a-b-test-performance-through-monte-carlo-simulations-6b1155315376?source=collection_archive---------3-----------------------#2023-08-05](https://towardsdatascience.com/a-beginners-guide-to-understanding-a-b-test-performance-through-monte-carlo-simulations-6b1155315376?source=collection_archive---------3-----------------------#2023-08-05)

[](https://idajohnsson.medium.com/?source=post_page-----6b1155315376--------------------------------)[![Ida Johnsson, PhD](../Images/f0af7e8c4af98d7af4cd17a2e5d7ea70.png)](https://idajohnsson.medium.com/?source=post_page-----6b1155315376--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b1155315376--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6b1155315376--------------------------------) [Ida Johnsson, PhD](https://idajohnsson.medium.com/?source=post_page-----6b1155315376--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F189928d77dfb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-understanding-a-b-test-performance-through-monte-carlo-simulations-6b1155315376&user=Ida+Johnsson%2C+PhD&userId=189928d77dfb&source=post_page-189928d77dfb----6b1155315376---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b1155315376--------------------------------) · 16分钟阅读 · 2023年8月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6b1155315376&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-understanding-a-b-test-performance-through-monte-carlo-simulations-6b1155315376&user=Ida+Johnsson%2C+PhD&userId=189928d77dfb&source=-----6b1155315376---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6b1155315376&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-beginners-guide-to-understanding-a-b-test-performance-through-monte-carlo-simulations-6b1155315376&source=-----6b1155315376---------------------bookmark_footer-----------)

本教程探讨了协变量如何影响随机实验中的 A/B 测试精度。一个适当随机化的 A/B 测试通过比较处理组和对照组的平均结果来计算提升。然而，除了处理以外的其他特征对结果的影响决定了 A/B 测试的统计特性。例如，在测试提升计算中遗漏有影响的特征，即使样本量增加，提升的估计值也可能高度不精确，尽管它会趋近于真实值。

你将学习 RMSE、偏差以及测试的大小，并通过生成模拟数据和运行蒙特卡罗实验来了解 A/B 测试的性能。这类工作有助于理解数据生成过程（DGP）的特性如何影响 A/B 测试性能，并将帮助你将这种理解应用于实际数据的 A/B 测试。首先，我们讨论一些估计器的基本统计特性。

# **估计器的统计特性**

## 均方根误差（RMSE）

RMSE（均方根误差）：RMSE 是衡量模型或估计器预测值与观察值之间差异的常用指标。它是预测值与实际观察值之间平均平方差的平方根。RMSE 的公式为：
