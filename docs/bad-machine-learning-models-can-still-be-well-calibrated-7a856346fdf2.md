# 糟糕的机器学习模型仍然可以很好地校准

> 原文：[https://towardsdatascience.com/bad-machine-learning-models-can-still-be-well-calibrated-7a856346fdf2?source=collection_archive---------6-----------------------#2023-02-13](https://towardsdatascience.com/bad-machine-learning-models-can-still-be-well-calibrated-7a856346fdf2?source=collection_archive---------6-----------------------#2023-02-13)

## 机器学习

## 你不需要一个完美的预言者来正确地获取你的概率。

[](https://michaloleszak.medium.com/?source=post_page-----7a856346fdf2--------------------------------)[![Michał Oleszak](../Images/61b32e70cec4ba54612a8ca22e977176.png)](https://michaloleszak.medium.com/?source=post_page-----7a856346fdf2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a856346fdf2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7a856346fdf2--------------------------------) [Michał Oleszak](https://michaloleszak.medium.com/?source=post_page-----7a856346fdf2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc58320fab2a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbad-machine-learning-models-can-still-be-well-calibrated-7a856346fdf2&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=post_page-c58320fab2a8----7a856346fdf2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a856346fdf2--------------------------------) ·11分钟阅读·2023年2月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7a856346fdf2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbad-machine-learning-models-can-still-be-well-calibrated-7a856346fdf2&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=-----7a856346fdf2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7a856346fdf2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbad-machine-learning-models-can-still-be-well-calibrated-7a856346fdf2&source=-----7a856346fdf2---------------------bookmark_footer-----------)![](../Images/e7631602da782fdb4e815070633580b0.png)

机器学习模型通常根据其性能来评估，性能通过某些指标接近零或一来衡量（具体取决于指标），但这并不是唯一决定其有用性的因素。在某些情况下，一个总体上不是很准确的模型仍然可以很好地校准，并找到有用的应用。在这篇文章中，我们将探讨良好校准与良好性能之间的区别，以及何时可能更倾向于其中一个。让我们深入了解吧！

# 概率校准

在其强定义下，概率校准是指分类模型预测的概率与数据集中目标类别的实际频率匹配的程度。一个[校准良好的模型](https://medium.com/towards-data-science/calibrating-classifiers-559abc30711a)生成的预测，整体上与实际结果紧密对齐。

这在实际操作中的意思是，如果我们用一个完全校准的二分类模型进行大量预测，然后仅考虑那些模型预测为70%正类概率的预测，那么模型的准确率应该是70%。类似地，如果我们仅仅...
