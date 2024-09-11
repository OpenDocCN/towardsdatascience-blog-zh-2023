# 置信区间与预测区间

> 原文：[`towardsdatascience.com/confidence-vs-prediction-intervals-are-you-making-these-costly-analysis-mistakes-fa02b074498?source=collection_archive---------6-----------------------#2023-06-04`](https://towardsdatascience.com/confidence-vs-prediction-intervals-are-you-making-these-costly-analysis-mistakes-fa02b074498?source=collection_archive---------6-----------------------#2023-06-04)

## 探讨置信区间与预测区间之间的主要区别

[](https://medium.com/@egorhowell?source=post_page-----fa02b074498--------------------------------)![Egor Howell](https://medium.com/@egorhowell?source=post_page-----fa02b074498--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fa02b074498--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa02b074498--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----fa02b074498--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconfidence-vs-prediction-intervals-are-you-making-these-costly-analysis-mistakes-fa02b074498&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----fa02b074498---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fa02b074498--------------------------------) ·6 分钟阅读·2023 年 6 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffa02b074498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconfidence-vs-prediction-intervals-are-you-making-these-costly-analysis-mistakes-fa02b074498&user=Egor+Howell&userId=1cac491223b2&source=-----fa02b074498---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffa02b074498&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconfidence-vs-prediction-intervals-are-you-making-these-costly-analysis-mistakes-fa02b074498&source=-----fa02b074498---------------------bookmark_footer-----------)![](img/0431f8f5d270f3410c3ddff11082e605.png)

图片由 [Jarosław Kwoczała](https://unsplash.com/@sumekler?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

术语‘[***置信区间***](https://medium.com/towards-data-science/confidence-intervals-simply-explained-58b0b11e985f)’和‘[***预测区间***](https://en.wikipedia.org/wiki/Prediction_interval)***l***’在数据科学会议中经常被混用。我必须承认，有时我自己也这样做过，只是为了显得聪明。

但是，这样做是危险的。**置信区间**和**预测区间**指的是非常不同的概念，你可能会被那些理解差异的人打个措手不及，那将非常尴尬。

但不要绝望！在本文中，我将直观地解释这两种区间之间的区别，并让你对应用它们充满信心。

# 置信区间

## 概述

更为人知的术语是置信区间，我们从这里开始。置信区间是对一些样本参数的不确定性的度量，例如样本的均值或[**回归模型**](https://en.wikipedia.org/wiki/Regression_analysis)中的系数。它帮助我们理解我们的估计值与真实总体值的接近程度。如果你对置信区间感兴趣，可以查看我以前关于这个话题的博客文章：
