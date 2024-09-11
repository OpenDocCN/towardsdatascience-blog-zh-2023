# 使用 Python 和 Sklearn 创建动画以展示 4 种基于质心的聚类算法

> 原文：[`towardsdatascience.com/creating-animation-to-show-4-centroid-based-clustering-algorithms-using-python-and-sklearn-d397ade89cb3?source=collection_archive---------5-----------------------#2023-08-16`](https://towardsdatascience.com/creating-animation-to-show-4-centroid-based-clustering-algorithms-using-python-and-sklearn-d397ade89cb3?source=collection_archive---------5-----------------------#2023-08-16)

## 使用数据可视化和动画来理解 4 种基于质心的聚类算法的过程。

[](https://medium.com/@borih.k?source=post_page-----d397ade89cb3--------------------------------)![Boriharn K](https://medium.com/@borih.k?source=post_page-----d397ade89cb3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d397ade89cb3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d397ade89cb3--------------------------------) [Boriharn K](https://medium.com/@borih.k?source=post_page-----d397ade89cb3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe20a7f1ba78f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-animation-to-show-4-centroid-based-clustering-algorithms-using-python-and-sklearn-d397ade89cb3&user=Boriharn+K&userId=e20a7f1ba78f&source=post_page-e20a7f1ba78f----d397ade89cb3---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d397ade89cb3--------------------------------) ·9 分钟阅读·2023 年 8 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd397ade89cb3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-animation-to-show-4-centroid-based-clustering-algorithms-using-python-and-sklearn-d397ade89cb3&user=Boriharn+K&userId=e20a7f1ba78f&source=-----d397ade89cb3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd397ade89cb3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-animation-to-show-4-centroid-based-clustering-algorithms-using-python-and-sklearn-d397ade89cb3&source=-----d397ade89cb3---------------------bookmark_footer-----------)![](img/25a9aac387589c5c2e52ada5e0acd659.png)

照片由 [Mel Poole](https://unsplash.com/@melpoole?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 聚类分析

聚类分析是一种有效的机器学习技术，通过数据的相似性和差异性对数据进行分组。获得的数据组可以用于多种目的，如分段、结构化和决策。

执行聚类分析有许多方法，基于不同的算法。本文将主要关注基于质心的聚类，这是一种常见且有用的技术。

# 基于质心的聚类

基本上，基于质心的技术通过反复计算来获得最佳质心（聚类中心），然后将数据点分配给最近的质心。

由于需要进行许多迭代，可以使用数据可视化来表达过程中的情况。因此，本文的目的是创建动画，展示使用 Python 和 Sklearn 的基于质心的过程。
