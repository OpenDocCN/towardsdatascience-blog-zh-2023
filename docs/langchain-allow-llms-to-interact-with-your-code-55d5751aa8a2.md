# 🦜🔗LangChain：允许 LLMs 与你的代码互动

> 原文：[`towardsdatascience.com/langchain-allow-llms-to-interact-with-your-code-55d5751aa8a2`](https://towardsdatascience.com/langchain-allow-llms-to-interact-with-your-code-55d5751aa8a2)

![](img/ff27b1999a99a1cc7a7add035a1901f0.png)

图片由[David Clode](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上

## 学习如何使用工具为你的 LLM 实现自定义功能

[](https://medium.com/@marcellopoliti?source=post_page-----55d5751aa8a2--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----55d5751aa8a2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----55d5751aa8a2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----55d5751aa8a2--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----55d5751aa8a2--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----55d5751aa8a2--------------------------------) ·5 分钟阅读·2023 年 7 月 5 日

--

## 介绍

生成模型受到了大家的关注。许多 AI 应用现在不再需要领域内的机器学习专家，而只需知道如何实现 API 调用。

最近，例如，我参加了一次黑客马拉松，我需要实现一个自定义命名实体识别，但我直接使用了 LLM，并利用了其少样本学习能力来获得我想要的结果，这对于赢得黑客马拉松来说已经足够了！（如果你想，你可以在[这里](https://www.brianknows.org/)查看项目）。

因此，对于许多现实世界的应用，重点更多地转向如何与这些 LLM 进行互动和使用，而不是创建模型。**LangChain** 是一个允许你做到这一点的库，我最近写了几篇关于它的文章。

## LangChain 工具

**工具是 LLM 可以用来增强其能力的工具**。工具可以在链或代理中实例化。

例如，LLM 可能会在回应之前进行维基百科搜索，以确保响应是最新的。

当然，一个代理可以使用多个工具，因此，通常会定义一个工具列表。

现在我们来看看工具的结构。**工具不过是由几个字段组成的类**：

+   **name** (str)：定义工具的唯一名称

+   **description** (str)：工具在自然语言中的用途描述。LLM 能够读取此描述，并判断是否需要该工具来回答查询。

+   **return_direct**（bool）：例如，工具可能会返回自定义函数的输出。我们是否希望将该输出直接呈现给用户（True），还是由 LLM 预处理（False）？

+   **args_schema**（Pydantic BaseModel）：例如，工具可能会使用一个自定义函数，其输入参数必须从用户的查询中提取。我们可以提供更多关于每个参数的信息，以便 LLM 可以更容易地完成这一步骤。

## 如何定义工具

有**多种方法定义工具**，我们将在这篇文章中探讨。首先，我们导入必要的库并实例化一个 OpenAI 模型。

要做到这一点，你需要一个令牌，你可以在我之前的文章中看到如何获得它。

```py
!pip install langchain
!pip install openai

from langchain import LLMMathChain, SerpAPIWrapper
from langchain.agents import AgentType, initialize_agent
from langchain.chat_models import ChatOpenAI
from langchain.tools import BaseTool, StructuredTool, Tool, tool
import os

os.environ["OPENAI_API_KEY"] = ... # insert your API_TOKEN here

llm = ChatOpenAI(temperature=0)
```

**实例化工具的第一种方法是使用 Tool 类。**

**假设我们想给工具增加搜索网络信息的能力，为此我们将使用一个叫 SerpAPI 的 Google API**，你可以在这里注册并获得 API：[`serpapi.com/`](https://serpapi.com/)

让我们实例化一个 SerpAPIWrapper 类，并用 from_function 方法定义工具。

在 func 字段中，我们需要放置一个指向我们希望使用该工具启动的方法的指针，即 SerpAPI 的 run 方法。正如我们所见，我们给工具起了一个名字和描述。这样做比解释要简单。

```py
search = SerpAPIWrapper()
tools = [
    Tool.from_function(
        func=search.run,
        name="Search",
        description="useful for when you need to answer questions about current events"
    ),
]
```

现在我们可以提供给我们的代理创建的工具列表，这里只有一个。

```py
agent = initialize_agent(
    tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True
)

agent.run(
    "Who is Bob Dylan's girlfriend?
)
```

## 自定义工具

在我看来，创建自定义工具最清晰的方法是继承 BaseTool 类。

```py
class CustomTool(BaseTool):
    name = "custom_tool"
    description = "useful for when you need to answer questions about medium articles"

    def _run(
        self, query: str, run_manager: Optional[CallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool."""
        return "I am not a Medium expert, but I know that Marcello is pretty good! :I)"

    async def _arun(
        self, query: str, run_manager: Optional[AsyncCallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool asynchronously."""
        raise NotImplementedError("custom_search does not support async")
```

你可以看到这是一个自定义工具的实现，每当用户询问关于 Medium 的问题时，它就会被使用。然而，返回的字符串不会完全是我设置的内容，因为它会被大型语言模型进一步处理。

如果我们想直接返回某些内容，只需添加一个“return_direct”字段，如下所示。

```py
class CustomTool(BaseTool):
    name = "custom_tool"
    description = "useful for when you need to answer questions about medium articles"
    return_direct=True

    def _run(
        self, query: str, run_manager: Optional[CallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool."""
        return "I am not a Medium expert, but I know that Marcello is pretty good! :I)"

    async def _arun(
        self, query: str, run_manager: Optional[AsyncCallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool asynchronously."""
        raise NotImplementedError("custom_search does not support async")
```

即使我们不使用 _arun 方法（它对异步调用很有用），我们仍然需要实现它，因为 BaseTool 是一个抽象类，如果我们不实现所有的抽象方法，将会出现错误。

## 实际案例

有一天，一个朋友对我说：“嘿 Marcello，既然你做 AI 这类的东西，为什么不帮我做一个**聊天机器人，当需要时返回医生的工作时间，并预约**？”

我解决这个问题的第一个想法是使用 LangChain，并让 LLM 与用户互动，然后只要模型明白用户要求查看工作时间，它就会返回一个 CSV 文件（或者如果你愿意的话是数据框）。

所以同样的东西也可以用于这个用例。假设我们有一个叫做 work_time.csv 的 CSV 文件。

```py
import pandas as pd

class WorkingHours(BaseTool):
    name = "working_hours"
    description = "useful for when you need to answer questions about working hours of the medical staff"
    return_direct=True+

    def _run(
        self, query: str, run_manager: Optional[CallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool."""
        df = pd.read_csv("working_hours.csv") #maybe you need to retieve some real time data from a DB
        return df

    async def _arun(
        self, query: str, run_manager: Optional[AsyncCallbackManagerForToolRun] = None
    ) -> str:
        """Use the tool asynchronously."""
        raise NotImplementedError("custom_search does not support async")
```

就这样，我朋友想要的应用程序原型只用了几行代码就准备好了！显然，和一个好的前端开发人员合作，让它看起来更好！

# 最终思考

LangChain 是一个最近的库，它允许我们在不同的环境中使用 LLM 的强大功能。

我发现能够使用 LLM 来理解上下文，**理解用户的请求，然后运行我自己的自定义函数以实际解决任务**是非常有用的。

这将使你能够编写灵活的代码。要为你的应用程序添加功能，你只需**编写一个函数并告诉模型在认为需要时使用这个函数**，这样就完成了！

如果你对这篇文章感兴趣，请在 Medium 上关注我！

💼 [Linkedin](https://www.linkedin.com/in/marcello-politi/) ️| 🐦 [Twitter](https://twitter.com/_March08_) | [💻](https://emojiterra.com/laptop-computer/) [网站](https://marcello-politi.super.site/)
