# Scikit-Learn 中的管道：一种捆绑转换的神奇方式

> 原文：[`towardsdatascience.com/pipelines-in-scikit-learn-an-amazing-way-to-bundle-transformations-9ef0594000ac?source=collection_archive---------15-----------------------#2023-04-05`](https://towardsdatascience.com/pipelines-in-scikit-learn-an-amazing-way-to-bundle-transformations-9ef0594000ac?source=collection_archive---------15-----------------------#2023-04-05)

## 管道如何帮助你编写更好的机器学习和数据科学代码 😍

[](https://medium.com/@ebbeberge?source=post_page-----9ef0594000ac--------------------------------)![Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----9ef0594000ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9ef0594000ac--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ef0594000ac--------------------------------) [Eirik Berge, PhD](https://medium.com/@ebbeberge?source=post_page-----9ef0594000ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7722f981eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpipelines-in-scikit-learn-an-amazing-way-to-bundle-transformations-9ef0594000ac&user=Eirik+Berge%2C+PhD&userId=7722f981eb&source=post_page-7722f981eb----9ef0594000ac---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ef0594000ac--------------------------------) · 5 分钟阅读 · 2023 年 4 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9ef0594000ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpipelines-in-scikit-learn-an-amazing-way-to-bundle-transformations-9ef0594000ac&user=Eirik+Berge%2C+PhD&userId=7722f981eb&source=-----9ef0594000ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9ef0594000ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpipelines-in-scikit-learn-an-amazing-way-to-bundle-transformations-9ef0594000ac&source=-----9ef0594000ac---------------------bookmark_footer-----------)![](img/af74d4aa798e6d0abd86bb7284cd5dc3.png)

图片由[Rodion Kutsaiev](https://unsplash.com/fr/@frostroomhead?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 你的旅程概述

1.  引言

1.  没有管道的问题？

1.  管道拯救者！

1.  有用的属性和实用函数

1.  总结

# 1 — 引言

处理机器学习任务的最受欢迎的 Python 库之一是 [scikit-learn](https://scikit-learn.org/stable/index.html)。它于 2010 年公开发布，并且自那时起就对实现流行的监督式机器学习算法如 [逻辑回归](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression)、[随机森林](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier) 和 [支持向量机](https://scikit-learn.org/stable/modules/generated/sklearn.svm.LinearSVR.html#sklearn.svm.LinearSVR) 至关重要。

在使用 scikit-learn 编写代码时，你可以利用一个叫做 **pipelines** 的功能。这个功能允许你将机器学习过程中的几个步骤打包成一个单一的组件。使用 pipelines 是决定 scikit-learn 代码是否易于操作的 *最重要因素之一*。令人沮丧的是，许多人在创建 scikit-learn 机器学习模型时忽视了 pipelines 😞

在这篇博客文章中，你将了解 scikit-learn pipelines 的优点。阅读完后，你应该…
