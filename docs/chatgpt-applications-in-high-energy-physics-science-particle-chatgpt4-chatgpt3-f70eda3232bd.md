# 推动边界：ChatGPT 在粒子物理中的应用

> 原文：[`towardsdatascience.com/chatgpt-applications-in-high-energy-physics-science-particle-chatgpt4-chatgpt3-f70eda3232bd`](https://towardsdatascience.com/chatgpt-applications-in-high-energy-physics-science-particle-chatgpt4-chatgpt3-f70eda3232bd)

## 探索 ChatGPT 在研究中的无限潜力

[](https://medium.com/@andvalenzuela?source=post_page-----f70eda3232bd--------------------------------)![Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----f70eda3232bd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f70eda3232bd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f70eda3232bd--------------------------------) [Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----f70eda3232bd--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f70eda3232bd--------------------------------) ·阅读时间 6 分钟·2023 年 6 月 8 日

--

![](img/b159929dfd45f5861f25debc065a8bcf.png)

[[Source](https://cms.cern/news/wait-overthe-lhc-run-3-has-started)] 在瑞士 CERN 的大型强子对撞机上的 CMS 探测器中的粒子碰撞。

上个月举行了对高能物理（HEP）社区至关重要的计算机会议之一：**所谓的** [**CHEP 2023**](https://www.jlab.org/conference/CHEP2023)，代表**C**omputing on **H**igh **E**nergy **P**hysics 和 Nuclear Physics — *是的，非常简单！* :)

作为一名在[CERN](https://home.cern/)工作的计算机工程师，这是一项重大事件：这是**观察我们领域最新技术趋势的机会**。尽管我完全了解 ChatGPT 目前的流行程度，但我没有预料到会有相关的讨论。但我完全错了，*确实有几个！*

我发现这些讲座非常吸引人，因此在这篇文章中，**我想描述这些讲座的主要要点**。ChatGPT 不仅在重塑我们的日常任务，还在影响像 HEP 这样的主要研究领域。

**让我们探索一下未来的发展吧！**

# HEP 和 CHEP — 简短介绍

HEP 社区指的是全球范围内参与高能物理领域的科学家、研究人员、工程师、技术员和机构的网络。这个社区致力于研究**物质的基本组成部分**、**支配它们相互作用的力量**，以及探索**宇宙的基本法则**。

CHEP 是一系列会议，重点关注计算、软件和数据管理在 HEP 领域中的应用 *— 还有核物理领域*。

![](img/07955bc902ec8e8a89c0331a5ccf807c.png)

[[Source](https://www.jlab.org/conference/CHEP2023)] CHEP 2023 的会议图片。

实际上，CHEP 是一个相当古老的会议。**第一次会议是在 1985 年举行的，从那时起，它每两年组织一次**。总体而言，CHEP 会议在推动计算和数据管理的进步方面发挥了至关重要的作用。

CHEP 作为知识交流、合作和新计算技术探索的平台，因此我感到非常惊讶：**如果 CHEP 上出现了什么东西，它很可能会成为一个新趋势！** 在最近的 CHEP 2023 中，我们有两次关于 ChatGPT 在 HEP 中的全体会议。

*准备好了吗？*

# ChatGPT 能做科学吗？

关于 ChatGPT 的第一次全体会议由来自[杰斐逊实验室](https://www.jlab.org/)的*David Dean*主持，时间安排得非常早。题为[*计算的演变与革命：前沿科学*](https://indico.jlab.org/event/459/contributions/12489/)，David 提供了计算领域最新革命的广泛概述。**不用担心，ChatGPT 就是其中之一！**

他具体探讨了*ChatGPT 是否能做物理*的问题，信息很明确：这是一个令人震惊的工具，它也能通过物理考试，但**有一个重大缺陷可能会阻止 ChatGPT 在不久的将来被作为工具纳入使用：模型*幻觉***。

![](img/9fbc27e893c19b306696abd3604ecbe4.png)

[[来源](https://arxiv.org/pdf/2303.08774.pdf)] 原始 ChatGPT-4 技术报告的截图。ChatGPT 解决物理考试的分数以黄色高亮显示。

## 模型**幻觉**

尽管模型能够生成类似人类的回答，但有时它仍会倾向于编造事实、坚持错误信息，并错误地执行任务。**这些错误的回答被称为*幻觉***。

事实上，给出错误答案本身并不是问题。主要问题是 ChatGPT 经常以令人信服和权威的方式表现出这些倾向。**幻觉有时甚至表现为高度详细的信息，给读者带来错误的准确感**，并增加了过度依赖的风险。这在研究界确实是一个问题。

为了将 ChatGPT 用作可靠的辅助工具，需要控制幻觉。目前，ChatGPT 会尝试对任何给定的问题提供答案，即使对目标主题没有足够的信息。

**ChatGPT 承认不能提供准确回应并没有什么坏处**，这会使该工具在如 HEP 研究等需要高准确性的环境中更为适用。

# ChatGPT 作为 HEP 的编码助手

第二次涉及 ChatGPT 的全体会议题为*AI/ML 启用的高能物理（HEP）未来的根本性变化*，由来自[威斯康星大学麦迪逊分校](https://www.wisc.edu/)的 Kyle Crammer 主讲。

**第二场演讲对 ChatGPT 作为 HEP 工具包中有价值资产的引入持更乐观态度**。事实上，Kyle 提到[另一场演讲](https://www.bnl.gov/p5meeting/)中的*Christian Weber*，来自[布鲁克海文国家实验室](https://www.bnl.gov/world/)，**他展示了 ChatGPT 作为编码助手的实际用例**，特别是在迁移和转换代码到新平台方面。事实上，ChatGPT 已经实现了用于[编码目的](https://arxiv.org/pdf/2107.03374.pdf)的 Python 解释器。

![](img/2b956e4d4e7c6feb1782170015b90837.png)

[[来源](https://openai.com/blog/chatgpt-plugins)] 截图来自官方 ChatGPT 文档。

HEP 社区中的每个实验都有自己特定的编码模板，*即使使用 Python 编程，科学家们也必须遵循一些类或风格规范*。**其中一个用例是微调 ChatGPT 以根据实验模板编写分析代码**。

被这个用例吸引，我尝试生成一个针对我当前实验的分析模板，即在瑞士 CERN 的[CMS 实验](https://home.cern/science/experiments/cms)，ChatGPT 完美生成了第一个模板。我只是使用了网络界面，**想象一下经过相关数据微调后的强大功能**。

![](img/2b23294c67e61d8364201bd9d8d5771f.png)

自制截图。使用 ChatGPT 生成 CMS 实验的 Python 分析模板。

根据演讲，即使有时分析不够准确，它也能生成一个初步的模板或骨架。这个想法被探索用于提供更快的上手培训给新实验成员，并更快地构建原型，等其他用例。

# 摘要

我们不能否认，大型语言模型（LLMs）如 ChatGPT 正在改变我们搜索信息、构建应用程序甚至编程的方式。

与任何技术进步一样，我认为评估任何新工具以利用其优势并将其应用于我们的主要领域是合理的。这两个全体会议只是像 HEP 这样的大型研究社区中这一评估过程的两个例子。

**虽然有些评估可能暂时抛弃 ChatGPT 作为研究助手，但其他评估可能允许在具体和有限的领域中纳入这样的工具**。无论如何，我相信不必惧怕 AI，并继续与其共同发展，分析其优势，了解如何优化其在目标领域的表现，更重要的是，意识到其缺陷以保持批判精神始终警觉！

就这些了！非常感谢阅读！***你能想到任何其他应用 ChatGPT 于研究领域的方法吗？***

你也可以订阅我的[**通讯**](https://medium.com/@andvalenzuela/subscribe)以便关注新内容。**特别是**，**如果你对 ChatGPT 相关文章感兴趣**：

[## ChatGPT 知道你的信息：OpenAI 在数据隐私方面的历程](https://www.towardsdatascience.com/what-chatgpt-knows-about-you-openai-towards-data-privacy-science-ai-b0fa2376a5f6?source=post_page-----f70eda3232bd--------------------------------)

### 管理 ChatGPT 中个人数据的新方法

[## ChatGPT 与 AI 检测器 — 下你的赌注！](https://pub.towardsai.net/chatgpt-vs-artificial-intelligence-detectors-chatgpt4-chatgpt3-openai-technology-2c46e6acf6b?source=post_page-----f70eda3232bd--------------------------------)

### 测试互联网上最受欢迎的 AI 检测器

[## 解锁 ChatGPT 的新维度：文本转语音集成](https://towardsdatascience.com/chatgpt-text-to-speech-artificial-intelligence-python-data-science-52456f51fad6?source=post_page-----f70eda3232bd--------------------------------)

### 提升 ChatGPT 互动中的用户体验

[## ChatGPT 文本转语音：人工智能与 Python 数据科学](https://pub.towardsai.net/chatgpt-text-to-speech-artificial-intelligence-python-data-science-52456f51fad6?source=post_page-----f70eda3232bd--------------------------------)

**如果有任何问题，随时向我提问**，你可以通过 *forcodesake.hello@gmail.com* 联系我 :)
