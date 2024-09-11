# 使用 SQLAlchemy 进行 UPSERT 的最简单方法

> 原文：[https://towardsdatascience.com/the-easiest-way-to-upsert-with-sqlalchemy-9dae87a75c35?source=collection_archive---------3-----------------------#2023-03-20](https://towardsdatascience.com/the-easiest-way-to-upsert-with-sqlalchemy-9dae87a75c35?source=collection_archive---------3-----------------------#2023-03-20)

## 一条命令同时`INSERT`新数据和`UPDATE`数据库中已有的记录

[](https://mikehuls.medium.com/?source=post_page-----9dae87a75c35--------------------------------)[![Mike Huls](../Images/8f9f55a0d25db00799c5d37383b7f5b6.png)](https://mikehuls.medium.com/?source=post_page-----9dae87a75c35--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9dae87a75c35--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9dae87a75c35--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----9dae87a75c35--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-easiest-way-to-upsert-with-sqlalchemy-9dae87a75c35&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----9dae87a75c35---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9dae87a75c35--------------------------------) · 5 分钟阅读 · 2023年3月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9dae87a75c35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-easiest-way-to-upsert-with-sqlalchemy-9dae87a75c35&user=Mike+Huls&userId=7ffb62c607ee&source=-----9dae87a75c35---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9dae87a75c35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-easiest-way-to-upsert-with-sqlalchemy-9dae87a75c35&source=-----9dae87a75c35---------------------bookmark_footer-----------)

在这篇简短的文章中，我们将了解如何在 SQLAlchemy 中进行`UPSERT`：我们`INSERT`新数据到数据库中，并用新提供的值`UPDATE`已经存在的记录。我们将从一个用例开始，准备我们的数据库，然后进行 UPSERT！很简单！让我们开始编码吧！

![](../Images/c6efe92cefc9eca0364df4857d41e585.png)

按行进行 UPSERT（图片来源于 [Tom Fisk](https://www.pexels.com/@tomfisk/) 在 [Pexels](https://www.pexels.com/photo/aerial-shot-of-green-milling-tractor-1595108/)）

# 什么时候我需要进行 UPSERT？

UPSERT 是将新记录`INSERT`到数据库表中，但同时`UPDATE`已经存在的数据集中的记录。数据库通过比较表的主键列来“知道哪些记录已经存在”。

重要的是要`UPDATE`现有记录，以确保我们的外键关系保持完整。在下一部分，我们将通过一个示例清楚地解释我们的实际目标。

[](/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424?source=post_page-----9dae87a75c35--------------------------------) [## 通过简单升级显著提高您的数据库插入速度

### 在 Python 中创建极速数据库连接的 4 个层级

towardsdatascience.com](/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424?source=post_page-----9dae87a75c35--------------------------------)

# 设置示例

想象一下我们经营一个吉他店的网上商店。当然，我们必须跟踪哪些吉他有库存，如何……
