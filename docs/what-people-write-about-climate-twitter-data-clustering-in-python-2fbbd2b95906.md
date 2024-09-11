# 关于气候的讨论：Python 中的 Twitter 数据聚类

> 原文：[`towardsdatascience.com/what-people-write-about-climate-twitter-data-clustering-in-python-2fbbd2b95906?source=collection_archive---------2-----------------------#2023-05-15`](https://towardsdatascience.com/what-people-write-about-climate-twitter-data-clustering-in-python-2fbbd2b95906?source=collection_archive---------2-----------------------#2023-05-15)

## 使用 K-Means、TF-IDF、Word2Vec 和 Sentence-BERT 进行 Twitter 数据的聚类

[](https://dmitryelj.medium.com/?source=post_page-----2fbbd2b95906--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----2fbbd2b95906--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2fbbd2b95906--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fbbd2b95906--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----2fbbd2b95906--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-people-write-about-climate-twitter-data-clustering-in-python-2fbbd2b95906&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----2fbbd2b95906---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2fbbd2b95906--------------------------------) ·21 分钟阅读·2023 年 5 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2fbbd2b95906&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-people-write-about-climate-twitter-data-clustering-in-python-2fbbd2b95906&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----2fbbd2b95906---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2fbbd2b95906&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-people-write-about-climate-twitter-data-clustering-in-python-2fbbd2b95906&source=-----2fbbd2b95906---------------------bookmark_footer-----------)![](img/b562105ac69264137465079315ba7c15.png)

推文集群可视化，图片由作者提供

人们对气候、疫情、战争或任何其他热点问题的看法和写作是什么样的？从社会学的角度来看，这类问题很有趣；了解人们观点的当前趋势对科学家、记者或政治家也可能很有价值。但是我们怎么能获得这些答案呢？收集数百万人的回应在过去可能是一个昂贵的过程，但今天我们可以从社交网络帖子中获得这些答案。如今有许多社交平台；我基于几个原因选择了 Twitter 进行分析：

+   Twitter 最初设计用于发布简短的帖子，这可能更容易进行分析。至少我希望尽管有文本大小限制，人们在分享想法时会尽量简洁。

+   Twitter 是一个大型社交网络；它成立近 20 年。到写这篇文章时，它大约有 4.5 亿活跃用户，因此很容易获得大量数据进行分析。

+   Twitter 有一个官方 API，其许可允许我们将数据用于研究目的。
