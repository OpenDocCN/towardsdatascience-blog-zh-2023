# 打破界限：探索 LLM 的函数调用

> 原文：[`towardsdatascience.com/breaking-boundaries-exploring-function-calling-for-llms-73d063d46fcb`](https://towardsdatascience.com/breaking-boundaries-exploring-function-calling-for-llms-73d063d46fcb)

## 函数调用如何为大型语言模型（LLM）与外部工具和 API 的无缝集成铺平道路

[](https://dpoulopoulos.medium.com/?source=post_page-----73d063d46fcb--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----73d063d46fcb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----73d063d46fcb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----73d063d46fcb--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----73d063d46fcb--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----73d063d46fcb--------------------------------) ·阅读时间 8 分钟·2023 年 8 月 10 日

--

![](img/c2db4889afc4841fc06a779401048f6c.png)

使用 SDXL 生成的图像

当我发现大型语言模型（LLM）具备与外部工具和 API 互动的能力时，我知道一切将不再相同。

这是否使我们更接近实现人工通用智能（AGI）？也许没有，但它无疑开启了人工智能的全新时代：一个 LLM 可以执行任何你能放入函数中的任务的时代。现在，这可能看起来有些异端，但对我来说，这使我们更接近我对 AGI 的愿景，因为我不是那种幻想有意识机器或其他科幻叙事的人。

想象一下这样一个场景：一个人工智能代理完全掌控一个任务，与你在线运行的其他代理进行沟通，获取数据，并返回你所期望的结果。这种变革性的能力不仅重新定义了我们与互联网的互动方式，而且还重塑了我们的思维过程。

快进几年：假设你想计划一次假期。第一步不是在网上搜索票务，而是指示一个 AI 代理从头到尾规划和组织一切。你只有在收到几封确认邮件时才会知道任务完成了——如果邮件仍然存在的话——并且你的信用卡上显示了一笔费用。

但我们不要走得太远。即使在我们当前的现实中，你也可以创建能够执行任何程序上可能的任务的代理。不过，这需要你付出一些努力。你需要熟悉 OpenAI 模型系列的新函数调用能力，并创建模型将使用的自定义工具。

这就是这篇博客文章的用武之地。把它当作一个动手指南，帮助你将 AI 代理变为现实——一个可以在你的个人 Google Calendar 账户上创建事件的代理。然而，通过简单地更改 LLM 调用的函数，世界将由你掌控。准备好深入了吗？

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=function-calling) 是一本面向对 ML 和 MLOps 世界感兴趣的人的通讯。如果你想了解更多类似的主题，请 [订阅](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=function-calling)。你将在每个月的最后一个星期天收到我的更新和关于最新 MLOps 新闻和文章的想法！

# 工具包

我们待办事项的首要任务是构建工具，即我们的 LLM 将调用的函数来创建新的日历事件。我们的选择武器？LangChain——可能是发展最快的 Python 库。

LangChain 已经配备了大量现成的 [工具](https://python.langchain.com/docs/integrations/tools/)。然而，它的武器库中没有提供将 LLM 与 Google Calendar API 集成的工具。但没关系——我们自己来创建！

我们从深入了解 Google Calendar Event [参考指南](https://developers.google.com/calendar/api/v3/reference/events) 开始。这有助于我们概述函数签名的样子。但请记住，我们并不需要为每一项提供值，所以让我们标记出我们认为最关键的参数，并定义函数的输入。小提示：LangChain 期望我们使用 Pydantic 模型来实现这一点。

```py
from pydantic import BaseModel, Field

class CalendarEventInput(BaseModel):
    summary: str = Field(description="The title of the event")
    location: str = Field(description="The location of the event")
    description: str = Field(description="The description of the event")
    start: EventDateTime = Field(description="The start datetime of the event")
    end: EventDateTime = Field(description="The end datetime of the event")
    attendees: List[Attendee] = Field(description="The attendees of the event")
    reminders: Reminders = Field(description="The reminders of the event")
    conferenceDataVersion: int = Field(
        description="Set to `1` if you need to create a new Google Meet link.")
    recurrence: List[str] = Field(
        description="A list of RRULE, EXRULE, RDATE and EXDATE lines for a"
                    " recurring event, as specified in RFC5545")
```

正如你所看到的，我们期望我们的模型提供事件的各种细节：摘要、位置、参与者、开始和结束时间等等。

这些字段有些是简单的类型，如字符串或列表，而其他字段则要求我们挽起袖子定义 Python 类。那么，事不宜迟，让我们深入探讨下一步：

```py
from typing import List
from dataclasses import dataclass

@dataclass()
class EventDateTime:
    dateTime: str
    timeZone: str

@dataclass()
class Attendee:
    displayName: str
    email: str
    optional: bool

@dataclass()
class ReminderOverride:
    method: str
    minutes: str

@dataclass()
class Reminders:
    useDefault: bool
    overrides: List[ReminderOverride]
```

很好，我们已经准备好函数模型。接下来一步——制作自定义工具。我们的任务是创建一个继承自 LangChain 的 `BaseTool` 类的类，至少需要实现 `_run` 或 `_arun` 方法。

我们还需要设置一些关键值，如工具的 `name`、`description`（提示模型何时使用它），以及 `args_schema` 属性，后者描述了工具的模式——我们之前定义的模式。让我们开始吧：

```py
from langchain.tools.base import BaseTool
from googleapiclient.discovery import build

class CalendarEventTool(BaseTool):
    name: str = "calendar_event"
    description: str = "Useful tool for creating new Google calendar events"
    args_schema: Type[BaseModel] = CalendarEventInput

    def _create_event(self, calendar_id: str, body: dict,
                      conferenceDataVersion: int):
        """Create a new Google calendar event.

        Args:
            calendar_id (str): The calendar id.
            body (str): The event body.
            conferenceDataVersion (int): Set to `1` to create a new Google
                Meet Event.

        Returns:
            dict: The event response.
        """
        service = build("calendar", "v3", credentials=get_credentials())
        event = (service.events()  # type: ignore
                        .insert(calendarId=calendar_id, body=body,
                                conferenceDataVersion=conferenceDataVersion)
                        .execute())

        return event

    def _run(self, summary: str, location: str, description: str,
             start: EventDateTime, end: EventDateTime,
             attendees: List[Attendee], reminders: Reminders,
             conferenceDataVersion: int, recurrence: List[str]):
        """Run the CalendarEventTool with the given parameters.

        Args:
            summary (str): The summary or title of the event.
            location (str): The location of the event.
            description (str): The description or details of the event.
            start (EventDateTime): The start date and time of the event.
            end (EventDateTime): The end date and time of the event.
            attendees (List[Attendee]): A list of attendees for the event.
            reminders (Reminders): The reminders for the event.
            conferenceDataVersion (int): The version of the conference data.
            recurrence (List[str]): A list of recurrence rules for the event.
        """
        body = create_request_body(summary, location, description,
                                   start, end, attendees, reminders,
                                   recurrence)
        event = self._create_event(CALENDAR_ID, body, conferenceDataVersion)

    def _arun(self):
        raise NotImplementedError("calendar_event does not support async")
```

我们的工具非常简单。我们只定义了 `_run` 函数，因为我们不需要为这个任务支持异步调用。`_run` 函数的作用是组装请求体并调用另一个私有函数，恰如其分地命名为 `_create_event`。

让我们逐步处理。首先，我们需要定义 `create_request_body` 实用函数：

```py
import random
import string

from typing import List

def _create_attendee_list(attendees):
    attendee_list = [{"displayName": attendee.displayName,
                      "email": attendee.email,
                      "optional": attendee.optional}
                     for attendee in attendees]
    return attendee_list

def _create_reminder_list(reminders):
    reminder_list = {"useDefault": reminders.useDefault,
                     "overrides": [{"method": override.method,
                                    "minutes": override.minutes}
                                   for override in reminders.overrides]}
    return reminder_list

def create_request_body(summary: str, location: str, description: str,
                        start: EventDateTime, end: EventDateTime,
                        attendees: List[Attendee], reminders: Reminders,
                        recurrence: List[str]) -> dict:
    attendee_list = _create_attendee_list(attendees)
    reminder_list = _create_reminder_list(reminders)
    request_id = ''.join(random.choice(string.ascii_letters) for _ in range(8))

    body = {
        "summary": summary,
        "location": location,
        "description": description,
        "start": {
            "dateTime": start.dateTime,
            "timeZone": start.timeZone
        },
        "end": {
            "dateTime": end.dateTime,
            "timeZone": end.timeZone
        },
        "attendees": attendee_list,
        "reminders": reminder_list,
        "conferenceData": {
            "createRequest": {
                "requestId": request_id,
                "conferenceSolutionKey": {
                    "type": "hangoutsMeet"
                }
            }
        },
        "recurrence": [r for r in recurrence]}

    return body
```

我们的`create_request_body`函数依赖于几个其他的实用函数来创建 JSON 对象。我们在它上面几行定义了这些私有函数。

我们的下一个目标？`_create_event`函数。这个函数构建了我们将用于执行 API 调用的服务。为了完成这项工作，你需要获取一个凭据文件。不确定怎么做？只需遵循这个指南：[OAuth 客户端 ID 凭据](https://developers.google.com/workspace/guides/create-credentials#oauth-client-id)。下载 JSON 文件后，将其重命名为`credentials.json`，然后使用下面的函数来获取和存储令牌：

```py
import os

from typing import Union
from pathlib import Path

import google.oauth2.credentials as oauth2
import google.auth.external_account_authorized_user as auth

from google.oauth2.credentials import Credentials
from google.auth.transport.requests import Request
from google_auth_oauthlib.flow import InstalledAppFlow

HOME = Path.home()
TOKEN_FILE = "token.json"
CREDS_FILE = "credentials.json"
SCOPES = ["https://www.googleapis.com/auth/calendar"]

def get_credentials() -> Union[oauth2.Credentials, auth.Credentials]:
    creds = None

    # If token.json exists, read it and check if it's valid
    if os.path.exists(TOKEN_FILE):
        creds = Credentials.from_authorized_user_file(TOKEN_FILE, SCOPES)

    # If there's no valid token.json, refresh it or create a new one
    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(
                CREDS_FILE, SCOPES)
            creds = flow.run_local_server(port=0)
        with open(TOKEN_FILE, "w") as token:
            token.write(creds.to_json())

    return creds
```

将所有这些代码整合在一起应该能为你提供所需的工具。这看起来可能有点复杂，但这不是你自己无法做到的，对吧？

现在，让我们换个方向，创建一个可以有效利用这个工具的 AI 代理。

# 代理

LangChain 确实消除了这一部分的猜测。你只需初始化一个新的`[OPENAI_FUNCTIONS](https://python.langchain.com/docs/modules/agents/agent_types/openai_functions_agent)`类型的代理。接下来，你需要为这个代理提供可以访问的工具，然后在用户查询上运行它。听起来很激动人心？那就让我们直接进入吧：

```py
from langchain.chat_models import ChatOpenAI
from langchain.agents import AgentType, initialize_agent

# Initialize the language model
llm = ChatOpenAI(temperature=0, model_name="gpt-4-0613")  # type: ignore

# Define the list of tools that the agent can use
tools = [CalendarEventTool()]

# Initialize the agent
agent = initialize_agent(tools, llm,
                         agent=AgentType.OPENAI_FUNCTIONS,
                         verbose=True)

user_input = input(f"{Emojis.ASSISTANT} How can I assist you today?\n"
                   f"{Emojis.USER} > ")
agent.run(input=user_input)
```

最后但同样重要的是，是时候将这一切付诸实践了。运行你的 Python 文件，并尝试一个测试查询，例如：

> “为我与 John 的会议创建一个新的事件，时间定在 8 月 10 日中午（雅典/希腊）。别忘了邀请 John（john@example.com）并设置一个在线会议室。”

这可能会非常顺利，不过你可能需要一些提示工程。但是你做到了！现在，你已经具备了创建可以执行任何你能编写代码的 AI 代理的能力。这有多令人兴奋？

# 结论

在这次激动人心的旅程中，我们探索了 OpenAI 的大型语言模型（LLMs）如何与外部工具和 API 集成。我们*深入探讨*了这一改变游戏规则的特性如何重新定义我们与 AI 和互联网的互动，承诺未来 AI 代理可以无缝地执行任何可以编程定义的任务。

从理解函数签名的重要参数到定义 Python 类，我们采取了实践的方法来创建一个可以与 Google 日历 API 交互的模型，使用了一个快速发展的 Python 库 LangChain。

我们的尝试结果是一个能够在个人 Google 日历上创建事件的 AI 代理。然而，你现在应该能够创建一个可以执行任何你能放入函数中的任务的代理。

AI 的未来以及它与数字世界的互动前景光明，随着我们继续利用 AI 的力量，我们可以实现的可能性是无限的。让我们看看你接下来会构建什么！

# 关于作者

我的名字是 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=function-calling)，我是一名在 [HPE](https://www.hpe.com/us/en/home.html) 工作的机器学习工程师。我为主要客户如欧盟委员会、国际货币基金组织、欧洲中央银行、宜家、Roblox 等设计和实施了人工智能和软件解决方案。

如果你对阅读更多关于机器学习、深度学习、数据科学和数据操作的文章感兴趣，可以在 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow)、[LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或在 Twitter 上关注 [@james2pl](https://twitter.com/james2pl)。

所表达的观点仅代表我个人，并不反映我雇主的观点或意见。
