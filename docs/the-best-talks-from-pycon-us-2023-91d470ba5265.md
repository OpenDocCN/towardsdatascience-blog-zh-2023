# PyCon US 2023 最佳讲座

> 原文：[`towardsdatascience.com/the-best-talks-from-pycon-us-2023-91d470ba5265`](https://towardsdatascience.com/the-best-talks-from-pycon-us-2023-91d470ba5265)

## 我们对世界上最大 Python 大会的看法

[](https://medium.com/@DataScienceRebalanced?source=post_page-----91d470ba5265--------------------------------)![Leah Berg and Ray McLendon](https://medium.com/@DataScienceRebalanced?source=post_page-----91d470ba5265--------------------------------)[](https://towardsdatascience.com/?source=post_page-----91d470ba5265--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----91d470ba5265--------------------------------) [Leah Berg and Ray McLendon](https://medium.com/@DataScienceRebalanced?source=post_page-----91d470ba5265--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----91d470ba5265--------------------------------) ·11 分钟阅读·2023 年 4 月 25 日

--

![](img/0efe1695b8c2ade33f7e844ae0f0a9ed.png)

PyCon 2023 标志。图片由 [PyCon US 2023](https://us.pycon.org/2023/) 提供

从 2023 年 4 月 19 日至 4 月 23 日，我们参加了第二届 [PyCon](https://us.pycon.org/2023/) —— 这是 Python 编程语言的最大年度大会。每年，PyCon 在全球举办多个会议，我们参加了在犹他州盐湖城举行的美国会议。

今年，PyCon 庆祝了其 20 周年，在开幕式上，回顾视频展示了之前参会者分享的过去会议的照片和记忆。虽然这是我们第二次参加 PyCon，但我们很高兴重新联系了去年结识的朋友，并建立了新的联系。总的来说，我们很享受这次会议，并想回顾一些我们喜欢的数据科学相关讲座。

如果你无法参加 PyCon，也不用担心！PyCon 计划将讲座上传到他们的 YouTube 频道供大家观看。我们会在讲座上线后在这里提供链接。

下面是我们将介绍的讲座的快速概述：

+   特征工程是为每个人准备的！— Leah Berg 和 Ray McLendon

+   Ned Batchelder 的主旨演讲

+   为什么你应该关注开源供应链安全 — Nina Zakharenko

+   基于 LLM 的代理：如何使用 Haystack 开发智能 NLP 驱动的应用程序 — Tuana Celik

+   进入 Logisticverse：使用 Python 提高交通网络效率 — Uzoma Nicholas Muoh

+   生成器、协程和纳米服务 — Reuven M. Lerner

+   公共数据讨论

+   自然语言处理中的公平性和偏见缓解方法 — Angana Borah

+   10 种测试中可能犯的错误 — Shai Geva

+   Python 与 UX 的结合：通过代码提升用户体验 — Neeraj Pandey, Aashka Dhebar

## **特征工程是为每个人准备的！— Leah Berg 和 Ray McLendon**

在会议演讲开始的前两天，PyCon 提供了多种不同主题的教程。这些教程是与会者学习一个主题并通过 Python 实际应用的绝佳方式。

去年我们举办了一次自然语言处理研讨会后，能够再次被选中教授一个关于 Python 特征工程的三小时初学者研讨会，我们感到非常兴奋。通常，特征工程仅在为机器学习模型创建输入的背景下讨论。然而，我们抓住机会解释了它如何也能增强数据可视化、数据质量和可解释性。

在整个研讨会中，我们介绍了如何探索和创建离散数据和连续数据的特征。与会者通过在 Google Colab 笔记本中分析技术产品评论和股票市场数据获得了实际经验。

现在，让我们深入探讨一下我们在会议中最喜欢的一些演讲。

## **主题演讲 — Ned Batchelder**

Ned Batchelder，波士顿 Python 组织者，[coverage.py](https://pypi.org/project/coverage/) 维护者，以及 edX 的贡献者，在会议的第一天以软件开发人员沟通的重要性开场。

虽然许多会议与会者可能对这次非技术性的演讲感到惊讶，但我们发现它非常清新且急需。沟通是任何协作环境中的一个重要组成部分，而 Ned 在技术会议上强调这一点，提醒我们在这个领域中它的价值。

![](img/2b8de77fd2822a99ebae406ca1d82b2d.png)

由 [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) 拍摄，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

Ned 的演讲突出了一个重要观点：每条信息，无论是面对面交流、通过文本、电话还是其他媒介传达，都承载着信息和情感。无论你的意图如何，人们会根据各种因素（包括他们与你的历史、与你的相似性或他们当前的情感状态）来解读你的信息情感。

Ned 提出的改善沟通的建议包括实践谦逊、明确表达以及谨慎选择用词。随着他分享了不良沟通的例子以及如何改进，我们不禁反思自己的沟通风格。这是一个发人深省的提醒，让我们意识到我们都可能曾经发送或接收过被误解的信息，我们都可以从努力更有效地沟通中受益。

## **为什么你应该关心开源供应链安全 — Nina Zakharenko**

![](img/cb8b41acaff683f0f004ba5be0913949.png)

开源供应链。图片由 [SLSA](https://slsa.dev/spec/v1.0/threats) 提供

许多数据科学家依赖开源 Python 模块，如 pandas、numpy 和 scikit-learn 来进行他们的工作。然而，容易忽视这些包可能存在的潜在风险，包括它们如何成为攻击的目标以及这可能产生的影响。如果你像我们一样，可能以前没有考虑过这些问题，但了解它们对确保你的工作安全和完整性至关重要。

Nina 的演讲对供应链每个步骤中的各种攻击进行了出色的概述，并提供了这些攻击的最新实例。

+   **未经授权的更改** — 攻击者修复了一个漏洞，同时引入了另一个漏洞（例如：[Linux 伪善者提交](https://www.code.ng/2021/05/hypocrite-commits-exeperiment-that-got.html)）

+   **被攻击的源代码仓库** — 攻击者获得对私人系统的访问权限并对源代码进行恶意修改（例如：[PHP 自托管 Git 服务器](https://arstechnica.com/gadgets/2021/03/hackers-backdoor-php-source-code-after-breaching-internal-git-server/)）

+   **从修改后的源代码构建** — 攻击者从一个与官方仓库不匹配的版本构建源代码（例如：[Webmin](https://thehackernews.com/2019/08/webmin-vulnerability-hacking.html)）

+   **被攻击的构建过程** — 攻击者获得对构建系统的访问权限并注入恶意代码（例如：[SolarWinds](https://www.techtarget.com/whatis/feature/SolarWinds-hack-explained-Everything-you-need-to-know)）

+   **被攻击的依赖** — 攻击者攻击了广泛使用的依赖项以访问其依赖项（例如：[event-stream](https://blog.npmjs.org/post/180565383195/details-about-the-event-stream-incident)）

+   **上传修改后的包** — 攻击者获得对系统的访问权限并可以上传修改后的包（例如：[Codecov](https://blog.gitguardian.com/codecov-supply-chain-breach/)）

+   **被攻击的包仓库** — 攻击者攻击了整个包仓库（例如：[Browserify 伪造攻击](https://blog.sonatype.com/damaging-linux-mac-malware-bundled-within-browserify-npm-brandjack-attempt)）

+   **依赖变得不可用** — 许多其他包依赖的包不再可用（例如：[left-pad](https://qz.com/646467/how-one-programmer-broke-the-internet-by-deleting-a-tiny-piece-of-code)）

Nina 通过分享一个名为 [Scorecard](https://github.com/ossf/scorecard) 的工具来结束了演讲，该工具帮助开源维护者评估他们项目的安全状态。

## **基于 LLM 的智能体构建：如何使用 Haystack 开发智能 NLP 驱动的应用程序 — Tuana Celik**

[Deepset](https://www.deepset.ai/) 是一家拥有搜索引擎背景的欧洲公司。他们将生成预训练变换器（GPT）模型集成到他们的工作中，并创建了一个名为 Haystack 的开源库。该库让你可以快速轻松地构建一个用于简单问答（QA）系统的“智能体”。

![](img/4e9ade7e98e389df5009a509b986cca9.png)

图片由 [Jon Tyson](https://unsplash.com/@jontyson?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

尽管 Haystack 可以应用于 QA 系统之外，但演讲特别集中在这一用例上。他们首先解释了 Haystack 如何将文档转化为向量或“嵌入”，这使得在进行查询时可以快速从存储中提取相关文档。通过这种方法，代理可以用类似于 ChatGPT 的自然语言回答查询。

使用 [Hugging Face](https://huggingface.co/) 上现有的模型是 Haystack 的一大优势。即使是较简单的模型也可以在无需完整 GPT-4 风格模型的情况下提供优异的结果。通过微调这些模型，性能可以进一步提升，从而创建出世界一流的 QA 系统。

使用这些技术，可以开发一个完全自包含的系统，达到 GPT 级别的性能，而不必使用第三方 API 来冒险泄露组织的数据。

## **进入物流宇宙：使用 Python 改善运输网络的效率 — Uzoma Nicholas Muoh**

完全披露，Nick 是我们的朋友，我们在 PyCon 2022 上认识了他，我们非常喜欢他今年的演讲。Nick 深入探讨了创建高效运输系统的复杂世界。

Nick 首先讨论了运输公司、司机和产品公司不同的视角。高效运输货物的挑战在于平衡产品运输需求、司机休息时间以及减少空载运输的里程。

![](img/4068c61d009ef1434fff7c0d5bb48ec9.png)

图片由 [Sven Brandsma](https://unsplash.com/@seffen99?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

我们很喜欢 Nick 介绍了我们之前没有使用过的几种 Python 库，例如 Google 的 [ortools](https://developers.google.com/optimization/introduction/python)。这让我们深刻认识到，我们试图通过数据科学解决的许多问题，已经被其他学科（如运筹学）处理过了。

演讲还包含了使用网络图可视化货物运输位置的精彩部分。这使得识别低效路线变得容易，并找到优化整体运输过程的方法。看到这些可视化如何用于解决物流中的复杂问题，真是非常有帮助。

## **生成器、协程和纳米服务 — Reuven M. Lerner**

作为数据科学家，我们曾从一些软件开发者朋友那里简要了解过生成器，但坦率地说，我们并没有多加使用。我们很高兴从流行的 Python 教育者和作者 Reuven 那里学到了更多关于它们的知识。

生成器允许你即时迭代一系列值，而不是一次性生成所有值。它们非常适合迭代大型数据集，因为它们在内存使用上非常高效，因为它们不会将所有值存储在内存中。数据科学家可以使用生成器读取大型数据集或生成无限的数字序列。

协程可以消耗和生成值。协程可用于创建轻量级的并发任务，这些任务可以暂停和恢复执行，以允许其他任务运行。这对于网络通信或输入/输出操作等任务很有用，因为在这些任务中，切换任务而不是阻塞程序直到任务完成是有益的。

最后，Reuven 介绍了纳米服务的概念，这可以被认为是坐在程序内并通过 API（即*send()*）访问的协程，类似于微服务如何被划分为小部分，每部分都有自己的服务器和状态。这种方法允许在代码中实现更大的模块化和灵活性，因为你可以在需要时访问这些小部分或“纳米服务”。

## **公民数据讨论**

PyCon 的一个独特方面是无法在线访问或在活动结束后获取的开放空间。这些是指定的房间，参与者可以聚集在一起讨论感兴趣的话题。

我们参加了一个专门讨论公民数据的开放空间，很高兴看到一群多样化的个人分享他们独特的观点。我们从讨论中得到了一些关键的启示。

首先，我们发现每个辖区都有自己的公民数据获取政策。虽然这些数据是公开的，但获取这些数据并不总是直接的。有些辖区要求数据必须亲自获取或在实体 CD 上获取，这对那些寻求数据的人来说可能带来挑战。

![](img/ebbb63b8ab387d8ea1878ff46e3c2067.png)

[Chris Yates](https://unsplash.com/fr/@chrjy?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上的照片

讨论的第二点是，第三方承包商通常会聚合数据，但合同不总是要求数据集必须是开放的。因此，以前免费的但难以获取的数据可能变得昂贵，这让我们感到惊讶。

最后，我们了解了[美国数字服务](https://www.usds.gov/)（USDS），这是一个致力于提升美国数字服务标准的政府组织。我们很高兴见到一些 USDS 成员，并听到他们为改变合同、使公民数据更开放而做出的精彩工作。

## **自然语言处理中的公平性与偏见缓解方法 — Angana Borah**

作为自然语言处理（NLP）领域的专业人士，我们对公平性和偏见话题有强烈兴趣。这次讲座很好地涵盖了这个主题，从高层概述到对用于开发更好 NLP 解决方案的标准指标和技术的深入探索。

Angana 对她所做和将继续做的工作的热情在讲座中显而易见。她强调了在自然语言处理（NLP）中解决公平性和偏见问题的重要性，以及当前训练方法的不足。进一步的资金和研究是必要的，以开发更好的系统来解决这些问题。

对于这个话题的深入了解，我们推荐 Michael Kearns 和 Aaron Roth 的[《伦理算法》](https://www.amazon.com/Ethical-Algorithm-Science-Socially-Design/dp/0190948205)。Angana 在讲解公平性和偏见方面做得很出色，但这本书进一步扩展了隐私等话题，这在当前被广泛使用的大型语言模型（LLMs）中是一个主要关注点。

## **10 种自我挖坑的测试方法 — Shai Geva**

数据科学家常常缺乏软件开发背景，这可能导致忽视最佳实践，例如编写测试。Shai 的演讲提供了一个很好的介绍，阐述了不仅要编写测试，还要有效编写测试的重要性。他的演讲涵盖了 10 个糟糕测试的标志。

![](img/b103f7af2212c4dd38c872da56e600dd.png)

图片来源：[Karl Pawlowicz](https://unsplash.com/@karlp?utm_source=medium&utm_medium=referral) 由 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

1.  **没有测试** — 如果你还没有为你的项目编写测试，先从小而简单的测试开始。

1.  **如果不失败，就不算通过** — 始终确保你的测试以你预期的方式失败。

1.  **测试过多内容** — 每个测试应确认代码行为的一个单一事实。

1.  **语言不明确** — 在测试的名称中使用决定性、明确和具体的语言。

1.  **细节决定成败** — 尝试将所有信息孤立在测试本身，而不是在代码中跳来跳去。

1.  **测试未隔离** — 你不希望测试结果会因运行顺序的不同而变化。

1.  **范围不当** — 尝试使用一致的行为测试。

1.  **到处都是测试替身** — 尽量不要使用 mock、patch 等，因为对代码库的更改会迅速使其过时。

1.  **测试慢** — 目标是三秒内完成测试，并在观察模式下运行，以便快速识别和解决瓶颈。

1.  **错误的优先级** — 测试的目标是发现 bug。它们应该具备以下特性：可维护、快速和强大。

我们喜欢 Shai 演讲的简洁性，并且在下次编写测试时会牢记这些原则。

## **Python 与用户体验：通过代码提升用户体验 — Neeraj Pandey, Aashka Dhebar**

Neeraj 的讲座突出了用户体验和数据科学之间的协同作用。用户体验（UX）设计师和研究人员通常会进行各种实验，收集有关设计选择和/或用户行为的数据。使用 Python 的数据科学技术可以帮助分析这些数据。

Neeraj 关于鞋店的案例研究完美地展示了这些协同作用。他演示了如何使用 k-means 聚类来个性化推荐、促销和内容，以及如何使用自然语言处理（NLP）技术如情感分析从客户反馈中识别用户痛点。

点击流分析用于优化网站导航和提高用户参与度，而市场篮分析帮助调整商店布局，以便根据经常购买的商品进行调整。Neeraj 还展示了如何使用 NLP 技术自动化用户研究调查结果，以及如何使用统计技术如假设检验来评估 A/B 测试结果的显著性。

![](img/d05ae53e776102f1e8a4e508c158823b.png)

图片由 [Firmbee.com](https://unsplash.com/@firmbee?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

总体而言，他的讲座突出了数据科学如何帮助改善 UX 并推动业务成功。尽管他讨论的技术对我们来说并不新鲜，但我们欣赏他清晰而引人入胜的演讲风格以及精美制作的幻灯片。

我们之前已经与公司 UX 部门分享了一些这些技术，但我们相信 Neeraj 的案例研究将进一步强化我们一直试图传达的观点。我们很高兴与 UX 的同事分享他的演讲。

## **结论**

我们在 PyCon 上进行的第二次工作坊非常愉快，并收到了有价值的反馈，这将帮助我们改进。我们很高兴将这些反馈融入到工作坊的扩展版本中，在这个版本中我们将教授我们的数据科学流程，帮助你提高项目的成功率（更多内容请见 [这里](https://www.datasciencerebalanced.com/)）。

虽然我们觉得今年的数据科学讲座没有去年那么多，但我们仍然在面向一般软件开发的会议中发现了价值。

如果你是一名数据科学家，正在寻找一个 Python 会议参加，我们推荐这个会议，如果你有兴趣成为一个更全面的程序员并学习更多关于软件开发的知识。无论你的技能水平如何，社区都很欢迎你，这也是一个扩展网络的绝佳机会。
