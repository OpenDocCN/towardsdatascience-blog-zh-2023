# 使用InterpretML分析纽约黄出租车行程记录

> 原文：[https://towardsdatascience.com/analysing-nyc-yellow-taxi-trip-records-with-interpretml-d4421bff32ac?source=collection_archive---------13-----------------------#2023-01-13](https://towardsdatascience.com/analysing-nyc-yellow-taxi-trip-records-with-interpretml-d4421bff32ac?source=collection_archive---------13-----------------------#2023-01-13)

## 回归分析和反事实解释

[](https://mgcodesandstats.medium.com/?source=post_page-----d4421bff32ac--------------------------------)[![Michael Grogan](../Images/af9ce19e2f61efb07664124e664c7e81.png)](https://mgcodesandstats.medium.com/?source=post_page-----d4421bff32ac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d4421bff32ac--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d4421bff32ac--------------------------------) [Michael Grogan](https://mgcodesandstats.medium.com/?source=post_page-----d4421bff32ac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Feec017a8b178&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalysing-nyc-yellow-taxi-trip-records-with-interpretml-d4421bff32ac&user=Michael+Grogan&userId=eec017a8b178&source=post_page-eec017a8b178----d4421bff32ac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d4421bff32ac--------------------------------) ·9分钟阅读·2023年1月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd4421bff32ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalysing-nyc-yellow-taxi-trip-records-with-interpretml-d4421bff32ac&user=Michael+Grogan&userId=eec017a8b178&source=-----d4421bff32ac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd4421bff32ac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanalysing-nyc-yellow-taxi-trip-records-with-interpretml-d4421bff32ac&source=-----d4421bff32ac---------------------bookmark_footer-----------)![](../Images/64c4442ade0c015dd072fde8ac9621d0.png)

来源：[StockSnap](https://pixabay.com/users/stocksnap-894430/)的照片，来自[Pixabay](https://pixabay.com/photos/coding-programming-working-macbook-924920/)

[InterpretML](https://interpret.ml/) 是一个由微软设计的**可解释机器学习**库，旨在使机器学习模型更易于理解并开放给人类解释。

当与业务利益相关者沟通发现结果时，这尤其重要，因为他们在很多情况下并非技术人员，且寻求了解机器学习模型所产生发现的业务影响。

本文的目的是说明**可解释的机器学习**和**反事实分析**如何让我们更好地理解数据集中的潜在趋势，以及**InterpretML**如何以直观的方式传达这些发现。

本例使用的数据集是**纽约市出租车及豪华车委员会——黄出租车行程记录**数据集。该数据集通过[Azure Open Data](https://learn.microsoft.com/en-us/azure/open-datasets/dataset-taxi-yellow?tabs=azureml-opendatasets)获取，该平台的数据源自[nyc.gov](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)网站，并受到nyc.gov [使用条款](https://www.nyc.gov/home/terms-of-use.page)的约束。该数据集由纽约市开放数据提供，数据在公司的[Kaggle](https://www.kaggle.com/datasets/nycopendata/new-york)账户下以CC0: Public Domain许可证公开。

请注意，进行下面分析时使用了**Python 3.8.0**。
