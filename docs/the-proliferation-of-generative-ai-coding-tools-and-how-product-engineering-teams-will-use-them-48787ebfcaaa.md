# AI 编程工具的到来：产品工程团队将如何使用它们

> 原文：[`towardsdatascience.com/the-proliferation-of-generative-ai-coding-tools-and-how-product-engineering-teams-will-use-them-48787ebfcaaa`](https://towardsdatascience.com/the-proliferation-of-generative-ai-coding-tools-and-how-product-engineering-teams-will-use-them-48787ebfcaaa)

![](img/c39ca7daba5eae128001d42abfa4127f.png)

作者使用 Midjourney 制作的图像

## 生成式 AI 将如何影响产品工程团队 — 第二部分

[](https://mark-ridley.medium.com/?source=post_page-----48787ebfcaaa--------------------------------)![Mark Ridley](https://mark-ridley.medium.com/?source=post_page-----48787ebfcaaa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----48787ebfcaaa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----48787ebfcaaa--------------------------------) [Mark Ridley](https://mark-ridley.medium.com/?source=post_page-----48787ebfcaaa--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----48787ebfcaaa--------------------------------) ·10 分钟阅读·2023 年 7 月 26 日

--

*这是一个六部分系列的第二部分，探讨了针对开发者的生成式 AI 生产力工具（如 Github Copilot、ChatGPT 和 Amazon CodeWhisperer）如何影响整个产品工程团队的结构。*

在 [第一部分，我们探讨了](https://mark-ridley.medium.com/how-generative-ai-will-impact-product-engineering-teams-83a5eaa8fc60)：

1.  产品工程的现状以及随着生成式 AI 工具的兴起，团队是否需要更少的人工工程师的可能性。

1.  传统的 5:1 比例在技术团队中的应用：在行业中，通常每位产品经理对应大约五名工程师。

1.  产品经理和工程师在当前产品开发过程中的角色，以及这些角色随着 AI 进步可能发生的变化。

1.  过去的研究如何对哪些职业受到 AI 影响最小的预测产生了偏差，以及大型语言模型（LLMs）如何颠覆了这些预测，特别是对技术和创意行业的影响。

# AI 编程工具的爆炸性增长

自动化几乎自软件工程存在以来就已经成为其一部分。**埃里克·雷蒙德**的 2003 年里程碑式论文，[《Unix 编程艺术》](https://en.wikipedia.org/wiki/The_Art_of_Unix_Programming) 反思了 17 条软件工程师的设计原则，其中包括 [生成规则](http://www.catb.org/esr/writings/taoup/html/ch01s06.html#id2878742)：“*避免手动编程；在可能的情况下，编写程序来编写程序*”。

雷蒙德的建议即使在发布 20 年后仍然适用：

> “人类在关注细节方面 notoriously 表现糟糕。因此，任何形式的手动编程都是延迟和错误的丰富来源。你的程序规范越简单和抽象，人类设计者弄对的可能性就越大。生成的代码（每一层）几乎总是比手动编写的更便宜、更可靠。”

自 Raymond 写下这些话以来，我们已经开发出了自动化测试工具，[linters](https://owasp.org/www-project-devsecops-guideline/latest/01b-Linting-Code)（自动检查我们编写的代码的工具）、开发环境的自动补全（像拼写检查器，但用于代码），甚至框架（如 React 和 Django），它们自动生成大量的基础代码，用于网站和移动应用等通用应用程序。在大多数情况下，开发者对自动化赞不绝口，同时始终默默相信我们的经验、技能和创意独特性会让我们难以被替代。麦肯锡及其同行[也告诉我们](https://www.mckinsey.com/featured-insights/future-of-work/jobs-lost-jobs-gained-what-the-future-of-work-will-mean-for-jobs-skills-and-wages)我们会从 AI 自动化的魔爪中安全逃脱，这一点也没有帮助。

代码自动化领域最近的补充是一组工具，它们承诺[与开发者平等地并肩工作](https://github.com/features/preview/copilot-x)，看起来非常有能力远超我们之前的想象。就像 Raymond 的第 17 条原则一样，这些工具可能会比人类更擅长编写复杂的软件。

我写作时，*开发者辅助工具*中的**头号明星**可能是[Github Copilot](https://github.com/features/copilot)，它毫不含糊地自称为*配对程序员*（即那个坐在你旁边共同编写代码的编程伙伴）。Copilot 于 2022 年 6 月发布给开发者，而它那更年轻、更迷人的兄弟，*Copilot X*，则在[今年 3 月](https://github.blog/2023-03-22-github-copilot-x-the-ai-powered-developer-experience/)宣布。Github 声称 Copilot 能够提高开发者生产力，并且他们的研究报告显示该工具将完成工程任务的时间缩短了多达 55%，这些声明看起来[完全合理](https://github.blog/2022-09-07-research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/)。

Github（别忘了这是一家由微软拥有的公司，微软还拥有 [传闻中的 45% 的 ChatGPT 制造商 OpenAI 股份](https://www.theverge.com/2023/1/23/23567448/microsoft-openai-partnership-extension-ai)）并不是唯一的选择。[亚马逊 CodeWhisperer](https://aws.amazon.com/codewhisperer/) 去年也宣布推出，声称能够 [提高开发者生产力 57%](https://devclass.com/2023/04/13/aws-codewhisperer-ai-coder-now-generally-available-remains-free-for-individual-developers/)。像 Copilot 一样，CodeWhisperer 可以在工程师的开发环境（IDE）中访问，并能够纠正、评论、解释和编写代码。

![](img/a6d89844b6e5c3a709e43c1f9f569cd3.png)

IDE 很久以来已经能够提供代码自动补全，但有了 Copilot X，它可以在很少的上下文下编写整个代码段

在一篇关于 [Github 团队如何开发和改进 Copilot 的引人入胜的文章](https://github.blog/2023-05-17-inside-github-working-with-the-llms-behind-github-copilot/)中，[John Berryman](https://github.com/jnbrymn)，Github 的高级研究员，解释了不仅仅是开发者面前的代码被用来提示 AI 模型。Berryman 解释道，

> “关键在于我们不仅要提供给模型 GitHub Copilot 用户当前正在编辑的原始文件；相反，我们会在 IDE 内寻找额外的上下文信息，以提示模型更好的完成方案。”

正是这些开发工具可以利用的更广泛的上下文——IDE、开发者机器上的文件、git 中的代码库，甚至潜在的应用程序文档——使得这些工具如此强大。这种广泛的上下文降低了开发者“[*提示工程师*](https://en.wikipedia.org/wiki/Prompt_engineering)”从 AI 中获取解决方案所需的技能水平。改进提示的示例可以从它们操作的环境内直接获得，也可以从 [数十亿行](https://aws.amazon.com/codewhisperer/features/) 预先存在的代码中获得。这使得大型语言模型不仅能生成合适的输出，还能超越普通开发者的能力。

有了 Copilot X，代码界面本身不再是 AI 唯一可以使用的调色板，也不再仅限于自动生成代码。用户可以在开发环境中突出显示代码，并简单地询问代码的作用：

![](img/558411f94339e06562ed43812a252aab.png)

Copilot X 扩展了编码助手的范围，进入了类似 ChatGPT 的聊天环境

虽然 Copilot 和 Codewhisperer 是针对开发者优化的大型语言模型的具体实现，但即使是通用的 ChatGPT 作为工程伴侣也非常方便。

我已经使用了 GPT-3.5 和 GPT-4 模型好几个月，涉及各种任务，并继续对它的能力感到困惑——无论是代码还是其他多个学科。最近，我在重新使用生疏的 Python 技能来玩弄 OpenAI 的 API 时，似乎有些不厚道地没有向 ChatGPT 请求帮助。

正如你现在所预期的，通用聊天机器人能够给我一些完全合理的设置说明，这些说明反映了[在线文档](https://platform.openai.com/docs/quickstart/build-your-application)，但额外的好处是我可以通过对话逐步展开并扩展到其他话题（比如为什么 Python 在我每次安装时总是在 Windows 上无法正常工作）。

![](img/b15df0c13d1943c09fbf3ad2bbfe625b.png)

即便是通用的 ChatGPT 也非常擅长在技术领域提供各种帮助。

为了不被 Copilot 和 Codewhisperer 落在后头，OpenAI 最近宣布了他们自己增强代码功能的版本——一个带有代码解释器的 GPT-4 模型，它既可以编写也可以执行 Python 代码。这改进了 ChatGPT 在相当通用的模型中的代码补全实现，实际上允许它在聊天环境中直接**编写和运行**代码。

![](img/b5c3f438480fd01d17bac1b8999b1b76.png)

带有代码解释器扩展的 ChatGPT 不仅可以编写代码，还可以运行代码。

到目前为止，我花了很多时间讨论这个领域的三大玩家：Github、Amazon 和 OpenAI，但其他公司也不甘示弱。谷歌宣布了[Google Cloud 的 Duet AI](https://cloud.google.com/blog/products/application-modernization/introducing-duet-ai-for-google-cloud)，这是一个类似 Copilot 和 Codewhisperer 的代码助手，同时还在其 Colab 中提供了[AI 辅助](https://blog.google/technology/developers/google-colab-ai-coding-features/)机器学习工具（实现了 Codey，这是基于他们自己 PaLM 2 模型的一系列代码模型）。

在开发者特定工具之外，微软正在推出一系列 Copilot，包括 Windows 和[Office（价格令人咋舌）](https://www.theverge.com/2023/7/18/23798627/microsoft-365-copilot-price-commercial-enterprise)，Salesforce 最近[似乎重新品牌](https://www.salesforce.com/plus/experience/salesforce_ai_day/series/salesforce_ai_day/episode/episode-s1e1)了他们的每一个产品，在产品名称后添加了‘GPT’。谷歌也不甘落后，预计将把 Duet 添加到 Google Workplace，这将像 Office Copilot 一样帮助人们编写文档和创建稍微少些枯燥的演示文稿。

看起来无论你往哪里看，都有人在将资金投入到使人类更高效的 AI 模型中。表面上看，早期迹象表明这一切是有效的，我们应该为其影响做好准备。

# 产品工程团队将如何使用这些工具？

第一次我花大量时间用 ChatGPT 生成代码时，我实际上是在要求它生成一些用户故事。一位朋友让我帮忙编写一个应用程序，而我的专业编程日子现在已经远去，我需要一些帮助。

我的思考过程相当简单：*“我现在不能再编程了，但我可以用开发人员可以理解的方式描述所需的内容，这样我们就可以聘请一个自由职业者来实际编写代码”*。

我向 ChatGPT 解释了我希望它创建一些 [行为驱动设计](https://cucumber.io/blog/bdd/user-stories-are-not-the-same-as-features/)（BDD）故事。BDD 是一种相当普遍采用的开发方法，它作为客户（和产品经理）所需内容与工程师将编写的代码之间的中介。编写 BDD 故事是检验自己对问题理解的一个好方法，所以自然地，我让 ChatGPT 为我做了这件事。

![](img/310c5f07eb707f29274049e9f483ac1d.png)

ChatGPT 创建的一个 BDD 用户故事的示例——因为巫师与机器人。这就是全部。

重要的是要理解，BDD 故事并不是对将要执行的代码的近似，而是一个非常好的方法来了解交付的应用程序是否与产品经理真正要求的相符（并且希望，符合客户真正想要的）。

在要求这些用户故事之后，我意识到没有理由不能要求 ChatGPT 编写代表这些故事的代码。通过一点提示工程，我建议 ChatGPT 用 Javascript、HTML 和 CSS（构建简单网页应用程序的最基本代码）创建这个应用程序。两分钟后，我得到了一个应用程序的构建块，它代表了我们为其编写用户故事的功能基础。

一旦我让应用程序以其摇摇晃晃、刚孵化出的形式运行时，我意识到任何自尊心的开发人员都会基于代码和用户故事创建测试。因此，在与 ChatGPT 简要讨论了最适合 Javascript 的测试运行器后，我要求创建一组更技术性的、测试驱动开发的（[TDD](https://www.testim.io/blog/is-jasmine-bdd-or-tdd/)）测试，以确保应用程序符合我最初的 BDD 故事。

![](img/11b688f47d6a924d4b30afed66fa19c9.png)

ChatGPT 很好地推荐了 Jasminne Javascript 测试运行器，并编写了测试。

整个过程最令人惊讶的是，完成一些开发人员最不喜欢的任务的轻松程度。对于大多数工程师来说，工作的美在于找到解决方案的创造力和代码成功运行的刺激。编写测试、创建文档、编写用户故事甚至引导基本代码是乏味的任务，从情感上讲，最好避免。

通过这个过程，我想起了 [Simon Wardley](https://www.linkedin.com/in/simonwardley/?originalSubdomain=uk) 的一个发人深省的 [推文线程](https://twitter.com/swardley/status/1490777578810101765)。Simon 是 [Wardley Maps](https://learnwardleymapping.com/) 方法的创建者。Simon 的评论提出，你的测试套件应当表示和解释你产品实际包含的复杂且相互依赖的关系图，这也是你大量知识产权所在之处。

> X : 编写一个商品规范的最佳方法是什么？
> 
> Me : 你的测试套件。
> 
> X : 啊？
> 
> Me : 每个新事物都从几个基本的测试开始，随着它的发展会增加更多测试，你的产品应该基于这些测试构建，最终这些测试应帮助定义商品。
> 
> Me : 你的业务、硬件和软件应该尽可能地将测试驱动开发融入其中。如何在复杂的环境中改变任何东西而不进行测试呢？地图上的每一条线都是一种关系，一个应该有测试的接口……
> 
> …这些测试定义了接口的操作，它们是你交给他人的规范，你用来检查产品的规范，你用来测试商品的规范，然后决定你希望进行的交易。这些测试应随着事物本身的演变而扩展和演变。

尽管测试的编写没有应用代码那样令人兴奋，并且常常被视为维护的负担，但良好的测试展示了应用程序的预期行为。更重要的是，测试不仅描述了某物*应该*如何表现，还展示了它确实能够一再如此表现。定义一个真正有价值的测试套件的范围和细节是产品和工程团队共同完成的协作工作的一部分。Simon 的推文激励我思考，也许，一个经过精心设计的测试开始了生成应用程序的未来。

从这个角度看，我们可以开始设想一个接受生成式 AI 工具的产品工程团队的未来。挖掘客户需求、理解潜在解决方案、分析为客户提供服务所创造的价值并优先排序工作将仍然像今天一样重要。产品管理无疑会从生成式 AI 工具中受益，但我怀疑对产品学科的影响会比对技术团队的影响小。

**这让我们回到了那五名工程师。**

**在第三部分，你可以阅读到：**

1.  生成式 AI 工具如何可能颠覆长期以来的 5 名工程师对 1 名产品经理的比例。

1.  像 Github Copilot 和 AWS Amplify Studio 这样的工具如何重塑产品开发，将工程师的重点从手动编码转向设计、架构和集成。

1.  生成式 AI 工具如何帮助面临过时技术、轻松处理复杂移植和重构的团队。

1.  AI 工具在移动和网页应用开发中的可能统一影响，减少重复劳动，弥合网页、Android 和 iOS 开发之间的技能差距。

1.  编码自动化对初级开发者和工程进展的影响

[点击这里阅读第三部分](https://mark-ridley.medium.com/if-engineers-start-to-use-ai-coding-tools-what-happens-to-our-product-teams-acd55fb273dd)

**本系列其他文章：**

+   [第一部分](https://mark-ridley.medium.com/how-generative-ai-will-impact-product-engineering-teams-83a5eaa8fc60)

+   [第三部分](https://mark-ridley.medium.com/if-engineers-start-to-use-ai-coding-tools-what-happens-to-our-product-teams-acd55fb273dd)

附注：如果你喜欢这些关于团队的文章，可以看看我的 [Teamcraft 播客](https://www.teamcraft.uk/)，在这里我和我的联合主持人安德鲁·麦克拉伦（Andrew Maclaren）与嘉宾讨论使团队运作的因素。
