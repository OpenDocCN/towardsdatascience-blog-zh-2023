# 一份全面的企业级 Plotly Dash 应用构建指南

> 原文：[`towardsdatascience.com/a-comprehensive-guide-to-building-enterprise-level-plotly-dash-apps-bd40dfe1313c?source=collection_archive---------3-----------------------#2023-05-15`](https://towardsdatascience.com/a-comprehensive-guide-to-building-enterprise-level-plotly-dash-apps-bd40dfe1313c?source=collection_archive---------3-----------------------#2023-05-15)

## 使用纯 Python 和 Docker 构建生产级 Web 应用

[](https://tinztwinspro.medium.com/?source=post_page-----bd40dfe1313c--------------------------------)![Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----bd40dfe1313c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bd40dfe1313c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd40dfe1313c--------------------------------) [Janik 和 Patrick Tinz](https://tinztwinspro.medium.com/?source=post_page-----bd40dfe1313c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4eb5d9652d9e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-to-building-enterprise-level-plotly-dash-apps-bd40dfe1313c&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=post_page-4eb5d9652d9e----bd40dfe1313c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd40dfe1313c--------------------------------) ·10 分钟阅读·2023 年 5 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbd40dfe1313c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-to-building-enterprise-level-plotly-dash-apps-bd40dfe1313c&user=Janik+and+Patrick+Tinz&userId=4eb5d9652d9e&source=-----bd40dfe1313c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbd40dfe1313c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-comprehensive-guide-to-building-enterprise-level-plotly-dash-apps-bd40dfe1313c&source=-----bd40dfe1313c---------------------bookmark_footer-----------)![](img/7de17ef1cc6ad4e71a3adb156a7deaa1.png)

照片由 [Scott Graham](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据科学项目总是需要某种可视化。在初步分析阶段，数据科学家通常使用 Jupyter Notebook 以及像 matplotlib 或 seaborn 这样的库。在探索性数据分析中，数据科学家使用直方图、散点图或进行统计评估。对于初步见解，这种方法非常适合。然而，互动式仪表盘更适合展示结果。许多客户正是需要这种！互动式仪表盘是一种经过验证的方法，用于以易于理解和全面的方式解释结果。

但创建一个互动式仪表盘并非易事。在我们看来，Plotly Dash 是创建令人印象深刻的图表的最佳选择。对于一个生产就绪的仪表盘应用程序，你必须考虑更多方面（例如，通过 Docker 进行部署）。

在本文中，我们希望分享使用 Plotly Dash 构建结构良好的仪表盘应用程序的最佳实践。此外，我们展示了如何通过 Docker 干净地部署一个 Dash 应用程序。我们始终欢迎改进建议，请在评论中写下你的想法！

# 为什么选择 Plotly Dash？
