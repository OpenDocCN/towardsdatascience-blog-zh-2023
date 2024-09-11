# 如何使用多功能数据工具（Versatile Data Kit）跟踪数据版本

> 原文：[https://towardsdatascience.com/how-to-keep-track-of-data-versions-using-versatile-data-kit-f1916f18737e?source=collection_archive---------9-----------------------#2023-05-03](https://towardsdatascience.com/how-to-keep-track-of-data-versions-using-versatile-data-kit-f1916f18737e?source=collection_archive---------9-----------------------#2023-05-03)

## 数据工程

## 学习关于慢变维度（SCD）以及如何在VDK中实现SCD Type 2

[](https://alod83.medium.com/?source=post_page-----f1916f18737e--------------------------------)[![Angelica Lo Duca](../Images/45aa2e2e504bb3af6d3b8009dc6f030e.png)](https://alod83.medium.com/?source=post_page-----f1916f18737e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f1916f18737e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f1916f18737e--------------------------------) [Angelica Lo Duca](https://alod83.medium.com/?source=post_page-----f1916f18737e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8bc34d63aee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-keep-track-of-data-versions-using-versatile-data-kit-f1916f18737e&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=post_page-f8bc34d63aee----f1916f18737e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1916f18737e--------------------------------) ·7 分钟阅读·2023年5月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff1916f18737e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-keep-track-of-data-versions-using-versatile-data-kit-f1916f18737e&user=Angelica+Lo+Duca&userId=f8bc34d63aee&source=-----f1916f18737e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff1916f18737e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-keep-track-of-data-versions-using-versatile-data-kit-f1916f18737e&source=-----f1916f18737e---------------------bookmark_footer-----------)![](../Images/1bc2772471e53b03eea61d2d5f4cbff2.png)

照片由[Joshua Sortino](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)拍摄

数据是任何组织的支柱，在当今快节奏的世界中，跟踪数据版本至关重要。随着企业的成长和发展，数据经历了许多变化，如果没有流畅的系统，这些变化很快就会变得令人不知所措。技术通过各种工具来帮助我们有效地管理数据。

其中一个工具是 [Versatile Data Kit](https://github.com/vmware/versatile-data-kit) (VDK)，它为控制数据版本需求提供了全面解决方案。**VDK 可帮助您轻松执行复杂操作，例如使用 SQL 或 Python 从不同来源进行数据摄取和处理**。您可以使用 VDK 构建 **数据湖** 并摄取从不同来源提取的原始数据，包括结构化、半结构化和非结构化数据。

在本文中，我们将探讨 VDK 如何通过提供直观界面和强大功能来简化您的生活，以跟踪对关键业务信息所做的所有更改。

我们将涵盖以下主题：

+   什么是缓慢变化的维度（SCD）？

+   SCD 类型

+   如何在 VDK 中实现 SCD 类型 2。

***图片注意****：所有图片均为作者所有，除非另有明确说明。*

# 什么是缓慢变化的维度（SCD）？

在数据仓库中，*维度* 是一种结构，用于分类事实和度量，以便用户回答业务问题。常用的维度有人员、产品、地点和时间。

**缓慢变化的维度**（SCD）帮助您跟踪维度数据随时间的变化。它们在数据仓库中存储和管理当前和历史数据。*缓慢* 意味着它们随时间变化较慢而不是定期变化。SCD 的例子包括地址、电子邮件、工资等。

SCD 对于跟踪数据随时间变化非常重要。我们可以使用它们来跟踪客户、产品或其他可能随时间变化的信息。

使用 SCD 可以更轻松地跟踪数据变更并保留数据如何变化的历史记录。这对分析趋势或回答关于特定数据如何演变的问题非常有价值。

# SCD 类型

有三种类型的 SCD：类型 1、类型 2 和类型 3。为了说明每种 SCD 类型的工作原理，我们使用以下示例。考虑以下客户维度表：

![](../Images/c91932ad2ac4e033722a1cde2b8686dc.png)

图 1 — 示例中的客户维度表

如果 John Smith 把他的电话号码改成 **555-5668**，会发生什么？

## SCD 类型 1

这种 SCD 类型不追踪数据变更；新数据会覆盖旧数据。当数据历史不重要且需要了解当前数据状态时，这种类型是合适的。类型 1 是最简单和最常见的 SCD 类型。

考虑图 1 中的示例。在 SCD 类型 1 中，新电话号码替换了旧电话号码，而且没有记录之前的电话号码，如下表所示。

![](../Images/f97ac854a4c7965ac6e5b4211415e0f1.png)

图 2 — 应用 SCD 类型 1 后的客户维度表

## SCD 类型 2

这种类型通过向维度表添加新记录来跟踪历史更改。旧数据仍然可用，但被新数据标记为废弃。每条记录包含唯一标识符、开始日期和结束日期。

再考虑图 1 中的示例。在 SCD 类型 2 中，创建一个新的记录，包含新的电话号码、唯一标识符和开始日期，同时前一个记录有一个结束日期。开始日期表示新记录生效的日期，结束日期表示旧记录过时的日期。

![](../Images/f9c34df9f1448f4ff8bd41d14c19da0e.png)

图 3— 应用 SCD 类型 2 后的客户维度表

## SCD 类型 3

这种类型通过向维度表中添加一个列以保存新值，仅跟踪最新的更改。当跟踪完整的更改历史记录不是必要时，使用此类型。

再考虑图 1 中的示例。在 SCD 类型 3 中，记录被更新为两个新列：`new_phone_number` 用于保存新值，`effective_date`。

![](../Images/d3d7ef32a0a38af3e7bc48ac4edd9ba1.png)

图 4— 应用 SCD 类型 3 后的客户维度表

# 如何在 VDK 中实现 SCD 类型 2

VDK 是由 VMware 作为开源发布的一个非常强大的框架。使用 VDK 构建数据湖并合并多个数据源。如果你是 VDK 的新手，可以阅读其 [官方文档](https://github.com/vmware/versatile-data-kit) 或这篇 [介绍文章](/an-overview-of-versatile-data-kit-a812cfb26de7)。

VDK 支持 SCD 类型 1 和类型 2。要在 VDK 中实现 SCD，请使用 [SQL 处理模板](https://github.com/vmware/versatile-data-kit/wiki/SQL-Data-Processing-templates-examples#sql-processing-templates)。SQL 处理模板是数据加载模板，是 VDK 提供的一个概念结构。根据 VDK 官方文档，

> 数据加载模板从位于源模式的 source_view 中提取数据，并将源数据加载到位于目标模式的 target_table 中（摘自 VDK 官方文档）。

实际操作中，模板简化了从源中提取数据并加载到目标表中的过程。VDK 提供了适用于 Impala 和 Trino 数据库的 SQL 数据处理模板。

为了说明 VDK 如何管理 SCD 类型 2，请考虑以下场景。

![](../Images/aa97ba7f239a36475234878d917e9520.png)

图 5 — 如何使用 VDK 管理 SCD 类型 2

从左开始，有 *数据源*（例如图 1 中的客户维度表），必须定义一个源模式和源视图。*VDK* 从 *数据源* 中摄取数据。为了管理 SCD 类型 2，VDK 使用 *SCD2 模板*，这是一个 SQL 处理模板。通过 *vdk-trino 插件*，VDK 使用 *Trino DB* 将数据存储到 *数据湖* 中。*数据湖* 必须包含 *目标模式*，即存储 SCD 类型 2 的模式。

VDK 实现了 *SCD2 模板* 作为 Input Job 的附加方法。以下代码展示了如何使用 *SCD2 模板*：

```py
def run(job_input: IJobInput) -> None:
    # ...
    job_input.execute_template(
        template_name='scd2',
        template_args={
            'source_schema': 'customer_schema',
            'source_view': 'customer_view',
            'target_schema': 'customer_target_schema',
            'target_table': 'customer_target_table',
            'id_column': 'customer_id',
            'sk_column': 'SID',
            'value_columns': ['name', 'address', 'phone_number'],
            'tracked_columns': ['phone_number'],
        },
    )
```

代码直接摘自[VDK文档](https://github.com/vmware/versatile-data-kit/tree/main/projects/vdk-plugins/vdk-trino/src/vdk/plugin/trino/templates/load/dimension/scd2)，并根据图3中的示例进行了调整，因此请参阅它以获取更多详细信息。实际上，该模板将源和目标模式作为输入，以及其他参数，例如列ID和要跟踪的列。

多亏了VDK，你可以轻松管理数据库中的SCD！有关完整而详细的示例，请参阅[VDK文档](https://github.com/vmware/versatile-data-kit/wiki/SQL-Data-Processing-templates-examples#versioned-strategy--slowly-changing-dimension-type-2)。

为了解释SCD2模板的工作原理，请再次考虑客户维度表。之前的代码仅跟踪`phone_number`列，因此如果该列发生变化，系统会将更改存储在新行中，如下图所示：

![](../Images/f4cf276dd035a2886cc1dece36e90f1b.png)

图6——如果跟踪的列发生变化，系统会将其存储为新行。

然而，如果未跟踪的列发生变化，系统会覆盖它，如下图所示：

![](../Images/4187434ed068f17fe006a078ba9a71a3.png)

图7——如果未跟踪的列发生变化，系统会覆盖它。

# 总结

恭喜！你刚刚了解了SCD是什么以及如何在VDK中实现它。

SCD对希望有效管理和分析数据的组织非常重要。通过识别数据集中哪些属性很少或不经常变化，你可以简化数据处理和存储，减少分析中错误或不一致的可能性。

要轻松识别和管理SCD，你可以使用VDK，它帮助你自动化慢变属性的变化。

总体而言，SCD 在大数据管理的宏观框架中可能看起来是一个微小的细节。但它可以显著影响分析过程的准确性和效率。通过利用像VDK这样的工具，你可以保持领先并最大化数据资产的价值。

# 你可能还感兴趣……

有许多相关主题可能会引起你的兴趣：

## 如何配置VDK与Trino DB一起使用

使用[完整示例](/from-raw-data-to-a-cleaned-database-a-deep-dive-into-versatile-data-kit-ab5fd992a02e)，展示了如何使用Versatile Data Kit和Trino DB。

## 如何使用VDK构建Web应用

[逐步教程](/how-to-build-a-web-app-with-data-ingested-through-versatile-data-kit-ddae43b5f62d)，讲解如何构建Web应用，结合Streamlit Python库和Versatile Data Kit。

## 如何使用VDK插件

[逐步教程](/how-to-create-a-data-formatting-plugin-in-vdk-dc5f1c7d206d)，演示如何通过编写VDK自定义插件来操作数据湖中的表。

## 如何处理缺失值

[教程](/handling-missing-values-in-versatile-data-kit-bb4f2a907b9c)，讲解如何使用VDK构建数据管道以处理缺失值

# 只再说一词……

不要忘记加入 VDK Slack 频道，以获取关于 VDK 的最新信息！

+   加入 [CNCF Slack 工作空间](https://communityinviter.com/apps/cloud-native/cncf)。

+   加入 [#versatile-data-kit](https://cloud-native.slack.com/archives/C033PSLKCPR) 频道。
