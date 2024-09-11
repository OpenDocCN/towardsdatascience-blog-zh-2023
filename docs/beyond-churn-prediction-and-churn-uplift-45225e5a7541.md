# 超越流失预测和流失提升

> 原文：[https://towardsdatascience.com/beyond-churn-prediction-and-churn-uplift-45225e5a7541?source=collection_archive---------4-----------------------#2023-07-25](https://towardsdatascience.com/beyond-churn-prediction-and-churn-uplift-45225e5a7541?source=collection_archive---------4-----------------------#2023-07-25)

## [因果数据科学](https://towardsdatascience.com/tagged/causal-data-science)

## 如何在流失的情况下最佳地制定政策

[](https://medium.com/@matteo.courthoud?source=post_page-----45225e5a7541--------------------------------)[![Matteo Courthoud](../Images/d873eab35a0cf9fc696658c0bee16b33.png)](https://medium.com/@matteo.courthoud?source=post_page-----45225e5a7541--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45225e5a7541--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----45225e5a7541--------------------------------) [Matteo Courthoud](https://medium.com/@matteo.courthoud?source=post_page-----45225e5a7541--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F666130fb420f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-churn-prediction-and-churn-uplift-45225e5a7541&user=Matteo+Courthoud&userId=666130fb420f&source=post_page-666130fb420f----45225e5a7541---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----45225e5a7541--------------------------------) ·12分钟阅读·2023年7月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F45225e5a7541&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-churn-prediction-and-churn-uplift-45225e5a7541&user=Matteo+Courthoud&userId=666130fb420f&source=-----45225e5a7541---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F45225e5a7541&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-churn-prediction-and-churn-uplift-45225e5a7541&source=-----45225e5a7541---------------------bookmark_footer-----------)![](../Images/af36704bf3fd645df857033ca3b5e3a6.png)

封面，图片由作者提供

数据科学中一个非常常见的任务是*流失预测*。然而，预测流失通常只是一个中间步骤，很少是最终目标。通常，我们真正关心的是**减少流失**，这是一个独立的目标，不一定相关。实际上，例如，知道长期客户的流失概率低于新客户，这并不是一个可操作的见解，因为我们无法延长客户的使用时间。我们真正想了解的是一个（或多个）处理方法如何影响流失。这通常被称为**流失提升**。

在本文中，我们将**超越**流失预测和流失提升，转而考虑流失预防活动的最终目标：**增加收入**。首先，减少流失的政策可能也会对收入产生影响，这一点需要考虑。然而，更重要的是，增加收入只有在客户不流失的情况下才是相关的。反之，减少流失对高收入客户更为重要。这种流失与收入之间的**互动**在理解任何处理活动的盈利能力时至关重要，不应被忽视。

# 礼品与订阅
