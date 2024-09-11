# 使用大型语言模型的最简单方法？

> 原文：[https://towardsdatascience.com/the-easiest-way-to-interact-with-language-models-4da158cfb5c5?source=collection_archive---------0-----------------------#2023-03-26](https://towardsdatascience.com/the-easiest-way-to-interact-with-language-models-4da158cfb5c5?source=collection_archive---------0-----------------------#2023-03-26)

## LangChain 概述

[](https://sophiamyang.medium.com/?source=post_page-----4da158cfb5c5--------------------------------)[![Sophia Yang, Ph.D.](../Images/c133f918245ea4857dc46df3a07fc2b1.png)](https://sophiamyang.medium.com/?source=post_page-----4da158cfb5c5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4da158cfb5c5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4da158cfb5c5--------------------------------) [Sophia Yang, Ph.D.](https://sophiamyang.medium.com/?source=post_page-----4da158cfb5c5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fae9cae9cbcd2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-easiest-way-to-interact-with-language-models-4da158cfb5c5&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=post_page-ae9cae9cbcd2----4da158cfb5c5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4da158cfb5c5--------------------------------) ·7分钟阅读·2023年3月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4da158cfb5c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-easiest-way-to-interact-with-language-models-4da158cfb5c5&user=Sophia+Yang%2C+Ph.D.&userId=ae9cae9cbcd2&source=-----4da158cfb5c5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4da158cfb5c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-easiest-way-to-interact-with-language-models-4da158cfb5c5&source=-----4da158cfb5c5---------------------bookmark_footer-----------)

LangChain 是与大型语言模型交互和构建应用程序的最简单方法吗？它是一个**开源**工具，并且最近添加了 ChatGPT 插件。它提供了许多我认为有用的功能：

✨ 与包括 OpenAI、Cohere、Huggingface 在内的各种 LLM 提供商集成。

✨ 使用你自己的文档创建一个问答或文本摘要机器人

✨ 提供 OpenAI ChatGPT Retriever 插件

✨ 使用 LangChain Memory 处理聊天记录

✨ 将各种 LLM 结合在一起，并使用一系列工具，如 Google 搜索、Python REPL 等。

✨ 还有更多功能

更令人惊讶的是，LangChain 是**开源**的，这是一项社区努力。在这篇博客文章中，我将介绍我认为 LangChain 中有用的 6 个功能。

你可以在我的 [Github repo here](https://github.com/sophiamyang/tutorials-LangChain/blob/main/LangChain_Overview.ipynb) 找到这篇博客文章的代码。你需要安装所需的包，导入包，并设置 API 密钥（请参见 [my notebook](https://github.com/sophiamyang/tutorials-LangChain/blob/main/LangChain_Overview.ipynb) 中的第 1–3 个单元格）然后再运行下面的代码。

# **1\. LangChain 与许多 LLM 提供商集成**

使用相同的接口，你可以访问来自多个 LLM 提供商的许多 LLM 模型，包括 OpenAI、Cohere、AI21、Huggingface Hub、Azure OpenAI、Manifest、Goose AI、Writer、Banana、Modal、StochasticAI、Cerebrium、Petals、Forefront AI、PromptLayer OpenAI、Anthropic、DeepInfra 和自托管模型。

这是访问 OpenAI ChatGPT 模型和 GPT3 模型、Cohere 的 command-xlarge 模型，以及托管在 Hugging Face Hub 上的 Google 的 Flan T5 模型的示例。

```py
chatgpt = ChatOpenAI(model_name='gpt-3.5-turbo')
gpt3 = OpenAI(model_name='text-davinci-003')
cohere = Cohere(model='command-xlarge')
flan = HuggingFaceHub(repo_id="google/flan-t5-xl")
```

然后我们可以给每个模型提供一个提示，看看它们返回什么。需要注意的是，对于聊天模型，我们可以指定消息是人类消息、AI 消息还是系统消息。这就是为什么对于 ChatGPT 模型，我将我的消息指定为人类消息。

![](../Images/bd4593009c4667ef1c8fd92a9f67c520.png)

# 2\. 外部文档的问答

你对使用外部文档构建问答机器人感兴趣吗？你的文档保存在 PDF、txt 文件，甚至 Notion 中吗？那如果我还想要基于 YouTube 视频进行问答怎么办？

没问题，LangChain 能满足你的需求。LangChain 提供了许多文档加载器：文件加载器、目录加载器、Notion、ReadTheDocs、HTML、PDF、PowerPoint、电子邮件、GoogleDrive、Obsidian、Roam、EverNote、YouTube、Hacker News、GitBook、S3 文件、S3 目录、GCS 文件、GCS 目录、Web 基础、IMSDb、AZLyrics、College Confidential、Gutenberg、Airbyte Json、CoNLL-U、iFixit、Notebook、Copypaste、CSV、Facebook Chat、图片、Markdown、SRT、Telegram、URL、Word 文档、Blackboard。你有其他没有列出的来源吗？因为 LangChain 是开源的，你可以贡献并添加到 LangChain 中！

这里是一个示例，我们使用 TextLoader 加载本地 txt 文件，创建 VectoreStore 索引，并查询你的索引。直观地说，你的文档将被拆分并嵌入到向量中，并存储在向量数据库中。查询将找到在向量数据库中最接近问题向量的向量。

![](../Images/60aed63ceea8b70ddd55e25a959101f4.png)

令人兴奋的消息是LangChain最近集成了**ChatGPT Retrieval Plugin**，因此人们可以使用这个检索器代替索引。索引和检索器有什么区别？根据[LangChain](https://blog.langchain.dev/retrieval/)， “索引是一种支持高效搜索的数据结构，而检索器是使用索引来查找并返回与用户查询相关的文档的组件。索引是检索器执行其功能所依赖的关键组件。”

# 3\. 保留/总结聊天历史

当您与ChatGPT聊天时，它不会保留您的聊天历史记录。您需要将聊天内容复制并粘贴到新的提示中，以便它能够了解历史记录。LangChain通过提供处理聊天历史记录的几种不同选项来解决这个问题——保留所有对话、保留最新的k次对话、总结对话以及上述选项的组合。

这是一个示例，我们使用`ConversationBufferWindowMemory`来保持最新的1轮对话，使用`ConversationSummaryMemory`来总结之前的对话，并使用`CombinedMemory`来结合这两种方法：

```py
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferWindowMemory, CombinedMemory, ConversationSummaryMemory

conv_memory = ConversationBufferWindowMemory(
    memory_key="chat_history_lines",
    input_key="input",
    k=1
)

summary_memory = ConversationSummaryMemory(llm=OpenAI(), input_key="input")
# Combined
memory = CombinedMemory(memories=[conv_memory, summary_memory])
_DEFAULT_TEMPLATE = """The following is a friendly conversation between a human and an AI. The AI is talkative and provides lots of specific details from its context. If the AI does not know the answer to a question, it truthfully says it does not know.

Summary of conversation:
{history}
Current conversation:
{chat_history_lines}
Human: {input}
AI:"""
PROMPT = PromptTemplate(
    input_variables=["history", "input", "chat_history_lines"], template=_DEFAULT_TEMPLATE
)
llm = OpenAI(temperature=0)
conversation = ConversationChain(
    llm=llm, 
    verbose=True, 
    memory=memory,
    prompt=PROMPT
)
```

这里是结果，您可以看到对话摘要以及最新对话传递到提示中的内容。

![](../Images/a17e134f38ad90bd1124fb108d84168b.png)

# 4\. 链接

有时您可能想要链式连接不同的LLM。例如，

+   您可能希望第一个LLM的输出作为第二个LLM的输入。

+   或者，您可能希望总结第一和第二个LLM的输出，并将摘要作为输入传递给第三个LLM。

+   或者，您可能希望将您的语言模型与另一个实用工具链进行链式操作，以进行数学运算或Python操作。

这是最基本的`SimpleSequentialChain`方法的一个示例。第一个链要求ChatGPT为一家制造彩色袜子的公司提供一个好的产品名称。ChatGPT返回了“Rainbow Sox Co.”。第二个链使用这个名称作为输入，并要求ChatGPT为公司“Rainbow Sox Co.”编写一句广告语。ChatGPT写道：“让Rainbow Sox Co.为您的步伐增添一抹色彩！”

```py
from langchain.chat_models import ChatOpenAI
from langchain import LLMChain
from langchain.prompts.chat import (
    ChatPromptTemplate,
    HumanMessagePromptTemplate,
)
human_message_prompt = HumanMessagePromptTemplate(
        prompt=PromptTemplate(
            template="What is a good name for a company that makes {product}?",
            input_variables=["product"],
        )
    )
chat_prompt_template = ChatPromptTemplate.from_messages([human_message_prompt])
chat = ChatOpenAI(temperature=0.9)
chain = LLMChain(llm=chat, prompt=chat_prompt_template)
second_prompt = PromptTemplate(
    input_variables=["company_name"],
    template="Write a catchphrase for the following company: {company_name}",
)
chain_two = LLMChain(llm=llm, prompt=second_prompt)
from langchain.chains import SimpleSequentialChain
overall_chain = SimpleSequentialChain(chains=[chain, chain_two], verbose=True)

# Run the chain specifying only the input variable for the first chain.
catchphrase = overall_chain.run("colorful socks")
print(catchphrase)
```

![](../Images/3474d1f20325434ecbcfde3f5b498db0.png)

# 5\. 代理

代理可以访问语言模型和一套工具，例如Google搜索、Python REPL、数学计算器等。代理使用LLM和各种框架来决定为哪些任务使用哪些工具。这与ChatGPT插件非常相似。以下是一个示例：

在这个示例中，我们加载了两个工具：serpapi用于Google搜索，llm-math用于数学计算。我们询问了“莱昂纳多·迪卡普里奥的女朋友是谁？她目前的年龄提高到0.43的幂是多少？”语言模型首先理解了问题，并决定执行搜索操作以触发搜索API调用。然后它从搜索代理那里获取了结果，决定使用llm-math进行计算，最后得到了最终结果。

![](../Images/1aa0b362e5fa6c8d2fe819ab5c42c647.png)

在另一个例子中，我们可以使用 Python REPL 执行 Python 代码进行计算，而不是使用 llm-math 作为计算器：

![](../Images/af00b22b37ab09942dea8a6b48b74772.png)

# 6\. ChatGPT 插件

ChatGPT 插件刚刚在本周推出，而 LangChain 已经添加了 ChatGPT 插件功能。这里是一个例子，我们使用 `AIPluginTool` 从购物网站加载插件。这个插件与 OpenAI 使用的完全相同。当我们问“在 Klarna 上有哪些 T 恤？”时，LLM 决定使用 Klarna 插件并得到了一个 OpenAI 规范系列的响应，然后决定发起请求以获取数据，最终返回了很好的答案。

```py
from langchain.chat_models import ChatOpenAI
from langchain.agents import load_tools, initialize_agent
from langchain.tools import AIPluginTool
tool = AIPluginTool.from_plugin_url("https://www.klarna.com/.well-known/ai-plugin.json")
llm = ChatOpenAI(temperature=0)
tools = load_tools(["requests"] )
tools += [tool]

agent_chain = initialize_agent(tools, llm, agent="zero-shot-react-description", verbose=True)

agent_chain.run("what t shirts are available in klarna?")
```

![](../Images/23b73b914df2773ae8e336adefc90589.png)

# 结论

总体而言，我对 LangChain 的能力和社区快速添加新功能的速度感到非常印象深刻。还有许多其他功能我在这篇博客文章中没有提到，包括数据增强、少量学习、评估、模型比较等。我希望这篇博客文章能帮助你更好地理解各种 LangChain 功能。祝你探索愉快！

![](../Images/efc10ee5a6acd9672df91dd7b66ed549.png)

照片由 [Aida L](https://unsplash.com/@aidamarie_photography?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上

. . .

由 [Sophia Yang](https://www.linkedin.com/in/sophiamyang/) 于 2023 年 3 月 26 日发布

Sophia Yang 是 Anaconda 的高级数据科学家。通过 [LinkedIn](https://www.linkedin.com/in/sophiamyang/)、[Twitter](https://twitter.com/sophiamyang) 和 [YouTube](https://www.youtube.com/SophiaYangDS) 与我联系，并加入 DS/ML [读书俱乐部](https://dsbookclub.github.io/) ❤️
