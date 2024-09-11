# 使用 TypeScript 进行空间数据工程

> 原文：[`towardsdatascience.com/spatial-data-engineering-with-typescript-fb5f59af8bb0?source=collection_archive---------9-----------------------#2023-09-05`](https://towardsdatascience.com/spatial-data-engineering-with-typescript-fb5f59af8bb0?source=collection_archive---------9-----------------------#2023-09-05)

![](img/d8b3269364c41a6de125247805d7042d.png)

图片由 [T K](https://unsplash.com/@realaxer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/9AxFJaNySB8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 建立数据管道以实现自动化空间数据科学

[](https://sutan.co.uk/?source=post_page-----fb5f59af8bb0--------------------------------)![Sutan Mufti](https://sutan.co.uk/?source=post_page-----fb5f59af8bb0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fb5f59af8bb0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb5f59af8bb0--------------------------------) [Sutan Mufti](https://sutan.co.uk/?source=post_page-----fb5f59af8bb0--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b3de0d6aa21&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspatial-data-engineering-with-typescript-fb5f59af8bb0&user=Sutan+Mufti&userId=6b3de0d6aa21&source=post_page-6b3de0d6aa21----fb5f59af8bb0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb5f59af8bb0--------------------------------) ·9 min read·2023 年 9 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffb5f59af8bb0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspatial-data-engineering-with-typescript-fb5f59af8bb0&user=Sutan+Mufti&userId=6b3de0d6aa21&source=-----fb5f59af8bb0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffb5f59af8bb0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fspatial-data-engineering-with-typescript-fb5f59af8bb0&source=-----fb5f59af8bb0---------------------bookmark_footer-----------)

# 介绍

我们可以把数据看作是水，把公司看作是城镇。正如城镇随着人口的增长而扩张，并需要更多的水来服务居民一样，公司在规模扩大时，也需要现成的数据来支持其运营。这些公司需要一个数据管道系统，类似于把水输送到城镇家庭的管道和基础设施。在我们的数据类比中，数据工程师就是那些建立和维护这些数据管道的人。对于普通的数组或表格数据，这相对简单，但涉及到空间数据时，则会复杂一些。

空间数据与普通数据略有不同，它包含空间属性。这些属性使我们能够建立空间关系，也称为[地理空间拓扑](https://en.wikipedia.org/wiki/Geospatial_topology)。即使两个表没有主键和外键，只要它们都有空间属性，我们仍然可以将它们连接起来。如果我们可视化空间属性，就会得到一张地图！
