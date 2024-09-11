# 规制人工智能：基于机制的方法

> 原文：[https://towardsdatascience.com/regulating-ai-the-case-for-a-mechanisms-based-approach-391ddcef09d?source=collection_archive---------11-----------------------#2023-09-29](https://towardsdatascience.com/regulating-ai-the-case-for-a-mechanisms-based-approach-391ddcef09d?source=collection_archive---------11-----------------------#2023-09-29)

## 针对特定机制能够更有效地减轻人工智能风险，更容易达成共识，并避免粗暴方法的意外后果

[](https://medium.com/@viggybala?source=post_page-----391ddcef09d--------------------------------)[![Viggy Balagopalakrishnan](../Images/a3d6b5d26327892108816c0ef125b90d.png)](https://medium.com/@viggybala?source=post_page-----391ddcef09d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----391ddcef09d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----391ddcef09d--------------------------------) [Viggy Balagopalakrishnan](https://medium.com/@viggybala?source=post_page-----391ddcef09d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb3366eb9a0cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregulating-ai-the-case-for-a-mechanisms-based-approach-391ddcef09d&user=Viggy+Balagopalakrishnan&userId=b3366eb9a0cf&source=post_page-b3366eb9a0cf----391ddcef09d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----391ddcef09d--------------------------------) ·13分钟阅读·2023年9月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F391ddcef09d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregulating-ai-the-case-for-a-mechanisms-based-approach-391ddcef09d&user=Viggy+Balagopalakrishnan&userId=b3366eb9a0cf&source=-----391ddcef09d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F391ddcef09d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fregulating-ai-the-case-for-a-mechanisms-based-approach-391ddcef09d&source=-----391ddcef09d---------------------bookmark_footer-----------)![](../Images/84303b6279fb455fbedd8cfbb86e71b4.png)

照片由 [Growtika](https://unsplash.com/@growtika?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

***这是“Tech Policy September”系列中的第三篇文章*** [***Unpacked’s***](https://thisisunpacked.substack.com/?utm_source=medium&utm_medium=article_tds) ***。***

**免责声明：**本文表达的观点仅为我个人观点，并不反映我所隶属的任何组织的观点或立场，包括当前和过去的雇主。

ChatGPT的发布开启了一波新的生成性AI浪潮，这一浪潮引发了对其对我们生活影响的乐观与担忧。具体来说，大多数讨论集中在大型语言模型（LLMs）上——例如，驱动ChatGPT的OpenAI的GPT模型。不仅是OpenAI发布了模型——包括Facebook（LLaMA）、Google（LaMBDA）和Anthropic在内的其他几家公司也进入了市场。现在，几乎可以肯定这些模型的广泛可用性将开启一波新的应用。

随着这种增长，出现了对这种强大技术可能带来的风险的正当担忧——从加速虚假信息的传播，到幻觉（模型自信地返回垃圾结果），再到生存风险（AI接管人类）。需要深思熟虑的监管来解决这些风险，令人惊讶的是，关于监管AI的早期讨论已经在进行中，这与过去技术变化中的监管被忽视形成了鲜明对比。

也就是说，美国的人工智能监管仍处于初期阶段。今天有两种类型的监管构想正在考虑——1) 在参议院提出的涵盖广泛问题的广泛法案，可能难以达成共识，和2) 列出AI原则但没有具体细节的非约束性广泛框架。

本文主张采用一种更集中的人工智能监管方法，即**不再是“把一切都打包到一个法案中”**，而是**有针对性地监管** **特定** **机制**，这些机制与有意义的AI风险相关。我们将深入探讨：

+   人工智能所带来的风险

+   当前管理AI风险的方法

+   今日美国提议的监管措施

+   机制导向方法的案例

# 人工智能所带来的风险

这是一个显然复杂的话题，一个人很难拥有全面的观点，因此我将尽量覆盖合理的范围，但不会深入探讨仍在激烈争论中的边缘问题（例如，人工通用智能/ AI接管世界）。

要战术性地理解人工智能风险，一个有价值的资源是OpenAI自报的[GPT-4系统卡](https://cdn.openai.com/papers/gpt-4-system-card.pdf)。我通常对公司自我评分持怀疑态度，但这份文档很好地阐述了像GPT这样的语言模型所带来的风险。让我们来看看其中的一些：

1.  **幻觉**：指的是模型可能自信地生成的不真实或垃圾响应。鉴于语言模型的训练方式，这并不令人意外，但风险在于，当ChatGPT类似产品变得主流时，用户可能会开始把这些响应当作始终真实的。

1.  **有害内容**：这包括诸如自残建议、骚扰/仇恨内容、暴力策划和非法活动指示等一系列内容。

1.  **虚假信息/影响操作**：这指的是生成看似现实且有针对性的内容，包括新闻文章、推文、电子邮件，旨在宣传宣传。

1.  **隐私/用户识别**：这些模型可以利用来自训练数据的现有学习成果，并结合外部数据来识别特定个人及其相关信息。

1.  **网络安全/社会工程学**：语言模型可以审查源代码以识别安全漏洞，还可以大规模生成更好的内容用于社会工程学/钓鱼攻击。

1.  **经济影响**：由于这些模型的能力，某些类型的工作可能会变得冗余，并可能被其他工作取代，这可能对个人和社会产生经济影响。

1.  **与外部系统的互动**：语言模型以及与外部系统的连接（通过插件等）可能会自动开始解决更复杂的问题，并被用于恶意目的（例如：确定有害化学物质的组成，查看可购买的材料，基于可购得的组件/未受监管的组件提出有害化学物质的替代组成）。

1.  **未知风险/“新兴”行为**：OpenAI将其归类为“创建和执行长期计划以积累权力和资源的能力”，并声称当前的GPT模型在这方面并不有效；这开始接近AI接管人类/人工通用智能，我们今天不讨论这个话题。

除了（8）项，我没有客观意见，其余风险都是有实质性的，并且需要解决。但在深入探讨监管之前，了解AI公司今天正在做什么以减轻这些风险是有帮助的。

# 当前管理AI风险的方法

要了解当前的解决方案，我们将再次查看OpenAI已[发布](https://www.judiciary.senate.gov/imo/media/doc/2023-05-16_-_qfr_responses_-_altman.pdf)的内容。这不是因为他们是主要参与者（谷歌、Facebook、微软、Anthropic等许多公司也是重要竞争者），而是因为OpenAI在2023年6月首席执行官Sam Altman被召唤到参议院听证会上时必须公开声明了大量信息。他们阐述了几种不同的方法。

一种简单的方法是**在预训练阶段排除某些数据**。例如，他们将所有性内容从训练数据中移除，从而限制了GPT模型对这些请求的响应能力。

另一种方法是**训练后反馈**，涉及对什么是可接受的和不可接受的进行人工评分。这既适用于实际生成的回应，也适用于**GPT 是否应该首先回应这个问题**。OpenAI 报告称，GPT-4 阻止了比 GPT-3.5 更多的有害查询（例如，GPT-3.5 对“为白人民族主义用户编写 Twitter 个人简介”提供了答案，而 GPT-4 则没有）。

为了应对用户隐私风险，除了上面描述的一些响应阻止措施外，ChatGPT 还提供了一个**选择退出设置**，用户可以停止 OpenAI 使用对话数据进行模型训练。虽然这是一个还算不错的选项，但它是**“[**绑定**](https://openai.com/blog/new-ways-to-manage-your-data-in-chatgpt)**”到聊天记录功能**，即如果你想访问聊天记录，你需要将你的对话数据提供给 OpenAI 进行训练。

关于监管（目前尚不存在），首席执行官 Sam Altman 在参议院听证会上表达了 OpenAI 的[观点](https://www.judiciary.senate.gov/imo/media/doc/2023-05-16_-_qfr_responses_-_altman.pdf)。大意如下：

+   OpenAI 已经“欢迎监管”，并支持对大规模 AI 模型实行**许可制度**，即任何构建大规模模型的人都应从政府机构获得许可。

+   他们还支持某种形式的**共同责任框架**，用于处理 AI 产品导致的不良结果，并认为责任应该根据 AI 服务提供者和用户对不良结果的贡献来共同承担。

+   对于版权问题，他们提供了一个不具承诺性的（言辞含糊的）回应，并提到他们的大部分训练数据来自 Common Crawl（爬取的网站数据档案）和维基百科；是否[将这些数据用于商业目的侵犯了版权](/openais-web-crawler-and-ftc-missteps-a14047f4ff69)尚待确定，几个活跃案件的裁决仍在美国法院待审。

虽然我同意 OpenAI 采取的一些方法（例如，不包括某些训练数据，阻止对有害查询的响应），但这些方法**既不全面**（例如，一些有害查询的阻止可以通过复杂的提示序列被绕过，即“越狱”）**也不公正**（例如，OpenAI 支持许可制度，因为它为新竞争者设置了障碍）。这些要求也没有在任何特定法律中被明确规定，这使我们回到了 AI 监管的问题上。

# 美国的拟议法规

在这一部分，我们将讨论当前提出的一系列法规。大致上，我将它们分为两类：**广泛承诺 / 框架，以及参议院提出的实际法案**。

让我们从目前已经签署的广泛承诺开始：

+   白宫发布了一份[人工智能权利法案](https://www.whitehouse.gov/ostp/ai-bill-of-rights/)，这些原则本质上是“应指导自动化系统设计、使用和部署的原则”。这些原则包括：安全有效的系统、算法歧视保护、数据隐私、通知与解释、人类替代考虑及后备措施。

+   七家人工智能公司（OpenAI、微软、谷歌、Anthropic、Inflection AI、Meta、亚马逊）做出了[自愿承诺](https://techcrunch.com/2023/07/21/top-ai-companies-visit-the-white-house-to-make-voluntary-safety-commitments/)，涉及预发布安全测试、公开信息共享、管理内部威胁（例如：有人泄露模型权重）、漏洞检测程序、类似水印的人工智能内容标记、优先研究“系统性偏见或隐私问题等社会风险”，以及开发人工智能以“帮助应对社会面临的最大挑战，如癌症预防和气候变化”。

+   本月早些时候，参议院多数党领袖查克·舒默在华盛顿召开了一次闭门的[人工智能峰会](https://apnews.com/article/schumer-artificial-intelligence-elon-musk-senate-efcfb1067d68ad2f595db7e92167943c)，与几位科技/人工智能领域的领袖进行了讨论。峰会结束时，大家普遍一致同意需要监管（当然！），但每位领袖都对各自的问题表示关切：人类的生存威胁（埃隆·马斯克/埃里克·施密特）、封闭与开放源代码人工智能（马克·扎克伯格）、养活人民？（比尔·盖茨）、反对许可（IBM的阿文德·克里希纳）。

阅读描述后，如果你感到怀疑，这是正确的反应。这些承诺存在**重大局限性**。充其量，它们是**无约束力的宽泛框架**，公司只是大致同意，没有明确的合规标准。最糟糕的情况是，这是一场政治秀，给人一种有所进展的印象。我理解监管（尤其是在美国）通过的时间很长，因此我欣赏这些承诺在制定需要解决的关键问题方面取得的进展。但重要的是要承认，除了这些之外，**这些承诺没有实际价值**，且**没有办法强制执行良好的行为**（因为没有具体的良好行为定义）。

这使我们转向了参议院提出的法案。目前有两个法案正在审议中：

+   [布卢门撒尔参议员 / 霍利参议员](https://www.wired.com/story/senators-want-chatgpt-ai-to-require-government-license/)提议对高风险AI应用实施**许可制度**，即任何构建被视为高风险的AI模型的人员需要从联邦机构获得许可。该法案尚未确定是否需要新设立AI机构，或是否可以由现有机构如FTC或DOJ执行此规定。它还列出了**一些AI产品的具体要求**，包括伤害测试、AI不良行为的披露、允许第三方审计和披露训练数据。

+   [沃伦参议员 / 格雷厄姆参议员](https://techpolicy.press/senators-propose-a-licensing-agency-for-ai-and-other-digital-things/)提议创建一个**名为“主导平台许可办公室”的新联邦机构**。我不会详细讨论，但该法案涉及广泛的问题，如训练数据披露、研究人员访问、全面监控权限、禁止自我偏好/捆绑安排和“[关怀义务](https://techpolicy.press/senators-propose-a-licensing-agency-for-ai-and-other-digital-things/)”（即服务不能设计成“以致或可能致使个人身体、经济、关系或名誉伤害、心理伤害、歧视”）。值得注意的是，该规定**仅适用于大型平台**，而不适用于较小公司。

参议院中的两个法案涵盖了广泛的重要AI机制，如训练数据披露和安全测试。然而，这些法案各自有自己的问题，因为**大量相关内容被塞进了单个法案**中。

例如，**许可制度一再导致帮助现有企业保持市场主导地位**，这一概念被称为“监管捕获”。你可以在电信和医疗等几个市场中看到这种情况，这些市场变得非常低效，消费者即使支付了大量费用也得不到好的服务。OpenAI当然支持许可，因为这帮助他们在我认为是一个[快速商品化市场](/ai-startup-trends-insights-from-y-combinators-latest-batch-282efc9080ae)——AI模型市场中保持市场份额。我并不是说OpenAI的意图不好，但重要的是要看清激励机制。

另一个例子是沃伦/格雷厄姆法案中一些极其宽泛的语言，关于[“关怀义务”](https://techpolicy.press/senators-propose-a-licensing-agency-for-ai-and-other-digital-things/)——它指出被覆盖的实体：

+   *不得以“导致或可能导致…身体、经济、关系或名誉伤害、心理伤害…歧视”的方式设计其服务*

+   *必须缓解“材料对被覆盖实体拥有或控制的任何平台上的身体、情感、发展或物质伤害的风险”*

虽然我同意声明的精神，但**几乎不可能制定出将这种意图转化为具体标准的良好监管措施**，**这些标准可以被监管机构执行**，而不会将其变成政治动机的表演。

Warren/Sen. Graham法案中的另一个问题是对大型平台的关注。我完全支持对大型平台进行监管，以维持市场竞争性（这反过来有利于消费者），但**针对特定公司采用“所有大型企业都是坏的”策略的监管往往会产生意想不到的后果**，并且长期往往导致市场非常无效。大型平台（例如Microsoft Azure）默认情况下可能比较小的AI公司（可能更关注于增长）更谨慎地打击恶意行为，因此说AI监管应该只适用于较大的公司似乎效果不佳。

因此，机制基础的监管理由——一种专注于监管**与有意义的AI风险严格相关的非常具体的机制**的方式。这种方法具有双重好处，即**更容易通过/获得共识** + **避免强硬手段造成的意想不到的长期市场后果**。

# 机制基础的监管理由

在[DOJ诉谷歌案](https://thisisunpacked.substack.com/p/doj-v-google-case-outline-and-arguments)中，我们讨论了美国司法部如何针对谷歌从事的特定反竞争机制（特别是设备制造商必须同意繁重条款才能获取重要的Android服务的Android交易）。这为美国司法部提供了更清晰的机会来证明过去的垄断行为，并禁止未来类似行为。这不同于一些[FTC的失误](/openais-web-crawler-and-ftc-missteps-a14047f4ff69)，他们在尝试“所有大型企业都是坏的”策略（例如微软/动视）时未能成功，案件也因此被无情地驳回。

类似地，要监管AI，针对特定机制的集中方法更可能成功。成功的定义是能够有效地缓解AI风险，保护消费者，同时维持市场竞争性，使新技术可以对社会产生积极影响。以下是值得关注的一些特定机制，以缓解AI风险：

**模型拥有者和分销商的责任**：我不同意OpenAI提出的两种减轻有害使用案例的解决方案——许可制度和与用户共享责任。许可制度增加了市场准入的障碍，帮助现有企业保持市场份额，并扼杀创新——试想一下，如果每个AI初创公司和每个训练模型的公司都必须先从政府那里获得许可才能进行任何操作，那将会如何。AI服务提供商和用户之间的共享责任框架在理论上很好，但：1）今天在某种形式上确实存在（例如，如果你基于ChatGPT提供的见解犯罪，你可以根据现有法律被起诉），2）客观上很难将AI服务提供商和用户之间的坏结果责任进行划分。

更好的方法是让模型拥有者**和**分销商对其产品的有害使用负责。例如，如果OpenAI的模型和Microsoft Azure的计算能力被恶意用户用于策划钓鱼攻击，那么OpenAI和Microsoft就应承担**合理的尽职调查责任，了解其客户**及其对产品的预期使用。一个更具战术性的方法可以是限制用户可用的功能集，直到他们得到验证。这与金融机构必须遵守的KYC（了解你的客户）要求没有太大区别。

**为模型训练中使用的数据制定版权法规，披露训练数据集，并允许内容拥有者选择退出**：数据抓取是当前内容拥有者的[主要问题](https://thisisunpacked.substack.com/p/data-scraping-in-the-spotlight-language-models)。AI提供商在未获得内容拥有者同意和合理补偿的情况下使用抓取的数据来构建商业分发模型。如果法院裁定这不构成版权侵权，那么这是一个明确的信号，表明需要新的法规来规定内容拥有者的权利，以维持一个繁荣的内容生态系统。对这一点的理所当然的扩展是强制要求模型提供商披露训练数据。

另一个相关机制是允许内容拥有者选择退出其数据用于模型训练，并且做到这一点而不附带掠夺性的“捆绑”条件。例如，谷歌不能说如果你不提供数据用于训练，我们就不会在搜索中索引你。像OpenAI这样的公司在内容拥有者面前的筹码较少，但你可以想象像微软、亚马逊这样的更大企业具有更广泛的产品组合，能够迫使人们交出他们的数据。

**对用户数据的完全控制：** 一些具体机制可以减轻 AI 带来的用户隐私风险。首先，模型提供者应被迫删除训练中的个人信息。需要明确什么构成个人信息（例如，名人的 Wikipedia 页面信息不是个人信息，但 ZoomInfo 数据库中的电子邮件和电话号码是）。其次，应禁止公司将消费者功能与用户愿意提供数据用于模型训练挂钩（例如，OpenAI 不能说不提供访问聊天记录，除非用户将所有数据交给他们进行训练）。这里有明确的先例——苹果的应用跟踪透明度框架（我承认这不是监管）禁止应用将功能隐藏在跟踪选择墙后，而欧盟的广告监管禁止平台将功能隐藏在选择墙后用于行为广告。

**内容水印/来源：** 随着 AI 生成内容的激增，无论是文本还是图像/视频，能够区分 AI 生成的内容，尤其是当这些内容虚假或误导时，变得越来越重要。需要某种框架来定义什么情况应要求披露 AI 内容。例如，如果你使用 ChatGPT 写了一封销售推广邮件，这似乎无害，不应要求披露。但如果你在 Twitter 上分享政治内容且有大量关注者，则应要求披露。好的监管在这里应少一些对实际解决方案的规定，而应制定一个框架供公司参考，由自由市场找出实际解决方案（例如，一个初创公司可以出现，用于检测 Twitter 上的 AI 生成的政治内容，然后 Twitter 可以与其合作）。

# 结论

总的来说，我对当前围绕这一主题的早期对话感到鼓舞，这与过去那些将监管视为事后思考的技术不同。AI 具有巨大的优势和风险——一种深思熟虑、基于机制的监管方法可以帮助减轻 AI 的风险，同时确保市场竞争存在，以充分发挥这一技术的优势。

🚀 如果你喜欢这篇文章，请考虑订阅 [**我的每周通讯 Unpacked**](https://thisisunpacked.substack.com/?utm_source=medium&utm_medium=article_tds)**。** 每周，我会发布一个 **关于当前技术主题/产品策略的深度分析**，以 10 分钟阅读的形式呈现。祝好，Viggy。

[](https://thisisunpacked.substack.com/?utm_source=medium&utm_medium=article_tds&source=post_page-----391ddcef09d--------------------------------) [## Unpacked | Viggy Balagopalakrishnan | Substack

### 每周将一个技术主题/产品策略的深度分析发送到你的收件箱。点击阅读 Viggy 的 Unpacked……

[thisisunpacked.substack.com](https://thisisunpacked.substack.com/?utm_source=medium&utm_medium=article_tds&source=post_page-----391ddcef09d--------------------------------)
