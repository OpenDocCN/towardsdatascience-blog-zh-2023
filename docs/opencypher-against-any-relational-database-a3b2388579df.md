# openCypher* 针对任何关系数据库

> 原文：[`towardsdatascience.com/opencypher-against-any-relational-database-a3b2388579df`](https://towardsdatascience.com/opencypher-against-any-relational-database-a3b2388579df)

## 关系数据库作为图形数据库 = Mindful (openCypher-2-SQL)

[](https://victormorgante.medium.com/?source=post_page-----a3b2388579df--------------------------------)![Victor Morgante](https://victormorgante.medium.com/?source=post_page-----a3b2388579df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a3b2388579df--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----a3b2388579df--------------------------------) [Victor Morgante](https://victormorgante.medium.com/?source=post_page-----a3b2388579df--------------------------------)

·发表于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----a3b2388579df--------------------------------) ·8 分钟阅读·2023 年 7 月 25 日

--

![](img/f3e3a60ae9575d4bbb263f5fbe810297.png)

图片由作者提供。阴阳月。由 [Syed Ahmad](https://unsplash.com/@syedabsarahmad?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 修改的免版税照片 [Unsplash](https://unsplash.com/photos/eWD4O1Me4rM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在任何关系数据库上运行的 openCypher 图查询的有限子集是 Mindful 计划。此阶段的查询是只读的，且不包含元图查询。Mindful 是 [微软的 openCypher 到 SQL 转换器](https://github.com/microsoft/openCypherTranspiler) 的封闭源代码修改版，遵循 [MIT 许可证](https://github.com/microsoft/openCypherTranspiler/blob/master/LICENSE)，其中 Mindful 生成 SQL 以操作任何关系/SQL 数据库。

考虑到这一点……让我们开始了解范围……

在 Mindful 的背景下，“任何关系数据库”意味着 openCypher 查询被转换为针对**任何实际关系数据库**的 SQL，而不是那些需要为图类型查询特别修改表格或将数据作为 JSON 注入字段并在该 JSON 数据上执行图形查询的关系数据库。

openCypher 查询被转换为 SQL，以便在任何标准关系数据库上运行。

解释视频：

## 对您业务的适用性 — 数据科学

您可能已经拥有一个现有的数据仓库、语义层或本质上是关系型的数据库，并且使用 SQL 作为主要查询语言……而您希望使用图形查询来查询您的数据资产。

相反，你可能急需从现有的图数据库迁移到关系/SQL 基于数据库，并需要数据迁移测试和实施工具。Mindful，一个 openCypher 到 SQL 的转译器，旨在成为实现你目标的工具。

现有的在关系数据库上进行图查询的实现需要特殊的表来有效表示节点类型和边类型（例如，具有单列主键的表）。Mindful 实现允许你在具有多列主键的表上运行 filter openCypher 查询。

在这篇文章中，我们展示了如何在不影响现有关系数据库栈的情况下实现这一点，通过采用一种数据科学策略，在 DDL 的评论部分使用 JSON 存储有关关系数据的同态图结构的元信息。例如，ORACLE、SQL Server、PostgreSQL 和大多数主流关系数据库都支持对元数据/DDL 进行注释。

实际上，通过使用现有数据库/数据仓库或语义层中提供的标准工具对数据库进行文档化，你将你的关系数据栈转换为图兼容的数据栈。

让我们深入探讨使这一切成为可能的数据科学和数据库理论……

## 关注细节

现代数据库管理系统显然具有以下特点，我们将对其进行探讨：

任何关系数据库都可以视为图数据库或关系数据库，取决于你的观点（如下所示）；以及

一旦数据库投入使用，数据库架构随处可见。例如，甚至连以前提倡无架构的图数据库业务如[Neo4j 也开始谈论“架构”](https://neo4j.com/developer-blog/describing-property-graph-data-model/)。

考虑到这一定位，我们可以想象，未来所有有价值的数据库都将提供图查询语言***以及***普通结构化查询语言（SQL）查询。

## 任何关系数据库都可以视作图数据库

图-关系范式、关系知识图谱 和多模型数据库并不新鲜，但为了设定背景，请注意实体-关系图与其对应的属性图模式及其反向映射：

![](img/6a872ad64fd4c16ec21eb6ddac3d92fb.png)

同态映射——属性图模式到实体-关系图。图像作者提供。

我们的架构用于在电影院预订座位以观看电影。一个人可以为在电影院观看电影的一个或多个座位进行预订；其中座位位于该电影院的某个区域和行中。

![](img/4f625ec6112a7fc989a5b5c7f1ef7b2b.png)

Shutterstock 图片，2187621947\. 授权给 Victor Morgante / FactEngine。 [免版税标准许可证 2187621947](https://www.shutterstock.com/image-photo/diverse-group-people-enjoying-watching-movie-2187621947)在 Shutterstock 上。

## 数据库理论…

使 Mindful 成功的两个方面是：

1.  **外键关系**和**多对多表**在其他关系数据库中与图数据库的**边类型**是同义的；

1.  图查询语言如 openCypher 支持可以用类似于 SQL 查询的方式编写的查询。

让我们单独查看这些内容…

## 外键和多对多表

**外键 <-> 边类型**

关系数据库中的外键关系与属性图模式中的对应边类型有 1:1 的映射关系。

在我们的模式中，每个名为 Row 的表中的 Row 记录都有一个指向‘Cinema’表中 Cinema 记录的外键引用，表示该行‘IS IN’的影院。

![](img/283b3889c8fe08c922063d3f42c2dd13.png)

实体关系图中的外键引用。图片由作者提供。

…在我们的属性图模式中有一个对应的边类型。

![](img/ffc13811f679e7b8034259d55d73681c.png)

属性图模式中的边类型。图片由作者提供。

…在我们的 DDL（数据库定义语言）中，这可以通过一个表示属性图模式的“IS_IN”标签的 JSON（JavaScript 对象表示法）轻松表示，嵌入为 CREATE TABLE DDL 语句中‘Row’表的对应外键参考定义的注释/文档。

```py
CREATE TABLE "Row" (Cinema_Id INTEGER REFERENCES [Cinema] NOT NULL
,RowNr NUMBER NOT NULL
, CONSTRAINT [Row_PK] PRIMARY KEY ([Cinema_Id],[RowNr])
, FOREIGN KEY ([Cinema_Id]) REFERENCES [Cinema] ([Cinema_Id])
     ON DELETE CASCADE ON UPDATE CASCADE /* { Label:"IS_IN"} */
)
```

…我们可以将关系视图和图视图之间的同态视为一种变形动画：

![](img/7277ccb0652ecad3458aa7b126fc073b.png)

外键关系与边类型之间的同态。图片由作者提供。

**多对多表 <-> 边类型**

类似地，关系数据库中的多对多连接表在传统图数据库的属性图模式中与其对应的边类型具有同态。

在我们的模式中，一个预订可能有多个座位，而一个座位（在其生命周期内）可能有多个预订。

注意，在我们的模式中，‘Booking’和‘Seat’表的主键是多列的，而我们在‘BookingHasSeat’多对多连接表上的主键有七列。

![](img/c6127e58561036a7741bf9f14fa888a4.png)

在 ER 图中的多对多连接表。图片由作者提供。

…在属性图模式视图中，Booking 和 Seat 节点类型之间有一个标记为‘HAS’的对应边类型。

![](img/6486f14c401d5d931ab4ff57d670db84.png)

属性图模式中的边类型。图片由作者提供。

…我们可以很容易地将‘HAS’标签作为 JSON 捕获在 DDL 中‘BookingHasSeat’表主键的注释中。

```py
CREATE TABLE "BookingHasSeat" (CinemaId INTEGER
,Letter TEXT(1)
,DateTime DATETIME
,RowNr NUMBER
,Person_Id INTEGER
,Cinema_Id INTEGER
,Film_Id INTEGER
,CONSTRAINT [BookingHasSeat_PK] PRIMARY KEY 
     ([CinemaId],[Letter],[DateTime],[RowNr],[Person_Id],[Cinema_Id],[Film_Id])
     /* { Label:"HAS"} */
,FOREIGN KEY ([Person_Id],[Film_Id],[DateTime],[CinemaId]) REFERENCES 
     [Booking] ([Person_Id],[Film_Id],[DateTime],[Cinema_Id])
     ON DELETE CASCADE ON UPDATE CASCADE /* { Label:"HAS"} */
,FOREIGN KEY ([RowNr],[Cinema_Id],[Letter]) REFERENCES 
     [Seat] ([RowNr],[Cinema_Id],[Letter])
     ON DELETE CASCADE ON UPDATE CASCADE /* { Label:"HAS"} */
)
```

…我们也可以将关系视图和图视图之间的同态视为一种变形动画：

![](img/abe388d3b2d5ea6edfe25f192801da0a.png)

**架构**

Mindful 的实现变得相当简单易懂。通过将适当的同态从关系模式映射到图模式，并且我们可以将有关同态的元信息存储在关系数据库的 DDL 中，我们有一种非常简单的机制来编写 openCypher 查询……因为实际上，关系数据库的元模型与图数据库的元模型之间接近同构。即，从适当的视角来看，它们在概念上几乎是相同的。

## 同态 — openCypher 到 SQL

…两种查询类型的故事

图查询语言有两种查询类型：

1.  过滤查询；以及

1.  元图/图遍历查询。

元图查询是一种在属性图模式的“图”（或模型）中查找路径、关系或结构的查询类型。

另一方面，过滤查询从数据库的所有数据的超集中过滤数据。例如，用自然语言表达：“Peter 预订了哪些影院？”

在这一阶段，我们最感兴趣的是过滤查询，因为这是 Mindful 代码所处的阶段，但一个元图查询可能会提出这样的问题，其他形式的自然语言表述为：“从一个人 Peter 到他预订的影院的最短路径是什么？”。

从图像上看，我们可以看到，对于我们的模式，最短路径是从人到预订，再到会话，再到影院，如下所示（而不是人-预订-座位-行-影院）：

![](img/7e93b4419851aa009306fc07eca31620.png)

最短路径 — 人到影院 — 元图/图遍历。图像作者提供。

对于我们的目的……我们需要过滤查询，因为 openCypher 过滤查询与 SQL 查询有同态，而 **标准 SQL 不支持元图查询**。

## 让我们使用 openCypher 查询

假设我们想知道登录名为“Peter”的人在哪个日期和时间在什么影院观看了哪个电影，以及座位字母和行号……我们可以为我们的模式编写以下 openCypher 查询：

**MATCH (p:Person)<-[:IS_FOR]-(b:Booking)-[:HAS]->(seat:Seat)-[:IS_IN]->(:Row)-[:IS_IN]->(c:Cinema), (b)-[:IS_FOR]->(s:Session)-[:IS_FOR]->(f:Film)

WHERE p.LoginName = ‘Peter’

RETURN p.LoginName, f.Name, s.DateTime, seat.Letter, seat.RowNr, c.CinemaName

这是一种过滤类型的查询，因为我们正在将结果筛选到那些登录名为“Peter”的人。

…现在让我们在关系数据库上运行这个查询，使用连接到 Mindful 的软件：

![](img/f70c7dd3285950b2f9736e61ffbe4ea1.png)

openCypher 查询与关系数据库 — 结果。图像作者提供。

在这个阶段，微软的 openCypher 到 SQL 转译器（以及 Mindful）生成的 SQL 相当繁琐，我在这里不会展示，但足以说明，相比于 openCypher 查询，相应的 SQL 查询相当长。以下是我们模式的等效 SQL 查询：

```py
SELECT Person.[LoginName],Film.[Name],Session.[DateTime],
       Seat.Letter, Seat.RowNr,Cinema.[CinemaName]
FROM Person,
Booking,
Session,
Film,
Seat,
Row,
Cinema
,BookingHasSeat
WHERE Booking.Person_Id = Person.Person_Id
AND Booking.Film_Id = Session.Film_Id
AND Booking.DateTime = Session.DateTime
AND Booking.Cinema_Id = Session.Cinema_Id
AND Session.Film_Id = Film.Film_Id
AND BookingHasSeat.Person_Id = Booking.Person_Id
AND BookingHasSeat.Film_Id = Booking.Film_Id
AND BookingHasSeat.DateTime = Booking.DateTime
AND BookingHasSeat.CinemaId = Booking.Cinema_Id
AND BookingHasSeat.RowNr = Seat.RowNr
AND BookingHasSeat.Cinema_Id = Seat.Cinema_Id
AND BookingHasSeat.Letter = Seat.Letter
AND Seat.Cinema_Id = Row.Cinema_Id
AND Seat.RowNr = Row.RowNr
AND Row.Cinema_Id = Cinema.Cinema_Id
AND Person.LoginName = 'Peter'
```

## 总结

所以，这就是如此简单。我们展示了关系模式和图模式之间的同态，使得可以编写软件，以便在任何适当配置的关系数据库上执行 openCypher 查询（过滤类型），并且 openCypher 被转换为 SQL。

感谢阅读。时间允许的话，我将写更多关于图数据库、关系数据库、图查询和同态的内容。

— — — — — — —

*Mindful openCypher 查询目前为只读，并且在此阶段没有元图查询。*

归属：本文中的模式源于最初在 [ActiveFacts examples github 页面](https://github.com/cjheath/activefacts-examples/tree/master/orm) 下以 [MIT 许可证](https://github.com/cjheath/activefacts-examples/blob/master/LICENSE.txt) 开源共享的内容。

— — — — — — — — — — — — — 结束 — — — — — — — — — — —
