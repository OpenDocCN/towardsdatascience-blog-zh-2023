# 了解 SQL 注入并学习如何在 Python 中使用 SQLAlchemy 避免它

> 原文：[https://towardsdatascience.com/understand-sql-injection-and-learn-to-avoid-it-in-python-with-sqlalchemy-2c0ba57733b2?source=collection_archive---------4-----------------------#2023-04-12](https://towardsdatascience.com/understand-sql-injection-and-learn-to-avoid-it-in-python-with-sqlalchemy-2c0ba57733b2?source=collection_archive---------4-----------------------#2023-04-12)

## 学习如何安全地与数据库交互

[](https://lynn-kwong.medium.com/?source=post_page-----2c0ba57733b2--------------------------------)[![Lynn G. Kwong](../Images/b9a05b6587db5ca41c1d8264adda5b06.png)](https://lynn-kwong.medium.com/?source=post_page-----2c0ba57733b2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2c0ba57733b2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2c0ba57733b2--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----2c0ba57733b2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff649eccbbc3d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstand-sql-injection-and-learn-to-avoid-it-in-python-with-sqlalchemy-2c0ba57733b2&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=post_page-f649eccbbc3d----2c0ba57733b2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2c0ba57733b2--------------------------------) ·5 min read·2023年4月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2c0ba57733b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstand-sql-injection-and-learn-to-avoid-it-in-python-with-sqlalchemy-2c0ba57733b2&user=Lynn+G.+Kwong&userId=f649eccbbc3d&source=-----2c0ba57733b2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2c0ba57733b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstand-sql-injection-and-learn-to-avoid-it-in-python-with-sqlalchemy-2c0ba57733b2&source=-----2c0ba57733b2---------------------bookmark_footer-----------)![](../Images/a20548112fa3a883fe1500f087c263f2.png)

图片由 mohamed_hassan (Hosting Web Man) 提供，来源于 Pixabay

SQL 注入是最常见且最危险的网络安全漏洞之一，它允许黑客将恶意 SQL 代码注入未经验证和未清理的普通 SQL 查询中。这也是新开发者常常忽视的一个问题。

SQL 注入的原因和解决方案其实非常简单。在这篇文章中，我们将通过一些简单的查询来深入了解 SQL 注入，并假装成攻击者来利用我们的数据库。在文章结束时，你将完全理解 SQL 注入，并在认识到其威力和危险后，绝不会再犯此错误。

## 准备工作

和往常一样，我们将使用 Docker 创建一个 MySQL 数据库：

```py
# Create a volume to persist the data.
$ docker volume create mysql8-data

# Create the container for MySQL.
$ docker run --name mysql8 -d -e MYSQL_ROOT_PASSWORD=root -p 13306:3306 -v mysql8-data:/var/lib/mysql mysql:8

# Connect to the local MySQL server in Docker.
$ docker exec -it mysql8 mysql -u root -proot

mysql> SELECT VERSION();
+-----------+
| VERSION() |
+-----------+
| 8.0.31    |
+-----------+
1 row in set (0.00 sec)
```
