# “近似预测”使特征选择变得极快

> 原文：[`towardsdatascience.com/approximate-predictions-make-feature-selection-radically-faster-0f9664877687?source=collection_archive---------1-----------------------#2023-11-17`](https://towardsdatascience.com/approximate-predictions-make-feature-selection-radically-faster-0f9664877687?source=collection_archive---------1-----------------------#2023-11-17)

## 特征选择之所以如此缓慢，是因为它需要创建许多模型。了解如何通过近似预测让它变得非常快速。

[](https://medium.com/@mazzanti.sam?source=post_page-----0f9664877687--------------------------------)![Samuele Mazzanti](https://medium.com/@mazzanti.sam?source=post_page-----0f9664877687--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0f9664877687--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----0f9664877687--------------------------------) [Samuele Mazzanti](https://medium.com/@mazzanti.sam?source=post_page-----0f9664877687--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe16f3bb86e03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapproximate-predictions-make-feature-selection-radically-faster-0f9664877687&user=Samuele+Mazzanti&userId=e16f3bb86e03&source=post_page-e16f3bb86e03----0f9664877687---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0f9664877687--------------------------------) ·10 min 阅读·2023 年 11 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0f9664877687&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapproximate-predictions-make-feature-selection-radically-faster-0f9664877687&user=Samuele+Mazzanti&userId=e16f3bb86e03&source=-----0f9664877687---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0f9664877687&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fapproximate-predictions-make-feature-selection-radically-faster-0f9664877687&source=-----0f9664877687---------------------bookmark_footer-----------)![](img/966d9bfba9303c9a87213f6ba5d91239.png)

[作者提供的图片]

在开发机器学习模型时，我们通常从一个大规模的特征集合开始，这些特征是我们特征工程工作的结果。

**特征选择是选择对我们的机器学习模型最优的小部分特征的过程**。

为什么要这么做，而不是仅仅保留所有特征呢？

+   **内存**。大数据占用大空间。舍弃特征意味着处理数据时需要更少的内存。有时也会存在外部约束。

+   **时间**。在少量数据上重新训练模型可以节省大量时间。

+   **准确性**。少即是多：这对机器学习同样适用。包含冗余或无关特征意味着引入不必要的噪音。经常情况下，用少量数据训练的模型表现更好。

+   **可解释性**。一个更小的模型更容易解释。

+   **调试**。一个更小的模型更易于维护和排错。
