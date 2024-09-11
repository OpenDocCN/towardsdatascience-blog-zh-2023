# dbt Core、Snowflake 和 GitHub Actions：数据工程师的个人项目

> 原文：[https://towardsdatascience.com/dbt-core-snowflake-and-github-actions-pet-project-for-data-engineers-815991a48b44?source=collection_archive---------7-----------------------#2023-12-01](https://towardsdatascience.com/dbt-core-snowflake-and-github-actions-pet-project-for-data-engineers-815991a48b44?source=collection_archive---------7-----------------------#2023-12-01)

## 数据/分析工程师的个人项目：探索现代数据堆栈工具——dbt Core、Snowflake、Fivetran 和 GitHub Actions。

[](https://medium.com/@kategera6?source=post_page-----815991a48b44--------------------------------)[![Kateryna Herashchenko](../Images/dd6018e0f3ffb6d4fecd8cb72100282c.png)](https://medium.com/@kategera6?source=post_page-----815991a48b44--------------------------------)[](https://towardsdatascience.com/?source=post_page-----815991a48b44--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----815991a48b44--------------------------------) [Kateryna Herashchenko](https://medium.com/@kategera6?source=post_page-----815991a48b44--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4fc94e2ed685&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdbt-core-snowflake-and-github-actions-pet-project-for-data-engineers-815991a48b44&user=Kateryna+Herashchenko&userId=4fc94e2ed685&source=post_page-4fc94e2ed685----815991a48b44---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----815991a48b44--------------------------------) ·6 分钟阅读·2023年12月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F815991a48b44&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdbt-core-snowflake-and-github-actions-pet-project-for-data-engineers-815991a48b44&user=Kateryna+Herashchenko&userId=4fc94e2ed685&source=-----815991a48b44---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F815991a48b44&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdbt-core-snowflake-and-github-actions-pet-project-for-data-engineers-815991a48b44&source=-----815991a48b44---------------------bookmark_footer-----------)![](../Images/c550e2d6ed787a2bf6243b3e727599ff.png)

图片由 [Gaining Visuals](https://unsplash.com/@gainingvisuals?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是一个简单且快速的宠物项目，适合希望尝试现代数据栈工具的 Data/Analytics 工程师，包括 dbt Core、Snowflake、Fivetran 和 GitHub Actions。通过这个实践经验，你可以开发一个端到端的数据生命周期，从从你的 Google Calendar 提取数据到在 Snowflake 分析仪表板中展示数据。在本文中，我将带你了解这个项目，并分享一些见解和技巧。[*查看 GitHub 仓库*](https://github.com/KHerashchenko/SurfalyticsWorkshop/tree/master)

# 技术概述

项目架构如下所示：

**Google Calendar** -> **Fivetran** -> **Snowflake** -> **dbt** -> **Snowflake Dashboard**，由**GitHub Actions**协调部署。

![](../Images/8ff05646a8b0204831aa105f5ef64cda.png)

架构

参考 Joe Reis 的《数据工程基础》，让我们根据数据生命周期的定义阶段来审视我们的项目：

![](../Images/f1733e478c42fe23b00c1fa9d7338888.png)

数据工程生命周期 [1]

+   **数据生成 — *Google Calendar, Fivetran*** 如果你是 Google Calendar 用户，你可能已经积累了大量的数据。现在，你可以通过利用 Fivetran 这样的“数据移动平台”轻松从你的账户中检索这些数据。该工具自动化了 ELT（提取、加载、转换）过程，将数据从 Google Calendar 源系统集成到我们的 Snowflake 数据仓库中。

    目前，Fivetran 提供 14 天的免费试用 — [链接](https://fivetran.com/docs/getting-started/free-trials/account-trial)。注册非常简单。

+   **存储 — *Snowflake*** Snowflake 是一个针对分析需求的云数据仓库，将作为我们的数据存储解决方案。我们处理的数据量较小，因此不会过度使用数据分区、时间旅行、Snowpark 和其他 Snowflake 高级功能。然而，我们会特别关注访问控制（这将用于 dbt 访问）。你需要 [设置试用账户](https://signup.snowflake.com/)，这将为你提供 30 天的免费使用和 400$ 的企业版额度。

+   **数据摄取 — *Fivetran*** 数据摄取可以通过 Fivetran 和 Snowflake 的 Partner Connect 功能进行配置。选择你喜欢的方法并设置 Google Calendar 连接器。初始同步后，你可以通过 Snowflake UI 访问你的数据。你可以访问连接器网页查看模式图 [这里](https://www.fivetran.com/connectors/google-calendar)。

    将专门为 Fivetran 同步创建一个新数据库，以及相应的仓库以运行 SQL 工作负载。你应该知道，Snowflake 采用了解耦的存储和计算，因此费用也是分开的。作为最佳实践，你应该为不同的工作负载（如临时、同步、BI 分析）或不同的环境（如开发、生产）使用不同的仓库。

![](../Images/f431365d1580340e358dcf4cd9e149a9.png)

转到 Partner Connect 以连接 Fivetran。

![](../Images/7a112d493f9447f09ce99c6536c95c72.png)

在 Fivetran 中设置 Google Calendar 连接器。

![](../Images/38a2542d3dac0d8066fd9bdccd10fae0.png)

同步的数据会出现在 Snowflake 中。

+   **Transformation — *dbt Core*** 数据存储在 Snowflake 中（默认每 6 小时自动同步），我们使用 dbt Core 进入变换阶段。dbt（数据构建工具）有助于 SQL 查询的模块化，使 SQL 工作流能够重用和版本控制，就像软件代码通常被管理一样。有两种方式可以访问 dbt：dbt Cloud 和 dbt Core。dbt Cloud 是一个付费的云版本服务，而 dbt Core 是一个提供所有功能的 Python 包，你可以免费使用。

    在你的机器上安装 dbt Core，通过 CLI 使用**“dbt init”**命令初始化项目，并设置 Snowflake 连接。请查看我的 [profiles.yml 文件](https://github.com/KHerashchenko/SurfalyticsWorkshop/blob/master/dbt_hol/profiles.yml)。

    为了能够连接到 Snowflake，我们还需要运行一系列 DCL（数据控制语言）SQL 命令，详细信息请见 [这里](https://github.com/KHerashchenko/SurfalyticsWorkshop/blob/master/snowflake_setup.sql)。遵循最小权限原则，我们将为 dbt 创建一个单独的用户，并仅授予其对源数据库（Fivetran 同步数据的位置）、开发数据库和生产数据库的访问权限（我们将在这些数据库中接收转换后的数据）。

+   按照 [最佳实践结构化方法](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview)，你需要创建三个文件夹，代表数据转换的 *staging、intermediate 和 marts 层*。在这里你可以试验你的模型，也可以复制我的 [示例](https://github.com/KHerashchenko/SurfalyticsWorkshop/tree/master/dbt_hol/models) 来处理 Google Calendar 数据。

    在仓库中，你会找到列出 Google Calendar 架构中所有表的 “sources.yml” 文件。创建了 3 个 staging 模型（event.sql，

    包括 1 个变换模型（utc_event.sql）和 1 个数据集市模型（event_attendee_summary.sql）在内的 attendee.sql 和 recurrence.sql。

    dbt 的重要功能是 [Jinja 和宏](https://courses.getdbt.com/courses/jinja-macros-packages)，你可以将其编织到 SQL 中，从而增强其影响力和可重用性。

![](../Images/875c9de7d5e6d260a7ea70df5ce0a13c.png)

选择不同类型的模型物化。

+   使用 dbt 中的通用或单一测试设置数据期望，以确保数据质量。一些数据质量规则位于 “source.yml” 文件中，

    以及 “/tests” 文件夹中。在**“dbt build”**命令期间，这些数据质量检查将与模型构建一起运行，以防止数据损坏。

![](../Images/4c204eaf6b005f942f81ca4c07541544.png)

你可以使用 “dbt test” 运行测试。

+   探索dbt的快照功能，用于变更数据捕获和类型2缓慢变化维度。在我们的[示例](https://github.com/KHerashchenko/SurfalyticsWorkshop/blob/master/dbt_hol/snapshots/recurrence_snapshot.sql)中，我们捕获了“recurrence”表中的变化。

![](../Images/e0da53b1e6955a9e2b81b1e635462f0f.png)

将快照存储在单独的模式中

+   使用**“dbt docs generate”**命令生成dbt文档可能需要一些时间。你将看到从项目中自动创建的数据血缘图和元数据。你可以通过在你的 .yml 文件中添加对数据实体的描述来进一步增强它。

    良好的文档提供了更好的数据发现性和治理。

![](../Images/4f4e168ba3ead8b7e73fb3217d083382.png)

运行 `dbt docs serve` 以在浏览器中打开它。

+   **服务 — *Snowflake仪表板*** 最后，使用Snowflake仪表板可视化你的转换数据。在Snowflake UI中创建一个仪表板，并根据你的SQL查询尝试使用图块（绘图）。

![](../Images/266ccc344bbcbe911847257bee354a3a.png)

仪表板示例

+   **部署 — *GitHub Actions*** 虽然dbt Cloud提供了一个简单的部署选项，但我们将使用GitHub Actions工作流来进行我们的dbt Core项目。你需要创建一个[workflow .yml 文件](https://github.com/KHerashchenko/SurfalyticsWorkshop/blob/master/.github/workflows/dbt_prod.yaml)，该文件将在对GitHub dbt repo进行更改时触发，运行指定的操作。在我的示例工作流中，你可以看到一个两步的部署过程：**dbt build**用于开发环境，成功后还会执行**dbt

    **用于生产环境的build**。

    注意：将Snowflake账户和密码等机密替换为GitHub机密。为此，请在你的repo网页上，转到Settings -> Secrets and Variables -> Actions。

    每次“master”分支更新时，你可以在repo的Actions标签中看到工作流的启动情况：

![](../Images/129492b139aaeea10506b6b16fd85bf2.png)

查看Actions结果

在这个项目中，我们仅仅触及了现代数据工程栈中各种技术的表面。这不仅是一个实际的成就，也是深入探索的绝佳起点。感谢阅读，祝编程愉快！

**如果你读到了文章的末尾，也许你觉得它很有价值，并且可能想通过** [**LinkedIn**](https://www.linkedin.com/in/kategera6/)**与我联系。*我对机会持开放态度！***

*除非另有说明，否则所有图片均为作者提供。*

参考文献：

[1] Reis, J. (2022). 《数据工程基础：计划和构建强大的数据系统》。O'Reilly Media。
