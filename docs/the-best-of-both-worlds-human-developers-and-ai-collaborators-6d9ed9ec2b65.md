# **人类开发者与 AI 合作伙伴的最佳结合**

> 原文：[https://towardsdatascience.com/the-best-of-both-worlds-human-developers-and-ai-collaborators-6d9ed9ec2b65?source=collection_archive---------15-----------------------#2023-08-01](https://towardsdatascience.com/the-best-of-both-worlds-human-developers-and-ai-collaborators-6d9ed9ec2b65?source=collection_archive---------15-----------------------#2023-08-01)

![](../Images/3b2861f37a5259e9febc9158eabcc223.png)

图片由作者使用 Midjourney 制作

## 生成性 AI 将如何影响产品工程团队 — 第六部分 | 后记

[](https://mark-ridley.medium.com/?source=post_page-----6d9ed9ec2b65--------------------------------)[![Mark Ridley](../Images/b7331b7e94c2500040bb55b462d0f6b6.png)](https://mark-ridley.medium.com/?source=post_page-----6d9ed9ec2b65--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6d9ed9ec2b65--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6d9ed9ec2b65--------------------------------) [Mark Ridley](https://mark-ridley.medium.com/?source=post_page-----6d9ed9ec2b65--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8a96ef478792&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-of-both-worlds-human-developers-and-ai-collaborators-6d9ed9ec2b65&user=Mark+Ridley&userId=8a96ef478792&source=post_page-8a96ef478792----6d9ed9ec2b65---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6d9ed9ec2b65--------------------------------) ·12 min read·2023年8月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6d9ed9ec2b65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-of-both-worlds-human-developers-and-ai-collaborators-6d9ed9ec2b65&user=Mark+Ridley&userId=8a96ef478792&source=-----6d9ed9ec2b65---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6d9ed9ec2b65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-best-of-both-worlds-human-developers-and-ai-collaborators-6d9ed9ec2b65&source=-----6d9ed9ec2b65---------------------bookmark_footer-----------)

这是六部分系列的最后一部分，调查生成性 AI 生产力工具如何影响整个产品工程团队的结构，如 GitHub Copilot、ChatGPT 和 Amazon CodeWhisperer。

在最后一部分中，我们将探讨之前文章中忽略的许多复杂性，人工工程师在人工智能时代的不可替代价值，并对领导者提出反思、适应和创新的号召。

# “以前在…”

在这一系列文章中，我们经历了不少旅程，我认为有必要简要回顾一下我们在前五篇文章中涵盖的内容，以便引入我们**尚未**涵盖的所有内容：

在[第1部分](https://mark-ridley.medium.com/how-generative-ai-will-impact-product-engineering-teams-83a5eaa8fc60)中，我们看到生成式AI编码工具如Copilot和ChatGPT有潜力显著提升技术角色人员的生产力，如工程师和数据科学家。这些工具通过自动化繁琐、重复、耗时的任务，改变了开发人员需要做的工作。理论上，它去除了开发人员不喜欢的工作内容，使他们的宝贵工作变得更快更轻松。

在[第2部分](https://medium.com/@mark-ridley/the-proliferation-of-generative-ai-coding-tools-and-how-product-engineering-teams-will-use-them-48787ebfcaaa)中，我们考虑到开发任务的自动化程度提高和开发人员生产力的提升，可能会导致产品团队中工程师与产品经理的比例发生重大变化；在历史上，团队中大约有五名工程师对应一名产品经理，但随着生成式AI工具的出现，这一比例可能会接近1:1。

在[第3部分](https://mark-ridley.medium.com/if-engineers-start-to-use-ai-coding-tools-what-happens-to-our-product-teams-acd55fb273dd)中，我们探讨了生成式AI编码助手不仅具备了协助基本工作如重构代码或编写测试的能力。我们还探讨了长期来看，AI工具可能会重构遗留代码，包括创建测试和文档，并简化一些团队结构，如移动应用开发，这些团队结构往往不是因为价值流的差异而创建，而是因为技术技能不同。我们还考虑了对初级工程师及其职业发展的可能影响。

在[第4部分](https://medium.com/ridley/if-ai-coding-tools-reduce-the-number-of-engineers-we-need-where-do-we-spend-our-budgets-abd6248ad833)中，我们考虑了这种变化如何使公司能够在相同产出的情况下大幅减少工程师人数、在相同预算下快速增加产出，或某种组合。我们开始建模投资增长、预算维护或成本减少之间的选择可能对工程和产品人数的影响。

在[第五部分](https://mark-ridley.medium.com/who-wins-and-who-loses-how-different-types-of-business-could-be-impacted-by-ai-tools-684d1e83a9e0)中，我们调查了这些好处可能如何以不同的方式影响不同类型的公司。新的创业公司应该尽快开始使用这些新工具。较大的公司，虽然可能有最强烈的财务压力来改变团队结构，但由于文化惯性会面临适应困难，但今天应该鼓励他们开始实验这些工具，并根据自己的战略做出如何采用这些工具的决策。我们还看到，“外包”公司最有可能受到负面影响，因为客户希望保留内部员工并在其他地方节省成本。

迄今为止我们讨论的一切都围绕着假设**“下一代产品工程团队可能会有更少的工程师”**展开，但我认为可以公平地说，我们在证明或反驳这一假设方面还远远不够。

# “是的，但……”

现在，我想讨论过去几个月我在这方面的对话中收到的许多发人深省的挑战，并且要讨论我在服务于最初假设时做出的大量假设。

我留下了很多潜在的反驳意见没有处理，不是因为它们没有困扰我，而是因为在实验这些工具并观察它们如何真正帮助团队的过程中，还有太多的不确定性和工作要做。

这是一个包含许多批评和未回答问题的大清单：

## 你把大量的信任寄托在AI工具上，希望它们能够编写优质的代码。它们不可能像人类一样优秀。

我承认，我对这些工具的质量感到印象深刻，但我也不试图证明它们需要像**优秀**开发者那样好。我故意避免建模没有技术专业知识的情境，因为根据我使用这些工具的经验，它们仍然需要大量的监督和指导。监督是为了确保它们不会做出愚蠢或危险的事情（比如将API密钥写入源代码），指导则是为了确保它们做对了事情（使用正确的语言、框架或设计模式）。

我有一种感觉，这些AI工具需要成为软件工程领域的麦当劳；虽然去一家优秀的非连锁餐厅与出色的员工合作可以是一种超凡的体验，但这并不总是可能的。麦当劳在全球范围内都是干净、便宜、健康（我的意思是没有细菌滋生）且最重要的是一致的。正如我一位亲爱的意大利朋友在面对一份大牌连锁披萨时所说，“它不会让你死”。在某种程度上，这就是我们对AI工具的期望。

但，我也不认为这就是故事的结局。我们今天看到的工具距离一年的质量还有很大差距。即便在我将原文编辑成这一系列文章的过程中，每天都有关于更多改进的消息；UC Berkeley 推出了一个[比 GPT4 更擅长编写 API 调用的模型](https://www.marktechpost.com/2023/07/21/researchers-from-uc-berkeley-introduce-gorilla-a-finetuned-llama-based-model-that-surpasses-gpt-4-on-writing-api-calls/)。微软[宣布了“膨胀注意力”，允许模型扩展到数十亿个标记](https://www.marktechpost.com/2023/07/08/microsoft-research-introduces-longnet-a-transformer-variant-that-can-scale-sequence-length-to-more-than-1-billion-tokens-with-no-loss-in-shorter-sequences/)的 LongNet。[StackOverflow 宣布了 OverflowAI](https://stackoverflow.blog/2023/07/27/announcing-overflowai/)，承诺对愚蠢的问题提供更多犀利的回应（抱歉，我的意思是，更好、更有用的搜索）。

即使你对现有工具的能力持怀疑态度，我担心忽视它们可能发展出的潜力会显得目光短浅。

[**编辑**：即便是在我第一次起草这篇文章的一周左右时间内，Stack Overflow 已经宣布了[OverflowAI](https://stackoverflow.blog/2023/07/27/announcing-overflowai/)，Github 宣布了[防止知识产权问题的额外工具](https://github.blog/2023-08-03-introducing-code-referencing-for-github-copilot/)，StabilityAI 宣布了一个[专注于编码的 LLM](https://venturebeat.com/programming-development/stability-ai-launches-stablecode-an-llm-for-code-generation/)。市场的发展速度令人惊叹]

## AI 工具将会面临知识产权问题。还会有安全问题。如果它们停止工作，我们将无法继续工作。

是的，这些都是可能的。

但，这不是我们第一次处理类似问题，我们已经有了应对的方法。我与许多公司交流时发现，他们因为担心泄露公司机密而陷入某种程度的僵局，这些机密可能会被用来进一步训练 LLM。虽然对于免费和个人订阅用户，这种情况可能确实存在，但我强烈建议亲爱的读者自行研究大型供应商，了解具体的风险，以及这些供应商为解决这个非常合理的担忧所采取的措施。查看一些大公司的常见问题解答，以确定是否有足够好的答案来满足你的使用案例和风险情况：[OpenAI](https://help.openai.com/en/articles/5722486-how-your-data-is-used-to-improve-model-performance)，[Github Copilot](https://docs.github.com/en/site-policy/privacy-policies/github-copilot-for-business-privacy-statement)，[AWS CodeWhisperer](https://docs.aws.amazon.com/codewhisperer/latest/userguide/sharing-data.html)（Google Duet 仍在封闭测试中，数据安全文档尚未发布）。

安全和数据保护也是类似的情况。今天阅读这篇文章的大多数人已经依赖于 Github、微软或 AWS 的安全性。你可能将代码存储在 Github 中，或将应用程序或数据存储在 Azure、GCP 或 Amazon 上。问问自己，为什么你愿意接受超云供应商的风险，却不愿接受编码工具的风险。使用 ChatGPT 的风险不可忽视，5 月份报告了数据泄漏，[本周报道的越狱潜力](https://mashable.com/article/chatgpt-claude-ai-chatbot-jailbreak)，以及云安全供应商 Netskope [报告的内部用户数据泄漏](https://itwire.com/business-it-news/security/chatgpt-data-leaks-most-commonly-involve-source-code-report.html)。和其他技术一样，你可以选择禁止它在你的组织中使用，但人们，像自然一样，总会找到办法。要妥善解决安全问题，你需要教育用户并提供安全、易于使用的替代方案。如果 OpenAI 无法胜任，也许其他供应商可以。

另一个担忧是[无意中暴露知识产权风险](https://www.zdnet.com/article/is-github-copilots-code-legal-ethically-right/)，例如模型可能在‘（偶然？）’训练过程中使用了封闭源材料，这使得工具可能使你的组织面临违反法律的风险（并承担纠正违约的风险）。坏消息是——如果你认为这是一个新风险，你可能应该更仔细地检查你在组织中使用开源软件的情况。许多公司在正确管理和理解其“软件材料清单”（SBOM）——即其软件所依赖的封闭和开源源代码的列表——方面远远不够。你绝对应该担心这些工具中的一个可能会偶然使你侵犯他人的知识产权，但你应该扩展你已经用于开源软件和开发者的大红剪贴按钮的控制措施。

这些风险由你承担，如果你认真考虑这些工具可能提供的机会，你也应该阅读文档并与供应商讨论他们采取的保护措施。确保你做好功课。[Copilot 的常识隐私报告](https://privacy.commonsense.org/privacy-report/GitHub-Copilot)给出的评分为63%，较低，获得了‘黄灯’警告（它在‘学校’和‘家长同意’上的评分特别低，拖低了总分）。

这应该**始终**成为你采购流程的一部分。你**始终**需要从风险的角度考虑任何你打算放置在生产代码或数据旁边的工具，决定你对风险的接受程度以及如何减轻风险是你的责任。

更积极的一点是，我认为 Salesforce 最近的公告很好地指示了这些工具的发展方向。Salesforce 推动 AI Day 的主要营销重点之一是他们称之为‘[Einstein Trust Layer](https://www.salesforce.com/uk/news/press-releases/2023/06/12/ai-cloud-news/)’的部分，这似乎是一个真正令人印象深刻的包装，针对不同的 LLM 模型，能够大大增强访问安全性并保护客户和公司信息（说真的，[看看这个视频](https://www.salesforce.com/plus/experience/world_tour/series/world_tour_london:_ai_day/episode/episode-s1e4)，尽管它不是关于代码的）。

我们刚刚看到七家主要的科技公司（亚马逊、Anthropic、谷歌、Inflection、Meta、微软和 OpenAI）签署了自愿的‘负责任 AI’承诺，其中[包括水印输出](https://www.deeplearning.ai/the-batch/bravo-to-ai-companies-that-agreed-to-voluntary-commitments/)。可以合理地假设，这些市场上的最大玩家，大多数已经是我们信任的公司，将发布类似的信任和安全包装，以解决有关知识产权、安全、隐私以及 LLM 可能有些有毒和与事实关系可疑的问题。

## 仍然需要有人设计整体解决方案。

另见：

+   需要有人管理所有的数据和数据管道。

+   需要有人管理应用程序之间的重叠。

+   需要有人确保产品的安全性。

+   需要有人监控和回应问题。

+   需要有人管理团队和产品之间的依赖关系和互动。

对，你说得对。仍然有很多工作需要完成，不只是编写代码。

**这太棒了！**

这意味着我们仍然需要人类一段时间，并且开始显示我们应该为工程师提供培训的地方。但我们也应该预期工具、自动化和更高质量的代码会开始积极影响这些问题清单。通过更多的文档、减少‘聪明’的代码、更清晰的接口、更好的解释性和更好的测试覆盖，我们可能会看到这些挑战减少。

## 但工程师的大部分时间实际上并不是在编写代码，因为我们不断被要求去参加无聊的会议。

是的，没错。AI 并不会解决所有问题，但如果你作为工程师在会议上的时间比在编程上多，那么问题不在于你的生产力，而在于你所在组织的管理方式。

也许 AI 有一天会解决这个问题，但在短期内，更小、更精简和更自动化的组织可能不需要这么多会议。

坦率地说，对于大多数组织来说，提高开发者生产力的最佳方法是更好地优先排序工作，对低价值结果说不，并给团队更多时间专注于交付高质量的产品。但如果我们不能做到这一点，也许我们可以有AI编码助手来替代。

## 我们不能用AI来替代产品经理吗？

啊，这个很不错。

我收到的关于这些文章的一个令人惊讶的反馈是：“这真的很有趣，但在我的业务中，我们的产品支持远远不够。” 结果我发现，我用5:1的工程师比例展示了一个无意识的偏见，许多技术团队已经因为他们的比例不仅高得多（比如说10:1或更高），而且产品技能在组织中的价值不够高而面临巨大挑战。一些公司似乎仍然认为工程师写代码，而产品经理是一种昂贵的奢侈品。

我认为从客户那里引出需求是非常重要的。我也认为运行一个有效且易于理解的商业优先级流程是非常困难的。在我们能让利益相关者自己进行软件提示设计之前，还需要一段时间。

产品经理对出色的产品工程至关重要。产品经理集中工程资源，并帮助业务识别最有价值的结果。我认为我们现在最不需要的就是减少产品支持，即使我们可以通过自动化来协助他们的一些日常任务。

我怀疑在这些新团队中，产品经理和技术负责人将是最关键的角色。

## 你没有考虑X、Y或Z

你几乎肯定是对的。不过如果你已经做到了，那就太好了！给我留言，开始讨论，或者最好的方式是写一篇新的文章，阐述你对Y或Z的看法。（但不要谈X。我们不再讨论X了。）

# 让我们总结一下

这整个系列文章的重点是考虑一个问题：今天，我觉得还没有足够的产品和技术领导者积极参与讨论，如果生成性AI编码工具的承诺兑现，可能对他们的团队产生什么影响。我们不仅仅是在谈论对开发者生产力的小幅提升。我们可能谈论的是产品创建方式的改变。

如果我们讨论的内容有一部分是真的，那么构建软件的团队可能会与现在大相径庭，这将对产品和技术人员的预算产生重大影响。整个行业面临着破坏的风险，尤其是开发者角色的减少。

尽管公司可能会通过投资更多的成果来缓解这一问题，但一个企业可以支持的价值流是有限的。那些花时间仔细和果断地理解和接受即将到来的变化的公司可能会获得巨大的竞争优势。

这个论点也有哲学方面的内容。我相信人类工程师可能有一种不可言喻和不可替代的价值，但我不再能自信地告诉你这是什么了。

我们知道开发者编写代码，我们可以根据他们编写代码的速度和代码的有效性来为他们定价。但是这一说法未能真正理解工程师的实际工作是什么。多年来，很容易证明熟练的人类工程师能够做一些算法无法做到的事情，但现在情况不再如此。如果人类与机器之间的界限仅仅是可以创建的解决方案的复杂程度，那么我们正处于危险的沙土之中。正如我之前所写：

**工程师不仅仅被教如何编程。他们被教如何思考。**

作为一个行业，我们必须认识到这一代新工具可以轻松复制广泛的工程任务。在这样做的过程中，我们还应理解，仍然存在一些人类具备的独特技能是难以复制的。作为领导者，我们的工作是识别这些技能并投资于人员，以优化这些技能。

这篇文章是一个行动号召。如果你是产品或技术领导者，你应该将其视为一个挑战，考虑生成型 AI 工具对你们团队的影响，并让你的团队参与回答这个问题。

给定时间和空间来考虑这些工具将如何帮助他们，最有可能的创新来源将是你们的团队本身。但是，作为一个领导者，你也需要认识到，火鸡可能不太愿意为圣诞节投票。人们可能因为对就业的真实威胁感到害怕和不愿意参与，你需要发挥他们的才能来帮助你了解是否应该削减预算。成为老板很难，但否则你的竞争对手可能比你更果断和无情。

我们应该明确，最有趣、最混乱、最不受控制、最有影响力和最令人兴奋的进展将出现在那些小型初创公司和技术以外的团队中。不要犯错，忽视你自己组织中的非技术人员或初创社区中微小公司的贡献。

首先要对自己进行教育，然后对你自己公司里的其他人进行教育，了解真正的风险是什么。通过关注你公司内部的好奇早期采用者和外部技术巨头及小型初创公司的动向，考虑机会。要意识到这些进展可能会从根本上重塑你的产品工程，然后走出去，保持好奇，提问，并鼓励你的团队也这样做。

我还没有设法推翻自己的假设——但现在我需要你的帮助来设计实验。

## **本系列的其他文章：**

+   [第1部分](https://mark-ridley.medium.com/how-generative-ai-will-impact-product-engineering-teams-83a5eaa8fc60)

+   [第2部分](https://medium.com/@mark-ridley/the-proliferation-of-generative-ai-coding-tools-and-how-product-engineering-teams-will-use-them-48787ebfcaaa)

+   [第3部分](https://mark-ridley.medium.com/if-engineers-start-to-use-ai-coding-tools-what-happens-to-our-product-teams-acd55fb273dd)

+   [第4部分](https://medium.com/ridley/if-ai-coding-tools-reduce-the-number-of-engineers-we-need-where-do-we-spend-our-budgets-abd6248ad833)

+   [第5部分](https://mark-ridley.medium.com/who-wins-and-who-loses-how-different-types-of-business-could-be-impacted-by-ai-tools-684d1e83a9e0)

附言：如果你喜欢这些关于团队的文章，看看我的[Teamcraft 播客](https://www.teamcraft.uk/)，在这里我和我的共同主持人安德鲁·麦克拉伦讨论了使团队成功的因素。
