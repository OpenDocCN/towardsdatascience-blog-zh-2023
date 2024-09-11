# 我真的应该吃那种蘑菇吗？

> 原文：[https://towardsdatascience.com/should-i-really-eat-that-mushroom-9edeaa69d934?source=collection_archive---------10-----------------------#2023-08-17](https://towardsdatascience.com/should-i-really-eat-that-mushroom-9edeaa69d934?source=collection_archive---------10-----------------------#2023-08-17)

## 使用 CatBoost 梯度提升决策树对可食用和有毒蘑菇进行分类

[](https://medium.com/@caroline.arnold_63207?source=post_page-----9edeaa69d934--------------------------------)[![Caroline Arnold](../Images/2560e106ba9deda7889c7d253792d814.png)](https://medium.com/@caroline.arnold_63207?source=post_page-----9edeaa69d934--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9edeaa69d934--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9edeaa69d934--------------------------------) [Caroline Arnold](https://medium.com/@caroline.arnold_63207?source=post_page-----9edeaa69d934--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9367198e7a3c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-i-really-eat-that-mushroom-9edeaa69d934&user=Caroline+Arnold&userId=9367198e7a3c&source=post_page-9367198e7a3c----9edeaa69d934---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9edeaa69d934--------------------------------) ·6分钟阅读·2023年8月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9edeaa69d934&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-i-really-eat-that-mushroom-9edeaa69d934&user=Caroline+Arnold&userId=9367198e7a3c&source=-----9edeaa69d934---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9edeaa69d934&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshould-i-really-eat-that-mushroom-9edeaa69d934&source=-----9edeaa69d934---------------------bookmark_footer-----------)

大多数教育性和现实世界的数据集包含*分类特征*。今天我们将介绍来自 [CatBoost](https://catboost.ai) 库的梯度提升决策树，该库原生支持分类数据。我们将使用一个包含可食用和有毒蘑菇的数据集。这些蘑菇通过其颜色、气味和形状等分类特征进行描述，我们要回答的问题是：

> 根据其分类特征，吃这种蘑菇安全吗？

如你所见，风险很高。我们希望确保机器学习模型的准确性，以免我们的蘑菇煎蛋卷以灾难告终。**作为额外奖励，最后我们将提供一个特征重要性排名，告诉你*哪个分类特征*是蘑菇安全性的最强预测因子。**

![](../Images/9cdc72d1005c4c8a46835b140da5f21e.png)

照片由[Andrew Ridley](https://unsplash.com/@aridley88?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

## 介绍蘑菇数据集

蘑菇数据集可以在这里找到：[https://archive.ics.uci.edu/dataset/73/mushroom](https://archive.ics.uci.edu/dataset/73/mushroom) [1]。为了清晰展示，我们从原始的简短变量中创建一个[pandas DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)，并用适当的列名和长格式变量进行了注释。我们使用pandas的`replace`函数来处理长格式...
