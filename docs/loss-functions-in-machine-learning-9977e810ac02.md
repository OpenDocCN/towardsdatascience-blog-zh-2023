# 机器学习中的损失函数

> 原文：[`towardsdatascience.com/loss-functions-in-machine-learning-9977e810ac02?source=collection_archive---------5-----------------------#2023-05-12`](https://towardsdatascience.com/loss-functions-in-machine-learning-9977e810ac02?source=collection_archive---------5-----------------------#2023-05-12)

## 理解最常见的损失函数以及何时使用每种

[](https://medium.com/@roiyeho?source=post_page-----9977e810ac02--------------------------------)![Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----9977e810ac02--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9977e810ac02--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9977e810ac02--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----9977e810ac02--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Floss-functions-in-machine-learning-9977e810ac02&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----9977e810ac02---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9977e810ac02--------------------------------) ·11 min 读·May 12, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9977e810ac02&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Floss-functions-in-machine-learning-9977e810ac02&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----9977e810ac02---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9977e810ac02&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Floss-functions-in-machine-learning-9977e810ac02&source=-----9977e810ac02---------------------bookmark_footer-----------)![](img/075f0e8f6ffba336b07154d146ab466c.png)

[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/XWar9MbNGUY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

损失函数在机器学习中具有重要作用，它们指导模型的学习过程并定义其目标。

有大量的损失函数可供选择，选择合适的函数对于训练准确模型至关重要。不同的损失函数选择可能导致不同的分类或回归模型。

在本文中，我们将讨论最常用的损失函数，它们的操作方式，优缺点以及何时使用每种。

# 什么是损失函数？

回顾一下，在[监督机器学习](https://medium.com/@roiyeho/introduction-to-supervised-machine-learning-313730eb5aa2)问题中，我们给定一个包含 *n* 个标记样本的训练集：*D* = {(**x**₁, *y*₁), (**x**₂, *y*₂), … , (**x**ₙ, *y*ₙ*)}，其中 **x**ᵢ 表示样本 *i* 的**特征**，*yᵢ* 表示该样本的**标签**。我们的目标是构建一个模型，使其预测尽可能接近真实标签。

**损失函数**衡量模型对给定样本的预测误差，即模型预测值与该样本真实值之间的差异。它有两个参数：样本的真实标签 *y* 和模型的预测值 *ŷ*：
