# 气候变化时间序列：预测极端天气事件

> 原文：[https://towardsdatascience.com/times-series-for-climate-change-forecasting-extreme-weather-events-335dc199fb6f?source=collection_archive---------10-----------------------#2023-05-15](https://towardsdatascience.com/times-series-for-climate-change-forecasting-extreme-weather-events-335dc199fb6f?source=collection_archive---------10-----------------------#2023-05-15)

## 如何使用时间序列分析和预测来应对气候变化

[](https://vcerq.medium.com/?source=post_page-----335dc199fb6f--------------------------------)[![维托尔·塞尔克拉](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----335dc199fb6f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----335dc199fb6f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----335dc199fb6f--------------------------------) [维托尔·塞尔克拉](https://vcerq.medium.com/?source=post_page-----335dc199fb6f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimes-series-for-climate-change-forecasting-extreme-weather-events-335dc199fb6f&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----335dc199fb6f---------------------post_header-----------) 发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----335dc199fb6f--------------------------------) ·7分钟阅读·2023年5月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F335dc199fb6f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimes-series-for-climate-change-forecasting-extreme-weather-events-335dc199fb6f&user=Vitor+Cerqueira&userId=efb5f27c836d&source=-----335dc199fb6f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F335dc199fb6f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftimes-series-for-climate-change-forecasting-extreme-weather-events-335dc199fb6f&source=-----335dc199fb6f---------------------bookmark_footer-----------)![](../Images/0d01fb5fec458a97752c2df8d11c5c87.png)

照片由[DESIGNECOLOGIST](https://unsplash.com/@designecologist?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上提供

这是系列文章*气候变化时间序列*的第5部分。文章列表：

+   第1部分：[风力发电预测](/time-series-for-climate-change-forecasting-wind-power-8ed6d653a255)

+   第2部分：[太阳辐照预测](/time-series-for-climate-change-solar-irradiance-forecasting-a972dac7418f)

+   第3部分: [大型海浪预测](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-large-ocean-waves-78484536be36)

+   第4部分: [能源需求预测](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-energy-demand-79f39c24c85e)

# 极端天气事件

气候变化导致极端天气事件的数量增加。这些事件对人类生命和基础设施构成了重大风险。

![](../Images/31d70187d859e91c3632d05ed9966c6f.png)

美国每年冰雹和雷暴风事件的数量。[数据来源](https://www.ncdc.noaa.gov/stormevents/ftp.jsp)。图片由作者提供。

极端天气事件可能导致人员伤亡，并造成高达数十亿美元的损失。这种财务影响源于基础设施或农业资源的破坏。这些事件对受影响地区的社会经济发展有持久的影响。
