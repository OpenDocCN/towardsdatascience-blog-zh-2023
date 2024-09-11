# Python 中的监督与非监督主题建模方法

> 原文：[`towardsdatascience.com/supervised-unsupervised-approach-to-topic-modelling-in-python-d03e0b9da1dc?source=collection_archive---------10-----------------------#2023-01-31`](https://towardsdatascience.com/supervised-unsupervised-approach-to-topic-modelling-in-python-d03e0b9da1dc?source=collection_archive---------10-----------------------#2023-01-31)

## 从头开始在 Python 中构建主题建模管道

[](https://vatsal12-p.medium.com/?source=post_page-----d03e0b9da1dc--------------------------------)![Vatsal](https://vatsal12-p.medium.com/?source=post_page-----d03e0b9da1dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d03e0b9da1dc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d03e0b9da1dc--------------------------------) [Vatsal](https://vatsal12-p.medium.com/?source=post_page-----d03e0b9da1dc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1c849b1a8ec0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupervised-unsupervised-approach-to-topic-modelling-in-python-d03e0b9da1dc&user=Vatsal&userId=1c849b1a8ec0&source=post_page-1c849b1a8ec0----d03e0b9da1dc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d03e0b9da1dc--------------------------------) ·11 分钟阅读·2023 年 1 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd03e0b9da1dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupervised-unsupervised-approach-to-topic-modelling-in-python-d03e0b9da1dc&user=Vatsal&userId=1c849b1a8ec0&source=-----d03e0b9da1dc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd03e0b9da1dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsupervised-unsupervised-approach-to-topic-modelling-in-python-d03e0b9da1dc&source=-----d03e0b9da1dc---------------------bookmark_footer-----------)![](img/0091d080160c43289a310e2c3495bcb5.png)

图片来源于 [Unsplash](https://unsplash.com/photos/c9OfrVeD_tQ) 由 [v2osk](https://unsplash.com/@v2osk)

本文将提供主题建模及其相关应用的高级直观理解。它将深入探讨解决需要主题建模的问题的各种方法，以及如何以监督和非监督的方式解决这些问题。我强调了对数据和初始问题进行重构，以便可以采用多种方法来执行解决方案。下表分解了文章中的内容。

**目录**

+   什么是主题建模？

+   主题建模的应用

+   监督学习与无监督学习

+   问题拆解

+   需求

+   数据

    - 加载数据

    - 清洗与预处理

    - 数据统计

+   无监督学习

    - 训练模型

    - 可视化

    - 主题分析

+   监督学习

    - 关键词统计

    - 生成标签

    - 训练模型

    - 评估

+   总结

+   资源

# 什么是主题建模？
