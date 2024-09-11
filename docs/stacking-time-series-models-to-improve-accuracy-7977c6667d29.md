# 堆叠时间序列模型以提高准确性

> 原文：[https://towardsdatascience.com/stacking-time-series-models-to-improve-accuracy-7977c6667d29?source=collection_archive---------3-----------------------#2023-02-28](https://towardsdatascience.com/stacking-time-series-models-to-improve-accuracy-7977c6667d29?source=collection_archive---------3-----------------------#2023-02-28)

## 从RNN、ARIMA和Prophet模型中提取信号以使用Catboost进行预测

[](https://mikekeith52.medium.com/?source=post_page-----7977c6667d29--------------------------------)[![Michael Keith](../Images/4ebd39b25a1faae3586eb25ec83d3e91.png)](https://mikekeith52.medium.com/?source=post_page-----7977c6667d29--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7977c6667d29--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7977c6667d29--------------------------------) [Michael Keith](https://mikekeith52.medium.com/?source=post_page-----7977c6667d29--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F85177a9cbd35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstacking-time-series-models-to-improve-accuracy-7977c6667d29&user=Michael+Keith&userId=85177a9cbd35&source=post_page-85177a9cbd35----7977c6667d29---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7977c6667d29--------------------------------) ·7 min read·2023年2月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7977c6667d29&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstacking-time-series-models-to-improve-accuracy-7977c6667d29&user=Michael+Keith&userId=85177a9cbd35&source=-----7977c6667d29---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7977c6667d29&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstacking-time-series-models-to-improve-accuracy-7977c6667d29&source=-----7977c6667d29---------------------bookmark_footer-----------)![](../Images/d4a54a6899fe0dff94fe8914297c3a54.png)

照片由 [Robert Sachowski](https://unsplash.com/@rsachowski?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

关于强大时间序列模型的研究很有深度。有很多选择，远远超出 ARIMA 等经典技术。最近，递归神经网络和 LSTM 已经成为许多研究人员的研究重点。从 [PePy](https://pepy.tech/project/prophet) 上列出的下载次数来看，Prophet 模型可能是时间序列预测者最广泛使用的模型，因为它的入门门槛较低。哪一种选项最适合你的时间序列？

也许答案是你应该尝试所有这些方法，并结合它们的各种优点。实现这一点的一个常见技术被称为堆叠。流行的机器学习库 scikit-learn 提供了一个可以用于时间序列任务的 [StackingRegressor](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.StackingRegressor.html)。我已经 [之前展示了如何使用它](https://medium.com/towards-data-science/expand-your-time-series-arsenal-with-these-models-10c807d37558) 处理类似的情况。

StackingRegressor 仍有一个限制，它只接受其他 scikit-learn 模型类和 API。因此，像 ARIMA 这样的模型，它在 scikit-learn 中不可用，或者来自 tensorflow 的深度网络，也就无法放入堆叠中。这里有一个解决方案。在这篇文章中，我将展示如何使用 [scalecast](https://github.com/mikekeith52/scalecast) 包和一个 Jupyter [notebook](https://scalecast-examples.readthedocs.io/en/latest/misc/stacking/custom_stacking.html) 扩展时间序列的堆叠方法。所使用的数据集…
