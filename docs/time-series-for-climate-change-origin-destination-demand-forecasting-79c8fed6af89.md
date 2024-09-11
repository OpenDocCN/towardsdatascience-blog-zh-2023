# 气候变化时间序列：起始-目的地需求预测

> 原文：[https://towardsdatascience.com/time-series-for-climate-change-origin-destination-demand-forecasting-79c8fed6af89?source=collection_archive---------14-----------------------#2023-06-15](https://towardsdatascience.com/time-series-for-climate-change-origin-destination-demand-forecasting-79c8fed6af89?source=collection_archive---------14-----------------------#2023-06-15)

## 开采浮动车数据应对气候变化

[](https://vcerq.medium.com/?source=post_page-----79c8fed6af89--------------------------------)[![Vitor Cerqueira](../Images/9e52f462c6bc20453d3ea273eb52114b.png)](https://vcerq.medium.com/?source=post_page-----79c8fed6af89--------------------------------)[](https://towardsdatascience.com/?source=post_page-----79c8fed6af89--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----79c8fed6af89--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----79c8fed6af89--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fefb5f27c836d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-for-climate-change-origin-destination-demand-forecasting-79c8fed6af89&user=Vitor+Cerqueira&userId=efb5f27c836d&source=post_page-efb5f27c836d----79c8fed6af89---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----79c8fed6af89--------------------------------) ·6分钟阅读·2023年6月15日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F79c8fed6af89&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-for-climate-change-origin-destination-demand-forecasting-79c8fed6af89&source=-----79c8fed6af89---------------------bookmark_footer-----------)![](../Images/8b8326d71e3d800402ab1a16ddf64987.png)

图片由[Denys Nevozhai](https://unsplash.com/fr/@dnevozhai?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是系列文章*气候变化时间序列*的第8部分。文章列表：

+   第1部分：[风能预测](/time-series-for-climate-change-forecasting-wind-power-8ed6d653a255)

+   第2部分：[太阳辐射预测](/time-series-for-climate-change-solar-irradiance-forecasting-a972dac7418f)

+   第三部分: [预测大型海洋波浪](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-large-ocean-waves-78484536be36)

+   第四部分: [预测能源需求](https://medium.com/towards-data-science/time-series-for-climate-change-forecasting-energy-demand-79f39c24c85e)

+   第五部分: [预测极端天气事件](/times-series-for-climate-change-forecasting-extreme-weather-events-335dc199fb6f)

+   第六部分: [使用深度学习进行精准农业](/time-series-for-climate-change-using-deep-learning-for-precision-agriculture-806878cab9c)

+   第七部分: [利用聚类减少食品浪费](https://medium.com/towards-data-science/time-series-for-climate-change-reducing-food-waste-with-clustering-c2f067ffa907)

# 浮动车数据用于出行建模

开采浮动车数据是智能交通系统中的关键任务。浮动车数据指的是由配备 GPS 设备的车辆收集的数据。这些数据提供了关于车辆位置和速度的信息。

理解城市内的出行模式是交通管理中的重要任务。例如，它有助于减少拥堵和整体交通活动。减少交通时间意味着排放的温室气体更少。因此，准确的模型对气候变化有积极影响。
