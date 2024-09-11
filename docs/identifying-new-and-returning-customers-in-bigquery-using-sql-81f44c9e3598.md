# 使用 SQL 在 BigQuery 中识别新客户和回头客

> 原文：[https://towardsdatascience.com/identifying-new-and-returning-customers-in-bigquery-using-sql-81f44c9e3598?source=collection_archive---------11-----------------------#2023-01-04](https://towardsdatascience.com/identifying-new-and-returning-customers-in-bigquery-using-sql-81f44c9e3598?source=collection_archive---------11-----------------------#2023-01-04)

## 为了更好地理解客户的兴趣和行为，以及改善您的营销策略

[](https://romaingranger.medium.com/?source=post_page-----81f44c9e3598--------------------------------)[![Romain Granger](../Images/ef3e5fa4f2e35f822830ed803df2cef2.png)](https://romaingranger.medium.com/?source=post_page-----81f44c9e3598--------------------------------)[](https://towardsdatascience.com/?source=post_page-----81f44c9e3598--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----81f44c9e3598--------------------------------) [Romain Granger](https://romaingranger.medium.com/?source=post_page-----81f44c9e3598--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F36c0056b1ee7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-new-and-returning-customers-in-bigquery-using-sql-81f44c9e3598&user=Romain+Granger&userId=36c0056b1ee7&source=post_page-36c0056b1ee7----81f44c9e3598---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----81f44c9e3598--------------------------------) ·8 分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F81f44c9e3598&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-new-and-returning-customers-in-bigquery-using-sql-81f44c9e3598&user=Romain+Granger&userId=36c0056b1ee7&source=-----81f44c9e3598---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F81f44c9e3598&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fidentifying-new-and-returning-customers-in-bigquery-using-sql-81f44c9e3598&source=-----81f44c9e3598---------------------bookmark_footer-----------)![](../Images/91184e9b57fb6e50c3e8cec94a069d4e.png)

照片由 [Vincent van Zalinge](https://unsplash.com/@vincentvanzalinge?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 主要好处

对客户进行分类，无论是新客户还是回头客，都可以帮助确定使用哪种营销或销售策略。毫无疑问，这将取决于您业务的性质，您是否优先考虑获取、留存或两者兼顾。

在深入探讨 SQL 方法和步骤之前，这里有一些术语的简单定义和业务示例：

+   **新客户：** 指首次购买的顾客

+   **回头客：** 指已经进行过多次购买的顾客

举例来说，**一家汽车公司**和**一家咖啡店**公司：

**对于一家汽车公司**，回头客的数量可能较低，因为汽车购买通常不频繁，主要是由于价格高昂，顾客可能每几年才购买一次。这种情况下，策略可能是专注于获取新客户并拓展市场。

**相比之下，在线咖啡店**销售的是可消耗的产品，例如咖啡豆或磨碎咖啡，这些产品通常需要定期购买。价格点更为实惠，这使得……
