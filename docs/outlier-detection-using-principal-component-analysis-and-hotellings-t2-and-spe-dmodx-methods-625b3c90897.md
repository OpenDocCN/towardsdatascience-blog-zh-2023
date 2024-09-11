# 使用主成分分析和 Hotelling 的 T2 及 SPE/DmodX 方法进行异常值检测

> 原文：[`towardsdatascience.com/outlier-detection-using-principal-component-analysis-and-hotellings-t2-and-spe-dmodx-methods-625b3c90897?source=collection_archive---------0-----------------------#2023-03-11`](https://towardsdatascience.com/outlier-detection-using-principal-component-analysis-and-hotellings-t2-and-spe-dmodx-methods-625b3c90897?source=collection_archive---------0-----------------------#2023-03-11)

## 由于主成分分析（PCA）的灵敏性，它可以用来检测多变量数据集中的异常值。

[](https://erdogant.medium.com/?source=post_page-----625b3c90897--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----625b3c90897--------------------------------)[](https://towardsdatascience.com/?source=post_page-----625b3c90897--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----625b3c90897--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----625b3c90897--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e636e2ef813&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-using-principal-component-analysis-and-hotellings-t2-and-spe-dmodx-methods-625b3c90897&user=Erdogan+Taskesen&userId=4e636e2ef813&source=post_page-4e636e2ef813----625b3c90897---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----625b3c90897--------------------------------) · 11 分钟阅读 · 2023 年 3 月 11 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F625b3c90897&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-using-principal-component-analysis-and-hotellings-t2-and-spe-dmodx-methods-625b3c90897&user=Erdogan+Taskesen&userId=4e636e2ef813&source=-----625b3c90897---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F625b3c90897&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foutlier-detection-using-principal-component-analysis-and-hotellings-t2-and-spe-dmodx-methods-625b3c90897&source=-----625b3c90897---------------------bookmark_footer-----------)![](img/c6d1216a283cf22c5bca9061dca4820b.png)

图片由[Andrew Ridley](https://unsplash.com/@aridley88?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/jR4Zf-riEjI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

主成分分析（PCA）是一种广泛使用的降维技术，同时保留相关信息。由于其敏感性，它也可以用来检测多变量数据集中的离群值。离群值检测可以提供异常情况的早期预警信号，允许专家在问题升级之前识别和解决问题。然而，由于高维度和缺乏标签，在多变量数据集中检测离群值可能会很具挑战性。PCA 在离群值检测中具有几个优点。***我将使用 PCA 描述离群值检测的概念。通过一个实际的例子，我将演示如何为连续和单独的分类数据集创建一个无监督模型来检测离群值。***

## 离群值检测。

离群值可以通过***单变量***或***多变量***方法进行建模（见图 1）。在单变量方法中，离群值是通过一次使用一个变量来检测的，其中数据分布分析是一种很好的方式。有关单变量离群值检测的更多细节，请参阅以下博客文章 [1]：
