# AI初创公司趋势：来自Y Combinator最新一批的洞察

> 原文：[https://towardsdatascience.com/ai-startup-trends-insights-from-y-combinators-latest-batch-282efc9080ae?source=collection_archive---------2-----------------------#2023-07-21](https://towardsdatascience.com/ai-startup-trends-insights-from-y-combinators-latest-batch-282efc9080ae?source=collection_archive---------2-----------------------#2023-07-21)

## 当前AI领域中正在构建哪些类型的公司

[![Viggy Balagopalakrishnan](../Images/a3d6b5d26327892108816c0ef125b90d.png)](https://medium.com/@viggybala?source=post_page-----282efc9080ae--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----282efc9080ae--------------------------------) [Viggy Balagopalakrishnan](https://medium.com/@viggybala?source=post_page-----282efc9080ae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb3366eb9a0cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-startup-trends-insights-from-y-combinators-latest-batch-282efc9080ae&user=Viggy+Balagopalakrishnan&userId=b3366eb9a0cf&source=post_page-b3366eb9a0cf----282efc9080ae---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----282efc9080ae--------------------------------) · 10 分钟阅读 · 2023年7月21日

--

![](../Images/88d631fe583c00b09d6cfdb1f36807ea.png)

图片由 [Proxyclick Visitor Management System](https://unsplash.com/@proxyclick?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

知名的硅谷初创公司加速器Y Combinator（YC）最近宣布了他们的2023年冬季批次，毫不奇怪的是，**约31%的初创公司** [**（269家中有80家）**](https://www.ycombinator.com/companies?batch=W23&tags=AI-powered+Drug+Discovery)**有自报的AI标签**。虽然实际数字可能有偏差，但趋势很明显——利用AI的初创公司现在已经成为YC批次中的一部分。

对于这一部分，我分析了这一批次中的20–25家初创公司，以了解一些较大的趋势，特别是那些利用**LLMs（大语言模型）**的初创公司。这些趋势涵盖了他们如何识别需要解决的问题，他们采取了哪些解决方案，他们做得对的方面以及他们的方法中可能存在的风险。

但在深入探讨趋势之前，让我们从一个通用框架开始，思考科技公司（无论大小）如何通过AI创造价值。

# AI价值链

如果你最近关注了科技新闻，就会发现关于AI的内容激增，很难始终理解这些新闻在整体格局中的位置。让我们用一个简化的框架来思考这个问题。

AI是一个非常广泛的术语，包括了从可以预测事物的回归模型，到能够识别物体的计算机视觉，再到最近的LLMs（大语言模型）等多种技术。为了讨论的目的，我们将重点关注LLMs，这些模型在OpenAI向公众开放ChatGPT后备受瞩目，并在公司之间掀起了一场AI竞赛。

利用AI的科技公司通常在以下三层之一运作：

+   **基础设施** — 这包括硬件提供商（例如，制造GPU以支持AI模型所需大量计算的NVIDIA），计算提供商（例如，提供云端处理能力的Amazon AWS、Microsoft Azure、Google Cloud），AI模型/算法（例如，提供LLMs的OpenAI、Anthropic），以及AI平台（例如，提供模型训练平台的TensorFlow）。

+   **数据平台/工具层** — 这包括了那些能够收集、存储和处理数据以用于AI应用的平台（例如，提供云数据仓库的Snowflake，提供统一分析平台的Databricks）。

+   **应用层** — 这涵盖了所有利用AI进行特定应用的公司（初创公司、中大型科技公司以及非科技公司）。

根据市场当前的状况以及过去类似情况的发展（例如，云计算市场），**基础设施和数据平台层可能会趋向于少数几个相对商品化的玩家**。例如：

+   在硬件玩家中，NVIDIA目前凭借其GPU产品处于领先地位（[其股票在2023年翻了三倍](https://www.investors.com/news/nvidia-stock-2023-buy-now/)），我们将看到还有谁能赶上。

+   计算市场已经趋于集中，AWS、Azure和Google Cloud占据了[三分之二的市场份额](https://www.statista.com/chart/18819/worldwide-market-share-of-leading-cloud-infrastructure-service-providers/)。

+   在AI算法层面，OpenAI凭借GPT模型表现突出，但这是一个竞争激烈的市场，参与者资金雄厚（如谷歌的Deepmind/Google Brain、Facebook Lambda、Anthropic、Stability AI）——如果你想深入了解，可以参见[这个分析](https://aisupremacy.substack.com/p/the-top-six-rivals-competing-with)。这里有两点需要注意：i) 大多数公司可以访问相同的数据集，如果一家公司的确获得了新的付费数据集（如Reddit），竞争对手也很可能会获得相同的数据集，ii) GPT模型位于算法层面，但ChatGPT产品位于应用层面（而非算法层面）。

鉴于这种可能的商品化路径，这些层面的公司有两种可能的路径可以追求：

1.  第一条路径是增强他们的产品，以跨层运作，最近的并购活动就是一个例证——Snowflake（数据平台层的数据仓库公司）最近[收购了Neeva](https://www.snowflake.com/blog/snowflake-acquires-neeva-to-accelerate-search-in-the-data-cloud-through-generative-ai/)以增强其搜索能力，并可能解锁LLMs在企业中的应用，Databricks（数据平台层的分析平台）[收购了MosaicML](https://www.databricks.com/company/newsroom/press-releases/databricks-signs-definitive-agreement-acquire-mosaicml-leading-generative-ai-platform)（在AI算法层面）以使*“生成式AI对每个组织可及，使他们能够利用自己的数据构建、拥有和保护生成式AI模型”*。

1.  第二条路径是向应用层发展——ChatGPT就是一个经典例子。OpenAI在AI算法层面上表现强劲，但随着消费者产品的推出，他们现在是几十年来第一个真正与谷歌搜索竞争的对手。

**未来从AI和LLMs中解锁的大多数价值将位于应用层面**，包括通过创办新初创公司所创造的价值，这将我们引向Y Combinator。

# Y Combinator（YC）的运作方式

简要介绍一下YC，然后我们会进入趋势部分。大多数YC公司处于非常早期阶段——[52%的批次](https://www.ycombinator.com/blog/meet-the-yc-winter-2023-batch)的公司仅凭一个想法被接受，77%的批次在YC之前没有任何收入。

YC非常挑剔（接受率低于2%），但主要依靠规模运作：

+   他们在2023年投资了300多家公司，分为两个批次，在2022年投资了600多家公司。

+   公司获得一个[标准交易](https://www.ycombinator.com/deal)（125,000美元资金换取7%的股权）。

+   YC为初创公司提供了大量的指导以及对包括YC校友、投资者等在内的大型网络的访问。

+   因此，为了成功，YC 只需要几个巨大的成功（类似于任何天使投资），并且一些校友已经取得了[极大的成功](https://www.ycombinator.com/topcompanies/valuation)

总之——YC 是一个很好的“代表性”列表，展示了早期阶段初创公司市场的现状以及刚起步的初创公司利用 AI 的机会。接下来，我们将深入探讨大趋势。

# AI 初创公司趋势

## 1\. 专注于特定问题和客户

初创公司正针对特定问题和客户群体，即“通用” AI 解决方案变得越来越少。

一个这样的例子是 [Yuma.ai](https://www.ycombinator.com/launches/IBB-yuma-chatgpt-for-customer-support)，它专注于帮助那些在处理客户请求和问题时遇到困难的 Shopify 商家（你可以在[这里](https://www.ycombinator.com/launches/IBB-yuma-chatgpt-for-customer-support)查看演示）。通过利用大型语言模型（LLMs），Yuma.ai 自动生成来自知识库的回复。另一个名为 [Speedy](https://www.ycombinator.com/launches/I7a-speedy-democratizing-marketing-to-smbs) 的初创公司致力于支持缺乏时间使用生成 AI 创建营销内容的小型和中型企业（SMBs）。[Haven](https://www.ycombinator.com/companies/haven-2) 旨在为物业经理自动化约 50% 的居民互动。[OfOne](https://www.ycombinator.com/launches/I5F-ofone-automating-order-taking-at-fast-food-drive-thrus) 目标是大型快餐店，帮助它们自动化点餐过程并提高盈利能力。

在所有这些例子中，关注的都是狭窄的问题空间和客户，并在此背景下应用 LLM。

## 2\. 与现有软件的集成

除了仅仅使用 GPT / LLM 并通过 UI 进行展示外，一些初创公司还进一步整合了他们客户已经使用的现有软件。

一个典型的例子是 [Lightski](https://www.ycombinator.com/launches/IHv-lightski-chatgpt-for-sales-teams)，它专注于与客户关系管理（CRM）软件如 Salesforce 集成。他们的目标是让客户通过 Slack 发送自然语言消息来更新 CRM，从而省去浏览用户界面的需要。[Yuma.ai](https://www.ycombinator.com/launches/IBB-yuma-chatgpt-for-customer-support) 提供一键安装功能，将 LLM 的强大功能与客户自己的知识库结合，生成服务代理的草拟回复。

这些集成已成为解锁新用例的主要驱动力，而这些用例是开箱即用的 LLM 应用如 ChatGPT 无法轻易解决的。

## 3\. 与其他 AI 技术结合利用 LLM

初创公司正在探索通过结合计算机视觉和预测等其他AI技术，与LLM一起创建差异化的产品。

一个这样的例子是 [Automat](https://www.ycombinator.com/launches/I6p-lasso-robotic-process-automation-for-chrome-using-chatgpt-and-computer-vision)，其客户提供他们希望自动化的重复Chrome过程的视频演示。Automat 然后利用应用于屏幕录制的计算机视觉技术，以及人类自然语言输入来创建所需的自动化。另一个名为 [Persana AI](https://www.ycombinator.com/launches/IDv-persana-ai-sales-copilot-to-boost-productivity) 的初创公司则利用CRM数据集成和公开数据来预测销售团队的潜在热点线索。他们接着使用LLM起草个性化的外发消息给每个识别出的线索，利用有关个人的可用自定义数据（你可以在 [这里](https://www.persana.ai/) 查看演示）。

结合多种技术正在帮助这些初创公司创建护城河，并与通用的LLM应用程序区分开来。

## 4\. LLM的定制化

许多初创公司提供基于用户过去数据和语言风格的定制选项，以定制客户使用的LLM模型。

例如，[Speedy](https://www.ycombinator.com/launches/I7a-speedy-democratizing-marketing-to-smbs)，一个帮助中小型企业生成营销内容的平台，与他们的客户进行品牌研讨会。从这些研讨会中获得的见解被输入到他们的模型中，使Speedy能够捕捉并融入每个业务的独特声音和品牌身份到生成的内容中。类似地，[Yuma.ai](https://www.ycombinator.com/launches/IBB-yuma-chatgpt-for-customer-support) 专注于从以前的帮助台票据中学习写作风格。通过分析这些互动中的模式和语言，Yuma.ai 能够生成符合既定风格的草稿回应，确保客户沟通的一致性和个性化。

## 5\. 创意用户界面

初创公司开始利用的一个被低估的杠杆是构建独特且有用的用户界面，而大多数当前的LLM产品（如chatGPT，Bard）在这方面表现不佳。这些界面，当定制为特定用例时，可以为客户解锁大量新价值，并吸引更多尚未采纳现有产品的用户，因为这些产品难以使用。

[Type](https://www.ycombinator.com/launches/I2D-type-an-ai-first-document-editor) 是一个有趣的例子——他们构建了一个灵活且快速的文档编辑器，允许用户在编写时通过按 cmd + k 快速调用强大的 AI 命令。Type 的 AI 理解文档的上下文，并随着你写作的增加而调整建议，学习你的风格（你可以在[这里](https://www.ycombinator.com/launches/I2D-type-an-ai-first-document-editor)查看演示）。

另外几个有趣的例子包括 [Lightski](https://www.ycombinator.com/launches/IHv-lightski-chatgpt-for-sales-teams) 将 Slack 用作更新 CRM 信息的接口，以及 [Persana AI](https://www.ycombinator.com/launches/IDv-persana-ai-sales-copilot-to-boost-productivity) 使用 Chrome 扩展程序以便在个人的 LinkedIn 页面上轻松访问外发草稿。

## 高信息量、高精度用例

这一批初创公司中有一些专注于特定用例，这些用例既需要处理大量信息，又要求对发现的见解具有高度的精确性。

[SPRX](https://www.ycombinator.com/launches/HfD-sprx-technologies-making-r-d-credits-simple-fast-and-affordable) 直接从你的工资单和会计系统中获取数据，以计算符合 IRS 要求的准确 R&D 信贷。

在医疗保健领域，[Fairway Health](https://www.ycombinator.com/launches/IIu-fairway-health-process-prior-authorization-faster) 利用大型语言模型（LLMs）提高了原本相当手动的流程效率——分析长篇医疗记录以评估患者是否符合某种治疗的资格。这帮助保险公司变得更加高效，并为消费者创造了更少挫败感的体验（你可以在[这里](https://www.ycombinator.com/launches/IIu-fairway-health-process-prior-authorization-faster)查看演示）。

[AiFlow](https://www.ycombinator.com/launches/IBY-aiflow-llms-for-private-equity-diligence) 使用 LLMs 从数百份文档分析中提取报价和数据，以帮助私募股权公司进行尽职调查。

## 数据孤岛化，BYOD 产品用于企业客户

与消费者不同，企业希望控制他们的数据如何被使用和与公司共享，包括AI软件的提供商。企业在整合来自不同来源的数据并将其带入内部方面投入了大量精力（这篇文章[合作伙伴集成 + 智能系统：今天最深的护城河](https://bootcamp.uxdesign.cc/partner-integrations-system-of-intelligence-todays-deepest-moat-4b4396420e0d)由同为Medium作者的[Phani Vuyyuru](https://medium.com/u/91e076a6bebd?source=post_page-----282efc9080ae--------------------------------)深入探讨了这一战略）。因此，他们对放弃数据控制的动机较小。他们希望在一个可以将自己的数据（BYOD）带入基线产品并在一个隔离的环境中定制产品的框架下操作。

[CodeComplete](https://www.ycombinator.com/launches/Hxe-codecomplete-github-copilot-for-enterprise)的想法最初出现是在他们的创始人在Meta使用GitHub Copilot时，他们的请求因数据隐私考虑在内部被拒绝。CodeComplete现在是一个AI编码助手工具，经过微调以适应客户自己的代码库，提供更相关的建议，这些模型直接部署在客户的本地或自己的云端。

类似地，[AlphaWatch AI](https://www.ycombinator.com/companies/alphawatch-ai)，一个为对冲基金提供的AI副驾驶，帮助客户使用自定义的LLM，这些LLM利用了外部数据源和安全的私人数据。

# 护城河风险

看到大量AI初创公司涌现确实令人兴奋，这有助于个人消费者和组织提高效率。这些产品无疑将在解决问题的生产力和有效性方面带来巨大的解锁。

然而，这些初创公司面临的一个关键风险是可能缺乏长期护城河。鉴于这些初创公司的阶段和有限的公开信息，很难过多地解读，但不难指出其长期防御性的漏洞。例如：

+   如果一个初创公司基于使用像GPT这样的基础LLM，构建到帮助台软件中的集成以理解知识库和写作风格，然后生成草稿回复，那么是什么阻止一个帮助台软件巨头（比如Zendesk，Salesforce）复制这个功能，并将其作为他们产品套件的一部分提供？

+   如果一个初创公司正在为文本编辑器构建一个有助于内容生成的酷炫界面，是什么阻止了已经在试验自动草稿的Google Docs和已经在试验Copilot工具的Microsoft Word去复制它？更进一步，是什么阻止他们提供一个效果差25%的产品，并与现有产品套件一起免费赠送（例如，Microsoft Teams接管Slack的市场份额）？

那些没有护城河的公司仍然可以在当前形式下取得成功，它们的业务性质使它们成为具有吸引力的收购目标，无论是从功能扩展还是从人才角度来看。然而，对于那些希望将早期想法转化为巨大成功的初创公司来说，建立护城河至关重要。

一种明确的方法是构建一个完整的产品，以解决一个问题领域，并且广泛使用 AI 作为其功能的一部分（而不是仅仅在现有问题领域上增加的 AI 产品）。

例如， [**Pair AI**](https://www.ycombinator.com/launches/I9G-pair-ai-monetize-your-knowledge-in-a-scalable-way) 专注于帮助创作者以类似 Tiktok 的格式构建更具吸引力的课程，并在其产品中包含一些 AI 功能（如对话式问答）。 [KURUKURU](https://www.ycombinator.com/launches/IDT-kurukuru-3d-engine-in-the-browser-for-comics-creation) 正在构建一个用于创建漫画的 3D 引擎，并且在角色创建方面有一些 AI 功能。

另一种方法是通过利用一些上述趋势（如数据集成、BYOD 模型、支持自定义、与其他 AI 技术结合）来增强产品功能（将 AI 功能扩展为更广泛的产品），从而解决问题领域。

这是一个快速发展的市场，我们距离看到这些初创公司如何发展的结果还有一段距离——看看它们在未来几年如何发展将会很有趣。祝 YC 的 W23 批次好运！

感谢阅读！喜欢这篇文章吗？请考虑订阅 [**我的每周 Substack 通讯**](https://thisisunpacked.substack.com/)**。** 我每周发布一篇关于当前科技和商业话题的深入分析，阅读时间大约为 10 分钟。你也可以在 Twitter 上关注我 [@viggybala](https://twitter.com/viggybala)。祝好，Viggy。

[](https://thisisunpacked.substack.com/?source=post_page-----282efc9080ae--------------------------------) [## Unpacked | Viggy Balagopalakrishnan | Substack

### 对当前科技和商业话题的深入分析，将帮助你保持领先。每周送到你的邮箱…

thisisunpacked.substack.com](https://thisisunpacked.substack.com/?source=post_page-----282efc9080ae--------------------------------)
