# 禁忌页面：美国图书禁令的数据分析

> 原文：[https://towardsdatascience.com/the-forbidden-pages-a-data-analysis-of-book-bans-in-the-us-e03a22fb0fa8?source=collection_archive---------6-----------------------#2023-03-06](https://towardsdatascience.com/the-forbidden-pages-a-data-analysis-of-book-bans-in-the-us-e03a22fb0fa8?source=collection_archive---------6-----------------------#2023-03-06)

## 关于书籍、禁令、审查和一点点人工智能

[](https://medium.com/@artfish?source=post_page-----e03a22fb0fa8--------------------------------)[![Yennie Jun](../Images/b635e965f21c3d55833269e12e861322.png)](https://medium.com/@artfish?source=post_page-----e03a22fb0fa8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e03a22fb0fa8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e03a22fb0fa8--------------------------------) [Yennie Jun](https://medium.com/@artfish?source=post_page-----e03a22fb0fa8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F12ca1ab81192&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-forbidden-pages-a-data-analysis-of-book-bans-in-the-us-e03a22fb0fa8&user=Yennie+Jun&userId=12ca1ab81192&source=post_page-12ca1ab81192----e03a22fb0fa8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e03a22fb0fa8--------------------------------) ·17 min 阅读·2023年3月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe03a22fb0fa8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-forbidden-pages-a-data-analysis-of-book-bans-in-the-us-e03a22fb0fa8&user=Yennie+Jun&userId=12ca1ab81192&source=-----e03a22fb0fa8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe03a22fb0fa8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-forbidden-pages-a-data-analysis-of-book-bans-in-the-us-e03a22fb0fa8&source=-----e03a22fb0fa8---------------------bookmark_footer-----------)![](../Images/e34e733f6515806141a573a835d66751.png)

一名女性站立着俯视从天而降的书堆。使用稳定扩散生成

*转载自我博客上的原始文章：* [*🎨 艺术鱼智能 🐡*](https://www.artfish.ai/p/the-forbidden-pages-a-data-analysis)

在过去的几年里，美国各州禁书的数量不断增加——预计这种趋势将在2023年[加速](https://www.theguardian.com/us-news/2022/dec/24/us-book-bans-streak-of-extremism)。近期在[德克萨斯州](https://www.texastribune.org/2022/09/19/texas-book-bans/)、[犹他州](https://www.kuer.org/education/2022-11-29/these-are-the-22-books-removed-from-utahs-alpine-school-district)和[佛罗里达州](https://www.buzzfeednews.com/article/juliareinstein/florida-school-book-bans-teachers-confusion)的禁书导致数百本书籍被从教室和图书馆的书架上撤下。这些被标记为“敏感材料”的书籍，大多是由或关于LGBTQ+人群和有色人种的书籍。

在这篇文章中，我分析了最近在美国被禁的书籍和作者：

+   我使用了2021-2022年被禁书籍的数据集，并结合了来自Google Books的元数据，以审视这些书籍的主要主题。我发现被禁的书籍不仅仅是关于LGBTQ+和性取向的书籍，还有关于黑人历史和女性科学家的书籍。许多被禁的书籍是近期（2000年之后）创作的，并且面向年轻读者。

+   我研究了那些创作顶级禁书的作者类型。这些作者中大多数认同为女性、非二元性别和/或有色人种。

+   我拿最常被禁的书籍并向大型语言模型提问……
