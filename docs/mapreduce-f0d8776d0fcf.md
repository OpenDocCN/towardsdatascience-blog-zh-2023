# 使用 MapReduce 处理大规模数据

> 原文：[`towardsdatascience.com/mapreduce-f0d8776d0fcf?source=collection_archive---------9-----------------------#2023-07-19`](https://towardsdatascience.com/mapreduce-f0d8776d0fcf?source=collection_archive---------9-----------------------#2023-07-19)

## 对 MapReduce 和并行化的深入探讨

[](https://gmyrianthous.medium.com/?source=post_page-----f0d8776d0fcf--------------------------------)![乔治·米里安索斯](https://gmyrianthous.medium.com/?source=post_page-----f0d8776d0fcf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0d8776d0fcf--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0d8776d0fcf--------------------------------) [乔治·米里安索斯](https://gmyrianthous.medium.com/?source=post_page-----f0d8776d0fcf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmapreduce-f0d8776d0fcf&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----f0d8776d0fcf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0d8776d0fcf--------------------------------) · 4 分钟阅读 · 2023 年 7 月 19 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff0d8776d0fcf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmapreduce-f0d8776d0fcf&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----f0d8776d0fcf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff0d8776d0fcf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmapreduce-f0d8776d0fcf&source=-----f0d8776d0fcf---------------------bookmark_footer-----------)![](img/485ac39f931bd934b5c4a820f518771a.png)

图片由 [卢卡·尼科莱蒂](https://unsplash.com/@luca_nicoletti?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/fkA-hGDs-Y8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在当前的市场环境中，组织必须进行数据驱动的决策，以保持竞争力并促进创新。因此，每天都会收集大量的数据。

尽管由于云存储的普及和价格实惠，数据持久性的问题已在很大程度上得到解决，但现代组织仍需应对高效处理大量数据的挑战。

在过去的几十年里，出现了许多编程模型来应对大规模处理大数据的挑战。毫无疑问，MapReduce 是最受欢迎和有效的方法之一。

## 什么是 MapReduce

MapReduce 是一种分布式编程框架，最初由[Jeffrey Dean 和 Sanjay Ghemawat 于 2004 年在 Google 开发](https://research.google/pubs/pub62/)，并受到函数式编程基本概念的启发。他们的提议包括一个由两个步骤组成的并行数据处理模型；*map* 和 *reduce*。

简单来说，*map* 步骤涉及将原始数据分割成小块，以便对单个数据块应用转换逻辑。数据处理可以…
