# SQL 用户：用这个小技巧将（部分）查询的长度减半

> 原文：[`towardsdatascience.com/sql-users-halve-the-length-of-some-of-your-queries-with-this-one-trick-c3c2c226cb35?source=collection_archive---------6-----------------------#2023-03-29`](https://towardsdatascience.com/sql-users-halve-the-length-of-some-of-your-queries-with-this-one-trick-c3c2c226cb35?source=collection_archive---------6-----------------------#2023-03-29)

## QUALIFY 子句纯粹是语法糖

[](https://medium.com/@mattchapmanmsc?source=post_page-----c3c2c226cb35--------------------------------)![Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----c3c2c226cb35--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c3c2c226cb35--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3c2c226cb35--------------------------------) [Matt Chapman](https://medium.com/@mattchapmanmsc?source=post_page-----c3c2c226cb35--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf7d13fc53db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-users-halve-the-length-of-some-of-your-queries-with-this-one-trick-c3c2c226cb35&user=Matt+Chapman&userId=bf7d13fc53db&source=post_page-bf7d13fc53db----c3c2c226cb35---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c3c2c226cb35--------------------------------) ·6 分钟阅读·2023 年 3 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc3c2c226cb35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-users-halve-the-length-of-some-of-your-queries-with-this-one-trick-c3c2c226cb35&user=Matt+Chapman&userId=bf7d13fc53db&source=-----c3c2c226cb35---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc3c2c226cb35&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-users-halve-the-length-of-some-of-your-queries-with-this-one-trick-c3c2c226cb35&source=-----c3c2c226cb35---------------------bookmark_footer-----------)![](img/7e7f9a7323462a08f37dd7ce8eb87c2c.png)

图片由 [Joanna Kosinska](https://unsplash.com/@joannakosinska) 提供，来源于 [Unsplash](https://unsplash.com/photos/-ayOfwsd9mY)

在这篇文章中，我将向你介绍 SQL `QUALIFY` 子句：一个帮助你编写更短、更易读的 SQL 查询的小技巧。

如果你从事数据科学/分析或机器学习工作，这里有四个原因说明这是一个必备的 SQL 工具：

1.  第一，因为它可以帮助你编写更***短小***的查询，简化数据收集流程。根据我的经验，`QUALIFY`能将许多查询的长度减少多达 50%。

1.  第二，它将帮助你编写更加***简洁***的查询。这很重要，因为正如保罗·格雷厄姆（Paul Graham）著名地说的，[简洁即力量](http://www.paulgraham.com/power.html)，它使你的代码更具可读性和可维护性。

1.  第三，`QUALIFY`帮助你编写更***高效***的查询，从而降低成本并加快开发时间。

1.  第四，这是一种炫耀的简单方法。我并不是说使用`QUALIFY`会让你赢得任何创新奖，但因为它是一个[相对较新的子句](https://modern-sql.com/caniuse/qualify)，你的老板可能还没听说过。无论如何，向你的团队展示一个新技巧会很有趣。

# `QUALIFY`子句的作用是什么？
