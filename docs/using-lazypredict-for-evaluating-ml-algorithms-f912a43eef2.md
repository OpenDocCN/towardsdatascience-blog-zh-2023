# 使用 LazyPredict 进行 ML 算法评估

> 原文：[https://towardsdatascience.com/using-lazypredict-for-evaluating-ml-algorithms-f912a43eef2?source=collection_archive---------16-----------------------#2023-03-27](https://towardsdatascience.com/using-lazypredict-for-evaluating-ml-algorithms-f912a43eef2?source=collection_archive---------16-----------------------#2023-03-27)

## 使用 LazyPredict 库自动选择最佳机器学习算法

[](https://weimenglee.medium.com/?source=post_page-----f912a43eef2--------------------------------)[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----f912a43eef2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f912a43eef2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f912a43eef2--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----f912a43eef2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-lazypredict-for-evaluating-ml-algorithms-f912a43eef2&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----f912a43eef2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f912a43eef2--------------------------------) ·10分钟阅读·2023年3月27日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff912a43eef2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fusing-lazypredict-for-evaluating-ml-algorithms-f912a43eef2&source=-----f912a43eef2---------------------bookmark_footer-----------)![](../Images/c5978871674c655a78ca67c37e7a48b0.png)

图片由 [Victoriano Izquierdo](https://unsplash.com/@victoriano?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

评估机器学习算法是数据科学家常见的任务。虽然数据科学家需要了解不同类型的机器学习算法以应对不同类型的问题，但至关重要的是，他/她必须将不同的算法应用到自己特定的数据集上。只有这样，他/她才能更好地了解使用哪个算法来训练模型，以及如何在之后进行超参数调整。然而，选择正确的算法是一个耗时且令人疲惫的过程。理想情况下，应该有一个自动化的过程，你只需要提供你的数据，系统就会为你选择理想的机器学习算法。

对此的答案是**LazyPredict**。**LazyPredict**是一个Python库，帮助你部分自动化选择最佳算法来训练你的数据集的过程。通过提供你的数据，LazyPredict将使用超过60种机器学习算法来训练一个模型。最终结果会呈现给你。然后，你可以选择表现最佳的机器学习算法来进一步训练或优化你的数据集。

# 选择机器…
