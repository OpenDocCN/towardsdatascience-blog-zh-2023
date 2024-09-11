# 如何准备机器学习的数据

> 原文：[`towardsdatascience.com/how-to-prepare-data-for-machine-learning-eb9d9973832f?source=collection_archive---------7-----------------------#2023-04-19`](https://towardsdatascience.com/how-to-prepare-data-for-machine-learning-eb9d9973832f?source=collection_archive---------7-----------------------#2023-04-19)

## *准备建模数据是数据科学流程中最基本的第一步之一*

[](https://medium.com/@theDrewDag?source=post_page-----eb9d9973832f--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----eb9d9973832f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb9d9973832f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb9d9973832f--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----eb9d9973832f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-prepare-data-for-machine-learning-eb9d9973832f&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----eb9d9973832f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb9d9973832f--------------------------------) ·8 分钟阅读·2023 年 4 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb9d9973832f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-prepare-data-for-machine-learning-eb9d9973832f&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----eb9d9973832f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb9d9973832f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-prepare-data-for-machine-learning-eb9d9973832f&source=-----eb9d9973832f---------------------bookmark_footer-----------)![](img/a4201ec0fe98ad642b92ad48c8e24ac9.png)

图片由 [John Moeses Bauan](https://unsplash.com/it/@johnmoeses?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) 提供

训练预测模型要求我们的数据处于适当的格式。我们不能直接将 `.csv` 文件输入模型，并期望模型能够正确地进行泛化学习。

在这篇文章中，我们将探讨如何为机器学习准备数据，从数据准备流程开始，到将数据分为训练集、验证集和测试集。

# 数据准备流程

管道是一系列步骤，执行这些步骤是为了准备数据供机器学习模型处理。管道可能会根据你拥有的数据类型有所不同，但通常包括以下步骤：

**数据收集**：第一步是收集你想用来训练机器学习模型的数据。这些数据可以来自不同的来源，例如 CSV 文件、数据库、API、物联网传感器等。

**数据清洗**：一旦数据收集完成，重要的是通过删除缺失数据、纠正错误以及去除重复或不相关的数据来清理数据。数据清洗有助于提高用于训练机器学习模型的数据质量。
