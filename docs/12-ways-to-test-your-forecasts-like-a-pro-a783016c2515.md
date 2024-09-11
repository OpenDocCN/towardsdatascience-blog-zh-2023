# 12 种像专家一样测试你的预测的方法

> 原文：[https://towardsdatascience.com/12-ways-to-test-your-forecasts-like-a-pro-a783016c2515?source=collection_archive---------2-----------------------#2023-03-07](https://towardsdatascience.com/12-ways-to-test-your-forecasts-like-a-pro-a783016c2515?source=collection_archive---------2-----------------------#2023-03-07)

## 如何在文献中提出的 12 种策略中找到最佳的时间序列预测性能估计方法。附带 Python 代码。

[](https://medium.com/@mazzanti.sam?source=post_page-----a783016c2515--------------------------------)[![Samuele Mazzanti](../Images/432477d6418a3f79bf25dec42755d364.png)](https://medium.com/@mazzanti.sam?source=post_page-----a783016c2515--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a783016c2515--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a783016c2515--------------------------------) [Samuele Mazzanti](https://medium.com/@mazzanti.sam?source=post_page-----a783016c2515--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe16f3bb86e03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F12-ways-to-test-your-forecasts-like-a-pro-a783016c2515&user=Samuele+Mazzanti&userId=e16f3bb86e03&source=post_page-e16f3bb86e03----a783016c2515---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a783016c2515--------------------------------) ·11 分钟阅读·2023年3月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa783016c2515&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F12-ways-to-test-your-forecasts-like-a-pro-a783016c2515&user=Samuele+Mazzanti&userId=e16f3bb86e03&source=-----a783016c2515---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa783016c2515&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F12-ways-to-test-your-forecasts-like-a-pro-a783016c2515&source=-----a783016c2515---------------------bookmark_footer-----------)![](../Images/d0f73fea676925a708e3dde3d9d48059.png)

[作者提供的图片]

数据科学家总是专注于寻找适合他们数据集的最佳模型。然而，他们常常**忽视了选择最佳性能估计方法的重要性**。

这很不幸，因为如果你不能合理确定 MAE 在未来的数据上实际接近 5.2，那么在测试集上估算 MAE 为 5.2 就是无用的。

性能估计的话题在处理预测时尤为关键。与其他应用不同，**在时间序列中观察的顺序很重要，因此我们不能仅仅使用交叉验证**并假设它会有效。

这就是为什么文献中提出了许多性能估计方法。在本文中，我们将比较这12种方法在58个真实时间序列数据集上的表现。结果令人惊讶。

# 处理时间序列

时间序列是对一个或多个变量在不同时间点的观察。这意味着，与其他类型的数据集不同，观察具有固有的顺序。
