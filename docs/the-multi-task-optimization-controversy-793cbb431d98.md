# 多任务优化争议

> 原文：[https://towardsdatascience.com/the-multi-task-optimization-controversy-793cbb431d98?source=collection_archive---------12-----------------------#2023-09-29](https://towardsdatascience.com/the-multi-task-optimization-controversy-793cbb431d98?source=collection_archive---------12-----------------------#2023-09-29)

## 我们是否需要特别的算法来同时训练多个任务的模型？

[](https://medium.com/@samuel.flender?source=post_page-----793cbb431d98--------------------------------)[![Samuel Flender](../Images/390d82a673de8a8bb11cef66978269b5.png)](https://medium.com/@samuel.flender?source=post_page-----793cbb431d98--------------------------------)[](https://towardsdatascience.com/?source=post_page-----793cbb431d98--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----793cbb431d98--------------------------------) [Samuel Flender](https://medium.com/@samuel.flender?source=post_page-----793cbb431d98--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce56d9dcd568&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-multi-task-optimization-controversy-793cbb431d98&user=Samuel+Flender&userId=ce56d9dcd568&source=post_page-ce56d9dcd568----793cbb431d98---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----793cbb431d98--------------------------------) · 6分钟阅读 · 2023年9月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F793cbb431d98&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-multi-task-optimization-controversy-793cbb431d98&user=Samuel+Flender&userId=ce56d9dcd568&source=-----793cbb431d98---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F793cbb431d98&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-multi-task-optimization-controversy-793cbb431d98&source=-----793cbb431d98---------------------bookmark_footer-----------)![](../Images/d49f4ff793f5ebba7bb708389b741416.png)

图片由[Javier Allegue Barros](https://unsplash.com/@soymeraki?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[多任务学习范式](/multi-task-learning-in-recommender-systems-a-primer-508e661a2029#:~:text=Multi%2Dtask%20learning%20matters%20because,each%20other%20%E2%80%94%20creating%20negative%20transfer.)——即同时训练多个任务的能力——既是福音也是诅咒。

这是一个福音，因为它允许我们构建一个单一模型，而之前我们需要多个模型。这使得生活变得更简单：需要维护、重新训练、调整和监控的模型更少。

这是一个诅咒，因为它打开了一整套全新的问题：哪些任务应该一起学习？我们真正需要哪些任务？如果任务之间相互竞争会发生什么？我们如何让模型优先处理某些任务而非其他任务？我们如何避免“任务衰退”，即任务头的积累最终导致模型性能下降？

正是这些问题催生了一个新的机器学习子领域，称为*多任务优化*，即如何在多个有时相互竞争的任务上优化模型的科学。

# 标量化

标量化是数学对多任务优化问题的回答。在一个多任务模型中，我们试图学习K个任务，例如预测…
