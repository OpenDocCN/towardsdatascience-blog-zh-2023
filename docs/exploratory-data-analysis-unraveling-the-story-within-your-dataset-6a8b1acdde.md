# 探索性数据分析：揭示数据集中的故事

> 原文：[`towardsdatascience.com/exploratory-data-analysis-unraveling-the-story-within-your-dataset-6a8b1acdde?source=collection_archive---------10-----------------------#2023-07-06`](https://towardsdatascience.com/exploratory-data-analysis-unraveling-the-story-within-your-dataset-6a8b1acdde?source=collection_archive---------10-----------------------#2023-07-06)

## 数据探索的秘密艺术——理解、清理和揭示数据集中的隐藏洞察

[](https://medium.com/@deepakchopra2911?source=post_page-----6a8b1acdde--------------------------------)![Deepak Chopra | Talking Data Science](https://medium.com/@deepakchopra2911?source=post_page-----6a8b1acdde--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6a8b1acdde--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a8b1acdde--------------------------------) [Deepak Chopra | Talking Data Science](https://medium.com/@deepakchopra2911?source=post_page-----6a8b1acdde--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb18a89417e77&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-unraveling-the-story-within-your-dataset-6a8b1acdde&user=Deepak+Chopra+%7C+Talking+Data+Science&userId=b18a89417e77&source=post_page-b18a89417e77----6a8b1acdde---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a8b1acdde--------------------------------) ·8 分钟阅读·2023 年 7 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6a8b1acdde&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-unraveling-the-story-within-your-dataset-6a8b1acdde&user=Deepak+Chopra+%7C+Talking+Data+Science&userId=b18a89417e77&source=-----6a8b1acdde---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6a8b1acdde&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploratory-data-analysis-unraveling-the-story-within-your-dataset-6a8b1acdde&source=-----6a8b1acdde---------------------bookmark_footer-----------)![](img/81db40098ec031a32117e7af7ff0583a.png)

图片由[Andrew Neel](https://unsplash.com/@andrewtneel?utm_source=medium&utm_medium=referral)提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)提供

作为一个数据爱好者，探索一个新的数据集是一项令人兴奋的工作。它让我们能够深入理解数据，并为成功分析奠定基础。对一个新的数据集有一个良好的感觉并不总是容易的，这需要时间。然而，良好而彻底的探索性数据分析（EDA）能大大帮助理解你的数据集，感知事物之间的联系，并了解需要做什么来正确处理数据集。

实际上，**你可能会把 80%的时间花在数据准备和探索上，而只有 20%的时间用于实际的数据建模**。对于其他类型的分析，探索可能占据更大比例的时间。

# **什么是探索性数据分析。**

**探索性数据分析，简单来说，就是探索数据的艺术。** 这是从不同角度调查数据的过程，以提升你的理解，探索模式，建立变量之间的关系，并在必要时提升数据本身。

就像是和你的数据集进行一次“盲目”约会，坐在桌子对面...
