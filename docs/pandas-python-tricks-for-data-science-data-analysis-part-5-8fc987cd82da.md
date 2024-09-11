# Pandas 和 Python 数据科学与数据分析技巧 — 第五部分

> 原文：[https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-5-8fc987cd82da?source=collection_archive---------10-----------------------#2023-04-10](https://towardsdatascience.com/pandas-python-tricks-for-data-science-data-analysis-part-5-8fc987cd82da?source=collection_archive---------10-----------------------#2023-04-10)

## 这是我的《Pandas & Python Tricks》系列的第五部分

[](https://zoumanakeita.medium.com/?source=post_page-----8fc987cd82da--------------------------------)[![Zoumana Keita](../Images/34a15c1d03687816dbdbc065f5719f80.png)](https://zoumanakeita.medium.com/?source=post_page-----8fc987cd82da--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8fc987cd82da--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8fc987cd82da--------------------------------) [Zoumana Keita](https://zoumanakeita.medium.com/?source=post_page-----8fc987cd82da--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe6ae785a30d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-5-8fc987cd82da&user=Zoumana+Keita&userId=e6ae785a30d&source=post_page-e6ae785a30d----8fc987cd82da---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8fc987cd82da--------------------------------) · 6 分钟阅读 · 2023年4月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8fc987cd82da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-5-8fc987cd82da&user=Zoumana+Keita&userId=e6ae785a30d&source=-----8fc987cd82da---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8fc987cd82da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpandas-python-tricks-for-data-science-data-analysis-part-5-8fc987cd82da&source=-----8fc987cd82da---------------------bookmark_footer-----------)![](../Images/2548e37f080486bf1d170b4a3216db00.png)

图片来源：[Andrew Neel](https://unsplash.com/@andrewtneel) 于 [Unsplash](https://unsplash.com/photos/cckf4TsHAuw)

# 介绍

几天前，我分享了[一些 Python 和 Pandas 的技巧](https://medium.com/towards-data-science/pandas-python-tricks-for-data-science-data-analysis-part-3-462d0e952925)，帮助数据分析师和数据科学家快速学习他们可能还不知道的新有价值的概念。这也是我每天在[LinkedIn](https://www.linkedin.com/in/zoumana-keita/)上分享的技巧系列的一部分。

# Pandas

## 结合 SQL 语句与 Pandas

我的直觉告诉我，超过 80% 的数据科学家在他们的日常数据科学活动中使用 Pandas。

我相信这是因为它作为 Python 宇宙更广泛范围的一部分所带来的好处，使得它对许多人来说都很容易接触。

𝙎𝙌𝙇 呢？

尽管不是每个人在日常生活中使用它（因为并非每个公司都必需有一个 SQL 数据库？），SQL 的性能是不可否认的。它也是人类可读的，这使得它容易理解……
