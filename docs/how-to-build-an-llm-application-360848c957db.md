# 如何构建 LLM 应用程序

> 原文：[`towardsdatascience.com/how-to-build-an-llm-application-360848c957db`](https://towardsdatascience.com/how-to-build-an-llm-application-360848c957db)

## 使用 Langchain 和 OpenAI 构建以 LLM 为中心的应用

[](https://johnadeojo.medium.com/?source=post_page-----360848c957db--------------------------------)![John Adeojo](https://johnadeojo.medium.com/?source=post_page-----360848c957db--------------------------------)[](https://towardsdatascience.com/?source=post_page-----360848c957db--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----360848c957db--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----360848c957db--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----360848c957db--------------------------------) ·5 分钟阅读·2023 年 7 月 21 日

--

![](img/5f840382aba59b8fb02a918285107a91.png)

图片来源：作者：使用 Midjourney 生成

# 以 LLM 为中心的应用

AI 创新的速度在短时间内取得了巨大的进展。特别是，两个创新为围绕大型语言模型（LLMs）构建应用程序开辟了大量可能性：[函数调用](https://openai.com/blog/function-calling-and-other-api-updates)和[代理](https://python.langchain.com/docs/modules/agents/)。

在本文中，我演示了如何利用函数调用和代理在航班数据库上执行搜索，使你能够找到便宜的航班、短途航班、长途航班或任何符合你偏好的航班。

请注意——至少，你需要以下内容才能使其为你工作：

+   一个[OpenAI API 密钥](https://www.howtogeek.com/885918/how-to-get-an-openai-api-key/)——用于访问大型语言模型。

+   一个[Amadeus API 密钥](https://developers.amadeus.com/)——用于访问航班数据。

现在，让我们**深入**探讨技术细节。

# 自主代理链

[Langchain](https://python.langchain.com/docs/get_started/introduction.html)一直处于 LLM 驱动的代理的前沿。这是一个简单但强大的概念。

从本质上讲，你可以赋予一个代理**推理**能力，在我们的案例中，这将是 GPT-4。

你可以赋予代理访问各种工具的权限。这些工具可以包括搜索引擎、pandas、SQL、Wolfram Alpha 等。这个列表每个月都在扩展，开发者们不断添加更多工具。

由大型语言模型驱动的代理利用分析推理来确定如何利用工具完成你分配的任务。

# 函数调用

OpenAI 的一个开发，函数调用允许你从自然语言输入中解析函数的参数。

这对用户如何使用自然语言或甚至语音与我们的应用程序交互具有重要意义。

函数调用将在后面的代码示例中变得更加清晰。

# 构建航班搜索应用

我们可以开发一个应用程序，通过自然语言查询航班，只需四个组件，前端除外。

作者提供的 GIF：基于查询的航班搜索 LLM 应用演示 — “请给我提供从伦敦到希腊的航班，出发日期为 2023 年 11 月 15 日，返回日期为 2023 年 12 月 10 日。适用于一位成人。”

+   **航班数据 API**：航班数据来自 Amadeus 的航班 API，包含有关航班的所有相关细节。

+   **函数调用**：选择的大型语言模型（LLM），在此应用程序中为 OpenAI 的 GPT-4。

+   **SQL 数据库**：将数据存储在内存中的 SQL 数据库中。

+   **SQL 数据库代理**：使用 LLM 和 SQL Alchemy。LLM 从自然语言输入生成 SQL 查询。SQL Alchemy 执行查询。

总体架构大致如下：

![](img/2e8a1fb8ce415be5fd8ce86a683d672c.png)

作者提供的图像：航班应用流程图

# OpenAI 函数调用脚本

函数调用使我们能够从自然语言输入中提取结构化数据。这对于航班搜索应用来说是完美的，因为我们需要提取关于航班的关键信息，以便请求 Amadeus 航班 API 的数据：

作者提供的脚本：函数调用以获取参数从航班 API 提取数据

# Amadeus API 请求脚本

接下来，我们向 Amadeus 发送请求以获取航班预订数据。他们提供了免费层，但你需要注册以获得你的 API 密钥。

我编写了一个脚本来 [转换](https://gist.github.com/john-adeojo/fcd8a286ce2c3c97c22d4c8b33456cf0) 数据，使其适合 SQL 查询。这对于应用程序的性能至关重要。注意函数 *load_data()* 在下一节中讲解。

作者提供的脚本：从 Amadeus API 请求航班数据，将其转换并保存到内存中的 SQL 数据库

# 创建数据库脚本

一旦我们有了数据，我们需要创建一个 SQL 数据库并将数据存储在其中，以便我们的代理可以查询它。

作者提供的脚本：将数据存储在内存中的 SQL lite 数据库中

# SQL 数据库代理脚本

我们可以调用 SQL 数据库代理来根据请求查询存储的航班数据库。该过程遵循以下一般工作流程：

+   第 1 步 — 将用户的自然语言查询与提示模板结合起来。

+   第 2 步 — 将组合查询和提示发送到 SQL 数据库代理

+   第 3 步 — SQL 数据库代理使用 [ReAct](https://python.langchain.com/docs/modules/agents/agent_types/react.html) 框架生成正确的查询以响应用户请求。

这是脚本：

作者提供的脚本：SQL 数据库代理

这是提示模板：

作者提供的脚本：生成查询的提示模板，以供 SQL 数据库代理使用。

在继续之前，我应该简要介绍一下 ReAct 框架。

本质上，ReAct 是一个用于提示 LLM 的框架，混合了思考和行动。它帮助模型制定计划并调整计划，同时允许模型与数据库或环境等外部资源互动，以获取更多信息。

这种方法已经在不同任务中进行了测试，显示出更好的结果，并且更容易为人们理解和信任。有关 ReAct 的更多细节，请阅读这个 [资源](https://react-lm.github.io/)。

# 综合考虑

该应用程序总共包括六个脚本，包括使用 [Streamlit](https://streamlit.io/) 开发的前端。设置说明和完整的项目可以在我的 [GitHub 仓库](https://github.com/john-adeojo/ai_travel_agent/tree/main) 中找到。

我有一个完整的操作指南，展示了如何从我的 GitHub 仓库克隆应用程序并在你自己的机器上运行。

我已经通过我的 [YouTube](https://youtu.be/ycAgX_Oe1Q4) 频道录制了现场演示。你可以看到应用程序的实际运行情况，窥探其内部，并了解如何在你自己的机器上进行设置。

# 最终思考

函数调用和 SQL 数据库代理非常强大。我设想它们会被用来构建许多以 LLM 为中心的应用程序。然而，我想提出一些警示。

+   **提示工程：** 这在应用程序的性能中发挥了重要作用。对提示工程的研究已经推出了像 ReAct 这样的框架。然而，仍然需要大量的工程工作来提供合理的响应。尤其是提示模板，很难做到完美。

+   **延迟：** 该应用程序从头到尾运行需要很长时间，这肯定不是我们通常习惯的即时反馈。性能较差的模型，如 GPT-3.5，将运行更快，但牺牲了回答更复杂问题的能力。从客户体验的角度来看，良好的前端和加载条可以在一定程度上缓解延迟问题。从长远来看，我们期望看到更快且性能更好的模型。

+   **幻觉：** LLM 有出现幻觉的倾向，可能会时不时返回错误结果。提示工程框架旨在减少幻觉率，但仍需进一步工作。我预计更先进的模型在这方面会减少幻觉问题，就像我们看到 GPT-4 的幻觉率低于 GPT-3.5 一样。

+   **数据整理：** 使用 SQL 数据库代理可以很好地工作，如果数据也经过良好的整理。我花了大部分时间来决定数据的正确逻辑表示，以确保 LLM 能够正确查询它。

+   **LLM 限制：** 应用程序中存在几个可以通过引入更多工具来解决的错误。一个主要的例子涉及日期。偶尔，LLM 无法推断出日期，如果年份和月份没有明确说明的话。通过代理向 LLM 提供 Python 访问权限可能有助于解决这个问题。

我欢迎对原型的反馈以及任何改进建议。

感谢阅读。

如果你渴望提升人工智能技能，可以加入[我的课程](https://www.data-centric-solutions.com/course)的等待名单，我将带领你深入了解开发大型语言模型驱动的应用程序的过程。

如果你寻求业务的 AI 转型，今天就预约一次发现通话吧。

[## Brainqub3 | AI 软件开发](https://www.brainqub3.com/?source=post_page-----360848c957db--------------------------------)

### 在 Brainqub3，我们开发定制的 AI 软件。我们使用最新的 AI 技术创建 qub3s，先进的人工智能大脑，来…

[www.brainqub3.com](https://www.brainqub3.com/?source=post_page-----360848c957db--------------------------------)

想要了解更多关于人工智能、数据科学和大型语言模型的内容，你可以订阅[YouTube](https://www.youtube.com/channel/UCkXe-exqi25V4GnZendgEaA)频道。
