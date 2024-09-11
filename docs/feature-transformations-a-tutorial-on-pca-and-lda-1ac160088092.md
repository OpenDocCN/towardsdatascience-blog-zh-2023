# 特征转换：PCA和LDA教程

> 原文：[https://towardsdatascience.com/feature-transformations-a-tutorial-on-pca-and-lda-1ac160088092?source=collection_archive---------9-----------------------#2023-07-14](https://towardsdatascience.com/feature-transformations-a-tutorial-on-pca-and-lda-1ac160088092?source=collection_archive---------9-----------------------#2023-07-14)

## 使用诸如PCA之类的方法减少数据集的维度

[](https://medium.com/@PadraigC?source=post_page-----1ac160088092--------------------------------)[![Pádraig Cunningham](../Images/9521fb3dc64c947edf6adf2ce7b80f0f.png)](https://medium.com/@PadraigC?source=post_page-----1ac160088092--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ac160088092--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1ac160088092--------------------------------) [Pádraig Cunningham](https://medium.com/@PadraigC?source=post_page-----1ac160088092--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F52562b8f71f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-transformations-a-tutorial-on-pca-and-lda-1ac160088092&user=P%C3%A1draig+Cunningham&userId=52562b8f71f9&source=post_page-52562b8f71f9----1ac160088092---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1ac160088092--------------------------------) ·7分钟阅读·2023年7月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ac160088092&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-transformations-a-tutorial-on-pca-and-lda-1ac160088092&user=P%C3%A1draig+Cunningham&userId=52562b8f71f9&source=-----1ac160088092---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ac160088092&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffeature-transformations-a-tutorial-on-pca-and-lda-1ac160088092&source=-----1ac160088092---------------------bookmark_footer-----------)![](../Images/4bb1b05b0d0163b3ecd9ab788f291fdb.png)

图片由 [Nicole Cagnina](https://unsplash.com/@nicole_c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/projection?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供

## 介绍

处理高维数据时，通常使用诸如主成分分析（PCA）的方法来降低数据的维度。这将数据转换为一组不同（更低维度）的特征。这与特征子集选择形成对比，后者选择原始特征的一个子集（有关特征选择的教程，请参见 [[1]](https://medium.com/towards-data-science/feature-subset-selection-6de1f05822b0)）。

PCA 是将数据线性转换到更低维空间的方法。本文首先解释什么是线性变换。然后，我们通过Python示例展示PCA的工作原理。文章最后介绍了线性判别分析（LDA），一种*监督*线性变换方法。该论文中介绍的方法的Python代码可在 [GitHub](https://github.com/PadraigC/FeatTransTutorial) 上获得。

## 线性变换

假设在度假后，比尔欠玛丽£5和$15，需要以欧元（€）偿还。汇率为：£1 = €1.15 和 $1 = €0.93。因此，债务以€表示为：

在这里，我们将一个二维债务（£，$）转换为一维债务（€）。图1中展示了三个示例，包括原始债务（£5，$15）和另外两个……
