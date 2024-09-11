# 处理 Python 数据框中的日期 第1部分 — 日期系列创建

> 原文：[https://towardsdatascience.com/dealing-with-dates-in-pythons-dataframe-part-1-date-series-creation-f4a800db9ae?source=collection_archive---------17-----------------------#2023-01-04](https://towardsdatascience.com/dealing-with-dates-in-pythons-dataframe-part-1-date-series-creation-f4a800db9ae?source=collection_archive---------17-----------------------#2023-01-04)

## Python 数据处理

## Pandas 日期系列创建方法

[](https://kahemchu.medium.com/?source=post_page-----f4a800db9ae--------------------------------)[![KahEm Chu](../Images/2f89d02e85f61f08f048773990f4d53f.png)](https://kahemchu.medium.com/?source=post_page-----f4a800db9ae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f4a800db9ae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f4a800db9ae--------------------------------) [KahEm Chu](https://kahemchu.medium.com/?source=post_page-----f4a800db9ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc8007b91ef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-dates-in-pythons-dataframe-part-1-date-series-creation-f4a800db9ae&user=KahEm+Chu&userId=7cc8007b91ef&source=post_page-7cc8007b91ef----f4a800db9ae---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f4a800db9ae--------------------------------) ·10分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff4a800db9ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-dates-in-pythons-dataframe-part-1-date-series-creation-f4a800db9ae&user=KahEm+Chu&userId=7cc8007b91ef&source=-----f4a800db9ae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff4a800db9ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdealing-with-dates-in-pythons-dataframe-part-1-date-series-creation-f4a800db9ae&source=-----f4a800db9ae---------------------bookmark_footer-----------)![](../Images/d3349995772e17d6c1e9e1b34526cc5b.png)

照片由[Jon Tyson](https://unsplash.com/@jontyson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/s/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

大多数时候，DateTime 对象是从数据中提取洞察的关键元素。我们可以通过日期了解数据的趋势、周期和季节性模式。基于发现的模式，我们可以准备报告，并进一步研究和分析数据。

DateTime对象在分析中的重要性促使我进一步研究如何在`pandas`模块中使用DateTime对象。然后，我记录了我经常使用的方法和属性，以及那些我觉得将来可能会用到的。除此之外，我根据我的理解将它们分成了几个部分。总共创建了以下2个部分：

![](../Images/31bdd968f0779f2b31bcfdec88c1c29a.png)

图片来自作者。

为了获得更好的阅读体验，我决定将该组分成2篇文章。这是第一篇文章，你可以在[这里找到第二篇文章](/dealing-with-dates-in-pythons-dataframe-part-2-the-basics-9ad5edacd2f8)。

让我们从第一部分开始，*处理DateTime序列的基础知识*。

## 第一部分 — 处理DateTime序列的基础知识
