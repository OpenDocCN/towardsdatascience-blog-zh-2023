# 如何在 Python 中创建时间序列网络图可视化

> 原文：[https://towardsdatascience.com/how-to-create-a-time-series-network-graph-visualization-in-python-8da0227fa4b8?source=collection_archive---------4-----------------------#2023-10-26](https://towardsdatascience.com/how-to-create-a-time-series-network-graph-visualization-in-python-8da0227fa4b8?source=collection_archive---------4-----------------------#2023-10-26)

## 使用 Plotly 和 NetworkX 显示网络如何随时间演变

[](https://ds-claudia.medium.com/?source=post_page-----8da0227fa4b8--------------------------------)[![Claudia Ng](../Images/e3f42d73e4a459a4a276aaa293fa6861.png)](https://ds-claudia.medium.com/?source=post_page-----8da0227fa4b8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8da0227fa4b8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8da0227fa4b8--------------------------------) [Claudia Ng](https://ds-claudia.medium.com/?source=post_page-----8da0227fa4b8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fba2da7b3b9c8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-time-series-network-graph-visualization-in-python-8da0227fa4b8&user=Claudia+Ng&userId=ba2da7b3b9c8&source=post_page-ba2da7b3b9c8----8da0227fa4b8---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8da0227fa4b8--------------------------------) · 9 分钟阅读 · 2023年10月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8da0227fa4b8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-time-series-network-graph-visualization-in-python-8da0227fa4b8&user=Claudia+Ng&userId=ba2da7b3b9c8&source=-----8da0227fa4b8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8da0227fa4b8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-a-time-series-network-graph-visualization-in-python-8da0227fa4b8&source=-----8da0227fa4b8---------------------bookmark_footer-----------)![](../Images/32d47dc42370ba8c6be98f368dde3c7a.png)

可视化与选定医生的连接随时间变化

在这篇文章中，你将学习如何在 Python 中创建时间序列网络可视化，展示网络中的连接如何随时间发展，如上方动画所示。网络数据非常有效地揭示了连接，而时间序列数据则有助于发现基础数据中的趋势和异常。

在这篇文章中，我们将使用Kaggle上的一个[数据集](https://www.kaggle.com/datasets/rohitrox/healthcare-provider-fraud-detection-analysis/data?select=Train_Beneficiarydata-1542865627584.csv)创建一个示例，该数据集涉及医疗提供者欺诈。（此数据集目前在Kaggle上授权为CC0：公有领域。请注意，此数据集可能不准确，仅用于本文的演示目的）。

我们将结合提供链接中的数据，获取与指定主治医生相关的欺诈索赔群体，并根据索赔开始日期绘制该医生与其他实体之间的连接（最多跳跃一定数量的距离）。

确保在你的Python虚拟环境中安装了[Plotly](https://plotly.com/python/getting-started/)和[NetworkX](https://networkx.org/documentation/stable/install.html)。

如果你想学习一种有效的方法来可视化网络如何随时间增长，请继续阅读！

# 数据集的发现
