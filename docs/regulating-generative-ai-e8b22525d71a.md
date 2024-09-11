# 监管生成式AI

> 原文：[https://towardsdatascience.com/regulating-generative-ai-e8b22525d71a?source=collection_archive---------4-----------------------#2023-08-08](https://towardsdatascience.com/regulating-generative-ai-e8b22525d71a?source=collection_archive---------4-----------------------#2023-08-08)

## 大型语言模型（LLM）在多大程度上遵守欧盟AI法案？

[](https://medium.com/@davidsweenor?source=post_page-----e8b22525d71a--------------------------------)[![David Sweenor](../Images/7dbb5c549ab67bc78f906fb707969ff6.png)](https://medium.com/@davidsweenor?source=post_page-----e8b22525d71a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e8b22525d71a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e8b22525d71a--------------------------------) [**David Sweenor**](https://medium.com/@davidsweenor?source=post_page-----e8b22525d71a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fec7aed1f3ef1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregulating-generative-ai-e8b22525d71a&user=David+Sweenor&userId=ec7aed1f3ef1&source=post_page-ec7aed1f3ef1----e8b22525d71a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e8b22525d71a--------------------------------) ·7分钟阅读·2023年8月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe8b22525d71a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregulating-generative-ai-e8b22525d71a&user=David+Sweenor&userId=ec7aed1f3ef1&source=-----e8b22525d71a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe8b22525d71a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregulating-generative-ai-e8b22525d71a&source=-----e8b22525d71a---------------------bookmark_footer-----------)![](../Images/60e39e9aeb8d30a09e4dab9420ebc048.png)

图片由作者提供 — **David. E. Sweenor**

随着生成式人工智能（AI）成为焦点，对于监管这一技术的呼声日益增高，因为它可能会迅速对大量人群产生负面影响。其影响可能表现为歧视、延续刻板印象、侵犯隐私、负面偏见，并削弱基本人类价值观。

2023年6月，美国政府发布了一套[自愿性AI指导方针](https://www.whitehouse.gov/briefing-room/statements-releases/2023/07/21/fact-sheet-biden-harris-administration-secures-voluntary-commitments-from-leading-artificial-intelligence-companies-to-manage-the-risks-posed-by-ai/)，几家知名公司同意遵循，包括Anthropic、Meta（Facebook）、Google、Amazon、OpenAI和Microsoft等。[1] 这对美国来说是一个重要步骤，但不幸的是，美国在AI法规方面一直落后于欧盟。在我之前的文章[生成AI伦理：在自主内容时代的关键考量](https://medium.com/towards-data-science/generative-ai-ethics-b2db92ecb909)中，我探讨了欧盟的AI伦理框架，并提供了一些在使用大型语言模型（LLM）时实施该框架的考虑。这篇博客专注于欧盟AI法案草案及LLM如何遵守该草案法规。

# 欧盟AI法案

2023年6月，[欧盟通过了世界上第一部AI草案法规](https://www.europarl.europa.eu/news/en/headlines/society/20230601STO93804/eu-ai-act-first-regulation-on-artificial-intelligence)。在2019年批准的AI伦理框架基础上，欧盟的优先任务是确保在欧盟使用的AI系统“安全、透明、可追溯、非歧视性和环境友好”。[2] 为了避免不利后果，欧盟框架坚持要求人类参与AI系统。换句话说，公司不能简单地让AI和自动化运行自己。

提议的法律将AI根据其对人类可能构成的风险分为三个不同的类别 — 每个风险级别都需要不同程度的监管。如果这一计划获得批准，它将成为世界上第一个AI法规。欧盟识别的三个风险层级是：不可接受的风险、高风险和有限风险。

1.  **不可接受的风险**：使用可能对人类有害并构成威胁的技术将被禁止。此类例子可能包括对个体或某些弱势群体的认知影响；基于社会地位对人们进行排名；以及大规模使用面部识别进行实时监视和远程身份识别。现在，我们都知道世界各国的军事力量正在专注于自主武器，但我在这里岔开了话题。

1.  **高风险**：欧盟对可能对安全或基本权利和自由产生有害影响的AI系统进行了分类。第一类是嵌入在零售产品中的AI，目前属于欧盟的产品安全法规范范围。这包括玩具、飞机、汽车、医疗设备、电梯等。第二类需要在欧盟数据库中注册。这包括生物识别技术、关键基础设施运营、培训与教育、与就业相关的活动、警务、边境控制以及法律分析等技术。

1.  **有限风险**：至少，低风险系统必须符合透明度和开放性的标准，以便让人们有机会做出明智的决策。欧盟规定，用户在与 AI 互动时应被通知。他们还要求模型的创建方式应避免生成非法内容，并要求模型制作者披露训练中使用的（如果有的话）版权材料。

欧盟 AI 法案接下来需要在成员国之间进行谈判，以便他们能够对法律的最终形式进行投票。欧盟计划在年底（2023）完成批准。

现在，让我们来看看当前 LLM 如何遵守草案法案。

# LLM 对草案 EU AI 法案的合规性

斯坦福大学的基础模型研究中心（CRFM）和以人为本的人工智能研究所（HAI）最近发布了一篇题为[《基础模型是否符合草案 EU AI 法案》](https://crfm.stanford.edu/2023/06/15/eu-ai-act.html)的论文。他们从该法案中提取了二十二项要求，对其进行了分类，并为其中十二项要求创建了一个 5 分制的评分标准。所有研究内容，包括标准、评分标准和分数，都可以在[GitHub 上 MIT 许可证下获取](https://github.com/stanford-crfm/TransparencyIndex/tree/main)。

研究团队将立法要求映射到表 1.1 中所示的类别中。需要注意的是，团队仅评估了二十二项要求中的十二项。最终，团队选择了基于公开数据和模型制作者提供的文档中最易评估的十二项要求。

**表 1.1：LLM 合规性表总结**

![](../Images/086eb000e4bf00adbff2e28588c8db82.png)

来源：斯坦福 CRFM 和 HAI 评分卡。使用 MIT 许可证。

对于那些可能不太了解的人，斯坦福团队还 painstakingly 编制了超过一百个 LLM 数据集、模型和应用程序，这些内容可以在他们的[生态系统图](https://crfm.stanford.edu/ecosystem-graphs/index.html?mode=table)上找到。为了便于管理，研究人员分析了“10 个基础模型提供者及其旗舰基础模型，与 12 项法案要求进行对比，基于他们的[评分标准]”。

研究人员审视了来自 OpenAI、Anthropic、Google、Meta、Stability.ai 等公司的模型。根据他们的分析，研究结果得出了以下评分卡。

**图 1.2：基础模型提供者对草案 EU AI 法案的合规性评分**

![](../Images/39d25951b3aa03fde8389628e2d79ae5.png)

来源：斯坦福 CRFM 和 HAI 评分卡。使用 MIT 许可证。

总体而言，研究人员指出，各个提供商之间模型合规性存在相当大的差异（这些仅是二十二个要求中的十二个），其中“一些提供商得分低于25%（AI21 Labs、Aleph Alpha、Anthropic），目前只有一个提供商得分至少为75%（Hugging Face/BigScience）。”[4]

我鼓励你阅读完整的研究报告，但研究人员指出，各个提供商之间还有相当大的改进空间。他们还识别出几个关键的“持续挑战”，包括：

● **模糊的版权问题**：大多数基础模型的训练数据来源于互联网，其中很大一部分可能受到版权保护。然而，大多数提供商并未明确训练数据的版权状态。使用和复制受版权保护的数据的法律影响，特别是在考虑许可条款时，并不明确，目前在美国正在积极诉讼中（参见 [华盛顿邮报 — AI 从他们的工作中学习了，现在他们想要赔偿](https://www.washingtonpost.com/technology/2023/07/16/ai-programs-training-lawsuits-fair-use/) [路透社 — 美国法官发现艺术家对 AI 公司诉讼存在缺陷](https://www.reuters.com/legal/litigation/us-judge-finds-flaws-artists-lawsuit-against-ai-companies-2023-07-19/)）我们还需要观察这件事如何发展。

● **缺乏风险缓解披露**：如引言所述，AI有可能迅速对许多人产生负面影响，因此了解LLM的风险至关重要。然而，几乎所有的基础模型提供商都忽视了草案立法中识别的风险披露。虽然许多提供商列出了风险，但很少有提供商详细说明他们采取了哪些措施来缓解识别出的风险。尽管不是生成性AI案例，但最近对美国健康保险公司Cigna Healthcare提起了诉讼，指控他们使用AI来拒绝支付（[Axios — AI 诉讼扩展到健康领域](https://www.axios.com/2023/07/25/ai-lawsuits-health-cigna-algorithm-payment-denial)）。比尔·盖茨写了一篇题为 [AI 的风险是真实的但可以管理的](https://www.gatesnotes.com/The-risks-of-AI-are-real-but-manageable) 的好文章，推荐你阅读。

● **评估和审计不足**：在评估基础模型的性能，特别是在潜在误用或模型鲁棒性等领域，缺乏一致的基准。美国的《芯片与科学法案》要求国家标准与技术研究院（NIST）为AI模型创建标准化评估。我最近讨论的 [GenAIOps 框架](https://medium.com/towards-data-science/genaiops-evolving-the-mlops-framework-b0012f936379) 就专注于评估和监控模型。最终，我们将看到GenAIOps、DataOps和DevOps在一个共同的框架下汇聚，但我们还需一段时间才能实现。

● **不一致的能源消耗报告**：我认为我们许多人都经历过最近全球范围内的热浪。对于大型语言模型（LLMs）来说，基础模型提供商在报告能源使用和相关排放方面差异很大。事实上，研究人员引用了其他研究，表明我们甚至不知道如何测量和计算能源使用。Nnlabs.org 报告了以下内容：“根据 OpenAI 的数据，具有 15 亿个参数的 GPT-2 需要 355 年的单处理器计算时间，并消耗了 28,000 千瓦时的能源进行训练。相比之下，具有 1750 亿个参数的 GPT-3 需要 355 年的单处理器计算时间，并消耗了 284,000 千瓦时的能源进行训练，是 GPT-2 的 10 倍。具有 3.4 亿个参数的 BERT 需要 64 个 TPUs 上训练 4 天，并消耗了 1,536 千瓦时的能源。”[5]

除了上述问题之外，实施生成性人工智能时还有许多其他问题需要解决。

# 总结

根据研究，生成性人工智能技术的提供者和采纳者仍面临很长的道路。立法者、系统设计者、政府和组织需要共同努力，解决这些重要问题。作为起点，我们可以确保在设计、实施和使用人工智能系统时保持透明。对于受监管的行业，这可能是一个挑战，因为大型语言模型（LLMs）往往拥有数十亿个参数。数十亿！在如此众多的因素下，如何确保系统的可解释性和透明度？这些系统需要具备清晰、明确的文档，并且我们需要尊重知识产权。为了支持环境、社会和公司治理（ESG），我们还需要设计一个标准化的能源消耗测量和报告框架。最重要的是，人工智能系统需要安全、尊重隐私，并维护人类价值观。我们需要采取以人为本的人工智能方法。

如果你想了解更多关于人工智能的内容，可以查看我在亚马逊上的书籍 [《人工智能：使 AI 为你的业务服务的高管指南》](https://www.amazon.com/Artificial-Intelligence-Executive-Guide-Business/dp/B09X4KTYS4) 或 [Google Play 上的 AI 解说有声书](https://play.google.com/store/audiobooks/details/Artificial_Intelligence_An_Executive_Guide_to_Make?id=AQAAAECisyHzkM&hl=en_US&gl=US&pli=1)。

[1] Shear, Michael D., Cecilia Kang, 和 David E. Sanger. 2023年。“在拜登的压力下，人工智能公司同意对新工具设立保护措施。”《纽约时报》，2023年7月21日，U.S. 部分 [https://www.nytimes.com/2023/07/21/us/politics/ai-regulation-biden.html](https://www.nytimes.com/2023/07/21/us/politics/ai-regulation-biden.html)。

[2] 欧洲议会. 2023年。“欧盟AI法案：首个人工智能法规 | 新闻 | 欧洲议会。” [www.europarl.europa.eu.](http://www.europarl.europa.eu.) 2023年8月6日。[https://www.europarl.europa.eu/news/en/headlines/society/20230601STO93804/eu-ai-act-first-regulation-on-artificial-intelligence](https://www.europarl.europa.eu/news/en/headlines/society/20230601STO93804/eu-ai-act-first-regulation-on-artificial-intelligence)。

[3] Bommasani, Rishi, Kevin Klyman, Daniel Zhang, 和 Percy Liang. 2023年。“斯坦福CRFM。” Crfm.stanford.edu. 2023年6月15日。[https://crfm.stanford.edu/2023/06/15/eu-ai-act.html](https://crfm.stanford.edu/2023/06/15/eu-ai-act.html)。

[4] Bommasani, Rishi, Kevin Klyman, Daniel Zhang, 和 Percy Liang. 2023年。“斯坦福CRFM。” Crfm.stanford.edu. 2023年6月15日。[https://crfm.stanford.edu/2023/06/15/eu-ai-act.html](https://crfm.stanford.edu/2023/06/15/eu-ai-act.html)。

[5] ai. 2023年。“训练现代大语言模型的电力需求。” Nnlabs.org. 2023年3月5日。[https://www.nnlabs.org/power-requirements-of-large-language-models/](https://www.nnlabs.org/power-requirements-of-large-language-models/)。
