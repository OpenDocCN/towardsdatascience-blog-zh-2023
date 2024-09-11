# 基于熵的不确定性预测

> 原文：[https://towardsdatascience.com/entropy-based-uncertainty-prediction-812cca769d7a?source=collection_archive---------6-----------------------#2023-09-02](https://towardsdatascience.com/entropy-based-uncertainty-prediction-812cca769d7a?source=collection_archive---------6-----------------------#2023-09-02)

## 本文探讨了如何将熵作为图像分割任务中的不确定性估计工具。我们将介绍熵的定义以及如何使用 Python 实现它。

[](https://medium.com/@francoisporcher?source=post_page-----812cca769d7a--------------------------------)[![François Porcher](../Images/9ddb233f8cadbd69026bd79e2bd62dea.png)](https://medium.com/@francoisporcher?source=post_page-----812cca769d7a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----812cca769d7a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----812cca769d7a--------------------------------) [François Porcher](https://medium.com/@francoisporcher?source=post_page-----812cca769d7a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e8e73046f53&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-based-uncertainty-prediction-812cca769d7a&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=post_page-8e8e73046f53----812cca769d7a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----812cca769d7a--------------------------------) · 7 min read · 2023年9月2日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F812cca769d7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-based-uncertainty-prediction-812cca769d7a&user=Fran%C3%A7ois+Porcher&userId=8e8e73046f53&source=-----812cca769d7a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F812cca769d7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fentropy-based-uncertainty-prediction-812cca769d7a&source=-----812cca769d7a---------------------bookmark_footer-----------)![](../Images/1a2be74cacdb994609be32b738dbe7bb.png)

[Michael Dziedzic](https://unsplash.com/@lazycreekimages?utm_source=medium&utm_medium=referral) 的照片，来自于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在剑桥大学担任神经成像和人工智能研究科学家期间，我面临着使用最新深度学习技术对复杂脑部数据集进行图像分割的挑战，特别是[nnU-Net](https://medium.com/towards-data-science/the-ultimate-guide-to-nnu-net-for-state-of-the-art-image-segmentation-6dda7f44b935)。在这个过程中，我观察到一个显著的差距：对不确定性估计的忽视。然而，**不确定性对可靠决策至关重要**。

在深入具体细节之前，可以随时查看我的[Github 仓库](https://github.com/FrancoisPorcher)，其中包含了本文讨论的所有代码片段。

# 图像分割中的不确定性重要性

在计算机视觉和机器学习领域，图像分割是一个核心问题。无论是在医学成像、自动驾驶汽车还是机器人技术中，准确的分割对有效的决策至关重要。然而，一个常常被忽视的方面是**与这些分割相关的不确定性测量**。

> 我们为什么要关心……
