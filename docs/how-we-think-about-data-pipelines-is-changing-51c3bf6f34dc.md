# 我们对数据管道的思考正在改变

> 原文：[`towardsdatascience.com/how-we-think-about-data-pipelines-is-changing-51c3bf6f34dc`](https://towardsdatascience.com/how-we-think-about-data-pipelines-is-changing-51c3bf6f34dc)

![](img/7d79fb389159e7b29999cbc1ea81691a.png)

照片由[Ali Kazal](https://unsplash.com/@lureofadventure?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)提供，来自[Unsplash](https://unsplash.com/photos/a-mountain-range-with-trees-in-the-foreground-and-a-field-in-the-foreground-walahB6h_sU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

## 目标是可靠且高效地将数据发布到生产环境

[](https://medium.com/@hugolu87?source=post_page-----51c3bf6f34dc--------------------------------)![Hugo Lu](https://medium.com/@hugolu87?source=post_page-----51c3bf6f34dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----51c3bf6f34dc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----51c3bf6f34dc--------------------------------) [Hugo Lu](https://medium.com/@hugolu87?source=post_page-----51c3bf6f34dc--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----51c3bf6f34dc--------------------------------) ·6 分钟阅读·2023 年 11 月 8 日

--

数据管道是一系列组织成[有向无环图](https://en.wikipedia.org/wiki/Directed_acyclic_graph)或“DAG”的任务。从历史上看，这些任务是在开源工作流编排软件（如[Airflow](https://airflow.apache.org/)或[Prefect](https://www.prefect.io/?gclid=Cj0KCQjwqP2pBhDMARIsAJQ0CzoV5DrzqjyDqDJonPcBPT5lE2ih47H2LMSKBst2jh6mR6h3azCcRnwaAhOJEALw_wcB)）上运行的，并且需要由数据工程师或平台团队管理的[基础设施](https://www.bhavaniravi.com/apache-airflow/deploying-airflow-on-kubernetes)。这些数据管道通常按[时间表](https://airflow.apache.org/docs/apache-airflow/1.10.1/scheduler.html)运行，并允许数据工程师更新数据仓库或数据湖等位置的数据。

这正在发生变化。心态发生了巨大的转变。随着数据工程行业的成熟，心态正在从“不惜一切代价移动数据以服务业务”的心态转变为“可靠性和效率”/“软件工程”的心态。

## 持续数据集成和交付

我之前写过关于[数据团队发布*数据*](https://medium.com/orchestras-data-release-pipeline-blog/a-new-paradigm-for-data-continuous-data-integration-and-delivery-miniseries-part-5-a3338b3ffd03)，而软件团队发布*代码*。

这个过程被称为“持续数据集成和交付”，是可靠高效地将数据发布到生产环境的过程。与软件工程中“[CI/CD](https://aws.amazon.com/solutions/app-development/ci-cd/#:~:text=An%20integral%20part%20of%20development,with%20collaborative%20and%20automated%20processes.)”的定义存在细微差异，如下所示。

![](img/5a9b3efdc44da8184627cefa728133c0.png)

作者的图片

在软件工程中，持续交付并不容易，因为在暂存环境中需要有一个[几乎完全相同的副本](https://www.techtarget.com/searchsoftwarequality/definition/staging-environment#:~:text=A%20staging%20environment%20(stage)%20is,like%20environment%20before%20application%20deployment.)来运行代码。

在数据工程中，这是不必要的，因为我们发布的是*数据*。如果有一张数据表，并且我们*知道*只要满足一些条件，数据就*足够优质*可以使用，那么这就足够让它被“发布”到生产环境中。

将数据发布到生产环境的过程——持续交付的类比——非常简单，因为它只涉及复制或[克隆](https://docs.snowflake.com/en/sql-reference/sql/create-clone)数据集。

此外，数据工程的一个*关键支柱*是对新数据的*及时反应*，或者检查新数据是否存在。在软件工程中没有类似的情况——软件应用程序不需要轮询 API 以检查新代码的存在，而数据应用程序需要。

鉴于数据中持续交付的类比如此微不足道，我们可以宽泛地定义持续数据集成为可靠高效地响应代码更改将数据发布到生产环境的过程。通过克隆、实体化视图和运行测试来“持续集成”控制数据状态的代码更改。

我们也可以宽泛地定义持续数据交付为可靠高效地将*新数据发布到生产环境*的过程。这包括对新数据的存在进行管道调用或操作。

将这两个过程视为相同类型的操作但在不同的上下文中，这与大多数数据团队对数据管道或*数据发布管道*的思考方式有了相当大的改变。

# 额外的考虑因素

这里有很多额外的考虑因素，这是因为除了仅仅在生产环境中发布数据之外，还有*很多要考虑的东西*。

数据并非静态的。它不仅仅存在于可以操作的地方。它分散在组织中的各个地方。它在工具之间移动。它以不同的频率到达，只有经过“ELT”的繁琐过程后，它才最终到达数据湖或数据仓库。

此外，Github 操作并不足以支持所有这些工作。也许作为编排层，但肯定不适用于进行大量计算和数据管理。

这些因素导致了许多额外的考虑，关于如何设计一个能够提供持续数据集成和交付的系统，我在这里讨论

## 用户界面

拥有一个单一的用户界面来查看数据部署是关键的。只使用多个云数据提供商的用户界面进行数据运营的数据团队，在聚合元数据以进行有效的数据运营，以及[BizFinOps](https://aws.amazon.com/blogs/enterprise-strategy/introducing-finops-excuse-me-devsecfinbizops/)方面将会损失。

还有要聚合的数据部署，通常是由于：

+   新数据到达

+   改变数据实现逻辑

目前使用工作流编排工具和 GitHub 操作或类似工具来处理这些。这会造成不连贯性 — 数据团队需要检查多个工具，以了解数据表何时已更新，它们的定义是什么等等。当然，你可以购买一个可观察性工具。然而，这又是*另一个用户界面，另一个工具，另一个成本*。

拥有一个真正的单一操作面板，用于编排、可观察性和某种运维功能，将是一个杀手级功能，我很想在 Codat 使用，我们在那里整合了一整套开源和闭源供应商 SAAS 工具。

## 可观察性或元数据收集

我在前一节中提到了这一点，但可观察性和元数据收集对于强大的数据管道至关重要。

这里重要的一点，我认为可观察性平台忽略了的是将观察放在管道本身。否则，向数据工程师呈现元数据、诸如“这个失败了”或“那个表已过时”的信息是不可取的，因为它们是 A）事后（为时已晚）和 B）与管道运行无关。当然 —— 一个表是坏的。但它是因为某人刚刚推送了一个更改还是因为有新数据到达？

最近我参加了[安德鲁·琼斯](https://andrew-jones.com/categories/data-contracts/)的另一个精彩演讲，他谈到了 1、10、100 金字塔。

![](img/c9e90125d2e5e686ec0628f83a9afc09.png)

预防成本为 1 美元，缓解成本为 10 美元，一旦为时已晚，成本为 100 美元。图片由安德鲁·琼斯提供，获得许可后发布。

+   **预防成本** — 在数据提取点预防错误将花费你 1 美元。

+   **更正成本** — 在提取后有人纠正错误将花费你 10 美元。

+   **失败成本** — 让错误数据通过流程到达最终位置将花费你 100 美元。

可观察性工具处于黄色到红色区域。如果它们设置在你的生产数据库上，很可能你正在与时间赛跑，以在有人意识到之前解决问题。

分析师使用 hacky SQL 修复错误数据位于黄色区域。

带有可观察性的编排位于绿色和黄色之间。如果您的所有数据管道都可以访问所有元数据，并且可以在您实现和更新表和视图时执行数据质量测试，那么当这些测试失败时，您可以随时暂停管道。这意味着[*不会有错误数据进入生产环境*](https://medium.com/snowflake/avoid-bad-data-completely-continuous-delivery-architectures-in-the-modern-data-stack-part-1-22a0d48935f6)*。*

这非常强大，这就是为什么我相信，拥有一个具有对细粒度元数据访问权限的编排工具是未来的发展方向（我创办的初创公司正在做这个）。

# 总结

我们正在摆脱数据不可知的工作流编排工具加上不稳定或不存在的持续集成，转向统一的持续*数据*集成和交付。

将会有平台使数据团队能够对数据集进行完整、可靠和高效的版本控制，并拥有坚固的数据管道。这些平台将内置可观察性功能，虽然许多平台尚未实现端到端，但肯定感觉事情正在朝着这个方向发展。

有许多成熟的数据工具正在做这个。例如，对于数据仓库的持续集成和交付，Y42 拥有“虚拟数据构建”（Virtual Data Builds）的[概念](https://www.y42.com/blog/virtual-data-builds-one-data-warehouse-environment-for-every-git-commit/)，基本上与本文的第一部分相同。对于数据湖环境，Lake FS / Treeverse 的 Einat Orr 最近在[LinkedIn 上发表了文章](https://www.linkedin.com/feed/update/urn:li:activity:7126949272050106369/?commentUrn=urn%3Ali%3Acomment%3A%28activity%3A7126949272050106369%2C7126969193651924992%29&dashCommentUrn=urn%3Ali%3Afsd_comment%3A%287126969193651924992%2Curn%3Ali%3Aactivity%3A7126949272050106369%29&dashReplyUrn=urn%3Ali%3Afsd_comment%3A%287126970713164439552%2Curn%3Ali%3Aactivity%3A7126949272050106369%29&replyUrn=urn%3Ali%3Acomment%3A%28activity%3A7126949272050106369%2C7126970713164439552%29）- 他们所做的在功能上非常相似，并且在 Snowflake 中很容易实现（[我在这里写了一篇文章](https://medium.com/snowflake/why-snowflakes-clone-command-changes-the-game-for-ci-cd-in-data-ccb6fb9955ba)）。SQLMesh 的“企业”版本（不要相信开源的[快乐](https://medium.com/@hugolu87/y-combinator-had-282-companies-this-winter-and-c-50-were-open-source-sunday-scaries-128e53318454)，这是一家风险投资支持的企业。他们会试图赚钱，就像我们所有人一样）具有内置的可观察性和版本控制功能，非常酷。

你当然可以使用类似 Airflow 这样的工具来完成*所有这些*。该死的，如果你愿意，你甚至可以用 Airflow 煮早晨的咖啡。我想问题是——你有时间、耐心和专业知识来编写所有这些代码吗？还是像我一样，只是想把事情做好？🐠
