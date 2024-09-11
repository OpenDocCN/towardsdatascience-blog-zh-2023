# GenAIOps：发展中的 MLOps 框架

> 原文：[https://towardsdatascience.com/genaiops-evolving-the-mlops-framework-b0012f936379?source=collection_archive---------5-----------------------#2023-07-18](https://towardsdatascience.com/genaiops-evolving-the-mlops-framework-b0012f936379?source=collection_archive---------5-----------------------#2023-07-18)

## 生成式 AI 需要新的部署和监控能力

[](https://medium.com/@davidsweenor?source=post_page-----b0012f936379--------------------------------)[![David Sweenor](../Images/7dbb5c549ab67bc78f906fb707969ff6.png)](https://medium.com/@davidsweenor?source=post_page-----b0012f936379--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b0012f936379--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b0012f936379--------------------------------) [David Sweenor](https://medium.com/@davidsweenor?source=post_page-----b0012f936379--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fec7aed1f3ef1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenaiops-evolving-the-mlops-framework-b0012f936379&user=David+Sweenor&userId=ec7aed1f3ef1&source=post_page-ec7aed1f3ef1----b0012f936379---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0012f936379--------------------------------) ·14 min read·Jul 18, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb0012f936379&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenaiops-evolving-the-mlops-framework-b0012f936379&user=David+Sweenor&userId=ec7aed1f3ef1&source=-----b0012f936379---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb0012f936379&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenaiops-evolving-the-mlops-framework-b0012f936379&source=-----b0012f936379---------------------bookmark_footer-----------)![](../Images/9df3e386524c734cb418954501ab0a90.png)

图片由作者提供 — **David Sweenor**

早在2019年，我曾发表过一篇LinkedIn博客，标题为[为什么你需要ML Ops来实现成功的创新](https://www.linkedin.com/pulse/why-you-need-ml-ops-successful-innovation-david-sweenor/?trackingId=sKI28CkFTfW4Ty7d3%2BBqFA%3D%3D)。快进到今天，将分析、机器学习（ML）和人工智能（AI）模型（或更确切地说，系统）投入实际应用仍然是许多组织面临的挑战。然而，值得注意的是，技术已经发展，新公司也应运而生，帮助解决在生产环境中部署、监控和更新模型的挑战。不过，随着生成性AI的发展，如OpenAI的[GPT-4](https://openai.com/gpt-4)、Google的[PaLM](https://ai.google/discover/palm2/) 2、Meta的[LLaMA](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/)以及[GitHub Copilot](https://github.blog/2022-06-21-github-copilot-is-generally-available-to-all-developers/)，组织们竞相了解LLMs的价值、成本、实施时间表和相关风险。由于我们才刚刚开始这段旅程，我认为大多数组织尚未做好精细调优、部署、监控和维护LLMs的准备，因此应谨慎行事。

# 什么是MLOps？

机器学习操作（即MLOps）可以定义为：

> ML Ops是一个跨职能的、协作的、持续的过程，专注于通过将统计学、数据科学和机器学习模型作为可重用的、高可用的软件工件进行管理，从而使数据科学操作化，并通过可重复的部署过程来实现。它涵盖了模型推理、可扩展性、维护、审计和治理等独特的管理方面，以及对生产环境中模型的持续监控，以确保它们在基础条件变化时仍能提供积极的业务价值。[1]

现在我们对MLOps有了清晰的定义，让我们讨论一下它对组织的重要性。

# 为什么MLOps重要？

在今天的算法驱动的商业环境中，MLOps的重要性不容忽视。随着组织越来越依赖复杂的机器学习模型来推动日常决策和提高运营效率，部署、管理、监控和更新这些模型的需求变得尤为重要。MLOps提供了一套框架和流程，以便数据科学家和计算机科学家（负责开发模型）与IT运维团队（负责部署、管理和维护模型）之间进行协作，确保模型可靠、最新，并能为业务带来价值。

# MLOps的关键能力

广义上讲，MLOps功能上包括自动化机器学习工作流、模型版本管理、模型监控和模型治理。

●**自动化工作流**简化了训练、验证和部署模型的过程；减少了人工操作并提高了速度。

● **模型版本控制** 允许跟踪变化并维护模型迭代的注册表。

● **模型监控** 对确保模型在生产系统中按预期表现至关重要。

● **模型治理** 提供合规性，以满足法规和组织政策。

这些能力共同使组织能够大规模地将ML和AI投入运营，为组织带来业务价值和竞争优势。

# MLOps：指标和KPI

为确保模型在生产系统中按预期表现并提供最佳预测，有几种类型的指标和关键绩效指标（KPI）用于跟踪其效果。与数据科学家交谈时，他们通常会强调以下指标：

● **模型性能指标**：这些是衡量模型预测性能的指标。它们可以包括准确率、精确率、召回率、F1分数、ROC曲线下面积（AUC-ROC）、平均绝对误差（MAE）、均方误差（MSE）等。选择指标取决于问题的类型（分类、回归等）和业务背景。

● **数据漂移**：这衡量生产工作流中输入数据与模型训练数据的偏差程度。显著的数据漂移可能表明模型的预测可能随着时间变得不那么可靠。我们在那个小小的“突发事件”COVID中看到了一个很好的例子。消费者习惯和商业规范一夜之间发生了变化，导致每个人的模型都崩溃了！

● **模型漂移**：类似于数据漂移，这衡量模型性能（通常是下降）随时间变化的程度，而不是衡量数据分布偏离常态的程度。如果基础数据分布发生变化，可能会导致模型假设变得不那么准确。

● **预测分布**：跟踪模型预测的分布可以帮助检测异常。例如，如果一个二分类模型突然开始预测比平时多得多的正例，这可能表明存在问题。这些通常与业务指标最为接近。

● **资源使用**：IT资源使用包括CPU使用率、内存使用率和延迟等指标。这些指标对于确保模型在系统的基础设施和架构约束内高效运行非常重要。

● **业务指标**：所有指标中最重要的，这些指标衡量模型对业务结果的影响。它们可能包括收入、客户流失率、转化率等指标。这些指标有助于评估模型是否提供了预期的业务价值。

现在我们已经对MLOps有了高层次的理解，知道了它的重要性、关键能力和指标，这与生成式AI有什么关系呢？

# 生成式AI：主要的跨职能用例

在生成式人工智能成为主流之前，组织主要实施的是针对结构化和半结构化数据的AI系统。这些系统主要以数字为训练基础，生成数字输出——预测、概率和分组分配（比如细分和聚类）。换句话说，我们会用历史数字数据如交易、行为、人口统计、技术、公司、地理空间和机器生成的数据来训练我们的AI模型——并输出流失、响应或与优惠互动的可能性。并不是说我们没有使用文本、音频或视频数据——我们有；情感分析、设备维护日志等；但这些用例远远不如基于数字的方法普遍。生成式人工智能具有一组新的能力，使组织能够利用多年来基本上忽视的数据——文本、音频和视频数据。

用途和应用有很多，但我总结了迄今为止生成式人工智能的关键跨职能用例。

# 内容生成

生成式人工智能可以生成类人质量的内容，包括音频、视频/图片和文本。

● **音频内容生成**：生成式人工智能可以制作适用于社交媒体平台如YouTube的音频轨道，或为你的书面内容添加AI驱动的配音，增强多媒体体验。事实上，我的前两本TinyTechGuides在Google Play上的配音完全由AI生成。我可以为AI朗读的书籍选择口音、性别、年龄、语速以及其他几个关键属性。查看这里的AI朗读有声书。

○ [人工智能：让AI为您的业务发挥作用的高管指南](https://play.google.com/store/audiobooks/details/Artificial_Intelligence_An_Executive_Guide_to_Make?id=AQAAAECisyHzkM&hl=en_US&gl=US&pli=1)

○ [现代B2B营销：营销卓越实务指南](https://play.google.com/store/audiobooks/details/David_Sweenor_Modern_B2B_Marketing?id=AQAAAECiM15z7M&hl=en_US&gl=US)

● **文本内容生成**：这可能是目前最受欢迎的生成式人工智能形式，从撰写博客文章、社交媒体更新、产品描述、草拟电子邮件、客户信件到RFP提案，生成式人工智能可以轻松生成各种文本内容，为企业节省大量时间和资源。不过请注意，内容虽然生成并听起来权威，并不意味着它在事实上的准确性。

● **图像和视频生成**：我们已经看到这种技术在好莱坞慢慢成熟，例如通过 AI 生成的《星球大战》角色到在最新的《夺宝奇兵》电影中 [去老化哈里森·福特](https://www.wired.com/story/indiana-jones-and-the-dial-of-destiny-de-aging-tech/)，AI 可以创建逼真的图像和电影。生成式 AI 可以通过为广告、演示文稿和博客生成内容来加速创意服务。我们已经看到像 [Adobe](https://www.forbes.com/sites/rashishrivastava/2023/06/08/adobe-brings-its-generative-ai-tool-firefly-to-businesses/?sh=14e91793582b) 和 [Canva](https://www.forbes.com/sites/alexkonrad/2023/03/23/canva-launches-magic-ai-tools-reaches-125-million-users/?sh=24f9e02f290e) 等公司在创意服务方面做出了共同努力。

● **软件代码生成**：生成式 AI 可以生成软件代码（如 Python）和 SQL，这些代码可以集成到分析和 BI 系统中，以及 AI 应用本身。实际上，微软正在继续研究使用 [‘教科书’来训练 LLMs](https://arxiv.org/abs/2306.11644) 以创建更准确的软件代码。

# 内容总结与个性化

除了为公司创建全新的现实内容，生成式 AI 还可以用来总结和个性化内容。除了 ChatGPT 外，像 [Writer](https://writer.com/)、[Jasper](https://www.jasper.ai/) 和 [Grammarly](https://www.grammarly.com/) 这样的公司也在针对营销职能和组织进行内容总结和个性化。这将允许营销组织花时间制定周密的内容日历和流程，然后这些不同的服务可以被微调，以生成似乎无限的授权内容变体，从而可以在合适的时间通过合适的渠道传递给合适的人。

# 内容发现与问答

生成式 AI 获得关注的第三个领域是内容发现和问答。从数据与分析软件的角度来看，各种供应商正在将生成式 AI 功能纳入其中，以创建更自然的界面（以简单语言）来促进组织内部新数据集的自动发现，以及编写现有数据集的查询和公式。这将使非专家的商业智能（BI）用户能够提出简单的问题，如“我在东北地区的销售额是多少？” 然后深入挖掘并提出越来越详细的问题。BI 和分析工具根据他们的查询自动生成相关的图表和图形。

我们还看到这一点在医疗行业和法律行业的使用增加。在医疗领域，生成式 AI 可以筛查大量数据，帮助总结医生笔记，并通过聊天机器人、电子邮件等方式个性化与患者的沟通和往来。虽然对仅仅将生成式 AI 用于诊断功能有所保留，但有了人的参与，我们会看到这种应用的增加。我们还将看到生成式 AI 在法律行业中的使用增加。作为一个以文件为中心的行业，生成式 AI 能够快速找到合同中的关键条款，帮助法律研究，总结合同，并为律师创建定制的法律文件。麦肯锡称之为[法律副驾驶](https://www.mckinsey.com/featured-insights/in-the-balance/legal-innovation-and-generative-ai-lawyers-emerging-as-pilots-content-creators-and-legal-designers)。

现在我们了解了与生成式 AI 相关的主要用途，让我们转向关键问题。

# 生成式 AI：主要挑战与考虑因素

生成式 AI 尽管前景广阔，但也带来了自己的障碍和潜在陷阱。组织在将生成式 AI 技术整合到业务流程中之前，必须仔细考虑几个因素。主要挑战包括：

● **准确性问题（幻觉）**：大型语言模型（LLMs）常常会生成误导性或完全虚假的信息。这些回答可能看起来很可信，但完全是捏造的。企业可以建立什么样的保护措施来检测和防止这些虚假信息？

● **偏见**：组织必须了解模型中的偏见来源，并实施缓解策略来加以控制。公司制定了哪些政策或法律要求来应对潜在的系统性偏见？

● **透明度缺失**：对于许多应用，尤其是在金融服务、保险和医疗等行业，模型透明度通常是业务要求。然而，LLMs 本质上并不具有可解释性或可预测性，导致“幻觉”和其他潜在的误差。如果您的企业需要满足审计师或监管机构的要求，您必须问自己，我们是否可以使用 LLMs？

● **知识产权（IP）风险**：用于训练许多基础LLMs的数据通常包括公开的可用信息——我们已经看到有关图像（例如[HBR — 生成性AI存在知识产权问题](https://hbr.org/2023/04/generative-ai-has-an-intellectual-property-problem)）、音乐（[The Verge — AI Drake刚刚给Google设下了一个不可能的法律陷阱](https://www.theverge.com/2023/4/19/23689879/ai-drake-song-google-youtube-fair-use)）和书籍（[LA Times — Sara Silverman和其他畅销书作者起诉Meta和OpenAI侵犯版权](https://www.latimes.com/entertainment-arts/books/story/2023-07-10/sarah-silverman-authors-sue-meta-openai-chatgpt-copyright-infringement)）的不当使用的诉讼。在许多情况下，训练过程不加选择地吸收所有可用数据，导致对IP暴露和版权侵权的潜在诉讼。这引出了一个问题，你的基础模型是用什么数据训练的，细调时又用了什么数据？

● **网络安全与欺诈**：随着生成性AI服务的广泛使用，组织必须为恶意行为者的潜在滥用做好准备。生成性AI可以用来制造深度伪造以进行社会工程攻击。你的组织如何确保用于训练的数据没有被欺诈者和恶意行为者篡改？

● **环境影响**：训练大规模AI模型需要大量计算资源，这会导致显著的能源消耗。这对环境有影响，因为所用的能源往往来自不可再生的来源，导致碳排放。对于已有环境、社会和治理（ESG）举措的组织来说，你的程序如何考虑LLM的使用？

现在，公司需要考虑的事情有很多，但主要的几个已经被涵盖。这引出了下一个问题，我们如何使生成性AI模型实现操作化？

# GenAIOps：需要一套新的能力

现在我们对生成性AI、关键用途、挑战和考虑因素有了更好的了解，接下来让我们看看MLOps框架必须如何演变——我将其称为GenAIOps，据我所知，我是第一个提出这个术语的人。

让我们来看看创建大语言模型（LLMs）的高层次流程；该图形改编自[基础模型的机遇与风险](https://arxiv.org/abs/2108.07258)。

**图1.1：训练和部署LLMs的流程**

![](../Images/3388767a92e2f3baa4e5d401a1a9deb5.png)

训练和部署LLMs的流程——图片由作者、TinyTechGuides创始人David E Sweenor提供

在上述过程中，我们看到数据被创建、收集、整理，然后模型被训练、调整和部署。鉴于此，对于一个全面的GenAIOps框架，应考虑哪些因素？

# GenAIOps：清单

最近，斯坦福大学发布了一篇论文 [基础模型提供者是否遵守了草案 EU AI 法案](https://crfm.stanford.edu/2023/06/15/eu-ai-act.html)？阅读之后，我以此为灵感生成了下面的 GenAIOps 框架检查表。

**数据：**

○ 用于训练模型的数据源有哪些？

○ 用于训练模型的数据是如何生成的？

○ 训练者是否获得了在该背景下使用数据的许可？

○ 数据中是否包含受版权保护的材料？

○ 数据中是否包含敏感或机密信息？

○ 数据中是否包含个人或 PII 数据？

○ 数据是否被污染？是否容易受到污染？

○ 数据是否真实，还是包含了 AI 生成的内容？

**建模：**

○ 模型有哪些限制？

○ 模型是否存在风险？

○ 模型性能基准是什么？

○ 如果需要，我们能否重新创建模型？

○ 模型是否透明？

○ 创建当前模型使用了哪些其他基础模型？

○ 训练模型使用了多少能源和计算资源？

**部署：**

○ 模型将部署在哪里？

○ 目标部署应用程序是否了解它们在使用生成型 AI？

○ 我们是否拥有满足审计员和监管机构要求的适当文档？

现在我们有了起点，让我们更详细地查看这些指标

# GenAIOps：指标和过程考虑

以 MLOps 指标和 KPI 为起点，让我们审视这些如何应用于生成型 AI 指标。我们希望 GenAIOps 能帮助解决生成型 AI 特有的挑战，例如生成虚假、伪造、误导性或有偏见的内容。

# 模型性能指标

在生成型 AI 的背景下，组织如何衡量模型的性能？我怀疑大多数组织可能会使用商业上可用的预训练 LLM，并使用自己的数据来微调和适应他们的模型。

现在，确实存在与基于文本的 LLM 相关的技术性能指标，如 BLEU、ROUGE 或 METEOR，还有其他针对图像、音频和视频的指标，但我更关心的是生成虚假、伪造、误导性或有偏见的内容。组织可以采取哪些控制措施来监控、检测和缓解这些情况？

我们确实看到过宣传的泛滥，社交媒体巨头如 Facebook、Google 和 Twitter 未能实施一种一致且可靠的工具来防止这种情况发生。如果是这样，你们的组织如何衡量生成型 AI 模型的性能？会有事实检查员吗？图像、音频和视频呢？如何衡量这些模型的性能？

# 数据漂移

鉴于模型训练需要大量资源和时间，模型创建者将如何确定数据是否在漂移，从而需要新的模型？组织将如何理解他们的数据是否演变到需要重新校准模型的程度？对于数值数据，这相对简单，但我认为我们仍在学习如何处理文本、图像、音频和视频等非结构化数据。

假设我们能够创建一个机制来定期调整我们的模型，我们还应该有一个控制措施来检测数据漂移是否由于真实事件还是AI生成内容的扩散？在我的文章[《AI熵：AI生成内容的恶性循环》](https://medium.com/towards-data-science/ai-entropy-the-vicious-circle-of-ai-generated-content-8aad91a19d4f)中，我讨论了当你在AI上训练AI时，它会随着时间变得越来越笨。

# 模型漂移

类似于你对模型性能和数据漂移的担忧，你的组织将如何检测和理解模型性能是否开始漂移？你会有人工监控输出还是向最终用户发送调查问卷？也许更直接的方式是不仅要建立控制措施来监控模型的技术性能，而且你的公司应该始终跟踪模型输出。毫无疑问，你是用模型来解决特定的业务挑战，你需要监控业务指标。你是否看到购物车放弃率上升，客服电话增多或减少，或者客户满意度评级发生变化？

# 预测分布

我认为我们在追踪基于数值的预测方面拥有不错的工具和技术。但现在我们在处理文本、图像、音频和视频时，你如何看待监控预测分布的问题？我们能否理解模型在部署目标地是否生成了虚假的相关性？如果可以，你会采取什么措施来衡量这种现象？

# 资源使用

表面上，这似乎相对直接。然而，随着生成使用的增长，你的组织将需要建立一个系统来跟踪和管理它的使用。定价模型在生成AI领域仍在发展，因此我们需要小心。类似于我们在云数据仓库领域看到的情况，我们开始看到成本失控。因此，如果你的公司有基于使用的定价，你将如何建立财务控制和治理机制，以确保你的成本可预测且不会失控？

# 业务指标

我之前提到过这一点，但你可以采取的最重要的监控和控制措施与业务指标有关。你的公司需要时刻关注你的模型对业务的实际影响。如果你将其用于关键业务流程，你有哪些服务水平协议保证来确保正常运行？

偏见是任何AI模型面临的重大问题，但在生成性AI中可能更为严重。你如何检测模型输出是否存在偏见，并且是否在延续不平等现象？Tim O’Reilly 写了一篇很棒的博客，标题为 [我们已经放出了瓶中的精灵](https://www.rockefellerfoundation.org/blog/we-have-already-let-the-genie-out-of-the-bottle/)，我鼓励你阅读。

从知识产权的角度来看，你如何保证专有、敏感或个人信息不会从你的组织中泄漏或流出？考虑到目前关于版权侵权的诉讼，这是你们组织需要面对的重要因素。你是否应该要求供应商保证这些信息不会出现在你的模型中，类似于Adobe的举措 ([FastCompany — Adobe如此自信其Firefly生成性AI不会侵犯版权，以至于愿意承担你的法律费用](https://www.fastcompany.com/90906560/adobe-feels-so-confident-its-firefly-generative-ai-wont-breach-copyright-itll-cover-your-legal-bills))？现在，他们愿意承担你的法律费用虽然很不错，但这会给你的公司带来什么声誉风险？如果你失去了客户的信任，你可能永远无法赢回他们。

最后，数据中毒无疑是一个热门话题。当你使用你们组织的数据来调整和微调模型时，如何确保数据不是有毒的？如何确保用于训练基础模型的数据没有被中毒？

# 总结

归根结底，这并不是为了提供解决GenAIOps的具体方法和指标，而是提出一系列组织在实施LLM之前需要考虑的问题。像任何事物一样，生成性AI具有帮助组织获得竞争优势的巨大潜力，但也存在一系列需要解决的挑战和风险。最终，GenAIOps需要一套跨越采纳组织和提供LLM的供应商的原则和能力。用蜘蛛侠的话说，能力越大，责任越大。

如果你想了解更多关于人工智能的内容，可以查看我的书《人工智能： [让AI为你的业务服务的执行指南](https://www.amazon.com/Artificial-Intelligence-Executive-Guide-Business/dp/B09X4KTYS4)》。

[1]Sweenor, David, Steven Hillion, Dan Rope, Dev Kannabiran, Thomas Hill, 和 Michael O’Connell. 2020\. 《ML Ops: 数据科学的操作化》。O’Reilly Media. [https://www.oreilly.com/library/view/ml-ops-operationalizing/9781492074663/](https://www.oreilly.com/library/view/ml-ops-operationalizing/9781492074663/).
