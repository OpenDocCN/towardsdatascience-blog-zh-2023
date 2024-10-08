# 选择正确的数据库模型，让你的数据摆脱电子表格

> 原文：[`towardsdatascience.com/choose-the-right-database-model-free-your-data-from-spreadsheets-8d1129626b42`](https://towardsdatascience.com/choose-the-right-database-model-free-your-data-from-spreadsheets-8d1129626b42)

## 你已经超越了 Excel：如何选择关系型、文档型或图数据库，并为未来做好准备

[](https://medium.com/@kalebnyquist?source=post_page-----8d1129626b42--------------------------------)![Kaleb Nyquist](https://medium.com/@kalebnyquist?source=post_page-----8d1129626b42--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8d1129626b42--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----8d1129626b42--------------------------------) [Kaleb Nyquist](https://medium.com/@kalebnyquist?source=post_page-----8d1129626b42--------------------------------)

·发表于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----8d1129626b42--------------------------------) ·阅读时间 16 分钟·2023 年 5 月 8 日

--

打开一个空白的电子表格时，无尽的行和列似乎成为了无限可能性的画布。然而，许多数据工程师和其他数字知识工作者越来越将电子表格中交错的灰线视为限制——具有讽刺意味地像是监狱牢房的横竖金属栏杆！

![](img/4f4481988d97ecf7f9711a1cd7d4de3a.png)

**诚然，为了使“电子表格监狱”的视觉隐喻生效，电子表格必须旋转 90°。但一旦你看到这种诡异的相似性，就不可能忘记。** *照片插图由作者提供。* [*照片*](https://unsplash.com/photos/JC7bE-eQXIk) *由* [*WWW PROD*](https://unsplash.com/@wwwprod) *提供，来自* [*Unsplash*](https://unsplash.com/photos/JC7bE-eQXIk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)*。*

“电子表格监狱”发生在决定将数据存储在电子表格中，使组织在高效实现目标时陷入瓶颈。这主要是因为电子表格的单元格（无意玩笑）在查询和管理方面受到限制：例如，今天单元格`K18`可能表示某个物品的数量，但如果明天添加了一行和一列，`K18`可能完全指代其他内容。

对于较大的项目，“电子表格困境”还发生在存储的数据量超过电子表格的最大大小时（Google Sheets 为 1000 万个单元格；Microsoft Excel 为 1,048,576 行和 16,384 列）。在一个极端的例子中，一份财务数据清单在[一名员工发现其大小与一个满员的 Excel 电子表格完全相同时被发现是伪造的，这一欺骗行为现在使公司的耻辱首席执行官面临数十年的实际监禁](https://workweek.com/2023/01/16/frank/)。

对于那些未参与犯罪活动的我们来说，电子表格困境的风险不那么字面化，但我们不当使用电子表格往往是成本高昂且繁琐的。考虑以下问题：

+   打开一个曾被认为是可靠和高效的信息查找方式的电子表格是否现在变成了令人恐惧的源头？

+   您的组织是否有工作人员每天花费多个小时回答那些答案要么 (1) 深埋在一个电子表格中，要么 (2) 分散在多个电子表格中？

+   是否有重要的分析问题让您的数据透视表专家也感到困惑？类似地，您的单元格公式是否脆弱，频繁出现错误或需要多个手动步骤来计算？

+   重要信息是否被 relegated 到单元格值之外，例如作为电子表格中的“注释”或“评论”？

如果您对所有这些问题的回答都是“否”，祝贺您！当数据管理不会阻碍您实现目标时，您就不会陷入电子表格困境。没有必要用过于强大的数据库解决方案替换那些有效的方案。

然而，如果您对这些问题中的任何一个回答为“是”，那不幸的是，这表明您正陷入电子表格困境。好消息是，通过采用许多更适合您需求的数据库技术，您可以摆脱困境。

接下来，我将通过介绍三种数据库模型范式来帮助缩小选择范围，同时提供一个特别适合转换为该模型的电子表格示例。在查看这些描述时，您可能希望保留一个最具问题的电子表格副本，以尝试诊断导致您陷入电子表格困境的问题，并确定哪种数据库模型是最佳解药。

这些描述是针对一些具备技术知识的项目经理和高管编写的，旨在帮助他们更好地理解数据需求。数据专业人士也可以参考这篇文章，以找到倡导采用更强大数据库技术所需的语言。

# 关系型数据库

关系数据库由“表”组成，这些表在概念上类似于电子表格的二维网格，但每行数据通过“键”在表之间相互连接。“主键”是一个数据点，它表示其他表中的一个单独的行，而“外键”存储来自其他表的哪一行（或多行）与给定表中的行具有特定关系。

SQL —— 或称为“结构化查询语言” —— 常用于与存储在关系数据库中的数据进行交互，包括提问涉及多个表的数据。尽管一开始可能听起来令人畏惧，SQL 实际上相对容易学习且成本低廉（网上有许多[免费的 SQL 课程](https://www.coursera.org/learn/sql-for-data-science)，许多社区学院也提供 SQL 的入门课程）。

值得注意的是，自 1986 年 [美国国家标准协会首次认可 SQL 的一种形式](https://blog.ansi.org/2018/10/sql-incits-american-national-standard/#gref) 以来，SQL 就成为了一个公共资源，这种形式可以在公司、政府机构和学术机构之间互换使用。如果你的目标是导入 [开放数据](https://en.wikipedia.org/wiki/Open_data) 或导出内部数据用于学术研究，那么拥有一个可以通过 SQL 访问的关系模型数据库可以说是最安全的选择。

由于关系数据库和 SQL 的普及，确定最合适的解决方案可能会让人感到不知所措。像 Oracle 和 SQL Server 这样的最受欢迎的数据库通常对于只是从电子表格迁移且尚未运行完整应用程序的大多数组织来说，显得过于强大且成本过高。更合适的是包括 [PostgreSQL](https://www.postgresql.org) 这样的免费和开源选项，它不在功能上打折扣，以及 [SQLite](https://www.sqlite.org) 这样的轻量级选项，便于部署。

如果关系数据库模型很吸引人，但设置和学习 SQL 的投资成本过高，[Airtable](https://airtable.com/invite/r/9jyJBxR8) 是一个很好的折中方案。Airtable 的电子表格类似的图形界面不需要查询语言，但对于与从终端用户的角度来看，类似于关系数据库模型的功能进行交互效果很好。

## 电子表格诊断

如果你的数据目前存储在电子表格中，如果该电子表格有大量的“重复子行”，你应该考虑迁移到关系数据库。

**这个电子表格跟踪了课后项目的参与者。注意到 Parent 是一个“重复子行”，特别是 Parent 2 单元格的值要么完全填充，要么完全为空。这使得电子表格的末端有一种“长条奶酪”的外观，行在末端呈现出磨损的效果。** 示例数据由作者提供。

比如，你运营一个课后项目。你的电子表格中的每一行代表一个孩子的数据（姓名、年级、过敏信息等），并且你有子行代表每个孩子父母的数据（姓名、联系信息等）。多个孩子很可能有相同的父母，但如果你需要更新父母的联系信息，你必须确保捕捉到父母出现的每一子行。

![](img/e3ed1dd0501b1ba5eb3f838b1c15babe.png)

**这个简单的实体关系图，使用** [**乌鸦脚符号**](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model#Crow's_foot_notation)**，展示了课后项目数据库中每个孩子都有一个姓名、年级、一份过敏清单（包括一个空列表）和一个或多个相关父母。每个父母都有姓名、电子邮件地址、电话号码和一个或多个相关学生。** 图示由作者提供。

关系数据库通过将“子行”转换为一个引用存储在不同“父”表中的数据的“外键”单元来解决这个问题。如果你更新一个父表的信息，你会同时更新与该父表相关的每个子表的信息。由于不需要在重复数据上进行批量更新，使用关系数据模型时，数据完整性的维护变得容易得多。

**课后项目的数据迁移到 Airtable 中。虽然从技术上讲这不是一个 SQL 数据库，但 Airtable 在保持熟悉的电子表格界面的同时，拥抱了关系数据范式。** 示例数据由作者提供。

当然，大多数课后项目都是由志愿者和其他没有时间和技能来管理完整 SQL 数据库的人运营的。[Airtable](https://airtable.com/invite/r/9jyJBxR8)是这个使用场景的一个极好的无代码替代方案，额外的好处是它有内置工具如“表单”，允许引导数据输入。

# 面向文档的数据库

面向文档的数据库，或称为“文档存储”，非常适合处理结构松散的数据。如果关系数据库的范式是由“行”和“列”组成的“表”，那么文档导向数据库的范式是由“键”和“值”对组成的“文档”的“集合”。例如，`Length (meters)`的键可能与`18`的值配对，或者`Locations`的键可能与一个由`["Portland", "Omaha", "San Antonio", "Savannah"]`组成的列表配对。此外，值甚至可以是一个称为“子文档”的全新键值对集合。假设每个文档都有一个唯一的 ID，那么一个值也可以指向另一个文档，从而模拟 SQL 数据库的关系性。

对数字数据术语不熟悉的人可能会对“文档”一词感到困惑。我必须承认，当我第一次了解文档导向数据库时，我天真地认为这些系统是为了存储一堆 PDF 和 Word 文档（我后来发现这类技术的术语是[“文档管理系统”](https://en.wikipedia.org/wiki/Document_management_system)）。然而，在这个上下文中，“文档”实际上指的是轻量级且流行的[JSON](https://www.json.org/json-en.html)格式，设计时考虑到人类和计算机都相对容易读取或写入。而关系数据库中的每一行必须符合表的模式（也就是“列”），而 JSON 文档在理论上可以具有任何所需的键值对。

由于这种灵活性，文档导向的数据库被认为是“适应性强”的数据库模型。随着时间的推移，当你发现有关所收集信息的更多数据时，你可以添加新的字段，而不会弄乱任何具有不同键值对的旧记录。此外，由于相同的键可以与不同的*类型*值关联，这种数据库模型非常适合数据的异质性。

利用这种异质性的一种实际方法是创建“占位符数据”，如一个文本字符串，稍后将扩展为子文档。它还使区分不同类型的空白数据变得更容易，例如通过（1）排除一个键来表示缺失数据，或者（2）将值设置为`null`或`None`，如果没有要观察的数据，例如完全在线运营的组织的地理坐标，或一个还未到驾驶年龄的小孩的驾照号码。

文档存储非常适合那些处于探索阶段的组织，特别是[转向文档导向数据库是改善 HealthCare.gov 网站命运的一部分](https://www.fedhealthit.com/2016/01/the-untold-story-of-rescuing-healthcare-gov/)，使其能够不断适应来自多个联邦和州机构的不断变化的数据输入。不过，如果数据库的工程师缺乏规范性并且在添加字段时不小心，数据库随着时间的推移可能会变得越来越混乱和难以管理。

最受欢迎的文档导向数据库解决方案是 MongoDB。凭借[几乎但不完全被认为是开源的许可证协议](https://en.wikipedia.org/wiki/Server_Side_Public_License)，他们的[免费 512MB 云层](https://www.mongodb.com/blog/post/free-your-genius-on-mongodb-atlas-free-tier)和[本地托管的社区版](https://www.mongodb.com/try/download/community)可以满足许多从电子表格过渡的用户的数据需求，而无需直接支出。此外，MongoDB 查询的基础（有时称为“MQL”）和其他数据导航工具如 Compass 可以[在线免费学习](https://university.mongodb.com/)。然而，与 SQL 不同，[“MQL”实际上不是标准化的查询语言，而是一组特定语言的驱动程序和 API](https://akira-kurogane.github.io/mongodb-guide/en/rw_data_guide/mql_vs_sql/)。这意味着虽然很难找到了解 MQL 的人才，但对于已经在特定编程语言（如 Python、JavaScript 等）中工作的人来说，学习 MQL 并实现 MongoDB 仍然很容易。

## 电子表格诊断

如果您的数据目前存储在电子表格中，且有大量单元格完全为空或数据过于拥挤（超过一半的电子表格是一个很好的经验法则），您应该考虑转向文档导向的数据库。您还需要注意那些仅用于输入少量数据的“临时列”或“杂项”备注列。

**这个电子表格存储了本地社区目录的信息。注意到使用了`|`符号在单元格内创建了“迷你电子表格”。此外，电子表格中还有许多空单元格，使得整个电子表格呈现出一种“瑞士奶酪”般的外观。** 示例数据由作者提供。

上述示例电子表格展示了一个假设的本地地点列表，这些地点除了有名称外，几乎没有共同之处。注意到有些地点的数据很多，比如 Joe’s Burgers，而有些地点如 Alcove Speakeasy 的数据稀少。许多单元格是空的，包括`Restrictions`列，该列仅包含一行数据。在`Websites`列下，不清楚单元格是否为空是因为数据丢失（如 Joe’s Burgers 的情况），还是因为该地点没有网站（如 Centennial Obelisk 的情况）。还有一些单元格包含大量嵌套数据，特别是在`Photos`、`Prices`和`Scores`列下。当然，还有一个`Miscellaneous`列，将三个潜在的列合并为一个，以防止电子表格变得过于庞大。

![](img/b8be51f57d912e506617a3590d188b83.png)

**相同的本地社区目录以 JSON 文件的形式。这类似于数据在面向文档的数据库中存储的方式。** [完整文件可以在此查看](https://gist.github.com/KalebNyquist/7e4708747840ef01b1f77b0a54fd6006)。作者提供的截图。

查看以 JSON 文件形式存储的相同社区目录，一些关键优势变得显而易见。

+   文档的大小可以根据存储的实际数据量而有所不同。这意味着，与其使用 `Miscellaneous` 列来减少电子表格列的数量，不如为少量文档创建一个 `Awards` 属性。

+   请注意，对于“Joe’s Burgers”，没有 `Website` 字段，而对于“Centennial Obelisk”却有。这是一种传达 Joe’s Burgers 的网站可能存在但在数据中缺失的方式，而 Centennial Obelisk 没有网站。与电子表格相比，这两种情况在电子表格中是不可区分的！

+   数据的表现形式有更多的空间，比喻地说。`Photos`、`Prices` 和 `Scores` 字段挤在电子表格单元格中，但可以作为自身的子文档进一步扩展，或者在 `Scores` 的情况下，作为一个列表。

+   使用 `@id` 字段作为“主键”创建了关系数据结构，而 `Type` 和 `User` 等字段可以通过它们的 `@id` 查找文档。

# 图数据库

图数据库旨在描述连接。这通过同时管理两种不同的数据顺序来实现：“节点”和“关系”（注意，术语可能因具体的图数据库技术而异）。**节点**具有独立于任何模式或其他数据的自身存在，尽管节点可以使用“标签”进行分类（类似于关系数据库使用“表”以及面向文档的数据库使用“集合”进行分类）。**关系**作为图数据的第二种顺序，根据源节点和目标节点定义，并按照关系类型进行分类。类似于面向文档的数据库中的文档，节点和关系都可以具有任意数量的属性，以键值对的形式表示。

再次，对于数字数据领域的新手来说，“图”的术语可能会分散对这种模型类型实际意义的注意力。与“条形图”或“折线图”等常见数据可视化不同，这里的“图”指的是这种数据模型类型更像是一个由圆圈（作为节点）和线（作为关系）构成的思维导图。这种数据模型类型被认为与人脑的工作方式相似，这不仅对[神经科学](https://ercim-news.ercim.eu/en125/r-i/graph-based-management-of-neuroscience-data-representation-integration-and-analysis)和[人工智能](https://www.zdnet.com/article/neo4j-ceo-why-graph-databases-and-ai-belong-together/)有影响，还意味着图数据模型对于技术不太熟悉的相关方相对容易理解。

具有讽刺意味的是，图数据库实际上比关系数据库更*注重关系*。虽然关系数据库通过在表之间匹配主键和外键来连接相关数据，但图数据库中的关系有其“存在”，因此可以加载额外的属性。这些属性可以描述关系的强度、质量、持续时间或其他特征。

领先的图数据库解决方案是[Neo4j](https://neo4j.com)。他们的社区版是[完全开源的](https://neo4j.com/licensing/)，可以用于本地或自托管，[免费云层的上限为 200,000 个节点和 400,000 个关系](https://neo4j.com/pricing/)。Neo4j 查询语言叫做“[Cypher](https://neo4j.com/docs/getting-started/cypher-intro/)”，[可以免费学习](https://graphacademy.neo4j.com/categories/cypher/)，并且具有独特的可视化特性，使非开发人员容易理解。目前，Cypher 启发的“图查询语言”（或 GQL）的标准化正在[进行中](https://en.wikipedia.org/wiki/Cypher_(query_language)#Standardization)，甚至被认为是联合国[可持续发展目标第 9 项——建立韧性基础设施、促进包容性和可持续工业化以及促进创新](https://www.iso.org/standard/76120.html)的一个步骤。

## 电子表格诊断

如果你们组织的数据目前都存储在电子表格中，你应该考虑跃迁到图数据库，特别是当你经常通过注释、评论和额外的列（这些列仅用于描述其他列中的数据）来上下文化数据时。

![](img/da8dcd20f2a064eb6d119ee37911a6a0.png)

**这个电子表格中的许多单元格都有一个“备注”，其中包含重要的背景数据，可以通过将光标移动到单元格上来显示。在 Microsoft Excel 中，带备注的单元格由右上角的红色三角形标识，使得电子表格看起来像是带有黑胡椒奶酪的外观。** [下载文件在这里](https://www.dropbox.com/s/6imt1mmg7ga983q/Graph%20Database%20Candidate%20-%20Power%20Mapping.xlsx?dl=0)。屏幕截图由作者提供。

上述示例展示了一个名为“Fayette Advocacy Network”的虚构倡导组织的[“权力映射”](https://en.wikipedia.org/wiki/Power_mapping)。权力映射用于追踪在特定问题上有影响力的人物、他们对该问题的态度，以及他们与组织的关系（无论间隔多少度）。

注意，这个图表中最有趣的数据并不在单元格的数值中，而是在每个单元格的备注中。例如：市议员纳迪亚·卡尔森和编辑委员会成员朱利斯·卡尔森是夫妻关系。斯科特·普尔不仅是 Fayette Teachers Union 的主席，还是 Fayette Public Schools 的员工，并且是 Fayette Advocacy Network 的活跃成员。尚未确认的 Fayette Advocacy Network 的原因已经被记录在案，因为这是通过他在社交媒体上的评论得知的。

![](img/22b059f704127aa24f411362b2825d2d.png)

**在这个图表视图中，决策者用蓝色表示，其他人用品红色表示，组织用绿色表示，对问题的立场用灰色表示，而 Fayette Advocacy Network 则有一个大大的金色节点。节点之间的关系用箭头表示，这本身就是一种有价值的数据形式。** [在 Arrows.app 中查看](https://drive.google.com/file/d/1eGh38T4oQO1obfK2lPMePEjPwBV6hUTQ/view?usp=sharing)。

由于图形数据库可以存储带有属性的关系，因此表格中仅备注的数据可以在图形数据库中突出显示。图形模型类型提高了这些宝贵的背景信息的组织性和可访问性。

当一次性查看所有数据时，它可能看起来是一团糟。可以通过查询更具体的问题来缩小数据范围，例如：“谁是与对问题的立场仍然未知的决策者有联系的 Fayette Advocacy Network 的成员？”作为参考，下面提供了一个相应的 Cypher 查询。注意，这段主要为计算机解析而编写的代码对于人类阅读相对简单。

```py
MATCH (faynet:Organization)<-[:MEMBER_OF]-(member:Person)-[:CONNECTED*1..2]-(decisionmaker:Person)-[:STANCE]->(pos:Position)
WHERE faynet.name = "Fayette Advocacy Network"
AND pos.name = "Unknown"
RETURN member
```

这个查询返回一个`Person`的列表，这些`Person`是“Fayette Advocacy Network”的`MEMBER_OF`，并且与`decisionmaker`（其对问题的`Position`为“unknown”）有`CONNECTED`（或与连接到`decisionmaker`的某人连接）。即使你不了解 Cypher，你也应该能够通过花几分钟时间琢磨各种箭头和括号的含义来弄清楚查询的工作原理。

值得一提的是：为了帮助理清这些混乱，Neo4j 还提供了一套图数据科学包，以帮助识别模式并计算节点重要性的常见度量，如 [度中心性](https://en.wikipedia.org/wiki/Centrality#Degree_centrality) 和 [中介中心性](https://en.wikipedia.org/wiki/Centrality#Betweenness_centrality)。越来越多的软件解决方案可用于可视化这些图数据，从 Neo4j 的简单内部解决方案 [Bloom](https://neo4j.com/product/bloom/) 一直到 [虚拟现实头戴设备](https://mod.studio/rd/grapho/)。不用说，这些查询、计算和可视化在单独使用电子表格时几乎是不可能完成的。

# 跳跃到数据库

本指南的动机原则是帮助你识别适合你数据需求的技术。很多时候，电子表格实际上会是合适的技术：用于以简单格式存储非关系数据，与技术水平较低的用户协作，快速处理一些数字，甚至作为数据驱动的头脑风暴和项目规划的创意场所。电子表格之所以有效，是因为它们将数据输入、工程、分析、协作和共享全部集中在一个空间中。

但将所有这些功能捆绑在一起的便利性，也可能使得电子表格对于你的数据来说变得限制重重。现实情况可能是，你需要关系数据库的关联性，或文档导向数据库的适应性，或图数据库的上下文化。即使你已经投入了数百小时来完善一个电子表格——实际上，你投入的时间越多，电子表格越可能阻碍你。

> “电子表格无论输入什么内容看起来都很好。有人把电子表格带到会议中，它看起来如此精确。它有数字和图表，还有漂亮的字体。它感觉像是在告诉你世界的真相。感觉像是真理。”
> 
> — David Kastenbaum 在 [电子表格！](https://www.npr.org/transcripts/389027988)

电子表格陷阱发生是因为电子表格让人感到安全。与数据库技术不同，像 Microsoft Excel 和 Google Sheets 这样的工具捆绑在家庭办公室套件中，并且具有与我们的文字处理器和幻灯片制作器相同的熟悉界面。我们中的许多人是在学校里第一次学会使用电子表格的，有时处理电子表格也是联系同事的纽带。[即使在 TikTok 上，也有社区专注于掌握电子表格的艺术](https://www.insider.com/tiktok-influencer-excel-spreadsheets-viral-miss-video-2021-12)。

![](img/073ffb387ec84451f2fb56e6432549f8.png)

**迈出一步。** 图片插图由作者制作。 [照片](https://unsplash.com/photos/Zdf3zn5XXtU)由 [Sammie Chaffin](https://unsplash.com/@sammiechaffin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/Zdf3zn5XXtU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上提供。

要将我们的数据从电子表格的囚笼中解放出来，需要一跃而起。我们必须相信，特定数据库模型的好处将超过采用陌生技术所带来的风险。好消息是，我们还有错误的余地：通过几行代码修复一个混乱的数据库比通过数百次鼠标点击修复笨重的电子表格要容易得多。此外，选择错误的数据库技术起步并不是世界末日：一旦电子表格被重新组织为表格（关系型）、集合（文档导向型）或标签（图形型），在这三种范式之间转换就会容易得多。

然而，逃离电子表格囚笼可能还有一个更重要的原因。随着我们将数据从电子表格中解放出来，我们也解放了对数据的想象力以及数据如何在组织中被利用。查找和输入数据从成为浪费时间的琐事变成了对日益激动人心和有价值的信息库的贡献。我们的数据从笨拙地保持的内部秘密变成了团队成员和外部合作伙伴的协作中心。

再仔细看一眼你的数据。如果电子表格囚笼阻碍了你实现目标，是时候深呼吸，勇敢地跃入数据库的世界了！

*我的目标是架起数字数据与“现实世界”组织之间的桥梁。如果你欣赏这份指南，以下是你可以支持这篇及未来作品的方式：*

+   *阅读一些* [*我的其他帖子*](https://medium.com/@kalebnyquist)*。如果你喜欢本文中的例子，你也会喜欢* *“非营利数据管理的 5 个最佳实践”* *，在其中我诊断了另一种有问题的电子表格：隐式数据的色彩编码承载体。*

+   *通过* [*通过我的推荐页面成为会员获取更多 Medium 内容*](https://medium.com/@kalebnyquist/membership)*。*

+   *通过我的邀请链接注册一个免费的 Airtable 账户* [*](https://airtable.com/invite/r/9jyJBxR8)*。

+   *有兴趣提升你的数据科学、网络安全、软件工程或产品设计技能吗？注册以* [*通过我的校友推荐链接获取关于 Flatiron School 的更多信息*](https://flatironschool.com/scholarships/2022q2ar/?utm_campaign=ec2a7f51-e911-403e-8e92-50950d9f7c32&utm_source=emailbatch&utm_medium=email&utm_term=txtlink)*。*

+   *通过* [*BuyMeACoffee.com*](https://www.buymeacoffee.com/kalebnyq) *或者* [*Ko-fi.com*](https://ko-fi.com/kalebnyquist)*发送小费*。
