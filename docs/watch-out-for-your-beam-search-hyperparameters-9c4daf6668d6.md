# 注意你的 Beam Search 超参数

> 原文：[https://towardsdatascience.com/watch-out-for-your-beam-search-hyperparameters-9c4daf6668d6?source=collection_archive---------12-----------------------#2023-01-11](https://towardsdatascience.com/watch-out-for-your-beam-search-hyperparameters-9c4daf6668d6?source=collection_archive---------12-----------------------#2023-01-11)

## 默认值从来不是最优的。

[](https://medium.com/@bnjmn_marie?source=post_page-----9c4daf6668d6--------------------------------)[![Benjamin Marie](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----9c4daf6668d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c4daf6668d6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9c4daf6668d6--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----9c4daf6668d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwatch-out-for-your-beam-search-hyperparameters-9c4daf6668d6&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----9c4daf6668d6---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c4daf6668d6--------------------------------) ·6 min 阅读·2023年1月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9c4daf6668d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwatch-out-for-your-beam-search-hyperparameters-9c4daf6668d6&user=Benjamin+Marie&userId=ad2a414578b3&source=-----9c4daf6668d6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c4daf6668d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwatch-out-for-your-beam-search-hyperparameters-9c4daf6668d6&source=-----9c4daf6668d6---------------------bookmark_footer-----------)![](../Images/0fab5a914d706a814394dc57392b6d05.png)

图片来源于[Paulius Dragunas](https://unsplash.com/@paulius005?utm_source=medium&utm_medium=referral)的[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

在使用神经模型开发应用时，尝试不同的超参数以训练模型是很常见的。

例如，学习率、学习计划和 dropout 率是重要的超参数，它们对模型的学习曲线有显著影响。

更少见的是对**最佳解码超参数**的搜索。如果你阅读深度学习教程或处理自然语言处理应用的科学论文，很可能**用于推理的超参数甚至没有被提及**。

大多数作者，包括我自己，都不愿意去寻找最佳的解码超参数，而是使用默认的超参数。

然而，这些超参数实际上会对结果产生显著影响，无论你使用什么解码算法，总会有一些**需要微调以获得更好结果**的超参数。

在这篇博客文章中，我通过简单的 Python 示例和机器翻译应用展示了解码超参数的影响。我重点介绍了束搜索，因为这是迄今为止最流行的解码算法，以及两个特定的超参数。
