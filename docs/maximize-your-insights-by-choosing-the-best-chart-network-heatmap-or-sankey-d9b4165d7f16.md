# 通过选择最佳图表：网络图、热图还是桑基图来最大化你的洞察？

> 原文：[`towardsdatascience.com/maximize-your-insights-by-choosing-the-best-chart-network-heatmap-or-sankey-d9b4165d7f16?source=collection_archive---------0-----------------------#2023-08-13`](https://towardsdatascience.com/maximize-your-insights-by-choosing-the-best-chart-network-heatmap-or-sankey-d9b4165d7f16?source=collection_archive---------0-----------------------#2023-08-13)

## 美丽的可视化图表固然很棒，但为了最大化其可解释性，你需要仔细选择图表类型。

[](https://erdogant.medium.com/?source=post_page-----d9b4165d7f16--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----d9b4165d7f16--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d9b4165d7f16--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9b4165d7f16--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----d9b4165d7f16--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaximize-your-insights-by-choosing-the-best-chart-network-heatmap-or-sankey-d9b4165d7f16&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----d9b4165d7f16---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d9b4165d7f16--------------------------------) ·9 分钟阅读·2023 年 8 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd9b4165d7f16&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaximize-your-insights-by-choosing-the-best-chart-network-heatmap-or-sankey-d9b4165d7f16&user=Erdogan+Taskesen&userId=4e636e2ef813&source=-----d9b4165d7f16---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd9b4165d7f16&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaximize-your-insights-by-choosing-the-best-chart-network-heatmap-or-sankey-d9b4165d7f16&source=-----d9b4165d7f16---------------------bookmark_footer-----------)![](img/fe52420bc5a33e68d8ba56e7ac10e1b5.png)

图片由 [David Pisnoy](https://unsplash.com/@davidpisnoy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/46juD4zY1XA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

可视化是数据分析的重要部分，因为它可以将数据转化为洞察，帮助你讲述故事。在这篇博客文章中，我将重点介绍网络图、热图和桑基图。这些图表有相同的输入，但我们应该记住，它们是为特定目标设计的，因此可解释性可能会有所不同。***我将描述网络图、热图和桑基图之间的差异、应用，并通过动手示例展示它们的可解释性。*** 所有示例均使用*D3Blocks 库*在 Python 中创建。

# ***热图和桑基图的输入。***

作为数据科学家，一个常见但重要的任务是制作图表。这些图表有时用作合理性检查，有时则会出现在演示中，形成故事的基础。尤其是在后者的情况下，我们的目标是将复杂的信息转化为逻辑的图形可视化。

> 制作图表就像摄影。你想捕捉风景……
