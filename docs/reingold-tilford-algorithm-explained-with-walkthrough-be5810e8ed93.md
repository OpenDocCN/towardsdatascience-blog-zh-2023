# Reingold-Tilford 算法的解释及演练

> 原文：[`towardsdatascience.com/reingold-tilford-algorithm-explained-with-walkthrough-be5810e8ed93?source=collection_archive---------6-----------------------#2023-09-12`](https://towardsdatascience.com/reingold-tilford-algorithm-explained-with-walkthrough-be5810e8ed93?source=collection_archive---------6-----------------------#2023-09-12)

## 绘制树节点的算法及数值示例和 Python 代码

[](https://kayjanwong.medium.com/?source=post_page-----be5810e8ed93--------------------------------)![Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----be5810e8ed93--------------------------------)[](https://towardsdatascience.com/?source=post_page-----be5810e8ed93--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----be5810e8ed93--------------------------------) [Kay Jan Wong](https://kayjanwong.medium.com/?source=post_page-----be5810e8ed93--------------------------------)

·

[发布于](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffee8693930fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freingold-tilford-algorithm-explained-with-walkthrough-be5810e8ed93&user=Kay+Jan+Wong&userId=fee8693930fb&source=post_page-fee8693930fb----be5810e8ed93---------------------post_header-----------) [Towards Data Science](https://towardsdatascience.com/?source=post_page-----be5810e8ed93--------------------------------) ·9 分钟阅读·2023 年 9 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbe5810e8ed93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freingold-tilford-algorithm-explained-with-walkthrough-be5810e8ed93&user=Kay+Jan+Wong&userId=fee8693930fb&source=-----be5810e8ed93---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbe5810e8ed93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freingold-tilford-algorithm-explained-with-walkthrough-be5810e8ed93&source=-----be5810e8ed93---------------------bookmark_footer-----------)![](img/4962af380309f59d5a1af978e2bb8265.png)

图片由[Sergiu Vălenaș](https://unsplash.com/@svalenas?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

1981 年的 Reingold-Tilford 算法通过将节点排列在树结构中，以最大化可读性，从而创建了一个视觉上令人愉悦的层次数据表示。换句话说，它是一个用于检索树中每个节点的`(x, y)`坐标的算法。

根据[论文](https://reingold.co/tidier-drawings.pdf)，一个好的树图应该遵循一些美学规则，

> 1\. 同一深度的节点应沿直线排列，定义深度的直线应平行。
> 
> 2\. 左子节点应位于其父节点的左侧，右子节点应位于其父节点的右侧（仅适用于二叉树）。
> 
> 3\. 父节点应居中于其子节点之上。
> 
> 4\. 树及其镜像应生成互为倒影的图形，并且子树无论出现在树的任何位置，其绘制方式应相同。

确定节点的`y`坐标是直接的，而`x`坐标稍微复杂一些。本文将尝试通过稍微更多的数值示例来解释算法...
