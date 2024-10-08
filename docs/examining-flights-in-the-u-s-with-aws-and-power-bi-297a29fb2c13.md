# 使用 AWS 和 Power BI 检查美国的航班

> 原文：[`towardsdatascience.com/examining-flights-in-the-u-s-with-aws-and-power-bi-297a29fb2c13`](https://towardsdatascience.com/examining-flights-in-the-u-s-with-aws-and-power-bi-297a29fb2c13)

## 我们可以通过 ETL 和 BI 获得哪些见解？

[](https://medium.com/@aashishnair?source=post_page-----297a29fb2c13--------------------------------)![Aashish Nair](https://medium.com/@aashishnair?source=post_page-----297a29fb2c13--------------------------------)[](https://towardsdatascience.com/?source=post_page-----297a29fb2c13--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----297a29fb2c13--------------------------------) [Aashish Nair](https://medium.com/@aashishnair?source=post_page-----297a29fb2c13--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----297a29fb2c13--------------------------------) ·阅读时间 9 分钟·2023 年 7 月 6 日

--

![](img/7bf31178eb840ce3f7f2851143eee040.png)

图片由 [John McArthur](https://unsplash.com/@snowjam?utm_source=medium&utm_medium=referral) 拍摄，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 目录

∘ 引言

∘ 问题陈述

∘ 数据

∘ AWS 架构

∘ 使用 AWS S3 的数据存储

∘ 设计架构

∘ 使用 AWS Glue 的 ETL

∘ 使用 AWS Redshift 进行数据仓储

∘ 使用 AWS Redshift 提取见解

∘ 使用 Power BI 进行数据可视化

∘ 未来步骤

∘ 结论

∘ 参考文献

## 引言

空中旅行已成为我们生活中不可或缺的一部分。它不仅是企业建立网络和开展商业活动的手段，也是家庭探访亲人或旅行的方式。

尽管航空行业具有很大影响力，但它也面临许多波动。由于经济萧条和繁荣、气候变化、Covid-19 大流行以及对可再生能源的依赖推动等外部因素，这一行业不断发生变化。

要关注这些变化及其对空中旅行的影响，值得跟踪这些航班的时间变化。这一工作需要一个稳健的数据仓储、数据分析和数据可视化策略。

## 问题陈述

本项目有两个主要目标。第一个目标是利用亚马逊网络服务（AWS）提供的资源，构建一个数据管道，以便存储、转换和分析美国航班数据。

第二个目标是构建一个 Power BI 可视化工具，能够有效地展示数据中的关键发现。

## 数据

该项目使用的数据集来自 [交通统计局](https://www.transtats.bts.gov/ot_delay/OT_DelayCause1.asp?20=E)。它主要报告了 2003 年至 2023 年间机场和航空公司总航班、延误和取消的数量。

以下是数据集的预览：

![](img/20a83d3f6f6e1034f9af42167ffc532c.png)

预览（作者创建）

一眼望去，原始数据集存在一些问题。

首先，`airport_name` 字段中的信息包括多个信息点。它不仅提供了机场的名称，还包括城市和州。为了便于访问这些信息，这个字段将需要拆分成 3 个单独的字段。

其次，这些数据当前采用了平面模型（即 1 张表）。然而，这并不是最佳设置，因为数据包含多个有关系的实体。

这些缺陷必须在数据可以用于分析或可视化之前解决。

## AWS 架构

让我们探讨构建数据管道所需的 AWS 架构。

所需资源的最佳示意图如下：

![](img/758a0dd8cbadacd0dea9655997bfa7af.png)

AWS 架构（作者创建）

云解决方案使用 Amazon S3 存储原始数据和转换后的数据，AWS Glue 创建 ETL 作业以促进数据转换，AWS Redshift 创建云数据仓库，使用户能够使用 SQL 从数据中提取见解。

最后，使用 Power BI 以仪表板形式展示数据提供的关键指标。

## 使用 AWS S3 存储数据

该项目利用了两个 S3 桶：`flights-data-raw` 和 `flights-data-processed`。

![](img/0f213beaeeccb77629d06383c7da858e.png)

S3 桶（作者创建）

`flights-data-raw` 桶包含原始数据集。

![](img/38f386af929602df266e84d67bd5e135.png)

flights-data-raw 桶（作者创建）

`flights-data-processed` 桶将包含数据转换后的内容（目前为空）。

## 设计模式

接下来，确定适合这些数据的模式非常重要。原始数据存储在一个平面文件中，该文件包含一张表：

![](img/105527d7546868f2b3caf746f2bff64b.png)

原始数据（作者创建）

不幸的是，这个模式只有一个表，其中包含多个实体，如日期、机场和航空公司。为了优化数据库以更快地检索数据，可以将这个平面模式转换为星型模式，使用维度建模：

![](img/5d4c59165f5ce2980bbfc8dcc9f42f0a.png)

星型模式（作者创建）

在这个新的模式中，`flights` 表作为事实表，而 `date`、`carrier` 和 `airport` 表作为维度表。

## 使用 AWS Glue 的 ETL

使用 AWS Glue 创建的 ETL 作业可以将原始数据转换为事实表和维度表，并将它们加载到 `flights-data-processed` 桶中。

ETL 任务使用导入的 Python 脚本进行维度建模。

![](img/7f1bc2982e72a6638976049967a88fc5.png)

创建 ETL 任务（作者创建）

该脚本使用 boto3，即 Python SDK，从 `flights-data-raw` 存储桶中提取原始数据集，创建星型模式中的 4 个表，并将它们作为 csv 文件加载到 `flights-data-processed` 存储桶中。

例如，以下代码片段用于创建 `carrier` 表。

用于创建模式中 4 个表的完整脚本可以在 [GitHub 仓库](https://github.com/anair123/Tracking-U.S.-Flights-With-AWS-and-Power-BI) 中访问。

ETL 任务运行正常：

![](img/fb20dac17dbf45a4a86732d748ff60f2.png)

成功运行（作者创建）

数据集已经转换为一个事实表和 3 个维度表，形式为 csv 文件，全部存储在 `flights-data-processed` 存储桶中。

![](img/1dda9d4f42a1d4ed6e42f1ebb0d559b1.png)

flights-data-processed 存储桶（作者创建）

## 使用 AWS Redshift 进行数据仓库管理

使用 AWS Glue，最初的平面模型现在可以在数据仓库中用更合适的星型模式表示。

该数据的云数据仓库将通过 AWS Redshift Serverless 创建。这包括创建一个命名空间 `flights-namespace` 以及一个名为 `dev` 的数据库。此外，还需要一个名为 `flights-workgroup` 的工作组，用于编写 SQL 查询。

> 注意：工作组已配置为允许 VPC 外的设备访问数据库。这在使用 Power BI 创建可视化时将非常有用。

![](img/c2564ad18a60d46c91742954c26312f8.png)

工作组（作者创建）

现在，我们可以打开 Redshift 中的查询编辑器，并开始在 `dev` 数据库中创建事实表和维度表。

![](img/854ce456d3239eb16f91323ce9eb8137.png)

查询编辑器（作者创建）

首先，需要使用以下命令在仓库中创建模式中的 4 个表：

![](img/92d92bc99aedf7001ddec7bf346d540b.png)

创建的表（作者创建）

这四个表现在已经在数据仓库中，但由于数据仍在 `flights-data-processed` 存储桶中，因此它们都是空的。

数据可以使用 `COPY` 命令复制到此数据仓库中。

例如，可以使用以下命令语法将 `flights.csv` 中的数据复制到 `flights` 表中：

> 注意：`iam_role` 变量应分配为创建工作组时所选的 iam 角色。

通过对 `flights-data-processed` 存储桶中的每个 csv 文件执行 `COPY` 命令，4 个表格应被填充所需的数据。

例如，这里是机场表的预览：

![](img/6734b0767b7283f78075038b89df7974.png)

查询输出（作者创建）

## 使用 AWS Redshift 提取洞察

现在所有表格都已加载数据，我们可以开始使用 SQL 查询进行分析！

由于数据已经转化为具有维度建模的星型模式，因此可以高效地检索数据，运行时间很短，因此这个设置非常适合临时分析。

以下是一些可以通过 SQL 查询回答的问题示例。

1.  **2022 年哪些机场的航班最多？**

![](img/dfd5daf57330433488995ead686a720b.png)

查询输出（由作者创建）

**2\. 从 2019 年起，哪种类型的延误对总延误的贡献最大？**

![](img/bce44e539eec366dceac1fc9f69407e1.png)

查询输出（由作者创建）

**3\. 在约翰·F·肯尼迪机场，每年的延误百分比变化是多少？**

![](img/e8f94d9feac99dccb02a2da76fcac59d.png)

查询输出预览（由作者创建）

## 使用 Power BI 进行数据可视化

当前的云数据仓库使用户能够以较少的时间和成本回答关键问题。

然而，我们可以更进一步，通过创建一个可视化工具，最终用户可以用它来回答类似的问题。

实现这一目标的一个方法是使用 Power BI 创建仪表板，这是一款非常受欢迎的 BI 工具。

尽管通过可视化可以挖掘出许多指标，但仪表板将重点审查以下内容：

+   航班、延误和取消的数量总结

+   跟踪航班、延误和取消的数量随时间的变化

+   确定最常用的机场和航空公司

+   各种类型延误的分解

此外，仪表板将包括允许用户针对特定时间和地点的筛选器。

总体而言，这些功能可以结合成以下仪表板的形式：

![](img/308cfc56b7deee250c343332377e6f60.png)

完整仪表板（由作者创建）

使用这样的工具，无法访问数据或不了解 SQL 的用户可以轻松回答关键问题。

此类问题包括：

1.  **哪个航空公司在 JFK 机场的航班最多？**

![](img/2d2e3806efed2861c8e71a301dafc6a0.png)

在筛选器中选择 JFK（由作者创建）

![](img/a9e3e83b268a3acd46149109d55a135c.png)

最受欢迎的航空公司（由作者创建）

**2\. 从 2019 年到 2022 年，加利福尼亚州取消了多少航班？**

![](img/834af58042544dee974e97f1f573a303.png)

在筛选器中选择 CA（由作者创建）

![](img/2c4fc0c477c1ed76cb85323249968798.png)

取消航班数量（由作者创建）

**3\. 什么类型的延误对美国航空的总延误贡献最大？**

![](img/29a31db15d766d8c5aa2e4d16b8b58f6.png)

在筛选器中选择美国航空（由作者创建）

![](img/7cc85669d851630206e0411c59c87e0f.png)

航班延误分解（由作者创建）

## 未来步骤

![](img/eb692dd42546a11c8a2965362c021f68.png)

照片由 Anna Shvets 拍摄：[`www.pexels.com/photo/white-round-medication-pill-on-red-surface-3683087/`](https://www.pexels.com/photo/white-round-medication-pill-on-red-surface-3683087/)

当前在 AWS 和 Power BI 中的设置促进了快速且廉价的数据分析和可视化。然而，值得考虑未来数据的新应用。

1.  **整合新数据源**

如果要包括新的数据源，则需要相应地修改模式。此外，还必须创建额外的 ETL 作业，以便将这些源的数据无缝地集成到现有的数据仓库中。

**2\. 执行时间序列分析**

BTS 提供的数据是时间序列。因此，考虑使用时间序列分析并构建预测模型以预测未来的航空旅行需求是有意义的。

## 结论

![](img/15181ffe5a0bb814fb4900d263da10f8.png)

图片由 [Alexas_Fotos](https://unsplash.com/@alexas_fotos?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

记录丰富的数据集，例如 BTS 提供的数据集，可能难以管理。然而，借助 AWS 提供的资源，可以构建一个数据管道来处理数据，并将其结构化为一种形式，使用户能够以具有成本效益的方式提取见解。

此外，像创建的 Power BI 仪表板这样的可视化是对某些指标进行背景化处理并为观众创造有影响力故事的有效方法。

要获取用于在 AWS Glue 中构建 ETL 作业的代码或用于创建表格和进行分析的 SQL 查询，请访问 GitHub 仓库：

[## GitHub - anair123/Tracking-U.S.-Flights-With-AWS-and-Power-BI](https://github.com/anair123/Tracking-U.S.-Flights-With-AWS-and-Power-BI?source=post_page-----297a29fb2c13--------------------------------)

### 通过在 GitHub 上创建账户，您可以为 anair123/Tracking-U.S.-Flights-With-AWS-and-Power-BI 的开发做出贡献。

[github.com](https://github.com/anair123/Tracking-U.S.-Flights-With-AWS-and-Power-BI?source=post_page-----297a29fb2c13--------------------------------)

感谢阅读！

## 参考文献

1.  *航空公司准时统计和延误原因*。BTS。（无日期）。[`www.transtats.bts.gov/ot_delay/OT_DelayCause1.asp?20=E`](https://www.transtats.bts.gov/ot_delay/OT_DelayCause1.asp?20=E)
