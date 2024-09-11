# TensorFlow 决策森林：全面介绍

> 原文：[`towardsdatascience.com/tensorflow-decision-forests-a-comprehensive-introduction-3b6056a6d6b0?source=collection_archive---------14-----------------------#2023-04-14`](https://towardsdatascience.com/tensorflow-decision-forests-a-comprehensive-introduction-3b6056a6d6b0?source=collection_archive---------14-----------------------#2023-04-14)

## 使用 TensorFlow 训练、调优、评估、解释和部署基于树的模型

[](https://medium.com/@antonsruberts?source=post_page-----3b6056a6d6b0--------------------------------)![Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----3b6056a6d6b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3b6056a6d6b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b6056a6d6b0--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----3b6056a6d6b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dee50b0374b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-decision-forests-a-comprehensive-introduction-3b6056a6d6b0&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=post_page-9dee50b0374b----3b6056a6d6b0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b6056a6d6b0--------------------------------) · 11 分钟阅读 · 2023 年 4 月 14 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3b6056a6d6b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-decision-forests-a-comprehensive-introduction-3b6056a6d6b0&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=-----3b6056a6d6b0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3b6056a6d6b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftensorflow-decision-forests-a-comprehensive-introduction-3b6056a6d6b0&source=-----3b6056a6d6b0---------------------bookmark_footer-----------)![](img/5366f2a6d39c66852d1ebbf295e77b6b.png)

图片由 [Javier Allegue Barros](https://unsplash.com/pt-br/@soymeraki?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

两年前，TensorFlow (TF) 团队开源了一个用于训练基于树的模型的库，称为 [TensorFlow Decision Forests (TFDF)](https://github-com.translate.goog/tensorflow/decision-forests?_x_tr_sl=en&_x_tr_tl=ru&_x_tr_hl=ru&_x_tr_pto=sc)。就在上个月，他们终于 [宣布](https://blog.tensorflow.org/2023/02/updates-tensorflow-decision-forests-is-production-ready.html) 这个包已经准备好投入生产，所以我决定是时候仔细了解一下了。这篇文章的目的是让你对这个包有更好的了解，并展示如何（有效地）使用它。下面你可以看到这篇文章的结构，随意跳到你最感兴趣的部分。

1.  什么是 TFDF，为什么要使用它？

1.  使用 TFDF 训练随机森林（RF）和梯度提升树（GBT）模型

1.  使用 TFDF 和 Optuna 调整超参数

1.  模型检查

1.  使用 TF Serving 部署 GBT 模型

# 设置

你可以在我的 [代码库](https://github.com/aruberts/tutorials/tree/main/tfdf/notebooks) 中找到所有的代码，所以如果你还没给它加星标，请务必加上。在这篇文章中，我们将使用 [美国小企业管理局数据集](https://www.kaggle.com/datasets/mirbektoktogaraev/should-this-loan-be-approved-or-denied)（CC BY-SA 4.0 许可）训练一些贷款违约预测模型。模型将使用已经预处理过的数据进行训练，但你可以…
