# 各种逻辑回归模型中的预测（第一部分）

> 原文：[`towardsdatascience.com/prediction-in-various-logistic-regression-models-2543281cd55a?source=collection_archive---------3-----------------------#2023-04-16`](https://towardsdatascience.com/prediction-in-various-logistic-regression-models-2543281cd55a?source=collection_archive---------3-----------------------#2023-04-16)

## R 统计系列

[](https://mdsohel-mahmood.medium.com/?source=post_page-----2543281cd55a--------------------------------)![Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----2543281cd55a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2543281cd55a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2543281cd55a--------------------------------) [Md Sohel Mahmood](https://mdsohel-mahmood.medium.com/?source=post_page-----2543281cd55a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd4a38d280273&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprediction-in-various-logistic-regression-models-2543281cd55a&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=post_page-d4a38d280273----2543281cd55a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2543281cd55a--------------------------------) ·8 分钟阅读·2023 年 4 月 16 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2543281cd55a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprediction-in-various-logistic-regression-models-2543281cd55a&user=Md+Sohel+Mahmood&userId=d4a38d280273&source=-----2543281cd55a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2543281cd55a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprediction-in-various-logistic-regression-models-2543281cd55a&source=-----2543281cd55a---------------------bookmark_footer-----------)![](img/a729b5d3974e85a463cd88a4e2f3fad9.png)

照片由 [Jen Theodore](https://unsplash.com/@jentheodore?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，发布在 [Unsplash](https://unsplash.com/photos/FaZD0xRotMk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> **介绍**

我们在过去的几篇文章中已经涵盖了各种类型的逻辑回归。这些模型的目标是尽可能准确地预测未来的数据点以及中间数据点。在本文中，我们将探讨如何使用 R 语言对二元和有序数据进行简单和多重逻辑回归的预测分析。

***数据集***

[成人数据集](https://archive.ics.uci.edu/ml/datasets/adult)将作为我们研究的一部分，作为案例研究使用。该数据集收集了超过 30000 个个体的人口统计数据。数据包括每个人的种族、教育、职业、性别、薪水、每周工作小时数、工作职位数量以及他们赚取的收入。

数据集的回顾：

+   *学士学位*：1 表示该人拥有学士学位，0 表示该人没有学士学位

+   *收入大于 50k 代码*：1 表示家庭总收入大于 50k 美元，0 表示家庭总收入少于 50k 美元

+   *婚姻状况代码*：1 表示该人已婚，0 表示该人未婚或…
