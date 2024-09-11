# 时间序列文本数据中的情感分析和结构性断裂

> 原文：[https://towardsdatascience.com/sentiment-analysis-and-structural-breaks-in-time-series-text-data-8109c712ca2?source=collection_archive---------15-----------------------#2023-03-06](https://towardsdatascience.com/sentiment-analysis-and-structural-breaks-in-time-series-text-data-8109c712ca2?source=collection_archive---------15-----------------------#2023-03-06)

## Arabica 现在提供结构性断裂和情感分析模块，以丰富时间序列文本挖掘

[](https://petrkorab.medium.com/?source=post_page-----8109c712ca2--------------------------------)[![Petr Korab](../Images/9f3afb4b8985584981220e30f18e3b69.png)](https://petrkorab.medium.com/?source=post_page-----8109c712ca2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8109c712ca2--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8109c712ca2--------------------------------) [Petr Korab](https://petrkorab.medium.com/?source=post_page-----8109c712ca2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F13a053cbaad9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsentiment-analysis-and-structural-breaks-in-time-series-text-data-8109c712ca2&user=Petr+Korab&userId=13a053cbaad9&source=post_page-13a053cbaad9----8109c712ca2---------------------post_header-----------) 发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----8109c712ca2--------------------------------) ·7分钟阅读·2023年3月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8109c712ca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsentiment-analysis-and-structural-breaks-in-time-series-text-data-8109c712ca2&user=Petr+Korab&userId=13a053cbaad9&source=-----8109c712ca2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8109c712ca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsentiment-analysis-and-structural-breaks-in-time-series-text-data-8109c712ca2&source=-----8109c712ca2---------------------bookmark_footer-----------)![](../Images/8f5897e7bff2d0dad77264730ceec6d2.png)

图片来自 [Adam Śmigielski](https://unsplash.com/@smigielski?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

文本数据包含大量定性信息，可以通过各种方法量化，包括情感分析技术。这些模型用于识别、提取和量化文本数据中的情感，并在商业和学术研究中得到广泛应用。由于文本通常是按时间序列记录的，文本数据集可能会显示结构性断裂，因为定量信息由于许多可能的因素而发生变化。

作为商业分析师，衡量客户对特定品牌的感知变化可能是关键任务之一。在研究角色中，可能对弗拉基米尔·普京公共声明随时间的变化感兴趣。[**Arabica**](https://pypi.org/project/arabica/)是一个专门设计用于处理类似问题的Python库。它包含这些用于时间序列文本数据集的探索性分析的方法：

+   **arabica_freq** 用于描述性的基于n-gram的探索性分析（EDA）。

+   **cappuccino** 是一个可视化模块，包括 ***热力图***、***词云*** 和 ***折线图***，用于单字、双字和三字频率的分析。
