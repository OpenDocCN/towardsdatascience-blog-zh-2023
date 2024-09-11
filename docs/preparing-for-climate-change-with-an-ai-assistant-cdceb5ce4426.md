# 准备应对气候变化与 AI 助手

> 原文：[https://towardsdatascience.com/preparing-for-climate-change-with-an-ai-assistant-cdceb5ce4426?source=collection_archive---------3-----------------------#2023-11-26](https://towardsdatascience.com/preparing-for-climate-change-with-an-ai-assistant-cdceb5ce4426?source=collection_archive---------3-----------------------#2023-11-26)

## 通过对话简化复杂数据

[](https://medium.com/@astrobagel?source=post_page-----cdceb5ce4426--------------------------------)[![Matthew Harris](../Images/4fa3264bb8a028633cd8d37093c16214.png)](https://medium.com/@astrobagel?source=post_page-----cdceb5ce4426--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cdceb5ce4426--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cdceb5ce4426--------------------------------) [Matthew Harris](https://medium.com/@astrobagel?source=post_page-----cdceb5ce4426--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4a2cd25b8ff9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpreparing-for-climate-change-with-an-ai-assistant-cdceb5ce4426&user=Matthew+Harris&userId=4a2cd25b8ff9&source=post_page-4a2cd25b8ff9----cdceb5ce4426---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cdceb5ce4426--------------------------------) ·13分钟阅读·2023年11月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcdceb5ce4426&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpreparing-for-climate-change-with-an-ai-assistant-cdceb5ce4426&user=Matthew+Harris&userId=4a2cd25b8ff9&source=-----cdceb5ce4426---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcdceb5ce4426&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpreparing-for-climate-change-with-an-ai-assistant-cdceb5ce4426&source=-----cdceb5ce4426---------------------bookmark_footer-----------)![](../Images/843a51eb863429365131e0a7bd40f1dc.png)

使用 OpenAI 的 ChatGPT 和 Dall-E-3 生成的图像

摘要

*在这篇文章中，我们探讨了如何使用来自* [*Probable Futures*](https://docs.probablefutures.org/) *API 和新的 OpenAI Assistants API 创建一个对话式 AI 代理。这个 AI 代理能够回答有关气候如何影响特定地点的问题，并且还能进行基本的数据分析。AI 助手非常适合这样的任务，为非技术用户提供了展示复杂数据的有希望的途径。*

我最近和一个邻居聊起了气候变化可能对我们产生的影响，以及如何最好地准备房屋以应对极端天气事件。有一些很棒的网站提供了与此相关的地图信息，但我想知道是否有时候人们只是想问像“***我的家会如何受到气候变化的影响？***”和“***我可以怎么做？***”这样的问题，并获得一个简明的总结和如何准备的建议。所以我决定探索一下最近几周发布的一些 AI 工具。

# OpenAI 的助手 API

由大型语言模型如 [GPT-4](https://openai.com/research/gpt-4) 驱动的 AI 代理正在成为人们通过对话与文档和数据互动的一种方式。这些代理解读人们的提问，调用 API 和数据库以获取数据，生成并运行代码进行分析，然后将结果呈现给用户。像 [langchain](https://www.langchain.com/) 和 [autogen](https://github.com/microsoft/autogen) 这样出色的框架正在引领潮流，提供了轻松实现代理的模式。最近，OpenAI 也加入了这个行列，推出了 [GPTs](https://openai.com/blog/introducing-gpts) 作为一种无代码的创建代理的方法，我在 [这篇文章](https://medium.com/towards-data-science/developing-a-climate-gpt-using-nasas-power-api-37b3d9e2a664) 中探索了这一点。这些设计得非常好，为更广泛的受众打开了大门，但它们确实有一些限制。它们需要一个带有 openapi.json 规范的 API，这意味着它们当前不支持 [graphql](https://graphql.org/) 等标准。它们也不支持注册功能的能力，这对于无代码解决方案来说是可以预期的，但可能限制了它们的能力。

介绍 OpenAI 最近的另一个发布——[助手 API](https://platform.openai.com/docs/assistants/overview)。

助手 API（测试版）是一种编程方式来配置 OpenAI 助手，支持功能、网页浏览和从上传文档中检索知识。与 GPTs 相比，功能是一个很大的区别，因为这些功能使得与外部数据源的互动变得更加复杂。功能是指大型语言模型（LLMs）如 GPT-4 被告知某些用户输入应导致调用代码函数。LLM 会生成一个 JSON 格式的响应，包含调用函数所需的确切参数，然后可以用于本地执行。要详细了解它们如何与 OpenAI 一起工作，请参见 [这里](https://cookbook.openai.com/examples/how_to_call_functions_with_chat_models)。

# 气候变化综合 API——Probable Futures

为了能够创建一个 AI 代理来帮助准备应对气候变化，我们需要一个好的气候变化数据来源和一个 API 来提取这些信息。任何这样的资源都必须采用严格的方法来结合气候模式（GCM）预测。

幸运的是，[Probable Futures](https://probablefutures.org/)的团队做得非常出色！

![](../Images/269fc20807be4eecd19871fb0094ee9a.png)

[Probable Futures](https://probablefutures.org/)提供了与气候变化预测相关的各种资源

Probable Futures是“*一个非营利气候素养倡议，提供在线实用工具、故事和资源，面向所有人，无论身在何处*”，他们提供了一系列基于CORDEX-CORE框架的地图和数据，这是对来自REMO2015和REGCM4区域气候模型的气候模型输出的标准化。[附注：我与Probable Futures没有关联]

重要的是，他们提供了一个[GraphQL API](https://docs.probablefutures.org/calling-the-api/)来访问这些数据，我在[请求API密钥](https://docs.probablefutures.org/api-access/)后可以访问。

根据[文档](https://docs.probablefutures.org/calling-the-api/)我创建了函数，并将其保存在文件`assistant_tools.py`中…

```py
pf_api_url = "https://graphql.probablefutures.org"
pf_token_audience = "https://graphql.probablefutures.com"
pf_token_url = "https://probablefutures.us.auth0.com/oauth/token"

def get_pf_token():
    client_id = os.getenv("CLIENT_ID")
    client_secret = os.getenv("CLIENT_SECRET")
    response = requests.post(
        pf_token_url,
        json={
            "client_id": client_id,
            "client_secret": client_secret,
            "audience": pf_token_audience,
            "grant_type": "client_credentials",
        },
    )
    access_token = response.json()["access_token"]
    return access_token

def get_pf_data(address, country, warming_scenario="1.5"):
    variables = {}

    location = f"""
        country: "{country}"
        address: "{address}"
    """

    query = (
        """
        mutation {
            getDatasetStatistics(input: { """
        + location
        + """ \
                    warmingScenario: \"""" + warming_scenario + """\" 
                }) {
                datasetStatisticsResponses{
                    datasetId
                    midValue
                    name
                    unit
                    warmingScenario
                    latitude
                    longitude
                    info
                }
            }
        }
    """
    )
    print(query)

    access_token = get_pf_token()
    url = pf_api_url + "/graphql"
    headers = {"Authorization": "Bearer " + access_token}
    response = requests.post(
        url, json={"query": query, "variables": variables}, headers=headers
    )
    return str(response.json())
```

我有意排除了`datasetId`以检索所有指标，这样AI代理就能拥有广泛的信息供其使用。

该API非常强大，既可以接受城镇和城市，也可以接受完整地址。例如…

```py
get_pf_data(address="New Delhi", country="India", warming_scenario="1.5")
```

返回包含位置气候变化信息的JSON记录…

```py
{'data': {'getDatasetStatistics': {'datasetStatisticsResponses': [{'datasetId': 40601, 'midValue': '17.0', 'name': 'Change in total annual precipitation', 'unit': 'mm', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40616, 'midValue': '14.0', 'name': 'Change in wettest 90 days', 'unit': 'mm', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40607, 'midValue': '19.0', 'name': 'Change in dry hot days', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40614, 'midValue': '0.0', 'name': 'Change in snowy days', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40612, 'midValue': '2.0', 'name': 'Change in frequency of “1-in-100-year” storm', 'unit': 'x as frequent', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40101, 'midValue': '28.0', 'name': 'Average temperature', 'unit': '°C', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40901, 'midValue': '4.0', 'name': 'Climate zones', 'unit': 'class', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {'climateZoneName': 'Dry semi-arid (or steppe) hot'}}, {'datasetId': 40613, 'midValue': '49.0', 'name': 'Change in precipitation “1-in-100-year” storm', 'unit': 'mm', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40701, 'midValue': '7.0', 'name': 'Likelihood of year-plus extreme drought', 'unit': '%', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40702, 'midValue': '30.0', 'name': 'Likelihood of year-plus drought', 'unit': '%', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40704, 'midValue': '5.0', 'name': 'Change in wildfire danger days', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40703, 'midValue': '-0.2', 'name': 'Change in water balance', 'unit': 'z-score', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40201, 'midValue': '21.0', 'name': 'Average nighttime temperature', 'unit': '°C', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40205, 'midValue': '0.0', 'name': 'Freezing days', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40301, 'midValue': '71.0', 'name': 'Days above 26°C (78°F) wet-bulb', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40302, 'midValue': '24.0', 'name': 'Days above 28°C (82°F) wet-bulb', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40303, 'midValue': '2.0', 'name': 'Days above 30°C (86°F) wet-bulb', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40102, 'midValue': '35.0', 'name': 'Average daytime temperature', 'unit': '°C', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40103, 'midValue': '49.0', 'name': '10 hottest days', 'unit': '°C', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40104, 'midValue': '228.0', 'name': 'Days above 32°C (90°F)', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40105, 'midValue': '187.0', 'name': 'Days above 35°C (95°F)', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40106, 'midValue': '145.0', 'name': 'Days above 38°C (100°F)', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40202, 'midValue': '0.0', 'name': 'Frost nights', 'unit': 'nights', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40304, 'midValue': '0.0', 'name': 'Days above 32°C (90°F) wet-bulb', 'unit': 'days', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40305, 'midValue': '29.0', 'name': '10 hottest wet-bulb days', 'unit': '°C', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40203, 'midValue': '207.0', 'name': 'Nights above 20°C (68°F)', 'unit': 'nights', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}, {'datasetId': 40204, 'midValue': '147.0', 'name': 'Nights above 25°C (77°F)', 'unit': 'nights', 'warmingScenario': '1.5', 'latitude': 28.6, 'longitude': 77.2, 'info': {}}]}}}
```

# 创建一个OpenAI助手

接下来，我们需要使用测试版API构建AI助手。在[文档](https://platform.openai.com/docs/assistants/overview)和非常有用的[OpenAI Cookbook](https://cookbook.openai.com/examples/assistants_api_overview_python)中有一些很好的资源。然而，由于这是新推出的测试版，信息还不是很多，所以有时需要一些试错过程。

首先，我们需要配置助手可以使用的工具，比如获取气候变化数据的功能。参考[文档](https://platform.openai.com/docs/assistants/tools/function-calling)…

```py
 get_pf_data_schema = {
        "name": "get_pf_data",
        "parameters": {
            "type": "object",
            "properties": {
                "address": {
                    "type": "string",
                    "description": ("The address of the location to get data for"),
                },
                "country": {
                    "type": "string",
                    "description": ("The country of location to get data for"),
                },
                "warming_scenario": {
                    "type": "string",
                    "enum": ["1.0", "1.5", "2.0", "2.5", "3.0"],
                    "description": ("The warming scenario to get data for. Default is 1.5"),
                }

            },
            "required": ["address", "country"],
        },
        "description": """
            This is the API call to the probable futures API to get predicted climate change indicators for a location
        """,
    }
```

你会注意到我们为每个参数提供了文本描述。根据实验，这似乎是助手在填充参数时使用的，所以要尽可能清晰，并记录任何特性，以便LLM能够调整。从这些定义工具…

```py
tools = [
    {
        "type": "function",
        "function": get_pf_data_schema,
    }
    {"type": "code_interpreter"},
]
```

你会注意到我保留了code_interpretor，赋予助手运行数据分析所需代码的能力。

接下来，我们需要指定一组用户指令（系统提示）。这些指令在将助手的性能调整到我们的任务上时至关重要。通过一些快速实验，我得出了这一组…

```py
instructions = """ 
    "Hello, Climate Change Assistant. You help people understand how climate change will affect their homes"
    "You will use Probable Futures Data to predict climate change indicators for a location"
    "You will summarize perfectly the returned data"
    "You will also provide links to local resources and websites to help the user prepare for the predicted climate change"
    "If you don't have enough address information, request it"
    "You default to warming scenario of 1.5 if not specified, but ask if the user wants to try others after presenting results"
    "Group results into categories"
    "Always link to the probable futures website for the location using URL and replacing LATITUDE and LONGITUDE with location values: https://probablefutures.org/maps/?selected_map=days_above_32c&map_version=latest&volume=heat&warming_scenario=1.5&map_projection=mercator#9.2/LATITUDE/LONGITUDE"
    "GENERATE OUTPUT THAT IS CLEAR AND EASY TO UNDERSTAND FOR A NON-TECHNICAL USER"
"""
```

你可以看到我添加了指令，要求助手提供如网站等资源，帮助用户为气候变化做准备。这有点‘开放’，对于生产环境的助手，我们可能希望有更严格的筛选。

一件现在可能的奇妙事情是，我们还可以就一般的语气提供指令，在上面的例子中请求输出对非技术用户清晰。显然，所有这些需要一些系统化的提示工程，但有趣的是，我们现在在某种程度上通过劝说来‘编程’。😊

好的，现在我们有了工具和指令，让我们创建助手……

```py
import os
from openai import AsyncOpenAI
import asyncio
from dotenv import load_dotenv
import sys

load_dotenv()

api_key = os.environ.get("OPENAI_API_KEY")
assistant_id = os.environ.get("ASSISTANT_ID")
model = os.environ.get("MODEL")
client = AsyncOpenAI(api_key=api_key)

name = "Climate Change Assistant"
try:
    my_assistant = await client.beta.assistants.retrieve(assistant_id)
    print("Updating existing assistant ...")
    assistant = await client.beta.assistants.update(
        assistant_id,
        name=name,
        instructions=instructions,
        tools=tools,
        model=model,
    )
except:
    print("Creating assistant ...")
    assistant = await client.beta.assistants.create(
        name=name,
        instructions=instructions,
        tools=tools,
        model=model,
    )
    print(assistant)
    print("Now save the DI in your .env file")
```

上述假设我们在`.env`文件中定义了密钥和代理ID。你会注意到代码首先检查是否存在代理，使用`.env`文件中的`ASSISTANT_ID`并进行更新，否则它会创建一个全新的代理，生成的ID必须复制到`.env`文件中。否则，我会创建大量的助手！

一旦创建了助手，它将在[OpenAI用户界面](https://platform.openai.com/assistants)上可见，可以在[Playground](https://platform.openai.com/playground)中进行测试。由于大多数功能调用相关的开发和调试实际上涉及调用代码，我发现Playground对这次分析并不是特别有用，但它设计得很好，可能在其他工作中会有用。

对于这次分析，我决定使用新的[GPT-4-Turbo](https://help.openai.com/en/articles/8555510-gpt-4-turbo)模型，将`model`设置为“gpt-4–1106-preview”。

# 创建用户界面

我们想创建一个完整的聊天机器人，因此我从这个[chainlit食谱示例](https://github.com/Chainlit/cookbook/tree/main/openai-assistant)开始，稍微调整了一下，将代理代码分离到一个专用文件中，并通过……

```py
import assistant_tools as at
```

Chainlit非常简洁，用户界面易于设置，你可以在[这里](https://github.com/datakind/climate-change-assistant/blob/main/app.py)找到应用的代码。

# 尝试我们的气候变化助手AI代理

把这些放在一起——见代码[这里](https://github.com/datakind/climate-change-assistant/tree/main)——我们用简单的`chainlit run app.py`启动代理……

![](../Images/c5919b5b0d6bf74ea521271af0f9fc17.png)

让我们询问一个位置……

![](../Images/532a9253a7abfc03622b330d1699eec2.png)

注意到上述我故意拼写错误了Mombasa。

然后代理开始工作，调用API并处理JSON响应（这大约花了20秒）……

![](../Images/49ed28382e012a30332ceb482eb82ec0.png)

根据我们的指令，它最终完成了……

![](../Images/e759fbfee93d2ea443add0f506ea5191.png)

但这是否正确？

让我们调用API并查看输出……

```py
get_pf_data(address="Mombassa", country="Kenya", warming_scenario="1.5")
```

这会查询API……

```py
mutation {
    getDatasetStatistics(input: { 
            country: "Kenya"
            address: "Mombassa"
            warmingScenario: "1.5" 
        }) {
        datasetStatisticsResponses{
            datasetId
            midValue
            name
            unit
            warmingScenario
            latitude
            longitude
            info
        }
    }
}
```

这给出了以下内容（缩略显示）……

```py
{
  "data": {
    "getDatasetStatistics": {
      "datasetStatisticsResponses": [
        {
          "datasetId": 40601,
          "midValue": "30.0",
          "name": "Change in total annual precipitation",
          "unit": "mm",
          "warmingScenario": "1.5",
          "latitude": -4,
          "longitude": 39.6,
          "info": {}
        },
        {
          "datasetId": 40616,
          "midValue": "70.0",
          "name": "Change in wettest 90 days",
          "unit": "mm",
          "warmingScenario": "1.5",
          "latitude": -4,
          "longitude": 39.6,
          "info": {}
        },
        {
          "datasetId": 40607,
          "midValue": "21.0",
          "name": "Change in dry hot days",
          "unit": "days",
          "warmingScenario": "1.5",
          "latitude": -4,
          "longitude": 39.6,
          "info": {}
        },
        {
          "datasetId": 40614,
          "midValue": "0.0",
          "name": "Change in snowy days",
          "unit": "days",
          "warmingScenario": "1.5",
          "latitude": -4,
          "longitude": 39.6,
          "info": {}
        },
        {
          "datasetId": 40612,
          "midValue": "1.0",
          "name": "Change in frequency of \u201c1-in-100-year\u201d storm",
          "unit": "x as frequent",
          "warmingScenario": "1.5",
          "latitude": -4,
          "longitude": 39.6,
          "info": {}
        },

        .... etc

        }
      ]
    }
  }
}
```

进行抽查时，似乎代理完美地捕捉了它们，并向用户提供了准确的总结。

# 通过指令改善可用性

AI代理可以通过一些指令来改进其信息呈现方式。

其中一项指令是始终生成一个返回到Probable Futures网站的地图可视化链接，点击后会转到正确的位置……

![](../Images/282e7fcb1c67dde0e6635cbcaf7951e9.png)

代理始终生成一个URL，将用户带到正确的[地图可视化](https://probablefutures.org/maps/?selected_map=days_above_32c&map_version=latest&volume=heat&warming_scenario=1.5&map_projection=mercator#9.2/-4/39.6)上，以便在可能的未来网站上进行查询。

另一个指令要求代理始终提示用户尝试其他变暖情景。默认情况下，代理会生成预测全球温度升高1.5摄氏度的结果，但我们允许用户探索其他——有些令人沮丧的——情景。

# 分析任务

既然我们给了AI代理代码解释器技能，它应该能够执行Python代码来进行基本的数据分析。让我们尝试一下。

首先我询问了气候变化如何影响伦敦和纽约，代理提供了总结。然后我问了……

![](../Images/a33e331f4044ea5a8dfd0ee42a1cf01c.png)

结果是代理使用代码解释器生成并运行Python代码以创建一个图表……

![](../Images/1060482040ee01dc6b1981bae17f1cdb.png)

AI代理能够使用从API提取的气候变化数据进行基本的数据分析任务。

不错！

# 结论与未来工作

通过使用Probable Futures API和OpenAI助手，我们能够创建一个对话界面，展示人们如何能够询问有关气候变化的问题并获得如何准备的建议。代理能够进行API调用以及一些基本的数据分析。这提供了另一种气候意识的渠道，这可能对一些非技术用户更具吸引力。

我们当然可以开发一个聊天机器人来确定意图/实体，并编写代码处理API，但这需要更多工作，并且在API发生更改或添加新API时需要重新审视。此外，大型语言模型代理在解释用户输入和总结方面表现良好，并且可以运行代码和进行基本数据分析。我们的特定用例似乎特别适合AI代理，因为任务范围有限。

不过也有一些挑战，这个技术有点慢（查询完成大约需要20-30秒）。此外，LLM令牌费用未在本文中分析，可能会很高。

也就是说，OpenAI Assistants API仍处于测试阶段。此外，代理没有经过任何调整，因此通过进一步的工作，额外的常用功能、性能和成本可能会针对这一令人兴奋的新技术进行优化。

# 参考文献

*本文基于Probable Futures提供的数据和其他内容，Probable Futures是SouthCoast Community Foundation的一个项目，部分数据可能由Woodwell Climate Research Center, Inc.或The Coordinated Regional Climate Downscaling Experiment (CORDEX)提供*

这个分析的代码可以在[这里](https://github.com/datakind/climate-change-assistant/tree/main)找到。

你可以在[这里](https://medium.com/@astrobagel)找到更多我的文章。
