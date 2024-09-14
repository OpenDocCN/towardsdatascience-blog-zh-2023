# 使用大语言模型制作有效总结提示

> 原文：[`towardsdatascience.com/crafting-effective-prompts-for-summarization-using-large-language-models-dbbdf019f664`](https://towardsdatascience.com/crafting-effective-prompts-for-summarization-using-large-language-models-dbbdf019f664)

## 人工智能和大语言模型

## 在拥有超过 2 年的经验以及来自 AI 开发者自身的教程、实践和示例之后提炼出的关键点。

[](https://lucianosphere.medium.com/?source=post_page-----dbbdf019f664--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----dbbdf019f664--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dbbdf019f664--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbbdf019f664--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----dbbdf019f664--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----dbbdf019f664--------------------------------) ·8 分钟阅读·2023 年 10 月 24 日

--

![](img/5d9c95adc4b403b4cdbf2d4155e8083a.png)

图片由[Emiliano Vittoriosi](https://unsplash.com/@emilianovittoriosi?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在信息丰富的时代——想想每天发布的大量文章，会议和演讲中消耗的时间，或你在工作中收到的电子邮件数量——将复杂内容提炼成简洁、富有洞察力的摘要的能力是无价的。总结工具已经存在了一段时间，但最近大语言模型的出现，如 GPT、Bard 或 LLaMa 系列中的一些最受欢迎的模型，已经将总结提升到了新的高度。这是因为大语言模型不是作为固定规则的总结器工作，而是可以“理解”输入文本的内容，并以比不使用语言模型的工具更大的灵活性生成简明的摘要。

特别是，传递正确的提示给语言模型不仅可以实现普通的总结，还可以按照特定风格进行重新措辞、在被动和主动形式之间转换、更改叙述视角，甚至理解包含不同语言或语调的文本，并用单一语调和语言写出一致的摘要。这些都是不具备大语言模型支持的总结工具所无法实现的，这些工具迅速变得相当过时。

从我上述的陈述来看，显然，发挥大型语言模型在总结中的全部潜力的关键在于制定有效引导它们的提示。在这里，我将概述编写期望生成高质量摘要的提示时必须考虑的最重要的要点。

# **提示的力量**

提示作为人类意图与人工智能执行之间的接口。它们提供了语言模型生成符合用户需求的摘要所需的指令和背景。这意味着提示必须是完整的，包括对输出的明确指导和要求，以及提供背景的相关信息。你将通过我的文章看到这些要点的示例。

有效的提示具有共同的特征，使其成为提取有意义摘要的强大工具。它们清晰、具体，并针对任务的内容和背景量身定制。理想的提示不留模糊空间。它准确表达了用户希望从内容中提取的内容。例如，与其要求“总结这篇文章”，更有效的提示应该是：

*生成以下文章关键发现和论点的逐点总结。*

上下文在生成相关摘要中至关重要。提示应提供有关来源材料、目标受众和期望细节水平的背景信息。这确保语言模型“理解”内容中的细微差别，从而生成符合用户目标的摘要。例如，你可以在提示中包括解释待总结的文本是会议备忘录、科学文章、某主题的新闻汇编、播客、电子邮件或你期望的输出示例的信息。我已经编写了几种程序，利用 GPT-3.5 或 GPT-4 在这些情况下生成总结，并且我总能看到提示中传递的背景影响很大，尤其是在开头部分。例如，请参见这两篇文章：

[](/control-web-apps-via-natural-language-by-casting-speech-to-commands-with-gpt-3-113177f4eab1?source=post_page-----dbbdf019f664--------------------------------) ## 通过将语音转换为命令来控制网页应用程序，使用 GPT-3

### 最后一篇文章展示了 GPT3 的实际应用，详细解释了工作流程和细节……

towardsdatascience.com [](/coupling-gpt-3-with-speech-recognition-and-synthesis-to-achieve-a-fully-talking-chatbot-that-runs-abfcb7bf580?source=post_page-----dbbdf019f664--------------------------------) ## 将 GPT-3 与语音识别和合成结合，实现完全对话的聊天机器人…

### 我是如何创建这个网络应用程序的，你可以用它与 GPT-3 自然对话，讨论你想要的任何主题，全程基于网络…

towardsdatascience.com

根据任务，提示也可以指定总结的期望抽象层次。例如，可以请求高级概述、深入分析、关键点的简要综合或关键点的列表等等。提示还可以调整输出的学术层次；例如，可以要求语言模型总结一篇“保持科学严谨”的硬核科学文章，或“为 10 岁儿童编写”，或解释一段代码“作为伪代码”或“仅解释其基本原理”。当然，请求的抽象层次和学术层次应与总结的目的相一致。

## 案例研究：我的 Summarizer Almighty 聊天机器人

我开发了一个名为“Summarizer Almighty”的网络应用程序，以同时解决两个问题：总结长度过长的文本以适配语言模型输入，并关注用户提供的特定问题或指示——而不是一般总结。

正如我最初所写，Summarizer Almighty 利用 GPT-3.5-turbo 语言模型来分析和提取基于用户提供的问题或指示的长文档中的相关信息。我刚刚将其更新到 GPT-4，虽然运行成本更高，但在许多情况下提供了更好的响应。

为了完成工作，Summarizer Almighty 将输入文档拆分为重叠的块，这些块通过这种特殊提示进行单独分析和总结：

*你能根据以下文章中的文本回复“[用户的问题]”吗？如果不能，请回复‘FALSE’而不做其他回复。如果可以，请回复‘TRUE’，并附上答案。以下是段落：“[从文档中提取的文本块，优化到大约 3000 个 tokens]”*

随后将输入发送到语言模型的 API，Summarizer Almighty 将部分答案积累成一个不断增长的段落，其中包含多个“FALSE”输出和一些“TRUE”输出，后者附有解释或答案。最后，程序显示所有部分输出，然后再次调用语言模型，这次使用注入的提示：

*请回答“[用户的问题]”使用以下提供的信息，这些信息来自较长文本的各个部分：[所有返回 TRUE 的调用所提供的答案的连接]。*

免费使用（但你必须提供有效的 OpenAI API 以使用 GPT 模型），我认为 Summarizer Almighty 在研究、新闻写作、教育、商业以及其他时间有限且详细信息至关重要的情况下非常有价值，能够节省时间和精力。欲了解更多，请查看我专门撰写的文章：

[## 使用语言模型工程提示链来打造“终极摘要器”Web 应用](https://pub.towardsai.net/engineering-prompt-chains-with-language-models-to-craft-a-summarizer-almighty-web-app-7286de0b0a71?source=post_page-----dbbdf019f664--------------------------------)

### 这个免费工具可以分析任意长度的文档，寻找有关问题或指令的信息……

[pub.towardsai.net](https://pub.towardsai.net/engineering-prompt-chains-with-language-models-to-craft-a-summarizer-almighty-web-app-7286de0b0a71?source=post_page-----dbbdf019f664--------------------------------)

# **来自 OpenAI 关于如何制定更好提示的资源**

OpenAI 的大型语言模型可能是当前市场上最成功的模型。客观来说，它们是第一个真正优秀的模型。此外，在我看来，它们在实践中是最容易使用的——仅通过 API 调用，无需安装。而且文档也最全面……

确实，OpenAI 的网站上有许多资源旨在帮助你编写更好的提示。这不仅涉及到提示中传递的一般规则和格式，还涉及到使用特殊的标记和指令。例如，专门讲解 OpenAI API 提示工程最佳实践的页面特别说明了指令应该尽量在开头传递（正如我在本文中根据自己的经验也解释过的那样），你可以使用像三重引号”””或井号###来将指令与上下文分开，提供引导词和期望输出的示例是非常有帮助的，还有更多：

[## 使用 OpenAI API 进行提示工程的最佳实践 | OpenAI 帮助中心](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api?source=post_page-----dbbdf019f664--------------------------------)

### 如何向 GPT-3 和 Codex 提供清晰有效的指令

[help.openai.com](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api?source=post_page-----dbbdf019f664--------------------------------)

OpenAI 的社区论坛还包含一个专门设计的部分，用于提问和回答关于提示工程的问题：

[## 提示工程](https://community.openai.com/c/prompting/8?source=post_page-----dbbdf019f664--------------------------------) 

### 通过分享最佳实践、你喜欢的提示等来了解更多关于提示的内容！

[community.openai.com](https://community.openai.com/c/prompting/8?source=post_page-----dbbdf019f664--------------------------------)

# **迭代优化**

在我丰富的经验中，主要是与 Google 的 Bard 和 GPT 家族的语言模型的经验中，我观察到一个重要的点是，往往迭代优化提示是至关重要的。你可能需要尝试不同的措辞、指令或参数，以优化生成摘要的质量。很多时候，第一次输出并不是我想要的，特别是在涉及总结时，因为语言模型不能神奇地理解你的确切意图、你希望关注的重点、你期望的输出语气以及其他细节，如上所述，你必须通过提示将这些注入模型。当这种情况发生时，大多数时候，我需要反复与语言模型迭代几次，直到我得到预期的结果，或者尝试不同的起始提示，直到我找到模型提供我所期待的输出。

有时你可以获得一个初步生成的结果，并将其与一个新的旨在改进输出的提示一起反馈给模型；但有时你需要从头开始，输入一个全新的提示。在少数情况下，检查语言模型的多个输出或在相同提示上重新运行可以得到更好的生成结果，但这种情况往往并不常见。

一个有助于处理某些方面的工具，特别是关于程序化使用 GPT 模型时输出质量的工具，是关注每个生成令牌相关的概率，就像我在这里为 GPT-3 所描述的那样：

## 探索令牌概率作为过滤 GPT-3 回答的一种手段

### 为了构建更好的 GPT-3 驱动的聊天机器人

towardsdatascience.com

# **结论**

有效的提示是释放大型语言模型总结能力的关键，确保生成的摘要相关、简明、深刻、完全符合你的需求，并以你所需的方式书写。精心设计的提示可以产生更精确和相关的摘要，确保计算机程序专注于提取对你有用的信息。清晰的提示减少了大量后处理的需要，节省了宝贵的时间。

掌握 - 如许多人所称的，我也不反对 - 提示设计的“艺术”将是，或者说现在已经是，现代工作中的一项重要技能。无论是从文章、研究论文、会议或其他任何内容中提取见解，恰当的提示可以指导你的语言模型生成有意义且可操作的摘要。将我的建议付诸实践，亲自体验一下。

# 你可能喜欢的一些相关文章

[我构建的运行在网页上的 GPT 驱动工具（聊天机器人、摘要、翻译器、电子邮件写作工具）](https://medium.com/ido-imake/i-build-gpt-powered-tools-that-run-on-the-web-chatbots-summarizes-translators-e-mail-writers-78e303210cb?source=post_page-----dbbdf019f664--------------------------------)

### 查看我的示例，你可以自由联系我获取工作机会！

[我在 2022 年 10 月的所有关于 GPT-3 的文章](https://medium.com/ido-imake/i-build-gpt-powered-tools-that-run-on-the-web-chatbots-summarizes-translators-e-mail-writers-78e303210cb?source=post_page-----dbbdf019f664--------------------------------)

### 我最喜欢的语言模型及其如何在在线应用中利用纯 JavaScript 进行多种用途的使用方式…

[我在 2022 年 10 月关于 GPT-3 的所有文章](https://lucianosphere.medium.com/all-my-articles-on-gpt-3-as-of-october-2022-10e95dcae199?source=post_page-----dbbdf019f664--------------------------------) [我写关于科学和技术的文章 - 第一部分：我的数据科学、人工智能的主要内容](https://medium.com/ido-imake/i-write-about-science-and-technology-part-1-my-main-content-on-data-science-artificial-5419e58ed7d1?source=post_page-----dbbdf019f664--------------------------------)

### 最近，我开始收到关于科学和技术写作的工作请求，其中一位客户专注于生物学…

[我写关于科学和技术的文章 - 第一部分：我的数据科学、人工智能的主要内容](https://medium.com/ido-imake/i-write-about-science-and-technology-part-1-my-main-content-on-data-science-artificial-5419e58ed7d1?source=post_page-----dbbdf019f664--------------------------------)

[***www.lucianoabriata.com***](https://www.lucianoabriata.com/) *我写的内容涉及我广泛的兴趣领域：自然、科学、技术、编程等。* [***订阅以获取我的新故事***](https://lucianosphere.medium.com/subscribe) ***通过电子邮件****。要* ***咨询小型工作*** *请查看我的* [***服务页面***](https://lucianoabriata.altervista.org/services/index.html)*。你可以* [***在这里联系我***](https://lucianoabriata.altervista.org/office/contact.html)***。***
