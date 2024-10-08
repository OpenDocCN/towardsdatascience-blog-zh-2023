# 开始使用 LangChain：构建 LLM 驱动应用程序的初学者指南

> 原文：[`towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c`](https://towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c)

## 使用大型语言模型在 Python 中构建任何东西的 LangChain 教程

[](https://medium.com/@iamleonie?source=post_page-----95fc8898732c--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----95fc8898732c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----95fc8898732c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----95fc8898732c--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----95fc8898732c--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----95fc8898732c--------------------------------) ·11 分钟阅读·2023 年 4 月 25 日

--

![](img/ba0a0432f474352f0aac6ee6c0e9eeea.png)

“随机的鹦鹉对其他的说了什么？”（作者绘制的图像）

自 ChatGPT 发布以来，大型语言模型（LLMs）已经获得了极大的关注。尽管你可能没有足够的资金和计算资源在地下室从头训练一个 LLM，但你仍然可以使用预训练的 LLM 来构建一些很酷的东西，比如：

+   [可以根据您的数据与外部世界互动的个人助手](https://python.langchain.com/en/latest/use_cases/personal_assistants.html)

+   [为您的目的定制的聊天机器人](https://python.langchain.com/en/latest/use_cases/chatbots.html)

+   [分析](https://python.langchain.com/en/latest/use_cases/question_answering.html)或[总结](https://python.langchain.com/en/latest/use_cases/summarization.html)您的文档或[代码](https://python.langchain.com/en/latest/use_cases/code.html)

> LLM 正在改变我们构建 AI 驱动产品的方式

通过其奇特的 API 和提示工程，LLM 正在改变我们构建 AI 驱动产品的方式。这就是为什么新的开发者工具在[“LLMOps”](https://wandb.ai/iamleonie/Articles/reports/Understanding-LLMOps-Large-Language-Model-Operations--Vmlldzo0MDgyMDc2)的名义下不断涌现的原因。

这些新工具之一是[LangChain](https://github.com/hwchase17/langchain)。

[](https://github.com/hwchase17/langchain?source=post_page-----95fc8898732c--------------------------------) [## GitHub - hwchase17/langchain: ⚡ 通过组合性构建 LLM 应用 ⚡

### ⚡ 通过组合性构建 LLM 应用 ⚡ 生产支持：当你将 LangChains 迁移到…

github.com](https://github.com/hwchase17/langchain?source=post_page-----95fc8898732c--------------------------------)

# LangChain 是什么？

LangChain 是一个框架，旨在通过提供以下内容来帮助你更轻松地构建 LLM 驱动的应用程序：

+   一种通用接口，用于各种不同的基础模型（见 Models），

+   一个帮助你管理提示的框架（见 Prompts），以及

+   一个中央接口，用于长期记忆（见 Memory）、外部数据（见 Indexes）、其他 LLMs（见 Chains）以及其他 LLM 无法处理的任务（例如计算或搜索）（见 Agents）。

这是一个开源项目（[GitHub 仓库](https://github.com/hwchase17/langchain)），由 [哈里森·查斯](https://twitter.com/hwchase17) 创建。

由于 LangChain 具有许多不同的功能，因此一开始可能很难理解它的作用。这就是为什么我们将在本文中介绍（目前的）六个关键模块，以便让你更好地理解其功能。

# 先决条件

要跟随本教程，你需要安装 `langchain` Python 包，并准备好所有相关的 API 密钥。

## 安装 LangChain

在安装 `langchain` 包之前，确保你有 Python 版本 ≥ 3.8.1 且 <4.0。

要安装 `langchain` Python 包，你可以使用 `pip` 进行安装。

```py
pip install langchain
```

在本教程中，我们使用的是版本 0.0.147。由于 [GitHub 仓库](https://github.com/hwchase17/langchain) 非常活跃，请确保你使用的是最新版本。

一旦你完成所有设置，导入 `langchain` Python 包。

```py
import langchain
```

## API 密钥

使用 LLM 构建应用程序需要一些服务的 API 密钥，并且一些 API 可能会产生费用。

**LLM 提供者（必需）** — 你首先需要一个用于所需 LLM 提供者的 API 密钥。我们目前正经历[“AI 的 Linux 时刻](https://hazyresearch.stanford.edu/blog/2023-01-30-ai-linux)”，在这一时刻，开发者必须**在性能和成本之间权衡，选择专有或开源基础模型**。

![](img/00b94afaccb89deaf33f4d10262f2f94.png)

LLM 提供者：专有和开源基础模型（图像由作者提供，灵感来源于 [Fiddler.ai](https://www.fiddler.ai/blog/the-missing-link-in-generative-ai)，首次发布在 [W&B 的博客](https://wandb.ai/iamleonie/Articles/reports/Understanding-LLMOps-Large-Language-Model-Operations--Vmlldzo0MDgyMDc2)）

**专有模型** 是由拥有大型专家团队和大额 AI 预算的公司拥有的封闭源基础模型。它们通常比开源模型大，因此性能更好，但它们的 API 也很昂贵。专有模型提供商的例子包括 [OpenAI](https://python.langchain.com/en/latest/modules/models/llms/integrations/openai.html)、[co:here](https://python.langchain.com/en/latest/modules/models/llms/integrations/cohere.html)、[AI21 Labs](https://python.langchain.com/en/latest/modules/models/llms/integrations/ai21.html) 或 [Anthropic](https://python.langchain.com/en/latest/modules/models/llms/integrations/anthropic_example.html)。

大多数可用的 LangChain 教程使用 OpenAI，但请注意 [OpenAI API（虽然实验使用不贵，但）并非免费](https://openai.com/pricing)。要获取 OpenAI API 密钥，你需要一个 OpenAI 账户，然后在 [API 密钥](https://platform.openai.com/account/api-keys) 下“创建新秘密密钥”。

```py
import os
os.environ["OPENAI_API_KEY"] = ... # insert your API_TOKEN here
```

**开源模型** 通常是比专有模型小的模型，能力较低，但它们比专有模型更具成本效益。开源模型的例子包括：

+   [BLOOM](https://huggingface.co/bigscience/bloom) 由 BigScience 提供

+   [LLaMA](https://huggingface.co/docs/transformers/main/en/model_doc/llama) 由 Meta AI 提供

+   [Flan-T5](https://huggingface.co/google/flan-t5-xl) 由 Google 提供

+   [GPT-J](https://huggingface.co/EleutherAI/gpt-j-6b) 由 Eleuther AI 提供

许多开源模型被组织和托管在 [Hugging Face](https://huggingface.co/) 作为一个社区中心。要获取 Hugging Face API 密钥，你需要一个 Hugging Face 账户，并在 [访问令牌](https://huggingface.co/settings/tokens) 下创建一个“新令牌”。

```py
import os

os.environ["HUGGINGFACEHUB_API_TOKEN"] = ... # insert your API_TOKEN here
```

你可以免费使用 [Hugging Face](https://huggingface.co/) 来访问开源 LLM，但你将被限制使用性能较低的小型 LLM。

***个人备注——*** *我们诚实地说一句：当然，你可以在这里尝试开源基础模型。我试图只使用在 Hugging Face 上托管的开源模型（google/flan-t5-xl 和 sentence-transformers/all-MiniLM-L6-v2）来制作这个教程，并使用普通账户进行操作。大多数示例都能正常工作，但也有一些示例很难让它们正常运行。最后，我决定购买一个 OpenAI 的付费账户，因为 LangChain 的大多数示例似乎都为 OpenAI 的 API 进行了优化。总的来说，制作这个教程的几个实验花费了我大约 1 美元。*

**向量数据库（可选）** —— 如果你想使用特定的向量数据库，如 [Pinecone](https://www.pinecone.io/)、[Weaviate](https://weaviate.io/) 或 [Milvus](https://milvus.io/)，你需要注册并获取一个 API 密钥，并查看它们的定价。在这个教程中，我们使用的是 [Faiss](https://engineering.fb.com/2017/03/29/data-infrastructure/faiss-a-library-for-efficient-similarity-search/)，它不需要注册。

**工具（可选）** — 根据你希望 LLM 与之交互的[工具](https://python.langchain.com/en/latest/modules/agents/tools.html)，如 OpenWeatherMap 或 SerpAPI，你可能需要注册以获得 API 密钥，并查看它们的定价。在本教程中，我们仅使用不需要 API 密钥的工具。

# 你可以用 LangChain 做什么？

该包提供了与许多基础模型的通用接口，支持提示管理，并作为其他组件的中央接口，如提示模板、其他 LLM、外部数据以及通过代理访问的其他工具。

在撰写本文时，LangChain（版本 0.0.147）涵盖了六个模块：

+   模型：从不同的 LLM 和嵌入模型中选择

+   提示：管理 LLM 输入

+   链：将 LLM 与其他组件结合

+   索引：访问外部数据

+   记忆：记住先前的对话

+   代理：访问其他工具

以下部分中的代码示例是从[LangChain 文档](https://python.langchain.com/en/latest/index.html)中复制和修改的。

## 模型：从不同的 LLM 和嵌入模型中选择

目前，许多不同的 LLM 正在出现。LangChain 提供了与各种模型的集成，并为所有模型提供了简化的接口。

LangChain 区分三种类型的模型，它们在输入和输出上有所不同：

+   **LLMs** 将一个字符串作为输入（提示），并输出一个字符串（完成）。

```py
# Proprietary LLM from e.g. OpenAI
# pip install openai
from langchain.llms import OpenAI
llm = OpenAI(model_name="text-davinci-003")

# Alternatively, open-source LLM hosted on Hugging Face
# pip install huggingface_hub
from langchain import HuggingFaceHub
llm = HuggingFaceHub(repo_id = "google/flan-t5-xl")

# The LLM takes a prompt as an input and outputs a completion
prompt = "Alice has a parrot. What animal is Alice's pet?"
completion = llm(prompt)
```

![](img/6f7c12ec849f0405cc0b12cdc130e129.png)

LLM 模型（图片由作者提供）

+   **聊天模型**类似于 LLM。它们将聊天消息列表作为输入，并返回一条聊天消息。

+   **文本嵌入模型**将文本输入并返回一个浮点数列表（嵌入），这些嵌入是输入文本的数值表示。嵌入帮助从文本中提取信息。这些信息可以在后续使用，例如，计算文本之间的相似性（例如电影摘要）。

```py
# Proprietary text embedding model from e.g. OpenAI
# pip install tiktoken
from langchain.embeddings import OpenAIEmbeddings
embeddings = OpenAIEmbeddings()

# Alternatively, open-source text embedding model hosted on Hugging Face
# pip install sentence_transformers
from langchain.embeddings import HuggingFaceEmbeddings
embeddings = HuggingFaceEmbeddings(model_name = "sentence-transformers/all-MiniLM-L6-v2")

# The embeddings model takes a text as an input and outputs a list of floats
text = "Alice has a parrot. What animal is Alice's pet?"
text_embedding = embeddings.embed_query(text)
```

![](img/0759e970e3b04f43b057231e5e2aec43.png)

文本嵌入模型（图片由作者提供）

## 提示：管理 LLM 输入

LLM 的 API 很奇怪。虽然用自然语言输入提示应该是直观的，但需要对提示进行相当多的调整，才能从 LLM 中获得期望的输出。这个过程被称为*提示工程*。

一旦你拥有了一个好的提示，你可能希望将其用作其他用途的模板。因此，LangChain 为你提供了所谓的`PromptTemplates`，这些模板帮助你从多个组件构建提示。

```py
from langchain import PromptTemplate

template = "What is a good name for a company that makes {product}?"

prompt = PromptTemplate(
    input_variables=["product"],
    template=template,
)

prompt.format(product="colorful socks")
```

上述提示可以视为**零-shot 问题设置**，你希望 LLM 在足够相关的数据上进行训练，从而提供令人满意的响应。

提高 LLM 输出的另一个技巧是添加几个示例到提示中，将其设为**少量示例问题设置**。

```py
from langchain import PromptTemplate, FewShotPromptTemplate

examples = [
    {"word": "happy", "antonym": "sad"},
    {"word": "tall", "antonym": "short"},
]

example_template = """
Word: {word}
Antonym: {antonym}\n
"""

example_prompt = PromptTemplate(
    input_variables=["word", "antonym"],
    template=example_template,
)

few_shot_prompt = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_prompt,
    prefix="Give the antonym of every input",
    suffix="Word: {input}\nAntonym:",
    input_variables=["input"],
    example_separator="\n",
)

few_shot_prompt.format(input="big")
```

上述代码将生成一个提示模板，并根据提供的示例和输入组成以下提示：

```py
Give the antonym of every input

Word: happy
Antonym: sad

Word: tall
Antonym: short

Word: big
Antonym:
```

## 链：将 LLM 与其他组件结合

在 LangChain 中，链式操作简单地描述了将 LLM 与其他组件结合以创建应用程序的过程。一些示例包括：

+   将 LLM 与提示模板结合（参见本节）

+   通过将第一个 LLM 的输出作为第二个 LLM 的输入来依次结合多个 LLM（参见本节）

+   将 LLM 与外部数据结合，例如，用于回答问题（参见 索引）

+   将 LLM 与长期记忆结合，例如，用于聊天记录（参见 记忆）

在上一节中，我们创建了一个提示模板。当我们想将其与 LLM 一起使用时，可以按如下方式使用 `LLMChain`：

```py
from langchain.chains import LLMChain

chain = LLMChain(llm = llm, 
                  prompt = prompt)

# Run the chain only specifying the input variable.
chain.run("colorful socks")
```

如果我们想将第一个 LLM 的输出作为第二个 LLM 的输入，可以使用 `SimpleSequentialChain`：

```py
from langchain.chains import LLMChain, SimpleSequentialChain

# Define the first chain as in the previous code example
# ...

# Create a second chain with a prompt template and an LLM
second_prompt = PromptTemplate(
    input_variables=["company_name"],
    template="Write a catchphrase for the following company: {company_name}",
)

chain_two = LLMChain(llm=llm, prompt=second_prompt)

# Combine the first and the second chain 
overall_chain = SimpleSequentialChain(chains=[chain, chain_two], verbose=True)

# Run the chain specifying only the input variable for the first chain.
catchphrase = overall_chain.run("colorful socks")
```

![](img/a19959a0c2a5d4f0c48c9aee7cb979a4.png)

使用 PromptTemplates 和 LLMs 在 LangChain 中的 SimpleSequentialChain 的输出（作者截图）

## 索引：访问外部数据

LLM 的一个限制是缺乏上下文信息（例如，访问某些特定文档或电子邮件）。你可以通过让 LLM 访问特定的外部数据来解决这个问题。

为此，你首先需要用文档加载器加载外部数据。LangChain 提供了各种加载器，用于不同类型的文档，从 PDFs 和电子邮件到网站和 YouTube 视频。

让我们从 YouTube 视频中加载一些外部数据。如果你想 [加载一个大型文本文档并使用](https://python.langchain.com/en/latest/modules/indexes/getting_started.html) [文本分割器](https://python.langchain.com/en/latest/modules/indexes/text_splitters.html) 将其拆分，可以参考官方文档。

```py
# pip install youtube-transcript-api
# pip install pytube

from langchain.document_loaders import YoutubeLoader

loader = YoutubeLoader.from_youtube_url("https://www.youtube.com/watch?v=dQw4w9WgXcQ")

documents = loader.load()
```

现在你已经准备好将外部数据作为 `documents`，你可以使用 **文本嵌入模型**（参见 模型）在向量数据库中进行索引——一个 **VectorStore**。流行的向量数据库包括 [Pinecone](https://www.pinecone.io/)、[Weaviate](https://weaviate.io/) 和 [Milvus](https://milvus.io/)。在本文中，我们使用 Faiss，因为它不需要 API 密钥。

```py
# pip install faiss-cpu
from langchain.vectorstores import FAISS

# create the vectorestore to use as the index
db = FAISS.from_documents(documents, embeddings)
```

你的文档（在这种情况下是视频）现在作为嵌入存储在向量存储中。

现在，你可以使用这些外部数据做各种事情。让我们用它来进行一个信息**检索器**的问答任务：

```py
from langchain.chains import RetrievalQA

retriever = db.as_retriever()

qa = RetrievalQA.from_chain_type(
    llm=llm, 
    chain_type="stuff", 
    retriever=retriever, 
    return_source_documents=True)

query = "What am I never going to do?"
result = qa({"query": query})

print(result['result'])
```

![](img/e9499bbce4053fff99174903e5578c71.png)

检索问答的输出（作者截图）

等一下——你刚刚被恶搞了？是的，你确实被恶搞了。

## 记忆：记住先前的对话

对于聊天机器人等应用程序来说，能够记住先前的对话至关重要。但默认情况下，LLM 不具有任何长期记忆，除非你输入聊天记录。

![](img/13683d095100fa5d71506adbe571435a.png)

带有和不带有对话记忆的聊天（图片由 [ifaketextmessage.com](https://ifaketextmessage.com/) 制作，灵感来自 [Pinecone](https://www.pinecone.io/learn/langchain-conversational-memory)）

LangChain 通过提供几种处理聊天记录的不同选项来解决这个问题：

+   保留所有对话，

+   保留最近的 k 个对话，

+   总结对话。

在这个示例中，我们将使用`ConversationChain`来为这个应用程序提供对话记忆。

```py
from langchain import ConversationChain

conversation = ConversationChain(llm=llm, verbose=True)

conversation.predict(input="Alice has a parrot.")

conversation.predict(input="Bob has two cats.")

conversation.predict(input="How many pets do Alice and Bob have?")
```

这将导致上图中右侧的对话。如果没有`ConversationChain`来保持对话记忆，对话将会像上图中左侧的那样。

## 代理：访问其他工具

尽管 LLM 相当强大，但它们有一些局限性：缺乏上下文信息（例如，无法访问训练数据中未包含的特定知识），可能会很快过时（例如，GPT-4 是[在 2021 年 9 月之前的数据上训练的](https://openai.com/research/gpt-4)），并且在数学方面表现不佳。

LLM 在数学方面表现不好

因为 LLM 在无法自行完成任务时可能会产生幻觉，我们需要提供额外的**工具**，如搜索（例如，[Google 搜索](https://python.langchain.com/en/latest/modules/agents/tools/examples/google_search.html)）、计算器（例如，[Python REPL](https://python.langchain.com/en/latest/modules/agents/tools/examples/python.html) 或 [Wolfram Alpha](https://python.langchain.com/en/latest/modules/agents/tools/examples/wolfram_alpha.html)），以及查找（例如，[维基百科](https://python.langchain.com/en/latest/modules/agents/tools/examples/wikipedia.html)）。

此外，我们需要代理程序来根据 LLM 的输出决定使用哪些工具来完成任务。

*请注意，一些 LLM 例如* `[*google/flan-t5-xl*](https://github.com/hwchase17/langchain/issues/1358)` [*可能不适用于以下示例，因为它们不符合*](https://github.com/hwchase17/langchain/issues/1358) `[*conversational-react-description*](https://github.com/hwchase17/langchain/issues/1358)` [*模板。*](https://github.com/hwchase17/langchain/issues/1358) *对我来说，这是我在 OpenAI 上设置付费账户并切换到 OpenAI API 的时刻。*

以下是一个示例，其中代理首先用维基百科查找巴拉克·奥巴马的出生日期，然后用计算器计算他在 2022 年的年龄。

```py
# pip install wikipedia
from langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType

tools = load_tools(["wikipedia", "llm-math"], llm=llm)
agent = initialize_agent(tools, 
                         llm, 
                         agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, 
                         verbose=True)

agent.run("When was Barack Obama born? How old was he in 2022?")
```

![](img/4b0008eae6eb77df7210741f368d43b4.png)

LLM 代理的输出（作者截图）

# 总结

就在几个月前，我们所有人（或者至少大多数人）都对 ChatGPT 的能力印象深刻。现在，像 LangChain 这样的新开发工具使我们能够在几小时内在笔记本电脑上构建同样令人印象深刻的原型——这些确实是令人兴奋的时代！

LangChain 是一个开源的 Python 库，使任何能够编写代码的人都可以构建 LLM 驱动的应用程序。该包提供了对许多基础模型的通用接口，支持提示管理，并作为其他组件（如提示模板、其他 LLM、外部数据和通过代理的其他工具）的中央接口——在撰写时。

图书馆提供的功能远超本文提到的。随着发展速度的加快，本文在一个月内也可能会过时。

在撰写本文时，我注意到图书馆和文档集中在 OpenAI 的 API 上。虽然许多示例使用的是开源基础模型`[google/flan-t5-xl](https://github.com/hwchase17/langchain/issues/1358)`，但我中途切换到了 OpenAI API。尽管不是免费的，但在本文中使用 OpenAI API 的实验仅花费了大约$1。

# 享受这个故事了吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时获得通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----95fc8898732c--------------------------------) [## 每当 Leonie Monigatti 发布新内容时，你将收到一封电子邮件。

### 每当 Leonie Monigatti 发布新内容时，你会收到一封电子邮件。通过注册，你将创建一个 Medium 账户，如果你还没有的话……

medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----95fc8898732c--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)，[*Twitter*](https://twitter.com/helloiamleonie)*，以及* [*Kaggle*](https://www.kaggle.com/iamleonie)*上找到我！*

# 参考资料

[1] Harrison Chase (2023). [LangChain 文档](https://python.langchain.com/en/latest/index.html)（访问日期：2023 年 4 月 23 日）
