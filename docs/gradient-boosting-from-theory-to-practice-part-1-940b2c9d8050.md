# 梯度提升从理论到实践（第一部分）

> 原文：[`towardsdatascience.com/gradient-boosting-from-theory-to-practice-part-1-940b2c9d8050?source=collection_archive---------4-----------------------#2023-07-11`](https://towardsdatascience.com/gradient-boosting-from-theory-to-practice-part-1-940b2c9d8050?source=collection_archive---------4-----------------------#2023-07-11)

## 了解流行的梯度提升算法背后的数学原理以及如何在实践中使用它

[](https://medium.com/@roiyeho?source=post_page-----940b2c9d8050--------------------------------)![Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----940b2c9d8050--------------------------------)[](https://towardsdatascience.com/?source=post_page-----940b2c9d8050--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----940b2c9d8050--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----940b2c9d8050--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosting-from-theory-to-practice-part-1-940b2c9d8050&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----940b2c9d8050---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----940b2c9d8050--------------------------------) ·19 分钟阅读·2023 年 7 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F940b2c9d8050&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosting-from-theory-to-practice-part-1-940b2c9d8050&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----940b2c9d8050---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F940b2c9d8050&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-boosting-from-theory-to-practice-part-1-940b2c9d8050&source=-----940b2c9d8050---------------------bookmark_footer-----------)![](img/24fe276d9c34eff1399a9e28bbbe6c98.png)

照片由[Jens Lelie](https://unsplash.com/@madebyjens?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布于[Unsplash](https://unsplash.com/photos/u0vgcIOQG08?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

梯度提升是一种广泛使用的机器学习技术，基于**提升**和**梯度下降**的结合。

Boosting 是一种 [集成方法](https://medium.com/towards-artificial-intelligence/introduction-to-ensemble-methods-226a5a421687)，它将多个弱学习器（或基础学习器）组合起来以创建一个强大的预测模型。这些基础模型是按顺序训练的，每个模型都专注于纠正前一个模型的错误。

在**梯度提升**中，每个基础模型被训练以预测相对于前一个模型预测的损失函数的负梯度。因此，将新训练的基础学习器添加到集成中，可以使损失函数最小值的下降方向更陡峭。这一过程类似于梯度下降，但它在函数空间中进行，而不是参数空间中。因此，这被称为**函数梯度下降**。

当弱学习器是 [决策树](https://medium.com/@roiyeho/decision-trees-part-1-da4e613d2369) 时，所得方法被称为**梯度提升决策树**（GBDT）或**梯度提升机器**（GBM）。

梯度提升是当前处理结构化（表格）数据的最佳算法之一，且…
