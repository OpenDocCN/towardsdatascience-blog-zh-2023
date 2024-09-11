# 贝塔分布：贝叶斯校准的基石

> 原文：[https://towardsdatascience.com/beta-distributions-a-cornerstone-of-bayesian-calibration-801f96e21498?source=collection_archive---------4-----------------------#2023-10-28](https://towardsdatascience.com/beta-distributions-a-cornerstone-of-bayesian-calibration-801f96e21498?source=collection_archive---------4-----------------------#2023-10-28)

## 探索贝塔分布在贝叶斯推断中的多功能性

[](https://medium.com/@MahamsMultiverse?source=post_page-----801f96e21498--------------------------------)[![Maham Haroon](../Images/5a9ac82369ecbf7719b765ec160a70ef.png)](https://medium.com/@MahamsMultiverse?source=post_page-----801f96e21498--------------------------------)[](https://towardsdatascience.com/?source=post_page-----801f96e21498--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----801f96e21498--------------------------------) [Maham Haroon](https://medium.com/@MahamsMultiverse?source=post_page-----801f96e21498--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F398c9514a58b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeta-distributions-a-cornerstone-of-bayesian-calibration-801f96e21498&user=Maham+Haroon&userId=398c9514a58b&source=post_page-398c9514a58b----801f96e21498---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----801f96e21498--------------------------------) · 9分钟阅读 · 2023年10月28日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F801f96e21498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeta-distributions-a-cornerstone-of-bayesian-calibration-801f96e21498&source=-----801f96e21498---------------------bookmark_footer-----------)![](../Images/fc1f75d3aabe4b9031947d45a476263e.png)

图片由 [Google DeepMind](https://unsplash.com/@googledeepmind?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，拍摄于 [Unsplash](https://unsplash.com/photos/6Y4EzfSP5Tc?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

嗨，大家好！

分布乍看起来可能并不是一个复杂的概念，但它们在数据分析和统计学的世界中却是非常强大且基础的。可以这样想：如果你收集了50件不同尺寸和颜色的衬衫，你就创建了一个颜色分布、一个尺寸分布，甚至可能还有一个“这件衬衫让你烦恼的程度”分布（当然是开玩笑的）。关键是，只要你有一个可以测量的类别，就总有一个分布等待被探索。

那么，分布到底是什么呢？它本质上是一种展示类别如何在概率或可能性范围内分布的方式。你可以通过你拥有的数据或对特定主题的了解来弄清楚这一点。你可能听说过正态分布、偏斜分布、长尾分布等术语——这些术语描述了数据点的形状。

今天我想谈谈Beta分布，特别是它在贝叶斯校准中的应用。贝叶斯校准是一种通过新数据更新贝叶斯推断的方法，以找到模型的最佳拟合值……
