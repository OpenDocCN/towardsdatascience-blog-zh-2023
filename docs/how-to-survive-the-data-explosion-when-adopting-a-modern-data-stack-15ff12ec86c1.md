# 如何在采用现代数据堆栈时应对数据爆炸

> 原文：[`towardsdatascience.com/how-to-survive-the-data-explosion-when-adopting-a-modern-data-stack-15ff12ec86c1?source=collection_archive---------12-----------------------#2023-02-03`](https://towardsdatascience.com/how-to-survive-the-data-explosion-when-adopting-a-modern-data-stack-15ff12ec86c1?source=collection_archive---------12-----------------------#2023-02-03)

## 从多个数据平台中筛选数据可能具有挑战性。以下是我们如何借助数据发现平台解决这个问题的方法。

[](https://medium.com/@veronica.fivetran?source=post_page-----15ff12ec86c1--------------------------------)![Veronica M. Zhai](https://medium.com/@veronica.fivetran?source=post_page-----15ff12ec86c1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----15ff12ec86c1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----15ff12ec86c1--------------------------------) [Veronica M. Zhai](https://medium.com/@veronica.fivetran?source=post_page-----15ff12ec86c1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd8317bbc9d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-survive-the-data-explosion-when-adopting-a-modern-data-stack-15ff12ec86c1&user=Veronica+M.+Zhai&userId=d8317bbc9d&source=post_page-d8317bbc9d----15ff12ec86c1---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----15ff12ec86c1--------------------------------) ·6 分钟阅读·2023 年 2 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F15ff12ec86c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-survive-the-data-explosion-when-adopting-a-modern-data-stack-15ff12ec86c1&user=Veronica+M.+Zhai&userId=d8317bbc9d&source=-----15ff12ec86c1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F15ff12ec86c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-survive-the-data-explosion-when-adopting-a-modern-data-stack-15ff12ec86c1&source=-----15ff12ec86c1---------------------bookmark_footer-----------)![](img/03adf9322b8b74e83c416105df72e628.png)

图片由[马克斯·温克勒](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上。

我在 Fivetran 负责分析产品和运营，Fivetran 是一家市值 56 亿美元的“互联网管道工”，在数据集成领域处于市场领先地位。在 Fivetran 之前，我在 J.P.摩根构建了第一个企业数据堆栈。

在 Fivetran，我们努力建立一个强大的数据文化。超过 90% 的员工定期使用数据工具。在如此强大的数据文化背后，存在一个普遍存在于所有规模公司中的问题，这些公司都在经历数字化转型——数据爆炸，其中数据的数量和种类变得令人难以承受。今天，我想分享一些工具和最佳实践来解决这个问题。

# 数据爆炸问题

采用现代数据栈会导致数据源、用户角色和数据资产的激增。这种爆炸是由以下原因造成的：

1.  SaaS 工具、事件流和其他生成数据的系统的激增。2021 年，平均企业部署了 187 个 SaaS 应用程序。各类组织都在努力将来自数百个来源的数据集中到一个数据库中以进行分析。

1.  现代数据管道使得将大量和多样的数据轻松地集中到一个地方变得极其简单。

1.  许多不同的人依赖于报告来进行日常操作。这些人涵盖了许多团队和所有级别的职位，从高管到个人贡献者。

1.  商业智能平台和仪表盘的激增可能使得找到正确的数据变得困难：难以确定参考哪个仪表盘；在浏览类似资产时的困惑，以及在数据模型中存在许多难以理解、定义不明确或模糊的字段、值等。

许多数据团队面临的最大痛点之一是不断收到建立更多——更多数据源、更多仪表盘和更多指标的请求。由于数据的庞大数量和多样性，他们通常很难找到合适的数据使用。

这些是跨越数字化转型鸿沟的不可避免的、非预期的后果。对于数据领导者而言，这可能使企业数据管理变得难以应对。对于业务用户而言，尽管他们经常使用一套操作仪表盘，但他们常常感到探索超出初始范围的数据令人畏惧。

# 解决方案：一个有效的地图

为了控制数据爆炸，我们需要一个帮助我们将一切重新拼凑起来并使其合理的平台。

这需要：

1.  所有数据资产的完整清单，包括表格、仪表盘和指标。

1.  每个数据资产的相关背景。这包括谁拥有这些资产，这些资产的受欢迎程度，谁使用它们最多，以及最重要的，列级数据溯源（每个字段如何与上游源和下游资产相关）。

1.  易于使用。管理企业数据已经很复杂；没有理由在数据管理过程中增加更多负担。理想的平台应该直观易用，能够正常工作。

在未来的状态下，数据消费者应能够轻松学习哪个数据资产最适合他们的日常工作以及临时的数据探索。数据团队可以轻松识别并淘汰未使用和重复的数据资产，组织和标准化数据资产，并创建易于维护的数据清单。

总之，为了解决数据资产的爆炸式增长，我们需要一个将所有数据资产集中在一个地方、提供有用数据背景并使数据资产易于搜索的数据发现平台。过去，人们称之为数据目录工具。

# 评估标准和决策过程

我们已经研究了市场上所有主要的数据发现工具。我们考虑工具选择的几个关键标准：

标准 1 — 兼容现代数据栈：我们需要一个兼容包括 BigQuery、Snowflake 和 Redshift 在内的云数据仓库、如 Looker 和 Tableau 的业务智能工具以及如 Fivetran 的管道工具的工具。

标准 2 — 价格。我们有一个预算，设定为业务智能工具总预算的不到 10%（这不包括云数据仓库费用）。

标准 3 — 易用性。这涵盖了 3 个主要方面：

+   易于设置 — 只需轻量配置即可。

+   直观易用 — 分析师和业务用户可以轻松找到他们所需的相关资产。

+   最小的持续维护开销 — 我们不想做“数据图书管理员”！

虽然大多数人认为数据发现是一个高度拥挤的领域，我们的候选工具初筛中有超过 10 个工具，但对我们来说，决策过程实际上非常简单。这可能听起来有些夸张，但让我解释一下：

+   现有的竞争者包括 Collibra、Informatica、Alation 和 Data.World，并不完全适合我们的数据栈。它们对我们的预算来说非常昂贵。它们的核心用户是 IT 人员，而不是数据分析师和业务用户。

+   我们也没有考虑像 [开源工具](https://eugeneyan.com/writing/data-discovery-platforms/) 如 Amundsen 和 DataHub，因为我们想要一个易于使用、开箱即用的解决方案。这样，我们的团队就不必花时间和资源来配置新工具。因为我们想节省时间，所以能够随时获得客户支持也很重要。

+   我们没有考虑像 Sled 这样具有垂直整合的工具，因为我们在内部不使用 Snowflake。Sled 是一个现代的数据目录工具，与 Snowflake 生态系统集成。他们在解决数据发现时的垂直方法，即深入而非广泛，是不同且有趣的：Sled 涵盖了从自动化数据质量检查到数据发现和血缘，从文档到指标层的能力。

+   我们将选择范围缩小到 Select Star 和 Atlan。我们进行了为期一个月的试用，最终选择了 Select Star。它最适合我们的用例，因为它提供的核心数据发现体验优于市场上的其他工具。

具体来说，Select Star 非常适合我们的需求，原因如下：

+   他们的价格合理。

+   我们可以轻松地与 BigQuery、Looker 和 Sigma 设置集成。借助 Select Star，我们能够在一天内启动实例并配置我们的数据源。

+   它能正常工作。我们非常明确地表示，我们不希望分析师变成“数据运维”或“数据管理员”。虽然许多工具期望数据团队进行大量配置以使其有用且可维护，但用户可以轻松找到所需的资产及其来源。

# 跨越探索鸿沟：在企业数据管理中保持领先

我们选择了一个帮助促进数据发现的工具，但接下来该做什么？无论你是像 J.P. Morgan 这样的强大企业中的数据高管，还是像 Fivetran 这样的快速增长的企业，都需要考虑数据治理的五个核心支柱，以便管理一个成功的数据组织。

**质量 —** 确保数据从源头到目的地的完整性、准确性和低延迟。

**完整性** — 保护和维护集中指标的有效性。

**分类法 —** 定义和维护关键业务指标的数据分类法。

**标准化 —** 定义和维护一致的逻辑数据模型和指标，以支持跨功能的用例。

**访问：**

+   **权限管理 —** 确保对数据的个体访问控制和一致性。

+   **发现 —** 确保数据资产易于发现。

虽然企业数据治理可能让人感到不知所措，但我将分享一些简单的行动点，帮助你入门：

+   **投资于源系统中的干净、可靠的数据** — 大多数数据质量问题来源于源系统中的不良数据。你需要投资于确保源数据的完整性和准确性的能力。

+   **投资于强大的数据管道工具 —** 构建定制化数据管道的过程费时费力、成本高昂、极其脆弱，并且需要不断维护。你的数据管道工具必须可靠且易于使用。你不希望你的数据团队变成数据管道工。该工具不应需要繁重的配置和维护来确保数据质量。

+   **利用数据发现平台或数据使用工具来组织资产 —** 你不希望你的数据团队成为数据运维或数据清理人员，但你需要设计一个过程来修剪你的数据花园。投资于与核心数据平台无缝集成的数据发现工具，或者内部构建一个数据使用工具，以便轻松淘汰未使用、过时和重复的资产。大多数商业智能工具都有使用信息。

+   **建立一个审议委员会以批准新资产**——每个组织都有用于存储资产的公共文件夹。这些公共文件夹中还包含了优先级最高的仪表板和北极星指标的金资产。公共文件夹中的新资产和金资产的更改应由审议委员会进行审查。否则，你的数据花园将会杂乱无章。

我希望这篇文章能帮助你更好地赋能你的组织，使其在大规模数字化转型中保持领先。尽管数据管理方面已经取得了重大技术进步，但我相信这仅仅是开始。
