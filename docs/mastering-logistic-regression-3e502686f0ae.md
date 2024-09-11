# 掌握逻辑回归

> 原文：[https://towardsdatascience.com/mastering-logistic-regression-3e502686f0ae?source=collection_archive---------1-----------------------#2023-05-20](https://towardsdatascience.com/mastering-logistic-regression-3e502686f0ae?source=collection_archive---------1-----------------------#2023-05-20)

## 从理论到 Python 实现

[](https://medium.com/@roiyeho?source=post_page-----3e502686f0ae--------------------------------)[![Dr. Roi Yehoshua](../Images/905a512ffc8879069403a87dbcbeb4db.png)](https://medium.com/@roiyeho?source=post_page-----3e502686f0ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3e502686f0ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3e502686f0ae--------------------------------) [Dr. Roi Yehoshua](https://medium.com/@roiyeho?source=post_page-----3e502686f0ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3886620c5cf9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-logistic-regression-3e502686f0ae&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=post_page-3886620c5cf9----3e502686f0ae---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e502686f0ae--------------------------------) · 17 分钟阅读 · 2023年5月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3e502686f0ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-logistic-regression-3e502686f0ae&user=Dr.+Roi+Yehoshua&userId=3886620c5cf9&source=-----3e502686f0ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3e502686f0ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-logistic-regression-3e502686f0ae&source=-----3e502686f0ae---------------------bookmark_footer-----------)![](../Images/054e0f77d6a401cd71794ce2aa09c550.png)

图片来源：[Gerd Altmann](https://pixabay.com/users/geralt-9301/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1044090) 来自 [Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1044090)

逻辑回归是最常见的机器学习算法之一。它可以用来预测事件发生的概率，例如基于给定的标记数据集来判断一封来邮是否是垃圾邮件，或判断一个肿瘤是否是恶性的。

由于其简单性，逻辑回归通常用作评估其他更复杂模型的基线。

该模型名字中有“逻辑”一词，因为它使用**逻辑函数**（sigmoid）将输入特征的线性组合转换为概率。

它的名字中也包含了“回归”一词，因为它的输出是介于0和1之间的连续值，尽管它通常被用作**二分类器**，通过选择一个阈值（通常为0.5）来分类，概率大于阈值的输入被归为正类，而低于阈值的输入被归为负类。

在这篇文章中，我们将深入探讨逻辑回归模型，从头开始在Python中实现它，然后展示在Scikit-Learn中的实现。

# 背景：二分类问题
