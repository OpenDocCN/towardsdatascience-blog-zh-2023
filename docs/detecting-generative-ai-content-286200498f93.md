# 检测生成式人工智能内容

> 原文：[`towardsdatascience.com/detecting-generative-ai-content-286200498f93?source=collection_archive---------5-----------------------#2023-11-15`](https://towardsdatascience.com/detecting-generative-ai-content-286200498f93?source=collection_archive---------5-----------------------#2023-11-15)

## 关于 deepfakes、真实性以及总统关于人工智能的行政命令

[](https://medium.com/@s.kirmer?source=post_page-----286200498f93--------------------------------)![Stephanie Kirmer](https://medium.com/@s.kirmer?source=post_page-----286200498f93--------------------------------)[](https://towardsdatascience.com/?source=post_page-----286200498f93--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----286200498f93--------------------------------) [Stephanie Kirmer](https://medium.com/@s.kirmer?source=post_page-----286200498f93--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa8dc77209ef3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-generative-ai-content-286200498f93&user=Stephanie+Kirmer&userId=a8dc77209ef3&source=post_page-a8dc77209ef3----286200498f93---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----286200498f93--------------------------------) ·8 分钟阅读·2023 年 11 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F286200498f93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-generative-ai-content-286200498f93&user=Stephanie+Kirmer&userId=a8dc77209ef3&source=-----286200498f93---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F286200498f93&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdetecting-generative-ai-content-286200498f93&source=-----286200498f93---------------------bookmark_footer-----------)![](img/685390d42ddd17dd4950fdcfcd925ec4.png)

你能识别出假的吗？由 [Liberty Ann](https://unsplash.com/@libertyanns?utm_source=medium&utm_medium=referral) 拍摄的照片在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

人工智能生成技术进步带来的众多有趣的伦理问题之一是**模型生成产品的检测**。对于那些消费媒体的我们来说，这也是一个实际问题。我正在阅读或观看的内容是一个人深思熟虑的作品，还是仅仅是概率生成的文字或图像，旨在吸引我？这是否重要？如果重要，我们该怎么办？

# 不可区分内容的含义

当我们谈论内容难以或不可能检测为 AI 生成时，我们实际上是在进入类似于图灵测试的领域。假设我给你一段文本或一张图像文件。如果我问你，“这是人类还是机器学习模型生成的？”而你无法准确判断，那么我们就到了需要考虑这些问题的地步。

在许多领域，我们接近这一点，特别是对于 GPT-4，但即使是较不复杂的模型，也取决于我们使用什么样的提示和上下文的量。如果我们有一份来自 GPT 模型的长文档，那么检测它不是人类生成的可能会更容易，因为每个新词都是模型做一些普通人不会做的事情的机会。视频或高分辨率图像也是如此——像素化或“恐怖谷”出现的机会越多，我们就越容易发现伪造内容。

我也很清楚，随着我们对模型生成内容的熟悉程度提高，我们将会更擅长识别内容中 AI 参与的明显迹象。正如我几周前在解释生成对抗网络（GAN）如何工作的文章中所描述的那样，我们和生成 AI 之间存在某种 GAN 关系。模型致力于创造尽可能类似人类的内容，而我们则提高识别这些内容非人类的能力。这就像一场比赛，双方都在努力超越对方。

> 随着我们对模型生成内容的熟悉程度提高，我们将会更擅长识别内容中 AI 参与的明显迹象。

# 检测方法

然而，我们在这种检测方面可能会有一个极限，模型最终可能会超越普通人眼耳（如果尚未如此的话）。我们只是没有像大型模型那样的感知能力和模式识别复杂性。幸运的是，我们也可以将模型作为我们这边比赛的工具，训练模型来审查内容并确定其是否由人类生成，这也是我们的一种工具。

然而，在某些情况下，尤其是小量内容中，可能真的没有任何可靠的迹象表明其来源于机器学习。从哲学上讲，考虑到模型的无限进步，可能有一个点在于这两种内容之间真的没有实际差别。此外，大多数人不会使用模型来测试我们消费的所有媒体，以确保其由人类生成且真实。作为回应，一些组织，如[内容真实性倡议](https://contentauthenticity.org/)，正在推动内容认证元数据的广泛采用，这可能会有所帮助。然而，这些努力需要模型提供者的善意和工作。

> 然而，在某些情况下，某些内容可能真的没有任何可靠的迹象表明其来源于机器学习。

你可能会问，那么，有些故意恶意行为者试图使用深度伪造或使用 AI 生成的内容制造伤害的人怎么办呢？他们不会自愿识别他们的内容，对吧？这是一个公平的问题。然而，至少目前，那些足够复杂以至于能够大规模欺骗人们的模型大多受到大公司（如 OpenAI 等）的控制。这种情况不会持续太久，但就目前而言，如果向公众提供最先进的 LLM 的人们采取一些行动，至少会在内容来源的问题上起到一定作用。

到目前为止，这不是一个非常乐观的故事。生成式 AI 正迅速向一个使得那些非常强大的模型足够小，以至于恶意行为者可以运行自己的模型，并且这些模型很容易创建出与有机人类内容在字面上无法区分的内容的地方发展。即使是其他模型也无法分辨。

# 检测的原因

我有点过于急躁了。但是，为什么大家都如此重视我们首先要弄清楚内容是否来自模型呢？如果你分辨不出来，那有什么关系呢？

一个主要原因是，内容的传播者可能有恶意意图，例如误导信息或深度伪造。在这里，创建图像、音频和视频是最常见的情况之一——使其看起来像某人说过或做过某事。如果你一直在关注[美国总统关于人工智能的行政命令](https://www.whitehouse.gov/briefing-room/presidential-actions/2023/10/30/executive-order-on-the-safe-secure-and-trustworthy-development-and-use-of-artificial-intelligence/)，你可能已经听说拜登总统实际上因为听说有人可能会恶意使用他的肖像和声音来进行误导信息而对此感兴趣。这是一个非常严重的问题，因为目前我们倾向于相信我们用自己的眼睛看到的图像或视频，这可能会对人们的生活和安全产生重大影响。

> 目前，我们倾向于相信我们用自己的眼睛看到的图像或视频，这可能会对人们的生活和安全产生重大影响。

另一个相关问题是，当模型用于模仿特定人类的作品时，不一定是出于恶意原因，而仅仅是因为那份工作令人愉悦和受欢迎，可能也是有利可图的。这是明显不道德的，但在大多数情况下可能并不是要积极伤害观众或被模仿的人。当深度伪造用于虚构某人的行为时，这也会造成声誉损害。（只需问问[乔·罗根，他一直在打击](https://mashable.com/article/joe-rogan-tiktok-deepfake-ad)使用他的肖像进行深度伪造的广告。）

我在 Casey Newton 在他[10 月 5 日的 Platformer 期刊](https://open.substack.com/pub/platformer/p/how-authoritarian-governments-are?r=7xzvu&utm_medium=ios&utm_campaign=post)讨论后开始思考的第三个角度是，公众人物可能会将问题颠倒过来，声称他们不良行为的真实、真实证据是人为生成的。当我们无法通过证据可靠地揭露不当行为时，该怎么办，因为“这是深度伪造”是一个无法反驳的回应？我们还未完全到达这一点，但我可以预见这在不久的将来可能成为一个真正的问题。

> 当我们无法通过证据可靠地揭露不当行为时，该怎么办，因为“这是深度伪造”是一个无法反驳的回应？

不那么紧迫的是，我也认为希望我的媒体消费能代表与另一个人互动，即使这是主要是单向的思想交流。我认为阅读或欣赏艺术是与另一个人的思想互动，而与模型编排的文字互动却没有相同的感觉。因此，就个人而言，我希望知道我消费的内容是否是由人类创作的。

# 行政命令

鉴于所有这些非常现实的风险，我们面临着一些严重的挑战。似乎在可检测来源（即，公众安全和我上述描述的所有问题）和模型复杂性之间存在权衡，作为一个行业，数据科学正朝着模型复杂性方向推进。谁来平衡这些尺度呢？

总统的行政命令在这个话题上代表了一些重要的进展。（它还讨论了许多其他重要问题，我可能会在其他时候讨论。）我花了过去一个半星期时间思考这一行政命令，并阅读了来自行业各方的观点。虽然有些人认为这会抑制进步（并且会使生成型 AI 的大玩家得以巩固，而小型竞争者则受到牺牲），但我认为我倾向于对这一行政命令持乐观态度。

创建具有竞争力的生成型 AI 模型是极其昂贵且资源密集的，这自然限制了最初有多少竞争者能够进入这一领域。以牺牲更广泛的社会安全来保护这一领域的假设新玩家对我来说没有意义。我也不认为该行政命令对那些有资源进入这一领域的组织来说是过于繁琐的。

命令本身也不是那么具备规范性。它要求创建一些东西，但对如何实现这些目标留有很大弹性，希望有见识的人会参与这些过程。🤞（领域中的数据科学家应该留意发生的情况，如果情况失控，就要积极发声。）特别是，它要求商务部制定“检测 AI 生成内容和验证官方内容的标准和最佳实践”。还有一些关于安全性和模型相关的重要组成部分。

**我是否对我们的政府在监管 AI 的同时不损害创新方面有极大的信任？** **不，真的没有。** 但我相信，如果让行业自行其是，它不会像所需的那样关注来源和内容检测的问题——到目前为止，他们还没有表现出这是一个优先事项。

> 我相信，如果让行业自行其是，它不会像所需的那样关注来源和内容检测的问题。

同时，我不确定检测生成性 AI 生成的内容在所有或大多数情况下是否实际上是可能的，特别是随着我们取得进展。行政命令没有提及如果内容跨越到不可检测的领域，是否阻止开发这些模型，但这种风险让我担忧。这真的会扼杀创新，我们需要仔细考虑权衡是什么或可能是什么。尽管如此，这匹马可能已经跑出了那座特定的马厩——如此多的开源生成性 AI 模型已经存在于世界上，不断在改进它们的能力。

# 结论

这个话题很棘手，正确的行动并不一定很明确。模型输出越复杂，我们检测这些输出不是人类生成的可能性就越高，但我们正处于一个技术竞赛中，这将使得检测越来越困难。对这一话题的政策参与可能会给我们一些保护措施，但我们还不能确定这是否真的有帮助，或者是否会变成一场笨拙的灾难。

这是其中一个我无法将讨论 neatly tie up 的时候。不可区分的生成性 AI 输出的潜在和实际风险是严重的，应当如此对待。然而，我们处于一个科学/数学的阶段，无法创造出快速或简单的解决方案，我们需要重视更先进的生成性 AI 可能带来的好处。

尽管如此，数据科学家应当花时间阅读行政命令或至少阅读事实清单，并清楚了解声明的含义。正如常读者所知，我认为我们必须对在生活中向外行传播准确且易于理解的信息负起责任，而这是一个好机会，因为该话题正在新闻中。确保你为周围的数据科学素养作出积极贡献。 

查看我的更多作品 [www.stephaniekirmer.com](https://www.stephaniekirmer.com)。 

# 参考文献 

Casey Newton - [10 月 5 日的 Platformer 期刊](https://open.substack.com/pub/platformer/p/how-authoritarian-governments-are?r=7xzvu&utm_medium=ios&utm_campaign=post)

[关于人工智能行政命令的事实清单](https://www.whitehouse.gov/briefing-room/statements-releases/2023/10/30/fact-sheet-president-biden-issues-executive-order-on-safe-secure-and-trustworthy-artificial-intelligence/) 

[人工智能行政命令的完整文件](https://www.whitehouse.gov/briefing-room/presidential-actions/2023/10/30/executive-order-on-the-safe-secure-and-trustworthy-development-and-use-of-artificial-intelligence/) 

[关于乔·罗根深度伪造广告欺诈的报道](https://mashable.com/article/joe-rogan-tiktok-deepfake-ad) 

[](https://contentauthenticity.org/?source=post_page-----286200498f93--------------------------------) [## 内容真实性倡议 

### 制定数字内容来源的标准。 

contentauthenticity.org](https://contentauthenticity.org/?source=post_page-----286200498f93--------------------------------) 
