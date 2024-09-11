# 实现机器学习的真实世界数据收集指南

> 原文：[https://towardsdatascience.com/a-guide-to-real-world-data-collection-for-machine-learning-a232c436ac19?source=collection_archive---------8-----------------------#2023-09-05](https://towardsdatascience.com/a-guide-to-real-world-data-collection-for-machine-learning-a232c436ac19?source=collection_archive---------8-----------------------#2023-09-05)

## 5 Actionable Strategies to Optimize Your Data Collection Process

[](https://medium.com/@DataScienceRebalanced?source=post_page-----a232c436ac19--------------------------------)[![Leah Berg and Ray McLendon](../Images/549a5697ea857af5a0f9f7905e2819b1.png)](https://medium.com/@DataScienceRebalanced?source=post_page-----a232c436ac19--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a232c436ac19--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a232c436ac19--------------------------------) [Leah Berg and Ray McLendon](https://medium.com/@DataScienceRebalanced?source=post_page-----a232c436ac19--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F52338acfb4b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-real-world-data-collection-for-machine-learning-a232c436ac19&user=Leah+Berg+and+Ray+McLendon&userId=52338acfb4b9&source=post_page-52338acfb4b9----a232c436ac19---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a232c436ac19--------------------------------) ·8 分钟阅读·Sep 5, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa232c436ac19&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-real-world-data-collection-for-machine-learning-a232c436ac19&user=Leah+Berg+and+Ray+McLendon&userId=52338acfb4b9&source=-----a232c436ac19---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa232c436ac19&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-guide-to-real-world-data-collection-for-machine-learning-a232c436ac19&source=-----a232c436ac19---------------------bookmark_footer-----------)![](../Images/c8190cbcd11452de2e504422873c957d.png)

Photo by [Henrik Dønnestad](https://unsplash.com/@spaceboy?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

无论你是刚刚入门数据科学的新手，还是大型组织的首席数据科学家，你可能都曾使用过精心制作的数据集来解决玩具机器学习问题。也许你使用了K-Means聚类来预测[Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set)数据集中的花卉品种。或者你尝试了逻辑回归模型来预测哪些乘客在[Titanic](https://www.kaggle.com/competitions/titanic)航行中幸存。

虽然这些数据集非常适合练习机器学习的基础，但它们并不能反映你在实际工作中遇到的真实世界数据。实际上，你的数据可能存在质量问题，可能不适合当前的任务，或者可能尚不存在。这意味着数据科学家经常需要卷起袖子收集数据——这是现代数据科学课程中通常没有涉及的挑战。

对于新的数据科学家来说，在深入解决问题之前收集大量数据可能会感到非常令人畏惧，因为这个阶段为整个机器学习项目奠定了基础。然而，凭借正确的策略，这个过程可以变得更加可控。

在我超过10年的数据科学家经历中，我遇到了各种各样的数据收集方式……
