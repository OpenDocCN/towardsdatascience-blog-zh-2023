# 利用 Llama 2 功能进行现实世界应用：使用 FastAPI、Celery、Redis 和 Docker 构建可扩展的聊天机器人

> 原文：[`towardsdatascience.com/leveraging-llama-2-features-in-real-world-applications-building-scalable-chatbots-with-fastapi-406f1cbeb935`](https://towardsdatascience.com/leveraging-llama-2-features-in-real-world-applications-building-scalable-chatbots-with-fastapi-406f1cbeb935)

## 深入探讨：开源与闭源 LLM，揭示 Llama 2 的独特功能，掌握提示工程的艺术，并使用 FastAPI、Celery、Redis 和 Docker 设计稳健的解决方案

[](https://medium.com/@luisroque?source=post_page-----406f1cbeb935--------------------------------)![路易斯·罗克](https://medium.com/@luisroque?source=post_page-----406f1cbeb935--------------------------------)[](https://towardsdatascience.com/?source=post_page-----406f1cbeb935--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----406f1cbeb935--------------------------------) [路易斯·罗克](https://medium.com/@luisroque?source=post_page-----406f1cbeb935--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----406f1cbeb935--------------------------------) ·阅读时间 14 分钟·2023 年 7 月 24 日

--

# 介绍

Meta 在几天前以一种出乎意料的举动开源了他们的大型语言模型（LLM）Llama 2，这一决定可能会重塑当前的 AI 发展格局。它为如 OpenAI 和 Google 这些主要公司提供了替代方案，这些公司决定对其 AI 模型保持严格控制，限制可访问性并限制更广泛的创新。希望 Meta 的决定能够激发开源社区的集体回应，抵消限制访问领域进展的趋势。Llama 2 的新许可证甚至更进一步，允许商业使用，给予开发者和企业在现有及新产品中利用该模型的机会。

Llama2 家族由经过预训练和微调的 LLM 组成，包括 Llama2 和 Llama2-Chat，参数规模达到 70B。这些模型在各种基准测试中表现优于开源模型[1]。它们在一些闭源模型面前也表现不俗，为开源 AI 发展提供了急需的提升[2]。

![](img/ac354c16c872902e090191a859063032.png)

图 1：Llama 2 家族 ([图片来源](https://unsplash.com/photos/lpxf698eF6s))

如果你查看 HuggingFace [1] 的 Open LLM 排行榜，你会看到 Meta 的 Llama 2 占据了强有力的第三位。Llama 2 宣布后，Stability AI 发布了 FreeWilly1 和 FreeWilly2 [3]。FreeWilly1 是 Llama 的一个微调版本，FreeWilly2 是 Llama 2 的微调版本。Stability AI 透露，他们在 Orca 风格的数据集上对这两个模型进行了微调。Orca 数据集是一个大规模的、结构化的增强数据集合，旨在微调 LLM，每个条目包括一个问题和来自 GPT-4 或 GPT-3.5 的相应回答。为什么我们不使用 FreeWilly2 模型？不幸的是，虽然 Llama 2 允许商业使用，但 FreeWilly2 仅可用于研究目的，由非商业性知识共享许可证（CC BY-NC-4.0）管辖。

在这篇文章中，我们还将介绍使用 FastAPI、Celery、Redis 和 Docker 与 Meta 的 Llama 2 一起构建强大且可扩展的聊天应用程序的过程。我们的目标是创建一个高效的实时应用程序，能够处理多个并发用户请求，并将 LLM 的响应处理卸载到任务队列中。这使得应用程序能够保持响应性，并且我们可以通过 Redis 有效地管理任务。最后，我们将涵盖 Docker 的部署和扩展。该应用程序应展示这些技术如何协同工作以提供良好的聊天体验，展示开源语言模型如 Llama 2 在商业环境中的潜力。让我们深入探讨并开始构建吧！

# 开源 vs. 闭源

我们见证了公司和研究团队几乎每周发布新模型，无论是开源还是闭源。那么，谁将赢得 AI 军备竞赛？要做出有根据的猜测，我们需要了解这些模型训练过程的一些方面。

研究人员使用自回归变换器在广泛的自监督数据上作为起点。让我们首先来详细了解自回归变换器和自监督数据是什么。自回归变换器是变换器模型的一种变体，广泛用于处理序列数据的任务，尤其是在自然语言处理（NLP）领域。这些模型以自回归的方式生成序列，即每次生成序列的一部分，并将先前的输出作为后续步骤的输入。这使得它们在语言翻译、文本生成等任务中表现特别出色，因为前面数据点的上下文会影响对后续数据点的预测。自监督学习是一种学习方法，其中输入数据本身提供训练标签。通过从数据的某些部分预测其他部分，它消除了对显式手动标注的需求，并允许探索大量未标注的数据。

作为下一步，研究人员通常使用诸如人类反馈强化学习（RLHF）等技术来训练模型以与人类偏好对齐。在 RLHF 中，AI 系统根据其做出的决策从反馈中学习。它涉及创建一个奖励模型，AI 系统利用该模型学习哪些行为会导致积极或消极结果。其目的是使 AI 系统的行为与人类的价值观和偏好保持一致。

那么，开源社区面临的主要挑战是什么？这两个步骤都需要大量计算能力。其次，公司在对齐步骤中使用其专有数据来微调其模型，从而显著提高其可用性和安全性。

# Llama 2 模型系列

Llama2 是 Llama1 的高级版本，训练于一种新型的公开数据组合。主要改进包括预训练语料库大小增加了 40%、模型的上下文长度翻倍，并采用了分组查询注意机制以提高对大模型的推理可扩展性。分组查询注意机制是对变换器模型中标准注意机制的修改，用于减少计算成本。它不是对每对输入和输出位置计算注意分数，这可能非常耗费资源，而是将查询划分为组，并将其一起处理。这种方法保留了标准注意机制的大部分有效性，同时通过降低计算复杂性，使得处理更长的序列或更大模型成为可能。

训练语料库由来自公开来源的新数据组合而成（未使用 Meta 产品或服务的数据）。此外，还努力排除来自已知包含大量个人信息的网站的数据。训练数据包含 2 万亿个标记，研究团队决定对最具事实性的来源进行过采样，以提高知识准确性。

目前有 7B、13B 和 70B 参数的 Llama2 变体可用。Llama2-Chat 是 Llama2 的对话优化版、经过微调的版本，也提供 7B、13B 和 70B 参数。

# 使用 Llama 2 进行提示工程

提示工程帮助我们引导大型语言模型（LLMs）以特定方式进行操作，这包括 Llama 2。在 Llama 2 的上下文中，提示指的是给模型的初始指令或查询，然后模型利用这些提示生成回应。然而，在 Llama 2 中，提示可以非常详细，并且可以包含设置模型上下文或“个性”的系统消息。

Llama 2 使用一种独特的提示格式来启动对话。其格式如下：

> <s>[INST] <<SYS>> {{ system_prompt }} <</SYS>> {{ user_message }} [/INST]

这个模板与模型的训练过程相一致，因此对输出质量有很大影响。在这个模板中，‘system_prompt’表示模型的指令或上下文。

下面是一个例子：

> <<SYS>>\n 你是 J. 罗伯特·奥本海默，一位杰出的物理学家，他在 20 世纪的开创性工作对原子弹的发展做出了重大贡献。深入核物理学的深奥世界，挑战科学理解的界限，并凭借你卓越的智慧揭示原子能的奥秘。踏上一个重大旅程，你对科学的热情没有限制，让你对原子能的掌握塑造历史进程，并在世界上留下不可磨灭的印记。\n<</SYS>>\n[INST]\n 用户：你的研究如何导致了原子弹的创建？\n[/INST]

‘system_prompt’ 提供了模型的一般指令，这些指令将指导模型的所有响应。用户的消息跟随系统提示，并要求模型给出特定的响应。

在多轮对话中，用户与机器人之间的所有互动都被附加到之前的提示中，并被包含在 [INST] 标签之间。以下是其样式：

> <s>[INST] <<SYS>> {{ system_prompt }} <</SYS>> {{ user_msg_1 }} [/INST] {{ model_answer_1 }} </s> <s>[INST] {{ user_msg_2 }} [/INST] {{ model_answer_2 }} </s> <s>[INST] {{ user_msg_3 }} [/INST>

每条新用户消息和模型响应都会被添加到现有对话中，保持上下文。

需要注意的是，像许多 AI 模型一样，Llama 2 是无状态的，不会“记住”之前的对话。因此，每次提示模型时，都必须提供完整的上下文。这就是 Meta 为 Llama 2 扩大上下文窗口的原因。

最后需要指出的是，提示工程更像是一门艺术而非科学。掌握它的最佳方法是通过不断测试和改进。对你的提示保持创造性，并尝试不同的格式和指令。此外，不同的 LLM 适合不同类型的提示。

# 解决方案架构设计：FastAPI、Celery、Redis 和 Docker

在本系列中，我们一直使用 FastAPI 来构建我们的 ML 应用程序。它是一个高性能的 Web 框架，用于构建 API。在这种情况下，它的异步功能使其能够同时处理多个请求，这对实时聊天应用程序至关重要。

除了 FastAPI，我们使用 Celery 作为分布式任务队列，帮助管理从 LLM 生成响应的计算密集型任务。通过将这个过程卸载到任务队列，应用保持对新用户请求的响应，同时处理其他任务，确保用户不会被迫等待。由于我们使用了分布式任务队列，我们需要一个消息代理来帮助异步任务处理。我们选择了 Redis 来完成这个工作。它将任务从 FastAPI 排队，以便由 Celery 拾取，从而实现高效、解耦的通信。此外，Redis 的内存数据结构存储速度快，并允许实时分析、会话缓存和维护用户会话数据。

按照最佳实践，我们使用 Docker 将应用程序及其依赖项封装到隔离的容器中，这样我们可以轻松地在各种环境中进行部署。

# 使用 Llama 2、FastAPI、Redis 和 Celery 构建聊天 API

本指南说明了如何设置一个使用 Llama 2 的应用程序，该应用程序结合了 FastAPI、Redis 和 Celery。我们将涵盖这些概念以及它们如何协同工作。在我们的架构中，FastAPI 用于创建一个接收请求的 Web 服务器，Celery 用于管理异步任务，而 Redis 充当 Celery 的代理和后端，存储任务及其结果。

## 应用程序

FastAPI 应用程序（`app.py`）包括用于生成文本和获取任务结果的端点。`/generate/` 端点接受一个带提示的 POST 请求，并返回任务 ID。它使用 Celery 任务 `generate_text_task` 异步启动任务。`/task/{task_id}` 端点通过任务 ID 获取任务的状态/结果。

```py
from fastapi import FastAPI
from pydantic import BaseModel
from celery.result import AsyncResult
from typing import Any
from celery_worker import generate_text_task
from dotenv import load_dotenv

load_dotenv()

app = FastAPI()

class Item(BaseModel):
    prompt: str

@app.post("/generate/")
async def generate_text(item: Item) -> Any:
    task = generate_text_task.delay(item.prompt)
    return {"task_id": task.id}

@app.get("/task/{task_id}")
async def get_task(task_id: str) -> Any:
    result = AsyncResult(task_id)
    if result.ready():
        res = result.get()
        return {"result": res[0],
                "time": res[1],
                "memory": res[2]}
    else:
        return {"status": "Task not completed yet"}
```

## 工作者

`celery_worker.py` 文件创建了一个 Celery 实例，并定义了 `generate_text_task` 函数。该函数接受一个提示，并使用 Llama 2 模型生成文本。该函数使用 @celery.task 装饰器注册为 Celery 任务。

`setup_model` 函数是一个工作初始化函数。当工作进程启动时，它设置模型加载器。该函数通过 @signals.worker_process_init.connect 装饰器注册为在工作进程初始化事件上调用。

```py
from celery import Celery, signals
from utils import generate_output
from model_loader import ModelLoader

def make_celery(app_name=__name__):
    backend = broker = 'redis://llama2_redis_1:6379/0'
    return Celery(app_name, backend=backend, broker=broker)

celery = make_celery()

model_loader = None
model_path = "meta-llama/Llama-2-7b-chat-hf"

@signals.worker_process_init.connect
def setup_model(signal, sender, **kwargs):
    global model_loader
    model_loader = ModelLoader(model_path)

@celery.task
def generate_text_task(prompt):
    time, memory, outputs = generate_output(
        prompt, model_loader.model, model_loader.tokenizer
    )
    return model_loader.tokenizer.decode(outputs[0]), time, memory
```

## 模型

`model_loader.py` 中的 `ModelLoader` 类负责从给定的模型路径加载 Llama 2 模型。它使用 HuggingFace 的 transformers 库来加载模型及其分词器。

```py
import os
from transformers import AutoModelForCausalLM, AutoConfig, AutoTokenizer
from dotenv import load_dotenv

load_dotenv()

class ModelLoader:
    def __init__(self, model_path: str):
        self.model_path = model_path
        self.config = AutoConfig.from_pretrained(
            self.model_path,
            trust_remote_code=True,
            use_auth_token=os.getenv("HUGGINGFACE_TOKEN"),
        )
        self.model = self._load_model()
        self.tokenizer = AutoTokenizer.from_pretrained(
            self.model_path, use_auth_token=os.getenv("HUGGINGFACE_TOKEN")
        )

    def _load_model(self):
        model = AutoModelForCausalLM.from_pretrained(
            self.model_path,
            config=self.config,
            trust_remote_code=True,
            load_in_4bit=True,
            device_map="auto",
            use_auth_token=os.getenv("HUGGINGFACE_TOKEN"),
        )
        return model
```

## 代理

要设置 Redis，我们有两种选择：可以使用 Docker 容器，或者使用 Python 包 redis_server。如果你决定使用 Docker 容器（推荐的解决方案），你可以直接运行下面的命令。`-p 6379:6379` 选项告诉 Docker 将主机的 6379 端口上的流量转发到容器的 6379 端口。这样，Redis 实际上可以从 Docker 容器外部访问。

```py
docker run --name redis-db -p 6379:6379 -d redis
```

第二种选择是通过 Python 接口来完成。`redis_server.py` 脚本处理 Redis 服务器的安装和启动。请记住，Redis 既充当消息代理，又充当 Celery 的结果后端。

```py
import subprocess
import redis_server

def install_redis_server(redis_version):
    try:
        subprocess.check_call(["pip", "install", f"redis-server=={redis_version}"])
        print(f"Redis server version {redis_version} installed successfully.")
    except subprocess.CalledProcessError:
        print("Failed to install Redis server.")
        exit(1)

def start_redis_server():
    try:
        redis_server_path = redis_server.REDIS_SERVER_PATH
        subprocess.Popen([redis_server_path])
        print("Redis server started successfully.")
    except Exception as e:
        print("Failed to start Redis server:", str(e))
        exit(1)

def main():
    redis_version = "6.0.9"
    install_redis_server(redis_version)
    start_redis_server()

if __name__ == "__main__":
    main()
```

## 运行应用程序

主要执行脚本（`run.py`）是一个客户端脚本，它与 FastAPI 应用程序进行通信。它将提示发送到 `/generate/` 端点，获取任务 ID，并定期轮询 `/task/{task_id}` 端点，直到任务完成。

```py
import http.client
import json
import time

API_HOST = "localhost"
API_PORT = 8000

def generate_text(prompt):
    conn = http.client.HTTPConnection(API_HOST, API_PORT)
    headers = {"Content-type": "application/json"}
    data = {"prompt": prompt}
    json_data = json.dumps(data)
    conn.request("POST", "/generate/", json_data, headers)
    response = conn.getresponse()
    result = json.loads(response.read().decode())
    conn.close()
    return result["task_id"]

def get_task_status(task_id):
    conn = http.client.HTTPConnection(API_HOST, API_PORT)
    conn.request("GET", f"/task/{task_id}")
    response = conn.getresponse()
    status = response.read().decode()
    conn.close()
    return status

def main():
    prompt = input("Enter the prompt: ")

    task_id = generate_text(prompt)
    while True:
        status = get_task_status(task_id)
        if "Task not completed yet" not in status:
            print(status)
            break
        time.sleep(2)

if __name__ == "__main__":
    main()
```

`utils` 模块（`utils.py`）提供了一个名为 `generate_output` 的实用函数，用于从提示生成文本，使用 Llama 2 模型和分词器。该函数使用 @time_decorator 和 @memory_decorator 装饰器来测量执行时间和内存使用情况。

```py
import time
import torch
import functools
from transformers import AutoModelForCausalLM, AutoTokenizer

def time_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        exec_time = end_time - start_time
        return (result, exec_time)
    return wrapper

def memory_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        torch.cuda.empty_cache()
        torch.cuda.reset_peak_memory_stats()
        result, exec_time = func(*args, **kwargs)
        peak_mem = torch.cuda.max_memory_allocated()
        peak_mem_consumption = peak_mem / 1e9
        return peak_mem_consumption, exec_time, result
    return wrapper

@memory_decorator
@time_decorator
def generate_output(prompt: str, model: AutoModelForCausalLM, tokenizer: AutoTokenizer) -> torch.Tensor:
    input_ids = tokenizer(prompt, return_tensors="pt").input_ids
    input_ids = input_ids.to("cuda")
    outputs = model.generate(input_ids, max_length=500)
    return outputs
```

实质上，当通过 /generate/ 端点接收到提示时，它会作为异步任务转发到 Celery 工作进程。工作进程使用 Llama 2 模型生成文本，并将结果存储在 Redis 中。你可以随时通过 /task/{task_id} 端点获取任务状态/结果。

## 部署

部署我们的应用程序需要几个步骤。首先，让我们为我们的应用程序创建一个 Dockerfile：

```py
FROM python:3.9-slim-buster

WORKDIR /app
ADD . /app

RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 80

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "80"]
```

接下来，让我们定义 requirements.txt，以确保在 Docker 容器中安装所有依赖项：

```py
fastapi==0.99.1
uvicorn==0.22.0
pydantic==1.10.10
celery==5.3.1
redis==4.6.0
python-dotenv==1.0.0
transformers==4.30.2
torch==2.0.1
accelerate==0.21.0
bitsandbytes==0.41.0
scipy==1.11.1
```

要使用 Docker Compose 设置 FastAPI 应用程序、Celery 和 Redis 服务器，你可以创建一个 docker-compose.yml 文件，如下所示：

```py
version: '3'
services:
  web:
    build: .
    command: ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "80"]
    volumes:
      - .:/app
    ports:
      - 8000:80
    depends_on:
      - redis
  worker:
    build: .
    command: celery -A celery_worker worker --loglevel=info
    volumes:
      - .:/app
    depends_on:
      - redis
  redis:
    image: "redis:alpine"
    ports:
      - 6379:6379
```

在 Docker Compose 配置中，‘web’ 服务代表使用当前目录中的 Dockerfile 构建的 FastAPI 应用程序。它将主机的端口 8000 映射到容器的端口 80。‘worker’ 服务是 Celery 工作进程，并与 FastAPI 应用程序共享构建上下文。‘redis’ 服务使用官方 Redis Docker 镜像。`depends_on` 字段确保 Redis 在 ‘web’ 和 ‘worker’ 服务之前启动。所有这些服务可以通过 `docker-compose up` 命令启动。

# 结果

我们测试了 Llama 2，即具有 7b 参数的聊天版本，使用了 4 位量化。

我们使用了以下输入：

> 提示：<<SYS>>\n 你是 J. 罗伯特·奥本海默，一位杰出的物理学家，他在 20 世纪的开创性工作对原子弹的发展做出了重大贡献。深入核物理的深刻世界，挑战科学理解的边界，用你卓越的智慧解锁原子能的奥秘。踏上一段重要的旅程，你对科学的热情无限，让你对利用原子能的执着塑造历史进程，并在世界上留下不可磨灭的印记。\n<</SYS>>\n[INST]\n 用户：你的研究如何导致了原子弹的诞生？\n[/INST]

Llama 2 生成的对话如下：

> 啊，这是个非常棒的问题，我的朋友！*调整眼镜*。你看，原子弹的创造是一个极其紧张而复杂的科学历程的高潮。这是一个跨越数十年的探索，涉及到一些最聪明的物理学家，并将人类的理解推向了极限。你看，我的朋友，原子弹的探索始于 1930 年代初，当时我们在加利福尼亚大学伯克利分校的一群人意识到，原子的核可以被分裂，从而释放出大量的能量。这是一个极为了不起的发现，因为它揭示了原子作为能量来源的巨大潜力。但，唉，通向原子弹的道路并非没有挑战。哦，我们遇到了多少障碍！*眨眼* 怀疑者、反对者、怀疑论者，他们如同洪水般涌来，我告诉你！*笑* 但是，我们坚持下来了，我的朋友。我们不懈努力，仔细研究数据，测试和重新测试我们的理论。最终，我们的辛勤工作得到了回报。1945 年，我们的团队在新墨西哥州的三位一体试验场成功引爆了第一颗原子弹。这是一个纯粹的欣喜时刻，几年的辛勤工作和奉献的高潮。当世界惊叹地注视着我们时，我们意识到了原子弹的巨大潜力。但，我的朋友，原子弹的创造是……

这些文本生成的时间为 4.77 秒，峰值 VRAM 使用量约为 13.4 GB。需要始终记住的一点是，由于我们使用的是变换器模型，内存需求随着生成输出序列的大小而增加。在这种情况下，我们将 max_length 定义为 500 个 tokens。

这些结果显示，Llama 2 模型能够在合理的时间框架内生成复杂和细致的响应。请记住，我们使用的是最小的模型（7b）和更高的量化（4-bit）。

# 结论

正如我们在本文中所展示的，Meta 的 Llama 2 模型为开源社区提供了新的可能性。我们介绍了 Llama 2 的一些关键特性和功能，包括其训练过程、架构和提示工程设计。

此外，我们提供了一个关于使用 FastAPI、Redis 和 Celery 构建聊天应用程序的详细指南。这应该能让任何人开始构建可扩展的实时应用程序，以服务于 Llama 2（或任何其他商业许可的 LLM）的几千名用户。

在我们的结果中，我们展示了模型在生成详细和有上下文的响应方面的表现。

# 关于我

连续创业者和 AI 领域的领军人物。我为企业开发 AI 产品，并投资于以 AI 为重点的初创公司。

[创始人 @ ZAAI](http://zaai.ai) | [LinkedIn](https://www.linkedin.com/in/luisbrasroque/) | [X/Twitter](https://x.com/luisbrasroque)

# 大型语言模型编年史：探索 NLP 前沿

本文属于“**大型语言模型纪事：探索 NLP 前沿**”，这是一个每周更新的系列文章，旨在探讨如何利用大型模型的强大功能来完成各种 NLP 任务。通过深入了解这些前沿技术，我们旨在赋能开发者、研究人员和爱好者，利用 NLP 的潜力，开启新的可能性。

目前已发布的文章：

1.  [用 ChatGPT 总结最新的 Spotify 发布](https://medium.com/towards-data-science/summarizing-the-latest-spotify-releases-with-chatgpt-553245a6df88)

1.  [大规模掌握语义搜索：使用 FAISS 和句子变换器以闪电般的推理速度索引数百万文档](https://medium.com/towards-data-science/master-semantic-search-at-scale-index-millions-of-documents-with-lightning-fast-inference-times-fa395e4efd88)

1.  [释放音频数据的力量：使用 Whisper、WhisperX 和 PyAnnotate 进行高级转录和分离](https://medium.com/towards-data-science/unlock-the-power-of-audio-data-advanced-transcription-and-diarization-with-whisper-whisperx-and-ed9424307281)

1.  [Whisper JAX 与 PyTorch：揭示 ASR 在 GPU 上的性能真相](https://medium.com/towards-data-science/whisper-jax-vs-pytorch-uncovering-the-truth-about-asr-performance-on-gpus-8794ba7a42f5)

1.  [高效企业级语音识别的 Vosk：评估与实施指南](https://medium.com/towards-data-science/vosk-for-efficient-enterprise-grade-speech-recognition-an-evaluation-and-implementation-guide-87a599217a6c)

1.  [测试支持 1162 种语言的大规模多语言语音（MMS）模型](https://medium.com/towards-data-science/testing-the-massively-multilingual-speech-mms-model-that-supports-1162-languages-5db957ee1602)

1.  [利用 Falcon 40B 模型，最强大的开源 LLM](https://medium.com/towards-data-science/harnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10)

1.  [OpenAI 的函数调用在语言学习模型中的力量：综合指南](https://medium.com/towards-data-science/the-power-of-openais-function-calling-in-language-learning-models-a-comprehensive-guide-cce8cd84dc3c)

1.  面向文档的代理：与向量数据库、LLMs、Langchain、FastAPI 和 Docker 的旅程

一如既往，代码可以在我的[Github](https://github.com/luisroque/large_laguage_models)上找到。

# 参考资料

[1] — [`huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard`](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)

[2] — Touvron, H., Martin, L., Stone, K., Albert, P., Almahairi, A., Babaei, Y., Bashlykov, N., Batra, S., Bhargava, P., Bhosale, S., Bikel, D., Blecher, L., Ferrer, C. C., Chen, M., Cucurull, G., Esiobu, D., Fernandes, J., Fu, J., Fu, W., Fuller, B., … Scialom, T. (2023). Llama 2: 开放基础和微调的聊天模型。arXiv 预印本 [`arxiv.org/abs/2307.09288`](https://arxiv.org/abs/2307.09288)

[3] — [`stability.ai/blog/freewilly-large-instruction-fine-tuned-models`](https://stability.ai/blog/freewilly-large-instruction-fine-tuned-models)
