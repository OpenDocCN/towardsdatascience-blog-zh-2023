# 时间序列预测中的模型评估

> 原文：[https://towardsdatascience.com/model-evaluation-in-time-series-forecasting-ae41993e267c?source=collection_archive---------2-----------------------#2023-03-05](https://towardsdatascience.com/model-evaluation-in-time-series-forecasting-ae41993e267c?source=collection_archive---------2-----------------------#2023-03-05)

## 介绍使用 Skforecast 库进行时间序列的回测

[![Javier Fernandez](../Images/d881a426c3f28ad5f41355a7aa92ed86.png)](https://javiferfer.medium.com/?source=post_page-----ae41993e267c--------------------------------) [Javier Fernandez](https://javiferfer.medium.com/?source=post_page-----ae41993e267c--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ae41993e267c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a71a903e8c3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-evaluation-in-time-series-forecasting-ae41993e267c&user=Javier+Fernandez&userId=8a71a903e8c3&source=post_page-8a71a903e8c3----ae41993e267c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ae41993e267c--------------------------------) ·6 min read·2023年3月5日

--

![](../Images/0a6a23a2ca72f5acd77ac7e855528173.png)

照片由 [Lukas](https://www.pexels.com/@goumbik) 提供，来源于 [Pexels](https://www.pexels.com/photo/chart-close-up-data-desk-590022/)

时间序列预测是基于历史时间数据进行预测，以推动未来的战略决策，这在广泛的应用中都至关重要。

在评估模型时，我们将数据分为训练集和测试集。训练集用于训练模型并确定最佳超参数，而测试集用于评估模型。为了更全面地评估模型性能，通常使用[交叉验证](https://en.wikipedia.org/wiki/Cross-validation_%28statistics%29#:~:text=Cross%2Dvalidation%20is%20a%20resampling,model%20will%20perform%20in%20practice.)。交叉验证是一种重采样方法，它使用不同的数据集在多个迭代中测试和训练模型。

然而，在时间序列数据上实施直接的交叉验证是不可能的，因为它忽略了观测值之间的时间组件。因此，本文介绍了用于评估时间序列模型的不同方法，称为[回测](https://en.wikipedia.org/wiki/Backtesting)。

# 1\. 引言

回测是建模中使用的术语，指的是使用现有历史数据评估模型。它涉及选择多个训练集和测试集，逐步向前推进。回测的主要思想与交叉验证类似，只是回测…
