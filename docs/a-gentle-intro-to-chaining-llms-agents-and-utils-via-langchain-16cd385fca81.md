# 《温和介绍：通过 LangChain 链接 LLMs、代理和工具》

> 原文：[https://towardsdatascience.com/a-gentle-intro-to-chaining-llms-agents-and-utils-via-langchain-16cd385fca81?source=collection_archive---------0-----------------------#2023-04-21](https://towardsdatascience.com/a-gentle-intro-to-chaining-llms-agents-and-utils-via-langchain-16cd385fca81?source=collection_archive---------0-----------------------#2023-04-21)

## *#初学者的 LLM*

## 理解代理、工具和提示的基础知识以及一些学习经验

[](https://varshitasher.medium.com/?source=post_page-----16cd385fca81--------------------------------)[![Varshita Sher 博士](../Images/a3f2e9bf1dc1d8cbe018e54f9341f608.png)](https://varshitasher.medium.com/?source=post_page-----16cd385fca81--------------------------------)[](https://towardsdatascience.com/?source=post_page-----16cd385fca81--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----16cd385fca81--------------------------------) [Varshita Sher 博士](https://varshitasher.medium.com/?source=post_page-----16cd385fca81--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff8ca36def59&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-intro-to-chaining-llms-agents-and-utils-via-langchain-16cd385fca81&user=Dr.+Varshita+Sher&userId=f8ca36def59&source=post_page-f8ca36def59----16cd385fca81---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----16cd385fca81--------------------------------) ·20分钟阅读·2023年4月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F16cd385fca81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-intro-to-chaining-llms-agents-and-utils-via-langchain-16cd385fca81&user=Dr.+Varshita+Sher&userId=f8ca36def59&source=-----16cd385fca81---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F16cd385fca81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-intro-to-chaining-llms-agents-and-utils-via-langchain-16cd385fca81&source=-----16cd385fca81---------------------bookmark_footer-----------)

> 受众：对于那些被庞大（但卓越）库感到不知所措的人…

![](../Images/4c6806a457e762ae481cc1f29a02d4bd.png)

作者使用[DALL.E 2](https://openai.com/product/dall-e-2)生成的图像

# 介绍

如果我说我掌握了整个 LangChain 库，那我就是在撒谎——实际上，我远远没有做到。但是，围绕它的热议足以让我摆脱写作 hiatus，去尝试一下 🚀。

最初的动机是看看LangChain在实践中添加了什么（在实际水平上），这使它不同于上个月我用`openai`包中的`ChatCompletion.create()`函数构建的聊天机器人。在这样做的过程中，我意识到需要先理解LangChain的基础构建块，然后再转向更复杂的部分。

这就是本文所做的事情。请注意，随着我对这个库的着迷和持续探索，将会有更多的部分出现。

让我们从理解LangChain的基本构建块 —— 即链条开始。如果你想跟进，请查看这个[GitHub仓库](https://github.com/V-Sher/LangChain-Tutorial)。

## LangChain中的链条是什么？

链条是通过以逻辑方式连接一个或多个大型语言模型（LLMs）而得到的。 （虽然链条可以由除LLMs以外的实体构建，但现在让我们暂时使用这个定义以简化问题）。

OpenAI是一种LLM（提供者），你可以使用它，但还有其他像Cohere、Bloom、Huggingface等。

*注意：几乎所有这些LLM提供者都需要您申请API密钥才能使用它们。所以请确保在继续阅读本博客的其余部分之前，您已经这样做了。例如：*

```py
import os
os.environ["OPENAI_API_KEY"] = "..."
```

*P.S. 我将在本教程中使用OpenAI，因为我有一个一个月后过期的积分密钥，但请随意替换为任何其他LLM。无论如何，这里涵盖的概念都将是有用的。*

链条可以简单（例如通用）或专业化（例如实用）。

1.  通用 — 单个LLM是最简单的链条。它接受一个输入提示和LLM的名称，然后使用LLM进行文本生成（即输出提示的结果）。这里是一个例子：

## 让我们构建一个基本的链条 —— 创建一个提示并获取预测结果

在Lanchain中，使用`PromptTemplate`创建提示（Prompt）有点花哨，但这可能是因为根据用例的不同，可以有多种不同的方式来创建提示（我们将涵盖`AIMessagePromptTemplate`等等）。

`HumanMessagePromptTemplate`等等将在下一篇博客文章中涵盖。现在先看一个简单的例子：

```py
from langchain.prompts import PromptTemplate

prompt = PromptTemplate(
    input_variables=["product"],
    template="What is a good name for a company that makes {product}?",
)

print(prompt.format(product="podcast player"))

# OUTPUT
# What is a good name for a company that makes podcast player?
```

*注意：如果您需要多个* `*input_variables*`，例如：* `*input_variables=["product", "audience"]*` *用于模板，例如* `*“一个公司的好名字，为{product}制作{audience}”*`，则需要执行* `print(prompt.format(product="podcast player", audience="children”)` *以获取更新后的提示。

一旦您建立了一个提示，我们就可以调用所需的LLM。为此，我们创建一个`LLMChain`实例（在我们的例子中，我们使用`OpenAI`的大型语言模型`text-davinci-003`）。要获取预测结果（即AI生成的文本），我们使用`run`函数和`product`的名称。

```py
from langchain.llms import OpenAI
from langchain.chains import LLMChain

llm = OpenAI(
          model_name="text-davinci-003", # default model
          temperature=0.9) #temperature dictates how whacky the output should be
llmchain = LLMChain(llm=llm, prompt=prompt)
llmchain.run("podcast player")

# OUTPUT
# PodConneXion
```

如果你有多个输入变量，那么就不能使用`run`。相反，你需要将所有变量作为`dict`传递。例如，`llmchain({"product": "podcast player", "audience": "children"})`。

*注意 1：根据* [*OpenAI*](https://openai.com/blog/introducing-chatgpt-and-whisper-apis)，`*davinci*` *文本生成模型的费用是其聊天对应模型的 10 倍，即* `*gpt-3.5-turbo*`*，因此我尝试从文本模型切换到聊天模型（即从* `*OpenAI*` *到* `*ChatOpenAI*`*），结果差别不大。*

*注意 2：你可能会看到一些教程使用* `*OpenAIChat*`*而不是* `*ChatOpenAI*`*。前者已经* [*弃用*](https://github.com/hwchase17/langchain/issues/1556#issuecomment-1463224442) *并且将不再受支持，我们应使用* `*ChatOpenAI*`*。*

```py
from langchain.chat_models import ChatOpenAI

chatopenai = ChatOpenAI(
                model_name="gpt-3.5-turbo")
llmchain_chat = LLMChain(llm=chatopenai, prompt=prompt)
llmchain_chat.run("podcast player")

# OUTPUT
# PodcastStream
```

这部分关于简单链的介绍到此为止。需要注意的是，我们很少将通用链作为独立链使用。更常见的是它们作为实用链的构建块（正如我们接下来会看到的）。

2\. 实用工具 — 这些是专门的链，由许多 LLM 组成，以帮助解决特定任务。例如，LangChain 支持一些端到端的链（如`[AnalyzeDocumentChain](https://python.langchain.com/docs/use_cases/question_answering/how_to/analyze_document)` 用于总结、问答等）和一些特定的链（如`[GraphQnAChain](https://python.langchain.com/en/latest/modules/chains/index_examples/graph_qa.html#querying-the-graph)` 用于创建、查询和保存图形）。在本教程中，我们将深入探讨一个名为 `PalChain` 的特定链。

PAL 代表 [程序辅助语言模型](https://arxiv.org/pdf/2211.10435.pdf)。`PALChain` 读取复杂的数学问题（用自然语言描述）并生成程序（用于解决数学问题）作为中间推理步骤，但将解决步骤委托给如 Python 解释器等运行时。

为了确认这一点，我们可以检查基础代码中的 `_call()` [这里](https://github.com/hwchase17/langchain/blob/master/langchain/chains/pal/base.py)。在底层，我们可以看到这个链：

+   [首先使用一个通用链](https://github.com/hwchase17/langchain/blob/master/langchain/chains/pal/base.py#L58) `[LLMChain](https://github.com/hwchase17/langchain/blob/master/langchain/chains/pal/base.py#L58)` [来理解我们传递的查询](https://github.com/hwchase17/langchain/blob/master/langchain/chains/pal/base.py#L58) 并获取预测。因此，这个链在初始化时需要传递一个 LLM（我们将使用之前的 OpenAI LLM）。

+   [第二，使用 Python REPL](https://github.com/hwchase17/langchain/blob/master/langchain/chains/pal/base.py#L63-L64) 来解决 LLM 输出的函数/程序。

*附注：检查* `*_call()*` *在* `*base.py*` *中是一个好习惯，可以查看 LangChain 中的任何链如何在底层工作。*

```py
from langchain.chains import PALChain
palchain = PALChain.from_math_prompt(llm=llm, verbose=True)
palchain.run("If my age is half of my dad's age and he is going to be 60 next year, what is my current age?")

# OUTPUT
# > Entering new PALChain chain...
# def solution():
#    """If my age is half of my dad's age and he is going to be 60 next year, what is my current age?"""
#    dad_age_next_year = 60
#    dad_age_now = dad_age_next_year - 1
#    my_age_now = dad_age_now / 2
#    result = my_age_now
#    return result
#
# > Finished chain.
# '29.5'
```

*注意1：如果你不需要看到中间步骤，`*verbose*` *可以设置为* `*False*`*。*

现在，有些人可能会想 — *但提示呢？我们肯定没有像我们建立的通用* `*llmchain*` *那样传递它。* 实际上，当使用`.from_math_prompt()`时，它会自动加载。您可以使用`palchain.prompt.template`检查默认提示，或者直接查看提示文件[这里](https://github.com/hwchase17/langchain/blob/master/langchain/chains/pal/math_prompt.py)。

```py
print(palchain.prompt.template)
# OUTPUT
# 'Q: Olivia has $23\. She bought five bagels for $3 each. How much money does she have left?\n\n# solution in Python:\n\n\ndef solution():\n    """Olivia has $23\. She bought five bagels for $3 each. How much money does she have left?"""\n    money_initial = 23\n    bagels = 5\n    bagel_cost = 3\n    money_spent = bagels * bagel_cost\n    money_left = money_initial - money_spent\n    result = money_left\n    return result\n\n\n\n\n\nQ: Michael had 58 golf balls. On tuesday, he lost 23 golf balls. On wednesday, he lost 2 more. How many golf balls did he have at the end of wednesday?\n\n# solution in Python:\n\n\ndef solution():\n    """Michael had 58 golf balls. On tuesday, he lost 23 golf balls. On wednesday, he lost 2 more. How many golf balls did he have at the end of wednesday?"""\n    golf_balls_initial = 58\n    golf_balls_lost_tuesday = 23\n    golf_balls_lost_wednesday = 2\n    golf_balls_left = golf_balls_initial - golf_balls_lost_tuesday - golf_balls_lost_wednesday\n    result = golf_balls_left\n    return result\n\n\n\n\n\nQ: There were nine computers in the server room. Five more computers were installed each day, from monday to thursday. How many computers are now in the server room?\n\n# solution in Python:\n\n\ndef solution():\n    """There were nine computers in the server room. Five more computers were installed each day, from monday to thursday. How many computers are now in the server room?"""\n    computers_initial = 9\n    computers_per_day = 5\n    num_days = 4  # 4 days between monday and thursday\n    computers_added = computers_per_day * num_days\n    computers_total = computers_initial + computers_added\n    result = computers_total\n    return result\n\n\n\n\n\nQ: Shawn has five toys. For Christmas, he got two toys each from his mom and dad. How many toys does he have now?\n\n# solution in Python:\n\n\ndef solution():\n    """Shawn has five toys. For Christmas, he got two toys each from his mom and dad. How many toys does he have now?"""\n    toys_initial = 5\n    mom_toys = 2\n    dad_toys = 2\n    total_received = mom_toys + dad_toys\n    total_toys = toys_initial + total_received\n    result = total_toys\n    return result\n\n\n\n\n\nQ: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipops did Jason give to Denny?\n\n# solution in Python:\n\n\ndef solution():\n    """Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipops did Jason give to Denny?"""\n    jason_lollipops_initial = 20\n    jason_lollipops_after = 12\n    denny_lollipops = jason_lollipops_initial - jason_lollipops_after\n    result = denny_lollipops\n    return result\n\n\n\n\n\nQ: Leah had 32 chocolates and her sister had 42\. If they ate 35, how many pieces do they have left in total?\n\n# solution in Python:\n\n\ndef solution():\n    """Leah had 32 chocolates and her sister had 42\. If they ate 35, how many pieces do they have left in total?"""\n    leah_chocolates = 32\n    sister_chocolates = 42\n    total_chocolates = leah_chocolates + sister_chocolates\n    chocolates_eaten = 35\n    chocolates_left = total_chocolates - chocolates_eaten\n    result = chocolates_left\n    return result\n\n\n\n\n\nQ: If there are 3 cars in the parking lot and 2 more cars arrive, how many cars are in the parking lot?\n\n# solution in Python:\n\n\ndef solution():\n    """If there are 3 cars in the parking lot and 2 more cars arrive, how many cars are in the parking lot?"""\n    cars_initial = 3\n    cars_arrived = 2\n    total_cars = cars_initial + cars_arrived\n    result = total_cars\n    return result\n\n\n\n\n\nQ: There are 15 trees in the grove. Grove workers will plant trees in the grove today. After they are done, there will be 21 trees. How many trees did the grove workers plant today?\n\n# solution in Python:\n\n\ndef solution():\n    """There are 15 trees in the grove. Grove workers will plant trees in the grove today. After they are done, there will be 21 trees. How many trees did the grove workers plant today?"""\n    trees_initial = 15\n    trees_after = 21\n    trees_added = trees_after - trees_initial\n    result = trees_added\n    return result\n\n\n\n\n\nQ: {question}\n\n# solution in Python:\n\n\n'
```

*注意：大多数实用链条的提示作为库的一部分是预定义的（在这里查看* [*这里*](https://github.com/hwchase17/langchain/tree/master/langchain/chains)*）。它们有时非常详细（即：有很多令牌），因此在成本和LLM响应质量之间肯定存在权衡。*

## 是否存在不需要LLM和提示的链条？

*尽管PalChain需要一个LLM（以及相应的提示）来解析用户用自然语言编写的问题，但在LangChain中有一些链条不需要。这些主要是预处理提示的转换链条，例如删除额外的空格，然后将其输入LLM。你可以在另一个例子中看到* [*这里*](https://python.langchain.com/en/latest/modules/chains/generic/transformation.html)*。*

## 我们能进入精彩部分并开始创建链条吗？

当然可以！我们已经有了开始逻辑地将LLM连接在一起的基本构建块。为此，我们将使用`SimpleSequentialChain`。

文档中有一些很好的例子，例如，你可以在[这里](https://python.langchain.com/en/latest/modules/chains/generic/transformation.html)看到如何组合两个链条，其中链条#1用于清理提示（删除额外空格，缩短提示等），而链条#2用于使用这个干净的提示调用LLM。这里还有[另一个例子](https://js.langchain.com/docs/modules/chains/foundational/sequential_chains/#simplesequentialchain)，其中链条#1用于为一部戏剧生成简介，而链条#2则用于基于此简介撰写评论。

虽然这些都是很好的例子，但我想专注于其他事情。如果你还记得，我提到链条可以由除了LLM以外的实体组成。更具体地说，我对将代理和LLM组合在一起很感兴趣。*但首先，什么是代理？*

## 使用代理动态调用LLM

对于解释代理的作用与其是什么，将会更加容易。

假设我们想知道明天的天气预报。如果我们使用简单的ChatGPT API并给它一个提示`Show me the weather for tomorrow in London`，它不会知道答案，因为它无法访问实时数据。

![](../Images/a379c0d1313089f343dc25f6510660a3.png)

如果我们能有一个安排，利用 LLM 理解我们的查询（即提示），然后代表我们调用天气 API 来获取所需数据，那不是很有用吗？这正是代理所做的（当然还有其他事情）。

> 代理可以访问 LLM 和一套工具，例如 Google 搜索、Python REPL、数学计算器、天气 API 等。

LangChain 支持很多代理——完整列表请见[这里](https://python.langchain.com/docs/modules/agents/agent_types/)，但坦率说，我在教程和 YouTube 视频中最常见的代理是 `zero-shot-react-description`。这个代理使用了[ReAct](https://arxiv.org/abs/2210.03629)（Reason + Act）框架，根据输入查询从工具列表中选择最合适的工具。

*P.S.:* [*这里*](https://tsmatz.wordpress.com/2023/03/07/react-with-openai-gpt-and-langchain/) *有一篇深入探讨 ReAct 框架的好文章。*

让我们使用 `initialize_agent` 初始化一个代理，并传递它所需的 `tools` 和 `LLM`。代理可以使用的工具清单可以在[这里](https://python.langchain.com/docs/integrations/tools/)找到。对于我们的例子，我们使用了上面提到的同一个数学解决工具，叫做 `pal-math`。这个工具在初始化时需要一个 LLM，因此我们传递给它之前相同的 OpenAI LLM 实例。

```py
from langchain.agents import initialize_agent
from langchain.agents import AgentType
from langchain.agents import load_tools

llm = OpenAI(temperature=0)
tools = load_tools(["pal-math"], llm=llm)

agent = initialize_agent(tools,
                         llm,
                         agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
                         verbose=True)
```

让我们在上述相同的例子上测试一下：

```py
agent.run("If my age is half of my dad's age and he is going to be 60 next year, what is my current age?")

# OUTPUT
# > Entering new AgentExecutor chain...
# I need to figure out my dad's current age and then divide it by two.
# Action: PAL-MATH
# Action Input: What is my dad's current age if he is going to be 60 next year?
# Observation: 59
# Thought: I now know my dad's current age, so I can divide it by two to get my age.
# Action: Divide 59 by 2
# Action Input: 59/2
# Observation: Divide 59 by 2 is not a valid tool, try another one.
# Thought: I can use PAL-MATH to divide 59 by 2.
# Action: PAL-MATH
# Action Input: Divide 59 by 2
# Observation: 29.5
# Thought: I now know the final answer.
# Final Answer: My current age is 29.5 years old.

# > Finished chain.
# 'My current age is 29.5 years old.'
```

*注意1：在每一步，你会注意到代理做了三件事之一——它要么有一个* `*observation*`*，要么有一个* `*thought*`*，要么采取一个* `*action*`*。这主要是由于 ReAct 框架和代理使用的相关提示：*

```py
print(agent.agent.llm_chain.prompt.template)
# OUTPUT
# Answer the following questions as best you can. You have access to the following tools:
# PAL-MATH: A language model that is really good at solving complex word math problems. Input should be a fully worded hard word math problem.

# Use the following format:

# Question: the input question you must answer
# Thought: you should always think about what to do
# Action: the action to take, should be one of [PAL-MATH]
# Action Input: the input to the action
# Observation: the result of the action
# ... (this Thought/Action/Action Input/Observation can repeat N times)
# Thought: I now know the final answer
# Final Answer: the final answer to the original input question
# Begin!
# Question: {input}
# Thought:{agent_scratchpad}
```

*注意2：你可能会想，为什么要让代理做 LLM 可以做的事情。一些应用不仅需要一个预定的 LLM/其他工具调用链，可能还需要一个取决于用户输入的未知链 [*[*来源*](https://python.langchain.com/en/latest/modules/agents.html#agents)*]。在这些类型的链中，有一个“代理”，可以访问一套工具。*

例如，* [*这是*](https://python.langchain.com/en/latest/modules/agents/agent_executors/examples/agent_vectorstore.html#create-the-agent) *一个代理的示例，它可以根据问题是指文档 A 还是文档 B，获取正确的文档（从向量存储中）。*

为了有趣，我尝试使输入问题更复杂（用 Demi Moore 的年龄作为 Dad 实际年龄的占位符）。

```py
agent.run("My age is half of my dad's age. Next year he is going to be same age as Demi Moore. What is my current age?")
```

不幸的是，答案有些偏差，因为代理没有使用最新的 Demi Moore 年龄（由于 OpenAI 模型的训练数据截至到 2020 年）。这可以通过包含另一个工具轻松修复——

`tools = load_tools([“pal-math”, **"serpapi"**], llm=llm)`。`serpapi` 对于回答当前事件的问题非常有用。

*注意: 添加尽可能多的相关工具对用户查询是很重要的。使用单一工具的问题在于，即使它不适用于特定的观察/行动步骤，代理也会继续尝试使用相同的工具。*

这是另一个你可以使用的工具示例——`podcast-api`。你需要[获取你自己的 API 密钥](https://www.listennotes.com/api/pricing/)并将其插入下面的代码中。

```py
 tools = load_tools(["podcast-api"], llm=llm, listen_api_key="...")
agent = initialize_agent(tools,
                         llm,
                         agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
                         verbose=True)

agent.run("Show me episodes for money saving tips.")

# OUTPUT
# > Entering new AgentExecutor chain...
# I should search for podcasts or episodes related to money saving
# Action: Podcast API
# Action Input: Money saving tips
# Observation:  The API call returned 3 podcasts related to money saving tips: The Money Nerds, The Rachel Cruze Show, and The Martin Lewis Podcast. These podcasts offer valuable money saving tips and advice to help people take control of their finances and create a life they love.
# Thought: I now have some options to choose from 
# Final Answer: The Money Nerds, The Rachel Cruze Show, and The Martin Lewis Podcast are great podcast options for money saving tips.

# > Finished chain.

# 'The Money Nerds, The Rachel Cruze Show, and The Martin Lewis Podcast are great podcast options for money saving tips.'
```

*注意1: 有一个* [*已知错误*](https://github.com/hwchase17/langchain/pull/1833) *，在使用这个 API 时你可能会看到，`*openai.error.InvalidRequestError: This model’s maximum context length is 4097 tokens, however you requested XXX tokens (XX in your prompt; XX for the completion). Please reduce your prompt; or completion length.*` *当 API 返回的响应可能过大时会发生这种情况。为了解决这个问题，文档建议返回更少的搜索结果，例如，通过将问题更新为* `"Show me episodes for money saving tips, return only 1 result"`。

*注意2: 在使用这个工具时，我注意到了一些不一致的地方。响应第一次生成时并不总是完整的，例如，以下是两次连续运行的输入和响应：*

*输入: “提高法语水平的播客”*

*回应 1: “学习法语的最佳播客是评价分数最高的那个。”*

*回应 2: “学习法语的最佳播客是‘FrenchPod101’。”*

在底层，这个工具首先使用 LLMChain 来[构建 API URL](https://github.com/hwchase17/langchain/blob/master/langchain/chains/api/base.py#L115)，根据我们的输入指令（类似于 `[https://listen-api.listennotes.com/api/v2/search?q=french&type=podcast&page_size=3](https://listen-api.listennotes.com/api/v2/search?q=french&type=podcast&page_size=3%29)`）和[进行 API 调用](https://github.com/hwchase17/langchain/blob/master/langchain/chains/api/base.py#L116)。接收到响应后，它使用另一个 LLMChain 来[总结响应](https://github.com/hwchase17/langchain/blob/master/langchain/chains/api/base.py#L117)，以获得对我们原始问题的回答。你可以在[这里](https://github.com/hwchase17/langchain/blob/master/langchain/chains/api/prompt.py)查看两个 LLMchains 的提示，它们详细描述了这个过程。

我倾向于猜测上述不一致的结果是由于总结步骤造成的，因为我已经通过Postman单独调试并测试了由LLMChain#1创建的API URL，并且得到了正确的响应。为了进一步确认我的疑虑，我还对总结链进行了压力测试，作为一个独立链使用了一个空的API URL，希望它能抛出一个错误，但得到了*“发现了‘投资’播客，总共有3个结果。”* 🤷‍♀ 我很好奇其他人是否在使用这个工具时比我更幸运！

## 用例 2：结合链创建一个适合年龄的礼物生成器

让我们充分利用代理和顺序链的知识，创建我们自己的顺序链。我们将结合：

+   链 #1 — 我们刚创建的`agent`，能够解决数学中的[年龄问题](https://www.cliffsnotes.com/study-guides/algebra/algebra-i/word-problems/age-problems)。

+   链 #2 — 一个LLM，它接受一个人的年龄并建议一个适合他们的礼物。

```py
# Chain1 - solve math problem, get the age
chain_one = agent

# Chain2 - suggest age-appropriate gift
template = """You are a gift recommender. Given a person's age,\n
 it is your job to suggest an appropriate gift for them.

Person Age:
{age}
Suggest gift:"""
prompt_template = PromptTemplate(input_variables=["age"], template=template)
chain_two = LLMChain(llm=llm, prompt=prompt_template) 
```

现在我们已经准备好了两个链，我们可以使用`SimpleSequentialChain`将它们结合起来。

```py
from langchain.chains import SimpleSequentialChain

overall_chain = SimpleSequentialChain(
                  chains=[chain_one, chain_two],
                  verbose=True)
```

需要注意几点：

+   我们不需要为`SimpleSequentialChain`明确传递`input_variables`和`output_variables`，因为其基本假设是链1的输出作为链2的输入。

最后，我们可以用之前的数学问题来运行它：

```py
question = "If my age is half of my dad's age and he is going to be 60 next year, what is my current age?"
overall_chain.run(question)

# OUTPUT
# > Entering new SimpleSequentialChain chain...

# > Entering new AgentExecutor chain...
# I need to figure out my dad's current age and then divide it by two.
# Action: PAL-MATH
# Action Input: What is my dad's current age if he is going to be 60 next year?
# Observation: 59
# Thought: I now know my dad's current age, so I can divide it by two to get my age.
# Action: Divide 59 by 2
# Action Input: 59/2
# Observation: Divide 59 by 2 is not a valid tool, try another one.
# Thought: I need to use PAL-MATH to divide 59 by 2.
# Action: PAL-MATH
# Action Input: Divide 59 by 2
# Observation: 29.5
# Thought: I now know the final answer.
# Final Answer: My current age is 29.5 years old.

# > Finished chain.
# My current age is 29.5 years old.

# Given your age, a great gift would be something that you can use and enjoy now like a nice bottle of wine, a luxury watch, a cookbook, or a gift card to a favorite store or restaurant. Or, you could get something that will last for years like a nice piece of jewelry or a quality leather wallet.

# > Finished chain.
```

```py
# '\nGiven your age, a great gift would be something that you can use and enjoy now like a nice bottle of wine, a luxury watch, a cookbook, or a gift card to a favorite store or restaurant. Or, you could get something that will last for years like a nice piece of jewelry or a quality leather wallet
```

有时你可能需要将一些额外的上下文传递给第二个链，而不仅仅是从第一个链接收的内容。例如，我想根据第一个链返回的年龄为礼物设定预算。我们可以使用`SimpleMemory`来实现。

首先，让我们更新`chain_two`的提示，并在`input_variables`中传递一个名为`budget`的第二个变量。

```py
template = """You are a gift recommender. Given a person's age,\n
 it is your job to suggest an appropriate gift for them. If age is under 10,\n
 the gift should cost no more than {budget} otherwise it should cost atleast 10 times {budget}.

Person Age:
{output}
Suggest gift:"""
prompt_template = PromptTemplate(input_variables=["output", "budget"], template=template)
chain_two = LLMChain(llm=llm, prompt=prompt_template)
```

如果你比较我们为`SimpleSequentialChain`准备的`template`与上述的模板，你会注意到我还将第一个输入的变量名从`age`更新为`output`。这是一个关键步骤，如果失败，将在[链验证](https://github.com/hwchase17/langchain/blob/master/langchain/chains/sequential.py#L41)时引发错误 — `*缺少必需的输入键：{age}，只有 {input, output, budget}*`。

这是因为链中的第一个实体（即`agent`）的输出将作为第二个实体（即`chain_two`）的输入，因此变量名必须匹配**。** 检查`agent`的输出键时，我们发现输出变量叫做`output`，因此进行了更新。

```py
print(agent.agent.llm_chain.output_keys)

# OUTPUT
["output"]
```

接下来，让我们更新我们正在制作的链的类型。我们不能再使用`SimpleSequentialChain`，因为它仅适用于单输入单输出的情况。由于`chain_two`现在需要两个`input_variables`，我们需要使用`SequentialChain`，它专门处理多个输入和输出。

```py
overall_chain = SequentialChain(
                input_variables=["input"],
                memory=SimpleMemory(memories={"budget": "100 GBP"}),
                chains=[agent, chain_two],
                verbose=True)
```

需要注意几点：

+   与`SimpleSequentialChain`不同，对于`SequentialChain`，传递`input_variables`参数是强制性的。它是一个包含链中第一个实体（即我们案例中的`agent`）期望的输入变量名称的列表。

    现在，你们中的一些人可能想知道如何知道`agent`将要使用的输入提示中使用的确切名称。我们确实没有为这个`agent`（如我们为`chain_two`所做的那样）编写过提示！事实上，通过检查`llm_chain`的提示模板，找出它其实非常简单。

```py
print(agent.agent.llm_chain.prompt.template)

# OUTPUT
#Answer the following questions as best you can. You have access to the following tools:

#PAL-MATH: A language model that is really good at solving complex word math problems. Input should be a fully worded hard word math problem.

#Use the following format:

#Question: the input question you must answer
#Thought: you should always think about what to do
#Action: the action to take, should be one of [PAL-MATH]
#Action Input: the input to the action
#Observation: the result of the action
#... (this Thought/Action/Action Input/Observation can repeat N times)
#Thought: I now know the final answer
#Final Answer: the final answer to the original input question

#Begin!

#Question: {input}
#Thought:{agent_scratchpad}
```

正如您可以在提示的最后看到的那样，最终用户提出的问题存储在一个名为`input`的输入变量中。如果因某种原因您必须在提示中操纵这个名称，请确保在创建`SequentialChain`时同时更新`input_variables`。

最后，您可以在不查看整个提示的情况下找到相同的信息：

```py
print(agent.agent.llm_chain.prompt.input_variables)

# OUTPUT
# ['input', 'agent_scratchpad']
```

+   `[SimpleMemory](https://github.com/hwchase17/langchain/blob/master/langchain/memory/simple.py#L6)` 是一种存储上下文或其他信息片段的简便方法，这些信息在提示之间不应更改。它在初始化时需要一个参数 — `memories`。您可以以`dict`形式传递元素给它。例如，`SimpleMemory(memories={“budget”: “100 GBP”})`。

最后，让我们用与之前相同的提示运行新链。您会注意到，最终输出包括一些奢侈礼品推荐，例如周末度假，与我们更新的提示中的更高预算相匹配。

```py
overall_chain.run("If my age is half of my dad's age and he is going to be 60 next year, what is my current age?")

# OUTPUT
#> Entering new SequentialChain chain...

#> Entering new AgentExecutor chain...
# I need to figure out my dad's current age and then divide it by two.
#Action: PAL-MATH
#Action Input: What is my dad's current age if he is going to be 60 next year?
#Observation: 59
#Thought: I now know my dad's current age, so I can divide it by two to get my age.
#Action: Divide 59 by 2
#Action Input: 59/2
#Observation: Divide 59 by 2 is not a valid tool, try another one.
#Thought: I can use PAL-MATH to divide 59 by 2.
#Action: PAL-MATH
#Action Input: Divide 59 by 2
#Observation: 29.5
#Thought: I now know the final answer.
#Final Answer: My current age is 29.5 years old.

#> Finished chain.

# For someone of your age, a good gift would be something that is both practical and meaningful. Consider something like a nice watch, a piece of jewelry, a nice leather bag, or a gift card to a favorite store or restaurant.\nIf you have a larger budget, you could consider something like a weekend getaway, a spa package, or a special experience.'}

#> Finished chain.
```

```py
For someone of your age, a good gift would be something that is both practical and meaningful. Consider something like a nice watch, a piece of jewelry, a nice leather bag, or a gift card to a favorite store or restaurant.\nIf you have a larger budget, you could consider something like a weekend getaway, a spa package, or a special experience.'}
```

# 结论

希望通过本文分享的学习内容能让您更轻松地深入了解这个库。本文只是皮毛，还有很多内容可以探讨。例如，如何在自己的数据集上构建问答聊天机器人，以及如何优化这些聊天机器人的记忆，以便您可以选择性地/总结性地发送对话，而不是将所有以前的聊天历史作为提示的一部分发送出去。

如往常一样，如果有更简单的方法来执行/解释本文中提到的一些内容，请告诉我。总的来说，避免未经请求的破坏性/垃圾/敌对的评论！

直到下次见 ✨

*我喜欢撰写逐步初学者指南、如何教程、解码ML/AI术语等。如果您希望全面访问我的所有文章（以及Medium上的其他文章），可以使用* [***我的链接***](https://varshitasher.medium.com/membership)*在这里注册*。

[](/step-by-step-guide-to-explaining-your-ml-project-during-a-data-science-interview-81dfaaa408bf?source=post_page-----16cd385fca81--------------------------------) [## 逐步指南：在数据科学面试中解释您的ML项目。

### 并附带一个示例脚本，让您可以悄悄展示您的技术技能！

[时间序列建模使用 Scikit、Pandas 和 Numpy](https://towardsdatascience.com/step-by-step-guide-to-explaining-your-ml-project-during-a-data-science-interview-81dfaaa408bf?source=post_page-----16cd385fca81--------------------------------) [](/time-series-modeling-using-scikit-pandas-and-numpy-682e3b8db8d1?source=post_page-----16cd385fca81--------------------------------) [## 时间序列建模使用 Scikit、Pandas 和 Numpy

### 直观地使用季节性来提高模型准确性。

[数据科学家实用的 GitHub Actions 介绍](https://towardsdatascience.com/time-series-modeling-using-scikit-pandas-and-numpy-682e3b8db8d1?source=post_page-----16cd385fca81--------------------------------) [](/hands-on-introduction-to-github-actions-for-data-scientists-f422631c9ea7?source=post_page-----16cd385fca81--------------------------------) [## 使用 GitHub Actions 进行动手实践的介绍

### 学习如何使用 Weights & Biases 自动化实验跟踪、单元测试、工件创建以及更多内容…

[数据科学家实用的 GitHub Actions 介绍](https://towardsdatascience.com/hands-on-introduction-to-github-actions-for-data-scientists-f422631c9ea7?source=post_page-----16cd385fca81--------------------------------) [](/deploying-an-end-to-end-deep-learning-project-with-few-clicks-part-2-89009cff6f16?source=post_page-----16cd385fca81--------------------------------) [## 使用少量点击部署端到端深度学习项目：第2部分

### 将模型从 Jupyter notebook 转移到 Flask 应用程序，使用 Postman 测试 API 端点，并进行 Heroku 部署

[将端到端深度学习项目部署到 Heroku](https://towardsdatascience.com/deploying-an-end-to-end-deep-learning-project-with-few-clicks-part-2-89009cff6f16?source=post_page-----16cd385fca81--------------------------------)
