# 动态符合区间用于任何时间序列模型

> 原文：[`towardsdatascience.com/dynamic-conformal-intervals-for-any-time-series-model-d1638aa48527?source=collection_archive---------13-----------------------#2023-04-17`](https://towardsdatascience.com/dynamic-conformal-intervals-for-any-time-series-model-d1638aa48527?source=collection_archive---------13-----------------------#2023-04-17)

## 应用并通过回测动态扩展区间

[](https://mikekeith52.medium.com/?source=post_page-----d1638aa48527--------------------------------)![Michael Keith](https://mikekeith52.medium.com/?source=post_page-----d1638aa48527--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d1638aa48527--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d1638aa48527--------------------------------) [Michael Keith](https://mikekeith52.medium.com/?source=post_page-----d1638aa48527--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F85177a9cbd35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-conformal-intervals-for-any-time-series-model-d1638aa48527&user=Michael+Keith&userId=85177a9cbd35&source=post_page-85177a9cbd35----d1638aa48527---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d1638aa48527--------------------------------) ·8 分钟阅读·2023 年 4 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd1638aa48527&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-conformal-intervals-for-any-time-series-model-d1638aa48527&user=Michael+Keith&userId=85177a9cbd35&source=-----d1638aa48527---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd1638aa48527&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdynamic-conformal-intervals-for-any-time-series-model-d1638aa48527&source=-----d1638aa48527---------------------bookmark_footer-----------)![](img/c0838f13c2dd273cf20f2025a6148207.png)

照片由[Léonard Cotte](https://unsplash.com/@ettocl?utm_source=medium&utm_medium=referral)提供，拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

根据生成预测的目的，评估准确的置信区间可能是一个关键任务。大多数经典经济计量模型，建立在关于预测和残差分布的假设基础上，都内置了这种方法。当转向使用机器学习进行时间序列预测时，例如 XGBoost 或递归神经网络，情况可能会更复杂。一种流行的技术是符合性区间——这是一种量化不确定性的方法，不对预测分布做任何假设。

# 朴素符合性区间

这种方法的最简单实现是训练一个模型并保留一个测试集。如果这个测试集至少包含 20 个观测值（假设我们需要 95% 的确定性），我们可以通过在任何点预测上加上一个正负值来构建一个区间，这个正负值代表测试集残差绝对值的第 95 个百分位数。然后我们在整个序列上重新拟合模型，并将这个正负值应用到所有未知范围的点预测上。这可以被认为是朴素符合性区间。

[Scalecast](https://github.com/mikekeith52/scalecast) 是一个 Python 中的预测库，如果你想在应用优化机器之前对序列进行变换，它会很好地工作。
