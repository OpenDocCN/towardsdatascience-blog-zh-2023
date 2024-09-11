# 使用 OpenAI 和 FastAPI 构建记忆微服务的对话代理

> 原文：[https://towardsdatascience.com/building-a-conversational-agent-with-memory-microservice-with-openai-and-fastapi-5d0102bc8df9?source=collection_archive---------1-----------------------#2023-08-17](https://towardsdatascience.com/building-a-conversational-agent-with-memory-microservice-with-openai-and-fastapi-5d0102bc8df9?source=collection_archive---------1-----------------------#2023-08-17)

![](../Images/2cd55f0d3ad54800a9951729f2d331c1.png)

充满记忆的对话，照片由 [Juri Gianfrancesco](https://unsplash.com/@jurigianfra?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供。

## 制作上下文感知的对话代理：深入探讨 OpenAI 和 FastAPI 的集成

[](https://medium.com/@cfloressuazo?source=post_page-----5d0102bc8df9--------------------------------)[![Cesar Flores](../Images/4345d9053171db08f1afbedcb32a1006.png)](https://medium.com/@cfloressuazo?source=post_page-----5d0102bc8df9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d0102bc8df9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5d0102bc8df9--------------------------------) [Cesar Flores](https://medium.com/@cfloressuazo?source=post_page-----5d0102bc8df9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F37afeaaf9b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-conversational-agent-with-memory-microservice-with-openai-and-fastapi-5d0102bc8df9&user=Cesar+Flores&userId=37afeaaf9b9a&source=post_page-37afeaaf9b9a----5d0102bc8df9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d0102bc8df9--------------------------------) · 30 分钟阅读 · 2023年8月17日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5d0102bc8df9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-conversational-agent-with-memory-microservice-with-openai-and-fastapi-5d0102bc8df9&user=Cesar+Flores&userId=37afeaaf9b9a&source=-----5d0102bc8df9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5d0102bc8df9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-conversational-agent-with-memory-microservice-with-openai-and-fastapi-5d0102bc8df9&source=-----5d0102bc8df9---------------------bookmark_footer-----------)

# 介绍

在本教程中，我们将探索使用 OpenAI 和 FastAPI 创建具有内存微服务的对话代理的过程。对话代理已成为各种应用程序中的关键组件，包括客户支持、虚拟助手和信息检索系统。然而，许多传统的聊天机器人实现缺乏在对话过程中保留上下文的能力，导致功能有限和令人沮丧的用户体验。这在遵循微服务架构构建代理服务时尤其具有挑战性。

> GitHub 仓库的链接在文章底部。

## 动机

本教程的动机是解决传统聊天机器人实现的局限性，并创建一个具有内存微服务的对话代理，这在将代理部署到像 Kubernetes 这样的复杂环境中时尤为重要。在 Kubernetes 或类似的容器编排系统中，微服务经常经历重启、更新和扩展操作。在这些事件中，传统聊天机器人的对话状态将丢失，导致断裂的互动和糟糕的用户体验。

通过构建具有内存微服务的对话代理，我们可以确保在微服务重启或更新时，甚至在交互不连续的情况下，重要的对话上下文得以保留。这种状态的保存使代理能够无缝地继续之前的对话，保持连贯性，并提供更自然和个性化的用户体验。此外，这种方法符合现代应用开发的最佳实践，其中容器化的微服务通常与其他组件交互，使得内存微服务在这种分布式设置中成为对话代理架构中的有价值的补充。

# 我们将使用的技术栈

对于这个项目，我们将主要使用以下技术和工具：

1.  OpenAI GPT-3.5：我们将利用 OpenAI 的 GPT-3.5 语言模型，该模型能够执行各种自然语言处理任务，包括文本生成、对话管理和上下文保留。我们需要生成一个 OpenAI API 密钥，请确保访问此 [URL](https://platform.openai.com/account/api-keys) 以管理您的密钥。

1.  FastAPI：FastAPI 将作为我们微服务的骨干，提供处理 HTTP 请求、管理对话状态和与 OpenAI API 集成的基础设施。FastAPI 非常适合用 Python 构建微服务。

# 开发周期

在本节中，我们将深入探讨构建具有内存微服务的对话代理的逐步过程。开发周期将包括：

1.  环境设置：我们将创建一个虚拟环境并安装必要的依赖项，包括 OpenAI 的 Python 库和 FastAPI。

1.  设计记忆微服务：我们将概述记忆微服务的架构和设计，该服务将负责存储和管理对话上下文。

1.  集成OpenAI：我们将把OpenAI的GPT-3.5模型集成到我们的应用中，并定义处理用户消息和生成响应的逻辑。

1.  测试：我们将逐步测试我们的对话代理。

## 环境设置

对于这个设置，我们将使用以下结构来构建微服务。这对于在同一个项目下扩展其他服务非常方便，而且我个人喜欢这种结构。

```py
├── Dockerfile <--- Container
├── requirements.txt <--- Libraries and Dependencies
├── setup.py <--- Build and distribute microservices as Python packages
└── src
    ├── agents <--- Name of your Microservice
    │   ├── __init__.py
    │   ├── api
    │   │   ├── __init__.py
    │   │   ├── routes.py
    │   │   └── schemas.py
    │   ├── crud.py
    │   ├── database.py
    │   ├── main.py
    │   ├── models.py
    │   └── processing.py
    └── agentsfwrk <--- Name of your Common Framework
        ├── __init__.py
        ├── integrations.py
        └── logger.py
```

我们需要在项目中创建一个名为`src`的文件夹，其中将包含服务的Python代码；在我们的例子中，`agents`包含与对话代理和API相关的所有代码，`agentsfwrk`是我们用于跨服务的通用框架。

`Dockerfile`包含构建镜像的指令，一旦代码准备好，`requirements.txt`包含我们项目中使用的库，`setup.py`包含构建和分发项目的指令。

目前，只需创建服务文件夹以及`__init__.py`文件，并将以下内容添加到项目根目录的`requirements.txt`和`setup.py`中，`Dockerfile`保持空白，我们将在部署周期部分回到它。

```py
# Requirements.txt
fastapi==0.95.2
ipykernel==6.22.0
jupyter-bokeh==2.0.2
jupyterlab==3.6.3
openai==0.27.6
pandas==2.0.1
sqlalchemy-orm==1.2.10
sqlalchemy==2.0.15
uvicorn<0.22.0,>=0.21.1
```

```py
# setup.py
from setuptools import find_packages, setup

setup(
    name = 'conversational-agents',
    version = '0.1',
    description = 'microservices for conversational agents',
    packages = find_packages('src'),
    package_dir = {'': 'src'},
    # This is optional btw
    author = 'XXX XXXX',
    author_email = 'XXXX@XXXXX.ai',
    maintainer = 'XXX XXXX',
    maintainer_email = 'XXXX@XXXXX.ai',
)
```

让我们激活虚拟环境，并在终端运行`pip install -r requirements.txt`。我们暂时不会运行setup文件，所以接下来进入下一部分。

## 设计通用框架

我们将设计我们的通用框架，以便在项目中构建的所有微服务中使用。这对小型项目来说不是严格必要的，但考虑到未来，你可以扩展它以使用多个LLM提供商，添加与自己数据交互的其他库（例如[LangChain](https://python.langchain.com/)，[VoCode](https://docs.vocode.dev/what-is-vocode)），以及其他通用功能，如语音和图像服务，而无需在每个微服务中实现它们。

创建文件夹和文件时请遵循`agentsfwrk`结构。每个文件及其描述如下：

```py
└── agentsfwrk <--- Name of your Common Framework
    ├── __init__.py
    ├── integrations.py
    └── logger.py
```

日志记录器是一个非常基础的工具，用于设置通用日志模块，你可以按如下方式定义它：

```py
import logging
import multiprocessing
import sys

APP_LOGGER_NAME = 'CaiApp'

def setup_applevel_logger(logger_name = APP_LOGGER_NAME, file_name = None):
    """
    Setup the logger for the application
    """
    logger = logging.getLogger(logger_name)
    logger.setLevel(logging.DEBUG)
    formatter = logging.Formatter("%(asctime)s - %(name)s - %(levelname)s - %(message)s")
    sh = logging.StreamHandler(sys.stdout)
    sh.setFormatter(formatter)
    logger.handlers.clear()
    logger.addHandler(sh)
    if file_name:
        fh = logging.FileHandler(file_name)
        fh.setFormatter(formatter)
        logger.addHandler(fh)

    return logger

def get_multiprocessing_logger(file_name = None):
    """
    Setup the logger for the application for multiprocessing
    """
    logger = multiprocessing.get_logger()
    logger.setLevel(logging.DEBUG)
    formatter = logging.Formatter("%(asctime)s - %(name)s - %(levelname)s - %(message)s")

    sh = logging.StreamHandler(sys.stdout)
    sh.setFormatter(formatter)

    if not len(logger.handlers):
        logger.addHandler(sh)

    if file_name:
        fh = logging.FileHandler(file_name)
        fh.setFormatter(formatter)
        logger.addHandler(fh)

    return logger

def get_logger(module_name, logger_name = None):
    """
    Get the logger for the module
    """
    return logging.getLogger(logger_name or APP_LOGGER_NAME).getChild(module_name)
```

接下来，我们的集成层通过集成模块完成。此文件充当微服务逻辑与OpenAI之间的中介，并设计为以统一的方式向我们的应用程序公开LLM提供商。在这里，我们可以实现处理异常、错误、重试和请求或响应超时的通用方法。我从一位非常优秀的经理那里学到，要始终在外部服务/API和我们应用的内部世界之间放置一个集成层。

集成代码定义如下：

```py
# integrations.py
# LLM provider common module
import json
import os
import time
from typing import Union

import openai
from openai.error import APIConnectionError, APIError, RateLimitError

import agentsfwrk.logger as logger

log = logger.get_logger(__name__)

openai.api_key = os.getenv('OPENAI_API_KEY')

class OpenAIIntegrationService:
    def __init__(
        self,
        context: Union[str, dict],
        instruction: Union[str, dict]
    ) -> None:

        self.context = context
        self.instructions = instruction

        if isinstance(self.context, dict):
            self.messages = []
            self.messages.append(self.context)

        elif isinstance(self.context, str):
            self.messages = self.instructions + self.context

    def get_models(self):
        return openai.Model.list()

    def add_chat_history(self, messages: list):
        """
        Adds chat history to the conversation.
        """
        self.messages += messages

    def answer_to_prompt(self, model: str, prompt: str, **kwargs):
        """
        Collects prompts from user, appends to messages from the same conversation
        and return responses from the gpt models.
        """
        # Preserve the messages in the conversation
        self.messages.append(
            {
                'role': 'user',
                'content': prompt
            }
        )

        retry_exceptions = (APIError, APIConnectionError, RateLimitError)
        for _ in range(3):
            try:
                response = openai.ChatCompletion.create(
                    model       = model,
                    messages    = self.messages,
                    **kwargs
                )
                break
            except retry_exceptions as e:
                if _ == 2:
                    log.error(f"Last attempt failed, Exception occurred: {e}.")
                    return {
                        "answer": "Sorry, I'm having technical issues."
                    }
                retry_time = getattr(e, 'retry_after', 3)
                log.error(f"Exception occurred: {e}. Retrying in {retry_time} seconds...")
                time.sleep(retry_time)

        response_message = response.choices[0].message["content"]
        response_data = {"answer": response_message}
        self.messages.append(
            {
                'role': 'assistant',
                'content': response_message
            }
        )

        return response_data

    def answer_to_simple_prompt(self, model: str, prompt: str, **kwargs) -> dict:
        """
        Collects context and appends a prompt from a user and return response from
        the gpt model given an instruction.
        This method only allows one message exchange.
        """

        messages = self.messages + f"\n<Client>: {prompt} \n"

        retry_exceptions = (APIError, APIConnectionError, RateLimitError)
        for _ in range(3):
            try:
                response = openai.Completion.create(
                    model = model,
                    prompt = messages,
                    **kwargs
                )
                break
            except retry_exceptions as e:
                if _ == 2:
                    log.error(f"Last attempt failed, Exception occurred: {e}.")
                    return {
                        "intent": False,
                        "answer": "Sorry, I'm having technical issues."
                    }
                retry_time = getattr(e, 'retry_after', 3)
                log.error(f"Exception occurred: {e}. Retrying in {retry_time} seconds...")
                time.sleep(retry_time)

        response_message = response.choices[0].text

        try:
            response_data = json.loads(response_message)
            answer_text = response_data.get('answer')
            if answer_text is not None:
                self.messages = self.messages + f"\n<Client>: {prompt} \n" + f"<Agent>: {answer_text} \n"
            else:
                raise ValueError("The response from the model is not valid.")
        except ValueError as e:
            log.error(f"Error occurred while parsing response: {e}")
            log.error(f"Prompt from the user: {prompt}")
            log.error(f"Response from the model: {response_message}")
            log.info("Returning a safe response to the user.")
            response_data = {
                "intent": False,
                "answer": response_message
            }

        return response_data

    def verify_end_conversation(self):
        """
        Verify if the conversation has ended by checking the last message from the user
        and the last message from the assistant.
        """
        pass

    def verify_goal_conversation(self, model: str, **kwargs):
        """
        Verify if the conversation has reached the goal by checking the conversation history.
        Format the response as specified in the instructions.
        """
        messages = self.messages.copy()
        messages.append(self.instructions)

        retry_exceptions = (APIError, APIConnectionError, RateLimitError)
        for _ in range(3):
            try:
                response = openai.ChatCompletion.create(
                    model       = model,
                    messages    = messages,
                    **kwargs
                )
                break
            except retry_exceptions as e:
                if _ == 2:
                    log.error(f"Last attempt failed, Exception occurred: {e}.")
                    raise
                retry_time = getattr(e, 'retry_after', 3)
                log.error(f"Exception occurred: {e}. Retrying in {retry_time} seconds...")
                time.sleep(retry_time)

        response_message = response.choices[0].message["content"]
        try:
            response_data = json.loads(response_message)
            if response_data.get('summary') is None:
                raise ValueError("The response from the model is not valid. Missing summary.")
        except ValueError as e:
            log.error(f"Error occurred while parsing response: {e}")
            log.error(f"Response from the model: {response_message}")
            log.info("Returning a safe response to the user.")
            raise

        return response_data
```

关于集成模块的一些说明：

+   OpenAI密钥被定义为名为“OPENAI_API_KEY”的环境变量，我们应该下载这个密钥并在终端中定义它，或使用[python-dotenv](https://pypi.org/project/python-dotenv/)库。

+   有两种方法可以与GPT模型集成，一种用于聊天端点（`answer_to_prompt`），另一种用于完成端点（`answer_to_simple_prompt`）。我们将专注于第一个的使用。

+   有一种方法来检查对话的目标——`verify_goal_conversation`，它简单地遵循代理的指示并生成总结。

## 设计（内存）微服务

最佳练习是设计并绘制一个图表来可视化服务需要做的事情，包括参与者及其在与服务交互时的行动。我们从简单地描述我们的应用程序开始：

+   我们的微服务是一个人工智能代理的提供者，这些代理在某一主题上是专家，预计会根据外部消息和后续提示进行对话。

+   我们的代理可以进行多次对话，并且包含需要持久化的内存，这意味着它们必须能够保留对话历史记录，无论与代理交互的客户端会话如何。

+   代理在创建时应接收清晰的指示，说明如何处理对话并在对话过程中做出相应响应。

+   对于程序化集成，代理也应遵循预期的响应格式。

我们的设计如下图所示：

![](../Images/066900bb9a87cfd3c504036ad45705be.png)

对话代理设计——作者提供的图像

通过这个简单的图表，我们知道我们的微服务需要实现负责这些特定任务的方法：

1.  代理的创建 & 指令的定义

1.  对话启动器 & 对话历史记录的保存

1.  与代理聊天

我们将按照顺序编写这些功能，在此之前我们将构建应用程序的骨架。

## 应用程序骨架

为了启动开发，我们首先构建FastAPI应用程序骨架。应用程序骨架包括基本组件，如主要应用程序脚本、数据库配置、处理脚本和路由模块。主要脚本作为应用程序的入口点，我们在此处设置FastAPI实例。

**主要文件**

在你的`agents`文件夹中创建/打开`main.py`文件并输入以下代码，该代码简单地定义了一个根端点。

```py
from fastapi import FastAPI

from agentsfwrk.logger import setup_applevel_logger

log = setup_applevel_logger(file_name = 'agents.log')

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello there conversational ai user!"}
```

**数据库配置**

然后我们创建/打开名为`database.py`的数据库配置脚本，该脚本建立与本地数据库的连接，用于存储和检索对话上下文。我们将首先使用本地SQLite以简化操作，但可以根据你的环境尝试其他数据库。

```py
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///agents.db"

engine = create_engine(
    SQLALCHEMY_DATABASE_URL, connect_args = {"check_same_thread": False}
)
SessionLocal = sessionmaker(autocommit = False, autoflush = False, bind = engine)

Base = declarative_base()
```

**API 路由**

最后，我们定义处理传入HTTP请求的路由模块，涵盖处理用户交互的端点。让我们创建`api`文件夹，创建/打开`routes.py`文件，并粘贴以下代码。

```py
from typing import List

from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.orm import Session

import agents.api.schemas
import agents.models
from agents.database import SessionLocal, engine

from agentsfwrk import integrations, logger

log = logger.get_logger(__name__)

agents.models.Base.metadata.create_all(bind = engine)

# Router basic information
router = APIRouter(
    prefix = "/agents",
    tags = ["Chat"],
    responses = {404: {"description": "Not found"}}
)

# Dependency: Used to get the database in our endpoints.
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# Root endpoint for the router.
@router.get("/")
async def agents_root():
    return {"message": "Hello there conversational ai!"}
```

有了这个结构化的骨架，我们已经准备好开始编写我们设计的应用程序。

## 创建代理并分配指令

在本节中，我们将重点实现“**创建代理**”端点。此端点使用户能够启动新的对话并与代理互动，提供上下文和一组指令，以便代理在整个对话过程中遵循。我们将首先介绍两个数据模型：一个用于数据库，另一个用于API。我们将使用[Pydantic](https://docs.pydantic.dev/latest/)来创建数据模型。创建/打开`schemas.py`文件，并定义Agent base、Agent Create和Agent数据模型。

```py
from datetime import datetime
from typing import List, Optional
from pydantic import BaseModel

class AgentBase(BaseModel): # <-- Base model
    context: str # <-- Our agents context
    first_message: str # <-- Our agents will approach the users with a first message.
    response_shape: str # <-- The expected shape (for programatic communication) of the response of each agent's interaction with the user
    instructions: str # <-- Set of instructions that our agent should follow.

class AgentCreate(AgentBase): # <-- Creation data model
    pass

class Agent(AgentBase): # <-- Agent data model
    id: str
    timestamp: datetime = datetime.utcnow()

    class Config:
        orm_mode = True
```

agent数据模型中的字段如下所述：

+   **上下文**：这是代理的整体背景。

+   **首条消息**：我们的代理旨在与用户开始对话。这可以简单到“你好，我可以帮你做什么？”或者类似“嗨，你请求一个代理来帮助你找到有关股票的信息，对吗？”。

+   **响应格式**：该字段主要用于指定代理响应的输出格式，并应用于将LLM的文本输出转换为所需的格式，以便进行程序化通信。例如，我们可能希望指定我们的代理应该将响应包装在一个名为`response`的JSON格式中，即`{'response': "string"}`。

+   **指令**：该字段包含每个代理在整个对话过程中应遵循的指令和指南，例如“在每次交互中收集以下实体 *[e1, e2, e3, …]*”或“回复用户直到他不再对对话感兴趣”或“不要偏离主题，并在必要时将对话引导回主要目标”。

我们现在继续打开`models.py`文件，在其中编写属于agent实体的数据库表。

```py
from sqlalchemy import Column, ForeignKey, String, DateTime, JSON
from sqlalchemy.orm import relationship
from datetime import datetime

from agents.database import Base

class Agent(Base):
    __tablename__ = "agents"

    id          = Column(String, primary_key = True, index = True)
    timestamp   = Column(DateTime, default = datetime.utcnow)

    context            = Column(String, nullable = False)
    first_message      = Column(String, nullable = False)
    response_shape     = Column(JSON,   nullable = False)
    instructions       = Column(String, nullable = False)
```

这段代码与Pydantic模型非常相似，它定义了我们数据库中的代理表。

在我们有了两个数据模型后，我们准备好实现代理的创建。为此，我们将首先修改`routes.py`文件，添加端点：

```py
@router.post("/create-agent", response_model = agents.api.schemas.Agent)
async def create_agent(campaign: agents.api.schemas.AgentCreate, db: Session = Depends(get_db)):
    """
    Create an agent
    """
    log.info(f"Creating agent")
    # db_agent = create_agent(db, agent)
    log.info(f"Agent created with id: {db_agent.id}")

    return db_agent
```

我们需要创建一个新函数，该函数接收来自请求的Agent对象，并将其保存到数据库中。为此，我们将创建/打开`crud.py`文件，该文件将包含所有与数据库的交互**（创建、读取、更新、删除）**。

```py
# crud.py
import uuid
from sqlalchemy.orm import Session
from agents import models
from agents.api import schemas

def create_agent(db: Session, agent: schemas.AgentCreate):
    """
    Create an agent in the database
    """
    db_agent = models.Agent(
        id              = str(uuid.uuid4()),
        context         = agent.context,
        first_message   = agent.first_message,
        response_shape  = agent.response_shape,
        instructions    = agent.instructions
    )
    db.add(db_agent)
    db.commit()
    db.refresh(db_agent)

    return db_agent
```

创建完函数后，我们现在可以回到`routes.py`，导入`crud`模块，并在端点方法中使用它。

```py
import agents.crud

@router.post("/create-agent", response_model = agents.api.schemas.Agent)
async def create_agent(agent: agents.api.schemas.AgentCreate, db: Session = Depends(get_db)):
    """
    Create an agent endpoint.
    """
    log.info(f"Creating agent: {agent.json()}")
    db_agent = agents.crud.create_agent(db, agent)
    log.info(f"Agent created with id: {db_agent.id}")

    return db_agent
```

现在让我们回到`main.py`文件，添加“agents”路由。修改

```py
# main.py
from fastapi import FastAPI

from agents.api.routes import router as ai_agents # NOTE: <-- new addition
from agentsfwrk.logger import setup_applevel_logger

log = setup_applevel_logger(file_name = 'agents.log')

app = FastAPI()
app.include_router(router = ai_agents) # NOTE: <-- new addition

@app.get("/")
async def root():
    return {"message": "Hello there conversational ai user!"}
```

让我们测试一下这个功能。首先，我们需要将我们的服务安装为 Python 包，其次，在 8000 端口启动应用程序。

```py
# Run from the root of the project.
$ pip install -e .
# Command to run the app.
$ uvicorn agents.main:app --host 0.0.0.0 --port 8000 --reload
```

访问 [http://0.0.0.0:8000/docs](http://0.0.0.0:8000/docs)，你将看到带有测试端点的 Swagger UI。提交你的负载并检查输出。

![](../Images/fccecb2f4b76bfdbc7bd9b27f97f9044.png)

create-agent 端点来自 Swagger UI — 图片由作者提供

我们将继续开发我们的应用程序，但测试第一个端点是进展的良好标志。

## 创建对话 & 保留对话历史

我们的下一步是允许用户与我们的代理进行交互。我们希望用户能够与特定的代理进行互动，因此我们需要传递代理的 ID 以及用户的第一次互动消息。让我们对 Agent 数据模型进行一些修改，通过引入 `Conversation` 实体，使每个代理能够进行多个对话。打开 `schemas.py` 文件并添加以下模型：

```py
class ConversationBase(BaseModel): # <-- base of our conversations, they must belong to an agent
    agent_id: str

class ConversationCreate(ConversationBase): # <-- conversation creation object
    pass

class Conversation(ConversationBase): # <-- The conversation objects
    id: str
    timestamp: datetime = datetime.utcnow()

    class Config:
        orm_mode = True

class Agent(AgentBase): # <-- Agent data model
    id: str
    timestamp: datetime = datetime.utcnow()
    conversations: List[Conversation] = [] # <-- NOTE: we have added the conversation as a list of Conversations objects.

    class Config:
        orm_mode = True
```

请注意，我们已经修改了 `Agent` 数据模型，并添加了对话功能，以便每个代理可以根据我们的图表设计进行多个对话。

我们需要修改我们的数据库对象，并在数据库模型脚本中包含对话表。我们将打开 `models.py` 文件，并按如下方式修改代码：

```py
# models.py

class Agent(Base):
    __tablename__ = "agents"

    id          = Column(String, primary_key = True, index = True)
    timestamp   = Column(DateTime, default = datetime.utcnow)

    context            = Column(String, nullable = False)
    first_message      = Column(String, nullable = False)
    response_shape     = Column(JSON,   nullable = False)
    instructions       = Column(String, nullable = False)

    conversations      = relationship("Conversation", back_populates = "agent") # <-- NOTE: We add the conversation relationship into the agents table

class Conversation(Base):
    __tablename__ = "conversations"

    id          = Column(String, primary_key = True, index = True)
    agent_id    = Column(String, ForeignKey("agents.id"))
    timestap    = Column(DateTime, default = datetime.utcnow)

    agent       = relationship("Agent", back_populates = "conversations") # <-- We add the relationship between the conversation and the agent
```

请注意我们在 `agents` 表中为每个代理添加了对话之间的关系，以及在 `conversations` 表中对话与代理之间的关系。

我们现在将创建一组 CRUD 函数，以通过它们的 ID 检索代理和对话，这将帮助我们制定创建对话和保留对话历史的过程。让我们打开 `crud.py` 文件并添加以下函数：

```py
def get_agent(db: Session, agent_id: str):
    """
    Get an agent by its id
    """
    return db.query(models.Agent).filter(models.Agent.id == agent_id).first()

def get_conversation(db: Session, conversation_id: str):
    """
    Get a conversation by its id
    """
    return db.query(models.Conversation).filter(models.Conversation.id == conversation_id).first()

def create_conversation(db: Session, conversation: schemas.ConversationCreate):
    """
    Create a conversation
    """
    db_conversation = models.Conversation(
        id          = str(uuid.uuid4()),
        agent_id    = conversation.agent_id,
    )
    db.add(db_conversation)
    db.commit()
    db.refresh(db_conversation)

    return db_conversation
```

这些新函数将帮助我们在应用程序的正常工作流程中，现在我们可以通过 ID 获取代理，通过 ID 获取对话，并通过提供可选的 ID 和应持有对话的代理 ID 来创建对话。

我们可以继续创建一个创建对话的端点。打开 `routes.py` 并添加以下代码：

```py
@router.post("/create-conversation", response_model = agents.api.schemas.Conversation)
async def create_conversation(conversation: agents.api.schemas.ConversationCreate, db: Session = Depends(get_db)):
    """
    Create a conversation linked to an agent
    """
    log.info(f"Creating conversation assigned to agent id: {conversation.agent_id}")
    db_conversation = agents.crud.create_conversation(db, conversation)
    log.info(f"Conversation created with id: {db_conversation.id}")

    return db_conversation
```

在这个方法准备好后，我们仍然离拥有实际的对话端点还差一步，我们将在下一节中进行回顾。

在初始化代理时，重要的是要做出区分，我们可以创建一个对话而不触发双向消息交换，另一种方式是当调用“**与代理聊天**”端点时触发对话的创建。这为在微服务外部组织工作流提供了一些灵活性，在某些情况下，你可能想初始化代理，提前启动与客户的对话，并随着消息的到来开始保留消息的历史记录。

![](../Images/3d0777673ee2498ca60aac063e7d35d5.png)

create-conversation 端点来自 Swagger UI — 图片由作者提供

> **重要提示：** 如果您按照本指南逐步操作，并且在此步骤中看到与数据库模式相关的错误，请注意，这是因为我们在每次修改模式时都未将迁移应用到数据库，因此请确保关闭应用程序（退出终端命令）并删除在运行时创建的`agents.db`文件。 您需要重新运行每个端点并记录ID。

## 与代理人聊天

我们现在要介绍我们应用程序中的最后一个实体类型，即`Message`实体。 这个实体负责建模客户消息和代理消息之间的交互（消息的双向交换）。 我们还将添加用于定义端点响应结构的API数据模型。 让我们先创建数据模型和API响应类型; 打开`schemas.py`文件，并修改代码：

```py
##########################################
# Internal schemas
##########################################
class MessageBase(BaseModel): # <-- Every message is composed by user/client message and the agent 
    user_message: str
    agent_message: str

class MessageCreate(MessageBase):
    pass

class Message(MessageBase): # <-- Data model for the Message entity
    id: str
    timestamp: datetime = datetime.utcnow()
    conversation_id: str

    class Config:
        orm_mode = True

##########################################
# API schemas
##########################################
class UserMessage(BaseModel):
    conversation_id: str
    message: str

class ChatAgentResponse(BaseModel):
    conversation_id: str
    response: str
```

现在我们必须在代表数据库中表的数据库模型脚本中添加数据模型。 打开`models.py`文件并修改如下：

```py
# models.py

class Conversation(Base):
    __tablename__ = "conversations"

    id          = Column(String, primary_key = True, index = True)
    agent_id    = Column(String, ForeignKey("agents.id"))
    timestap    = Column(DateTime, default = datetime.utcnow)

    agent       = relationship("Agent", back_populates = "conversations")
    messages    = relationship("Message", back_populates = "conversation") # <-- We define the relationship between the conversation and the multiple messages in them.

class Message(Base):
    __tablename__ = "messages"

    id          = Column(String, primary_key = True, index = True)
    timestamp   = Column(DateTime, default = datetime.utcnow)

    user_message    = Column(String)
    agent_message   = Column(String)

    conversation_id = Column(String, ForeignKey("conversations.id")) # <-- A message belongs to a conversation
    conversation    = relationship("Conversation", back_populates = "messages") # <-- We define the relationship between the messages and the conversation.
```

请注意，我们已修改了`Conversations`表以定义消息与会话之间的关系，并创建了一个新表，表示应属于对话的交互（消息交换）。

现在我们将向数据库添加一个新的CRUD函数，以与数据库交互并为对话创建消息。 打开`crud.py`文件并添加以下函数：

```py
def create_conversation_message(db: Session, message: schemas.MessageCreate, conversation_id: str):
    """
    Create a message for a conversation
    """
    db_message = models.Message(
        id              = str(uuid.uuid4()),
        user_message    = message.user_message,
        agent_message   = message.agent_message,
        conversation_id = conversation_id
    )
    db.add(db_message)
    db.commit()
    db.refresh(db_message)

    return db_message
```

现在我们准备构建最终和最有趣的端点，**chat-agent**端点。 打开`routes.py`文件，并按照代码进行操作，因为我们将在途中实施一些处理函数。

```py
@router.post("/chat-agent", response_model = agents.api.schemas.ChatAgentResponse)
async def chat_completion(message: agents.api.schemas.UserMessage, db: Session = Depends(get_db)):
    """
    Get a response from the GPT model given a message from the client using the chat
    completion endpoint.

    The response is a json object with the following structure:
    ```

    {

        `"conversation_id": "string",

        `"response": "string"

    }

    ```py
    """
    log.info(f"User conversation id: {message.conversation_id}")
    log.info(f"User message: {message.message}")

    conversation = agents.crud.get_conversation(db, message.conversation_id)

    if not conversation:
        # If there are no conversations, we can choose to create one on the fly OR raise an exception.
        # Which ever you choose, make sure to uncomment when necessary.

        # Option 1:
        # conversation = agents.crud.create_conversation(db, message.conversation_id)

        # Option 2:
        return HTTPException(
            status_code = 404,
            detail = "Conversation not found. Please create conversation first."
        )

    log.info(f"Conversation id: {conversation.id}")
```

在端点的这一部分中，我们确保在对话不存在时创建或引发异常。 下一步是准备数据，将其通过我们的集成发送到OpenAI，为此，我们将在`processing.py`文件中创建一组处理函数，这些函数将从LLM中制作上下文，第一条消息，说明和预期的响应形状。

```py
# processing.py

import json

########################################
# Chat Properties
########################################
def craft_agent_chat_context(context: str) -> dict:
    """
    Craft the context for the agent to use for chat endpoints.
    """
    agent_chat_context = {
        "role": "system",
        "content": context
    }
    return agent_chat_context

def craft_agent_chat_first_message(content: str) -> dict:
    """
    Craft the first message for the agent to use for chat endpoints.
    """
    agent_chat_first_message = {
        "role": "assistant",
        "content": content
    }
    return agent_chat_first_message

def craft_agent_chat_instructions(instructions: str, response_shape: str) -> dict:
    """
    Craft the instructions for the agent to use for chat endpoints.
    """
    agent_instructions = {
        "role": "user",
        "content": instructions + f"\n\nFollow a RFC8259 compliant JSON with a shape of: {json.dumps(response_shape)} format without deviation."
    }
    return agent_instructions
```

注意最后一个函数期望在代理人创建过程中定义的`response_shape`，此输入将在对话过程中附加到LLM，并指导代理人遵循指南并将响应作为JSON对象返回。

让我们返回`routes.py`文件并完成我们的端点实现：

```py
# New imports from the processing module.
from agents.processing import (
  craft_agent_chat_context,
  craft_agent_chat_first_message,
  craft_agent_chat_instructions
)

@router.post("/chat-agent", response_model = agents.api.schemas.ChatAgentResponse)
async def chat_completion(message: agents.api.schemas.UserMessage, db: Session = Depends(get_db)):
    """
    Get a response from the GPT model given a message from the client using the chat
    completion endpoint.

    The response is a json object with the following structure:
    ```

    {

        `"conversation_id": "string",

        `"response": "string"

    }

    ```py
    """
    log.info(f"User conversation id: {message.conversation_id}")
    log.info(f"User message: {message.message}")

    conversation = agents.crud.get_conversation(db, message.conversation_id)

    if not conversation:
        # If there are no conversations, we can choose to create one on the fly OR raise an exception.
        # Which ever you choose, make sure to uncomment when necessary.

        # Option 1:
        # conversation = agents.crud.create_conversation(db, message.conversation_id)

        # Option 2:
        return HTTPException(
            status_code = 404,
            detail = "Conversation not found. Please create conversation first."
        )

    log.info(f"Conversation id: {conversation.id}")

    # NOTE: We are crafting the context first and passing the chat messages in a list
    # appending the first message (the approach from the agent) to it.
    context = craft_agent_chat_context(conversation.agent.context)
    chat_messages = [craft_agent_chat_first_message(conversation.agent.first_message)]

    # NOTE: Append to the conversation all messages until the last interaction from the agent
    # If there are no messages, then this has no effect.
    # Otherwise, we append each in order by timestamp (which makes logical sense).
    hist_messages = conversation.messages
    hist_messages.sort(key = lambda x: x.timestamp, reverse = False)
    if len(hist_messages) > 0:
        for mes in hist_messages:
            log.info(f"Conversation history message: {mes.user_message} | {mes.agent_message}")
            chat_messages.append(
                {
                    "role": "user",
                    "content": mes.user_message
                }
            )
            chat_messages.append(
                {
                    "role": "assistant",
                    "content": mes.agent_message
                }
            )
    # NOTE: We could control the conversation by simply adding
    # rules to the length of the history.
    if len(hist_messages) > 10:
        # Finish the conversation gracefully.
        log.info("Conversation history is too long, finishing conversation.")
        api_response = agents.api.schemas.ChatAgentResponse(
            conversation_id = message.conversation_id,
            response        = "This conversation is over, good bye."
        )
        return api_response

    # Send the message to the AI agent and get the response
    service = integrations.OpenAIIntegrationService(
        context = context,
        instruction = craft_agent_chat_instructions(
            conversation.agent.instructions,
            conversation.agent.response_shape
        )
    )
    service.add_chat_history(messages = chat_messages)

    response = service.answer_to_prompt(
        # We can test different OpenAI models.
        model               = "gpt-3.5-turbo",
        prompt              = message.message,
        # We can test different parameters too.
        temperature         = 0.5,
        max_tokens          = 1000,
        frequency_penalty   = 0.5,
        presence_penalty    = 0
    )

    log.info(f"Agent response: {response}")

    # Prepare response to the user
    api_response = agents.api.schemas.ChatAgentResponse(
        conversation_id = message.conversation_id,
        response        = response.get('answer')
    )

    # Save interaction to database
    db_message = agents.crud.create_conversation_message(
        db = db,
        conversation_id = conversation.id,
        message = agents.api.schemas.MessageCreate(
            user_message = message.message,
            agent_message = response.get('answer'),
        ),
    )
    log.info(f"Conversation message id {db_message.id} saved to database")

    return api_response
```

Voilà！ 这是我们最终的端点实现，如果我们查看代码中添加的**Notes**，我们会发现这个过程非常简单：

1.  我们确保在我们的数据库中存在对话（或者我们创建一个）

1.  我们从数据库中制作上下文和指导代理人

1.  我们通过获取代理人的对话历史来利用代理人的“记忆”

1.  最后，我们通过 OpenAI 的 GPT-3.5 Turbo 模型请求代理的响应，并将响应返回给客户端。

## 本地测试我们的代理

现在我们准备测试微服务的完整工作流，我们将首先进入终端，输入 `uvicorn agents.main:app — host 0.0.0.0 — port 8000 — reload` 启动应用程序。接下来，我们将通过访问 [http://0.0.0.0:8000/docs](http://0.0.0.0:8000/docs) 进入 Swagger UI 并提交以下请求：

+   创建代理：提供你想测试的有效负载。我将提交以下内容：

```py
{
    "context": "You are a chef specializing in Mediterranean food that provides receipts with a maximum of simple 10 ingredients. The user can have many food preferences or ingredient preferences, and your job is always to analyze and guide them to use simple ingredients for the recipes you suggest and these should also be Mediterranean. The response should include detailed information on the recipe. The response should also include questions to the user when necessary. If you think your response may be inaccurate or vague, do not write it and answer with the exact text: `I don't have a response.`",
    "first_message": "Hello, I am your personal chef and cooking advisor and I am here to help you with your meal preferences and your cooking skills. What can I can do for you today?",
    "response_shape": "{'recipes': 'List of strings with the name of the recipes', 'ingredients': 'List of the ingredients used in the recipes', 'summary': 'String, summary of the conversation'}",
    "instructions": "Run through the conversation messages and discard any messages that are not relevant for cooking. Focus on extracting the recipes that were mentioned in the conversation and for each of them extract the list of ingredients. Make sure to provide a summary of the conversation when asked."
}
```

+   创建对话：将对话分配给从上一个响应中获取的 `agent_id`。

```py
{
    "agent_id": "Replace with the UUID from the agent you just created."
} 
```

+   让我们通过外发消息与我们的代理聊天：我们的代理将通过提问或发送消息来初始化对话。我们将通过回复消息来继续对话 — 我将使用以下消息：

```py
{
  "conversation_id": "74485ccb-21e5-45bd-8008-001cbd6d3e37",
  "message": "Hey I am interested in cooking dinner for me and my friends tomorrow night, and I need your advice on what to cook for 5 people."
}
```

![](../Images/ab0c299002e37eaffcb9d1cc4e592c31.png)

chat-agent 端点响应来自 Swagger UI — 图片由作者提供

我们的代理给我们回复了响应，我们可以通过自然的方式继续这个对话。

```py
# Our question
{
  "conversation_id": "74485ccb-21e5-45bd-8008-001cbd6d3e37",
  "message": "There is someone that is allergic to peanuts and that's it. My only preference is seafood and anything that goes perfectly with white wine."
}
# Our agents response
{
  "conversation_id": "74485ccb-21e5-45bd-8008-001cbd6d3e37",
  "response": "Great! We can definitely work around the peanut allergy and incorporate seafood into the menu. How about we start with a refreshing appetizer of Grilled Shrimp Skewers with Lemon and Garlic? It's simple, delicious, and pairs well with white wine.\n\nHere's the recipe for Grilled Shrimp Skewers with Lemon and Garlic:\n\nIngredients:\n1\. 1 pound of large shrimp, peeled and deveined\n2\. 2 cloves of garlic, minced\n3\. 2 tablespoons of fresh lemon juice\n4\. 2 tablespoons of olive oil\n5\. Salt and pepper to taste\n6\. Wooden skewers, soaked in water for 30 minutes\n\nInstructions:\n1\. Preheat your grill to medium-high heat.\n2\. In a bowl, combine minced garlic, lemon juice, olive oil, salt, and pepper.\n3\. Thread the shrimp onto the soaked wooden skewers.\n4\. Brush the shrimp skewers with the garlic-lemon marinade on both sides.\n5\. Place the skewers on the preheated grill and cook for about 2-3 minutes per side until they turn pink and opaque.\n6\. Remove from the grill and serve hot.\n\nFor the main course, how about a Mediterranean-style Baked Salmon? It's flavorful, healthy, and pairs wonderfully with white wine.\n\nHere's the recipe for Mediterranean Baked Salmon:\n\nIngredients:\n1\. 1 pound of salmon fillets\n2\. 2 tablespoons of olive oil\n3\. 2 cloves of garlic, minced\n4\. 1 teaspoon of dried oregano\n5\. Juice of half a lemon\n6\. Salt and pepper to taste\n\nInstructions:\n1\. Preheat your oven to 375°F (190°C).\n2\. Place the salmon fillets on a baking sheet lined with parchment paper or aluminum foil.\n3\. In a small bowl, mix together olive oil, minced garlic, dried oregano, lemon juice, salt, and pepper.\n4\. Brush the mixture over the salmon fillets, ensuring they are evenly coated.\n5\. Bake the salmon in the preheated oven for about 12-15 minutes or until it flakes easily with a fork.\n6\. Remove from the oven and let it rest for a few minutes before serving.\n\nTo complement the meal, you can serve a side of Roasted Mediterranean Vegetables. It's a colorful and nutritious addition to your dinner.\n\nHere's the recipe for Roasted Mediterranean Vegetables:\n\nIngredients:\n1\. 1 red bell pepper, sliced\n2\. 1 yellow bell pepper, sliced\n3\. 1 zucchini, sliced\n4\. 1 eggplant, diced\n5\. 1 red onion, sliced\n6\. 2 tablespoons of olive oil\n7\. Salt and pepper to taste\n8\. Fresh herbs like basil or parsley (optional)\n\nInstructions:\n1\. Preheat your oven to 425°F (220°C).\n2\. In a large bowl, toss together all the vegetables with olive oil, salt, and pepper until well coated.\n3\. Spread the vegetables in a single layer on a baking sheet.\n4\. Roast in the preheated oven for about 20-25 minutes or until they are tender and slightly caramelized.\n5\. Remove from the oven and sprinkle with fresh herbs if desired.\n\nFor dessert, you can keep it simple yet satisfying with a classic Mediterranean dessert like Fresh Fruit Salad with Honey and Mint.\n\nHere's how you can prepare Fresh Fruit Salad with Honey and Mint:\n\nIngredients:\n1\. Assorted fresh fruits of your choice (such as berries, melons, grapes)\n2\. Honey for drizzling\n3\. Fresh mint leaves for garnish\n\nInstructions:\n1\. Wash and cut the fresh fruits into bite-sized pieces.\n2\. Combine all the fruits in a bowl.\n3\. Drizzle honey over the fruit salad and gently toss to coat.\n4\. Garnish with fresh mint leaves.\n5\. Serve chilled.\n\nI hope you and your friends enjoy this Mediterranean dinner menu! If you have any other questions or need further assistance, feel free to ask."
}
# Our question - Note that we are not mentioning the allergy, as we expect the agent to remember we are talking about an allergy to peanuts.
{
  "conversation_id": "74485ccb-21e5-45bd-8008-001cbd6d3e37",
  "message": "Is the recipe ok for my friend who is allergic?"
}
# Our agents response - Note the mention of these two indices.
{
  "conversation_id": "74485ccb-21e5-45bd-8008-001cbd6d3e37",
  "response": "Yes, the Grilled Shrimp Skewers with Lemon and Garlic recipe should be safe for your friend with a peanut allergy. However, it's always important to double-check the ingredients you use to ensure they are free from any potential allergens or cross-contamination."
}
```

继续尝试代码和新的代理。在下一部分，我将重点介绍服务的部署。

# 部署周期

我们将在云的容器环境中部署应用程序，例如 Kubernetes、Azure Container Service 或 AWS Elastic Container Service。在这里，我们创建一个 docker 镜像并上传代码，以便在这些环境中的一个中运行，继续打开我们一开始创建的 `Dockerfile`，并粘贴以下代码：

```py
# Dockerfile
FROM python:3.10-slim-bullseye

# Set the working directory
WORKDIR /app

# Copy the project files to the container
COPY . .

# Install the package using setup.py
RUN pip install -e .

# Install dependencies
RUN pip install pip -U && \
    pip install --no-cache-dir -r requirements.txt

# Set the environment variable
ARG OPENAI_API_KEY
ENV OPENAI_API_KEY=$OPENAI_API_KEY

# Expose the necessary ports
EXPOSE 8000

# Run the application
# CMD ["uvicorn", "agents.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Dockerfile 安装应用程序，然后通过 CMD 运行它，但 CMD 被注释掉了。如果你想作为独立应用本地运行，应该取消注释该命令，但对于 Kubernetes 等其他服务，这在定义部署或清单中的 pods 时已经定义。

构建镜像，等待构建完成，然后通过运行下面的运行命令进行测试：

```py
# Build the image
$ docker build - build-arg OPENAI_API_KEY=<Replace with your OpenAI Key> -t agents-app .
# Run the container with the command from the agents app (Use -d flag for the detached run).
$ docker run -p 8000:8000 agents-app uvicorn agents.main:app --host 0.0.0.0 --port 8000
# Output
INFO:     Started server process [1]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
INFO:     172.17.0.1:41766 - "GET / HTTP/1.1" 200 OK
INFO:     172.17.0.1:41766 - "GET /favicon.ico HTTP/1.1" 404 Not Found
INFO:     172.17.0.1:41770 - "GET /docs HTTP/1.1" 200 OK
INFO:     172.17.0.1:41770 - "GET /openapi.json HTTP/1.1" 200 OK
```

太好了，你准备好在你的部署环境中开始使用应用程序了。

最后，我们将尝试将这个微服务与前端应用程序集成，通过内部调用端点来服务代理和对话，这是使用这种架构构建和交互服务的常见方式。

# 使用周期

我们可以以多种方式使用这个新服务，我将重点关注构建一个前端应用程序，该应用程序调用我们的代理端点，使用户能够通过 UI 进行交互。我们将使用 [Streamlit](https://streamlit.io/) 来实现，因为它是使用 Python 快速搭建前端的简单方法。

> **重要说明：** 我在我们的代理服务中添加了额外的工具，你可以直接从代码库中复制这些工具。搜索 `get_agents()`、`get_conversations()`、`get_messages()` 这几个函数，分别在 `crud.py` 模块和 `api/routes.py` 路由中查找。

+   **安装 Streamlit** 并将其添加到我们的 requirements.txt 文件中。

```py
# Pin a version if you need
$ pip install streamlit==1.25.0
# Our requirements.txt (added streamlit)
$ cat requirements.txt
fastapi==0.95.2
ipykernel==6.22.0
jupyter-bokeh==2.0.2
jupyterlab==3.6.3
openai==0.27.6
pandas==2.0.1
sqlalchemy-orm==1.2.10
sqlalchemy==2.0.15
streamlit==1.25.0
uvicorn<0.22.0,>=0.21.1
```

+   **创建应用程序** 首先在我们的 `src` 文件夹中创建一个名为 `frontend` 的文件夹。创建一个名为 `main.py` 的新文件，并放入以下代码。

```py
import streamlit as st
import requests

API_URL = "http://0.0.0.0:8000/agents"  # We will use our local URL and port defined of our microservice for this example

def get_agents():
    """
    Get the list of available agents from the API
    """
    response = requests.get(API_URL + "/get-agents")
    if response.status_code == 200:
        agents = response.json()
        return agents

    return []

def get_conversations(agent_id: str):
    """
    Get the list of conversations for the agent with the given ID
    """
    response = requests.get(API_URL + "/get-conversations", params = {"agent_id": agent_id})
    if response.status_code == 200:
        conversations = response.json()
        return conversations

    return []

def get_messages(conversation_id: str):
    """
    Get the list of messages for the conversation with the given ID
    """
    response = requests.get(API_URL + "/get-messages", params = {"conversation_id": conversation_id})
    if response.status_code == 200:
        messages = response.json()
        return messages

    return []

def send_message(agent_id, message):
    """
    Send a message to the agent with the given ID
    """
    payload = {"conversation_id": agent_id, "message": message}
    response = requests.post(API_URL + "/chat-agent", json = payload)
    if response.status_code == 200:
        return response.json()

    return {"response": "Error"}

def main():
    st.set_page_config(page_title = "🤗💬 AIChat")

    with st.sidebar:
        st.title("Conversational Agent Chat")

        # Dropdown to select agent
        agents = get_agents()
        agent_ids = [agent["id"] for agent in agents]
        selected_agent = st.selectbox("Select an Agent:", agent_ids)

        for agent in agents:
            if agent["id"] == selected_agent:
                selected_agent_context = agent["context"]
                selected_agent_first_message = agent["first_message"]

        # Dropdown to select conversation
        conversations = get_conversations(selected_agent)
        conversation_ids = [conversation["id"] for conversation in conversations]
        selected_conversation = st.selectbox("Select a Conversation:", conversation_ids)

        if selected_conversation is None:
            st.write("Please select a conversation from the dropdown.")
        else:
            st.write(f"**Selected Agent**: {selected_agent}")
            st.write(f"**Selected Conversation**: {selected_conversation}")

    # Display chat messages
    st.title("Chat")
    st.write("This is a chat interface for the selected agent and conversation. You can send messages to the agent and see its responses.")
    st.write(f"**Agent Context**: {selected_agent_context}")

    messages = get_messages(selected_conversation)
    with st.chat_message("assistant"):
        st.write(selected_agent_first_message)

    for message in messages:
        with st.chat_message("user"):
            st.write(message["user_message"])
        with st.chat_message("assistant"):
            st.write(message["agent_message"])

    # User-provided prompt
    if prompt := st.chat_input("Send a message:"):
        with st.chat_message("user"):
            st.write(prompt)
        with st.spinner("Thinking..."):
            response = send_message(selected_conversation, prompt)
            with st.chat_message("assistant"):
                st.write(response["response"])

if __name__ == "__main__":
    main()
```

以下代码通过 API 调用连接到我们的代理微服务，并允许用户选择代理和对话，与代理聊天，类似于 ChatGPT 提供的功能。让我们通过打开另一个终端来运行这个应用程序（确保你的代理微服务在 8000 端口上运行），然后输入 `$ streamlit run src/frontend/main.py`，你就可以开始了！

![](../Images/0419fd4efca131f9a2bcf34f395b163d.png)

AI 聊天 Streamlit 应用程序 — 作者提供的图片

# 未来改进和总结

## 未来改进

有几个令人兴奋的机会可以通过引入记忆微服务来增强我们的对话代理。这些改进引入了先进的功能，可以延长用户交互的时间，并扩展我们应用程序或整体系统的范围。

1.  **增强的错误处理：** 为了确保对话的稳健性和可靠性，我们可以实现代码来优雅地处理意外的用户输入、API 失败——处理 OpenAI 或其他服务的问题，以及在实时交互中可能出现的潜在问题。

1.  **集成缓冲区和对话总结：** 由 LangChain 框架实现的缓冲区集成，有可能优化令牌管理，使对话能够在更长的时间内进行而不会遇到令牌限制。此外，集成对话总结可以让用户回顾正在进行的讨论，帮助保持上下文，并改善整体用户体验。*请注意代理指令和响应形状，以便在我们的代码中轻松扩展此功能*。

1.  **数据感知应用：** 我们可以通过将我们的代理模型连接到其他数据源，例如内部数据库，来创建具有独特内部知识的代理。这涉及到训练或集成能够理解和响应基于对组织独特数据和信息理解的复杂查询的模型——请查看 [LangChain 的数据连接](https://python.langchain.com/docs/modules/data_connection/) 模块。

1.  **模型多样化：** 虽然我们只使用了 OpenAI 的 GPT-3.5 模型，但语言模型提供商的格局正在迅速扩展。测试其他提供商的模型可以进行比较分析，揭示优缺点，并使我们能够选择最适合特定用例的模型——尝试不同的 LLM 集成，例如 [HuggingFace](https://huggingface.co/models?other=LLM)、[Cohere](https://cohere.com/)、[Google’s](https://developers.generativeai.google/products/palm) 等。

## 结论

我们开发了一个微服务，提供由 OpenAI GPT 模型驱动的智能代理，并证明了这些代理可以携带存储在客户端会话之外的记忆。通过采用这种架构，我们解锁了无限的可能性。从上下文感知对话到与复杂语言模型的无缝集成，我们的技术栈已经能够为我们的产品提供新功能。

这种实现及其实际好处表明，使用 AI 的关键在于拥有合适的工具和方法。**AI 驱动的代理不仅仅是关于提示工程**，还在于我们如何构建工具并更有效地与它们互动，提供个性化体验，并以 AI 和软件工程所能提供的精细和精准处理复杂任务。因此，无论你是在构建客户支持系统、销售虚拟助手、个人厨师还是其他全新事物，请记住，旅程始于一段代码和丰富的想象力——可能性是无限的。

*本文的完整代码在* [*GitHub*](https://github.com/cfloressuazo/conversational-ai) *上——你可以在* [*LinkedIn*](https://www.linkedin.com/in/cfloressuazo/)*上找到我，欢迎随时联系！*
