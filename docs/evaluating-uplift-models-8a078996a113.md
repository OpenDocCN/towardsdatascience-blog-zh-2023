# 评估提升模型

> 原文：[https://towardsdatascience.com/evaluating-uplift-models-8a078996a113?source=collection_archive---------2-----------------------#2023-07-13](https://towardsdatascience.com/evaluating-uplift-models-8a078996a113?source=collection_archive---------2-----------------------#2023-07-13)

## [因果数据科学](https://towardsdatascience.com/tagged/causal-data-science)

## 如何比较和选择最佳的提升模型

[](https://medium.com/@matteo.courthoud?source=post_page-----8a078996a113--------------------------------)[![Matteo Courthoud](../Images/d873eab35a0cf9fc696658c0bee16b33.png)](https://medium.com/@matteo.courthoud?source=post_page-----8a078996a113--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8a078996a113--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8a078996a113--------------------------------) [Matteo Courthoud](https://medium.com/@matteo.courthoud?source=post_page-----8a078996a113--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F666130fb420f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluating-uplift-models-8a078996a113&user=Matteo+Courthoud&userId=666130fb420f&source=post_page-666130fb420f----8a078996a113---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8a078996a113--------------------------------) ·18 分钟阅读·2023年7月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8a078996a113&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluating-uplift-models-8a078996a113&user=Matteo+Courthoud&userId=666130fb420f&source=-----8a078996a113---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8a078996a113&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluating-uplift-models-8a078996a113&source=-----8a078996a113---------------------bookmark_footer-----------)![](../Images/8182be522fb1050cb15fce92296a5526.png)

封面，图片由作者提供。

行业中因果推断的最广泛应用之一是**提升建模**，即条件平均处理效应的估计。

在估计**治疗**（如药物、广告、产品等）对**结果**（如疾病、公司收入、客户满意度等）的因果效应时，我们不仅关心治疗是否有效于整体，还希望了解哪些**对象**（患者、用户、客户等）上效果更好或更差。

估计异质增量效应或提升是改善**目标**政策的一个重要中间步骤。例如，我们可能希望提醒某些人他们更可能经历药物副作用，或仅向特定客户展示广告。

虽然存在许多建模提升的方法，但在特定应用中并不总是清楚应该使用哪一种。至关重要的是，由于**因果推断的基本问题**，我们关注的目标，即提升，无法被观察到，因此我们不能像使用机器学习预测算法那样验证我们的估计器。我们无法留出验证…
