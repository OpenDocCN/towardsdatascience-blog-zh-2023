# 指标存储的实际应用

> 原文：[https://towardsdatascience.com/metrics-store-in-action-76b16a928b97?source=collection_archive---------1-----------------------#2023-02-23](https://towardsdatascience.com/metrics-store-in-action-76b16a928b97?source=collection_archive---------1-----------------------#2023-02-23)

## 使用 MetricFlow、Python、DuckDB、dbt 和 Streamlit 的教程

[](https://medium.com/@paul.kinsvater?source=post_page-----76b16a928b97--------------------------------)[![Paul Kinsvater](../Images/a117d2ba4ede758108ed76492e48a56a.png)](https://medium.com/@paul.kinsvater?source=post_page-----76b16a928b97--------------------------------)[](https://towardsdatascience.com/?source=post_page-----76b16a928b97--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----76b16a928b97--------------------------------) [Paul Kinsvater](https://medium.com/@paul.kinsvater?source=post_page-----76b16a928b97--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8ae6dbd4c80b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetrics-store-in-action-76b16a928b97&user=Paul+Kinsvater&userId=8ae6dbd4c80b&source=post_page-8ae6dbd4c80b----76b16a928b97---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----76b16a928b97--------------------------------) ·11分钟阅读·2023年2月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F76b16a928b97&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetrics-store-in-action-76b16a928b97&user=Paul+Kinsvater&userId=8ae6dbd4c80b&source=-----76b16a928b97---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F76b16a928b97&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmetrics-store-in-action-76b16a928b97&source=-----76b16a928b97---------------------bookmark_footer-----------)![](../Images/9ff87bf639bf82a3288b42575ba6a7c3.png)

指标层在中心定义了所有关键业务指标和维度。它将指标请求转换为 SQL，抽象化实现细节。图片由作者创建。

关于现代数据堆栈（MDS）的文献很多——大多数讨论集中在存储、摄取、转换和展示上。今天我们关注的是指标，这是众多其他[MDS类别](https://www.moderndatastack.xyz/categories)之一。

有人说指标是语义层的一个组成部分——见[这篇 Airbyte 博客文章](https://airbyte.com/blog/the-rise-of-the-semantic-layer-metrics-on-the-fly)。还有人（如我们）将“指标存储”、“指标层”和“语义层”这几个术语互换使用。

## 什么是指标存储，也称为指标层，或语义层？

> “上个月的收入在分析师的笔记本中报告的数据与我们仪表板上的数据不同。我们应该向审计报告哪个数字？”
> 
> “你知道我们在留存应用中如何定义流失率KPI吗？”
> 
> “我们如何将客户订单的收入按地理区域进行细分？”按客户账单地址？按交货地址？还是其他？

这些问题在我曾工作的公司中经常出现。我们都知道原因：不同的团队各自为政，使用自己偏好的技术栈来实现指标。细节隐藏在受限访问的数据库中的SQL程序里、某些本地Power BI文件中或其他地方……
