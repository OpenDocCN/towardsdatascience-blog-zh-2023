# 时间序列变换（及恢复）变得简单

> 原文：[https://towardsdatascience.com/time-series-transformations-and-reverting-made-easy-f4f768c18f63?source=collection_archive---------5-----------------------#2023-01-26](https://towardsdatascience.com/time-series-transformations-and-reverting-made-easy-f4f768c18f63?source=collection_archive---------5-----------------------#2023-01-26)

## 探索时间序列的变换及如何使用scalecast在Python中将其恢复

[](https://mikekeith52.medium.com/?source=post_page-----f4f768c18f63--------------------------------)[![Michael Keith](../Images/4ebd39b25a1faae3586eb25ec83d3e91.png)](https://mikekeith52.medium.com/?source=post_page-----f4f768c18f63--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f4f768c18f63--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f4f768c18f63--------------------------------) [Michael Keith](https://mikekeith52.medium.com/?source=post_page-----f4f768c18f63--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F85177a9cbd35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-transformations-and-reverting-made-easy-f4f768c18f63&user=Michael+Keith&userId=85177a9cbd35&source=post_page-85177a9cbd35----f4f768c18f63---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f4f768c18f63--------------------------------) ·5分钟阅读·2023年1月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff4f768c18f63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-transformations-and-reverting-made-easy-f4f768c18f63&user=Michael+Keith&userId=85177a9cbd35&source=-----f4f768c18f63---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff4f768c18f63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-transformations-and-reverting-made-easy-f4f768c18f63&source=-----f4f768c18f63---------------------bookmark_footer-----------)![](../Images/6ce1d41146609f8ca174df859a6cde91.png)

图片由作者提供

在预测时间序列数据时，平稳性是一个重要因素。某些模型——如ARIMA、霍尔特-温特斯、指数平滑等——专门针对时间序列数据，并不一定要求数据是平稳的。一个序列的平稳性指的是其随时间回到均值的趋势。非平稳序列在数据集中引入了趋势，这可能导致依赖于该趋势的函数过于偏倚，从而产生虚假的结果（看似良好的结果）。

指数平滑模型平滑最近的趋势，以应对非平稳性，而 ARIMA 会在积分因子（标准 ARIMA 顺序中的中间项）设置为大于 0 时对数据进行差分。其他时间序列模型以不同的方式处理平稳性。

然而，像 XGBoost 这样的流行机器学习模型并不考虑时间序列的平稳性，因为它并不是为了处理时间序列数据的特性而开发的。这并不一定意味着你不应该使用它进行预测，但在这样做之前，处理潜在的非平稳性是个好主意。这通常意味着应用一种变换——处理原始数据...
