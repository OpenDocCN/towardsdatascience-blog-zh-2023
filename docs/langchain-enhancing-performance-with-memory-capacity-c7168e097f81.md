# 🦜🔗LangChain：通过记忆容量提升性能

> 原文：[`towardsdatascience.com/langchain-enhancing-performance-with-memory-capacity-c7168e097f81`](https://towardsdatascience.com/langchain-enhancing-performance-with-memory-capacity-c7168e097f81)

![](img/7ad10b2af7c0deba0e3b68918593a2f0.png)

图片由 [Milad Fakurian](https://unsplash.com/ko/@fakurian?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## LangChain 通过记忆扩展技术提升性能

[](https://medium.com/@marcellopoliti?source=post_page-----c7168e097f81--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----c7168e097f81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c7168e097f81--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7168e097f81--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----c7168e097f81--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c7168e097f81--------------------------------) ·4 分钟阅读·2023 年 6 月 21 日

--

我之前已经发布过关于 LangChain 的[文章](https://medium.com/towards-data-science/develop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a)，介绍了这个库及其所有功能。现在我想集中讨论一个关键方面，即如何在智能聊天机器人中管理记忆。

聊天机器人或代理程序也需要一种信息存储机制，这可以采取不同的形式并执行不同的功能。**在聊天机器人中实现记忆系统不仅有助于使它们更加聪明，还能让它们对用户更自然、更有用**。

幸运的是，LangChain 提供了使开发人员能够轻松实现应用程序中记忆的 API。在本文中，我们将更详细地探讨这一方面。

## 在 LangChain 中使用记忆

开发聊天机器人的最佳实践之一是**保存聊天机器人与用户的所有互动**。这是因为**LLM 的状态可能会根据过去的对话而变化**，实际上，LLM 对两个用户提出的相同问题会给出不同的回答，因为他们与聊天机器人有不同的过去对话，因此它处于不同的状态。

因此，聊天机器人记忆创建的内容无非就是旧消息的列表，这些消息在提出新问题之前会反馈给它。当然，**LLM 有有限的上下文，因此你需要稍微发挥创造力，选择如何将这些历史记录反馈给 LLM**。最常见的方法是返回旧消息的摘要或仅返回最近的 N 条消息，这些消息可能是最具信息性的。

## 从 ChatMessageHistory 的基础开始

这是允许我们管理聊天机器人（AI）与用户（Human）之间消息的主要类。此类提供了两个主要方法，如下所示。

+   **add_user_message:** 允许我们将消息添加到聊天机器人的记忆中，并将该消息标记为“用户”

+   **add_ai_message:** 允许我们将消息添加到聊天机器人的记忆中，并将该消息标记为“AI”

```py
!pip install langchain
from langchain.memory import ChatMessageHistory

history = ChatMessageHistory()

history.add_user_message("Hi!")
history.add_ai_message("Hey, how can I help you today?")

#print messages
history.messages
```

这个类允许你做各种事情，但在最简单的使用情况下，你可以将其视为不时将各种消息保存到列表中。然后，你也可以通过以下方式遍历历史记录，简单地查看你添加的所有消息。

```py
for message in history.messages:
    print(message.content)
```

## 使用 ConversationBufferMemory 进行高级记忆管理

**ConversationBufferMemory** 类在消息存储方面的行为有点类似于 ChatMessageHistory 类，但它**提供了聪明的方法来检索旧消息**。

例如，我们可以以消息列表或一个大字符串的形式检索旧消息，这取决于我们的需求。如果我们想让 LLM 对过去的对话进行总结，将过去的对话作为一个大字符串可能会很有用。如果我们想对过去的对话进行详细分析，我们可以通过提取列表来逐条阅读消息。

使用 ConversationBufferMemory 类，我们还可以通过 add_user_message 和 add_user_message 方法将消息添加到历史记录中。

另一方面，load_memory_variables 方法用于提取旧消息，可以是列表或字典形式，具体取决于指定的内容，让我们看一个示例。

```py
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory()

memory.chat_memory.add_user_message("Hi, how are you?")
memory.chat_memory.add_ai_message("hey,I am fine! How are you?")

memory_variables = memory.load_memory_variables({})
print(memory_variables['history'])
```

## 管理多个对话的记忆

我们已经看到了如何通过保存和检索消息来管理记忆的简单示例。**但在实际应用中，你可能需要管理多个对话的记忆**。LangChain 允许你通过使用所谓的**链**来管理这种情况。

**链不过是实现某个目标的一系列简单或复杂步骤的工作流。**

**例如，一个 LLM 查找维基百科上的一条信息，因为它不知道如何回答某个问题，这就是一个链。**

要处理各种对话，只需将一个 ConversationBufferMemory 实例与每个创建的链关联即可，这些链是通过 ConversationChain 类的实例化来创建的。

这样，当调用模型的 predict 方法时，链中的所有步骤都会运行，因此模型会读取对话中的过去消息。

让我们看一个简单的例子。

```py
from langchain.llms import OpenAI
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory()
conversation = ConversationChain(llm= OpenAI(), memory=memory)

conversation.predict(input="Hey! how are you?")
```

# 结束语

总结一下，**记忆是聊天机器人的一个关键组成部分，** LangChain 提供了多个框架和工具来有效管理记忆。通过使用如**ChatMessageHistory 和 ConversationBufferMemory** 等类，你可以捕捉并存储用户与 AI 的互动，并利用这些信息指导未来的 AI 响应。我希望这些信息能帮助你构建更智能、更强大的聊天机器人！

在下一篇文章中，我将向你展示如何使用 LangChain 工具。

如果你觉得这篇文章有用，可以在 Medium 上关注我！[😉](https://emojipedia.org/it/apple/ios-15.4/faccina-che-fa-l-occhiolino/)

# 结束

*马塞洛·波利提*

[Linkedin](https://www.linkedin.com/in/marcello-politi/)、[Twitter](https://twitter.com/_March08_)、[Website](https://marcello-politi.super.site/)
