# 🦜🔗 LangChain：开发由语言模型驱动的应用程序

> 原文：[`towardsdatascience.com/develop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a`](https://towardsdatascience.com/develop-applications-powered-by-language-models-with-langchain-d2f7a1d1ad1a)

![](img/eadb36548cf05e78b4161169a551f8aa.png)

图片由 [Choong Deng Xiang](https://unsplash.com/@dengxiangs?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 开始使用 LangChain 和 Python 利用 LLM

[](https://medium.com/@marcellopoliti?source=post_page-----d2f7a1d1ad1a--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----d2f7a1d1ad1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d2f7a1d1ad1a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2f7a1d1ad1a--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----d2f7a1d1ad1a--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d2f7a1d1ad1a--------------------------------) ·阅读时间 12 分钟·2023 年 4 月 26 日

--

# 介绍

**LangChain 是一个框架，使得利用大型语言模型（如 GPT-3）快速而轻松地开发应用程序成为可能**。

然而，这个框架引入了额外的可能性，例如，轻松使用外部数据源，如维基百科，以增强模型提供的能力。我相信你们可能都尝试过使用 Chat-GPT，并发现它无法回答某个日期之后发生的事件。在这种情况下，维基百科的搜索可以帮助 GPT 回答更多问题。

# LangChain 结构

该框架分为六个模块，每个模块允许你管理与 LLM 交互的不同方面。让我们看看这些模块是什么。

+   [**模型**](https://python.langchain.com/en/latest/modules/models.html)：允许你实例化和使用不同的模型。

+   [**提示**](https://python.langchain.com/en/latest/modules/prompts.html)：提示是我们与模型互动以尝试获得输出的方式。现在知道如何编写有效的提示至关重要。这个框架模块允许我们更好地管理提示。例如，通过创建可以重用的模板。

+   [**索引**](https://python.langchain.com/en/latest/modules/indexes.html)：最好的模型通常是那些与一些文本数据相结合的模型，以便为模型添加上下文或解释某些内容。这个模块帮助我们做到这一点。

+   [**链**](https://python.langchain.com/en/latest/modules/chains.html)：很多时候，单次调用 LLM API 是不够的。该模块允许整合其他工具。例如，一次调用可以是一个复合链，其目的是从维基百科获取信息，然后将这些信息作为输入提供给模型。这个模块允许将多个工具串联起来，以解决复杂的任务。

+   [**记忆**](https://python.langchain.com/en/latest/modules/memory.html)：该模块允许我们在模型调用之间创建持久状态。能够使用一个记住过去所说内容的模型，无疑会提高我们的应用程序的效果。

+   [**代理**](https://python.langchain.com/en/latest/modules/agents.html)：代理是一个 LLM，它做出决策、采取行动、对自己所做的事情做出观察，并以这种方式继续，直到完成任务。该模块提供了一组可以使用的代理。

现在让我们更详细地了解一下如何通过利用不同的模块来实现代码。

# 模型

*模型* 允许使用三种不同类型的语言模型，它们是：

+   **大型语言模型（LLMs）：** 这些是能够理解自然语言的基础机器学习模型。这些模型接受字符串作为输入，并生成字符串作为输出。

+   **聊天模型：** 这些模型由 LLM 提供支持，但专门用于与用户聊天。你可以在[这里](https://medium.com/aiguys/reinforcement-learning-from-human-feedback-instructgpt-and-chatgpt-693d00cb9c58)阅读更多信息。

+   **文本嵌入模型：** 这些模型用于将文本数据投影到几何空间中。这些模型将文本作为输入，并返回一个数字列表，即文本的嵌入。

![](img/81937b2be62eb65b9c3931398deda8af.png)

Open AI API 密钥

让我们开始使用这个模块。首先，我们需要安装并导入库。要使用这个库，你需要一个来自 Open AI 网站的 API 密钥。

```py
!pip install langchain >> null
!pip install openai >> null
```

```py
from langchain.llms import OpenAI
```

```py
#past you api key here

import os
os.environ['OPENAI_API_KEY'] = "yuor-openai-key"
```

现在我们准备实例化一个 LLM 模型。

```py
llm = OpenAI(model_name="text-ada-001", n=2, best_of=2)
```

```py
llm("tell me a story please.")
```

***我的输出****:* *一个年轻的女人去了一个她从未听说过的狂欢派对。她是那里唯一一个在黑暗中带有光亮的人，也是唯一一个能够看到人们美好的一面的人。她喝酒、跳舞，做了所有可能的事情来让这一切发生。在几个小时的舞蹈和饮酒之后，她认识了派对上的一个人。他是一个有些吸引人的男人，留着胡须和山羊胡。她说：“嗨，我是那里唯一一个在黑暗中带有光亮的人。”他说：“嗨，我是那里唯一一个在黑暗中带有光亮的人。”*

使用 generate() 方法，你还可以输入一个列表并接收多个答案，我们来看看怎么做。

```py
llm_result = llm.generate(["Tell me a short story", "whath is your favourite colour?", "Is the earth flat?"])
```

![](img/2a2fbc6563852fd6091ef5140276f8b6.png)

llm.generate 输出

你还可以提取有关大型语言模型结果的一些额外信息。

```py
llm_result.llm_output
```

***我的输出：*** *{‘token_usage’: {‘completion_tokens’: 527, ‘total_tokens’: 544, ‘prompt_tokens’: 17}, ‘model_name’: ‘text-ada-001’}*

LLMs 无法理解过长的输入文本。特别是，包含太多标记的文本（如果你不知道标记是什么，可以考虑单词的音节）。在将字符串传递给模型之前，你可以使用简单的方法估计标记的数量。

为了做到这一点，你需要安装[tiktoken](https://github.com/openai/tiktoken)库。

```py
!pip install tiktoken >> None
import tiktoken
```

```py
llm.get_num_tokens("How many old are you?")

#OUTPUT : 6
```

# 提示

提示是编程 NLP 模型的新方式。然而，创建一个好的提示并非易事。以不同的方式提问可能会导致不同的结果，这些结果可能更准确也可能不准确。提示也可以根据你所面对的使用案例而有所不同。让我们看看这个模块如何帮助我们创建一个好的提示。

## 提示模板

正如名称所示，提示模板允许我们创建可以重复使用的模板，以便向我们的模型提出问题。模板将包含变量，这些变量是用户会不时更改的唯一内容，以使提示适应其特定的使用案例。

现在让我们看看它们如何被使用。

```py
from langchain import PromptTemplate

template = """
I want you to act as businessman.

You have few passions in your life which are:

- Money
- Data
- Basketball

I want to write a Medium Blog post about {product}.
What is a good for a title of such post?
"""

prompt = PromptTemplate(
    input_variables=["product"],
    template=template,
)
```

现在我们可以用我们想要的任何字符串替换提示中的变量‘product’。这样我们就可以根据自己的需要自定义提示。

```py
prompt.format(product = "how to make a bunch of money")
```

***我的输出：*** *我希望你充当商人。你生活中有几个热情所在：- 钱- 数据- 篮球。我想写一篇关于如何赚一大笔钱的 Medium 博客文章。这样的文章标题应该是什么？*

已经有一些模板被编写好，你可以直接导入它们。要了解这些模板是什么，你可以查看文档。我们现在来尝试导入一个。

```py
from langchain.prompts import load_prompt

prompt = load_prompt("lc://prompts/conversation/prompt.json")
prompt
```

***我的输出：*** *PromptTemplate(input_variables=[‘history’, ‘input’], output_parser=None, partial_variables={}, template=’以下是人类和 AI 之间的友好对话。AI 很健谈，并从其上下文中提供了许多具体细节。如果 AI 不知道问题的答案，它会如实地说不知道。\n\n 当前对话：\n{history}\n 人类：{input}\nAI：’, template_format=’f-string’, validate_template=True)*

在这个模板中，我们有多个可以填写的变量。其中之一是*history*。我们需要历史记录来告诉模板一些先前发生的事情，以便它有更多的上下文。如果我们想从这个模板中请求模型的输出，这非常简单。

```py
llm(prompt.format(history="", input="What is 3 - 3?"))
```

## 少量示例

有时候我们想向机器学习模型询问特别棘手的事情。此时，获得更准确回答的一种方法是向模型展示正确答案的类似示例，然后提出我们的问题。LangChain 提供了一种保存专门用于保存这些示例的模板的方法。让我们看看如何做到这一点。

首先，让我们创建一些示例。如果我想创建单词的最高级：tall -> tallest。

```py
from langchain import PromptTemplate, FewShotPromptTemplate

# First, create the list of few shot examples.
examples = [
    {"word": "cool", "superlative": "coolest"},
    {"word": "tall", "superlative": "tallest"},
]
```

现在我们创建模板，如之前所做的那样，包含两个变量，一个用于基本词，一个用于最高级词。

```py
# Next, we specify the template to format the examples we have provided.
# We use the `PromptTemplate` class for this.
example_formatter_template = """
Word: {word}
Superlative: {superlative}\n
"""
example_prompt = PromptTemplate(
    input_variables=["word", "superlative"],
    template=example_formatter_template,
)
```

现在我们将一切结合起来，使用 FewShotPromptTemplate 类，它接受示例、模板、前缀（通常是我们希望给模板的指令）和一个后缀（即模板输出的形式）作为输入。

```py
# Finally, we create the `FewShotPromptTemplate` object.
few_shot_prompt = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_prompt,

    prefix="Give the superlative of every input",

    suffix="Word: {input}\nSuperlative:",

    input_variables=["input"],
    example_separator="\n\n",
)
```

```py
print(few_shot_prompt.format(input="big"))
```

***我的输出：*** *给出每个输入单词的最高级：cool 最高级：coolest 单词：tall 最高级：tallest 单词：big 最高级：*

现在你可以输入模型并获得实际输出。

```py
llm(few_shot_prompt.format(input="large"))
```

# 索引

这个模块允许我们与外部文档交互，我们想要将其提供给模型。这个模块基于 Retriever 的概念。实际上，我们最常做的就是去获取最能回答我们查询的文档。因此，这是一个信息检索系统。让我们看看 Retriever 接口的样子，以便更好地理解它。（对于那些还不知道的人，接口是一个不能实例化的类，如果你想了解更多，你可以阅读我关于[设计模式](https://medium.com/towards-data-science/design-patterns-with-python-for-machine-learning-engineers-observer-23cde7ecb2ed)的文章。）

```py
from abc import ABC, abstractmethod
from typing import List
from langchain.schema import Document

class BaseRetriever(ABC):
    @abstractmethod
    def get_relevant_documents(self, query: str) -> List[Document]:
        """Get texts relevant for a query.

        Args:
            query: string to find relevant texts for

        Returns:
            List of relevant documents
        """
```

*get_relevant_documents*方法非常简单，你只需了解如何阅读英语即可理解它的作用。字符串可以根据你的喜好进行更改，所以如果你想修改或实现一个自定义的 Retriever，也没有什么复杂的。

现在我们来看一个实际的例子。我们想要创建一个关于特定文档的问答应用程序。也就是说，模型需要能够回答我关于特定文档的问题。

我们首先需要安装*Chroma*，它允许我们与*Vectorstores*一起工作，我们稍后会看到它的用途。

```py
!pip install chromadb >> null
```

让我们导入一些我们需要的类。

```py
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI
```

现在让我们从网络上下载一个 txt 文档。

```py
#download data
import requests

url = "https://raw.githubusercontent.com/hwchase17/langchain/master/docs/modules/state_of_the_union.txt"
response = requests.get(url)
data = response.text
with open("state_of_the_union.txt", "w") as text_file:
    text_file.write(data)
```

让我们使用 TextLoader 加载文档。

```py
from langchain.document_loaders import TextLoader
loader = TextLoader('state_of_the_union.txt', encoding='utf8')
```

*Retriever* 总是依赖于所谓的*Vectorstore*检索器。因此，我们可以实例化一个*Vectorstore retriever*并将我们的文本加载器传递给它。

```py
index = VectorstoreIndexCreator().from_loaders([loader])
```

现在我们终于有了索引，我们可以对数据提问了。

```py
query = "What did Ohio Senator Sherrod Brown say?"
index.query(query)
```

***我的输出：*** *俄亥俄州参议员谢罗德·布朗说，“是时候埋葬‘铁锈带’这个标签了。”*

# 链

链允许我们创建更复杂的应用程序。**仅仅使用 LLM 通常是不够的**，我们想要做得更多。例如，我们首先创建一个模板，然后将编译好的模板作为输入提供给 LLM，这可以通过链简单实现。对我来说，可以将链想象成你在 scikit-learn 中使用的 Pipeline。

LLMChain 就是一个这样的链，它接受输入，将其格式化到模板中，然后将其作为输入传递给模板。

首先，让我们创建一个简单的模板。

```py
from langchain.prompts import PromptTemplate
from langchain.llms import OpenAI

llm = OpenAI(temperature=0.9)
prompt = PromptTemplate(
    input_variables=["product"],
    template="What is a good name for a website that sells {product}?",
)
```

现在我们通过指定要使用的模板和模型来创建一个链。

```py
from langchain.chains import LLMChain
chain = LLMChain(llm=llm, prompt=prompt)

#run the chain with the input needed for the prompt
print(chain.run("paints"))
```

你也可以创建一个自定义链，但我会写一篇更详细的文章来讲解。

# 内存

每次我们与模型互动时，它都会给我们一个答案，这个答案不会依赖于上下文，因为它不会记住过去的事件。这就像每次我们都是在与一个新的模型交谈一样。**在应用程序中，我们通常希望模型具备某种记忆，以便它通过根据之前我们说的话不断改进来学习如何回复我们**，特别是当我们在开发聊天机器人时。这就是这个模块的用途。

内存可以通过多种方式实现。例如，我们可以将之前的 N 条消息作为一个字符串或字符串序列提供给模型。现在让我们看看我们可以实现的最简单类型的内存，称为缓冲区。

我们可以使用一个 ChatMessageHistory 类，它允许我们轻松保存所有发送给模型的消息。

```py
from langchain.memory import ChatMessageHistory
```

```py
history = ChatMessageHistory()history.add_user_message("hello friend!")history.add_ai_message("how are you?")
```

现在我们可以轻松检索消息。

```py
history.messages
```

现在我们理解了这个概念，我们可以使用我们刚刚使用的类的包装器，称为 ConversationBufferMemory，它允许我们实际使用消息历史记录。

```py
from langchain.memory import ConversationBufferMemory
```

```py
memory = ConversationBufferMemory()
memory.chat_memory.add_user_message("hello friend!")
memory.chat_memory.add_ai_message("how are you?")
```

```py
memory.load_memory_variables({})
```

最后，我们来看看如何在对话链中使用这个功能。

所以让我们开始与模型进行对话。

```py
from langchain.llms import OpenAI
from langchain.chains import ConversationChain

llm = OpenAI(temperature=0)
conversation = ConversationChain(
    llm=llm, 
    verbose=True, 
    memory=ConversationBufferMemory()
)
```

```py
conversation.predict(input="Hello friend!")
```

![](img/7836436ee44205cce342194f148231e1.png)

```py
conversation.predict(input="I would like to discuss about the universe")
```

![](img/a108320e04ad98ef250e62891677400d.png)

```py
conversation.predict(input="Whats your favourite planet?")
```

![](img/52ae722345c2a49f47f024a439091eb0.png)

你可以通过访问内存来检索旧消息。

```py
conversation.memory
```

现在你可以保存你的消息，以便在你想从某个特定点开始对话时重新加载它们并提供给模型。

# 代理

我们看到的那种链条，遵循像流水线一样的预定步骤。但是我们通常不知道模型回答特定问题时需要采取哪些步骤，因为这也取决于用户不时给出的回答。

我们可以让模型使用各种工具来改进其回答。一个常见的例子是首先去维基百科上阅读一些信息，然后回答一个特定的问题。

```py
from langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType
from langchain.llms import OpenAI
```

我们将使用两个特定的工具，SERPAPI，它允许模型进行浏览器搜索并获取信息，以及 llm-math，以提高其数学技能。

要安装 SERPAPI，你必须在网站上注册并复制 API Token。

这是网站：[`serpapi.com/`](https://serpapi.com/)

完成后，我们安装库并将令牌设置为环境变量。

```py
!pip install google-search-results >> null
os.environ['SERPAPI_API_KEY'] = "your token here"
```

现在我们已经准备好使用它所需的工具来实例化我们的代理了。

因此，我们需要 3 样东西：

+   LLM：我们想要使用的大型语言模型

+   工具：我们希望用来改进基本 LLM 的工具

+   代理：处理 LLM 与工具之间的互动

```py
llm = OpenAI(model_name="text-ada-001")
```

```py
tools = load_tools(["serpapi", "llm-math"], llm=llm)
```

```py
agent = initialize_agent(tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True)
```

现在我们可以向模型提问，依靠它将使用各种可用的工具来回答我们。

```py
agent.run("How old is Joe Biden? What is his current age raised to the 0.56 power?")
```

# 结论

在这篇文章中，我们介绍了 LangChain 及其各种模块。每个模块都对提高大型语言模型的能力有用，并且对于基于这些模型开发应用程序至关重要。请注意，因为我在本文中使用的模型不是最好的，所以回答可能不是最优的，而且你得到的回答可能与我的差异很大。不过，目的是为了熟悉这个库。我很期待看到未来基于新大型语言模型开发的所有应用程序。关注我，阅读我即将发布的关于这些话题的深入文章！ [😉](https://emojipedia.org/it/apple/ios-15.4/faccina-che-fa-l-occhiolino/)

# 结束

*Marcello Politi*

[Linkedin](https://www.linkedin.com/in/marcello-politi/)，[Twitter](https://twitter.com/_March08_)， [Website](https://marcello-politi.super.site/)
