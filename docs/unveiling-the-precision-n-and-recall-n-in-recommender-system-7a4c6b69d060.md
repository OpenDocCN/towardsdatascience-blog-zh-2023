# 揭示推荐系统中的 Precision@N 和 Recall@N

> 原文：[https://towardsdatascience.com/unveiling-the-precision-n-and-recall-n-in-recommender-system-7a4c6b69d060?source=collection_archive---------14-----------------------#2023-06-29](https://towardsdatascience.com/unveiling-the-precision-n-and-recall-n-in-recommender-system-7a4c6b69d060?source=collection_archive---------14-----------------------#2023-06-29)

## 优化推荐系统：对精确度和召回率用例的深入解读

[](https://medium.com/@christienatashia?source=post_page-----7a4c6b69d060--------------------------------)[![Christie Natashia](../Images/168aa61f8495c7f3a3eccb880c8a023c.png)](https://medium.com/@christienatashia?source=post_page-----7a4c6b69d060--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a4c6b69d060--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7a4c6b69d060--------------------------------) [Christie Natashia](https://medium.com/@christienatashia?source=post_page-----7a4c6b69d060--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb867d9015fd1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-precision-n-and-recall-n-in-recommender-system-7a4c6b69d060&user=Christie+Natashia&userId=b867d9015fd1&source=post_page-b867d9015fd1----7a4c6b69d060---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a4c6b69d060--------------------------------) ·7 min read·Jun 29, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7a4c6b69d060&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-precision-n-and-recall-n-in-recommender-system-7a4c6b69d060&user=Christie+Natashia&userId=b867d9015fd1&source=-----7a4c6b69d060---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7a4c6b69d060&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-precision-n-and-recall-n-in-recommender-system-7a4c6b69d060&source=-----7a4c6b69d060---------------------bookmark_footer-----------)![](../Images/82d0300f568aaadbebfc1c9e9808d001.png)

照片来源：[Norbert Braun](https://unsplash.com/@medion4you?utm_source=medium&utm_medium=referral) 通过 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**主要讨论的话题包括：**

1.  一句话概括的精确度和召回率

1.  将精确度和召回率定义应用于推荐系统用例

+   二进制偏好转换的必要性

+   不切实际的召回率问题

+   解决不切实际的召回率问题：Top-N 项目

+   一个说明性的实现

+   代码实现

# **介绍**

准确率指标（Accuracy Metrics）是评估机器学习整体性能的有用指标，它表示数据集中正确分类实例的比例。结合准确率使用的评估指标，如精确率和召回率，可以更全面地理解模型的表现。

一般来说，精确率（Precision）和召回率（Recall）比较预测类别与测试集的实际类别，并计算正确预测的比例与总预测次数的比值。

# 分类问题中的精确率和召回率
