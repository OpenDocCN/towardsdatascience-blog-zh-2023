# 序列特征选择的实际介绍

> 原文：[`towardsdatascience.com/a-practical-introduction-to-sequential-feature-selection-a5444eb5b2fd?source=collection_archive---------11-----------------------#2023-02-16`](https://towardsdatascience.com/a-practical-introduction-to-sequential-feature-selection-a5444eb5b2fd?source=collection_archive---------11-----------------------#2023-02-16)

## 对这种不同寻常的特征选择技术的深入探讨

[](https://gianlucamalato.medium.com/?source=post_page-----a5444eb5b2fd--------------------------------)![Gianluca Malato](https://gianlucamalato.medium.com/?source=post_page-----a5444eb5b2fd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a5444eb5b2fd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a5444eb5b2fd--------------------------------) [Gianluca Malato](https://gianlucamalato.medium.com/?source=post_page-----a5444eb5b2fd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1f015e9ee21d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-sequential-feature-selection-a5444eb5b2fd&user=Gianluca+Malato&userId=1f015e9ee21d&source=post_page-1f015e9ee21d----a5444eb5b2fd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a5444eb5b2fd--------------------------------) ·4 分钟阅读·2023 年 2 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa5444eb5b2fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-sequential-feature-selection-a5444eb5b2fd&user=Gianluca+Malato&userId=1f015e9ee21d&source=-----a5444eb5b2fd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa5444eb5b2fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-practical-introduction-to-sequential-feature-selection-a5444eb5b2fd&source=-----a5444eb5b2fd---------------------bookmark_footer-----------)![](img/0c29dc7bc61b88a6b4adf9cde8fd61cf.png)

*Robert Stump* 的照片[*Robert Stump*](https://unsplash.com/@stumpie10?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *在* [*Unsplash*](https://unsplash.com/it/foto/pQyTChJwEDI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

特征选择对数据科学家来说总是一个具有挑战性的任务。识别正确的特征集对模型的成功至关重要。有几种技术利用特征集对模型的表现。其中之一就是序列特征选择。

# 什么是序列特征选择？

顺序特征选择是一种监督式特征选择方法。它利用一个监督模型，并可以用来从大型数据集中删除无用特征或通过逐步添加来选择有用特征。

该算法按照以下步骤进行工作：

+   从数据集中选择能够最大化你模型在 k 折交叉验证中平均性能的特征。这个数据集由一个特征组成。

+   根据相同的原则（最大化模型的 CV 性能），向数据集中添加第二个特征。

+   继续向数据集中添加特征，直到达到所需的特征数量或性能没有显著提升为止。
