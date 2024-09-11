# openCypher* 对抗任何关系数据库

> 原文：[`towardsdatascience.com/opencypher-against-any-relational-database-a3b2388579df?source=collection_archive---------22-----------------------#2023-07-25`](https://towardsdatascience.com/opencypher-against-any-relational-database-a3b2388579df?source=collection_archive---------22-----------------------#2023-07-25)

## 关系数据库作为图数据库 = 留意（openCypher-2-SQL）

[](https://victormorgante.medium.com/?source=post_page-----a3b2388579df--------------------------------)![Victor Morgante](https://victormorgante.medium.com/?source=post_page-----a3b2388579df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a3b2388579df--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a3b2388579df--------------------------------) [Victor Morgante](https://victormorgante.medium.com/?source=post_page-----a3b2388579df--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe10afd255341&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fopencypher-against-any-relational-database-a3b2388579df&user=Victor+Morgante&userId=e10afd255341&source=post_page-e10afd255341----a3b2388579df---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a3b2388579df--------------------------------) ·8 min read·2023 年 7 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa3b2388579df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fopencypher-against-any-relational-database-a3b2388579df&user=Victor+Morgante&userId=e10afd255341&source=-----a3b2388579df---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa3b2388579df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fopencypher-against-any-relational-database-a3b2388579df&source=-----a3b2388579df---------------------bookmark_footer-----------)![](img/f3e3a60ae9575d4bbb263f5fbe810297.png)

图片由作者提供。阴阳月。修改自 [Syed Ahmad](https://unsplash.com/@syedabsarahmad?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/eWD4O1Me4rM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的免版权费照片

在任何关系型数据库上执行 openCypher 图查询的有限子集即是 Mindful 项目。查询是只读的，目前阶段没有元图查询。Mindful 是对[微软的 openCypher 到 SQL 转换器](https://github.com/microsoft/openCypherTranspiler)的闭源修改，使用[MIT 许可证](https://github.com/microsoft/openCypherTranspiler/blob/master/LICENSE)，Mindful 生成 SQL 以在任何关系型/SQL 数据库上运行。

有了这些背景知识……让我们开始了解范围……

在“Mindful”的背景下，“任何关系型数据库”意味着 openCypher 查询会被转换成针对**任何实际的关系型数据库**的 SQL，而不是那些必须为图类型查询特别修改表格的关系型数据库，或者那些将数据作为 JSON 注入字段并对这些 JSON 数据进行图状查询的数据库。

openCypher 查询会被转换成 SQL 以便在任何标准关系型数据库上运行。

说明视频：
