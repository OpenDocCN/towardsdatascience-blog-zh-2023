# Excel 中的梯度提升线性回归

> 原文：[`towardsdatascience.com/gradient-boosted-linear-regression-in-excel-a08522f13d6a?source=collection_archive---------2-----------------------#2023-03-17`](https://towardsdatascience.com/gradient-boosted-linear-regression-in-excel-a08522f13d6a?source=collection_archive---------2-----------------------#2023-03-17)

## 为了更好地理解梯度提升

[](https://medium.com/@angela.shi?source=post_page-----a08522f13d6a--------------------------------)![Angela and Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----a08522f13d6a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a08522f13d6a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a08522f13d6a--------------------------------) [Angela and Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----a08522f13d6a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bf03e38122e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosted-linear-regression-in-excel-a08522f13d6a&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=post_page-2bf03e38122e----a08522f13d6a---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a08522f13d6a--------------------------------) ·7 分钟阅读·2023 年 3 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa08522f13d6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosted-linear-regression-in-excel-a08522f13d6a&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=-----a08522f13d6a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa08522f13d6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosted-linear-regression-in-excel-a08522f13d6a&source=-----a08522f13d6a---------------------bookmark_footer-----------)

梯度提升是一种通常应用于决策树的集成方法。如此频繁，以至于我们通常将**梯度提升**简称为**梯度提升决策树**。例如，在 scikit learn 中，估计器 GradientBoostingRegressor 或 GradientBoostingClassifier 使用决策树。然而，作为一种集成方法，它也可以应用于其他基础模型，例如**线性回归**。但有一个显而易见的结论，你可能已经知道：

> 梯度提升线性回归就是线性回归。

但是实现它仍然很有趣，此外，我们将使用 Excel 来实现，因此即使你不熟悉编程复杂算法，你仍然可以理解算法步骤。

# 三步机器学习

我写了[一篇文章来始终区分机器学习的三步骤以有效学习](https://medium.com/towards-data-science/machine-learning-in-three-steps-how-to-efficiently-learn-it-aefcf423a9e1)，让我们将这一原则应用于梯度提升线性回归，这里是这三步：

+   **1. 模型：线性回归** 是一种机器学习模型，因为它接受输入（特征）以预测输出。

+   **1bis. 集成方法：梯度提升** 是一种集成方法，它本身不是一个模型（即它不接受输入以预测目标变量的输出）。它必须是……
