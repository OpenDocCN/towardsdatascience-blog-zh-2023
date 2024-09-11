# XGBoost: 权威指南（第1部分）

> 原文：[https://towardsdatascience.com/xgboost-the-definitive-guide-part-1-cc24d2dcd87a?source=collection_archive---------4-----------------------#2023-08-09](https://towardsdatascience.com/xgboost-the-definitive-guide-part-1-cc24d2dcd87a?source=collection_archive---------4-----------------------#2023-08-09)

## 对流行的XGBoost算法进行逐步推导，包括详细的数值说明

[](https://medium.com/@roiyeho?source=post_page-----cc24d2dcd87a--------------------------------)[![Dr. Roi Yehoshua](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----cc24d2dcd87a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cc24d2dcd87a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cc24d2dcd87a--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----cc24d2dcd87a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-the-definitive-guide-part-1-cc24d2dcd87a&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----cc24d2dcd87a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cc24d2dcd87a--------------------------------) · 15分钟阅读 · 2023年8月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcc24d2dcd87a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-the-definitive-guide-part-1-cc24d2dcd87a&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----cc24d2dcd87a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcc24d2dcd87a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-the-definitive-guide-part-1-cc24d2dcd87a&source=-----cc24d2dcd87a---------------------bookmark_footer-----------)![](../Images/6146a3f112054a1a9ea8ef91d58e7b4e.png)

图片由 [Sam Battaglieri](https://unsplash.com/@st_b?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/_PXtCCQ4Dj0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

XGBoost（即eXtreme Gradient Boosting的缩写）是一个开源库，提供了梯度提升决策树的优化和可扩展实现。它结合了各种软件和硬件优化技术，使其能够处理海量数据。

XGBoost 最初由 Tianqi Chen 和 Carlos Guestrin 于 2016 年作为研究项目开发 [1]，现在已经成为解决结构化（表格）数据上的监督学习任务的首选方案。它在许多标准回归和分类任务上提供了最先进的结果，许多 Kaggle 竞赛的获胜者将 XGBoost 作为其获胜方案的一部分。

尽管在处理表格数据时，深度神经网络取得了显著进展，但在许多标准基准测试中，它们仍被 XGBoost 和其他基于树的模型所超越 [2, 3]。此外，XGBoost 的调整需求远低于深度模型。

XGBoost 相较于其他梯度提升算法的主要创新包括：

1.  聪明的决策树正则化。

1.  使用二阶近似进行优化……
