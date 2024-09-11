# 预测多个时间段：以天气数据为例

> 原文：[`towardsdatascience.com/forecast-multiple-horizons-an-example-with-weather-data-8d5fa4321e07?source=collection_archive---------4-----------------------#2023-08-06`](https://towardsdatascience.com/forecast-multiple-horizons-an-example-with-weather-data-8d5fa4321e07?source=collection_archive---------4-----------------------#2023-08-06)

## 使用预测时间段作为特征来预测瑞士的降水量。

[](https://medium.com/@davide.burba?source=post_page-----8d5fa4321e07--------------------------------)![Davide Burba](https://medium.com/@davide.burba?source=post_page-----8d5fa4321e07--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8d5fa4321e07--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d5fa4321e07--------------------------------) [Davide Burba](https://medium.com/@davide.burba?source=post_page-----8d5fa4321e07--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9f58aaaeaed7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforecast-multiple-horizons-an-example-with-weather-data-8d5fa4321e07&user=Davide+Burba&userId=9f58aaaeaed7&source=post_page-9f58aaaeaed7----8d5fa4321e07---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8d5fa4321e07--------------------------------) ·8 分钟阅读·2023 年 8 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8d5fa4321e07&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforecast-multiple-horizons-an-example-with-weather-data-8d5fa4321e07&user=Davide+Burba&userId=9f58aaaeaed7&source=-----8d5fa4321e07---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8d5fa4321e07&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fforecast-multiple-horizons-an-example-with-weather-data-8d5fa4321e07&source=-----8d5fa4321e07---------------------bookmark_footer-----------)![](img/0a51955e062687c7048d4f894669ee7e.png)

天气预报，作者 [Giulia Roggia](https://www.instagram.com/giulia_roggia__/)。经许可使用。

+   介绍

+   示例：瑞士降水量

# 介绍

## *传统方法*

当我们希望预测时间序列的未来值时，我们通常对多个未来时间段感兴趣，例如 1、2 或 3 个月后会发生什么。预测这些不同时间段的传统方法是为每个目标时间段训练一个单独的模型。

## *常见替代方案*

一种常见的替代方案是训练一个单一模型来处理短期预测，然后通过递归应用（即将之前的预测作为输入来生成接下来的预测）扩展到多时间范围。然而，这种方法在生产系统中可能实现起来较为复杂，并且可能导致误差传播：短期内的误差可能对后续预测产生不利影响。

另一种选择是使用多变量模型同时预测所有的时间范围。然而，支持多变量输出的模型类型有限，并且需要额外的努力来……
