# XGBoost：深度学习如何替代梯度提升和决策树 —— 第一部分

> 原文：[`towardsdatascience.com/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-291dc9365656?source=collection_archive---------5-----------------------#2023-05-29`](https://towardsdatascience.com/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-291dc9365656?source=collection_archive---------5-----------------------#2023-05-29)

[](https://medium.com/@guillaume.saupin?source=post_page-----291dc9365656--------------------------------)![Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----291dc9365656--------------------------------)[](https://towardsdatascience.com/?source=post_page-----291dc9365656--------------------------------)![向数据科学](https://towardsdatascience.com/?source=post_page-----291dc9365656--------------------------------) [Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----291dc9365656--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F891e27328f3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-291dc9365656&user=Saupin+Guillaume&userId=891e27328f3a&source=post_page-891e27328f3a----291dc9365656---------------------post_header-----------) 发表在[向数据科学](https://towardsdatascience.com/?source=post_page-----291dc9365656--------------------------------) ·7 分钟阅读·2023 年 5 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F291dc9365656&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-291dc9365656&user=Saupin+Guillaume&userId=891e27328f3a&source=-----291dc9365656---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F291dc9365656&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-291dc9365656&source=-----291dc9365656---------------------bookmark_footer-----------)![](img/d836a60a327155b897054d6830cd01dc.png)

照片由[Preethi Viswanathan](https://unsplash.com/@sallybrad2016?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上提供

在这篇文章中，你将了解使用可微分程序设计方法重写决策树，这是由 NODE 论文提出的，从而使得该模型以类似神经网络的方式重新表述。

推导这个公式是一个很好的练习，因为它引发了在构建和训练自定义神经网络时经常遇到的许多问题：

+   如何避免梯度消失问题？

+   初始权重的好选择是什么？

+   为什么使用批量归一化？

但在回答这些问题之前，让我们看看如何将决策树在神经网络的数学框架中重新表述：可微编程。

更新 — 第二篇文章已上线：

[](/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-part-2-training-b432620750f8?source=post_page-----291dc9365656--------------------------------) ## XGBoost：深度学习如何替代梯度提升和决策树 — 第二部分：训练

### 一个没有“如果”的世界

towardsdatascience.com

# XGBoost 非常高效，但…

如果你阅读过我之前关于梯度提升和决策树的文章，你就会知道梯度提升结合决策树集成…
