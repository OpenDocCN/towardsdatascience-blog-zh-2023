# L1 与 L2 正则化在机器学习中的应用：区别、优势及如何在 Python 中应用

> 原文：[`towardsdatascience.com/l1-vs-l2-regularization-in-machine-learning-differences-advantages-and-how-to-apply-them-in-72eb12f102b5?source=collection_archive---------4-----------------------#2023-02-23`](https://towardsdatascience.com/l1-vs-l2-regularization-in-machine-learning-differences-advantages-and-how-to-apply-them-in-72eb12f102b5?source=collection_archive---------4-----------------------#2023-02-23)

## *深入探讨机器学习中的 L1 和 L2 正则化技术，解释它们在防止模型过拟合中的重要性*

[](https://medium.com/@theDrewDag?source=post_page-----72eb12f102b5--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----72eb12f102b5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72eb12f102b5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----72eb12f102b5--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----72eb12f102b5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fl1-vs-l2-regularization-in-machine-learning-differences-advantages-and-how-to-apply-them-in-72eb12f102b5&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----72eb12f102b5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72eb12f102b5--------------------------------) ·8 分钟阅读·2023 年 2 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72eb12f102b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fl1-vs-l2-regularization-in-machine-learning-differences-advantages-and-how-to-apply-them-in-72eb12f102b5&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----72eb12f102b5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72eb12f102b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fl1-vs-l2-regularization-in-machine-learning-differences-advantages-and-how-to-apply-them-in-72eb12f102b5&source=-----72eb12f102b5---------------------bookmark_footer-----------)![](img/423a619cb1e90d37b508328d6453e1d2.png)

图片由作者提供。

机器学习是一个在技术和工业领域都经历巨大发展的学科。

多亏了其算法和建模技术，我们可以构建能够从过去的数据中学习、进行泛化并对新数据进行预测的模型。

然而，在某些情况下，**模型可能会对训练数据进行过拟合，从而失去泛化能力**。这种现象称为*过拟合*。

对于分析师来说，理解什么是过拟合以及为什么它代表了机器学习中创建预测模型时的主要障碍之一是非常重要的。

过拟合的一个基本概念是这样的：

> 当一个模型过于复杂或对训练数据的拟合过好时，它可能对这些特定的数据非常准确，但对之前未见过的数据泛化能力很差。**这意味着当模型应用于现实生活中的新数据时将无效**。
