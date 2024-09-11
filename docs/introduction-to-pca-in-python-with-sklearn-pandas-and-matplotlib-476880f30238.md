# 使用 Sklearn、Pandas 和 Matplotlib 的 Python 中的 PCA 介绍

> 原文：[`towardsdatascience.com/introduction-to-pca-in-python-with-sklearn-pandas-and-matplotlib-476880f30238?source=collection_archive---------2-----------------------#2023-09-06`](https://towardsdatascience.com/introduction-to-pca-in-python-with-sklearn-pandas-and-matplotlib-476880f30238?source=collection_archive---------2-----------------------#2023-09-06)

## *通过将多维数据集转换为任意数量的维度并使用 Matplotlib 可视化降维数据，了解 PCA 在 Python 和 Sklearn 中的直觉*

[](https://medium.com/@theDrewDag?source=post_page-----476880f30238--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----476880f30238--------------------------------)[](https://towardsdatascience.com/?source=post_page-----476880f30238--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----476880f30238--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----476880f30238--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pca-in-python-with-sklearn-pandas-and-matplotlib-476880f30238&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----476880f30238---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----476880f30238--------------------------------) · 13 分钟阅读 · 2023 年 9 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F476880f30238&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pca-in-python-with-sklearn-pandas-and-matplotlib-476880f30238&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----476880f30238---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F476880f30238&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pca-in-python-with-sklearn-pandas-and-matplotlib-476880f30238&source=-----476880f30238---------------------bookmark_footer-----------)![](img/acfc1cd8e8829588471b9dc0c9f7508a.png)

图片由 [Nivenn Lanos](https://unsplash.com/@nivenn?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

作为数据分析师和科学家，我们经常面临由于信息量不断增长而带来的复杂挑战。

不可否认，从各种来源积累的数据已经成为我们生活中的常态。不论是否是数据科学家，**每个人几乎都将现象描述为变量或属性的集合**。

很少能在没有处理多维数据集的情况下解决分析挑战——这一点在今天尤为明显，因为数据收集日益自动化，技术使我们能够从广泛的来源获取信息，包括**传感器、物联网设备、社交媒体、在线交易等**。

但是，随着现象的复杂性增加，数据科学家需要面对的挑战也随之增加，以实现其目标。

这些挑战可能包括……

+   **高维度**：拥有许多列可能导致……
