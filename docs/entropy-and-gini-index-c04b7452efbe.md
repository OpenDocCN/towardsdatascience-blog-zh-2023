# 熵和基尼指数简介

> 原文：[`towardsdatascience.com/entropy-and-gini-index-c04b7452efbe?source=collection_archive---------2-----------------------#2023-11-05`](https://towardsdatascience.com/entropy-and-gini-index-c04b7452efbe?source=collection_archive---------2-----------------------#2023-11-05)

## 理解这些措施如何帮助我们量化数据集中的不确定性

[](https://medium.com/@gurjinderkaur95?source=post_page-----c04b7452efbe--------------------------------)![Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----c04b7452efbe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c04b7452efbe--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c04b7452efbe--------------------------------) [Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----c04b7452efbe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F79ee1ef48e0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-and-gini-index-c04b7452efbe&user=Gurjinder+Kaur&userId=79ee1ef48e0c&source=post_page-79ee1ef48e0c----c04b7452efbe---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c04b7452efbe--------------------------------) ·7 分钟阅读·2023 年 11 月 5 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc04b7452efbe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-and-gini-index-c04b7452efbe&user=Gurjinder+Kaur&userId=79ee1ef48e0c&source=-----c04b7452efbe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc04b7452efbe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-and-gini-index-c04b7452efbe&source=-----c04b7452efbe---------------------bookmark_footer-----------)![](img/7471e1f27896be8a7c34dd14a251bf09.png)

你能告诉我哪些是最纯净和最不纯净的类别吗？（来源：作者提供的图片）

**熵**和**基尼指数**是重要的机器学习概念，特别是在决策树算法中有助于确定划分的质量。这两个指标的计算方式不同，但最终用于量化数据集中的相同事物，即不确定性（或不纯度）。

> 熵（或基尼指数）越高，数据的随机性（混合程度）就越大。

让我们直观地了解数据集中的不纯度，并理解这些指标如何帮助衡量它。（不纯度、不确定性、随机性、异质性——在我们的上下文中可以互换使用，最终目标是减少这些因素以获得更好的清晰度。）

## **什么是杂质——通过一个例子来解释**

想象一下你和你的朋友——*爱丽丝*和*鲍勃*一起去超市买水果。你们每个人都拿了一个购物车，因为你们都不喜欢分享水果。让我们看看你们买了什么（看来你们真的很喜欢苹果!!）：
