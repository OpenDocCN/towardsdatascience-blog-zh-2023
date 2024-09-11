# 如何：使用时间序列数据进行交叉验证

> 原文：[`towardsdatascience.com/how-to-cross-validation-with-time-series-data-9802a06272c6?source=collection_archive---------4-----------------------#2023-12-29`](https://towardsdatascience.com/how-to-cross-validation-with-time-series-data-9802a06272c6?source=collection_archive---------4-----------------------#2023-12-29)

## 处理时间序列数据时，你必须以不同的方式进行交叉验证

[](https://medium.com/@pelletierhaden?source=post_page-----9802a06272c6--------------------------------)![Haden Pelletier](https://medium.com/@pelletierhaden?source=post_page-----9802a06272c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9802a06272c6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9802a06272c6--------------------------------) [Haden Pelletier](https://medium.com/@pelletierhaden?source=post_page-----9802a06272c6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb14d1de976eb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-cross-validation-with-time-series-data-9802a06272c6&user=Haden+Pelletier&userId=b14d1de976eb&source=post_page-b14d1de976eb----9802a06272c6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9802a06272c6--------------------------------) ·5 分钟阅读·2023 年 12 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9802a06272c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-cross-validation-with-time-series-data-9802a06272c6&user=Haden+Pelletier&userId=b14d1de976eb&source=-----9802a06272c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9802a06272c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-cross-validation-with-time-series-data-9802a06272c6&source=-----9802a06272c6---------------------bookmark_footer-----------)![](img/e6086d85a950ceb80f3d99b0979ecd94.png)

标准 k 折交叉验证。图像由作者提供

**交叉验证**是训练和评估机器学习模型的重要部分。它可以让你估计一个训练好的模型在新数据上的表现。

大多数学习如何进行交叉验证的人首先会了解**K 折叠方法**。我也是这样学到的。在 K 折叠交叉验证中，数据集会被随机拆分成 n 个折叠（通常是 5 个）。在 5 次迭代过程中，模型在 5 个折叠中的 4 个上进行训练，而剩下的 1 个作为测试集来评估性能。这会重复进行，直到所有 5 个折叠在某个时间点被用作测试集。到最后，你将得到 5 个误差分数，将这些分数平均，便能得出你的交叉验证分数。

但这里有个问题——这种方法实际上只适用于非时间序列/非顺序数据。如果数据的顺序很重要，或者某些数据点依赖于前面的值，**你不能使用 K 折叠交叉验证**。

原因很简单。如果你使用 KFold 将数据拆分成 4 个训练折叠和 1 个测试折叠，你将随机化数据的顺序。因此，曾经在其他数据点之前的数据点可能会出现在...
