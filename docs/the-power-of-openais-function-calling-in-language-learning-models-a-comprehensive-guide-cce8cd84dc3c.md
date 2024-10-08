# OpenAI 的函数调用在数据管道中的力量：全面指南

> 原文：[`towardsdatascience.com/the-power-of-openais-function-calling-in-language-learning-models-a-comprehensive-guide-cce8cd84dc3c`](https://towardsdatascience.com/the-power-of-openais-function-calling-in-language-learning-models-a-comprehensive-guide-cce8cd84dc3c)

## 利用 OpenAI 的函数调用功能改造数据管道：使用 PostgreSQL 和 FastAPI 实现电子邮件发送工作流

[](https://medium.com/@luisroque?source=post_page-----cce8cd84dc3c--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----cce8cd84dc3c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cce8cd84dc3c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cce8cd84dc3c--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----cce8cd84dc3c--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cce8cd84dc3c--------------------------------) ·阅读时间 11 分钟·2023 年 6 月 22 日

--

# 介绍

AI 的激动人心的世界通过 OpenAI 最新的大型语言模型（LLMs）引入的函数调用能力又迈进了一步。这个新功能增强了人类与 AI 之间的互动，将其从简单的问答形式转变为更具动态性和主动性的对话。

那么这些函数调用能力到底是什么呢？本质上，它们允许 LLM 在对话中根据输入指令调用预定义的函数。这可以是任何操作，从发送电子邮件到根据对话的上下文从数据库中提取数据。

使用函数调用能力的好处和应用广泛。它显著提升了 AI 在各种应用中的动态效用，从客户服务到数据管道的构建。

想象一下一个客户服务 AI，它可以回答问题并执行诸如预约、向电子邮件地址发送信息或实时更新客户详情等操作。或者考虑一个数据管道，其中 LLM 代理可以根据命令获取、更新和处理数据。

在接下来的章节中，我们将进一步探讨这些应用，并提供一个逐步指南，介绍如何通过构建一个发送电子邮件的管道来利用这一新能力。

![](img/55187b6c2b616d891dc92fe4e5836f25.png)

图 1：LLMs 正成为我们可以合作的智能代理（[图片来源](https://unsplash.com/photos/cbRuQDMNr4E)）

一如既往，代码可以在我的 [Github](https://github.com/luisroque/large_laguage_models) 上找到。

# 理解新的函数调用能力并与 LangChain 进行比较

我们想要了解 OpenAI 新引入的函数调用功能在 AI 竞赛中带来了什么。为此，让我们深入了解它与市场上其他工具和框架（如 LangChain）的区别。我们在本系列的第一篇文章中已经介绍了 LangChain。它是一个用于开发 AI 驱动应用程序的流行框架。函数调用功能和 LangChain 带来了独特的优势和能力，旨在使 AI 更加可用、多样和动态。

我们已经知道，函数调用功能为 AI 应用程序增加了一层额外的互动性。它使模型能够在对话中调用预定义的函数，增强了 LLM 的活力和反应能力。这个新功能可能会简化将功能添加到 AI 应用程序的过程。开发者需要定义这些函数，然后模型可以根据上下文在对话中执行它们。这里的主要优势是与 OpenAI 模型的直接集成，这便于使用、快速设置，并且降低了熟悉 OpenAI 生态系统的开发者的学习曲线。

另一方面，LangChain 提供了一个全面且多功能的框架，旨在开发更复杂、数据感知和自主的 AI 应用程序。它允许语言模型与其环境互动，并根据高级指令做出决策。其模块提供了构建应用程序的抽象和标准接口，包括模型、提示、记忆、索引、链、代理和回调。

LangChain 的方法促进了构建应用程序，其中 LLM 可以在不同的交互中保持其状态，顺序调用和链式调用不同的实用程序，甚至与外部数据源互动。我们可以将这些视为增强的函数调用能力。因此，它对开发者特别有用，尤其是那些构建复杂、多步骤应用程序并利用语言模型的开发者。增加的复杂性带来的缺点是使用起来的学习曲线更陡峭。

# 用例 — 转换数据管道

在我看来，数据管道是 LLMs 新函数调用能力最令人兴奋的应用领域之一。数据管道是任何数据驱动组织中一个关键组件，它负责收集、处理和分发数据。通常，这些过程是静态的、预定义的，并且需要人工干预以进行任何更改或更新。这正是我们上面讨论的 LLM 动态行为创造机会的地方。

传统上，数据库查询需要特定的查询语言知识，如 SQL。借助 LLM 直接调用函数、服务和数据库的能力，用户可以通过对话检索数据，而不需要显式的查询表述。LLM 可以将用户的请求转换为数据库查询，获取数据，并以用户友好的格式实时返回。这一功能可以使组织内的不同角色都能平等地访问数据。

另一个可能发生变化的方面是数据转换。数据转换通常需要在分析之前进行单独的数据清理和处理步骤。LLM 可以通过根据用户的指示交互式地执行数据清理和操作任务来简化这一过程。此外，在对话中处理实时数据允许进行更多的探索性和迭代性数据分析。

第三个用例是数据监控。它涉及定期检查以确保数据管道中数据的准确性和一致性。借助 LLM，监控任务可以变得更加互动和高效。例如，LLM 可以在对话中提醒用户数据不一致，并立即采取纠正措施。

最后，LLM 还可以自动创建和分发数据报告。用户可以指示 LLM 根据特定标准生成报告，LLM 可以获取数据，创建报告，甚至将其发送给相关的接收者。

# 利用 OpenAI 函数调用能力构建电子邮件发送管道

我们的目标是创建一个发送电子邮件给用户的流程。虽然这听起来很简单，但这个过程的美在于由 LLM 控制的不同组件之间的相互作用。AI 模型不仅仅生成电子邮件正文；它还动态地与数据库互动以检索用户信息，撰写上下文适当的邮件，然后指示服务发送邮件。

我们的管道由三个主要组件组成：PostgreSQL、FastAPI 和 OpenAI LLM。我们使用 PostgreSQL 存储用户数据。这些数据包括用户名及其关联的电子邮件地址。它作为我们用户信息的真实来源。FastAPI 是一个现代、高性能的 Python Web 框架，用于构建 API。我们使用 FastAPI 服务来模拟发送电子邮件的过程。当服务接收到发送电子邮件的请求时，它会返回一个确认电子邮件已发送的响应。LLM 作为整个过程的协调者。它控制对话，根据上下文确定必要的操作，与 PostgreSQL 数据库互动以获取用户信息，撰写电子邮件内容，并指示 FastAPI 服务发送电子邮件。

## 实施 PostgreSQL 数据库

我们管道的第一个组件是 PostgreSQL 数据库，我们在其中存储用户数据。使用 Docker 设置 PostgreSQL 实例变得简单且可重复，Docker 是一个允许我们容器化和隔离数据库环境的平台。

你首先需要安装 Docker 来设置 PostgreSQL Docker 容器。安装后，你可以拉取 PostgreSQL 镜像并将其作为容器运行。我们将容器的端口 5432 映射到主机的端口 5432，以访问数据库。在生产环境中，请将密码设置为环境变量，不要像下面这样直接在命令中设置。我们这样做是为了加快我们的过程。

```py
docker run --name user_db -e POSTGRES_PASSWORD=testpass -p 5432:5432 -d postgres
```

在我们的 PostgreSQL 实例运行后，我们现在可以创建一个数据库和一个表来存储我们的用户数据。我们将使用一个初始化脚本来创建一个 `users` 表，其中包含 `username` 和 `email` 列，并用一些虚拟数据填充它。这个脚本被放置在一个目录中，然后映射到容器中的 `/docker-entrypoint-initdb.d` 目录。PostgreSQL 在启动时执行此目录下的任何脚本。以下是脚本（`user_init.sql`）的样子：

```py
CREATE DATABASE user_database;
\c user_database;

CREATE TABLE users (
    username VARCHAR(50),
    email VARCHAR(50)
);

INSERT INTO users (username, email) VALUES
    ('user1', 'user1@example.com'),
    ('user2', 'user2@example.com'),
    ('user3', 'user3@example.com'),
    ...
    ('user10', 'user10@example.com');
```

LLM 能够理解 SQL 命令并用于查询 PostgreSQL 数据库。当 LLM 收到涉及获取用户数据的请求时，它可以生成一个 SQL 查询来从数据库中获取所需的数据。

例如，如果你要求 LLM 发送一封电子邮件给 `user10`，LLM 可以生成如下查询：

```py
SELECT  email FROM users WHERE username=’user10'; 
```

这使它能够从 `users` 表中获取 `user10` 的电子邮件地址。LLM 然后可以使用这个电子邮件地址指示 FastAPI 服务发送电子邮件。

在下一部分，我们将指导你实现发送电子邮件的 FastAPI 服务。

## 创建 FastAPI 电子邮件服务

我们的第二个组件是 FastAPI 服务。该服务将模拟发送电子邮件的过程。这是一个直接的 API，接收包含收件人姓名、电子邮件和电子邮件正文的 POST 请求。它将返回一个确认电子邮件已发送的响应。我们将再次使用 Docker 确保我们的服务是隔离和可重复的。

首先，你需要安装 Docker（如果尚未安装）。然后，创建一个新的目录用于你的 FastAPI 服务并进入该目录。在这里，创建一个新的 Python 文件（例如，`main.py`）并添加以下代码：

```py
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    email: str
    body: str

@app.post("/send_email")
async def send_email(user: User):
    return {
        "message": f"Email successfully sent to {user.name} with email {user.email}. Email body:\n\n{user.body}"
    }
```

这段代码定义了一个 FastAPI 应用程序，包含一个单独的端点 `/send_email/`。该端点接受 POST 请求，并期望一个包含收件人姓名、电子邮件和电子邮件正文的 JSON 数据。

接下来，在相同的目录中创建一个 Dockerfile，内容如下：

```py
FROM python:3.9-slim-buster

WORKDIR /app
ADD . /app

RUN pip install --no-cache-dir fastapi uvicorn

EXPOSE 1000

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "1000"]
```

这个 Dockerfile 指示 Docker 创建一个基于 `python:3.9-slim-buster` 镜像的镜像，这是一个理想的轻量级镜像，用于高效运行 Python 应用程序。然后，它将我们的 `main.py` 文件复制到镜像中的 `/app/` 目录下。

你可以使用以下命令构建 Docker 镜像：

```py
docker build -t fastapi_email_service .
```

然后运行它：

```py
docker run -d -p 1000:1000 fastapi_email_service
```

LLM 使用 POST 请求与 FastAPI 服务进行交互。当 LLM 决定发送电子邮件时，它生成对 `send_email` 函数的函数调用。此函数调用的参数包含名称、电子邮件和电子邮件正文。

函数调用由我们的 Python 脚本处理，该脚本提取函数参数，并用它们向我们的 FastAPI 服务发送 POST 请求。FastAPI 服务回应一个消息，指示电子邮件已成功发送。

现在，我们已经有了管道的所有组件。下一部分将把所有内容串联在一起，解释 LLM 如何协调 PostgreSQL 数据库和 FastAPI 服务之间的交互以发送电子邮件。

## 与 OpenAI LLM 集成

我们管道的最后一部分是 OpenAI LLM 集成。LLM 作为协调者，解释我们的命令，查询数据库以获取用户信息，并指示 FastAPI 服务发送电子邮件。

我们的脚本使用 OpenAI 的 API 进行基于聊天的完成。每个完成请求由一系列消息和可选的函数规格列表组成，模型可以调用这些函数。我们以用户消息开始对话，这为助手提供了提示。

这是我们用来向 API 发送请求的 `chat_completion_request` 函数：

```py
@retry(wait=wait_random_exponential(min=1, max=40), stop=stop_after_attempt(3))
def chat_completion_request(messages, functions=None, model=GPT_MODEL):
    headers = {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + openai.api_key,
    }
    json_data = {"model": model, "messages": messages}
    if functions is not None:
        json_data.update({"functions": functions})

    response = requests.post(
        "https://api.openai.com/v1/chat/completions",
        headers=headers,
        json=json_data,
    )
    return response
```

我们使用 `Chat` 类来管理对话历史。它有方法可以将新消息添加到历史记录中，并显示整个对话：

```py
class Chat:
    def __init__(self):
        self.conversation_history = []

    def add_prompt(self, role, content):
        message = {"role": role, "content": content}
        self.conversation_history.append(message)

    def display_conversation(self):
        for message in self.conversation_history:
            print(f"{message['role']}: {message['content']}")
```

在我们的使用案例中，LLM 需要与我们的 PostgreSQL 数据库和 FastAPI 服务进行交互。我们定义这些函数并将其包含在我们的完成请求中。这是我们如何定义 `sql_query_email` 和 `send_email` 函数的：

```py
functions = [
    {
        "name": "send_email",
        "description": "Send a new email",
        "parameters": {
            "type": "object",
            "properties": {
                "to": {
                    "type": "string",
                    "description": "The destination email.",
                },
                "name": {
                    "type": "string",
                    "description": "The name of the person that will receive the email.",
                },
                "body": {
                    "type": "string",
                    "description": "The body of the email.",
                },
            },
            "required": ["to", "name", "body"],
        },
    },
    {
        "name": "sql_query_email",
        "description": "SQL query to get user emails",
        "parameters": {
            "type": "object",
            "properties": {
                "query": {
                    "type": "string",
                    "description": "The query to get users emails.",
                },
            },
            "required": ["query"],
        },
    },
]
```

当我们发出完成请求时，LLM 以其预期的操作作出响应。如果响应包括函数调用，我们的脚本会执行该函数。例如，如果 LLM 决定调用 `sql_query_email` 函数，我们的脚本会从数据库中检索用户的电子邮件，然后将结果添加到对话历史记录中。当调用 `send_email` 函数时，我们的脚本会使用 FastAPI 服务发送电子邮件。

我们脚本的主循环检查 LLM 响应中的函数调用并相应地采取行动：

```py
chat = Chat()
chat.add_prompt("user", "Send an email to user10 saying that he needs to pay the monthly subscription fee.")
result_query = ''

for i in range(2):
    chat_response = chat_completion_request(
        chat.conversation_history,
        functions=functions
    )
    response_content = chat_response.json()['choices'][0]['message']

    if 'function_call' in response_content:
        if response_content['function_call']['name'] == 'send_email':
            res = json.loads(response_content['function_call']['arguments'])
            send_email(res['name'], res['to'], res['body'])
            break
        elif response_content['function_call']['name'] == 'sql_query_email':
            result_query = query_db(json.loads(response_content['function_call']['arguments'])['query'])
            chat.add_prompt('user', str(result_query))
    else:
        chat.add_prompt('assistant', response_content['content'])
```

当我们运行脚本时，我们得到以下输出：

```py
{
  "message": "Email successfully sent to User 10 with email user10@example.com.",
  "Email body": "\n\nDear User 10, \n\nThis is a reminder that your monthly subscription fee is due. Please make the payment as soon as possible to ensure uninterrupted service. Thank you for your cooperation. \n\nBest regards, \nYour Subscription Service Team"
}
```

让我们解析一下我们得到这个输出的过程。我们的提示是 *“给 user10 发送一封邮件，说明他需要支付每月订阅费。”* 请注意，我们的消息中没有关于 `user10` 的电子邮件信息。LLM 识别到缺失的信息，并明白我们的函数 `query_email` 可以从数据库中获取该用户的电子邮件。在获取电子邮件后，它再次正确地完成了两件事：首先，它生成了电子邮件的正文，其次，它调用 `send_email` 函数来触发通过 FastAPI 邮件服务发送电子邮件。

# 结论

本文通过实施一个案例研究探讨了函数调用功能，其中 LLM 协调了一个涉及 PostgreSQL 数据库和 FastAPI 电子邮件服务的管道。LLM 成功地完成了从数据库中检索用户电子邮件并指示电子邮件服务发送个性化消息的任务，所有这些都是对单一提示的响应。

在 AI 模型中，函数调用的影响可能是巨大的，开启了自动化和简化流程的新可能性。数据管道可能会从静态和工程密集型的状态转变为动态实体，使得非技术用户能够通过自然语言快速获取最新数据。

# 关于我

连续创业者和 AI 领域的领军人物。我为企业开发 AI 产品，并投资于专注于 AI 的初创公司。

[创始人 @ ZAAI](http://zaai.ai) | [LinkedIn](https://www.linkedin.com/in/luisbrasroque/) | [X/Twitter](https://x.com/luisbrasroque)

# **大语言模型编年史：探索 NLP 前沿**

本文属于“**大语言模型编年史：探索 NLP 前沿**”系列文章的第一篇，这是一个新的每周系列，将探讨如何利用大型模型的力量来处理各种 NLP 任务。通过深入研究这些前沿技术，我们旨在赋能开发者、研究人员和爱好者，利用 NLP 的潜力，解锁新的可能性。

迄今为止发布的文章：

1.  [使用 ChatGPT 总结最新的 Spotify 发布](https://medium.com/towards-data-science/summarizing-the-latest-spotify-releases-with-chatgpt-553245a6df88)

1.  [掌握大规模语义搜索：使用 FAISS 和句子变换器以闪电般的推理速度索引百万级文档](https://medium.com/towards-data-science/master-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88)

1.  [释放音频数据的力量：使用 Whisper、WhisperX 和 PyAnnotate 进行高级转录和分话](https://medium.com/towards-data-science/unlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281)

1.  [Whisper JAX 与 PyTorch：揭示 ASR 在 GPU 上的性能真相](https://medium.com/towards-data-science/whisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5)

1.  [Vosk 高效企业级语音识别：评估与实施指南](https://medium.com/towards-data-science/vosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c)

1.  [测试支持 1162 种语言的大规模多语言语音（MMS）模型](https://medium.com/towards-data-science/testing-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602)

1.  [利用最强大的开源 LLM——Falcon 40B 模型](https://medium.com/towards-data-science/harnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10)
