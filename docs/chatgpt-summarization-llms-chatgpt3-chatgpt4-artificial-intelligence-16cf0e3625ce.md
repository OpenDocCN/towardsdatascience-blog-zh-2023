# 掌握 ChatGPT：使用 LLM 进行有效的摘要生成

> 原文：[`towardsdatascience.com/chatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce`](https://towardsdatascience.com/chatgpt-summarization-llms-chatgpt3-chatgpt4-artificial-intelligence-16cf0e3625ce)

## 如何提示 ChatGPT 以获得高质量的摘要

[](https://medium.com/@andvalenzuela?source=post_page-----16cf0e3625ce--------------------------------)![Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----16cf0e3625ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16cf0e3625ce--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----16cf0e3625ce--------------------------------) [Andrea Valenzuela](https://medium.com/@andvalenzuela?source=post_page-----16cf0e3625ce--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----16cf0e3625ce--------------------------------) ·10 分钟阅读·2023 年 5 月 22 日

--

![](img/6c875157182b9485516e809510ba0514.png)

自制 gif。

你是否是那种每次去新餐厅时都会在 Google Maps 上留下评论的人？

或许你是那种在 Amazon 购买时会分享意见的人，特别是当你对低质量产品感到愤怒时？

*别担心，我不会责怪你——我们都有这样的时刻！*

在当今的数据世界中，我们以多种方式贡献于数据泛滥。我觉得特别有趣的一种数据类型是文本数据，如每天在互联网上发布的大量评论。*你是否曾经停下来考虑过标准化和压缩文本数据的重要性？* **欢迎来到摘要生成代理的世界！**

![](img/b1300eb548d9ed867e48cea796690892.png)

AI 图像生成工具 Dall-E 想象中的摘要生成代理。

摘要生成代理无缝地融入了我们的日常生活，压缩信息并在各种应用程序和平台中提供快速访问相关内容。

在本文中，我们将探讨如何利用 ChatGPT 作为强大的摘要生成代理，服务于我们的自定义应用程序。由于大型语言模型（LLM）处理和理解文本的能力，**它们可以帮助阅读文本并生成准确的摘要或标准化信息**。然而，了解如何挖掘它们在这项任务中的潜力，以及认识到它们的局限性，是非常重要的。

*摘要生成的最大限制是什么？*

**大型语言模型在遵守特定字符或单词限制方面经常显得不足**。

**让我们探索使用 ChatGPT 生成总结的最佳实践**，以及其局限性的原因和如何克服这些限制！

# 使用 ChatGPT 进行有效总结

总结代理在互联网上广泛使用。例如，网站使用总结代理提供文章的简洁总结，使用户能够快速了解新闻而无需深入阅读整个内容。社交媒体平台和搜索引擎也会这样做。

**从新闻聚合器和社交媒体平台到电子商务网站，总结代理已成为我们数字环境的重要组成部分**。随着大型语言模型（LLMs）的兴起，这些代理中的一些现在使用人工智能来获得更有效的总结结果。

ChatGPT 在使用总结代理构建应用程序以加速阅读任务和分类文本时可以成为一个好的助手。例如，假设我们有一个电子商务网站，我们希望处理所有客户评论。**ChatGPT 可以帮助我们将任何给定的评论总结成几句话，将其标准化为通用格式，确定评论的情感，并相应地分类**。

虽然我们确实可以简单地将评论提供给 ChatGPT，但为了发挥 ChatGPT 在这个具体任务中的威力，有一系列的最佳实践*— 以及需要避免的事项 —*。

**让我们通过这个示例来探索一下选项吧！**

# 示例：电子商务评论

![](img/086702ae2103f86e931e36ff8ec4a39f.png)

自制 gif。

考虑上述示例，我们希望处理电子商务网站上给定产品的所有评论。我们会对处理以下关于我们明星产品的评论感兴趣：**孩子们的第一台电脑！**

在这种情况下，我们希望 ChatGPT：

+   将评论分类为积极或消极。

+   提供一个 20 个字的评论总结。

+   以具体结构输出响应，以将所有评论标准化为一种格式。

## 实现说明

这是我们可以用来从自定义应用程序提示 ChatGPT 的基本代码结构。我还提供了一个链接到 [Jupyter Notebook](https://github.com/for-code-sake/chatgpt/blob/main/summarization-with-llms/summarization-examples.ipynb) 的链接，其中包含了本文中使用的所有示例。

函数 `get_completion()` 调用 ChatGPT API，并带有给定的*提示*。如果提示包含额外的*用户文本*，例如在我们的案例中是评论本身，它会通过三引号与其余代码分开。

**让我们使用** `**get_completion()**` **函数来提示 ChatGPT！**

这是一个满足上述要求的提示：

> ⚠️ 在这个例子中使用的提示指南，例如使用分隔符将输入文本与其余提示分开，以及请求结构化输出，在[**我从 OpenAI 的提示工程课程中学到的 — 提示指南**](https://medium.com/geekculture/prompt-engineering-prompting-guidelines-chatgpt-chatgpt3-chatgpt4-artificial-intelligence-6b74f35d2695)**中有详细解释**。

这是 ChatGPT 的回答：

从输出结果中可以观察到，评论准确且结构良好，尽管**它遗漏了一些我们作为电子商务所有者可能感兴趣的信息**，例如关于产品交付的信息。

## 聚焦于 <运输和交付> 的总结

**我们可以通过迭代改进我们的提示，要求 ChatGPT 在总结中包含一些焦点**。在这种情况下，我们对关于运输和交付的任何细节感兴趣：

这一次，ChatGPT 的回答如下：

现在评论更加完整。**提供关于原始评论的重要焦点的细节对于避免 ChatGPT 跳过可能对我们使用案例有价值的信息至关重要**。

*你是否注意到，尽管这第二次尝试包含了关于交付的信息，但它跳过了原始评论中唯一的负面方面？*

**让我们解决这个问题！**

# “提取”而不是“总结”

通过调查总结任务，我发现**如果用户提示不够准确，总结可能是 LLM 的一个棘手任务**。

当请求 ChatGPT 提供给定文本的总结时，它可能会跳过对我们*— 正如我们最近经历过的 —* 可能相关的信息，或者它会对文本中的所有主题给予相同的重要性，仅提供主要点的概述。

在使用 LLM 进行此类任务时，专家们使用*提取*和附加关注点的信息，而不是*总结*。

**总结旨在提供文本主要点的简明概述，包括与焦点主题无关的主题，而信息提取则专注于检索具体细节**，可以为我们提供我们确切寻找的内容。让我们尝试一下提取吧！

在这种情况下，通过提取，我们只获取关于我们关注主题的信息：`Shipping: Arrived a day earlier than expected.`

# 自动化

这个系统适用于单一评论。然而，在为具体应用设计提示时，**重要的是在一批示例中测试它，以便我们可以发现模型中的任何异常或不良行为**。

如果处理多个评论，这里有一个示例 Python 代码结构可以帮助你。

这是我们评论批次的总结：

> ⚠️ 请注意，尽管我们提示中的字数限制足够明确，但我们可以很容易地看到这个字数限制在任何迭代中都没有得到遵守。

这种词数不匹配的现象发生是因为**LLM 对词或字符数没有精确的理解**。其原因与其架构中的一个重要组件有关：**分词器**。

# 分词器

像 ChatGPT 这样的 LLM（大语言模型）旨在基于从大量语言数据中学到的统计模式生成文本。**虽然它们在生成流畅且连贯的文本方面非常有效，但它们缺乏对词数的精确控制**。

在上述例子中，当我们对词数提出非常精确的要求时，**ChatGPT 往往难以满足这些要求**。相反，它生成的文本实际上比指定的词数要短。

在其他情况下，它可能生成更长的文本，或者文本可能过于冗长或缺乏细节。此外，**ChatGPT 可能会优先考虑连贯性和相关性等因素，而不是严格遵循词数要求**。这可能导致生成的文本在内容和连贯性方面质量很高，但不完全符合词数要求。

**分词器是 ChatGPT 架构中的关键元素，明显影响生成输出的单词数量**。

![](img/eab3dee577752c4e18c4dc06019c4b5c.png)

自制 gif。

## 分词器架构

分词器是文本生成过程中的第一步。**它负责将我们输入给 ChatGPT 的文本分解成单独的元素 *— 词元 —***，这些词元随后被语言模型处理以生成新文本。

当分词器将一段文本拆分成词元时，它是基于一套旨在识别目标语言中有意义的单位的规则。然而，这些规则并不总是完美的，**可能会出现分词器以影响文本总体词数的方式拆分或合并词元的情况**。

例如，考虑以下句子：*“我想吃一份花生酱三明治”。* 如果分词器被配置为根据空格和标点符号来拆分词元，它可能会将这个句子拆分成以下词元，总词数为 8，与词元数相等。

![](img/b3c808ab0be23847a489e30ef868ef03.png)

自制图像。

然而，如果分词器被配置为将*“花生酱”*视为一个复合词，它可能会将句子拆分成以下词元，**总词数为 8，但词元数为 7**。

![](img/10d9eac60a0ece12a78885329a5adc1a.png)

**因此，分词器的配置方式会影响文本的总体词数**，这可能会影响 LLM 按照精确词数要求执行指令的能力。虽然一些分词器提供了自定义文本分词方式的选项，但这并不总是足以确保精确遵循词数要求。**对于 ChatGPT 而言，我们无法控制其架构的这一部分**。

这使得 ChatGPT 在完成字符或字数限制时表现不佳，**但可以尝试使用句子，因为分词器不会影响句子的数量，而是句子的长度**。

了解这一限制可以帮助你为你的应用程序构建最合适的提示。*了解 ChatGPT 如何处理字数，让我们对电子商务应用程序的提示做最后一次迭代！*

# 总结：电子商务评论

让我们将本文的学习成果结合起来形成一个最终提示！在这种情况下，我们将要求结果以`HTML`格式输出，以获得更好的效果：

这是 ChatGPT 的最终输出：

![](img/6f0192580ead353510a25e251dc3cdfe.png)

自制截图来自于[**Jupyter Notebook**](https://github.com/for-code-sake/chatgpt/blob/main/summarization-with-llms/summarization-examples.ipynb)，其中包含本文使用的示例。

# 摘要

在这篇文章中，**我们讨论了将 ChatGPT 作为总结代理用于自定义应用程序的最佳实践**。

我们已经看到，在构建应用程序时，第一次尝试时很难提出完全符合应用程序需求的完美提示。我认为一个很好的结论是**将提示视为一个迭代过程**，在这个过程中你不断完善和建模提示，直到获得完全期望的输出。

通过迭代地完善你的提示并在投入生产之前将其应用于一批示例，你可以确保**输出在多个示例中保持一致并涵盖异常响应**。在我们的例子中，可能会有人提供随机文本而不是评论。**我们可以指示 ChatGPT 也提供标准化的输出，以排除这些异常响应**。

此外，在使用 ChatGPT 执行特定任务时，了解使用 LLMs 进行目标任务的优缺点也是一个好习惯。这就是我们了解到***提取*任务在需要常见的人类类似总结时比*总结*任务更有效**。我们还了解到**提供总结的重点可以改变生成的内容**。

最终，虽然大型语言模型（LLMs）在生成文本方面可以非常有效，但**它们并不适合精确遵循字数或其他特定格式要求的指示**。为了实现这些目标，可能需要坚持句子计数或使用其他工具或方法，例如手动编辑或更专业的软件。

就这样！非常感谢阅读！

我希望这篇文章能帮助**在构建自定义应用程序时使用 ChatGPT！**

你还可以订阅我的[**新闻通讯**](https://medium.com/@andvalenzuela/subscribe)以关注新内容。**特别是**，**如果你对关于 ChatGPT 的文章感兴趣**：

[解锁 ChatGPT 的新维度：文本到语音集成](https://medium.com/geekculture/prompt-engineering-prompting-guidelines-chatgpt-chatgpt3-chatgpt4-artificial-intelligence-6b74f35d2695?source=post_page-----16cf0e3625ce--------------------------------)

### 增强 ChatGPT 互动中的用户体验

[我从 OpenAI 的提示工程课程中学到的—提示指南](https://medium.com/geekculture/prompt-engineering-prompting-guidelines-chatgpt-chatgpt3-chatgpt4-artificial-intelligence-6b74f35d2695?source=post_page-----16cf0e3625ce--------------------------------) 

### 了解 OpenAI 关于更好提示的指南

[ChatGPT 对你的了解：OpenAI 在数据隐私方面的历程](https://medium.com/geekculture/prompt-engineering-prompting-guidelines-chatgpt-chatgpt3-chatgpt4-artificial-intelligence-6b74f35d2695?source=post_page-----16cf0e3625ce--------------------------------) 

### 管理 ChatGPT 中个人数据的新方法

[通过提示工程提升 ChatGPT 性能](https://medium.com/geekculture/openai-fine-tuning-custom-chatgpt-transfer-learning-prompt-data-science-machine-learning-chatgpt3-chatgpt4-2aad7148438a?source=post_page-----16cf0e3625ce--------------------------------) 

### 如何向 ChatGPT 提问以最大化成功回答的机会

[创建自定义 ChatGPT 的迁移学习方法](https://levelup.gitconnected.com/improve-chatgpt-performance-prompt-engineering-data-science-artificial-intelligence-6fa3953bc5b6?source=post_page-----16cf0e3625ce--------------------------------) 

### 提升 ChatGPT 能力，微调你自己的模型

[ChatGPT 与 AI 检测器——你会押注哪一方！](https://pub.towardsai.net/chatgpt-vs-artificial-intelligence-detectors-chatgpt4-chatgpt3-openai-technology-2c46e6acf6b?source=post_page-----16cf0e3625ce--------------------------------) 

### 测试互联网中最受欢迎的 AI 检测器

[pub.towardsai.net](https://pub.towardsai.net/chatgpt-vs-artificial-intelligence-detectors-chatgpt4-chatgpt3-openai-technology-2c46e6acf6b?source=post_page-----16cf0e3625ce--------------------------------)

如果你有任何问题，**随时可以转发**到 *forcodesake.hello@gmail.com* :)
