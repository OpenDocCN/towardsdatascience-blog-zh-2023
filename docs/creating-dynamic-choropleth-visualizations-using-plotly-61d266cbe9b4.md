# 使用 Plotly 创建动态的分级统计图可视化

> 原文：[`towardsdatascience.com/creating-dynamic-choropleth-visualizations-using-plotly-61d266cbe9b4?source=collection_archive---------7-----------------------#2023-12-18`](https://towardsdatascience.com/creating-dynamic-choropleth-visualizations-using-plotly-61d266cbe9b4?source=collection_archive---------7-----------------------#2023-12-18)

## 使用易于学习的工具包创建复杂的可视化

[](https://hd2zm.medium.com/?source=post_page-----61d266cbe9b4--------------------------------)![Hari Devanathan](https://hd2zm.medium.com/?source=post_page-----61d266cbe9b4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----61d266cbe9b4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----61d266cbe9b4--------------------------------) [Hari Devanathan](https://hd2zm.medium.com/?source=post_page-----61d266cbe9b4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe157cea547ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-dynamic-choropleth-visualizations-using-plotly-61d266cbe9b4&user=Hari+Devanathan&userId=e157cea547ee&source=post_page-e157cea547ee----61d266cbe9b4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----61d266cbe9b4--------------------------------) · 12 min 阅读 · 2023 年 12 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F61d266cbe9b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-dynamic-choropleth-visualizations-using-plotly-61d266cbe9b4&user=Hari+Devanathan&userId=e157cea547ee&source=-----61d266cbe9b4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F61d266cbe9b4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-dynamic-choropleth-visualizations-using-plotly-61d266cbe9b4&source=-----61d266cbe9b4---------------------bookmark_footer-----------)![](img/17c9e259f0a21a20f9cdd5fc479fc7db.png)

照片由 [NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据可视化是一个常被数据科学家忽视的步骤。它通过分析和整理数据，将其转化为易于理解的形式，帮助我们讲述故事。通过去除所有技术细节和噪声，突出关键信息，数据科学家可以向非技术经理和高层解释他们工作的意义。

有许多工具可以帮助可视化数据。多年来，微软 Excel 主导了静态可视化市场。随着时间的推移，我们转向了动态可视化和灵活性，以更清晰的方式展示更多数据。两种类型的工具有助于创建动态视觉效果。

+   **商业智能和分析软件：** Tableau, PowerBI

+   **开源编程库：** D3.js, Plotly Dash

像 Tableau 和 PowerBI 这样的第三方软件工具对于非技术人员来说非常出色。拖放界面和抽象功能使分析师能够轻松创建动态仪表板。缺点是

+   软件工具价格昂贵

+   学习这些工具有一定的学习曲线

+   可视化设计的限制；软件可能不允许某些组件
