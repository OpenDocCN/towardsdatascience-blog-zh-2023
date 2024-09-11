# 深入了解 Softmax 回归

> 原文：[https://towardsdatascience.com/deep-dive-into-softmax-regression-62deea103cb8?source=collection_archive---------1-----------------------#2023-05-25](https://towardsdatascience.com/deep-dive-into-softmax-regression-62deea103cb8?source=collection_archive---------1-----------------------#2023-05-25)

## 理解 softmax 回归背后的数学原理，以及如何使用它来解决图像分类任务

[](https://medium.com/@roiyeho?source=post_page-----62deea103cb8--------------------------------)[![Roi Yehoshua 博士](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----62deea103cb8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----62deea103cb8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----62deea103cb8--------------------------------) [Roi Yehoshua 博士](https://medium.com/@roiyeho?source=post_page-----62deea103cb8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-softmax-regression-62deea103cb8&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----62deea103cb8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----62deea103cb8--------------------------------) ·13 分钟阅读·2023年5月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F62deea103cb8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-softmax-regression-62deea103cb8&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----62deea103cb8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F62deea103cb8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-softmax-regression-62deea103cb8&source=-----62deea103cb8---------------------bookmark_footer-----------)

Softmax 回归（或**多项式逻辑回归**）是逻辑回归在多类问题上的推广。

它可以用来预测某事件不同可能结果的概率，例如根据患者的特征（性别、年龄、血压、各种测试结果等）预测某人是否患有特定疾病。

在本文中，我们将从基本原理出发推导 softmax 回归模型，并展示如何使用它来解决图像分类任务。

在阅读本文之前，我强烈建议你阅读我之前关于逻辑回归的文章：

[](/mastering-logistic-regression-3e502686f0ae?source=post_page-----62deea103cb8--------------------------------) [## 精通逻辑回归

### 从理论到 Python 实现

towardsdatascience.com](/mastering-logistic-regression-3e502686f0ae?source=post_page-----62deea103cb8--------------------------------)

# 背景：多类别分类问题

回忆一下，在[监督学习](https://medium.com/@roiyeho/introduction-to-supervised-machine-learning-313730eb5aa2)问题中，我们会得到一个包含 *n* 个标记样本的训练集：*D* = {(**x**₁, *y*₁), (**x**₂, *y*₂), … , (**x***ₙ, yₙ*)}，其中 **x***ᵢ* 是一个 *m* 维向量，包含样本 *i* 的 **特征**，*yᵢ* 表示该样本的 **标签**。我们的目标是建立一个模型……
