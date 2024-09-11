# 做出正确决策：AI 建议、决策辅助工具以及大语言模型的前景

> 原文：[https://towardsdatascience.com/making-the-right-decisions-ai-advice-decision-aids-and-the-promise-of-llms-65b70682ee08?source=collection_archive---------9-----------------------#2023-09-28](https://towardsdatascience.com/making-the-right-decisions-ai-advice-decision-aids-and-the-promise-of-llms-65b70682ee08?source=collection_archive---------9-----------------------#2023-09-28)

## 探索大语言模型带来的决策新时代

[](https://medium.com/@ujwal07?source=post_page-----65b70682ee08--------------------------------)[![乌贾瓦尔·加迪拉朱](../Images/ee7345cf2e7fbf6ee29866659b60b18e.png)](https://medium.com/@ujwal07?source=post_page-----65b70682ee08--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65b70682ee08--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----65b70682ee08--------------------------------) [乌贾瓦尔·加迪拉朱](https://medium.com/@ujwal07?source=post_page-----65b70682ee08--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc529f92bfe0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-the-right-decisions-ai-advice-decision-aids-and-the-promise-of-llms-65b70682ee08&user=Ujwal+Gadiraju&userId=c529f92bfe0d&source=post_page-c529f92bfe0d----65b70682ee08---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----65b70682ee08--------------------------------) · 10 分钟阅读 · 2023年9月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F65b70682ee08&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-the-right-decisions-ai-advice-decision-aids-and-the-promise-of-llms-65b70682ee08&user=Ujwal+Gadiraju&userId=c529f92bfe0d&source=-----65b70682ee08---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F65b70682ee08&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmaking-the-right-decisions-ai-advice-decision-aids-and-the-promise-of-llms-65b70682ee08&source=-----65b70682ee08---------------------bookmark_footer-----------)![](../Images/087d06c143f7cd0a5b5f62c7bc6176c7.png)

照片由[罗伯特·鲁吉耶罗](https://unsplash.com/@robert2301?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/X2bcQMMhaow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

人工智能的民主化导致了AI系统在各种领域的采用。最近一波生成模型，例如预训练的大型语言模型（LLMs），已广泛应用于我们日常生活中的不同活动——从通过帮助撰写邮件提高生产力，到帮助解决令新手和专家作家都感到困扰的“空白页”难题。由于对LLMs在决策中的依赖日益增加，本文提供了人类决策和人类与AI决策演变的综合分析。最后，文章反思了LLMs在辅助决策任务中提供的机会以及对LLMs决策依赖的相关威胁。

# 人类决策

在一个几乎每个日常决策（*例如*，购买食品或穿着衣物、阅读书籍、听音乐或观看电影，从生活方式选择到旅行目的地）的选择范围不断扩大的世界中，决策质量受到了重新关注。在他有影响力的作品中揭示“*选择悖论*”的巴里·施瓦茨（Barry Schwartz）阐述了这种随着千年技术进步而逐渐增加的决策困难。施瓦茨通过一个例子来解释这一点：医生向患者提供一系列治疗方案，传达潜在风险并将其与每种治疗的收益进行权衡。在这种情况下，高风险决策的负担从专家医生转移到非专家患者。除了其他因素外，选择的过多往往会阻碍有效的人类决策。

不同的研究社区，从进化心理学到认知科学和神经科学，已经探索了人类决策的性质以及塑造人类决策过程的各种因素。人类决策受认知偏差困扰，充满了非理性，这一点并不鲜见。这一点在诺贝尔奖获奖行为经济学家丹尼尔·卡尼曼的*《思考，快与慢》*一书中得到了最著名的记录。

![](../Images/b1c22ad1ee60a9ba1c68c1dc31d44dc6.png)

由 [Robynne Hu](https://unsplash.com/@robynnexy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，来源于 [Unsplash](https://unsplash.com/photos/HOrhCnQsxnQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 人类与人工智能的决策

技术的出现带来了决策支持系统的增长，这些系统可以帮助人类克服决策过程中遇到的障碍。决策支持系统在更广泛的社会技术背景下有各种形态和形式——从驱动用户交互的算法到帮助用户进行预测和预报的复杂机器学习模型。例如，推荐系统可以通过向用户展示可能最能满足其需求的内容或产品来提供帮助。其他算法系统则可以挖掘大量数据，以在各种决策任务中向用户提供建议。

所有的人类与AI决策制定背景中一个共同的核心目标是通过将人类智能与算法系统的计算能力相结合，提升决策效果。然而，这与许多协作的人类与AI决策过程在现实世界中的展开方式相去甚远。人类在决策任务中未能适当地依赖AI系统，从而导致团队表现次优。*适当的依赖*被概念化为在人类依赖AI建议时AI正确时的依赖，以及AI不正确时的人类自我依赖[11]。有许多因素在塑造这些结果中发挥作用——人类因素（例如，领域知识、对技术交互的亲和力、先前经验）；系统因素（例如，AI系统的准确性或信心）；任务因素（例如，任务复杂性、任务不确定性、风险）。

对人类与AI在各种背景下的决策制定的实证探索，包括贷款申请决策和医学诊断，已揭示出人类要么对AI建议的依赖不足，错失了改善决策结果的机会，要么对AI建议的依赖过度，从而导致次优结果。为了应对过度依赖和不足依赖的问题，并促进对AI建议的适当依赖，先前的研究提出了使用解释[13]、*认知强制功能*（即，在决策过程中强制进行关键考虑和反思的干预措施）[4]、传达AI系统优缺点的教程或培训课程，以及提高人群一般AI素养的倡议。最近的研究提出了一种名为“*评估性AI*”的替代框架，以促进对AI建议的适当依赖。该框架建议决策支持工具应提供支持和反对人们所做决策的证据，而不是提供接受或拒绝的推荐[7]。

认知偏差也影响了人机决策[1, 3]。Rastogi等人[9]认为，我们对决策任务的整体感知和理解可能会受到认知偏差的扭曲，例如确认偏差、锚定偏差和可得性偏差。他们探讨了锚定偏差的作用，并提出了减少其对协作决策表现负面影响的方法。He等人[22]展示了厄尔斯效应（一种元认知偏差）如何影响人们对AI系统建议的依赖。他们揭示了那些高估自己能力或表现的用户倾向于对AI系统表现出过少的依赖，从而阻碍了决策任务中的最佳团队表现。其他因素，如算法厌恶和欣赏，也被证明对人机决策的成果有影响[17]。

尽管在人机协作的广泛领域中正在进行持续的工作，但在决策任务中如何适当地依赖AI系统仍然是一个未解的问题。人工智能、机器学习和人机交互交汇的不同研究社区正积极推进我们对这一领域的理解，并开发方法、工具和框架，帮助我们从人机协作的潜力中受益。

目前，大型语言模型（LLMs）在各个领域得到了广泛的应用和采纳。在本文的余下部分，我们将深入探讨LLMs在辅助人类决策中提供的机会以及潜在的好处。

# LLMs用于决策任务

尽管大型语言模型（LLMs）表现出偏见和可能造成伤害的潜在风险，它们在各种社会技术系统中的使用仍在不断增加。话虽如此，它们也显示出了在大规模上产生积极影响的潜力——例如，通过支持审计过程，如Rostagi等人[8]所展示的，通过一个由生成型LLM驱动的审计工具。作者提议在商业语言模型的协同审计中利用人类和生成模型的互补优势。Wu等人[14]提出了*AutoGen*，一个通过多代理对话支持复杂LLM-based工作流程的框架。AutoGen可以支持在线决策任务，如游戏或网络互动。

一方面，有证据表明，像GPT-3这样的LLMs表现出与人类直觉惊人相似的行为，以及随之而来的认知误差[16]。最近的研究强调了使用ChatGPT进行放射学决策的可行性，潜在地改善临床工作流程和放射学服务的负责任使用[18]。为了增强决策过程中的AI安全性，Jin等人[15]旨在复制并赋予LLMs在新颖或异常情况下决定是否打破规则的能力。另一方面，LLMs可能无意中加剧对边缘化群体的刻板印象[20]，并表现出涉及种族、性别、宗教和政治取向的偏见。与决策支持系统中培养适当信任和依赖的挑战类似，如果人类依赖LLMs做出决策，我们将需要更好地理解这种互动的利与弊。在LLM驱动的互动中，尤其突出的风险是通过使用解释在决策任务中创造的解释深度的假象，导致对AI系统的过度依赖。如果人类与决策支持系统的互动变得更加“无缝”（例如通过交互式或对话界面），我们可以预期会发现更多不适当依赖的实例。

通过它们的架构和超参数的视角研究LLMs变得越来越困难。此时有足够的证据表明，生成式AI可以产生高质量的书面和视觉内容，这些内容可以为社会福祉服务，也可能被误用以造成伤害。Potsdam Mann等人[19]认为，为LLMs输出分配责任存在信誉-责备不对称性，并且伦理和政策影响集中在LLMs上。

# **接下来需要做什么？**

显然，在决策任务中安全且健壮地使用LLMs需要更多的研究和实证工作。特别是考虑到目前在多模态和多语言LLMs方面的限制。以下是一些仍然至关重要的问题汇编，用于确定我们可以从将LLMs嫁接到日常决策中中持续获益的程度：

+   *如何促进对LLMs或LLM注入系统进行有效决策支持的适当依赖？*

+   *我们如何提高LLM注入的决策支持系统的稳健性、可靠性和信任度？*

+   *如何在多模态和多语言决策环境中培养对LLMs的适当信任和依赖？*

+   *如何支持不同能力、个人特征、先验知识、教育和资格以及其他人口统计学特征的人在LLMs决策任务中得到平等的支持？*

**因此，如果你手边有一个大语言模型，还是不要急于把它当作黑箱决策辅助工具来依赖！**

> Dr. ir. Ujwal Gadiraju 是[*代尔夫特理工大学*](https://www.tudelft.nl/en/)的终身副教授。他共同指导了[Delft “Design@Scale” AI实验室](https://www.tudelft.nl/en/ai/design-at-scale-lab)并领导了一个以人为本的AI与众包计算研究方向。他是ACM的杰出演讲者，也是[*CHI Netherlands*](https://chinederland.nl/)的董事会成员。Ujwal在[Toloka AI](https://toloka.ai/)的AI、数据和研究团队中花费部分时间工作，同时也是[Deeploy](https://www.deeploy.ml/)的顾问委员会成员，Deeploy是一家正在成长的MLOps公司。

# 参考文献

1.  Bertrand, A., Belloum, R., Eagan, J. R., & Maxwell, W. (2022年7月). *认知偏差如何影响XAI辅助决策：系统评估.* 见于2022 AAAI/ACM AI、伦理与社会会议论文集（第78–91页）。

1.  Bossaerts, P., & Murawski, C. (2017). *计算复杂性与人类决策.* 认知科学趋势, 21(12), 917–929。

1.  Boonprakong, N., He, G., Gadiraju, U., van Berkel, N., Wang, D., Chen, S., Liu, J., Tag, B., Goncalves, J. 和 Dingler, T., 2023\. *关于理解和缓解人类与AI协作中的认知偏差的研讨会。*

1.  Buçinca, Z., Malaya, M. B., & Gajos, K. Z. (2021). *信任还是思考：认知强制函数可以减少对AI的过度依赖.* ACM人机交互学报, 5(CSCW1), 1–21。

1.  Haupt, C. E., & Marks, M. (2023). *AI生成的医学建议——GPT及其超越。* Jama, 329(16), 1349–1350。

1.  Kahneman, D. (2011). *思考，快与慢。* 麦克米伦。

1.  Miller, T. (2023年6月). *可解释AI已死，万岁可解释AI！使用评估性AI的假设驱动决策支持.* 见于2023 ACM公平性、问责性与透明度会议论文集（第333–342页）。

1.  Rastogi, C., Tulio Ribeiro, M., King, N., Nori, H., & Amershi, S. (2023年8月). *支持人类与AI协作的审计LLMs.* 见于2023 AAAI/ACM AI、伦理与社会会议论文集（第913–926页）。

1.  Rastogi, C., Zhang, Y., Wei, D., Varshney, K. R., Dhurandhar, A., & Tomsett, R. (2022). *快速与缓慢决策：认知偏差在AI辅助决策中的作用.* ACM人机交互学报, 6(CSCW1), 1–22。

1.  Santos, L. R., & Rosati, A. G. (2015). *人类决策的进化根源.* 心理学年鉴, 66, 321–347。

1.  Schemmer, M., Hemmer, P., Kühl, N., Benz, C., & Satzger, G. (2022). *我是否应该听从基于AI的建议？衡量人类与AI决策中的适当依赖性.* arXiv预印本 arXiv:2204.06916。

1.  Schwartz, B. (2004). *选择的悖论：为何更多更少。* 纽约。

1.  Vasconcelos, H., Jörke, M., Grunde-McLaughlin, M., Gerstenberg, T., Bernstein, M. S., & Krishna, R. (2023). *解释可以减少在决策过程中对AI系统的过度依赖*。ACM人机交互学报，7(CSCW1)，1–38。

1.  Wu, Qingyun, Gagan Bansal, Jieyu Zhang, Yiran Wu, Shaokun Zhang, Erkang Zhu, Beibin Li, Li Jiang, Xiaoyun Zhang, 和 Chi Wang. *AutoGen：通过多代理对话框架启用下一代LLM应用*。arXiv预印本 arXiv:2308.08155 (2023)。

1.  Jin, Z., Levine, S., Gonzalez Adauto, F., Kamal, O., Sap, M., Sachan, M., Mihalcea, R., Tenenbaum, J. 和 Schölkopf, B., 2022. *何时例外：将语言模型作为人类道德判断的解释*。神经信息处理系统进展，35， 第28458–28473页。

1.  Hagendorff, T., Fabi, S., & Kosinski, M. (2022). *机器直觉：揭示GPT-3.5中的类人直观决策*。arXiv预印本 arXiv:2212.05206。

1.  Erlei, A., Das, R., Meub, L., Anand, A., & Gadiraju, U. (2022年4月). *值得关注的是：人类会忽略其经济自利以避免与AI系统讨价还价*。见于2022年CHI计算机系统人因会议论文集（第1–18页）。

1.  Rao, A., Kim, J., Kamineni, M., Pang, M., Lie, W., & Succi, M. D. (2023). *评估ChatGPT作为放射科决策的辅助工具*。medRxiv, 2023–02。

1.  Porsdam Mann, S., Earp, B. D., Nyholm, S., Danaher, J., Møller, N., Bowman-Smart, H., … & Savulescu, J. (2023). *生成式AI涉及信贷–责备不对称*。自然机器智能，1–4。

1.  Dhingra, H., Jayashanker, P., Moghe, S., & Strubell, E. (2023). *酷儿首先是人：解构大语言模型中的性别身份刻板印象*。arXiv预印本 arXiv:2307.00101。

1.  He, G., Kuiper, L., & Gadiraju, U. (2023年4月). *了解认知：人类能力的幻觉可能阻碍对AI系统的适当依赖*。见于*2023年CHI计算机系统人因会议论文集*（第1–18页）。
