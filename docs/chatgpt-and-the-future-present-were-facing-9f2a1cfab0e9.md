# ChatGPT 与我们面临的未来（现在）

> 原文：[`towardsdatascience.com/chatgpt-and-the-future-present-were-facing-9f2a1cfab0e9`](https://towardsdatascience.com/chatgpt-and-the-future-present-were-facing-9f2a1cfab0e9)

## 意见

## 2023 年将比 2022 年更加紧张和压倒性，所以请系好安全带。

[](https://albertoromgar.medium.com/?source=post_page-----9f2a1cfab0e9--------------------------------)![Alberto Romero](https://albertoromgar.medium.com/?source=post_page-----9f2a1cfab0e9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9f2a1cfab0e9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f2a1cfab0e9--------------------------------) [Alberto Romero](https://albertoromgar.medium.com/?source=post_page-----9f2a1cfab0e9--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9f2a1cfab0e9--------------------------------) ·10 分钟阅读·2023 年 2 月 2 日

--

![](img/927f6f493c472e595a970f0c616d9257.png)

版权：Midjourney

直到 ChatGPT 不再是 AI 领域最重要的新闻，我想我们还会继续讨论它……开玩笑的，我会确保穿插其他话题，否则我们可能会感到疲惫。

关于 ChatGPT 的直接和长期影响仍有很多话题要讨论。我已经写过关于 [ChatGPT 是什么](https://thealgorithmicbridge.substack.com/p/chatgpt-is-the-worlds-best-chatbot) 和 [如何最大化利用它](https://thealgorithmicbridge.substack.com/p/how-to-get-the-most-out-of-chatgpt)，关于 [识别其输出的挑战](https://thealgorithmicbridge.substack.com/p/openai-has-the-key-to-identify-chatgpts)，以及 [它对 Google 和传统搜索引擎的威胁](https://thealgorithmicbridge.substack.com/p/microsoft-vs-google-will-language)，但我还没有涉及到那些 [一些人预见到的风险和危害](https://garymarcus.substack.com/p/ais-jurassic-park-moment) 在现实世界中已经开始显现。

发布两个月后，我们都可以同意 ChatGPT 已经进入主流，并将 AI 领域带入了公众视野。作为一个小故事，一个对 AI 一无所知的朋友在我告诉他之前，就已经开始谈论 ChatGPT 了。这对我来说是第一次——而我并不是唯一一个。

这就是为什么讨论 AI 的后果非常紧迫的原因：ChatGPT 比任何关于如何有效使用它或绝对不应该如何使用它的资源传播得都要快。如今使用 AI 工具的人数比以往任何时候都多（不仅仅是 ChatGPT；Midjourney 在 Discord 服务器上几乎有 1000 万会员），这意味着比以往任何时候都更多的人会*误用*这些工具。

与我预测/推测性的文章不同，这篇文章不是关于*可能* *发生*的事情，而是关于*正在发生*的事情。我将重点讨论 ChatGPT，因为这是世界上讨论的焦点，但以下大部分内容也适用于其他类型的生成 AI，只要进行适当的翻译。

*这篇文章摘自* [***The Algorithmic Bridge***](https://thealgorithmicbridge.substack.com/subscribe?)*，这是一本教育性新闻通讯，旨在弥合 AI、算法与人之间的鸿沟。它将帮助你理解 AI 对你生活的影响，并开发更好地应对未来的工具。*

[](https://thealgorithmicbridge.substack.com/subscribe?source=post_page-----9f2a1cfab0e9--------------------------------) [## The Algorithmic Bridge

### **弥合算法与人之间的鸿沟**。这是关于对你有意义的 AI 的新闻通讯。点击阅读《The…》

[thealgorithmicbridge.substack.com](https://thealgorithmicbridge.substack.com/subscribe?source=post_page-----9f2a1cfab0e9--------------------------------)

# ChatGPT 的危害不再是理论上的

2024 年 1 月 6 日，安全研究小组 Check Point Research (CPR) 发表了一篇名为 “[OpwnAI: 网络罪犯开始使用 ChatGPT](https://research.checkpoint.com/2023/opwnai-cybercriminals-starting-to-use-chatgpt/)” 的惊人文章。虽然并不意外，但我没想到会这么快。

CPR [此前研究过](https://research.checkpoint.com/2022/opwnai-ai-that-can-save-the-day-or-hack-it-away/) 恶意黑客、诈骗者和网络罪犯如何利用 ChatGPT。他们展示了聊天机器人如何“创建完整的感染流程，从网络钓鱼到运行反向 Shell”，以及如何生成动态运行的脚本，适应环境。

尽管 OpenAI 设置了警告框，当 CPR 强迫 ChatGPT 执行违反使用政策的操作时，会出现橙色警告通知，研究小组仍然顺利生成了一封简单的网络钓鱼邮件。他们总结道：“复杂的攻击过程也可以被自动化，使用 LLMs API 生成其他恶意伪造物。”

![](img/256d6ea6cdf1b7ff1bd59f78a82e82bb.png)

ChatGPT 生成的基本钓鱼邮件。信用：[CPR](https://research.checkpoint.com/2022/opwnai-ai-that-can-save-the-day-or-hack-it-away/)（已授权）

CPR 研究人员对 ChatGPT 仅能在*假设*情况下执行这项任务的证据并不满意（怀疑者常常会收到这样的批评：他们警告的潜在风险从未转化为现实世界的危害）。他们希望找到人们以类似方式实际滥用它的真实例子。他们找到了。

CPR 分析了“几个主要的地下黑客社区”，发现至少有 [三个具体例子](https://research.checkpoint.com/2023/opwnai-cybercriminals-starting-to-use-chatgpt/) 表明网络犯罪分子正在以违反服务条款且可能直接造成可测量危害的方式使用 ChatGPT。

首先是信息窃取者。在一个题为“ChatGPT — 恶意软件的好处”的帖子中，一位用户分享了他的实验，他“重建了许多恶意软件变种。”正如 CPR 所指出的，OP 的其他帖子表明，“这个人[旨在]向技术能力较差的网络犯罪分子展示如何利用 ChatGPT 进行恶意目的。”

![](img/59aafd8d416e98226ce27e0f1be527e6.png)

“网络犯罪分子展示了他如何使用 ChatGPT 创建信息窃取工具。” 版权归 [CPR](https://research.checkpoint.com/2023/opwnai-cybercriminals-starting-to-use-chatgpt/)（已授权）

其次是加密工具。一位名为“USDoD”的用户发布了一个包含“加密和解密功能”的 Python 脚本。CPR 认为，“这个脚本可以很容易地修改为完全加密某人的机器，而无需任何用户交互。”虽然 USDoD 的“技术技能有限”，但他“参与了各种非法活动。”

![](img/792b3b5c72c4a2c46912b222ad6067b0.png)

“网络犯罪分子 USDoD 发布了多层加密工具。” 版权归 [CPR](https://research.checkpoint.com/2023/opwnai-cybercriminals-starting-to-use-chatgpt/)（已授权）

最后的例子是诈骗活动。帖子标题很有启发性：“滥用 ChatGPT 创建暗网市场脚本。” CPR 写道：“这些网络犯罪分子发布了一段代码，利用第三方 API 获取最新的加密货币 …… 价格，作为暗网市场支付系统的一部分。”

![](img/a6224c6f694115ad9a08977e2655678e.png)

“威胁行为者使用 ChatGPT 创建暗网市场脚本。” 版权归 [CPR](https://research.checkpoint.com/2023/opwnai-cybercriminals-starting-to-use-chatgpt/)（已授权）

很明显，ChatGPT 因为免费使用和高度直观，吸引了包括技术水平较低的网络犯罪分子。正如 Check Point 的威胁情报组经理 **谢尔盖·谢基维奇** 所解释的那样：

> *“正如 ChatGPT 可以被用来协助开发者编写代码，它也可以被用作恶意目的。尽管我们在本报告中分析的工具相当基础，但更复杂的威胁行为者提升他们使用基于 AI 的工具的方式只是时间问题。”*

ChatGPT 作为在线安全问题的驱动因素并非恐慌者的假设，而是一个难以否认的现实。对于那些认为这在 ChatGPT 之前也可能发生的人，有两点：首先，ChatGPT 可以弥合技术差距。其次，规模在这里非常重要 —— ChatGPT 可以在几秒钟内自动编写脚本。

# OpenAI 不应该这么快就让 ChatGPT 免费使用

网络安全、虚假信息、剽窃……许多人反复警告 ChatGPT 类 AI 可能引发的问题。现在恶意用户开始增多。

仍有人可能试图为 ChatGPT 辩护。也许它并不是*那么*有问题——利大于弊——但*也许*它是。一个“也许”应该足以让我们三思而后行。OpenAI 在 GPT-2 被认为是“无害”时放松了警惕（他们看到“[迄今没有强有力的误用证据](https://openai.com/blog/gpt-2-1-5b-release/)”），而且他们再也没有重新提高警惕。

我同意 Scott Alexander 的观点，“[也许世界领先的 AI 公司无法控制他们的 AI 是一件坏事](https://astralcodexten.substack.com/p/perhaps-it-is-a-bad-thing-that-the)”。也许通过人工反馈的强化学习还不够好。也许公司应该找到更好的方法来控制他们的模型，如果他们打算将它们放出去。也许 GPT-2 并不那么危险，但过几次迭代后我们就有了需要担忧的东西。如果不是，我们在再过几次迭代后就会有。

我不是说 OpenAI 没有尝试——他们尝试过（甚至因过于保守而受到批评）。我争论的是，如果我们将这种“我已经尽力做好，所以现在我有了发布 AI 的绿灯”的心态延续到短期未来，我们将遇到越来越多的弊端，而没有任何好处可以弥补。

有一个问题困扰了我几个星期：如果 OpenAI 对做对事如此担忧，为什么他们没有在将模型公开之前建立[水印方案](https://thealgorithmicbridge.substack.com/i/89278939/openais-plan-to-identify-chatgpts-outputs)来识别 ChatGPT 的输出呢？Scott Aaronson 仍在努力使其有效——*一个月* *之后* 模型完全 viral 了。

[来源](https://twitter.com/TheWeatherStn/status/1610313487540195331)

我认为水印不会解决这种技术所涉及的根本问题，但它会通过争取时间来帮助。时间让人们适应，科学家们寻找解决最紧迫问题的办法，以及监管者制定相关立法。

# GPT 检测器是最后一个（健康的）前沿。

由于 OpenAI 的不作为，我们只能依赖于勉强尝试建立 GPT 检测器，这些检测器可能为人们提供避免 AI 误信息、诈骗或钓鱼攻击的手段。一些人试图将[3 年前的 GPT-2 检测器](https://openai-openai-detector.hf.space/)改用于 ChatGPT，但[它不起作用](https://twitter.com/JanelleCShane/status/1601729685385535488)。其他人，如[Edward Tian](https://twitter.com/edward_the6)，普林斯顿大学计算机科学与新闻学的高年级学生，已经从零开始开发了专门针对 ChatGPT 的系统。

[来源](https://twitter.com/edward_the6/status/1610067688449007618)

目前，已有超过 10,000 人测试了 GPTZero，包括我在内（[这是演示](https://etedward-gptzero-main-zqgfwb.streamlit.app/)）。田正在打造一个已有[3K+ 教师订阅](https://gptzero.substack.com/p/gptzero-update-v2)的产品。我承认我曾经骗过它一次（仅仅因为 ChatGPT 拼写错误），但也没有太费劲。

该检测器相当简单；它评估一段文本的“困惑度”和“突发性”。困惑度衡量一句话有多“令检测器惊讶”（即输出单词的分布与语言模型预期的匹配程度），突发性则衡量困惑度在句子之间的一致性。简而言之，GPTZero 利用人类写作通常比 AI 更奇怪的事实 —— 这一点在你读到一页 AI 生成的文本时立刻显而易见。它如此乏味……

在一个[<2%的误报率](https://gptzero.substack.com/p/gptzero-update-v1)下，GPTZero 是最好的检测器。田自豪地说：“人们有权知道写作是否是人类所为，”[他对《每日野兽》说](https://www.thedailybeast.com/princeton-student-edward-tian-built-gptzero-to-detect-ai-written-essays)。我同意 —— 即使 ChatGPT 不抄袭，人们声称自己是 ChatGPT 所写内容的作者也是道德上不对的。

但我知道它并非万无一失。一些对输出的更改（例如拼写错误或夹杂自己的内容）可能足以欺骗系统。要求 ChatGPT 避免重复单词效果很好，[正如 Yennie Jun 在这里展示的](https://blog.yenniejun.com/p/using-ai-to-improve-ai-generated)。最后，GPTZero 可能很快会过时，因为新的语言模型每隔几周就会出现 —— AnthropicAI 非正式地宣布了 Claude，[正如 Riley Goodside 的分析所证明](https://twitter.com/goodside/status/1611309175828520962)，它比 ChatGPT 更好。

而且[GPT-4](https://thealgorithmicbridge.substack.com/p/gpt-4-rumors-from-silicon-valley)就要来了。

这是一场猫鼠游戏，正如一些人所称 —— 而且老鼠总是领先一步。

# 禁止 ChatGPT：一个糟糕的解决方案

如果检测器运作正常，很多人会感到愤怒。大多数人希望无障碍地使用 ChatGPT。例如，学生们无法在书面作业中作弊，因为一位精通 AI 的教授可能知道检测器的存在（[这已经发生过](https://www.facebook.com/title17/posts/pfbid0DSWaYQVwJxcgSGosS88h7kZn6dA7bmw5ziuRQ5br2JMJcAHCi5Up7EJbJKdgwEZwl)）。3K+ 教师订阅田的即将推出的产品这一事实说明了一切。

但是，由于检测器的可靠性不足，那些不愿面对必须猜测某些书面材料是否是 ChatGPT 产品的不确定性的人采取了最保守的解决方案：禁止 ChatGPT。

《卫报》周五报道了“[纽约市学校禁止 ChatGPT](https://www.theguardian.com/us-news/2023/jan/06/new-york-city-schools-ban-ai-chatbot-chatgpt)”的消息。部门发言人珍娜·莱尔提到，“对学生学习的负面影响的担忧，以及对内容的安全性和准确性的担忧”是决定的原因。尽管我理解教师的观点，但我认为这不是一个明智的做法——这可能是更简单的选择，但不是正确的选择。

Stability.ai 的戴维·哈在新闻发布时发了这条推特：

[来源](https://twitter.com/hardmaru/status/1611014829950799878)

我承认（[之前也有做过](https://thealgorithmicbridge.substack.com/i/74599448/kids-are-using-ai-to-write-essays-and-get-straight-as-can-education-keep-up-with-ai-progress)）学校面临的问题（例如广泛的难以检测的抄袭），但我必须同意哈的观点。

这是一个困境：这项技术不会消失。它是未来的一部分——可能是很重要的一部分——而且学生（包括你、我和其他人）学习它是非常重要的。禁止 ChatGPT 进入学校不是解决方案。正如哈的推特所暗示的那样，禁止它可能比允许它更有害。

然而，使用它来作弊或写论文的学生会浪费教师的时间和精力，同时阻碍自己的发展而未意识到。正如莱尔所说，ChatGPT 可能会阻碍学生学习“批判性思维和解决问题的能力”。

我（以及许多人）预见的解决方案是什么？教育系统将不得不适应。尽管更困难，但这是更好的解决方案。鉴于学校系统的破损程度，这可能对学生和教师来说是双赢的局面。当然，不用说，在那之前教师最好有一个可靠的检测工具——但我们不应以此为借口来避免让教育适应这些变化的时代。

教育系统有*很多*改进的空间。如果它在这么多年里没有改变，那是因为没有足够强的动力去做。ChatGPT 给我们一个重新构想教育的理由，唯一缺失的部分是决策者的意愿。

# 人工智能是新的互联网

确实是这样。有些人将人工智能与[火或电](https://www.cnbc.com/2018/02/01/google-ceo-sundar-pichai-ai-is-more-important-than-fire-electricity.html)相比，但这些发明是逐渐融入社会的，且距离现在已经很久了。我们不知道那时候的感受。人工智能更像是互联网，它将迅速改变世界。

我在这篇文章中试图捕捉一个已经更像现在而非未来的未来。一方面，像 GPT-3 或 DALL-E 这样的人工智能确实存在，另一方面，世界上每个人都意识到了它们。这些假设（例如虚假信息、网络攻击、抄袭等）不再只是理论。它们正在发生在这里，现在，我们将看到更多绝望的措施来阻止它们（例如，建立简陋的检测器或禁止人工智能）。

我们必须假设有些事情将永远改变。但是，在某些情况下，我们可能不得不捍卫我们的立场（就像艺术家们在进行文本到图像转换时所做的，或少数群体在使用分类系统时曾经做过的）。无论你是谁，人工智能都会以某种方式影响你。你最好做好准备。

*订阅* [**算法桥梁**](https://thealgorithmicbridge.substack.com/)*。弥合算法与人之间的差距。关于对你生活重要的人工智能的通讯。*

*你还可以直接在 Medium 上支持我的工作，并通过使用我的推荐链接成为会员来获得无限访问权限* [**这里**](https://albertoromgar.medium.com/membership)! *:)*
