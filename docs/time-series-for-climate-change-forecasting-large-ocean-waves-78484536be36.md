# 气候变化的时间序列：大型海洋波浪的预测

> 原文：[https://towardsdatascience.com/time-series-for-climate-change-forecasting-large-ocean-waves-78484536be36?source=collection_archive---------13-----------------------#2023-04-25](https://towardsdatascience.com/time-series-for-climate-change-forecasting-large-ocean-waves-78484536be36?source=collection_archive---------13-----------------------#2023-04-25)

## 如何使用时间序列分析和预测来应对气候变化

[](https://vcerq.medium.com/?source=post_page-----78484536be36--------------------------------)[![Vitor Cerqueira](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----78484536be36--------------------------------)[](https://towardsdatascience.com/?source=post_page-----78484536be36--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----78484536be36--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----78484536be36--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-for-climate-change-forecasting-large-ocean-waves-78484536be36&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----78484536be36---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----78484536be36--------------------------------) ·7分钟阅读·2023年4月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F78484536be36&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-for-climate-change-forecasting-large-ocean-waves-78484536be36&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----78484536be36---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F78484536be36&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-for-climate-change-forecasting-large-ocean-waves-78484536be36&source=-----78484536be36---------------------bookmark_footer-----------)![](../Images/51be17536773dba8b8f4d4fcf45816a8.png)

图片由 [Silas Baisch](https://unsplash.com/@silasbaisch?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是系列*气候变化的时间序列*的第3部分。文章列表：

+   第1部分：[风能预测](/time-series-for-climate-change-forecasting-wind-power-8ed6d653a255)

+   第2部分：[太阳辐射预测](/time-series-for-climate-change-solar-irradiance-forecasting-a972dac7418f)

# 海洋波浪能

![](../Images/a824fd1899aa906e02c05aaf6c489269.png)

波浪能转换器是从海洋波浪中收集能量的浮标。照片（浮标，但不是WEC）由[Emmy C](https://unsplash.com/@emmy__c?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

海洋波浪是一个有前景的可再生能源来源。

为什么选择海洋波浪？

当谈到可再生能源时，人们通常会想到太阳能或风能。这些是最受欢迎的可再生能源来源。然而，由于其一致性，海洋波浪具有巨大的潜力。

我们可以在大约90%的时间内从海洋波浪中收集能量。这个数字对于太阳能或风能约为20%-30%。你可以查阅参考文献[1]获取详细信息。例如，太阳能技术只能在白天工作。

除了其一致性，波浪能的可预测性也优于上述两种替代方案。当前，海洋波浪能的主要限制是生产成本。现在，这些成本相对较高…
