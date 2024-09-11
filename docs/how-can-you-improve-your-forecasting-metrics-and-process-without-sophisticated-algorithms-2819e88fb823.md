# 如何在没有复杂算法的情况下提高预测指标和过程？

> 原文：[https://towardsdatascience.com/how-can-you-improve-your-forecasting-metrics-and-process-without-sophisticated-algorithms-2819e88fb823?source=collection_archive---------11-----------------------#2023-08-26](https://towardsdatascience.com/how-can-you-improve-your-forecasting-metrics-and-process-without-sophisticated-algorithms-2819e88fb823?source=collection_archive---------11-----------------------#2023-08-26)

## 开发描述性分析可以帮助确定重点领域，以改善需求规划的关键绩效指标（KPI）和过程效率。

[](https://medium.com/@rkumar5680?source=post_page-----2819e88fb823--------------------------------)[![Ramkumar K](../Images/37695331380b695bbc3b3a23eae6a267.png)](https://medium.com/@rkumar5680?source=post_page-----2819e88fb823--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2819e88fb823--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2819e88fb823--------------------------------) [Ramkumar K](https://medium.com/@rkumar5680?source=post_page-----2819e88fb823--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe330097ea68c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-can-you-improve-your-forecasting-metrics-and-process-without-sophisticated-algorithms-2819e88fb823&user=Ramkumar+K&userId=e330097ea68c&source=post_page-e330097ea68c----2819e88fb823---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2819e88fb823--------------------------------) · 10分钟阅读 · 2023年8月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2819e88fb823&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-can-you-improve-your-forecasting-metrics-and-process-without-sophisticated-algorithms-2819e88fb823&user=Ramkumar+K&userId=e330097ea68c&source=-----2819e88fb823---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2819e88fb823&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-can-you-improve-your-forecasting-metrics-and-process-without-sophisticated-algorithms-2819e88fb823&source=-----2819e88fb823---------------------bookmark_footer-----------)![](../Images/ff06529f8fef5c12f893bec16cc1ea80.png)

照片由[Lukas Blazek](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral)拍摄，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

# 介绍

需求规划是*综合业务规划*（IBP）过程的前沿环节。这个阶段的结果形成了其他阶段（例如供应规划、生产规划）的基础。这个阶段通常从生成基于历史销售数据的统计预测开始，并将其与销售输入相结合，以得出初步基线预测。然后，通过与销售和市场相关方的审查和对齐，这一预测会被细化成共识预测。我们通常将*预测准确性和偏差*作为这一阶段的关键绩效指标（KPI）来跟踪。供应链、销售和市场团队可以花费大量时间、精力（和资金）来确保预测准确，通常会投资于昂贵的预测工具。虽然拥有一套先进的算法来提高预测准确性确实很有帮助，但从历史销售和预测数据中得出的描述性分析也可以帮助简化需求规划过程并提升KPI。

# 为什么我们需要这样的…
