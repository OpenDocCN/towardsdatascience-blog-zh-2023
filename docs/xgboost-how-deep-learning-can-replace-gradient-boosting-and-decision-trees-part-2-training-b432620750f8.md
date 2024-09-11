# XGBoost：深度学习如何取代梯度提升和决策树——第二部分：训练

> 原文：[`towardsdatascience.com/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-part-2-training-b432620750f8?source=collection_archive---------5-----------------------#2023-09-21`](https://towardsdatascience.com/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-part-2-training-b432620750f8?source=collection_archive---------5-----------------------#2023-09-21)

[](https://medium.com/@guillaume.saupin?source=post_page-----b432620750f8--------------------------------)![Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----b432620750f8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b432620750f8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b432620750f8--------------------------------) [Saupin Guillaume](https://medium.com/@guillaume.saupin?source=post_page-----b432620750f8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F891e27328f3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-part-2-training-b432620750f8&user=Saupin+Guillaume&userId=891e27328f3a&source=post_page-891e27328f3a----b432620750f8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b432620750f8--------------------------------) ·6 分钟阅读·2023 年 9 月 21 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb432620750f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-part-2-training-b432620750f8&user=Saupin+Guillaume&userId=891e27328f3a&source=-----b432620750f8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb432620750f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-part-2-training-b432620750f8&source=-----b432620750f8---------------------bookmark_footer-----------)![](img/528a94d279856509bbfad323f8112359.png)

图片由 [Simon Wilkes](https://unsplash.com/@simonfromengland?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 一个没有*如果*的世界

在之前的文章中：

[](/xgboost-how-deep-learning-can-replace-gradient-boosting-and-decision-trees-291dc9365656?source=post_page-----b432620750f8--------------------------------) ## XGBoost：深度学习如何替代梯度提升和决策树——第一部分

### 在这篇文章中，你将学习如何使用可微编程方法重写决策树，如提议的…

towardsdatascience.com

你已经学习了如何使用可微编程方法重写决策树，如 NODE 论文所建议的。这篇论文的核心思想是用神经网络替代 XGBoost。

更具体地说，在解释了为什么构建决策树的过程不可微分之后，它引入了正则化与决策节点相关的两个主要元素所需的数学工具：

+   特征选择

+   分支检测

NODE 论文显示，这两者都可以使用 entmax 函数处理。

总结来说，我们展示了如何**不使用比较操作符**来创建二叉树。

前一篇文章以关于训练正则化决策树的开放问题结束。现在是时候回答这些问题了。
