# 分类器集成：投票分类器

> 原文：[https://towardsdatascience.com/ensemble-of-classifiers-voting-classifier-ef7f6a5b7795?source=collection_archive---------7-----------------------#2023-08-17](https://towardsdatascience.com/ensemble-of-classifiers-voting-classifier-ef7f6a5b7795?source=collection_archive---------7-----------------------#2023-08-17)

## 结合多种不同的模型以获得更好的预测结果

[](https://saptashwa.medium.com/?source=post_page-----ef7f6a5b7795--------------------------------)[![Saptashwa Bhattacharyya](../Images/b01238113a1f6b91cb6fb0fbfa50303a.png)](https://saptashwa.medium.com/?source=post_page-----ef7f6a5b7795--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ef7f6a5b7795--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ef7f6a5b7795--------------------------------) [Saptashwa Bhattacharyya](https://saptashwa.medium.com/?source=post_page-----ef7f6a5b7795--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9a3c3c477239&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-of-classifiers-voting-classifier-ef7f6a5b7795&user=Saptashwa+Bhattacharyya&userId=9a3c3c477239&source=post_page-9a3c3c477239----ef7f6a5b7795---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef7f6a5b7795--------------------------------) ·7 分钟阅读·2023年8月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fef7f6a5b7795&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-of-classifiers-voting-classifier-ef7f6a5b7795&user=Saptashwa+Bhattacharyya&userId=9a3c3c477239&source=-----ef7f6a5b7795---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fef7f6a5b7795&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fensemble-of-classifiers-voting-classifier-ef7f6a5b7795&source=-----ef7f6a5b7795---------------------bookmark_footer-----------)![](../Images/96aa1b7e6415940eeaa32ba8a06bad00.png)

使用投票分类器的决策边界（图片由作者提供：代码见参考资料）

在机器学习的背景下，"集成"一词指的是一个有限数量的机器学习模型的集合（可能包括人工神经网络），这些模型都是为相同任务而训练的。通常，这些模型是独立训练的，然后将它们的预测结果进行组合。

当不同模型的预测结果不一致时，有时使用集成模型进行分类比任何单个分类器更有用。在这里，我们将结合不同的分类器，创建一个集成模型，然后用该集成模型进行预测任务。本篇文章将讨论什么内容？

+   使用 Sklearn 的 VotingClassifier 来构建一个集成模型。

+   在VotingClassifier中，什么是硬投票（Hard Voting）和软投票（Soft Voting）？

+   使用VotingClassifier检查个体模型的表现。

+   最后，使用GridSearchCV + VotingClassifier来找到个体模型的最佳参数。

开始吧！

## 数据准备：

为了查看VotingClassifier的实际应用示例，我使用了[心脏病预测数据集](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction)（根据开放数据库许可提供）。这里的任务是二分类预测某位具有特定属性的患者是否可能患有心脏病……
