# 适应现有的 LLM 项目以使用 LangChain

> 原文：[`towardsdatascience.com/adapting-existing-llm-projects-to-use-langchain-cd07028c01b0`](https://towardsdatascience.com/adapting-existing-llm-projects-to-use-langchain-cd07028c01b0)

## 将 OpenAI 调用重构为更强大的 LangChain 功能

[](https://medium.com/@oaguy1?source=post_page-----cd07028c01b0--------------------------------)![Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----cd07028c01b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cd07028c01b0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd07028c01b0--------------------------------) [Lily Hughes-Robinson](https://medium.com/@oaguy1?source=post_page-----cd07028c01b0--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cd07028c01b0--------------------------------) ·6 分钟阅读·2023 年 7 月 1 日

--

![](img/814da28df7832cee3963ba401e7ba764.png)

照片由 [JJ Ying](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

恭喜你，你拥有一个令人自豪且准备向世界展示的 LLM 概念验证项目！也许你直接使用了 OpenAI 库，或者你使用了不同的基础模型和 HuggingFace transformers。无论哪种方式，你都付出了辛勤的努力，并在寻找下一步。那可能意味着代码重构、支持多个基础模型，或者添加更高级的功能，如代理或向量数据库。这时 LangChain 就派上用场了。

[LangChain](https://langchain.com) 是一个开源框架，用于处理 LLM 和在其基础上开发应用程序。它有 Python 和 JavaScript 版本，支持多个基础模型和 LLM 提供商。它还提供了许多实用函数，用于处理常见任务，如文档分块或与向量数据库交互。你是否相信这值得一试？看看这篇出色的文章，这是了解可能性的一个很好的起点：

[](/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c?source=post_page-----cd07028c01b0--------------------------------) ## 使用 LangChain 入门：构建 LLM 驱动应用程序的初学者指南

### 一个 LangChain 教程，用于在 Python 中构建基于大型语言模型的应用程序

towardsdatascience.com

本文将不关注绿色田野开发，而是专注于重构现有应用程序。它也会假设你对 LangChain 有一定的基础理解，但会提供相关文档的链接。具体来说，我将讨论重构我编写的项目[AdventureGPT](https://github.com/oaguy1/AdventureGPT)，这是一个用于玩 1977 年*巨大洞穴冒险*文字冒险游戏的自主代理。如果你对这个项目感兴趣，可以查看我之前关于它的文章：

[## AdventureGPT: 使用 LLM 支持的代理玩文字冒险游戏](https://betterprogramming.pub/adventuregpt-using-llm-backed-agents-to-play-text-based-adventure-games-be52f243866a?source=post_page-----cd07028c01b0--------------------------------)

[betterprogramming.pub](https://betterprogramming.pub/adventuregpt-using-llm-backed-agents-to-play-text-based-adventure-games-be52f243866a?source=post_page-----cd07028c01b0--------------------------------)

我最感兴趣的几个重构领域：

1.  使用[Chains](https://python.langchain.com/en/latest/modules/chains.html)而不是直接的 OpenAI 调用

1.  替换自定义文档工具以处理 LangChain [Document/Data](https://python.langchain.com/docs/modules/data_connection/)

这些问题将依次解决。

让我们首先了解什么是链。链是一种将多个提示处理技术组合成一个单元的方法，结果是一次基础模型调用。一旦有了有效的链，可以将链组合在一起，使用它们来完成更复杂的任务。

LangChain 提供了几种不同类型的链，本篇文章专注于 LLMChain 和 ConversationChain。LLMChain 是最简单的链类型，将提示模板与 LangChain 支持的 LLM 对象结合在一起。ConversationChains 则提供了定制化的对话工作流体验，例如聊天机器人。ConversationChain 的一个主要特性是能够包括记忆，并将对话的过去部分轻松存储到提示中。

提示模板是 LangChain 最强大的功能之一，允许你在提示中包含变量。在手动完成此任务时，可以使用 f-strings 结合字符串连接和[自定义 __repr__ 方法](https://www.digitalocean.com/community/tutorials/python-str-repr-functions)来处理数据并将其插入到基础模型的提示中。使用提示模板，你可以通过用括号转义变量来格式化字符串。这就是你需要做的全部。

根据你创建的链的类型，一些变量会默认为你设置，例如对话历史或用户输入。这可能会涉及相当复杂的内容。在传统的对话提示中，有来自系统、AI 助手和用户或人类的消息。当手动编写提示时，你会使用“System”、“Human”和“AI”等标签来标记这些消息。LangChain 可以为你处理这些，使你可以使用 [ChatPromptTemplate](https://api.python.langchain.com/en/latest/modules/prompts.html#langchain.prompts.ChatPromptTemplate) 方法 `from_messages`，允许你将每条消息指定为对象列表，从而实现更高层次的抽象和自动历史记录包含及格式化。

所有这些强大功能都带来了复杂性的代价。与其仅仅用文本调整提示，还需要阅读详尽的文档，并可能扩展现有代码以适应特定的用例。例如，对话提示通常只包括用户的输入和对话历史作为变量。然而，我希望在我的提示中包含额外的游戏上下文，以供负责与游戏世界互动的 PlayerAgent 使用。在将额外变量添加到我的提示后，我遇到了以下错误：

```py
Got unexpected prompt input variables. The prompt expects ['completed_tasks', 'input', 'history', 'objective'], but got ['history'] as inputs from memory, and input as the normal input key. (type=value_error)
```

我做了一些调查，发现了一个现有的 [GitHub 问题](https://github.com/hwchase17/langchain/issues/891)，描述了我遇到的确切问题，但没有明确的解决方案。没有气馁，我查看了 [ConversationChain](https://github.com/hwchase17/langchain/blob/master/langchain/chains/conversation/base.py) 类的源代码，发现有一个特定的方法用于验证是否只传入了预期的变量。我创建了一个新类，继承了原始类，并重写了这个方法。拿到我的 CustomConversationChain 类后，我还需要指定 ConversationalMemoryBuffer 应该利用哪个变量来处理用户（或在我的案例中，游戏）的输入，因为有多个输入变量。通过 input_key 实例变量这一步很简单，结果一切顺利。

一旦我完成了将 OpenAI 调用转换为链的工作，就到了处理文档摄取方式的时候。作为游戏循环的一部分，我接受了一个 walkthrough 文本的路径，然后将其转换为由 PlayerAgent 完成的游戏任务。当我第一次添加这个功能时，我只是把整个 walkthrough 传递给提示，并希望能有所成效。随着我发现更复杂的 walkthrough，这种做法已不再可行，因为 walkthrough 的长度超出了 OpenAI 为 ChatGPT 允许的上下文窗口。因此，我将文本切分为 500 个标记的块，并多次运行提示，将 walkthrough 转换为游戏任务。

当我说我将文本按大约 500 个标记进行拆分时，我是非常粗略地使用 Python 的字符串 `split` 方法对文本进行标记化（这是一种非常粗略的近似，与大多数 LLM 标记化文本的方法不符），然后通过 String 类的 `join` 方法将标记数组重新转换为字符串。虽然这样做有效，但 LangChain 提供了更好的解决方案。

LangChain 能以多种方式拆分文本。对大多数人来说，最相关的是按标记拆分，因为它保留了文档的流。有关按标记拆分文本的不同方法，有一整页文档 [在这里](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/split_by_token)。许多 NLP 库支持标记化，但我选择了 LLM 原生解决方案 tiktoken，这也是第一个描述的方法。只需几行代码就能更轻松且更有效地拆分文本，同时保留空白。

这只是 LangChain 能够处理的文档准备的一部分。它还能够将文本块转换为适合存储在向量数据库中的文本嵌入，以便稍后检索和包含在提示中。我计划在项目的未来中进行这项工作，包括将相关的供应示例块添加到 PlayerAgent。

LangChain 是一个强大的开源框架，提供了一系列功能和实用工具，用于与 LLMs 进行工作和开发应用程序。无论你使用的是 OpenAI 库还是其他基础模型，LangChain 都支持多种基础模型和 LLM 提供商，使其成为你项目的多功能选择。

尽管 LangChain 相比于原始提示管理可能引入了一些复杂性，但它提供了众多好处，并简化了与 LLMs 的交互。它标准化了流程，并提供了有用的工具来增强你的提示并最大化所选择 LLM 的潜力。

如果你有兴趣了解 LangChain 如何在实际项目中实现，你可以查看更新后的 AdventureGPT 代码库，该库利用 LangChain 来重构和改进现有应用程序。

[](https://github.com/oaguy1/AdventureGPT/tree/langchain?source=post_page-----cd07028c01b0--------------------------------) [## GitHub - oaguy1/AdventureGPT at langchain

### 通过在 GitHub 上创建一个账户来贡献于 oaguy1/AdventureGPT 的开发。

github.com](https://github.com/oaguy1/AdventureGPT/tree/langchain?source=post_page-----cd07028c01b0--------------------------------)

总体而言，LangChain 是一个对开发人员非常有价值的资源，提供了一个全面的框架和广泛的功能来增强 LLM 驱动的应用程序。探索 LangChain，释放你 LLM 项目的全部潜力！
