# 图像数据中的非负矩阵分解（NMF）用于降维

> 原文：[`towardsdatascience.com/non-negative-matrix-factorization-nmf-for-dimensionality-reduction-in-image-data-8450f4cae8fa?source=collection_archive---------2-----------------------#2023-05-06`](https://towardsdatascience.com/non-negative-matrix-factorization-nmf-for-dimensionality-reduction-in-image-data-8450f4cae8fa?source=collection_archive---------2-----------------------#2023-05-06)

## 讨论理论和使用 Python 与 Scikit-learn 实现

[](https://rukshanpramoditha.medium.com/?source=post_page-----8450f4cae8fa--------------------------------)![Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----8450f4cae8fa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8450f4cae8fa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8450f4cae8fa--------------------------------) [Rukshan Pramoditha](https://rukshanpramoditha.medium.com/?source=post_page-----8450f4cae8fa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff90a3bb1d400&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnon-negative-matrix-factorization-nmf-for-dimensionality-reduction-in-image-data-8450f4cae8fa&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=post_page-f90a3bb1d400----8450f4cae8fa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8450f4cae8fa--------------------------------) ·9 分钟阅读·2023 年 5 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8450f4cae8fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnon-negative-matrix-factorization-nmf-for-dimensionality-reduction-in-image-data-8450f4cae8fa&user=Rukshan+Pramoditha&userId=f90a3bb1d400&source=-----8450f4cae8fa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8450f4cae8fa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnon-negative-matrix-factorization-nmf-for-dimensionality-reduction-in-image-data-8450f4cae8fa&source=-----8450f4cae8fa---------------------bookmark_footer-----------)![](img/85529f41284031529617ac661c1a911b.png)

原始图像由 [an_photos](https://pixabay.com/users/an_photos-3160435/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4503287) 提供，来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=4503287)（作者略微编辑）

我已经详细讨论了不同类型的降维技术。

**主成分分析（PCA）**、**因子分析（FA）**、**线性判别分析（LDA）**、**自编码器（AEs）** 和 **核主成分分析（Kernel PCA）** 是最流行的方法之一。

非负矩阵分解（NMF 或 NNMF）也是一种*线性*降维技术，可用于减少特征矩阵的维度。

所有的降维技术都属于无监督机器学习的范畴，可以在不需要标签的情况下揭示数据中的隐藏模式和重要关系。

因此，降维算法处理未标记的数据。在训练这种算法时，**fit()** 方法只需要特征矩阵 **X** 作为输入，而不需要标签列 **y**。

正如其名称所示，非负矩阵分解（NMF）需要特征矩阵是非负的。

由于这种非负约束，NMF 的使用仅限于具有非负值的数据，如图像数据（像素...
