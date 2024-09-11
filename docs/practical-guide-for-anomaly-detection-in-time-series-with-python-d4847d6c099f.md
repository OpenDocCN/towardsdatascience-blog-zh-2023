# 使用Python进行时间序列异常检测的实用指南

> 原文：[https://towardsdatascience.com/practical-guide-for-anomaly-detection-in-time-series-with-python-d4847d6c099f?source=collection_archive---------1-----------------------#2023-03-16](https://towardsdatascience.com/practical-guide-for-anomaly-detection-in-time-series-with-python-d4847d6c099f?source=collection_archive---------1-----------------------#2023-03-16)

## 一篇关于使用Python和sklearn检测时间序列数据中异常值的实操文章

[](https://medium.com/@marcopeixeiro?source=post_page-----d4847d6c099f--------------------------------)[![Marco Peixeiro](../Images/7cf0a81d87281d35ff47f51e3026a3e9.png)](https://medium.com/@marcopeixeiro?source=post_page-----d4847d6c099f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d4847d6c099f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d4847d6c099f--------------------------------) [Marco Peixeiro](https://medium.com/@marcopeixeiro?source=post_page-----d4847d6c099f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F741c1c8fcfbd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-guide-for-anomaly-detection-in-time-series-with-python-d4847d6c099f&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=post_page-741c1c8fcfbd----d4847d6c099f---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d4847d6c099f--------------------------------) ·13 min read·2023年3月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd4847d6c099f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-guide-for-anomaly-detection-in-time-series-with-python-d4847d6c099f&user=Marco+Peixeiro&userId=741c1c8fcfbd&source=-----d4847d6c099f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd4847d6c099f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-guide-for-anomaly-detection-in-time-series-with-python-d4847d6c099f&source=-----d4847d6c099f---------------------bookmark_footer-----------)![](../Images/4b3f1bd78c54730bd401ce05e9b15b28.png)

照片由[Will Myers](https://unsplash.com/@will_myers?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

异常检测是一项任务，旨在识别与大多数数据显著偏离的稀有事件。

时间序列中的异常检测有广泛的实际应用，从制造业到医疗保健。异常现象表明有意外事件发生，它们可能由生产故障或系统缺陷引起。例如，如果我们监控一个网站的访客数量，当数量降到0时，这可能意味着服务器出现故障。

在进行预测建模之前，检测时间序列数据中的异常也很有用。许多预测模型是自回归的，这意味着它们会考虑过去的值来进行预测。过去的异常值肯定会影响模型，因此去除这些异常值可以获得更合理的预测。

在这篇文章中，我们将探讨三种不同的异常检测技术，并在Python中实现它们。

1.  平均绝对偏差（MAD）

1.  隔离森林

1.  局部离群因子（LOF）
