# 使用交叉验证和 Matplotlib 在 Python 中对机器学习模型进行基准测试

> 原文：[https://towardsdatascience.com/benchmarking-machine-learning-models-with-cross-validation-and-matplotlib-in-python-4957a41149e?source=collection_archive---------18-----------------------#2023-01-23](https://towardsdatascience.com/benchmarking-machine-learning-models-with-cross-validation-and-matplotlib-in-python-4957a41149e?source=collection_archive---------18-----------------------#2023-01-23)

## *了解如何通过使用交叉验证和结果可视化来创建面向对象的方法，以比较和评估机器学习模型的性能*

[](https://medium.com/@theDrewDag?source=post_page-----4957a41149e--------------------------------)[![Andrea D'Agostino](../Images/58c7c218815f25278aae59cea44d8771.png)](https://medium.com/@theDrewDag?source=post_page-----4957a41149e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4957a41149e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4957a41149e--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----4957a41149e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbenchmarking-machine-learning-models-with-cross-validation-and-matplotlib-in-python-4957a41149e&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----4957a41149e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4957a41149e--------------------------------) ·5 分钟阅读·2023 年 1 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4957a41149e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbenchmarking-machine-learning-models-with-cross-validation-and-matplotlib-in-python-4957a41149e&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----4957a41149e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4957a41149e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbenchmarking-machine-learning-models-with-cross-validation-and-matplotlib-in-python-4957a41149e&source=-----4957a41149e---------------------bookmark_footer-----------)![](../Images/943df594f470acf297324fd669536fb8.png)

作者提供的图片。

在本文中，我们将探讨如何使用 Python 比较和评估机器学习模型的性能。

我们将使用 Sklearn 进行模型测试，并使用 Matplotlib 显示结果。

这样做的主要动机是**对模型性能有清晰而准确的理解，从而改进模型选择过程**。

交叉验证是一种在训练数据以外的数据上测试模型的稳健方法。它允许我们在*折叠*数据上评估模型性能，这些数据没有被用于训练模型，从而给我们提供了对模型在真实数据上表现的更准确估计。

有关交叉验证的详细解释，请查看这篇文章
