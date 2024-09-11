# Streamlit 和 MongoDB：将数据存储在云端

> 原文：[https://towardsdatascience.com/streamlit-and-mongodb-storing-your-data-in-the-cloud-c28663313ade?source=collection_archive---------8-----------------------#2023-08-25](https://towardsdatascience.com/streamlit-and-mongodb-storing-your-data-in-the-cloud-c28663313ade?source=collection_archive---------8-----------------------#2023-08-25)

## **Streamlit Cloud 没有本地存储，因此在应用程序终止时，创建的数据将会丢失——除非你使用像 MongoDB 这样的第三方存储**

[](https://medium.com/@alan-jones?source=post_page-----c28663313ade--------------------------------)[![Alan Jones](../Images/359379fab1d6685ff08080b98173e67c.png)](https://medium.com/@alan-jones?source=post_page-----c28663313ade--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c28663313ade--------------------------------)[![数据科学的前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c28663313ade--------------------------------) [Alan Jones](https://medium.com/@alan-jones?source=post_page-----c28663313ade--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7d3f5fb94faa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamlit-and-mongodb-storing-your-data-in-the-cloud-c28663313ade&user=Alan+Jones&userId=7d3f5fb94faa&source=post_page-7d3f5fb94faa----c28663313ade---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----c28663313ade--------------------------------) · 12分钟阅读 · 2023年8月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc28663313ade&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamlit-and-mongodb-storing-your-data-in-the-cloud-c28663313ade&user=Alan+Jones&userId=7d3f5fb94faa&source=-----c28663313ade---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc28663313ade&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fstreamlit-and-mongodb-storing-your-data-in-the-cloud-c28663313ade&source=-----c28663313ade---------------------bookmark_footer-----------)![](../Images/f90175fb704850839f3927b973483025.png)

一种经典的 NoSQL 数据库 — 图片由 [Jan Antonin Kolar](https://unsplash.com/@jankolar?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Streamlit 允许你将公共应用程序免费部署到他们的云端，但你本地创建的任何文件或数据库在应用程序结束时都会消失。这可能不是你希望的行为，所以我们将探索使用 MongoDB 的解决方案。

对于许多应用程序来说，丢失这些数据不是问题。例如，如果你设计了一个从外部来源读取数据的仪表板，那么你生成的任何数据可能只是临时的，仅在应用程序运行时需要。

但是，正如我在为文章开发应用时提到的，[使用 Streamlit 进行简单调查](https://medium.com/towards-data-science/simple-surveys-with-streamlit-and-databutton-d027586f1c71)，如果应用程序本身生成需要存储的数据，这就不那么简单了。在那款应用中，我将数据存储在本地文件中，但在基于云的部署中，当应用程序停止运行时，这些文件将不复存在——正确的解决方案是使用外部数据存储。

我们将探讨如何使用 MongoDB 实现这一目标，但还有其他选择。

## 有哪些选择？
