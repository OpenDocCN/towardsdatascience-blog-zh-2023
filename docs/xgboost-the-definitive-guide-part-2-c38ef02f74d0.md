# XGBoost: 权威指南（第二部分）

> 原文：[https://towardsdatascience.com/xgboost-the-definitive-guide-part-2-c38ef02f74d0?source=collection_archive---------6-----------------------#2023-08-15](https://towardsdatascience.com/xgboost-the-definitive-guide-part-2-c38ef02f74d0?source=collection_archive---------6-----------------------#2023-08-15)

## 从头实现 XGBoost 算法的 Python 代码

[](https://medium.com/@roiyeho?source=post_page-----c38ef02f74d0--------------------------------)[![Dr. Roi Yehoshua](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----c38ef02f74d0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c38ef02f74d0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c38ef02f74d0--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----c38ef02f74d0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-the-definitive-guide-part-2-c38ef02f74d0&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----c38ef02f74d0---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c38ef02f74d0--------------------------------) ·14分钟阅读·2023年8月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc38ef02f74d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-the-definitive-guide-part-2-c38ef02f74d0&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----c38ef02f74d0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc38ef02f74d0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-the-definitive-guide-part-2-c38ef02f74d0&source=-----c38ef02f74d0---------------------bookmark_footer-----------)![](../Images/798f47ae83cd1fe10d15404d5b3ca636.png)

图片由[StockSnap](https://pixabay.com/users/stocksnap-894430/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2557468)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2557468)

在[上一篇文章](https://medium.com/towards-data-science/xgboost-the-definitive-guide-part-1-cc24d2dcd87a)中，我们讨论了 XGBoost 算法，并展示了其伪代码实现。在本文中，我们将从头实现这个算法的 Python 代码。

提供的代码是 XGBoost 算法的一个简洁而轻量的实现（仅约 300 行代码），旨在演示其核心功能。因此，它并未针对速度或内存使用进行优化，也不包括 XGBoost 库提供的全部选项（有关库的更多细节，请参见 [https://xgboost.readthedocs.io/](https://xgboost.readthedocs.io/)）。更具体地说：

1.  代码是用纯 Python 编写的，而 XGBoost 库的核心部分是用 C++ 编写的（它的 Python 类只是对 C++ 实现的薄包装）。

1.  它不包括各种优化，这些优化使 XGBoost 能够处理大量数据，例如加权分位数草图、外部树学习，以及数据的并行和分布式处理。这些优化将在系列的下一篇文章中详细讨论。

1.  该实现目前仅支持回归和二分类任务，而 XGBoost 库还支持……
