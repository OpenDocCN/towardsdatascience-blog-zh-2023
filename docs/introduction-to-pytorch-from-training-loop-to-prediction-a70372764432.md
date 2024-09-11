# PyTorch 介绍：从训练循环到预测

> 原文：[`towardsdatascience.com/introduction-to-pytorch-from-training-loop-to-prediction-a70372764432?source=collection_archive---------5-----------------------#2023-03-28`](https://towardsdatascience.com/introduction-to-pytorch-from-training-loop-to-prediction-a70372764432?source=collection_archive---------5-----------------------#2023-03-28)

## *PyTorch 的训练循环和应对该库更陡峭的初始学习曲线的一般方法介绍*

[](https://medium.com/@theDrewDag?source=post_page-----a70372764432--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----a70372764432--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a70372764432--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a70372764432--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----a70372764432--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pytorch-from-training-loop-to-prediction-a70372764432&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----a70372764432---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a70372764432--------------------------------) ·14 min read·2023 年 3 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa70372764432&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pytorch-from-training-loop-to-prediction-a70372764432&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----a70372764432---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa70372764432&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pytorch-from-training-loop-to-prediction-a70372764432&source=-----a70372764432---------------------bookmark_footer-----------)![](img/39bd895d764577c1f195119868a023ec.png)

图片由作者提供。

在这篇文章中，我们将讨论如何使用 PyTorch 在 Python 中实现逻辑回归模型。

**PyTorch 是全球数据科学家和机器学习工程师最著名和使用最广泛的深度学习框架之一**，因此，如果你想在应用人工智能领域建立职业生涯，学习这个工具成为你学习路径中的一个关键步骤。

它与 TensorFlow 一同被广泛使用，后者是由 Google 开发的另一个非常著名的深度学习框架。

除了它们的 API 结构和组织可能非常不同之外，没有显著的基本差异。

虽然这两个框架都允许我们创建非常复杂的神经网络，但由于 PyTorch 的风格更为*pythonic*，并且允许开发者将自定义逻辑集成到软件中，因此通常更受欢迎。

我们将使用**Sklearn 乳腺癌数据集**，这是一个开源数据集，之前在我的一些文章中已经使用过，用于训练二分类模型。
