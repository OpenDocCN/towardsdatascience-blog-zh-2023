# 使用MLflow实验和HyperOpt调优训练XGBoost

> 原文：[https://towardsdatascience.com/training-xgboost-with-mlflow-experiments-and-hyperopt-c0d3a4994ea6?source=collection_archive---------3-----------------------#2023-01-09](https://towardsdatascience.com/training-xgboost-with-mlflow-experiments-and-hyperopt-c0d3a4994ea6?source=collection_archive---------3-----------------------#2023-01-09)

## MLOps 旅程的起点

[](https://animadurkar.medium.com/?source=post_page-----c0d3a4994ea6--------------------------------)[![Ani Madurkar](../Images/ad54a9e110c56ba1f4c7f5ce0bc7d7e4.png)](https://animadurkar.medium.com/?source=post_page-----c0d3a4994ea6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c0d3a4994ea6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c0d3a4994ea6--------------------------------) [Ani Madurkar](https://animadurkar.medium.com/?source=post_page-----c0d3a4994ea6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc9b0adccc01d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-xgboost-with-mlflow-experiments-and-hyperopt-c0d3a4994ea6&user=Ani+Madurkar&userId=c9b0adccc01d&source=post_page-c9b0adccc01d----c0d3a4994ea6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c0d3a4994ea6--------------------------------) ·10分钟阅读·2023年1月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc0d3a4994ea6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-xgboost-with-mlflow-experiments-and-hyperopt-c0d3a4994ea6&user=Ani+Madurkar&userId=c9b0adccc01d&source=-----c0d3a4994ea6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc0d3a4994ea6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftraining-xgboost-with-mlflow-experiments-and-hyperopt-c0d3a4994ea6&source=-----c0d3a4994ea6---------------------bookmark_footer-----------)![](../Images/2a9a554a515ef56cb3a1694c3f067a09.png)

亚迪朗达克山的色彩。图像来源于作者

# 介绍

随着你在机器学习领域的不断发展，不管你喜欢与否，你会发现自己越来越靠近MLOps。构建高效、可扩展且有韧性的机器学习系统是一项挑战，也是数据科学家的真正工作（在我看来），而不仅仅是进行建模。

建模部分在大多数用例中已经基本搞定。除非你想走在技术的最前沿，否则你很可能处理的是结构化的表格数据集。模型的选择可能会根据数据集的大小、假设和技术限制有所不同，但大体上是相当可重复的。在实验阶段，我的监督学习机器学习工作流已经收敛到使用`XGBoost`配合`HyperOpt`和`MLflow`。`XGBoost`用于选择模型，`HyperOpt`用于超参数调优，而`MLflow`则用于实验和跟踪。

这也是你踏上MLOps旅程的一个极佳的第一步，因为我认为在实验阶段（模型跟踪、版本控制、注册等）开始做更多的MLOps工作是最容易的。这种方法轻量且高度可配置，便于进行扩展和缩减……
