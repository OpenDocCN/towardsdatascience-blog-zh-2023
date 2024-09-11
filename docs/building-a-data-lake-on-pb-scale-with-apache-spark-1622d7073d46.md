# 使用 Apache Spark 构建 PB 规模的数据湖

> 原文：[https://towardsdatascience.com/building-a-data-lake-on-pb-scale-with-apache-spark-1622d7073d46?source=collection_archive---------1-----------------------#2023-01-26](https://towardsdatascience.com/building-a-data-lake-on-pb-scale-with-apache-spark-1622d7073d46?source=collection_archive---------1-----------------------#2023-01-26)

## 我们在 Emplifi 处理大数据的方式

[](https://medium.com/@vrba.dave?source=post_page-----1622d7073d46--------------------------------)[![David Vrba](../Images/7689bf42fcfc0c029de87450d01069ba.png)](https://medium.com/@vrba.dave?source=post_page-----1622d7073d46--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1622d7073d46--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1622d7073d46--------------------------------) [David Vrba](https://medium.com/@vrba.dave?source=post_page-----1622d7073d46--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb7f216c64e33&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-data-lake-on-pb-scale-with-apache-spark-1622d7073d46&user=David+Vrba&userId=b7f216c64e33&source=post_page-b7f216c64e33----1622d7073d46---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1622d7073d46--------------------------------) ·15 分钟阅读·2023年1月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1622d7073d46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-data-lake-on-pb-scale-with-apache-spark-1622d7073d46&user=David+Vrba&userId=b7f216c64e33&source=-----1622d7073d46---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1622d7073d46&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-data-lake-on-pb-scale-with-apache-spark-1622d7073d46&source=-----1622d7073d46---------------------bookmark_footer-----------)![](../Images/ec428d788058b2d1569bf4768b4938db.png)

图片来源：[Victor Hanacek](https://picjumbo.com/author/viktorhanacek/) 于 [picjumbo](https://picjumbo.com/big-data/)

从职业角度来看，我在 Emplifi（前身为 Socialbakers）公司的数据工程团队工作了四年，其中我参与的最大项目之一是构建一个分布式数据存储系统，该系统目前存储了近一个 PB 的数据，目标是为数据分析师和研究人员提供他们可以高效分析和研究的数据表。正如你可能想象的那样——构建和维护这样一个数据湖并非易事，因为数据不仅经常变化，而且其模式随着时间的推移而演变，拥有数十个甚至数百个具有不同嵌套级别的字段。

在本文中，我想分享我在这个过程中的经验和亮点，主要集中在技术层面。

我们每天处理的数据来源于社交网络，如 Facebook、Twitter、Instagram、YouTube、LinkedIn 或 TikTok。处理的数据集主要是这些网络上发布帖子的公共个人资料。一小部分数据也来自内部系统。我们的存储系统建立在 S3 AWS 上，我们称之为数据湖，因为我们在这里以原始格式以及预处理、处理和聚合的形式存储数据。原始数据主要是...
