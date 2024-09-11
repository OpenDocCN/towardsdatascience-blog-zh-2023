# LLM 能否取代数据分析师？构建一个 LLM 驱动的分析师

> 原文：[https://towardsdatascience.com/can-llms-replace-data-analysts-building-an-llm-powered-analyst-851578fa10ce?source=collection_archive---------0-----------------------#2023-12-11](https://towardsdatascience.com/can-llms-replace-data-analysts-building-an-llm-powered-analyst-851578fa10ce?source=collection_archive---------0-----------------------#2023-12-11)

## 第1部分：赋能 ChatGPT 工具

[](https://miptgirl.medium.com/?source=post_page-----851578fa10ce--------------------------------)[![玛丽亚·曼苏罗娃](../Images/b1dd377b0a1887db900cc5108bca8ea8.png)](https://miptgirl.medium.com/?source=post_page-----851578fa10ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----851578fa10ce--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----851578fa10ce--------------------------------) [玛丽亚·曼苏罗娃](https://miptgirl.medium.com/?source=post_page-----851578fa10ce--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a29a4fc6ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-llms-replace-data-analysts-building-an-llm-powered-analyst-851578fa10ce&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=post_page-15a29a4fc6ad----851578fa10ce---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----851578fa10ce--------------------------------) ·19 分钟阅读·2023年12月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F851578fa10ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-llms-replace-data-analysts-building-an-llm-powered-analyst-851578fa10ce&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=-----851578fa10ce---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F851578fa10ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-llms-replace-data-analysts-building-an-llm-powered-analyst-851578fa10ce&source=-----851578fa10ce---------------------bookmark_footer-----------)![](../Images/b088e6e4add866f67e47f8714dd3bae5.png)

图片来自 DALL-E 3

我认为在过去的一年里，我们每个人至少都有过一次怀疑 ChatGPT 是否（或者更准确地说何时）能够取代你的角色。我也不例外。

我们有一个共同的观点，即生成型 AI 的最新突破将对我们的个人生活和工作产生重大影响。然而，目前尚未明确我们角色的变化会如何发展。

花大量时间思考不同的未来情景及其概率可能很吸引人，但我建议一种完全不同的方法——自己动手构建原型。首先，这相当具有挑战性和趣味性。其次，这将帮助我们以更结构化的方式来看待我们的工作。第三，这将给我们提供一个实践最前沿方法——LLM代理的机会。

在这篇文章中，我们将从简单开始，学习LLM如何利用工具并执行直接任务。但在接下来的文章中，我们将深入探讨不同的方法和LLM代理的最佳实践。

那么，旅程开始吧。

# 什么是数据分析？

在深入了解LLM之前，我们先定义什么是分析，以及我们作为分析师要做哪些任务。

我的座右铭是分析团队的目标是帮助产品团队在可用时间内基于数据做出正确的决策。这是一个很好的使命，但为了定义LLM驱动的分析师的范围，我们应该进一步分解分析工作。

我喜欢Gartner提出的[框架](https://www.gartner.com/en/topics/data-and-analytics)。它识别了四种不同的数据和分析技术：

+   **描述性分析**回答诸如“发生了什么？”的问题。例如，12月份的收入是多少？这种方法包括报告任务和使用BI工具。

+   **诊断分析**更进一步，提出诸如“为什么会发生这种情况？”的问题。例如，为什么收入比去年减少了10%？这种技术需要对数据进行更深入的挖掘和切分。

+   **预测分析**允许我们回答诸如“会发生什么？”的问题。这种方法的两个基石是预测（预测业务正常情况下的未来）和模拟（建模不同可能的结果）。

+   **规范性分析**影响最终决策。常见的问题包括“我们应该关注什么？”或“我们如何能将量提高10%？”。

通常，公司会逐步经历这些阶段。如果你的公司尚未掌握描述性分析（例如没有数据仓库、BI工具或指标定义），那么几乎不可能开始看预测和不同的情景分析。因此，这个框架也可以展示公司的数据成熟度。

同样，当分析师从初级成长为高级时，她通常会经历这些阶段，从明确的报告任务开始，逐步发展到模糊的战略问题。因此，这个框架在个人层面也具有相关性。

如果我们回到LLM驱动的分析师，我们应该专注于描述性分析和报告任务。最好从基础开始。因此，我们将重点学习LLM，以了解有关数据的基本问题。

我们已经为第一个原型定义了重点。因此，我们准备进入技术问题，讨论 LLM 代理和工具的概念。

# LLM 代理和工具

当我们之前使用 LLM 时（例如，为了做主题建模[这里](/topic-modelling-in-production-e3b3e99e4fca)），我们在代码中自己描述了确切的步骤。例如，让我们看看下面的链条。首先，我们要求模型确定客户评论的情感。然后，根据情感，从评论中提取提到的优点或缺点。

![](../Images/351d036bc9e9dda4689b9cad8da4841b.png)

作者插图

在这个例子中，我们明确地定义了 LLM 的行为，并且 LLM 很好地解决了这个任务。然而，如果我们构建更高层次和模糊的东西，比如一个由 LLM 驱动的分析师，这种方法就不起作用了。

如果你曾经作为分析师或与分析师合作过至少一天，你会知道分析师会收到各种不同的问题，从基础问题（如“昨天我们网站上有多少客户？”或“你能为我们明天的董事会会议做个图表吗？”）到非常高层次的问题（例如，“主要的客户痛点是什么？”或“我们应该接下来进入哪个市场？”）。不用说，描述所有可能的场景是不现实的。

然而，有一种方法可能对我们有所帮助——代理。代理的核心思想是将 LLM 用作推理引擎，可以选择下一步做什么，以及何时向客户返回最终答案。这听起来非常接近我们的行为：我们接到任务，定义所需工具，使用它们，然后在准备好时返回最终答案。

与代理相关的基本概念（我已经在上面提到过）是工具。工具是 LLM 可以调用以获取缺失信息的功能（例如，执行 SQL、使用计算器或调用搜索引擎）。工具至关重要，因为它们使你能够将 LLM 提升到一个新的水平，并与世界互动。在本文中，我们将主要关注作为工具的 OpenAI 函数。

OpenAI 已经对模型进行了微调，以便能够处理函数：

+   你可以将包含描述的函数列表传递给模型；

+   如果与你的查询相关，模型会返回一个函数调用——函数名称和调用它的输入参数。

你可以在[文档](https://platform.openai.com/docs/guides/function-calling)中找到更多信息和支持函数的模型的最新列表。

使用 LLM 的函数有两个显著的用例：

+   标记和提取——在这些情况下，函数用于确保模型的输出格式。你将得到一个结构化的函数调用，而不是通常的内容输出。

+   工具和路由——这是一个更有趣的用例，允许你创建一个代理。

让我们从提取的更直接的用例开始，以了解如何使用 OpenAI 函数。

# 用例 #1: 标记与提取

你可能会想知道标记和提取之间有什么区别。这些术语很接近。唯一的区别在于模型是提取文本中呈现的信息还是标记文本提供新信息（即定义语言或情感）。

![](../Images/a5448906e9ce962ab3d0a17a25e1e470.png)

作者插图

由于我们决定专注于描述性分析和报告任务，让我们使用这种方法来结构化传入的数据请求，并提取以下组件：度量标准、维度、筛选条件、时间段和期望输出。

![](../Images/1ebc1a0b667eb225b3d60c210bc71d94.png)

作者插图

这将是一个提取示例，因为我们只需要文本中存在的信息。

## OpenAI Completion API基础示例

首先，我们需要定义函数。OpenAI期望一个作为JSON的函数描述。这个JSON会传递给LLM，所以我们需要告诉它所有的上下文：这个函数的作用是什么以及如何使用它。

这是一个函数JSON的示例。我们已经指定了：

+   函数本身的`name`和`description`，

+   每个参数的`type`和`description`，

+   函数所需的输入参数列表。

```py
extraction_functions = [
    {
        "name": "extract_information",
        "description": "extracts information",
        "parameters": {
            "type": "object",
            "properties": {
                "metric": {
                    "type": "string",
                    "description": "main metric we need to calculate, for example, 'number of users' or 'number of sessions'",
                },
                "filters": {
                    "type": "string",
                    "description": "filters to apply to the calculation (do not include filters on dates here)",
                },
                "dimensions": {
                    "type": "string",
                    "description": "parameters to split your metric by",
                },
                "period_start": {
                    "type": "string",
                    "description": "the start day of the period for a report",
                },
                "period_end": {
                    "type": "string",
                    "description": "the end day of the period for a report",
                },
                "output_type": {
                    "type": "string",
                    "description": "the desired output",
                    "enum": ["number", "visualisation"]
                }
            },
            "required": ["metric"],
        },
    }
]
```

在这个用例中没有必要实现函数本身，因为我们不会使用它。我们只是以结构化的方式作为函数调用获得LLM响应。

现在，我们可以使用标准的OpenAI Chat Completion API来调用函数。我们传递给API调用：

+   模型——我使用了最新的ChatGPT 3.5 Turbo，它可以与函数一起工作，

+   消息列表——一个系统消息以设置上下文和一个用户请求，

+   我们之前定义的函数列表。

```py
import openai

messages = [
    {
        "role": "system",
        "content": "Extract the relevant information from the provided request."
    },
    {
        "role": "user",
        "content": "How did number of iOS users change over time?"
    }
]

response = openai.ChatCompletion.create(
    model = "gpt-3.5-turbo-1106", 
    messages = messages,
    functions = extraction_functions
)

print(response)
```

结果是我们得到了以下JSON。

```py
{
  "id": "chatcmpl-8TqGWvGAXZ7L43gYjPyxsWdOTD2n2",
  "object": "chat.completion",
  "created": 1702123112,
  "model": "gpt-3.5-turbo-1106",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": null,
        "function_call": {
          "name": "extract_information",
          "arguments": "{\"metric\":\"number of users\",\"filters\":\"platform='iOS'\",\"dimensions\":\"date\",\"period_start\":\"2021-01-01\",\"period_end\":\"2021-12-31\",\"output_type\":\"visualisation\"}"
        }
      },
      "finish_reason": "function_call"
    }
  ],
  "usage": {
    "prompt_tokens": 159,
    "completion_tokens": 53,
    "total_tokens": 212
  },
  "system_fingerprint": "fp_eeff13170a"
}
```

> 记住，函数和函数调用会计入令牌限制并产生费用。

模型返回了一个函数调用而不是常见的响应：我们可以看到`content`是空的，而`finish_reason`等于`function_call`。在响应中，还有函数调用的输入参数：

+   `metric = "number of users"`，

+   `filters = "platform = 'iOS'"`，

+   `dimensions = "date"`，

+   `period_start = "2021-01-01"`，

+   `period_start = "2021-12-31"`，

+   `output_type = "visualisation"`。

模型做得很好。唯一的问题是它凭空假设了时间段。我们可以通过向系统消息添加更明确的指导来解决这个问题，例如，`"从提供的请求中提取相关信息。仅提取初始请求中呈现的信息；不要添加任何其他内容。如果缺少内容，返回部分信息。"`

默认情况下，模型决定是否独立使用函数（`function_call = 'auto'`）。我们可以要求它每次返回特定的函数调用，或者根本不使用函数。

```py
 # always calling extract_information function
response = openai.ChatCompletion.create(
    model = "gpt-3.5-turbo-1106",
    messages = messages,
    functions = extraction_functions,
    function_call = {"name": "extract_information"}
)

# no function calls
response = openai.ChatCompletion.create(
    model = "gpt-3.5-turbo-1106",
    messages = messages,
    functions = extraction_functions,
    function_call = "none"
)
```

我们得到了第一个使用LLM函数的工作程序。这很棒。然而，在JSON中描述函数并不是很方便。让我们讨论如何简化这个过程。

## 使用Pydantic定义函数

为了更方便地定义函数，我们可以利用[Pydantic](https://docs.pydantic.dev/latest/)。Pydantic是最受欢迎的数据验证Python库。

> 我们已经[使用](https://medium.com/towards-data-science/topic-modelling-in-production-e3b3e99e4fca)Pydantic定义了LangChain输出解析器。

首先，我们需要创建一个继承自`BaseModel`类的类，并定义所有字段（我们函数的参数）。

```py
from pydantic import BaseModel, Field
from typing import Optional

class RequestStructure(BaseModel):
  """extracts information"""
  metric: str = Field(description = "main metric we need to calculate, for example, 'number of users' or 'number of sessions'")
  filters: Optional[str] = Field(description = "filters to apply to the calculation (do not include filters on dates here)")
  dimensions: Optional[str] = Field(description = "parameters to split your metric by")
  period_start: Optional[str] = Field(description = "the start day of the period for a report")
  period_end: Optional[str] = Field(description = "the end day of the period for a report")
  output_type: Optional[str] = Field(description = "the desired output", enum = ["number", "visualisation"])
```

然后，我们可以使用LangChain将Pydantic类转换为OpenAI函数。

```py
from langchain.utils.openai_functions import convert_pydantic_to_openai_function
extract_info_function = convert_pydantic_to_openai_function(RequestStructure, 
    name = 'extract_information')
```

LangChain验证我们提供的类。例如，它确保函数描述被指定，因为LLM需要它来使用这个工具。

结果是，我们得到了相同的JSON以传递给LLM，但现在我们将其表示为Pydantic类。

```py
{'name': 'extract_information',
 'description': 'extracts information',
 'parameters': {'title': 'RequestStructure',
  'description': 'extracts information',
  'type': 'object',
  'properties': {'metric': {'title': 'Metric',
    'description': "main metric we need to calculate, for example, 'number of users' or 'number of sessions'",
    'type': 'string'},
   'filters': {'title': 'Filters',
    'description': 'filters to apply to the calculation (do not include filters on dates here)',
    'type': 'string'},
   'dimensions': {'title': 'Dimensions',
    'description': 'parameters to split your metric by',
    'type': 'string'},
   'period_start': {'title': 'Period Start',
    'description': 'the start day of the period for a report',
    'type': 'string'},
   'period_end': {'title': 'Period End',
    'description': 'the end day of the period for a report',
    'type': 'string'},
   'output_type': {'title': 'Output Type',
    'description': 'the desired output',
    'enum': ['number', 'visualisation'],
    'type': 'string'}},
  'required': ['metric']}}
```

现在，我们可以在调用OpenAI时使用它。让我们从OpenAI API切换到LangChain，使我们的API调用更加模块化。

## 定义LangChain链

让我们定义一个链，以从请求中提取所需的信息。我们将使用LangChain，因为它是LLM最受欢迎的框架。如果你之前没有使用过它，我推荐你在[我之前的一篇文章](https://medium.com/towards-data-science/topic-modelling-in-production-e3b3e99e4fca)中学习一些基础知识。

我们的链很简单。它由一个Open AI模型和一个包含一个变量`request`（用户消息）的提示组成。

我们还使用了`bind`函数将`functions`参数传递给模型。`bind`函数[允许](https://python.langchain.com/docs/expression_language/how_to/binding)我们为模型指定常量参数，这些参数不是输入的一部分（例如，`functions`或`temperature`）。

```py
from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI

model = ChatOpenAI(temperature=0.1, model = 'gpt-3.5-turbo-1106')\
  .bind(functions = [extract_info_function])

prompt = ChatPromptTemplate.from_messages([
    ("system", "Extract the relevant information from the provided request. \
            Extract ONLY the information presented in the initial request. \
            Don't add anything else. \
            Return partial information if something is missing."),
    ("human", "{request}")
])

extraction_chain = prompt | model
```

现在是尝试我们的函数的时候了。我们需要使用invoke方法并传递一个`request`。

```py
extraction_chain.invoke({'request': "How many customers visited our site on iOS in April 2023 from different countries?"})
```

在输出中，我们得到了没有任何内容但有一个函数调用的`AIMessage`。

```py
AIMessage(
  content='', 
  additional_kwargs={
    'function_call': {
       'name': 'extract_information', 
       'arguments': '''{
         "metric":"number of customers", "filters":"device = 'iOS'",
         "dimensions":"country", "period_start":"2023-04-01",
         "period_end":"2023-04-30", "output_type":"number"}
        '''}
  }
)
```

所以，我们已经学习了如何在LangChain中使用OpenAI函数来获得结构化输出。现在，让我们转到更有趣的用例——工具和路由。

# 用例 #2：工具与路由

现在是使用工具并赋予我们的模型外部能力的时候了。在这种方法中，模型是推理引擎，它们可以决定使用哪些工具以及何时使用（这称为路由）。

LangChain有一个[工具](https://python.langchain.com/docs/modules/agents/tools/)的概念——代理可以用来与世界互动的接口。工具可以是函数、LangChain链或甚至其他代理。

我们可以使用`format_tool_to_openai_function`轻松将工具转换为OpenAI函数，并不断将`functions`参数传递给LLM。

## 定义自定义工具

让我们教会我们的LLM驱动的分析师计算两个指标之间的差异。我们知道LLM在数学上可能会出错，因此我们希望要求模型使用计算器，而不是依靠自己计算。

要定义一个工具，我们需要创建一个函数并使用`@tool`装饰器。

```py
from langchain.agents import tool

@tool
def percentage_difference(metric1: float, metric2: float) -> float:
    """Calculates the percentage difference between metrics"""
    return (metric2 - metric1)/metric1*100
```

现在，这个函数有`name`和`description`参数，这些参数将传递给LLMs。

```py
print(percentage_difference.name)
# percentage_difference.name

print(percentage_difference.args)
# {'metric1': {'title': 'Metric1', 'type': 'number'},
# 'metric2': {'title': 'Metric2', 'type': 'number'}}

print(percentage_difference.description)
# 'percentage_difference(metric1: float, metric2: float) -> float - Calculates the percentage difference between metrics'
```

这些参数将用于创建OpenAI函数规范。让我们将我们的工具转换为OpenAI函数。

```py
from langchain.tools.render import format_tool_to_openai_function
print(format_tool_to_openai_function(percentage_difference))
```

我们得到了以下的JSON作为结果。它概述了结构，但缺少字段描述。

```py
{'name': 'percentage_difference',
 'description': 'percentage_difference(metric1: float, metric2: float) -> float - Calculates the percentage difference between metrics',
 'parameters': {'title': 'percentage_differenceSchemaSchema',
  'type': 'object',
  'properties': {'metric1': {'title': 'Metric1', 'type': 'number'},
   'metric2': {'title': 'Metric2', 'type': 'number'}},
  'required': ['metric1', 'metric2']}
}
```

我们可以使用Pydantic来指定参数的模式。

```py
class Metrics(BaseModel):
    metric1: float = Field(description="Base metric value to calculate the difference")
    metric2: float = Field(description="New metric value that we compare with the baseline")

@tool(args_schema=Metrics)
def percentage_difference(metric1: float, metric2: float) -> float:
    """Calculates the percentage difference between metrics"""
    return (metric2 - metric1)/metric1*100
```

现在，如果我们将新版本转换为OpenAI函数规范，它将包括参数描述。这样更好，因为我们可以与模型共享所有需要的上下文。

```py
{'name': 'percentage_difference',
 'description': 'percentage_difference(metric1: float, metric2: float) -> float - Calculates the percentage difference between metrics',
 'parameters': {'title': 'Metrics',
  'type': 'object',
  'properties': {'metric1': {'title': 'Metric1',
    'description': 'Base metric value to calculate the difference',
    'type': 'number'},
   'metric2': {'title': 'Metric2',
    'description': 'New metric value that we compare with the baseline',
    'type': 'number'}},
  'required': ['metric1', 'metric2']}}
```

所以，我们已经定义了LLM可以使用的工具。让我们在实践中尝试一下。

## 实践中使用工具

让我们定义一个链，并将我们的工具传递给函数。然后，我们可以在用户请求上测试它。

```py
model = ChatOpenAI(temperature=0.1, model = 'gpt-3.5-turbo-1106')\
  .bind(functions = [format_tool_to_openai_function(percentage_difference)])

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a product analyst willing to help your product team. You are very strict to the point and accurate. You use only facts, not inventing information."),
    ("user", "{request}")
])

analyst_chain = prompt | model
analyst_chain.invoke({'request': "In April we had 100 users and in May only 95\. What is difference in percent?"})
```

我们得到了一个带有正确参数的函数调用，所以它正在工作。

```py
AIMessage(content='', additional_kwargs={
    'function_call': {
      'name': 'percentage_difference', 
      'arguments': '{"metric1":100,"metric2":95}'}
  }
)
```

为了更方便地处理输出，我们可以使用`OpenAIFunctionsAgentOutputParser`。让我们将它添加到我们的链中。

```py
from langchain.agents.output_parsers import OpenAIFunctionsAgentOutputParser
analyst_chain = prompt | model | OpenAIFunctionsAgentOutputParser()
result = analyst_chain.invoke({'request': "There were 100 users in April and 110 users in May. How did the number of users changed?"})
```

现在，我们以更结构化的方式获得了输出，我们可以轻松地检索工具的参数，像`result.tool_input`。

```py
AgentActionMessageLog(
   tool='percentage_difference', 
   tool_input={'metric1': 100, 'metric2': 110}, 
   log="\nInvoking: `percentage_difference` with `{'metric1': 100, 'metric2': 110}`\n\n\n", 
   message_log=[AIMessage(content='', additional_kwargs={'function_call': {'name': 'percentage_difference', 'arguments': '{"metric1":100,"metric2":110}'}})]
)
```

所以，我们可以像LLM请求的那样执行函数。

```py
observation = percentage_difference(result.tool_input)
print(observation)
# 10
```

如果我们想从模型中获取最终答案，我们需要将函数执行结果传递回去。为此，我们需要定义一个消息列表来传递给模型观察结果。

```py
from langchain.prompts import MessagesPlaceholder

model = ChatOpenAI(temperature=0.1, model = 'gpt-3.5-turbo-1106')\
  .bind(functions = [format_tool_to_openai_function(percentage_difference)])

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a product analyst willing to help your product team. You are very strict to the point and accurate. You use only facts, not inventing information."),
    ("user", "{request}"),
    MessagesPlaceholder(variable_name="observations")
])

analyst_chain = prompt | model | OpenAIFunctionsAgentOutputParser()
result1 = analyst_chain.invoke({
    'request': "There were 100 users in April and 110 users in May. How did the number of users changed?",
    "observations": []
})

observation = percentage_difference(result1.tool_input)
print(observation)
# 10
```

然后，我们需要将观察结果添加到我们的`observations`变量中。我们可以使用`format_to_openai_functions`函数以模型期望的方式格式化结果。

```py
from langchain.agents.format_scratchpad import format_to_openai_functions
format_to_openai_functions([(result1, observation), ])
```

结果，我们得到了LLM能够理解的消息。

```py
[AIMessage(content='', additional_kwargs={'function_call': {'name': 'percentage_difference', 
                                           'arguments': '{"metric1":100,"metric2":110}'}}),
 FunctionMessage(content='10.0', name='percentage_difference')]
```

让我们再调用一次链，将函数执行结果作为观察结果传递。

```py
result2 = analyst_chain.invoke({
    'request': "There were 100 users in April and 110 users in May. How did the number of users changed?",
    "observations": format_to_openai_functions([(result1, observation)])
})
```

现在，我们得到了来自模型的最终结果，听起来很合理。

```py
AgentFinish(
  return_values={'output': 'The number of users increased by 10%.'}, 
  log='The number of users increased by 10%.'
)
```

> 如果我们使用的是原始OpenAI Chat Completion API，我们可以只添加另一条消息，角色设置为tool。你可以在[这里](https://platform.openai.com/docs/guides/function-calling)找到详细的示例。

如果我们开启调试，我们可以看到传递给OpenAI API的确切提示。

```py
System: You are a product analyst willing to help your product team. You are very strict to the point and accurate. You use only facts, not inventing information.
Human: There were 100 users in April and 110 users in May. How did the number of users changed?
AI: {'name': 'percentage_difference', 'arguments': '{"metric1":100,"metric2":110}'}
Function: 10.0
```

要开启LangChain调试，执行以下代码并调用你的链，看看在幕后发生了什么。

```py
import langchain
langchain.debug = True
```

我们尝试使用了一个工具，但让我们扩展我们的工具包，看看LLM如何处理它。

## 路由：使用多个工具

让我们在分析工具包中再添加几个工具：

+   获取每月活跃用户

+   使用Wikipedia。

首先，让我们定义一个虚拟函数来计算按月份和城市过滤的观众。我们将再次使用Pydantic来指定函数的输入参数。

```py
import datetime
import random

class Filters(BaseModel):
    month: str = Field(description="Month of customer's activity in the format %Y-%m-%d")
    city: Optional[str] = Field(description="City of residence for customers (by default no filter)", 
                    enum = ["London", "Berlin", "Amsterdam", "Paris"])

@tool(args_schema=Filters)
def get_monthly_active_users(month: str, city: str = None) -> int:
    """Returns number of active customers for the specified month"""
    dt = datetime.datetime.strptime(month, '%Y-%m-%d')
    total = dt.year + 10*dt.month
    if city is None:
        return total
    else:
        return int(total*random.random())
```

然后，让我们使用[the wikipedia](https://pypi.org/project/wikipedia/) Python包来允许模型查询Wikipedia。

```py
import wikipedia

class Wikipedia(BaseModel):
    term: str = Field(description="Term to search for")

@tool(args_schema=Wikipedia)
def get_summary(term: str) -> str:
    """Returns basic knowledge about the given term provided by Wikipedia"""
    return wikipedia.summary(term)
```

让我们定义一个包含模型目前知道的所有函数的字典。这个字典将帮助我们以后进行路由。

```py
toolkit = {
    'percentage_difference': percentage_difference,
    'get_monthly_active_users': get_monthly_active_users,
    'get_summary': get_summary
}

analyst_functions = [format_tool_to_openai_function(f) 
  for f in toolkit.values()]
```

我对我们之前的设置做了一些更改：

+   我稍微调整了系统提示，强制LLM在需要基本知识时咨询Wikipedia。

+   我已经将模型更改为 GPT 4，因为它更适合处理需要推理的任务。

```py
from langchain.prompts import MessagesPlaceholder

model = ChatOpenAI(temperature=0.1, model = 'gpt-4-1106-preview')\
  .bind(functions = analyst_functions)

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a product analyst willing to help your product team. You are very strict to the point and accurate. \
        You use only information provided in the initial request. \
        If you need to determine some information i.e. what is the name of the capital, you can use Wikipedia."),
    ("user", "{request}"),
    MessagesPlaceholder(variable_name="observations")
])

analyst_chain = prompt | model | OpenAIFunctionsAgentOutputParser()
```

我们可以调用我们的链条中的所有函数。让我们从一个非常直接的查询开始。

```py
result1 = analyst_chain.invoke({
    'request': "How many users were in April 2023 from Berlin?",
    "observations": []
})
print(result1)
```

我们得到的结果是对 `get_monthly_active_users` 的函数调用，输入参数为 `{'month': '2023–04–01', 'city': 'Berlin'}`，这看起来是正确的。模型能够找到正确的工具并解决任务。

让我们尝试将任务变得更复杂一些。

```py
result1 = analyst_chain.invoke({
    'request': "How did the number of users from the capital of Germany\
        change between April and May 2023?",
    "observations": []
})
```

让我们停下来想一想我们希望模型如何推理。显然，模型没有足够的信息来直接回答，所以它需要进行一系列函数调用：

+   调用 Wikipedia 获取德国的首都

+   调用 `get_monthly_active_users` 函数两次，以获取 4 月和 5 月的 MAU

+   调用 `percentage_difference` 以计算指标之间的差异。

这看起来相当复杂。让我们看看 ChatGPT 是否能够处理这个问题。

在第一次调用中，LLM 返回了一个对 Wikipedia 的函数调用，参数为 `{'term': 'capital of Germany'}`。到目前为止，它遵循了我们的计划。

让我们提供观察结果，看看接下来的步骤会是什么。

```py
observation1 = toolkit[result1.tool](result1.tool_input)
print(observation1)

# The capital of Germany is the  city state of Berlin. It is the seat of 
# the President of Germany, whose official residence is Schloss Bellevue. 
# The Bundesrat ("federal council") is the representation of the Federal States 
# (Bundesländer) of Germany and has its seat at the former Prussian Herrenhaus 
# (House of Lords). Though most of the ministries are seated in Berlin, 
# some of them, as well as some minor departments, are seated in Bonn, 
# the former capital of West Germany.
# Although Berlin is officially the capital of the Federal Republic of Germany,
# 8,000 out of the 18,000 total officials employed at the federal bureaucracy 
# still work in Bonn, about 600 km (370 mi) away from Berlin.

# source: https://en.wikipedia.org/wiki/Capital_of_Germany 

result2 = analyst_chain.invoke({
    'request': "How did the number of users from the capital of Germany change between April and May 2023?",
    "observations": format_to_openai_functions([(result1, observation1)])
})
```

模型想要执行 `get_monthly_active_users`，参数为 `{'month': '2023–04–01', 'city': 'Berlin'}`。让我们执行它，并再次将信息返回给模型。

```py
observation2 = toolkit[result2.tool](result2.tool_input)
print(observation2)
# 168

result3 = analyst_chain.invoke({
    'request': "How did the number of users from the capital of Germany change between April and May 2023?",
    "observations": format_to_openai_functions([(result1, observation1), (result2, observation2)])
})
```

然后，模型请求再次调用 `get_monthly_active_users`，参数为 `{'month': '2023–05–01', 'city': 'Berlin'}`。到目前为止，它表现得非常出色。让我们跟随它的逻辑。

```py
observation3 = toolkit[result3.tool](result3.tool_input)
print(observation3)
# 1046

result4 = analyst_chain.invoke({
    'request': "How did the number of users from the capital of Germany change between April and May 2023?",
    "observations": format_to_openai_functions(
      [(result1, observation1), (result2, observation2), 
      (result3, observation3)])
})
```

随后的结果是一个对 percentage_difference 的函数调用，参数为 `{'metric1': 168, 'metric2': 1046}`。让我们计算观察结果并再次调用我们的链条。希望这将是最后一步。

```py
observation4 = toolkit[result4.tool](result4.tool_input)
print(observation4)

# 523.27

result5 = analyst_chain.invoke({
    'request': "How did the number of users from the capital of Germany change between April and May 2023?",
    "observations": format_to_openai_functions(
      [(result1, observation1), (result2, observation2), 
      (result3, observation3), (result4, observation4)])
})
```

最终，我们从模型中得到以下响应：`德国首都柏林的用户数量在 2023 年 4 月到 5 月间增加了约 523.27%。`

这是针对这个问题的 LLM 调用的完整方案。

![](../Images/227c76d1bfd7066c2b81cb64329fb0e6.png)

作者插图

在上述示例中，我们手动逐一触发了后续调用，但这可以很容易地自动化。

这是一个很棒的结果，我们能够看到 LLM 如何进行推理并利用多种工具。模型用了 5 步达成结果，但它遵循了我们最初制定的计划，因此这是一个相当合乎逻辑的路径。然而，如果你计划将 LLM 用于生产环境，请记住，它可能会犯错误，并引入评估和质量保证流程。

> 你可以在 [GitHub](https://github.com/miptgirl/miptgirl_medium/blob/main/analyst_agent/functions_how_to.ipynb) 上找到完整的代码。

# 总结

本文教会了我们如何利用OpenAI函数为LLMs提供外部工具。我们已经考察了两个使用案例：提取以获得结构化输出和路由以使用外部信息回答问题。最终结果让我感到鼓舞，因为LLM能够使用三种不同的工具回答相当复杂的问题。

回到最初的问题，LLMs是否能取代数据分析师。我们当前的原型仍然很基础，远未达到初级分析师的能力，但这只是开始。敬请关注！我们将深入探讨LLM代理的不同方法。下次，我们将尝试创建一个能够访问数据库并回答基本问题的代理。

# 参考

本文的灵感来源于DeepLearning.AI的[“LangChain中的函数、工具和代理”](https://www.deeplearning.ai/short-courses/functions-tools-agents-langchain/)课程。
