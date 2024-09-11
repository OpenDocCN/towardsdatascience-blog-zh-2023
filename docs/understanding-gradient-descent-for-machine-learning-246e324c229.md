# 理解机器学习中的梯度下降

> 原文：[`towardsdatascience.com/understanding-gradient-descent-for-machine-learning-246e324c229?source=collection_archive---------1-----------------------#2023-05-21`](https://towardsdatascience.com/understanding-gradient-descent-for-machine-learning-246e324c229?source=collection_archive---------1-----------------------#2023-05-21)

## 使用 Python 深入探讨批量梯度下降、随机梯度下降和小批量梯度下降算法

[](https://idilismiguzel.medium.com/?source=post_page-----246e324c229--------------------------------)![Idil Ismiguzel](https://idilismiguzel.medium.com/?source=post_page-----246e324c229--------------------------------)[](https://towardsdatascience.com/?source=post_page-----246e324c229--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----246e324c229--------------------------------) [Idil Ismiguzel](https://idilismiguzel.medium.com/?source=post_page-----246e324c229--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d965c736f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-gradient-descent-for-machine-learning-246e324c229&user=Idil+Ismiguzel&userId=6d965c736f2&source=post_page-6d965c736f2----246e324c229---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----246e324c229--------------------------------) ·14 分钟阅读·2023 年 5 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F246e324c229&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-gradient-descent-for-machine-learning-246e324c229&user=Idil+Ismiguzel&userId=6d965c736f2&source=-----246e324c229---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F246e324c229&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-gradient-descent-for-machine-learning-246e324c229&source=-----246e324c229---------------------bookmark_footer-----------)![](img/5287b14c08ccf02afd428f2beb4f7204.png)

图片来源：[Lucas Clara](https://unsplash.com/ko/@lux17?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

梯度下降是一种流行的优化算法，广泛用于机器学习和深度学习模型中，如线性回归、逻辑回归和神经网络。它通过迭代地使用一阶导数来最小化成本函数，方法是更新模型系数（用于回归）和权重（用于神经网络）。

在本文中，我们将深入探讨梯度下降的数学理论，并探索如何使用 Python 进行计算。我们将审查包括批量梯度下降、随机梯度下降和小批量梯度下降在内的各种实现，并评估它们在不同测试案例中的效果。

在阅读本文时，你可以查看我在 GitHub 上的[Jupyter Notebook](https://github.com/Idilismiguzel/Machine-Learning/blob/master/Gradient_Descent/gradient_descent_implementation.ipynb)以获取完整的分析和代码。

在深入探讨梯度下降之前，我们首先了解一下损失函数。

# 什么是损失函数？

**损失**或**成本**这两个词可以互换使用来描述预测中的误差。损失值表示预测与实际值之间的差异，损失函数汇总所有损失……
