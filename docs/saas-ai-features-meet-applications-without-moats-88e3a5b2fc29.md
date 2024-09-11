# SaaS AI 特性与无护城河的应用相遇

> 原文：[https://towardsdatascience.com/saas-ai-features-meet-applications-without-moats-88e3a5b2fc29?source=collection_archive---------2-----------------------#2023-10-17](https://towardsdatascience.com/saas-ai-features-meet-applications-without-moats-88e3a5b2fc29?source=collection_archive---------2-----------------------#2023-10-17)

## 几家企业 SaaS 公司最近宣布了生成型 AI 功能，这对缺乏可持续竞争优势的 AI 初创公司构成了直接威胁

[](https://medium.com/@viggybala?source=post_page-----88e3a5b2fc29--------------------------------)[![Viggy Balagopalakrishnan](../Images/a3d6b5d26327892108816c0ef125b90d.png)](https://medium.com/@viggybala?source=post_page-----88e3a5b2fc29--------------------------------)[](https://towardsdatascience.com/?source=post_page-----88e3a5b2fc29--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----88e3a5b2fc29--------------------------------) [Viggy Balagopalakrishnan](https://medium.com/@viggybala?source=post_page-----88e3a5b2fc29--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb3366eb9a0cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsaas-ai-features-meet-applications-without-moats-88e3a5b2fc29&user=Viggy+Balagopalakrishnan&userId=b3366eb9a0cf&source=post_page-b3366eb9a0cf----88e3a5b2fc29---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----88e3a5b2fc29--------------------------------) ·12 分钟阅读·2023年10月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F88e3a5b2fc29&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsaas-ai-features-meet-applications-without-moats-88e3a5b2fc29&user=Viggy+Balagopalakrishnan&userId=b3366eb9a0cf&source=-----88e3a5b2fc29---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F88e3a5b2fc29&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsaas-ai-features-meet-applications-without-moats-88e3a5b2fc29&source=-----88e3a5b2fc29---------------------bookmark_footer-----------)

回到七月，我们 [深入探讨了生成型AI初创公司](/ai-startup-trends-insights-from-y-combinators-latest-batch-282efc9080ae) 来自 Y Combinator 的 W23 批次——特别是那些利用大型语言模型（LLM）如 GPT 来驱动 ChatGPT 的初创公司。我们识别出这些初创公司的几个主要趋势——例如专注于非常具体的问题和客户（例如，为中小企业提供营销内容），与现有软件的集成（例如，与 Salesforce 等 CRM 平台的集成），以及为特定环境定制大型语言模型的能力（例如，公司的品牌声音）。

文章的一个次要但不常被强调的部分是关于 [护城河风险](/ai-startup-trends-insights-from-y-combinators-latest-batch-282efc9080ae) — 引用自当时的报道：

> 这些初创公司面临的一个关键风险是长期护城河的可能缺乏。鉴于这些初创公司的阶段和有限的公开信息，很难对其进行过多的解读，但长期的防御性却并非难事。例如：
> 
> 如果一家初创公司是基于以下前提构建的：利用像 GPT 这样的大型语言模型，将其集成到帮助台软件中，以理解知识库和写作风格，然后生成草案回复，那么有什么阻止帮助台软件巨头（比如 Zendesk、Salesforce）复制此功能，并将其作为产品套件的一部分提供？
> 
> 如果一家初创公司正在为文本编辑器构建一个酷炫的界面，帮助内容生成，那么有什么阻止谷歌文档（已在尝试自动起草）和微软 Word（已在尝试使用 Copilot 工具）复制这一技术？更进一步说，有什么阻止它们提供一个比现有产品套件稍差 25% 的产品，并免费赠送（例如微软 Teams 占领 Slack 的市场份额）？

这正是过去几个月发生的事情。几家大型企业 SaaS 公司宣布和/或推出了他们的生成式 AI 产品 — 例如 Slack、Salesforce、Dropbox、Microsoft 和 Google 等。这直接威胁到为企业客户构建有用的生产力应用程序的生成式 AI 初创公司，但其竞争优势有限且缺乏持久性（即没有护城河）。在本文中，我们将深入探讨：

+   AI 价值链回顾

+   企业 SaaS 公司最近的 AI 功能

+   初创公司如何在这种环境中构建护城河

# AI 价值链回顾

我们不会在这方面花太多时间，但快速提醒一下，企业如何从 AI 中获得价值的一种方式是通过 [AI 价值链](/ai-startup-trends-insights-from-y-combinators-latest-batch-282efc9080ae) 的概念。具体来说，你可以将价值链分解为三个层次：

+   基础设施（例如，NVIDIA 制造用于运行 AI 应用程序的芯片，Amazon AWS 提供用于 AI 的云计算，Open AI 提供像 GPT 这样的大型语言模型来构建产品）

+   平台（例如，Snowflake 提供基于云的解决方案，用于在一个平台上管理所有数据需求，从摄取到清理到处理）

+   应用（例如，一家初创公司正在构建一款帮助中小企业快速创建营销内容的产品）

![](../Images/b2867bdfaf6481bc5cffc92816637414.png)

AI 价值链；来源：作者

尽管生成性 AI 浪潮始于 OpenAI 推出的 ChatGPT（由 GPT 模型驱动），但基础设施层商品化的趋势越来越明显，包括 Facebook（LLaMA）、Google（LaMDA）、Anthropic 等几个大型玩家纷纷进入市场。商品化的原因是大多数模型使用相同的公开数据集进行训练（如爬取互联网网站的 CommonCrawl 和维基百科）。

在这个数据池之外，任何拥有大量第一方数据的大公司要么是[将数据自用，要么是创建许可模式](https://thisisunpacked.substack.com/i/132915250/what-strategies-are-content-platforms-adopting-to-respond-to-this)，这意味着这些数据要么不可用，要么对每个模型提供者都可用于训练，即商品化。这与云计算市场的情况类似，当时 Amazon AWS、Microsoft Azure 和 Google Cloud 现在占据了市场的大部分份额，但彼此之间竞争激烈。

虽然平台层的商品化程度较低，且可能还有更多玩家可以满足各种客户需求（如初创公司与中小企业与大型企业客户），但它正朝着商品化的方向发展，大型玩家开始增强他们的产品（例如，数据仓储平台 Snowflake 最近收购了 Neeva，以解锁企业的 LLM 应用，分析平台 Databricks 收购了 MosaicML，以为客户提供生成性 AI）。

因此，AI 的大部分价值将会在应用层产生。然而，尚未解答的问题是哪些公司可能从大型语言模型（如 GPT）解锁的应用中获益。毫不意外，在[Y Combinator 的 W23 批次](https://www.ycombinator.com/companies?batch=W23)中的 269 家初创公司中，约 31% 标注了 AI 标签。虽然这些应用在客观上都是有用的，并且为客户解锁了价值，尤其是在企业 SaaS 领域，但越来越明显的是，现有的 SaaS 公司在从 AI 中获益方面处于更有利的位置。

# 企业 SaaS 公司近期的 AI 特性

在过去几周里，SaaS 公司发布了大量公告。让我们来逐一了解一下。

Slack 最初通过支持 [ChatGPT 机器人](https://www.theverge.com/2023/3/7/23628673/chatgpt-slack-salesforce-einstein-ai-business-messaging) 来功能于你的 Slack 工作区，不仅可以总结对话线程，还可以帮助草拟回复。此功能迅速扩展至支持 Claude 机器人（Claude 是 Anthropic 的 GPT 模型的对应物）。更重要的是，Slack 宣布他们在应用程序内原生构建了自己的生成式 AI，支持在各个线程和频道中进行广泛的总结功能（例如，告诉我今天这个频道发生了什么，告诉我项目 X 是什么）。本来可能是由初创公司构建的插件，现在变成了 Slack 内置的原生功能，因为 Slack 可以轻松地将像 GPT 这样的模型现成地拿来使用并构建生成式 AI 功能。这并不是特别困难，同时也节省了 Slack 处理集成问题和来自未知插件的繁琐用户体验的麻烦。

Salesforce 也有了新的宣布。他们的产品 [Einstein GPT](https://www.salesforce.com/news/press-releases/2023/03/07/einstein-generative-ai/?d=cta-body-promo-8) 被定位为他们 CRM 的生成式 AI。它将允许 Salesforce 用户查询各种信息（例如，我现在的主要线索是谁），自动生成和迭代电子邮件草稿，甚至根据这些查询创建自动化工作流。这个功能在截图中可能看起来比现实更好，但可以公平地预测 Salesforce 能在一年内构建出一个相对无缝的产品。实际上，这正是一些生成式 AI 初创公司今天正在构建的功能。虽然短期内很有用，但这些初创公司的成功不仅仅在于比 Einstein GPT 更好，而在于是否能好到让企业 SaaS 购买者愿意接受新产品的上手摩擦（我在我的评价中不会提及初创公司，因为从零开始构建产品很难，写评价相对容易）。

类似地，Dropbox 宣布了 [Dropbox Dash](https://blog.dropbox.com/topics/product/introducing-AI-powered-tools)，它被定位为一个 AI 驱动的通用搜索工具。它支持广泛的功能，包括从存储在 Dropbox 上的所有文档中提供问答答案，总结文档中的内容，并回答来自文档内容的特定问题（例如，这份合同什么时候到期）。同样，今天有一些生成式 AI 初创公司实质上也在构建这些功能，而 Dropbox 由于已经拥有所需的数据并能够在其产品中创建无缝接口，因此在长期成功的道路上相对更容易。

列表继续：

+   Zoom宣布了[Zoom AI](https://explore.zoom.us/en/ai-assistant/)，它提供会议总结，如果你错过了某些信息并希望赶上进度，还能回答会议中的问题，并总结聊天记录。如今，许多初创公司正在将这些功能作为独立产品（例如笔记工具）进行开发。

+   [微软365 Copilot](https://blogs.microsoft.com/blog/2023/09/21/announcing-microsoft-copilot-your-everyday-ai-companion/)将读取你的未读邮件并进行总结，回答所有文档中的问题，并起草文档等。这些功能还将无缝嵌入到Word、Excel、OneNote和OneDrive等产品的界面中。

+   谷歌也有一个类似的产品叫[Duet AI](https://workspace.google.com/solutions/ai/)用于他们的生产力套件。

+   即使是OpenAI（虽然不是主导SaaS公司）也推出了[ChatGPT企业版](https://openai.com/blog/introducing-chatgpt-enterprise)，它可以基本上接入公司的所有工具，并为员工提供简单的答案。

我绝不是说战斗已经结束。如果你使用过任何生成性AI产品，会发现有些惊艳的时刻，但更多的则是平平无奇。上述产品的宣传很有吸引力，但大多数要么处于试点阶段，要么是描述产品未来状态的新闻公告。

这些产品的采纳也受到几个未解决问题的限制。定价混乱，有些产品提供免费的AI功能以进行竞争，而其他一些更全面的助手产品则按座位收费。微软365 Copilot的定价为[$30/用户/月](https://blogs.microsoft.com/blog/2023/07/18/furthering-our-ai-ambitions-announcing-bing-chat-enterprise-and-microsoft-365-copilot-pricing/)，而ChatGPT企业版的价格约为[$20/用户/月](https://www.thenationalnews.com/business/technology/2023/08/29/chatgpts-new-paid-business-tier-all-you-need-to-know/)——虽然从消费者的角度看，这似乎还算可以，但对于一些企业买家来说，这个价格在大规模应用时可能显得可笑，尤其是当成本快速增加时。数据共享问题也是一个主要障碍，因为企业对与语言模型共享敏感数据持谨慎态度（尽管企业AI产品明确表示不会将客户数据用于训练目的）。

也就是说，这些问题是可以解决的，大型SaaS公司在构建AI功能时的专注意味着这些问题将在短期内得到解决。这就把我们带回了护城河问题——生成性AI初创公司如果想要在面对SaaS公司AI功能时继续繁荣，需要建立强大的护城河。

# 初创公司如何在这种环境中建立护城河

让我们从明显的非护城河开始：将大型语言模型从货架上取下并在其上构建一个小的价值主张（例如，更好的用户界面，连接一个数据源）并不会创造出长期、可持续的优势。这些很容易被模仿，即使你拥有先发优势，你也可能输给一个拥有更容易访问数据或更多接口灵活性的现有企业，或者陷入价格战的困境。

下面是一些非详尽的方法来为企业 AI 产品建立护城河。

## 1\. 领域/垂直专业化

一些领域/垂直市场比其他领域更适合构建 AI 应用。例如，在 CRM 软件之上进行构建非常难以防守，因为像 Salesforce 这样的 CRM 公司拥有数据连接和对接口的控制，能够更好地完成这项工作。你可以提出非常聪明的创新（例如，创建一个 LinkedIn 插件，利用 CRM 数据自动起草外展邮件），但创新者/市场首发者并不总是能赢得市场。

法律是 AI 初创公司可以大放异彩的一个领域。法律文件篇幅长，阅读起来需要耗费大量的人力，且对于所有涉事方而言都是一个令人沮丧的过程。总结/分析合同、从合同内容中回答问题、总结法律论点、从文件中提取证据，都是时间密集型任务，LLMs 可以有效地完成。[Casetext](https://casetext.com/)、[Harvey.ai](https://techcrunch.com/2022/11/23/harvey-which-uses-ai-to-answer-legal-questions-lands-cash-from-openai/) 是几家为律师提供副驾驶产品的初创公司，已构建了专门针对法律用例的定制体验。

医疗保健是一个急需提高效率的领域。部署 AI 在医疗保健中面临几个挑战，包括数据隐私/敏感性、需要处理的复杂软件（ERP、调度工具等）以及大型医疗保健产品公司技术深度/灵活性的不足。这些都是初创公司可以迅速推出产品并利用先发优势作为护城河的明显机会。

## 2\. 数据/网络效应

机器学习模型（包括大型语言模型）的表现随着训练数据量的增加而提升。这是为什么，例如，Google 搜索是世界上表现最好的搜索引擎之一——这不仅仅是因为 Google 索引了世界上所有的页面（其他搜索引擎也可以做到这一点），而是因为数十亿人使用这个产品，每个用户的交互都是一个数据点，反哺到搜索相关性模型中。

然而，企业产品面临的挑战是，企业客户将明确禁止SaaS或AI软件的提供商使用他们的数据进行训练（这是完全正当的）。企业拥有大量敏感信息——从客户数据到公司战略数据——他们不希望这些数据被输入到 OpenAI 或 Google 的大型语言模型中。

因此，围绕此问题构建护城河是困难的，但在某些情况下是可能的。例如，AI 工具生成的用于广告或营销目的的内容较不敏感，企业更可能允许这些数据用于改进模型（从而提高自身的未来表现）。另一种方法是拥有一个非企业版的产品，默认情况下用户选择将使用数据用于训练——个人和中小企业用户更可能接受这种方法。

## 3\. 引入多个数据源

将大型语言模型应用于特定企业用例的最困难部分不是从货架上挑选一个模型并部署，而是构建所需的管道，以便将公司的相关数据集输送给模型访问。

假设你是一家像 Intuit 这样的公司，向中小企业销售会计和税务软件。你支持成千上万的中小企业客户，当其中一位客户向你提出支持问题时，你希望为他们提供定制的响应。很可能，这位客户使用的产品数据存储在一个内部数据库中，而客户与产品的最新互动数据存储在另一个数据库中，他们过去的支持问题历史则存在于一个帮助台SaaS产品中。生成式AI初创公司构建护城河的一种方法是识别那些需要多个数据源的特定用例，而这些数据源并非由单一大型SaaS公司拥有，并构建集成以引入这些数据。

这在其他环境中效果极佳——例如，[客户数据平台](https://segment.com/resources/cdp/)市场的整个兴起源于需要从多个来源汇总数据，以便对客户有一个集中化的视图。

## 4\. 数据孤岛

大型企业不愿将敏感数据暴露给模型，尤其是那些由竞争对手或在市场上拥有过多影响力的公司（即由于缺乏替代方案，企业被迫与之共享数据的公司）拥有的模型。

从[YC W23 文章](/ai-startup-trends-insights-from-y-combinators-latest-batch-282efc9080ae)中，CodeComplete 是一个很好的例子，它就是从这一痛点中诞生的。

> [CodeComplete](https://www.ycombinator.com/launches/Hxe-codecomplete-github-copilot-for-enterprise)的构思最初源于他们的创始人在Meta时尝试使用GitHub Copilot时，由于数据隐私考虑，他们的请求在内部被拒绝。CodeComplete现在是一个AI编码助手工具，经过针对客户自身代码库的微调，以提供更相关的建议，模型直接部署在本地或客户自己的云中。

## 5\. 打造一个更全面的产品

基于以上所有原因，我个人对大多数独立AI应用是否具备长期护城河的潜力持怀疑态度，特别是那些面向企业客户的应用。虽然率先进入市场无疑是一种策略，也确实可能成为快速收购的良好途径，但建立真正强大的护城河的唯一方法是打造一个更全面的产品。

专注于仅仅为营销提供AI文案的公司始终面临被更大营销工具竞争取代的风险，例如来自Google/Meta平台的营销云或创意生成工具。建立在CRM或客服工具之上的AI层的公司也很可能被现有的SaaS公司模仿。

解决这个问题的方法是打造一个更全面的产品。例如，如果目标是提升营销内容创作的效果，一个更全面的产品将是一个解决核心用户问题的平台（例如：创建内容所需的时间，必须创建多种尺寸的内容），然后包括一个强大的生成AI功能集（例如：为Instagram生成最佳视觉效果）。

# 结论

我对生成AI能释放的生产力感到兴奋。虽然我个人至今尚未经历生产力的跳跃式提升，但我相信在不久的将来这种情况会迅速发生。考虑到基础设施和平台层的合理商品化，AI驱动的生产力所带来的最大价值将被应用层的产品所捕获。特别是在企业产品领域，我确实认为大量的价值将被现有的SaaS公司所捕获，但我对具有AI前瞻性功能集和因此具备实际护城河的新型全面产品的出现持乐观态度。

🚀 如果你喜欢这篇文章，请考虑订阅[**我的每周通讯Unpacked**](https://thisisunpacked.substack.com/?utm_source=medium&utm_medium=article_tds)**。** 每周，我会以10分钟的阅读时间发布一项**深度分析** **关于当前技术话题/产品策略**。祝好，Viggy。

[](https://thisisunpacked.substack.com/?source=post_page-----88e3a5b2fc29--------------------------------) [## Unpacked | Viggy Balagopalakrishnan | Substack

### 每周将一项技术话题/产品策略的深度分析送到你的邮箱。点击阅读Viggy的Unpacked…

[thisisunpacked.substack.com](https://thisisunpacked.substack.com/?source=post_page-----88e3a5b2fc29--------------------------------)
