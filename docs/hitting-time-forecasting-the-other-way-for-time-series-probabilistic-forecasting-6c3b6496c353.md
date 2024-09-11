# 命中时间预测：时间序列概率预测的另一种方式

> 原文：[https://towardsdatascience.com/hitting-time-forecasting-the-other-way-for-time-series-probabilistic-forecasting-6c3b6496c353?source=collection_archive---------8-----------------------#2023-06-27](https://towardsdatascience.com/hitting-time-forecasting-the-other-way-for-time-series-probabilistic-forecasting-6c3b6496c353?source=collection_archive---------8-----------------------#2023-06-27)

## 需要多长时间才能达到特定值？

[](https://medium.com/@cerlymarco?source=post_page-----6c3b6496c353--------------------------------)[![Marco Cerliani](../Images/48a07a024349bac3c8e397bf5a0372e2.png)](https://medium.com/@cerlymarco?source=post_page-----6c3b6496c353--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6c3b6496c353--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6c3b6496c353--------------------------------) [Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----6c3b6496c353--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc843902314c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhitting-time-forecasting-the-other-way-for-time-series-probabilistic-forecasting-6c3b6496c353&user=Marco+Cerliani&userId=c843902314c7&source=post_page-c843902314c7----6c3b6496c353---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c3b6496c353--------------------------------) ·4 min read·2023年6月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6c3b6496c353&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhitting-time-forecasting-the-other-way-for-time-series-probabilistic-forecasting-6c3b6496c353&user=Marco+Cerliani&userId=c843902314c7&source=-----6c3b6496c353---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6c3b6496c353&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhitting-time-forecasting-the-other-way-for-time-series-probabilistic-forecasting-6c3b6496c353&source=-----6c3b6496c353---------------------bookmark_footer-----------)![](../Images/56b7e137fff75e868c9e0625e9861608.png)

图片由 [Mick Haupt](https://unsplash.com/@rocinante_11?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

能够进行准确预测的能力是每个时间序列预测应用的基础。为了实现这个目标，**数据科学家习惯于选择从点预测角度来看最小化误差的最佳模型**。这是正确的，但可能并不总是最有效的方法。

数据科学家还应考虑开发概率预测模型的可能性。这些模型不仅会提供点估计，还会生成上限和下限的可靠性区间，未来观测值很可能会落在这些区间内。尽管概率预测似乎是统计学或深度学习解决方案的特权，**任何模型都可以用来生成概率预测**。这一概念在我的[上一篇文章](https://medium.com/towards-data-science/time-series-forecasting-with-conformal-prediction-intervals-scikit-learn-is-all-you-need-4b68143a027a)中有所解释，**我介绍了保形预测作为一种利用任何scikit-learn模型来估计预测区间的方法**。

确实，点预测更容易向非技术利益相关者传达。同时，生成关于预测可靠性的关键绩效指标（KPI）的可能性是一种附加价值。**概率输出可能携带更多的信息来支持决策**。传达出有60%…
