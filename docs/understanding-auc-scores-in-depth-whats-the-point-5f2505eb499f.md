# 深入理解AUC分数：这有什么意义？

> 原文：[https://towardsdatascience.com/understanding-auc-scores-in-depth-whats-the-point-5f2505eb499f?source=collection_archive---------9-----------------------#2023-09-02](https://towardsdatascience.com/understanding-auc-scores-in-depth-whats-the-point-5f2505eb499f?source=collection_archive---------9-----------------------#2023-09-02)

## 探索替代指标以获得更深层次的洞察

[](https://medium.com/@MahamsMultiverse?source=post_page-----5f2505eb499f--------------------------------)[![Maham Haroon](../Images/5a9ac82369ecbf7719b765ec160a70ef.png)](https://medium.com/@MahamsMultiverse?source=post_page-----5f2505eb499f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5f2505eb499f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5f2505eb499f--------------------------------) [Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----5f2505eb499f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F398c9514a58b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-auc-scores-in-depth-whats-the-point-5f2505eb499f&user=Maham+Haroon&userId=398c9514a58b&source=post_page-398c9514a58b----5f2505eb499f---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5f2505eb499f--------------------------------) ·11分钟阅读·2023年9月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5f2505eb499f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-auc-scores-in-depth-whats-the-point-5f2505eb499f&user=Maham+Haroon&userId=398c9514a58b&source=-----5f2505eb499f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5f2505eb499f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-auc-scores-in-depth-whats-the-point-5f2505eb499f&source=-----5f2505eb499f---------------------bookmark_footer-----------)![](../Images/3dfdfb8105f958d905f36f02fa339e0a.png)

图片由[Jonathan Greenaway](https://unsplash.com/@jogaway?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/TNUEQ73frkA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你好！

今天，我们将深入探讨一种用于评估模型性能的特定指标——AUC分数。但是在我们深入具体内容之前，你是否曾经想过为什么有时需要使用一些不直观的分数来评估我们的模型表现？

无论我们的模型处理单一类别还是多个类别，基本目标始终不变：优化准确预测，同时最小化错误预测。为了深入了解这一基本目标，我们首先来看看包含真正例、假正例、真负例和假负例的混淆矩阵。

![](../Images/4fc6d99b4ef3d5cf0a1584103cfb21c3.png)

作者提供的图像

> 对于任何分类或预测问题，只有两种结果：真实或虚假。

因此，所有旨在衡量预测或分类算法性能的指标都建立在这两个度量上。完成这一目标的最简单指标是**准确率**。

## 准确率
