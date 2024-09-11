# 3 种 SQL 优化技巧，瞬间提升查询速度

> 原文：[https://towardsdatascience.com/3-important-sql-optimization-technique-d6da3e9c8442?source=collection_archive---------2-----------------------#2023-04-05](https://towardsdatascience.com/3-important-sql-optimization-technique-d6da3e9c8442?source=collection_archive---------2-----------------------#2023-04-05)

## 在完全切换到其他数据模型之前可以尝试的简单技巧

[](https://thuwarakesh.medium.com/?source=post_page-----d6da3e9c8442--------------------------------)[![Thuwarakesh Murallie](../Images/44f1a14a899426592bbd8c7f73ce169d.png)](https://thuwarakesh.medium.com/?source=post_page-----d6da3e9c8442--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d6da3e9c8442--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d6da3e9c8442--------------------------------) [Thuwarakesh Murallie](https://thuwarakesh.medium.com/?source=post_page-----d6da3e9c8442--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F93ce19993bef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-important-sql-optimization-technique-d6da3e9c8442&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=post_page-93ce19993bef----d6da3e9c8442---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d6da3e9c8442--------------------------------) ·6分钟阅读·2023年4月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd6da3e9c8442&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-important-sql-optimization-technique-d6da3e9c8442&user=Thuwarakesh+Murallie&userId=93ce19993bef&source=-----d6da3e9c8442---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd6da3e9c8442&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-important-sql-optimization-technique-d6da3e9c8442&source=-----d6da3e9c8442---------------------bookmark_footer-----------)![](../Images/ddceac6c6d256afbc01f49702c75cac0.png)

图片由 [Heramb kamble](https://unsplash.com/@heramb_2k?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/PfGXoM4ZPCc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

许多数据库应运而生，以解决特定的软件开发和数据科学需求。然而，关系型数据库仍然是许多人最常使用和首选的数据库。

查询速度是人们考虑使用不同数据模型的最常见原因。基于文档、列族和图形数据库主要用于解决速度问题。 

但对于大多数使用场景，关系型方法已经足够了。此外，我们还可以对其进行速度优化，同时享受 SQL 提供的所有好处。

我不排斥改变。然而，在任何项目中，保持技术栈尽可能简洁往往是我的主要目标。这样一来，SQL 是大多数工程师都能理解的——即使是新手也能轻松上手！

[](/fast-load-data-to-sql-from-python-2d67aea946c0?source=post_page-----d6da3e9c8442--------------------------------) [## Python 到 SQL — 现在我可以快20倍地加载数据

### 上传大量数据的好方法、坏方法和丑陋方法

towardsdatascience.com](/fast-load-data-to-sql-from-python-2d67aea946c0?source=post_page-----d6da3e9c8442--------------------------------)

因此，切换到不同的数据模型是我最后考虑的事情。相反，我会首先做这些调整，以充分利用 SQL 的关系型数据库。
