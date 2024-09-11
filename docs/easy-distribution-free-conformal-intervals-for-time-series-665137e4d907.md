# 轻松的分布无关的时间序列拟合区间

> 原文：[https://towardsdatascience.com/easy-distribution-free-conformal-intervals-for-time-series-665137e4d907?source=collection_archive---------18-----------------------#2023-02-15](https://towardsdatascience.com/easy-distribution-free-conformal-intervals-for-time-series-665137e4d907?source=collection_archive---------18-----------------------#2023-02-15)

## 使用 Python 和你的测试集来推导分布无关的区间

[](https://mikekeith52.medium.com/?source=post_page-----665137e4d907--------------------------------)[![Michael Keith](../Images/4ebd39b25a1faae3586eb25ec83d3e91.png)](https://mikekeith52.medium.com/?source=post_page-----665137e4d907--------------------------------)[](https://towardsdatascience.com/?source=post_page-----665137e4d907--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----665137e4d907--------------------------------) [Michael Keith](https://mikekeith52.medium.com/?source=post_page-----665137e4d907--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F85177a9cbd35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-distribution-free-conformal-intervals-for-time-series-665137e4d907&user=Michael+Keith&userId=85177a9cbd35&source=post_page-85177a9cbd35----665137e4d907---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----665137e4d907--------------------------------) ·7分钟阅读·2023年2月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F665137e4d907&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-distribution-free-conformal-intervals-for-time-series-665137e4d907&user=Michael+Keith&userId=85177a9cbd35&source=-----665137e4d907---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F665137e4d907&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feasy-distribution-free-conformal-intervals-for-time-series-665137e4d907&source=-----665137e4d907---------------------bookmark_footer-----------)![](../Images/8430caa524f9400ed30923d829884482.png)

照片由 [Gilly](https://unsplash.com/@gillyberlin?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

对于预测应用来说，生成一个点估计和确定实际值可能与预测值的偏差一样重要。大多数预测并非 100% 准确，因此在处理模型实现时，对可能性的良好把握变得至关重要。对于具有底层函数形式的模型，例如 ARIMA，可以使用残差的假设分布和估计的标准误差来确定置信区间。这些区间是合理的，因为它们随着预测从已知最后值越远而扩大——随着不确定性的积累，这以数学方式呈现，与我们的直觉相符。而*如果*模型假设成立，95% 的置信区间可以保证包含 95% 的实际值。

# 符合预测

然而，当处理一个无法用简单方程表示且假设底层数据没有分布的机器学习模型时，创建一个可靠的置信区间变得更加具有挑战性。解决这一问题的一个流行方法是符合预测。GitHub 仓库，[Awesome Conformal Prediction](https://github.com/valeman/awesome-conformal-prediction)……
