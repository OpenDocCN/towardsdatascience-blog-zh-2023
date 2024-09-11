# 在Grafana中创建时间序列图

> 原文：[https://towardsdatascience.com/creating-time-series-plots-in-grafana-f9dade30dff4?source=collection_archive---------9-----------------------#2023-02-09](https://towardsdatascience.com/creating-time-series-plots-in-grafana-f9dade30dff4?source=collection_archive---------9-----------------------#2023-02-09)

## 学习如何使用Python和Grafana绘制动态时间序列图

[](https://weimenglee.medium.com/?source=post_page-----f9dade30dff4--------------------------------)[![Wei-Meng Lee](../Images/10fc13e8a6858502d6a7b89fcaad7a10.png)](https://weimenglee.medium.com/?source=post_page-----f9dade30dff4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9dade30dff4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f9dade30dff4--------------------------------) [Wei-Meng Lee](https://weimenglee.medium.com/?source=post_page-----f9dade30dff4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6599e1e08a48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-time-series-plots-in-grafana-f9dade30dff4&user=Wei-Meng+Lee&userId=6599e1e08a48&source=post_page-6599e1e08a48----f9dade30dff4---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9dade30dff4--------------------------------) ·5分钟阅读·2023年2月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff9dade30dff4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-time-series-plots-in-grafana-f9dade30dff4&user=Wei-Meng+Lee&userId=6599e1e08a48&source=-----f9dade30dff4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff9dade30dff4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-time-series-plots-in-grafana-f9dade30dff4&source=-----f9dade30dff4---------------------bookmark_footer-----------)![](../Images/d9cd7b7c201a9cd6cf930c74708e8d41.png)

照片由[Dan Lohmar](https://unsplash.com/@dlohmar?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Grafana 是一个多平台开源分析和交互式可视化 Web 应用程序。如果你进行数据分析，Grafana 是一个非常宝贵的工具，它允许你使用各种数据源和不同的内置可视化类型来构建仪表板。

> 你可以从[https://grafana.com/grafana/download](https://grafana.com/grafana/download)下载和安装Grafana。支持的平台包括Mac、Windows、Linux、Docker和ARM。

在这篇文章中，我将向你展示如何构建一个时间序列图，以显示特定位置的温度和湿度。

> 对于这篇文章，我假设你已经对Grafana和MySQL的基础知识有所了解。如果你是Grafana的新手，请查看我在Code Magazine上发表的文章（[https://www.codemag.com/Article/2207061/Developing-Dashboards-Using-Grafana](https://www.codemag.com/Article/2207061/Developing-Dashboards-Using-Grafana)）。

[## 使用Grafana开发仪表板](https://www.codemag.com/Article/2207061/Developing-Dashboards-Using-Grafana?source=post_page-----f9dade30dff4--------------------------------)

### 作者：魏孟李 发表在：CODE Magazine: 2022年7月/8月 最后更新日期：2022年8月31日 在我之前的文章中…

[www.codemag.com](https://www.codemag.com/Article/2207061/Developing-Dashboards-Using-Grafana?source=post_page-----f9dade30dff4--------------------------------)

# 示例应用
