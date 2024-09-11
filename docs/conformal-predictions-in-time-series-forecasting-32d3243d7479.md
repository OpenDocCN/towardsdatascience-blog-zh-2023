# 时间序列预测中的**符合性预测**

> 原文：[`towardsdatascience.com/conformal-predictions-in-time-series-forecasting-32d3243d7479?source=collection_archive---------3-----------------------#2023-12-12`](https://towardsdatascience.com/conformal-predictions-in-time-series-forecasting-32d3243d7479?source=collection_archive---------3-----------------------#2023-12-12)

## 探索符合性预测的概念，并将其应用于时间序列预测领域，并在 Python 中实现

[](https://medium.com/@marcopeixeiro?source=post_page-----32d3243d7479--------------------------------)![Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----32d3243d7479--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32d3243d7479--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----32d3243d7479--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----32d3243d7479--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconformal-predictions-in-time-series-forecasting-32d3243d7479&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----32d3243d7479---------------------post_header-----------) 发表在 [数据科学的未来](https://towardsdatascience.com/?source=post_page-----32d3243d7479--------------------------------) ·10 分钟阅读·2023 年 12 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F32d3243d7479&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconformal-predictions-in-time-series-forecasting-32d3243d7479&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=-----32d3243d7479---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F32d3243d7479&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconformal-predictions-in-time-series-forecasting-32d3243d7479&source=-----32d3243d7479---------------------bookmark_footer-----------)![](img/c4815b8031d5f2b1e038b0ee4a966ea6.png)

[Keith Markilie](https://unsplash.com/@rhino007?utm_source=medium&utm_medium=referral) 的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

考虑预测呼叫中心的呼叫量这一任务。预测在其中起着至关重要的作用，因为它们决定了预算分配和人员规划（如果预期有更多的电话，那么应当安排更多的接线员）。

因此我们建立了一个预测模型，并报告下周中心将接到 2451 个电话。

当然，任何未来的预测都会带来一些误差和不确定性。那么我们如何量化它呢？

逻辑上的答案是使用**预测区间**。这样，我们可以在一定的置信水平下报告一系列可能的未来值。

尽管有许多计算预测区间的方法，但它们并不适用于所有模型，并且通常依赖于特定的分布。

这带来了两个主要问题。首先，分布假设在某些情况下可能不成立。其次，我们可能在选择建模技术时受到限制。

例如，没有简单的方法来测量神经网络的预测区间，但这些模型可以…
