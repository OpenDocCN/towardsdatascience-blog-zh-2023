# DataHub 实操 第二部分

> 原文：[https://towardsdatascience.com/datahub-hands-on-part-ii-169c36ee082d?source=collection_archive---------8-----------------------#2023-01-03](https://towardsdatascience.com/datahub-hands-on-part-ii-169c36ee082d?source=collection_archive---------8-----------------------#2023-01-03)

## 数据摄取、数据发现和实体链接

[](https://simon-j-preis.medium.com/?source=post_page-----169c36ee082d--------------------------------)[![教授西蒙·J·普雷斯，博士](../Images/ea0586e8d1fd6ba434fa2cfb9a71e4a5.png)](https://simon-j-preis.medium.com/?source=post_page-----169c36ee082d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----169c36ee082d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----169c36ee082d--------------------------------) [教授西蒙·J·普雷斯，博士](https://simon-j-preis.medium.com/?source=post_page-----169c36ee082d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8c9dbf8eb438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdatahub-hands-on-part-ii-169c36ee082d&user=Professor+Simon+J.+Preis%2C+Ph.D.&userId=8c9dbf8eb438&source=post_page-8c9dbf8eb438----169c36ee082d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----169c36ee082d--------------------------------) · 10 分钟阅读 · 2023年1月3日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F169c36ee082d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdatahub-hands-on-part-ii-169c36ee082d&user=Professor+Simon+J.+Preis%2C+Ph.D.&userId=8c9dbf8eb438&source=-----169c36ee082d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F169c36ee082d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdatahub-hands-on-part-ii-169c36ee082d&source=-----169c36ee082d---------------------bookmark_footer-----------)![](../Images/bba8d0b890e4cff0bc0ca4d6958243ae.png)

图片由 [Duy Pham](https://unsplash.com/@miinyuii?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

欢迎来到我的 DataHub 实操故事的第二部分！在 [上一篇](https://medium.com/towards-data-science/datahub-hands-on-part-i-f0709e7efec9) 中，我们讨论了如何从头开始设置数据目录，如何使用业务术语进行语义建模以及数据治理（DG）的动机。在本部分中，我们将更详细地了解 DataHub 的一些技术部分，以便摄取和链接元数据。

# 数据摄取

DataHub 的数据摄取（DI）部分可以通过右上角的菜单栏访问：

![](../Images/520149b88d360de658bd8d23463bead3.png)

Source: Author

一旦进入 DI 区域，我们会看到已配置的数据源列表，包括显示执行统计的概览。从这里，我们可以运行已配置的 DI 过程，可以编辑配置，可以删除或复制配置，或创建一个新源。

![](../Images/90b56e0c2c7fdb85f6fe146cb0050028.png)

Source: Author

另有一个“Secrets”按钮，可用于配置访问数据源的密码。此功能有助于将密码安全地存储在 DataHub 数据库中。在源配置过程中，可以将秘密与数据源关联。

## 重要术语

为了理解 DI 配置的工作原理，让我们快速定义一些重要术语。如上所述，*sources* 指的是元数据的来源。Sources 指我们希望在数据目录中管理的数据库——通常来自生产环境。除了 sources，DataHub 还提供了 *sinks* 的概念。Sinks 是元数据的目的地。通常，DI 引擎将收集到的元数据发送到“datahub-rest” sink，使其在 DataHub UI 中可用。但是，如果需要，也可以选择 Kafka 和文件导出。接下来是 *recipes*，这是配置文件，指示摄取脚本从哪里提取数据以及将数据放在哪里。配方可以通过 UI 配置（如果有适配器），也可以通过 YAML 脚本[1]进行配置。

## 数据源和适配器

现在，让我们看看 DataHub 提供的预配置数据库适配器（即连接器）：

![](../Images/b205b659900d7a9ff409006bb1a1a011.png)

Source: Author

我们看到一些已经建立的 SQL 和 NoSQL 技术图标。为了理解这不仅仅是为了营销目的，让我们比较两个选定的适配器。在下图的左侧，我们看到一个 BigQuery 的摄取配方，在右侧，我们看到一个 SQLServer 的配方。显然，摄取 UI 根据技术期望不同的信息来建立数据库连接。

![](../Images/4203c5c1d30308f43c3cdf33569e42e4.png)

Source: Author

大多数数据源使用基于Python的元数据摄取系统进行拉取式集成。在第一步中，元数据从源系统中拉取，在第二步中，通过HTTP或Kafka²将信息推送到DataHub服务层。每个源适配器具有不同的功能，因此建议阅读在线文档以避免混淆。例如，[MySQL](https://datahubproject.io/docs/generated/ingestion/sources/mysql)和SAP HANA适配器支持数据分析和检测已删除的实体，而MongoDB或Oracle适配器不支持这些功能。相反，[BigQuery](https://datahubproject.io/docs/generated/ingestion/sources/bigquery)适配器支持这些功能以及表级血缘关系等其他功能。要了解适配器功能之间的差异，我们只需访问github查看每个适配器的代码。例如，我们可以看到，sqlalchemy用于连接[MySQL](https://github.com/datahub-project/datahub/blob/master/metadata-ingestion/src/datahub/ingestion/source/sql/mysql.py)源，而[BigQuery](https://github.com/datahub-project/datahub/blob/master/metadata-ingestion/src/datahub/ingestion/source/bigquery_v2/bigquery.py)适配器则使用Google提供的标准连接包。因此，可以得出结论，每个适配器都是独立实现的，并可能使用不同的连接包。这一点以及数据库技术在其能提供的元数据方面的不同（例如，MongoDB中没有外键概念）对适配器的摄取能力有影响。

## 完成配方

还有一个过滤器部分，有助于缩小要摄取的模式或表。如果没有设置过滤器，DI引擎会对配置的数据库用户有权限的所有方案、表和视图执行元数据扫描——这应考虑到以避免对扫描的数据库和摄取过程造成性能影响。从安全角度来看，建议直接在数据库中限制用户权限（例如，通过创建一个特定的只读DataHub用户）。你可以在附加部分切换高级设置，例如是否应考虑视图进行扫描，或仅考虑表，或两者都考虑。同样重要的是：你可以选择是否对表或列进行分析（例如，表中有多少记录，列中有多少缺失值）。这对于了解公司数据环境的透明度非常有用。

一旦配置了配方，你可以指定执行计划。在这里，你定义了DI执行的频率（例如每日、每月、每小时）。在创建向导的最后一步，你必须为新源分配一个唯一名称，然后点击“保存并运行”，这将直接触发第一次元数据扫描过程。

![](../Images/33d47afe85804f592295aee118a04b18.png)

来源：作者

一旦过程执行完毕，你可以查看详细信息，以获得对导入资产的初步印象，或查看失败的详细信息。在我的测试用例中，我们有一个 MySQL 数据库服务器，包含来自4个数据库的78个视图和38个表。

![](../Images/65646c38886c8e330d4a00c56f8887ae.png)

来源：作者

# 数据发现

## 搜索和筛选

一旦我们的元数据被推送到 DataHub 服务层，我们可以通过 DataHub UI 开始数据发现之旅。作为90年代的粉丝，我在测试用例中使用了 Microsoft 的“northwind”数据库。如果我想搜索模式，我可以浏览表和视图，或者选择一些筛选条件（例如，排除视图）。

![](../Images/e2cb329f6fd41e8aee2fa811f4e254c0.png)

来源：作者

## 表格分析结果

假设我想更详细地查看产品数据，那么我只需点击列表中的产品数据集。我首先看到的信息是表格模式概览，显示字段、数据类型，甚至一些数据模型细节，如主键和外键。在顶部（数据集名称下方），我们还可以一目了然地看到我们的表中当前有79条记录。

![](../Images/6c0f9495accfbad08f4510f2fb69aca2.png)

来源：作者

第二个表单涉及文档，默认情况下为空。在这里，数据管理员可以在 DataHub 中添加关于该表的技术知识和/或添加指向已有材料的链接。这是促进公司数据架构成熟度的重要工具——数据目录的帮助程度仅取决于专家共享和输入的知识。数据集还有一些其他有趣的表单，如血缘和验证，我们将在第三部分中介绍——敬请关注 :)

## 列分析结果

现在，我想展示 DI 引擎提供的内容，只需在我们之前的配方中选择“列分析”选项。为此，让我们进入名为“stats”的表单。在这里，我们可以看到选定表中每一列的统计信息。

![](../Images/ac7a1f63a3a051b4ddf29b5fdf16bbf7.png)

来源：作者

例如，我们在第一行看到字段“ProductID”具有100%的唯一值。这一发现并不令人惊讶，因为这个字段是表的主键——从技术上讲，这些值在每条记录中必须不同。我们还看到 NULL 计数和 NULL 百分比信息，例如，我们看到两个产品记录没有供应商参考。这可以被视为一个关键的数据质量问题，因为没有关联的供应商，你无法实际将这些产品提供给客户。

还有什么呢？我们可以看到描述性度量，例如最小值、最大值、均值，这在记录量较多的交易表中更为有趣。至少，我们可以一眼看到有关我们产品组合的单位价格的统计度量（例如，半数产品的价格低于 $19.45 → 中位数）。此外，我们还可以看到数据集中每列的样本值。

# 将属于同一类别的内容结合起来

## 实体链接

当然，数据发现已经非常有启发性，并促进了透明度，特别是如果你从未在实践中体验过这种功能。然而，数据目录的真正力量只有在我们将IT世界和商业世界结合起来时才能释放出来。这听起来很棒——但这意味着什么呢？在这个意义上，“IT世界”指的是所有与数据集相关的技术信息。“商业世界”是我们在前一部分讨论的内容：我们在术语表中维护的所有商业信息。以现实公司为例，必须强调的是（特别是在较大的公司中），有不同的小组负责数据集和术语表。一方面，我们有数据保管员，他们是技术话题的专家，例如开发和改进数据库。另一方面，我们有数据管理员，他们是商业话题的专家，例如如何根据条件指示哪些产品可以在哪个地点生产——以及做出决定所需的数据。因此，当我们谈论链接实体时，我们实际上是在连接人员！

大词汇！那么这与DataHub如何配合呢？让我们继续讨论我们的数据集“products”，它反映了一个实际的数据库表。这是我们在DataHub中的起点。在下面的屏幕截图中，我们可以看到各种标记的信息，这些信息指向可以共同使用的多种类型的与“商业世界”的链接。

1.  选项：我们可以在表格列中添加术语表项。此链接指示某个术语的原始数据可以在该列中找到，例如实际的产品名称。

1.  选项：我们可以在整个表格中添加术语表项。此链接指示某个术语相当复杂，并由多个属性描述，这些属性可以在该表中找到。

1.  选项：我们可以添加一个数据集所属的领域。此链接类似于术语表项与领域的链接（有关更多信息，请参见第一部分）。

![](../Images/7b0e37958e78e1c7d4ea86641f1d62b1.png)

来源：作者

## 人员

然而，我们的模型中仍然没有人员。因此，让我们为数据集“products”添加一个所有者：

![](../Images/de32cdc4b1e23f4ecd45a57027f5b877.png)

来源：作者

DataHub建议这个人应该是“技术所有者”，这类似于其他数据治理框架中的“数据保管员”角色。现在我们需要将人员添加到相关的术语中。我们可以简单地点击“产品”术语，DataHub将我们导航到术语记录。在那里，我们也可以添加所有者。例如，我们可以将Brian添加为“数据管理员”，将Lilly添加为“业务所有者”。如果我们将鼠标悬停在名字上，会看到分配给该名字的角色。DataHub允许多次添加人员——如果一个人有多个角色（例如业务所有者+数据管理员），这可能会很有用。不幸的是，该工具也允许多次添加相同的人员和角色组合，这并不实用。

![](../Images/27acc92107ee5c74962be79e4c8f30cb.png)

来源：作者

现在假设Brian是公司新员工，他想在产品主数据表上做一些更改，因此他需要找到合适的IT专家来澄清实施细节。在这种情况下，他只需点击“产品”数据集，该数据集在“相关实体”部分可见（见上图）。然后他会被转到数据集（我们在上面看到过）并可以看到：我的IT联系人是John！当然，组织中需要支持或有产品数据更改请求的其他人员也可以使用这个链接信息——他们只需搜索“产品”，并只需2次点击即可收集正确的数据相关人员（Lilly、Brian和John）。

# 第二部分结论

我们已经看到，设置数据摄取过程只需几分钟，因为DataHub提供了多个已建立的数据库技术的配置向导。我们还看到，数据发现是一个强大的功能，可以提高公司数据架构的透明度。表和列的分析是自动执行的，统计结果可以在用户界面中查看。数据集和术语的链接或添加所有者也非常简单，不过，我们看到数据一致性检查并不总是由DataHub执行。我们可以多次添加相同角色的相同所有者，但该工具至少能阻止我们多次将相同术语添加到数据集中。将相同的术语分配给同一表中的多个列也是可能的——也许在实际应用中这种灵活性是有用的。总的来说，我的实践建议是——至少在一个表内——列和术语之间应保持1:1关系。如果不是这样，你应审查你的术语表的粒度，或者你的物理数据模型存在问题。

再次恭喜你读到这里！在下一部分，也是最后一部分，我们将**深入探讨**数据质量管理，包括数据溯源和数据验证。也许，我会写一个进一步的故事，讨论DataHub在数据治理（DG）角色概念方面与其他DG框架的比较。

[1] [https://datahubproject.io/docs/metadata-ingestion](https://datahubproject.io/docs/metadata-ingestion)

[2] [https://datahubproject.io/docs/architecture/metadata-ingestion](https://datahubproject.io/docs/architecture/metadata-ingestion)
