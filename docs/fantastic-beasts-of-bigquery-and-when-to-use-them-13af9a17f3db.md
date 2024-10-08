# BigQuery 的神奇生物及其使用时机

> 原文：[`towardsdatascience.com/fantastic-beasts-of-bigquery-and-when-to-use-them-13af9a17f3db`](https://towardsdatascience.com/fantastic-beasts-of-bigquery-and-when-to-use-them-13af9a17f3db)

## 揭示 BigQuery Studio、DataFrames、生成 AI/AI 函数和 DuetAI 的特点

[](https://medium.com/@martosi?source=post_page-----13af9a17f3db--------------------------------)![Marina Tosic](https://medium.com/@martosi?source=post_page-----13af9a17f3db--------------------------------)[](https://towardsdatascience.com/?source=post_page-----13af9a17f3db--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----13af9a17f3db--------------------------------) [Marina Tosic](https://medium.com/@martosi?source=post_page-----13af9a17f3db--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----13af9a17f3db--------------------------------) ·8 分钟阅读·2023 年 12 月 31 日

--

![](img/21d378b95769bcc96f6dcb557570b5a2.png)

“BigQuery 是一个集数据库、商业智能、机器学习和生成 AI 功能于一体的 Google 服务。” [照片由 [Korng Sok](https://unsplash.com/@korng_sok?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)]

# 了解更多关于 BigQuery 的内容

我最喜欢的书之一是 [J.K. 罗琳](https://en.wikipedia.org/wiki/J._K._Rowling) 的“[神奇动物在哪里](https://en.wikipedia.org/wiki/Fantastic_Beasts_and_Where_to_Find_Them)”。这是一个关于魔法生物在非魔法世界中肆意活动的故事。它还讲述了*魔法师*和*非魔法师*如何建立友谊以保护魔法生物。在这个任务中，主要的*非魔法师*角色发现了一个充满魔法的世界，并爱上了所有的挑战，渴望自己也是一名巫师。

作为一个*非魔法师*，[我从机械工程转向数据世界的过程最初充满了挑战](https://medium.com/towards-data-science/ending-the-year-with-12-lessons-about-data-career-8786afc068f4)。每当我进入一个新的数据领域时，我都会想：*“要是我也是个巫师就好了。”* ;)

当我第一次开始学习关于***数据库 (DB) 和商业智能 (BI)*** 的知识时，我脑海中也有这个想法。

当我深入学习***机器学习 (ML)*** 相关主题时，这个想法再次出现。

现在，我正试图在***生成 AI (GenAI) 开发***领域中运筹帷幄——你猜对了，这个想法再次陪伴着我。

即使在获得了数据库、商业智能和机器学习的经验之后，如果没有一个 Google 服务——[**BigQuery**](https://cloud.google.com/bigquery/docs/introduction) **(BQ)**，生成 AI 领域对我来说将更加具有挑战性。

***你知道为什么吗？***

因为 BigQuery 提供了**“全能型”**解决方案，涵盖了**“DB-BI-ML-GenAI”**组合。或者，正如 Google 在其某次网络研讨会中所宣布的，它涵盖了“*从数据到 AI*”的功能 [[1](https://cloudonair.withgoogle.com/events/cloud-onboard-data-to-ai)]。

我认为应该如何宣布：***“BigQuery 的奇妙生物”***。

在我最喜欢的 BigQuery 特性——[BQML](https://cloud.google.com/bigquery/docs/bqml-introduction)——的基础上，Google 最近实现了更多变革性特性，使 BQ 更类似于分析开发环境，而不只是数据库环境。

这些新特性使数据专业人员能够进行端到端的分析任务**无需在多个工具之间切换**。

关于端到端任务，我想到的是执行 [探索性数据分析](https://en.wikipedia.org/wiki/Exploratory_data_analysis)（EDA）、使用 SQL 或 Python 与 Spark 进行预测建模，以及通过使用生成性 AI 特性来创建新的见解。所有这些现在都可以在代码共同创建功能的帮助下完成。

BigQuery 生态系统的最新演变激励我写这篇文章，并展示将改变我们数据专业人员工作方式的新 BQ 进展，可能让我们感觉有点像*Maj* 人。;)

> 换句话说，本文**旨在展示**新 BQ 特性何时可以在分析工作流程中使用。

但在我们开始解释之前，我需要分享这些“奇妙生物”的名称：

1.  [***BigQuery Studio***](https://cloud.google.com/bigquery/docs/query-overview#bigquery-studio) ***和*** [***BigQuery DataFrames***](https://cloud.google.com/python/docs/reference/bigframes/latest)

1.  ***BigQuery GenAI 和 AI 函数***

1.  [***DuetAI in BigQuery***](https://cloud.google.com/bigquery/docs/write-sql-duet-ai)

现在让我们开始揭示它们的独特特征，并指出如何利用它们来提升你的表现。

# **BigQuery 的奇妙生物**

为了暗示新 BQ 特性的目的，我制作了一个图示，展示了它们如何与知识数据发现过程（分析工作流程）对齐。

![](img/4e7ab1afb1a3c9c0b4b75a8d5c46e5ee.png)

新的 BigQuery 特性与分析工作流程对齐 [图像由作者使用 [draw.io](https://www.drawio.com/)]

从图中可以看出，在分析工作流程的基础上是*DuetAI*，这是一个 AI 编码辅助功能。除了编码支持，DuetAI 还是一个聊天机器人，你可以用它进行头脑风暴。

这意味着数据专业人员可以向聊天机器人提出与***输入***问题定义相关的不同问题（例如，我如何对数据集进行子集化或能否解释一个特定的函数）以及关于如何构建分析***输出***的建议（例如，我如何展示我的发现）。

在分析*输入 → 输出* 流程之间，其他特性也派上用场：

+   在 ***第一阶段***，即 **数据准备和理解阶段**，可以通过 BigQuery Studio 使用 BigQuery SQL（用于数据子集和处理）、GenAI/AI 函数（用于丰富数据集）、BigQuery DataFrames 和其他 Python 库（用于探索数据集）。

+   在 ***第二阶段***，即 ***数据建模和洞察综合阶段***，可以在 BQ Studio 中将 BigQuery SQL 或 BQML 函数与 BigQuery DataFrames 一起使用（用于 BI/ML 模型创建），以获取所需的分析结果（描述性或预测性结果）。

现在我们将展示这些功能的神奇特性。

## BigQuery Studio 和 DataFrames

在这里不要混淆 [BigQuery Studio](https://cloud.google.com/bigquery/docs/query-overview#bigquery-studio) 和 [Looker Studio](https://cloud.google.com/looker-studio?hl=en)（前身为 Data Studio）。后者是一个自助式商业智能工具，而前者是一个新的协作工作空间，支持完整的分析工作流。

综上所述，BigQuery Studio 具有以下主要特性 [[2](https://cloud.google.com/blog/products/data-analytics/announcing-bigquery-studio)]：

***#1: 在统一的界面中支持多种语言和工具。***

我的意思是，它简化了不同数据职业之间的工作，因为：

+   *数据工程*（数据摄取和数据处理），

+   *数据分析*（描述性统计/探索性数据分析），以及

+   *数据科学* 任务（预测建模）可以在一个环境中完成，或者更好的是，在一个 **笔记本** 中完成。

BQ Studio 提供了 [Colab 界面](https://cloud.google.com/colab/docs/introduction)，使数据专业人员能够在一个笔记本文件中使用 SQL、Python、Spark 或自然语言进行 BigQuery 分析（与 DuetAI 结合使用）。此外，开发的笔记本可以通过 [Vertex AI](https://cloud.google.com/vertex-ai?hl=en) 访问，以进行机器学习工作流。

在数据摄取格式方面，它支持来自不同云平台的结构化、半结构化和非结构化格式。

![](img/1ac87d33537f9dbbb3c52436d99f2415.png)

Google 对 BigQuery Studio 笔记本的展示 [[2](https://cloud.google.com/blog/products/data-analytics/announcing-bigquery-studio)]

***#2: 通过连接到外部代码仓库来增强协作和版本控制。***

我不得不说，这个特性是我一直以来希望的。尽管它（还）不支持所有 [git 命令](https://www.atlassian.com/git/glossary#commands)，但 BQ Studio 支持诸如持续集成/持续部署（CI/CD）、版本历史记录和数据代码资产的源代码管理等软件开发实践。简而言之，现在可以查看笔记本的历史记录并 [还原到](https://www.atlassian.com/git/tutorials/undoing-changes/git-revert) 或 [从](https://www.atlassian.com/git/tutorials/using-branches) 特定笔记本版本创建分支。

![](img/9c5c1d3b2c19168e155f4bc148d2c25a.png)

Google 对版本控制 BQ Studio 功能的展示 [[2](https://cloud.google.com/blog/products/data-analytics/announcing-bigquery-studio)]

***#3: 在 BigQuery 中实施安全性和数据治理。***

BQ Studio 实施安全性，因为它减少了在 BigQuery 之外共享数据的需要。换句话说，通过在服务之间采用统一的凭证管理，分析师可以例如访问 Vertex AI 基础模型来执行复杂的分析任务（如情感分析），而无需将数据共享给第三方工具。

此外，还有数据治理特性，包括[数据血统](https://www.ibm.com/topics/data-lineage)跟踪、[数据分析](https://www.ibm.com/topics/data-profiling)和实施[质量](https://www.ibm.com/topics/data-quality)约束。我只能说“对此表示赞同”。

![](img/ff4445687e8fa11ec0766d59d0ef5cd9.png)

Google 对数据治理功能的展示 [[2](https://cloud.google.com/blog/products/data-analytics/announcing-bigquery-studio)]

总结上述特征，很明显 BigQuery Studio 是一个神奇的功能，因为它通过执行安全和治理措施，从数据摄取到数据建模的任务都能实现。

如果 Google 没有提供一个可以在 BigData Studio 笔记本中用于数据分析和建模的附加功能，[BigQuery DataFrames](https://cloud.google.com/python/docs/reference/bigframes/latest)，这个故事将不会完整。

通过安装`bigframes`包（类似于[安装任何其他 Python 包](https://packaging.python.org/en/latest/tutorials/installing-packages/)使用`pip`），数据专业人士可以使用以下 Python API [[3](https://cloud.google.com/python/docs/reference/bigframes/latest)]：

+   `bigframes.pandas` 是一个用于分析和处理的[pandas](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html) API，并且

+   `bigframes.ml`，即一个用于机器学习的[scikit-learn](https://scikit-learn.org/stable/modules/classes.html) API。

在机器学习主题上，我希望结束这一部分。

原因在于，在下一节中，我将详细介绍新的[BigQuery ML](https://cloud.google.com/bigquery/docs/bqml-introduction)函数。

## BigQuery GenAI 和 AI 功能

正如前言中提到的，我非常喜欢 BQML 函数，因为它们使数据专业人士能够使用 SQL 语法创建预测模型。

除了已经很不错的 BQML 函数组合用于监督学习和无监督学习之外，Google 现在增加了我下一个最喜欢的函数：**生成式 AI**和**AI 功能**。

关于生成式 AI 函数[ML.GENERATE_TEXT](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-generate-text)，我最近写了一篇博客来展示其特性。

[](/the-new-generative-ai-function-in-bigquery-38d7a16d4efc?source=post_page-----13af9a17f3db--------------------------------) ## BigQuery 中的新生成 AI 功能

### 如何使用 BigQuery 的 GENERATE_TEXT 远程函数

[towardsdatascience.com

总之，该功能可以用于从存储在 BigQuery 数据集中的非结构化文本中创建新的见解。或者更准确地说，你可以用它来创建*新类别*或*属性*（分类分析、情感分析或实体提取）、*总结*或*重写自然语言记录*，以及*生成广告*或*创意概念*。

我会说，基于 SQL 的数据工程现在具备了下一个层次的能力。

除了这个魔法功能，其他强大的新 SQL 基础**AI 功能包括：**

+   [ML.UNDERSTAND_TEXT](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-understand-text) — 帮助分析存储在 BigQuery 中的记录的文本的函数，并支持与 ML.GENERATE_TEXT 函数类似的功能。这意味着它支持实体、情感、分类和语法分析。

+   [ML.TRANSLATE](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-translate) — 用于将文本从一种语言翻译成另一种语言的函数。

+   [ML.PROCESS_DOCUMENT](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-process-document) — 用于处理来自[对象表](https://cloud.google.com/bigquery/docs/object-table-introduction)（例如发票）的非结构化文档的函数。

+   [ML.TRANSCRIBE](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-transcribe) — 用于从对象表中转录音频文件的函数。

+   [ML.ANNOTATE_IMAGE](https://cloud.google.com/bigquery/docs/reference/standard-sql/bigqueryml-syntax-annotate-image) — 用于从对象表中进行图像标注的函数。

尽管这些函数是用 SQL 编写的，但理解其查询结构和参数是正确使用的必备条件。为了加快学习曲线，一点编码帮助——DuetAI——会非常有用。

## **DuetAI 在 BigQuery 中**

就这么简单：[*DuetAI*](https://cloud.google.com/duet-ai/docs/use-cases/analyze-data-duet-ai)的魔法功能可以帮助数据专业人员在 BQ 和 BQ Studio 环境中编写、改进和理解他们的多语言代码。

更准确地说，该功能具有以下特征：

***#1: 在 BQ 环境或 BQ Studio 笔记本中从头创建查询或代码***

![](img/6908b1edca042e4c6a29f812b0e90125.png)

使用 DuetAI 在 BigQuery 中的代码补全示例 [作者提供的图片]

***#2: 解释 BQ 环境或 BQ Studio 笔记本中的查询和代码片段***

![](img/4e894af089e5feb4baf0e6ad49613d74.png)

使用 DuetAI 在 BigQuery 中的代码补全示例 [作者提供的图片]

***#3: 提升 BQ 环境或 BQ Studio 笔记本中的代码质量***

我的意思是，DuetAI 可以通过以下方式改进代码：

+   *语法纠正*：它可以识别并建议修正[语法错误](https://en.wikipedia.org/wiki/Syntax_error)。

+   *逻辑改进*：它可以建议替代的代码结构方式，从而提高整体效率和可读性。

+   *文档生成*：它可以自动生成代码文档，使其更易于理解。

最后，通过这个魔法特性，我将结束新 BigQuery “神奇动物”的部分和展示。

# 无限可能的世界

在书籍“[神奇动物在哪里](https://en.wikipedia.org/wiki/Fantastic_Beasts_and_Where_to_Find_Them)”中，J.K. 罗琳展示了当 *Maji* 和 *No-Maj* 人们了解魔法生物的积极特质时，它们变得不再那么可怕。同样，在这篇博客文章中，我想展示 BigQuery 的新奇功能，并指出它们在不同分析层面上的积极特质。

![](img/a35c6ba4c0af434875c982fc5deb1248.png)

作者使用[DALL-E](https://openai.com/dall-e-2?_sm_vck=ZrFSTvft5N6ZFV4rFDZ1Jn8fPNjfn4FMvqT8TnfR5fPTvFQf5Sn5)扩展在 ChatGPT 中创建的三只 BigQuery 野兽的插图（野兽的数量正确，但名字的数量不正确 ;))

我的目标是展示新功能如何在完整的分析工作流中支持你并简化工作，无论你是数据工程师、分析师还是科学家。此外，我还想指出它们如何增强具有不同数据背景的团队成员之间的协作。

希望你能亲身体验魔法，并了解更多关于“BigQuery 的神奇动物”的知识。

学习愉快！

> 感谢阅读我的帖子。请关注更多故事，[Medium](https://medium.com/@martosi/subscribe)和[Linkedin](https://www.linkedin.com/in/martosi/)。

# 知识资源

[[1](https://cloudonair.withgoogle.com/events/cloud-onboard-data-to-ai)] Google Cloud 网络研讨会：“Cloud OnBoard：使用 BigQuery 和 Vertex AI 从数据到 AI”，访问日期：2023 年 12 月 10 日，[`cloudonair.withgoogle.com/events/cloud-onboard-data-to-ai`](https://cloudonair.withgoogle.com/events/cloud-onboard-data-to-ai)

[[2](https://cloud.google.com/blog/products/data-analytics/announcing-bigquery-studio)] Google Cloud 博客：“宣布 BigQuery Studio——一个协作分析工作区，加速数据到 AI 的工作流”，访问日期：2023 年 12 月 11 日，[`cloudonair.withgoogle.com/events/cloud-onboard-data-to-AI`](https://cloud.google.com/blog/products/data-analytics/announcing-bigquery-studio)

[[3](https://cloud.google.com/python/docs/reference/bigframes/latest)] Google Cloud 文档：“BigQuery DataFrames”，访问日期：2023 年 12 月 11 日，[`cloud.google.com/python/docs/reference/bigframes/latest`](https://cloud.google.com/python/docs/reference/bigframes/latest)
