# 从聚类到洞察；下一步

> 原文：[`towardsdatascience.com/from-clusters-to-insights-the-next-step-1c166814e0c6?source=collection_archive---------3-----------------------#2023-05-10`](https://towardsdatascience.com/from-clusters-to-insights-the-next-step-1c166814e0c6?source=collection_archive---------3-----------------------#2023-05-10)

## 了解如何定量检测哪些特征驱动了聚类的形成

[](https://erdogant.medium.com/?source=post_page-----1c166814e0c6--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----1c166814e0c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c166814e0c6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c166814e0c6--------------------------------) [埃尔多安·塔克森](https://erdogant.medium.com/?source=post_page-----1c166814e0c6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-clusters-to-insights-the-next-step-1c166814e0c6&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----1c166814e0c6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c166814e0c6--------------------------------) ·9 分钟阅读·2023 年 5 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1c166814e0c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-clusters-to-insights-the-next-step-1c166814e0c6&user=Erdogan+Taskesen&userId=4e636e2ef813&source=-----1c166814e0c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1c166814e0c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-clusters-to-insights-the-next-step-1c166814e0c6&source=-----1c166814e0c6---------------------bookmark_footer-----------)![](img/f07bdc7ebd537a94cb31545f18cd623b.png)

图片由作者提供。

聚类分析是一种用于识别具有相似模式的群体的优秀技术。然而，一旦群体形成，确定驱动这些群体的特征可能仍然具有挑战性。但这一步骤对于揭示可能被忽视的宝贵见解至关重要，这些见解可以用于决策和更深入理解数据集。确定驱动特征的一种方法是通过对特征值进行着色。虽然这种方法很有启发性，但当特征数达到数百时，它的劳动强度很大。此外，判断一组特征的确切贡献可能很困难，尤其是在不同大小和密度的群体中。***我将演示如何定量检测群体背后的驱动特征。*** *在这个博客中，* ***clusteval*** *库用于群体评估，并确定形成群体背后的驱动特征。*

# 背景

无监督聚类是一种在不使用预定义标签或类别的情况下识别数据中的自然或数据驱动群体的技术。聚类方法的挑战在于不同的方法可能会由于隐含的…
