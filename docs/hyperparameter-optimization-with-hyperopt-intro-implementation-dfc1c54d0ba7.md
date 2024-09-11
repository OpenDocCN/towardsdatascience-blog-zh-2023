# 使用 Hyperopt 进行超参数优化 — 介绍与实现

> 原文：[https://towardsdatascience.com/hyperparameter-optimization-with-hyperopt-intro-implementation-dfc1c54d0ba7?source=collection_archive---------2-----------------------#2023-06-05](https://towardsdatascience.com/hyperparameter-optimization-with-hyperopt-intro-implementation-dfc1c54d0ba7?source=collection_archive---------2-----------------------#2023-06-05)

## 通过超参数优化提升机器学习模型的性能。

[](https://medium.com/@fmnobar?source=post_page-----dfc1c54d0ba7--------------------------------)[![Farzad Mahmoodinobar](../Images/2d75209693b712300e6f0796bd2487d0.png)](https://medium.com/@fmnobar?source=post_page-----dfc1c54d0ba7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dfc1c54d0ba7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dfc1c54d0ba7--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----dfc1c54d0ba7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperparameter-optimization-with-hyperopt-intro-implementation-dfc1c54d0ba7&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----dfc1c54d0ba7---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dfc1c54d0ba7--------------------------------) ·11分钟阅读·2023年6月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdfc1c54d0ba7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperparameter-optimization-with-hyperopt-intro-implementation-dfc1c54d0ba7&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----dfc1c54d0ba7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdfc1c54d0ba7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhyperparameter-optimization-with-hyperopt-intro-implementation-dfc1c54d0ba7&source=-----dfc1c54d0ba7---------------------bookmark_footer-----------)![](../Images/ba34fe1074371d14f74e66344336a40b.png)

图片由 [Te NGuyen](https://unsplash.com/@tenguyen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/Wt7XT1R6sjU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

[Hyperopt](https://github.com/hyperopt/hyperopt) 是一个开源的超参数优化工具，我个人用来改进我的机器学习项目，发现它很容易实现。超参数优化是确定最佳超参数组合的过程，以使机器学习模型满足目标函数（这通常定义为“最小化”目标函数以保持一致性）。换个比喻，每个机器学习模型都有各种可以调节的旋钮和杠杆，直到我们从模型中得到期望的结果。找到能实现我们期望结果的超参数组合的过程被称为超参数优化。一些这样的参数例子包括：学习率、神经网络的架构（如隐藏层的数量）、优化器的选择等。

如果你有兴趣探索其他超参数优化策略，如网格搜索、随机搜索和贝叶斯优化，可以查看下面的帖子：
