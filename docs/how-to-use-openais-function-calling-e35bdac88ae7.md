# 如何使用 OpenAI 的函数调用

> 原文：[`towardsdatascience.com/how-to-use-openais-function-calling-e35bdac88ae7`](https://towardsdatascience.com/how-to-use-openais-function-calling-e35bdac88ae7)

## 函数调用概述及其对构建 LLM 应用的影响

[](https://johnadeojo.medium.com/?source=post_page-----e35bdac88ae7--------------------------------)![John Adeojo](https://johnadeojo.medium.com/?source=post_page-----e35bdac88ae7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e35bdac88ae7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e35bdac88ae7--------------------------------) [John Adeojo](https://johnadeojo.medium.com/?source=post_page-----e35bdac88ae7--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e35bdac88ae7--------------------------------) ·阅读时间 6 分钟·2023 年 7 月 25 日

--

![](img/a09010bff67cd967314369c4fcabb3a3.png)

图片来源：由 Midjourney 生成

# 结构化非结构化数据

函数调用是[OpenAI](https://openai.com/)的一项创新，扩展了在大型语言模型应用开发方面的可能性。

然而，我发现它仍然被一些人误解。在这篇文章中，我旨在在你泡一杯咖啡的时间内澄清函数调用的概念。

如果你有意构建 LLM 应用，将 LLM 集成到你的业务中，或仅仅是想扩展你在这方面的知识，那么这篇文章适合你。

# 函数调用有什么了不起的地方？

函数调用使我们能够在现有的 API 上开发自然语言接口。如果这听起来令你困惑，不用担心——随着你继续阅读，细节会变得更加清晰。

那么，自然语言 API 是什么样的？我认为最好通过示意图来展示。这是一个示例应用，使用函数调用来帮助用户查找航班。

![](img/42b57c9e4e98903e44fbf7ea6dcfef4d.png)

图片来源：函数调用的示意图

如果没有函数调用，要实现这种类型的应用，你需要提示用户通过菜单、选择框等输入航班信息。

函数调用还使用户可以通过语音发出请求。你只需一个额外的转录服务，便可以拥有一个 AI 个人助手。

希望你现在已经对函数调用及其用途有了更清晰的了解。让我们通过一些技术演练来巩固这些新知识。

# 单一函数的函数调用

让我们来查看一个简单的天气 API 使用案例。

最基本的用例涉及一个函数的调用。为了说明这一点，我建模了一个提供温度预报的 API —— [请见这里](https://gist.github.com/john-adeojo/13ae233ecfb25aa6908ae834ef16ef53)。

请花一点时间研究一下这个温度 API；注意它有两个参数：位置和日期。

现在，假设我们想基于这个 API 构建一个应用程序。我们可能采取的第一步是编写一个利用函数调用处理用户查询的脚本。它可能如下所示：

作者脚本：一个函数的调用示例

让我们讨论一下函数调用。我们将函数定义为字典的列表。该函数允许我们从用户查询中提取参数，然后我们可以将其用来馈送到 API 中。在我们的情况下，我们希望提取的参数是“位置”和“日期”。

我们声明函数的名称，在这种情况下是“get_temperature”，并提供它的功能描述。

接下来，我们需要描述函数的参数，这一点至关重要。我们从函数调用中收到的内容受我们如何描述的影响。请查看我如何为两个参数“位置”和“日期”概述描述和类型。

描述很重要，因为函数调用是由 OpenAI 的大型语言模型驱动的，这些模型解释描述并作出相应的回应。

接下来，我们设置我们的 OpenAI 模型。该设置类似于标准的文本生成用例，但有两个关键区别——“functions”参数和“function_call”参数。

我们将字典中定义的函数传递给“functions”参数。对于“function_call”参数，我们将其设置为“auto”，允许大型语言模型决定何时使用函数或根本不使用函数。

那么，让我们看看当我们通过这个函数调用运行查询时会发生什么。

这是我们的查询：

> query = "2023 年 9 月 1 日柏林的天气预报是多少？"

这是响应：

```py
<OpenAIObject at 0x1fd8de69ea0> JSON: {
  "role": "assistant",
  "content": null,
  "function_call": {
    "name": "get_temperature",
    "arguments": "{\n  \"location\": \"Berlin\",\n  \"date\": \"2023-09-01\"\n}"
  }
}
```

首先需要注意的是，函数调用返回了一个结构化的响应，即一个 JSON 对象。

其次，函数调用从我们的查询中确定了我们希望使用“get_temperature”函数，并相应地解析了该函数的参数（位置和日期）。

这很好，因为我们现在可以将这些参数发送到我们构建的天气 API 并获得有关温度的响应。

```py
# Parse the location and date from our function call response 

def parse_function(response):
    parsed_output = response["function_call"]["arguments"]
    parsed_output = json.loads(parsed_output)
    return parsed_output

location = parse_function(response)['location']
date = parse_function(response)['date']

# Get our weather data

data = get_temperature(location, date)
```

这是我们从天气 API 获得的响应：

```py
'{"location": "Berlin", "date": "2023-09-01", "temerature": 9.72, "scale": "Farenheit"}'
```

***注意****：理解函数调用并不会执行函数。这只是从自然语言输入中解析出函数的参数。*

# 多函数调用

我们甚至可以指定多个函数进行“调用”。当我们有一个自然语言接口覆盖多个 API 时，这尤其有用。让我们通过一个例子来说明。

为了补充我们的天气 API，我创建了一个新闻 [头条 API](https://gist.github.com/john-adeojo/d90431d2d6847c01af3fbe497b1267ad)，根据主题生成模拟新闻头条。

这个主题本身将从自然语言查询中请求，我们可以通过函数调用来解析。

我们可以将其添加到现有的函数调用中。它是这样显示的：

作者脚本：带有两个函数的函数调用示例

由于我们将“function_call”参数设置为“auto”，大语言模型可以仅基于查询推断出最合适的函数进行调用。让我们看看这对两个不同的查询是如何工作的。

> query_1 = "2023 年 9 月底纽约的天气预报是什么？"

查询 1 的响应：

```py
<OpenAIObject at 0x1fd8de6b590> JSON: {
  "role": "assistant",
  "content": null,
  "function_call": {
    "name": "get_temperature",
    "arguments": "{\n  \"location\": \"NYC\",\n  \"date\": \"2023-09-30\"\n}"
  }
}
```

现在让我们对不同的查询运行多功能函数调用。

> query_2 = "给我最新的柏林头条新闻"

查询 2 的响应：

```py
<OpenAIObject at 0x1fd8de67e00> JSON: {
  "role": "assistant",
  "content": null,
  "function_call": {
    "name": "get_current_news",
    "arguments": "{\n  \"topic\": \"Berlin\"\n}"
  }
}
```

在这两种情况下，函数调用根据查询正确选择了获取参数的函数。

# 如果我们不使用它怎么办？

此时，你可能会问自己：“这很好，但我不能通过聊天完成 API 和提示来获得相同的结果吗？我们为什么还需要函数调用？”

简单来说就是结构。虽然聊天完成 API 经过训练以响应用户查询，你可以提示它执行“函数调用”，但你得到的输出可能没有结构，导致解析参数变得困难。

举个例子：

作者脚本：用于解析参数的聊天完成 API

在这里，我们完全跳过了函数调用，仅仅要求聊天完成 API 用一些提示来响应我们的参数。

以及响应：

```py
'The news topic required is "Berlin".'
```

虽然答案是正确的，但由于其无结构性，作为参数解析起来更困难。

还有一致性的问题。你可以通过提示尝试强制某些结构，但响应可能并不总是按你期望的方式构造。

函数调用保证每次都有结构化的响应，使其成为解析参数以供下游函数使用的更可靠方法。

# 结论

函数调用为开发大量基于 LLM 的新应用程序开辟了道路。我们现在可以从无结构数据中一致地生成结构化输出。

如果你想深入了解如何构建你的第一个 LLM 应用程序，请阅读这个：

[如何构建 LLM 应用程序](https://towardsdatascience.com/how-to-build-an-llm-application-360848c957db?source=post_page-----e35bdac88ae7--------------------------------) [## 如何构建 LLM 应用程序

### 使用 Langchain 和 OpenAI 构建以 LLM 为中心的应用程序

[如何构建 LLM 应用程序](https://towardsdatascience.com/how-to-build-an-llm-application-360848c957db?source=post_page-----e35bdac88ae7--------------------------------)

感谢阅读。

完整的笔记本可以通过我的 [GitHub](https://github.com/john-adeojo/function_calling_tutorial) 仓库获得。YouTube 上也有一个直播视频教程 [YouTube](https://youtu.be/VswwV9RJN7s)。

如果你希望提升人工智能技能，可以加入[我的课程](https://www.data-centric-solutions.com/course)的等待名单，在课程中我将指导你开发大语言模型驱动的应用程序。

如果你寻求为你的企业实现人工智能转型，今天就预约一次发现电话会议吧。

[](https://www.brainqub3.com/?source=post_page-----e35bdac88ae7--------------------------------) [## Brainqub3 | 人工智能软件开发

### 在 Brainqub3，我们开发定制的人工智能软件。我们使用最新的人工智能技术创建 qub3s，即先进的人工智能大脑，以…

www.brainqub3.com](https://www.brainqub3.com/?source=post_page-----e35bdac88ae7--------------------------------)

欲了解更多关于人工智能、数据科学和大语言模型的见解，你可以订阅[YouTube](https://www.youtube.com/channel/UCkXe-exqi25V4GnZendgEaA)频道。
