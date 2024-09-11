# 在 Twitter 帖子中寻找时间模式：使用 Python 进行探索性数据分析

> 原文：[https://towardsdatascience.com/finding-temporal-patterns-in-twitter-posts-exploratory-data-analysis-with-python-8aac618c8699?source=collection_archive---------1-----------------------#2023-05-26](https://towardsdatascience.com/finding-temporal-patterns-in-twitter-posts-exploratory-data-analysis-with-python-8aac618c8699?source=collection_archive---------1-----------------------#2023-05-26)

## 使用 Python、K-Means 和 t-SNE 对 Twitter 数据进行聚类

[](https://dmitryelj.medium.com/?source=post_page-----8aac618c8699--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----8aac618c8699--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8aac618c8699--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8aac618c8699--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----8aac618c8699--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-temporal-patterns-in-twitter-posts-exploratory-data-analysis-with-python-8aac618c8699&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----8aac618c8699---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8aac618c8699--------------------------------) · 17 分钟阅读 · 2023年5月26日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8aac618c8699&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-temporal-patterns-in-twitter-posts-exploratory-data-analysis-with-python-8aac618c8699&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----8aac618c8699---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8aac618c8699&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-temporal-patterns-in-twitter-posts-exploratory-data-analysis-with-python-8aac618c8699&source=-----8aac618c8699---------------------bookmark_footer-----------)![](../Images/3732b5836870992d47e361c0c7dd91e3.png)

Tweet 聚类 t-SNE 可视化，作者提供的图片

在文章“[人们对气候的讨论](/what-people-write-about-climate-twitter-data-clustering-in-python-2fbbd2b95906)”中，我使用自然语言处理、向量化和聚类分析了Twitter帖子。通过这种技术，可以在非结构化文本数据中找到不同的群体，例如，从关于气候的成千上万条推文中提取关于冰雪融化或电动交通的消息。在处理这些数据时，出现了另一个问题：如果我们可以将相同的算法应用于消息发布的时间，而不是消息本身，会怎么样？这将使我们能够分析*何时*和*多频繁*不同的人在社交媒体上发帖。这不仅从社会学或心理学角度看很重要，而且正如我们后面会看到的，也有助于检测机器人或发送垃圾邮件的用户。最后但同样重要的是，几乎每个人现在都在使用社交平台，因此了解一些关于*我们的*新信息也很有趣。显然，相同的算法不仅可以用于Twitter帖子，也可以用于任何媒体平台。

## 方法论

我将主要使用与第一部分关于Twitter数据分析中描述的相同方法。我们的数据处理流程将包括几个…
