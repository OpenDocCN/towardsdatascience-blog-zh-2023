# 节奏、努力与耐力

> 原文：[https://towardsdatascience.com/pacing-effort-and-stamina-6b340ab53650?source=collection_archive---------6-----------------------#2023-11-29](https://towardsdatascience.com/pacing-effort-and-stamina-6b340ab53650?source=collection_archive---------6-----------------------#2023-11-29)

## 对最近都柏林城市马拉松比赛的技术分析

[](https://barrysmyth.medium.com/?source=post_page-----6b340ab53650--------------------------------)[![barrysmyth](../Images/b8a047ec4b651b9a476a3d430c5723f6.png)](https://barrysmyth.medium.com/?source=post_page-----6b340ab53650--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6b340ab53650--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6b340ab53650--------------------------------) [barrysmyth](https://barrysmyth.medium.com/?source=post_page-----6b340ab53650--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa995c3b2ae8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpacing-effort-and-stamina-6b340ab53650&user=barrysmyth&userId=a995c3b2ae8&source=post_page-a995c3b2ae8----6b340ab53650---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6b340ab53650--------------------------------) · 12分钟阅读 · 2023年11月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6b340ab53650&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpacing-effort-and-stamina-6b340ab53650&user=barrysmyth&userId=a995c3b2ae8&source=-----6b340ab53650---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6b340ab53650&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpacing-effort-and-stamina-6b340ab53650&source=-----6b340ab53650---------------------bookmark_footer-----------)![](../Images/e78ef30169c3edb800efa0b924ddd5dc.png)

*在我们开始之前，我想将这篇文章定位为几篇最近文章中的一篇，这些文章位于数据科学和马拉松跑步之间的交汇点。在之前的文章中，我集中讨论了与* [*马拉松训练可视化*](/improving-the-strava-training-log-4d2039c49ec4) *和* [*表现数据*](/how-to-create-a-heat-line-plot-82f8038d1659)*相关的几个技术挑战。在这里，我将重点转向分析（我自己的）最近马拉松表现，使用一些之前讨论的可视化技术以及来自运动科学领域的几个表现指标。因此，这篇文章提供了一个我自己探索性数据科学过程的具体示例，展示了如何使用相对简单的概念如（跑步）速度和努力，来探究更复杂的当代生理测量，如恢复力和耐久性，从而更好地理解马拉松中的表现。与此同时，值得注意的是，类似的观点在许多其他耐力领域，如自行车、铁人三项、滑冰等，也证明是相关的。*

我在生活中晚些时候才开始跑步。我是在40多岁时开始的。我现在已经完成了8次都柏林城市马拉松（DCM）——从2013年开始——并且在这个过程中幸运地取得了几个新的*个人最佳成绩*（*PBs*），尽管随着年龄增长。我的最近一次努力是今年（2023年）……
