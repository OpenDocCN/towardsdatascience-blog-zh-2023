# 微软与谷歌：语言模型是否会超越搜索引擎？

> 原文：[`towardsdatascience.com/microsoft-vs-google-will-language-models-overtake-search-engines-4e9590bed232`](https://towardsdatascience.com/microsoft-vs-google-will-language-models-overtake-search-engines-4e9590bed232)

## 评论

## 如果微软更名 Bing，我会考虑……

[](https://albertoromgar.medium.com/?source=post_page-----4e9590bed232--------------------------------)![Alberto Romero](https://albertoromgar.medium.com/?source=post_page-----4e9590bed232--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e9590bed232--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e9590bed232--------------------------------) [Alberto Romero](https://albertoromgar.medium.com/?source=post_page-----4e9590bed232--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e9590bed232--------------------------------) ·阅读时间 12 分钟·2023 年 1 月 10 日

--

![](img/3afa838fd9ba9a0c93096c984da15504.png)

巨头之战。图片来源：Midjourney

有传言称微软已经启动了一个项目，预计将在未来几年内对科技领域产生影响。正如[The Verge 的 Tom Warren 所写](https://www.theverge.com/2023/1/4/23538552/microsoft-bing-chatgpt-search-google-competition)的那样，这家软件巨头“据说计划推出一个使用 ChatGPT 的 Bing 版本。”二十年来，搜索领域即将迎来一场革命。

这条不令人惊讶的消息是在微软的合作伙伴 OpenAI 发布了[ChatGPT](https://openai.com/blog/chatgpt/)，一个为对话优化的强大语言模型之后一个月传出的，我认为这是[世界上最好的聊天机器人](https://thealgorithmicbridge.substack.com/p/chatgpt-is-the-worlds-best-chatbot)——虽然可能不会持续太久。

在 ChatGPT 于 11 月 30 日发布之后，人们迅速意识到其存在带来了语言模型（LM）在短期内超越传统搜索引擎（SE）作为在线信息检索主要手段的非忽视概率。进一步说，这意味着谷歌在搜索领域二十年的霸主地位可能面临威胁。

微软（尚未正式宣布）的声明重新点燃了语言模型与搜索引擎的辩论，尽管没人知道事件将如何发展，但有一个共识；无论如何，语言模型和搜索在未来很可能将成为一个更大整体中不可分割的部分。

就像重力把我们拉向地面一样，技术自发地流动——如[热力学](https://en.wikipedia.org/wiki/Spontaneous_process#:~:text=In%20thermodynamics%2C%20a%20spontaneous%20process,(closer%20to%20thermodynamic%20equilibrium).)中所用——在一个方向上：让我们的生活更轻松。语言模型更具直观性，与它们的交互对我们来说是自然的。搜索引擎要么改变，要么消亡，这似乎是一个不可避免的结果。

我知道，这听起来像是你典型的泛泛而谈的不可证伪的预测。幸运的是，我们可以揭示一些未知的问题：ChatGPT 是否对 Google 构成真实威胁？微软能否推翻 Google？搜索巨头能否做出适当反应？最终哪家公司会脱颖而出？语言模型会取代搜索吗？还是会补充它？语言模型将在什么方面改善或削弱搜索？这一切将如何以及何时发生？

让我们尝试回答这些问题，了解未来语言模型和搜索引擎将如何互动，微软、谷歌和 OpenAI 对此有何看法，以及我认为未来几个月/几年将如何发展。

*这篇文章摘自* [***算法桥梁***](https://thealgorithmicbridge.substack.com/subscribe?)*，这是一本旨在弥合算法与人们之间差距的教育通讯。它将帮助你理解 AI 对你生活的影响，并发展出更好地应对未来的工具。*

[](https://thealgorithmicbridge.substack.com/subscribe?source=post_page-----4e9590bed232--------------------------------) [## 算法桥梁

### 弥合算法与人们之间的差距。关于对你有意义的 AI 的通讯。点击阅读《The…》

[thealgorithmicbridge.substack.com](https://thealgorithmicbridge.substack.com/subscribe?source=post_page-----4e9590bed232--------------------------------)

# 语言模型与搜索引擎

一位名为 josh 的 Twitter 用户在 ChatGPT 公开的那一天首先指出：“[谷歌完了](https://twitter.com/jdjkelly/status/1598021488795586561)。”其他人，比如现在已退出 Twitter 的[乔治·霍兹](https://twitter.com/realGeorgeHotz/status/1598172113038299137)，也同意这一观点——但并不是所有人都得出了相同的结论。

加里·马库斯教授用[实证证据](https://twitter.com/GaryMarcus/status/1598329961383067649)回应了乔治·霍兹的观点，谷歌的弗朗索瓦·肖莱也指出了类似的问题：“搜索是搜索问题，而不是生成问题：”

[来源](https://twitter.com/fchollet/status/1598544429727776770)

我同意马库斯和肖莱的观点。语言模型本身*无法超越*搜索引擎。然而，搜索引擎可以显著改进，达到那些未集成基于语言模型的特性就会变得过时的地步。

如果我们接受这个假设，很容易看出，最适合将语言模型和搜索结合起来的公司是[谷歌](https://twitter.com/ArthurB/status/1598283958961475586)。不是 OpenAI，不是微软。即便是分别来看，谷歌在这两个领域的世界领先地位也无与伦比。尽管 OpenAI 很受欢迎，GPT-3、ChatGPT 以及所有类似的模型都基于谷歌的技术，而且谷歌的搜索引擎占据了市场份额的 4/5。

如果这家公司没有推出太多的 AI 产品，那是因为其“制度惯性”，正如[Stability 的 Emad Mostaque 所说](https://twitter.com/EMostaque/status/1610609874743738370)。在研究深度和广度方面，谷歌无疑是全球领先的 AI 公司。

然而，正如受欢迎的投资者[Balaji Srinivasan 所解释](https://twitter.com/balajis/status/1560915856955039744)，研究和生产是两种不同的事物：谷歌不能冒着从头重构其搜索引擎以通过语言模型提升其功能的风险。这家公司多年来推出了新的搜索功能和渐进式的改进，但没有像微软——以及像[Perplexity](https://www.perplexity.ai/)、[You](https://you.com/)、和[Neeva](https://neeva.com/)——那样的革命性进展。

[来源](https://twitter.com/balajis/status/1560915856955039744)

我对[语言模型与搜索引擎](https://thealgorithmicbridge.substack.com/i/88242827/is-google-dead)的看法可以总结如下：“搜索引擎的局限性要大得多，但在[搜索网络]的任务上装备更好……[但是] 我认为[传统的]搜索引擎将无法在语言模型面前存活。”这里的关键词——我在原文中没有提到——是“传统的”。

搜索引擎*将会*存在，但它们会变得如此不同，以至于难以辨认。语言模型很可能是原因所在。

（我不会详细讨论将语言模型整合进搜索引擎是否是个好主意。Gary Marcus 有一篇很棒的文章，我几乎完全同意他的观点：“[ChatGPT 是否真的对谷歌搜索构成‘红色警报’?](https://garymarcus.substack.com/p/is-chatgpt-really-a-code-red-for)”）

# 微软与谷歌：技术时代的大战

微软对 OpenAI 的 10 亿美元投资——以及获得的[独家许可证](https://blogs.microsoft.com/blog/2020/09/22/microsoft-teams-up-with-openai-to-exclusively-license-gpt-3-language-model/)——明确表明了其对这一领域的兴趣。毫不奇怪，他们计划将[DALL-E](https://techcrunch.com/2022/10/12/microsoft-brings-dall-e-2-to-the-masses-with-designer-and-image-creator/)和 ChatGPT 整合进他们的服务中。增强版的 Bing 搜索引擎可能会“挑战谷歌的主导地位”，正如 Tom Warren[所写](https://www.theverge.com/2023/1/4/23538552/microsoft-bing-chatgpt-search-google-competition)。

当然，这个想法并不是用语言模型来取代搜索引擎，而是为了补充它。一位微软发言人[告诉彭博社](https://www.bloomberg.com/news/articles/2023-01-04/microsoft-hopes-openai-s-chatbot-will-make-bing-smarter)，“对用户查询的对话式和上下文回应将通过提供超越链接的高质量答案来赢得搜索用户的青睐。”

不像谷歌，微软非常清楚语言模型不如搜索引擎可靠。公司必须评估实现那些人们无法 100%依赖的功能的风险与其在与谷歌竞争中的潜在收益。微软正在“权衡…聊天机器人的准确性[以及]初始发布可能是对少数用户的有限测试。”听起来是一个合理的开始。

然而，如果有人比微软更了解语言模型能做什么和不能做什么，那就是谷歌。在一篇[2021 年的论文](https://arxiv.org/pdf/2105.02274.pdf)中——这篇论文发布的时间远早于 ChatGPT 成为构想的种子——谷歌研究人员探讨了使用语言模型来“重新思考搜索”的问题。

他们考虑了是否可以做到，更重要的是，是否*应该*做到：

> *“传统的信息检索系统 [即传统的搜索引擎] 不直接回答信息需求，而是提供对（希望是权威的）答案的参考。*
> 
> *…*
> 
> *相对而言，预训练语言模型能够直接生成可能响应信息需求的文章，但目前它们只是外行而非领域专家——它们对世界并没有真正的理解，容易产生幻觉，至关重要的是，它们无法通过引用它们所训练过的语料库中的支持文档来证明自己的言论。”*

谷歌最终得出的结论是，使用类似 ChatGPT 的系统来提升其搜索引擎会带来较高的“声誉风险”。首席执行官桑达尔·皮查伊和 AI 负责人杰夫·迪安[告诉 CNBC](https://www.cnbc.com/2022/12/13/google-execs-warn-of-reputational-risk-with-chatgbt-like-tool.html)，“如果出现问题，成本将比 OpenAI 更高，因为人们必须信任他们从谷歌获得的答案。”

谷歌在 2021 年 5 月宣布了 LaMDA（但没有发布）。鉴于 LaMDA 与 ChatGPT 相当——如果不是更好，[正如 Blake Lemoine 所称](https://twitter.com/cajundiscordian/status/1600531100098383875)——有理由质疑为什么谷歌没有利用它来规避像 OpenAI 这样的威胁。Balaji Srinivasan 预测这是因为公司没有足够的“风险预算”，事实证明，他是对的。

像谷歌这样的大公司，为数十亿用户（而非像 OpenAI 那样的几百万用户）提供的高可靠性服务，如谷歌搜索，不能仅仅因为某项新技术似乎是未来趋势并且大家都疯狂追捧就冒险嵌入一个不值得信赖、未经严格测试的技术。

但 Google 高管并不是傻子。他们知道 ChatGPT，尽管由一家规模更小、风险规避程度较低的公司拥有，却确实是一个威胁——尤其是当像微软这样的直接竞争对手在其中拥有大量股份时。这就是为什么他们将 ChatGPT 宣布为“红色警报”，正如 [纽约时报](https://www.nytimes.com/2022/12/21/technology/ai-chatgpt-google-search.html) 报道的那样：

> *“…随着一种新型聊天机器人技术的出现，可能会重新定义甚至取代传统搜索引擎，Google 可能面临对其主要搜索业务的第一次严重威胁。一位 Google 高管描述这些努力是决定 Google 未来的关键。”*
> 
> *…*
> 
> *“Google 必须投入其中，否则行业可能会在没有它的情况下继续发展…”*

就目前的情况而言，Google 面临着微软——在搜索领域是一个强劲的直接竞争对手——以及 OpenAI——拥有类似的 AI 技术，尽管预算要紧张得多——同时，还要平衡由于语言模型固有的不可靠性和它们在低风险规避初创公司手中构成的明显威胁所带来的声誉风险。

正如 [皮查伊所说](https://www.cnbc.com/2022/12/13/google-execs-warn-of-reputational-risk-with-chatgbt-like-tool.html)，Google 必须“大胆且负责任”，并找到一个折衷解决方案。“我们必须做到这一点，这非常重要，”迪恩总结道。

# 我的对事件发展如何展开的预测

鉴于目前的情况，我认为有三个关键点需要关注，以理解将会发生什么以及如何发生。首先，Google 真的在与谁竞争，以至于将“声誉风险”报告为前进的主要障碍？第二，是否可能通过语言模型和当前的 AI 安全/对齐技术“做到正确”？第三，即使可以做到，公司也认为应该这样做，是否能从中衍生出一个可行的商业模式？

## Google 的真正敌人

当我阅读了关于 ChatGPT 威胁的 [皮查伊和迪恩的论点](https://www.cnbc.com/2022/12/13/google-execs-warn-of-reputational-risk-with-chatgbt-like-tool.html) 时，我注意到了一些奇怪的地方：他们似乎在暗示 Google 正在与 OpenAI 竞争。确实，OpenAI 的技术被 Google 高管称为“红色警报”，但我认为 OpenAI 并不是 Google 的威胁——这是错误的框架。

一方面，OpenAI 无法在技术研究和 AI 专业知识方面与 Google 竞争。Google 的预算和人才远远超过 OpenAI——即使只是从数量上来看。正如 [Emad Mostaque 论述的](https://twitter.com/EMostaque/status/1610609874743738370)：

[来源](https://twitter.com/EMostaque/status/1610609874743738370)

另一方面，OpenAI *不想* 与 Google 竞争。

OpenAI 的声誉风险远低于 Google，因为它是一家相对较新且规模较小的公司，最多为几百万用户提供服务，而估计声称 Google 搜索被[全球超过 40 亿人使用](https://www.semrush.com/blog/google-search-statistics/)，占据了令人瞩目的[84%市场份额](https://www.statista.com/statistics/216573/worldwide-market-share-of-search-engines/)。

然而，OpenAI 报告的使命是建立[一种有益的通用人工智能（AGI）](https://openai.com/about/)。他们为什么要冒着与主要目标完全不重叠的大公司竞争的风险呢？

即使 OpenAI 主要关注经济利润（不可否认，取代 Google 会导致一个极其成功的赚钱机器），公司也有更好的选择，不会与其长期目标冲突，比如设置付费订阅或按需付费模式，正如他们现在所做的（例如 GPT-3 和 DALL-E）。

在影响力、规模、预算以及最重要的目标方面，Google 的真正竞争对手是微软。但以这种方式看，Google 面临的 *更高* 声誉风险的论点就会崩溃。微软的用户数量与 Google 相当，公司也必须照顾其精心建立的声誉——正如其[在 2016 年关闭种族主义聊天机器人 Tay 的决定](https://www.cbsnews.com/news/microsoft-shuts-down-ai-chatbot-after-it-turned-into-racist-nazi/)所示。

支持“声誉风险”观点的一点是微软的搜索市场份额与 Google 不可同日而语。然而，如果微软成功地将语言模型和搜索结合起来，他们将增加用户数量，因此声誉风险也会相应增加。

剩下的问题是微软是否愿意决定将 ChatGPT 集成到 Bing 中，冒着声誉风险，因为用户可能因新服务的更强大功能而被吸引，只为有机会取代 Google。

Google 打算如何应对这一点？

## “做对事情”是一个听起来很美好的目标，但实际上不可行。

Jeff Dean 解释 Google 正在等待“做对事情”的说法让我想起了我对将伦理原则融入 AI 模型和对抗虚假信息的重要性的更为乐观的看法。我认为这些努力至关重要，我会继续坚持这一观点，但我能看到，虽然在理论上它们是值得尊敬的，但在实践中几乎变得难以忍受。

在我看来，只有完全重新定义、重新设计和重建语言模型（LMs），才能符合 Dean 所指的那种标准。如果[正如 Gary Marcus 所建议](https://garymarcus.substack.com/p/how-come-gpt-can-seem-so-brilliant)的那样，它们根本不具备真实、准确、可靠和中立的能力，那么无论多少临时性保护措施都无法遏制语言模型数据驱动核心所带来的恶劣本质。

也许，一旦公司尝试将 SE 与 LM 结合，前者的所有关键特性就会被 LMs 的功能设计缺陷所破坏。Marcus 在对 Perplexity、Neeva 和 You 的分析中提供了大量证据。[他的结论](https://garymarcus.substack.com/p/is-chatgpt-really-a-code-red-for)保持了对未来的希望，但目前解决了这一争论：

> *“我能说的最好的事情是，Perplexity.ai 和 you.com 的聊天确实在探索一个有趣的想法：结合经典搜索引擎与大型语言模型的混合体，可能允许更快速的更新。但仍然有* 许多 *工作需要完成，以正确整合这两者，即经典搜索和大型语言模型。”*

另一个问题是当前最先进的 AI 对齐技术是否足够好或朝着正确的目标发展。[Scott Alexander 写了一篇很好的文章](https://astralcodexten.substack.com/p/perhaps-it-is-a-bad-thing-that-the)讨论了通过人类反馈进行强化学习（RLHF）的局限性，而 ChatGPT 使用这种方法，并且似乎是公司阻止 LMs 行为缺陷的唯一方法。

Alexander 直言不讳地说：“RLHF 效果不是很好。”正如我在我的 ChatGPT 文章中所写的，“人们可以‘轻松’[通过其过滤器](https://twitter.com/zswitten/status/1598088267789787136)，它也容易受到[提示注入](https://twitter.com/goodside/status/1598253337400717313)的影响。”经过 RLHF 优化的模型还可能进入优先级冲突的循环。Alexander 表示，“惩罚无用的答案会使 AI 更可能给出错误的答案；惩罚错误的答案会使 AI 更可能给出冒犯性的答案；等等。”同时生成有用、真实且不冒犯的响应可能是不可能的。

此外，如果 LM 在 RLHF 中的改进是渐近的，正如 Alexander 所思考的，我们将永远无法“做到正确”——然而，由于这是表现最佳的方法，公司没有动力花费时间和资源去研究另一种可能——也可能——同样有效的方法。

如果上述所有情况都属实——LMs 本质上不适合搜索，而我们能访问的最佳技术也只是平庸的——那么短期内不会有一个“做到正确”的时刻，就如 Jeff Dean 所希望的那样——而 Google 也需要。

谷歌将面临一个困境：一方面，他们可以让微软领先，承担“声誉风险”，有可能重新定义搜索的未来，成为该领域的下一个霸主。另一方面，他们可以决定“做对”是一个过于宏大的目标，自行承担声誉风险，配合一些公关手段（例如“我们尽了最大努力”）和不成熟的功能（例如“现在效果更好”）——但最终在 AI 和搜索领域保持领先，并在未来几十年继续竞争。

如果谷歌不得不在声誉和生存之间做出选择，我想我们都知道会发生什么。

## 基于语言模型的搜索是否与盈利兼容？

但挑战的最后部分，如果一切对谷歌来说顺利，将会是一个不可避免的障碍。微软也不会摆脱这一困境。如果搜索通过广告业务模型是有利可图的，那么当无需点击任何东西时，企业如何从基于语言模型的搜索中获利呢？

如果谷歌在领先的情况下，能否找到一种方法，在基于语言模型的搜索周围建立护城河，同时围绕语言模型+搜索构建一种新的可行商业模型？谷歌的 PageRank 算法结合广告模型二十年前是无敌的组合。谷歌能否重现这一壮举？

当然，不用说，如果我们能享受一个没有广告的互联网，那将是很棒的。然而，另一种选择是将搜索转变为付费服务。人们愿意接受这种反惯性的改变吗？

我看到的一个替代可能性（可能只是一个疯狂的假设）是，微软可能决定通过将搜索变成无盈利的服务（没有广告或其他任何形式的盈利）来商品化搜索，目的是在几年内有效地将谷歌从市场中移除。

然而，还有其他问题可能阻止微软尝试这一点。正如 Marcus 在他的文章中解释的那样，当前的搜索比语言模型便宜得多，而且速度更快。这意味着利润更少，这是公司所厌恶的。微软将同时耗费资金与谷歌竞争，这将使双方陷入一个非常冒险的商业操作。

不论最终结果如何，很明显，搜索空间，已经停滞了近二十年，即将经历一个前所未有的拐点。

*订阅* [**The Algorithmic Bridge**](https://thealgorithmicbridge.substack.com/)*。弥合算法与人们之间的差距。关于对你的生活重要的 AI 的新闻通讯。*

*你也可以通过使用我的推荐链接直接在 Medium 上支持我的工作，并成为会员，获得无限访问权限* [**这里**](https://albertoromgar.medium.com/membership) *:)* 
