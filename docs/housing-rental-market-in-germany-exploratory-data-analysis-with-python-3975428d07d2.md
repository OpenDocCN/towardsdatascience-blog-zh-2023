# 德国住房租赁市场：使用 Python 进行探索性数据分析

> 原文：[https://towardsdatascience.com/housing-rental-market-in-germany-exploratory-data-analysis-with-python-3975428d07d2?source=collection_archive---------7-----------------------#2023-04-04](https://towardsdatascience.com/housing-rental-market-in-germany-exploratory-data-analysis-with-python-3975428d07d2?source=collection_archive---------7-----------------------#2023-04-04)

## 使用 Python、Pandas 和 Bokeh 获取统计数据洞察

[](https://dmitryelj.medium.com/?source=post_page-----3975428d07d2--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----3975428d07d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3975428d07d2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3975428d07d2--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----3975428d07d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhousing-rental-market-in-germany-exploratory-data-analysis-with-python-3975428d07d2&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----3975428d07d2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3975428d07d2--------------------------------) ·27 min 阅读·2023年4月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3975428d07d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhousing-rental-market-in-germany-exploratory-data-analysis-with-python-3975428d07d2&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----3975428d07d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3975428d07d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhousing-rental-market-in-germany-exploratory-data-analysis-with-python-3975428d07d2&source=-----3975428d07d2---------------------bookmark_footer-----------)![](../Images/583688b266b6d654c7bc5738f3ec33b9.png)

Salzbrücke, 德国，图片来源 [https://en.wikipedia.org/wiki/German_Timber-Frame_Road](https://en.wikipedia.org/wiki/German_Timber-Frame_Road)

德国不仅是欧洲最大的经济体，而且还是一个拥有美丽风景和有趣文化的国家。德国自然成为了来自世界各地的游客和外籍人士的热门目的地。对德国住房租赁市场进行探索性数据分析不仅对数据分析师有趣，对那些计划在德国生活和工作的人也非常有意义。我将展示一些可以通过 Python、Pandas 和 Bokeh 发现的有趣趋势。

让我们开始吧。

# 数据收集

为了寻找数据，我决定使用 ImmoScout24，这不仅是最大的（截至本文撰写时，约有 72K 套公寓和房屋在上面列出），而且还是这类网站中最古老的。根据 [https://web.archive.org](https://web.archive.org/)，其第一个版本是在 1999 年制作的，距今已有 20 多年。ImmoScout24 还提供了一个 API 和开发者页面。我联系了公关部门，他们允许我使用网站数据进行此次出版，但未能提供 API 密钥。这个 API 可能仅供合作伙伴使用，用于添加或编辑住房数据，而不是用于批量读取…
