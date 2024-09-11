# OpenAI 的网络爬虫和 FTC 失误

> 原文：[https://towardsdatascience.com/openais-web-crawler-and-ftc-missteps-a14047f4ff69?source=collection_archive---------7-----------------------#2023-08-22](https://towardsdatascience.com/openais-web-crawler-and-ftc-missteps-a14047f4ff69?source=collection_archive---------7-----------------------#2023-08-22)

## OpenAI 推出默认的自动同意爬虫以抓取互联网，而 FTC 进行了一项模糊的消费者欺骗调查

[](https://medium.com/@viggybala?source=post_page-----a14047f4ff69--------------------------------)[![Viggy Balagopalakrishnan](../Images/a3d6b5d26327892108816c0ef125b90d.png)](https://medium.com/@viggybala?source=post_page-----a14047f4ff69--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a14047f4ff69--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a14047f4ff69--------------------------------) [Viggy Balagopalakrishnan](https://medium.com/@viggybala?source=post_page-----a14047f4ff69--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb3366eb9a0cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fopenais-web-crawler-and-ftc-missteps-a14047f4ff69&user=Viggy+Balagopalakrishnan&userId=b3366eb9a0cf&source=post_page-b3366eb9a0cf----a14047f4ff69---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a14047f4ff69--------------------------------) · 11 分钟阅读 · 2023 年 8 月 22 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa14047f4ff69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fopenais-web-crawler-and-ftc-missteps-a14047f4ff69&user=Viggy+Balagopalakrishnan&userId=b3366eb9a0cf&source=-----a14047f4ff69---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa14047f4ff69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fopenais-web-crawler-and-ftc-missteps-a14047f4ff69&source=-----a14047f4ff69---------------------bookmark_footer-----------)![](../Images/ecb37def2d4a6a1adc0328858ef8ff0c.png)

图片由 [Giammarco Boscaro](https://unsplash.com/@giamboscaro?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

随着 AI 采用的急剧上升，数据专业人士越来越需要考虑数据来源。虽然最初一波高性能的 LLM 是使用一种普遍但有争议的数据抓取策略进行训练的，但这种有问题的做法最近[受到关注](https://thisisunpacked.substack.com/p/data-scraping-in-the-spotlight-language-models)，引发了诉讼和数据所有权的问题。本文提供了对这些法律概念的深入理解以及监管机构如何应对这一问题（剧透：效果并不显著）。

***来自 Towards Data Science 编辑的说明：*** *虽然我们允许独立作者根据我们的* [*规则和指南*](/questions-96667b06af5) *发布文章，但我们并不赞同每位作者的贡献。你不应在未寻求专业建议的情况下依赖某位作者的作品。详情请参见我们的* [*读者条款*](/readers-terms-b5d780a700a4) *。*

上周，Open AI（ChatGPT 的制造商）正式宣布了他们的[网络爬虫](https://platform.openai.com/docs/gptbot)——这是一个从互联网上所有网站抓取内容的软件，这些内容随后用于 AI 模型训练。爬虫的存在并不令人惊讶，今天存在着几种合法的网络爬虫，包括索引整个互联网的 Google 爬虫。然而，这还是 OpenAI 首次明确宣布其存在，并提供了一个机制让网站选择退出被抓取。

请注意，爬虫程序默认是**需要主动选择的**，也就是说，你需要明确更改网站上的一段代码来要求爬虫程序不要抓取你的数据。主动选择/退出的默认设置是固定的，通常决定了大多数人的行为，因为大多数人不会费心去更改默认设置。这也是[苹果 iOS14 隐私变更](https://developer.apple.com/documentation/apptrackingtransparency)对数字广告行业产生重大影响的原因。

![](../Images/f6d03e3512cf7163819cb5e0345b61ab.png)

OpenAI 网络爬虫（来源：[OpenAI](https://platform.openai.com/docs/gptbot)）

那么，为什么还要提供退出选项呢？这可能是 OpenAI 对[最近的诉讼](https://cyberscoop.com/openai-lawsuit-privacy-data-scraping/)采取的预防性措施，诉讼指控内容拥有者的版权受到侵犯（如果你想进一步了解，更多关于[数据抓取](https://thisisunpacked.substack.com/p/data-scraping-in-the-spotlight-language-models)的深度文章可以阅读）。ChatGPT 的竞争对手 Google Bard 面临类似挑战，但 Google 尚未宣布相应的解决方案——他们确实提出了如何升级[robots.txt](https://www.rfc-editor.org/rfc/rfc9309.html)来解决这个问题的征求意见（以一些[巧妙的公关写作](https://blog.google/technology/ai/ai-web-publisher-controls-sign-up/)呈现）。

在本文中，我们将深入探讨：

+   OpenAI 爬虫对内容拥有者的影响

+   FTC 目前对 OpenAI 的调查

+   我们目前所处的法律环境

+   为什么 FTC 追究 OpenAI 的做法是（又一个）错误的步骤

# OpenAI 的爬虫对内容拥有者的影响

尽管公告为广告商提供了一个选项，可以阻止 OpenAI 的爬虫抓取他们的数据，但仍有几个问题：

1.  默认情况下是选择加入的，这意味着 OpenAI 可以继续抓取，直到网站明确告诉他们不要抓取

1.  **关于在未经同意的情况下抓取数据用于模型训练时内容拥有者的权利，尚未有明确的法律裁决**（这基本上适用于所有被迫默认选择加入的情况）

目前，有两个法律构架决定了语言模型是否可以在未获同意的情况下获取所有这些数据——版权和公平使用。

版权（在[美国版权法第 102 条](https://www.copyright.gov/title17/92chap1.html#102)中）为特定类型的内容提供保护，但也有例外：

> 版权保护根据本标题存在于任何有形的表达媒介中固定的原创作品中，无论现在已知还是以后开发，从中可以被感知、复制或以其他方式传达，无论是直接还是借助机器或设备。作品的类别包括以下几类：（1）文学作品；（2）音乐作品，包括任何附带的文字；（3）戏剧作品，包括任何附带的音乐；（4）哑剧和舞蹈作品；（5）图画、图形和雕塑作品；（6）电影和其他视听作品；（7）声音录音；（8）建筑作品。
> 
> （b）在任何情况下，原创作品的版权保护**不会扩展到任何想法、程序、过程、系统、操作方法、概念、原则或发现**，无论以何种形式描述、解释、插图或体现于该作品中

例如，版权保护大多数原创作品（例如，如果你写了一篇原创的博客文章或书籍），但**不保护广泛的思想**（例如，你不能声称你是第一个写关于 AI 如何影响数据权利的人，因此这个想法属于你）。

版权保护的另一个例外是公平使用（[美国版权法第 107 条](https://www.copyright.gov/title17/92chap1.html#107)）：

> 对于受版权保护的作品的公平使用，包括通过复制或其他该节指定的方式使用该作品用于**批评、评论、新闻报道、教学（包括用于课堂使用的多份副本）、学术研究或研究，并不构成版权侵权。**
> 
> 在确定任何特定情况下对作品的使用是否属于合理使用时，需要考虑的因素包括（1）使用的目的和性质，包括**这种使用是否具有商业性质**或是否用于非营利性教育目的；（2）受版权保护作品的性质；（3）使用部分相对于受版权保护作品整体的数量和实质性；以及（4）使用对潜在市场或受版权保护作品价值的影响。

例如，如果你从一篇研究论文中提取了内容并写了评论，这是可以的，并且你不会侵犯内容所有者的版权。当我从此页面链接到另一篇文章并添加该文章的引用文本时，情况也是一样的。

这两个概念的创建旨在保护内容所有者的权利，同时也允许信息的自由流动，特别是在教育、研究和评论的背景下。

我不是法律专家，但根据我对上述语言的研究/理解，**在AI模型抓取训练内容时，情况变得模糊**：

+   AI公司通常会从内容所有者的网站上抓取全文（这些是受版权保护的），训练模型以学习“思想”/“概念”/“原理”（这些是不受版权保护的），然后模型最终会生成不同的文本。在这种情况下，内容所有者是否会获得版权保护？

+   由于训练后的语言模型现在最终用于商业目的（例如，ChatGPT Plus 是一款付费产品），这是否违反了内容所有者的版权（因为公平使用例外不再适用）？

目前尚未有法院对此作出裁决，因此很难预测结果。我这个非律师的观点是，第二种情况可能更容易解决：OpenAI抓取了数据并用它创建了商业产品，因此他们不适用公平使用的例外。我想象第一种情况（模型是训练在“思想”上还是仅仅是原始文本上）则无人能知晓。请注意，这两个条件都需要对内容所有者有利，内容所有者只有在上述两个例外（“思想”例外或公平使用例外）都不适用于OpenAI时才能获胜。

我提到这个细微差别是因为在人工智能风险的范围内（并不详尽）——从内容所有者的权利、放大欺诈、自动化工作到AGI / 人类毁灭——**最紧迫的短期问题是内容所有者的权利**，这一点可以从大量的诉讼和对内容平台的影响（例如 [StackOverflow的故事](https://thisisunpacked.substack.com/p/stackoverflow-pivot-overflowai)）中看出。

虽然像FTC这样的监管机构可以考虑真正的长期问题，并提出假设性/创造性的方法来应对这些风险，但它们的真正短期**潜力在于能够应对那些将在5到10年内影响我们的风险。例如版权侵权。**这引出了FTC在做什么。

# FTC对OpenAI的当前调查

在七月中旬，FTC宣布正在调查OpenAI。令其有趣（和令人沮丧）的地方在于[FTC调查的原因](https://www.washingtonpost.com/technology/2023/07/13/ftc-openai-chatgpt-sam-altman-lina-khan/)。ChatGPT的制造商正在被调查，以评估该公司是否违反了**消费者保护法律**，**将个人声誉和数据置于风险之中**。这不合理？你并不孤单。让我们进一步了解一下这一情况的背景。

FTC对AI监管的[最明确立场](https://www.ftc.gov/news-events/news/press-releases/2023/04/ftc-chair-khan-officials-doj-cfpb-eeoc-release-joint-statement-ai)在四月提出：“法律书中没有AI豁免条款，FTC将积极执行法律，以打击不公平或欺骗性行为或不公平的竞争方式。” 随后出现了一些与诽谤相关的问题：电台主持人[马克·沃尔特斯起诉OpenAI](https://www.theverge.com/2023/6/9/23755057/openai-chatgpt-false-information-defamation-lawsuit)因为ChatGPT指控他欺诈非营利组织，一位法学教授被[ChatGPT错误指控性骚扰](https://www.independent.co.uk/tech/chatgpt-sexual-harassment-law-professor-b2315160.html)。

这两种情况对相关人员来说都很糟糕，我对此表示同情。然而，语言模型（如GPT）和基于它们的产品（如ChatGPT）会“产生幻觉”，并且经常不准确。FTC对调查的第一个前提是**—— ChatGPT产生幻觉，从而造成声誉损害。**

在一次激烈的国会听证会上，一位代表（有理）[询问FTC](https://www.washingtonpost.com/technology/2023/07/13/ftc-openai-chatgpt-sam-altman-lina-khan/)为什么要追究诽谤和中伤，这些通常由州法律处理。FTC主席丽娜·汗给出了一个[复杂的论点](https://www.washingtonpost.com/technology/2023/07/13/ftc-openai-chatgpt-sam-altman-lina-khan/)：

> 韩表示，诽谤和中伤不是FTC执行的重点，但在AI训练中滥用个人私人信息可能是一种欺诈或欺骗行为，违反FTC法案。韩说：“我们关注的是，‘是否对人们造成了实质性的伤害？’伤害可以表现为各种形式。”

将整个论点总结起来——FTC 认为**ChatGPT 的幻觉生成了不正确的信息（包括诽谤），这可能构成消费者欺骗**。此外，敏感的用户私人信息可能被使用/泄露（基于[一个漏洞](https://openai.com/blog/march-20-chatgpt-outage) OpenAI 已迅速修复）。

作为调查的一部分，FTC 要求 OpenAI 提供一长串信息——包括他们的模型是如何训练的，使用了哪些数据来源，如何向客户展示他们的产品，以及因为识别到风险而暂停发布模型的情况。

问题是——在当前法律环境下，FTC 最好的做法是监管可能成为最大 AI 公司之一的 OpenAI 吗？

# 我们今天所处的法律环境

要批评 FTC 对 OpenAI 的战略，了解我们今天所处的法律环境是很有帮助的。我们不会深入细节，但可以简单地用[反垄断历史](https://www.linkedin.com/pulse/antitrust-note-1-where-did-all-begin-vignesh-balagopalakrishnan/?trackingId=CD63OdXqS%2Ba0WOZqqTBYug%3D%3D)作为例子：

+   在1900年代，巨大的企业集团（“信托”）出现，公共和私人权力的平衡转移到了这些公司手中。

+   为了应对这种情况，1890年的《谢尔曼法》被通过，以对私人权力施加限制并维护竞争；这一法律被用于诉讼并打破从事反竞争行为（掠夺性定价、卡特尔交易、分销垄断）的“信托”。

+   大约在1960年代，法官们因根据法律精神而非法律字面进行裁决而遭遇大量反对。例如，解释《谢尔曼法》以确定一组公司是否“过度限制贸易”涉及主观性，法官们被指责参与司法激进主义。

+   为了引入客观性，芝加哥学派开创了消费者福利标准——“法院应 exclusively 以消费者福利为指导”（例如，垄断者明显提高价格是错误的，但对于其他行为，证明消费者伤害的责任在于监管者）。

+   这种标准今天仍然适用，并且是 FTC 和 DOJ 难以对付大型科技公司的原因之一——例如，FTC 无法主张 Google 提高价格，因为他们的大多数产品是免费的，即使 Google 从事了其他反竞争行为。

从中可以得出的结论是——我们今天继续在一个案件 heavily 基于“法律字面”而非“法律精神”的环境中运作。这一点，加上今天美国最高法院的组成，导致了对法律的相当保守的解释。

对于FTC来说，这意味着要接受这一现状并**找出赢得案件的办法**。FTC和DOJ的运营模式（是合理的）是追求少数几个大案件并实施严格的执法，以便让其他公司在违法之前三思而后行。要实现这一点，**FTC需要在一些关键问题上取得重大胜利**，并且需要在**当前法律环境的限制下制定一个胜利策略**。

# 为什么FTC对OpenAI的追击是（又一次）失误

FTC在对抗大型科技公司方面屡次失败，我认为这些失败都可以归因于一种失败的“我们讨厌一切大型科技公司”的策略，类似于用锤子而非手术刀的方式来对付这些公司。

例如，FTC采取了强硬手段阻止了$69B的微软-动视收购案，但[失败了](https://www.theverge.com/2023/7/20/23795591/ftc-microsoft-activision-administrative-challenge)（可以说是非常惨败）。FTC辩称微软收购动视会杀死游戏市场的竞争。法官写了一份相当直白的判决，驳回了FTC的所有论点，这里有[法官的评论之一](https://storage.courtlistener.com/recap/gov.uscourts.cand.413969/gov.uscourts.cand.413969.305.0_1.pdf)：

> 没有内部文件、电子邮件或聊天记录与微软声明的意图相矛盾，即不将《使命召唤》独占到Xbox主机上。尽管FTC行政程序中进行了大量的发现，包括生产了近100万份文件和30份证词，FTC没有发现任何一份与微软公开承诺将《使命召唤》提供给PlayStation（和Nintendo Switch）相矛盾的文件。

另一个强硬手段的案例是FTC试图阻止Meta收购VR公司Within，[他们失败了](https://techcrunch.com/2023/02/09/meta-acquires-within-despite-ftc-concerns/)。他们为什么要这样做？他们想测试一下是否有意阻止在特定市场变得庞大之前的收购，而鉴于当前的法律环境，这个尝试不出所料地被驳回了。

FTC对OpenAI的调查问题类似：

1.  他们在追究（在我看来）一个相当琐碎的问题以及语言模型的已知局限——幻觉；他们应该将精力集中在5至10年内真正重要的人工智能问题上，例如版权。

1.  尽管当前法律环境中多种“创造性”的法律途径被否决，他们仍在尝试另一种创造性的论点：幻觉 → 诽谤 → 消费者欺诈

对他们行动的宽泛解释是，他们想为他们的“人工智能不免于现有法律”立下先例，而这场徒劳的追逐战使他们从OpenAI那里获得了大量自我报告的数据（FTC发布了[20页的要求](https://www.washingtonpost.com/documents/67a7081c-c770-4f05-a39e-9d02117e50e8.pdf?itid=lk_inline_manual_4)）。

然而，鉴于他们一再采用暴力手段/任何大科技公司的非竞争性方法，并结合那些在法院被一再驳回的创意论点，我认为FTC在这个案件中并未赢得应有的信任。

# 结论

我绝对认为OpenAI应该受到监管。这不是因为他们的LLM会出现幻觉（当然会），而是因为他们公然未经许可使用创作者的内容。这不是因为它会改变过去，而是因为它将有助于为创作者建立一个健康的未来，在那里他们的内容所有权得到保护（法院是否会将现状视为版权侵权还有待观察）。

如果FTC继续重复其错误，采用“铁锤而非手术刀”的方法，这种情况不会改变。针对大科技公司的手术刀方法有明确的成功先例，其中最著名的是英国竞争与市场管理局。他们赢得的两个大案件集中在特定的反竞争机制上：[阻止谷歌对其广告技术堆栈中的自身产品给予优待](https://competitionandmarkets.blog.gov.uk/2023/04/28/googles-privacy-sandbox-commitments-implementation-and-what-comes-next/)，以及允许[其他支付提供商](https://www.reuters.com/world/uk/uk-watchdog-looks-googles-proposals-giving-freedom-app-developers-playstore-2023-04-19/)进行应用内支付。

如果FTC继续走当前的道路，他们的败绩将激励科技公司继续为所欲为，因为他们知道他们可以赢得官司。FTC是时候反思自己的失败，向其他监管机构学习成功经验，并进行调整。

🚀 如果你喜欢这篇文章，可以考虑订阅[**我的每周通讯**](https://thisisunpacked.substack.com/)**。** 每周，我都会发布一篇**深度分析** **关于当前科技话题/产品策略**的文章，阅读时间大约为10分钟。祝好，Viggy。

[](https://thisisunpacked.substack.com/?source=post_page-----a14047f4ff69--------------------------------) [## Unpacked | Viggy Balagopalakrishnan | Substack

### 针对当前科技和商业话题的深度分析，帮助你保持领先。每周送到你的邮箱…

thisisunpacked.substack.com](https://thisisunpacked.substack.com/?source=post_page-----a14047f4ff69--------------------------------)
