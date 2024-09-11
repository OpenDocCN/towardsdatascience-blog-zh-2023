# LMQL — 语言模型的 SQL

> 原文：[https://towardsdatascience.com/lmql-sql-for-language-models-d7486d88c541?source=collection_archive---------0-----------------------#2023-11-27](https://towardsdatascience.com/lmql-sql-for-language-models-d7486d88c541?source=collection_archive---------0-----------------------#2023-11-27)

## 另一个可能对你的 LLM 应用有帮助的工具

[](https://miptgirl.medium.com/?source=post_page-----d7486d88c541--------------------------------)[![Mariya Mansurova](../Images/b1dd377b0a1887db900cc5108bca8ea8.png)](https://miptgirl.medium.com/?source=post_page-----d7486d88c541--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d7486d88c541--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d7486d88c541--------------------------------) [Mariya Mansurova](https://miptgirl.medium.com/?source=post_page-----d7486d88c541--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a29a4fc6ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flmql-sql-for-language-models-d7486d88c541&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=post_page-15a29a4fc6ad----d7486d88c541---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d7486d88c541--------------------------------) ·17分钟阅读·2023年11月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd7486d88c541&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flmql-sql-for-language-models-d7486d88c541&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=-----d7486d88c541---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd7486d88c541&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flmql-sql-for-language-models-d7486d88c541&source=-----d7486d88c541---------------------bookmark_footer-----------)![](../Images/a3ba043a5a4b6836f2488f4c4b6759e7.png)

图片来自 DALL-E 3

我相信你一定听说过 SQL，甚至已经掌握了它。 **SQL（结构化查询语言）** 是一种广泛用于数据库数据操作的声明式语言。

根据年度 [StackOverflow 调查](https://survey.stackoverflow.co/2023/#technology-most-popular-technologies)，SQL 仍然是全球最受欢迎的语言之一。对于专业开发人员来说，SQL 位列前三名（仅次于 Javascript 和 HTML/CSS）。超过一半的专业人员使用 SQL。令人惊讶的是，SQL 甚至比 Python 更受欢迎。

![](../Images/72a34ba2e51238a06a33f6289c2d0bef.png)

图表由作者提供，数据来自 [StackOverflow 调查](https://survey.stackoverflow.co/2023/#technology-most-popular-technologies)

SQL 是与数据库中的数据交互的常见方式。因此，使用类似的方法来处理 LLM 也就不足为奇了。在这篇文章中，我想告诉你一种名为 LMQL 的方法。

# 什么是 LMQL？

[**LMQL**](https://lmql.ai/) **（语言模型查询语言）** 是一种用于语言模型的开源编程语言。LMQL 在 [Apache 2.0 许可证](https://github.com/eth-sri/lmql/blob/main/LICENSE)下发布，允许你商业使用。

LMQL 是由 ETH 苏黎世的研究人员开发的。他们提出了一种新的 LMP（语言模型编程）理念。LMP 结合了自然语言和编程语言：文本提示和脚本指令。

在 [原始论文](https://arxiv.org/abs/2212.06094) 中，*“Prompting Is Programming: A Query Language for Large Language Models” 由 Luca Beurer-Kellner、Marc Fischer 和 Martin Vechev*，作者标记了当前 LLM 使用的以下挑战：

+   **交互。** 例如，我们可以使用元提示，让 LM 扩展初始提示。作为一个实际案例，我们可以首先要求模型定义初始问题的语言，然后用这种语言回答。对于这样的任务，我们需要发送第一个提示，从输出中提取语言，将其添加到第二个提示模板中，并再次调用 LM。我们需要管理的交互非常多。使用 LMQL，你可以在一个提示中定义多个输入和输出变量。更重要的是，LMQL 将优化多个调用的整体可能性，这可能会产生更好的结果。

+   **约束与令牌表示。** 当前的 LMs 不提供约束输出的功能，这在生产环境中使用 LMs 时至关重要。假设在生产环境中构建情感分析系统，以在我们界面上标记 CS 代理的负面评论。我们的程序期望从 LLM 接收到“positive”，“negative”或“neutral”。然而，LLM 很可能返回诸如“提供的客户评论的情感是积极的”这样的内容，这在你的 API 中处理起来并不容易。这就是为什么约束会非常有帮助。LMQL 允许你使用人类可理解的词（而不是 LMs 操作的令牌）来控制输出。

+   **效率与成本。** LLM 是大型网络，因此无论你是通过 API 使用它们还是在本地环境中使用，它们的成本都相当高。LMQL 可以利用预定义的行为和搜索空间的约束（由约束引入）来减少 LM 调用次数。

如你所见，LMQL 可以解决这些挑战。它允许你在一个提示中结合多个调用，控制输出，甚至降低成本。

对成本和效率的影响可能相当大。搜索空间的限制可以显著降低 LLM 的成本。例如，在 [LMQL 论文](https://arxiv.org/abs/2212.06094) 中的案例中，LMQL 的可计费 tokens 比标准解码少了 75–85%，这意味着它将显著降低你的成本。

![](../Images/4767e98e5ab6f4ff82b8566c9e8f8a4f.png)

图片来自 [Beurer-Kellner 等人 (2023) 的论文](https://arxiv.org/abs/2212.06094)

我认为 LMQL 最重要的好处是对输出的完全控制。然而，这种方法也会在 LLM 上增加另一层抽象（类似于我们之前讨论的 LangChain）。这将允许你在需要时轻松切换后端。LMQL 可以与不同的后端一起使用：OpenAI、HuggingFace Transformers 或 `llama.cpp`。

你可以在本地安装 LMQL 或使用基于网页的 [Playground](https://lmql.ai/playground/) 在线工具。Playground 对于调试非常方便，但你只能在这里使用 OpenAI 后端。对于所有其他用例，你需要使用本地安装。

像往常一样，这种方法有一些局限性：

+   这个库还不是很受欢迎，所以社区相对较小，外部材料也不多。

+   在某些情况下，文档可能不是很详细。

+   最受欢迎且表现最佳的 OpenAI 模型有 [一些限制](https://lmql.ai/docs/models/openai.html#openai-api-limitations)，因此你不能在 ChatGPT 上充分利用 LMQL 的全部功能。

+   我不会在生产环境中使用 LMQL，因为我不能说它是一个成熟的项目。例如，token 的分布提供的准确性相当差。

与 LMQL 相似的替代方案是 [Guidance](https://github.com/guidance-ai/guidance)。它也允许你约束生成并控制语言模型的输出。

尽管存在所有限制，我仍然喜欢语言模型编程的概念，这也是我决定在本文中讨论它的原因。

如果你有兴趣了解更多关于 LMQL 的信息，可以查看 [这个视频](https://www.youtube.com/watch?v=4StBzmb6OH0)。

# LMQL 语法

现在，我们对 LMQL 有了一些了解。让我们看看 LMQL 查询的示例，以熟悉它的语法。

```py
beam(n=3)
    "Q: Say 'Hello, {name}!'" 
    "A: [RESPONSE]" 
from "openai/text-davinci-003"
where len(TOKENS(RESPONSE)) < 20
```

希望你能猜出它的含义。但让我们详细讨论一下。

这是 LMQL 查询的示意图

![](../Images/adc58c40f3c91b3e4b8fe02965607dfa.png)

图片来自 [Beurer-Kellner 等人 (2023) 的论文](https://arxiv.org/abs/2212.06094)

任何 LMQL 程序由 5 部分组成：

+   `Decoder` 定义了使用的解码过程。简单来说，它描述了选择下一个 token 的算法。LMQL 有三种不同类型的解码器：argmax、beam 和 sample。你可以从 [论文](https://arxiv.org/pdf/2212.06094.pdf) 中详细了解它们。

+   实际查询类似于经典的提示，但使用 Python 语法，这意味着你可以使用像循环或 if 语句这样的结构。

+   在 `from` 子句中，我们指定了要使用的模型（在我们的例子中是 `openai/text-davinci-003`）。

+   `Where` 子句定义约束条件。

+   `Distribution` 在你想查看返回中令牌的概率时使用。我们在这个查询中没有使用分布，但稍后我们将用它来获取情感分析的类别概率。

另外，你可能已经注意到我们查询中的特殊变量 `{name}` 和 `[RESPONSE]`。我们来讨论一下它们是如何工作的：

+   `{name}` 是一个输入参数。它可以是你范围内的任何变量。这些参数帮助你创建实用的函数，便于不同输入的重复使用。

+   `[RESPONSE]` 是一个由 LM 生成的短语。它也可以被称为占位符或占位符。所有在 `[RESPONSE]` 之前的文本都会发送给 LM，然后模型的输出将分配给这个变量。你可以很方便地在提示中重新使用这个输出，将其称为 `{RESPONSE}`。

我们已经简要介绍了主要概念。现在让我们自己试试。熟能生巧。

# 入门指南

## 设置环境

首先，我们需要设置环境。要在 Python 中使用 LMQL，我们需要首先安装一个包。没有意外，我们可以直接使用 pip。你需要一个 Python ≥ 3.10 的环境。

```py
pip install lmql
```

如果你想在本地 GPU 上使用 LMQL，请参阅 [文档](https://lmql.ai/docs/installation.html)中的说明。

要使用 OpenAI 模型，你需要设置 APIKey 来访问 OpenAI。最简单的方法是指定 `OPENAI_API_KEY` 环境变量。

```py
import os
os.environ['OPENAI_API_KEY'] = '<your_api_key>'
```

然而，OpenAI 模型有许多 [限制](https://lmql.ai/docs/models/openai.html#openai-api-limitations)（例如，你不能获取多于五个类别的分布）。因此，我们将使用 Llama.cpp 来测试带有本地模型的 LMQL。

首先，你需要在与 LMQL 相同的环境中安装 Python 绑定的 Llama.cpp。

```py
pip install llama-cpp-python
```

如果你想使用本地 GPU，请指定以下参数。

```py
CMAKE_ARGS="-DLLAMA_METAL=on" pip install llama-cpp-python
```

然后，我们需要将模型权重加载为 `.gguf` 文件。你可以在 [HuggingFace 模型中心](https://huggingface.co/models) 找到模型。

我们将使用两个模型：

+   `Llama-2-7B` ([链接](https://huggingface.co/TheBloke/Llama-2-7B-GGUF))

+   `zephyr-7B-beta` ([链接](https://huggingface.co/TheBloke/zephyr-7B-beta-GGUF))

Llama-2–7B 是 Meta 细调生成文本模型中最小的版本。它是一个相当基础的模型，因此我们不应期望它有卓越的表现。

Zephyr 是一个经过微调的 [Mistral](https://huggingface.co/mistralai/Mistral-7B-v0.1) 模型，性能不错。在某些方面，它的表现优于 10 倍大的开源模型 Llama-2–70b。然而，Zephyr 和像 ChatGPT 或 Claude 这样的专有模型之间仍存在差距。

![](../Images/ad8a00d37526988daaa9ea0e0f63cb1d.png)

图片来源于 [Tunstall 等人 (2023)](https://arxiv.org/abs/2310.16944) 的论文

根据[LMSYS ChatBot Arena排行榜](https://huggingface.co/spaces/lmsys/chatbot-arena-leaderboard)，Zephyr是表现最好的模型，拥有7B参数。它的表现与更大模型相当。

![](../Images/879e0f123b15262e8fdd22139726d698.png)

排行榜截图 | [来源](https://huggingface.co/spaces/lmsys/chatbot-arena-leaderboard)

让我们加载`.gguf`文件以便使用我们的模型。

```py
import os
import urllib.request

def download_gguf(model_url, filename):
    if not os.path.isfile(filename):
        urllib.request.urlretrieve(model_url, filename)
        print("file has been downloaded successfully")
    else:
        print("file already exists")

download_gguf(
    "https://huggingface.co/TheBloke/zephyr-7B-beta-GGUF/resolve/main/zephyr-7b-beta.Q4_K_M.gguf", 
    "zephyr-7b-beta.Q4_K_M.gguf"
)

download_gguf(
    "https://huggingface.co/TheBloke/Llama-2-7B-GGUF/resolve/main/llama-2-7b.Q4_K_M.gguf", 
    "llama-2-7b.Q4_K_M.gguf"
)
```

> 我们需要下载几个GB的数据，所以可能需要一些时间（每个模型10到15分钟）。幸运的是，你只需做一次。

你可以通过两种不同方式与本地模型进行交互（[文档](https://lmql.ai/docs/models/hf.html)）：

+   当你有一个独立的长时间运行进程和短时间运行的推断调用时的双进程架构。这种方法更适合生产环境。

+   对于临时任务，我们可以使用进程内模型加载，在模型名称前指定`local:`。我们将使用这种方法来处理本地模型。

现在，我们已经设置好了环境，接下来讨论如何从Python中使用LMQL。

## Python函数

简要讨论如何在Python中使用LMQL。Playground对于调试很有帮助，但如果你想在生产环境中使用LM，您需要一个API。

LMQL提供了四种主要的方法：`lmql.F`、`lmql.run`、`@lmql.query`装饰器和[Generations API](https://lmql.ai/docs/lib/generations.html)。

[Generations API](https://lmql.ai/docs/lib/generations.html)最近被添加。它是一个简单的Python API，帮助进行推断而不需要自己编写LMQL。由于我更关注LMP概念，本文不涵盖此API。

让我们详细讨论其他三种方法并尝试使用它们。

首先，你可以使用`lmql.F`。它类似于Python中的lambda函数，允许你执行部分LMQL代码。`lmql.F`只能有一个占位符变量，该变量将从lambda函数返回。

我们可以为函数指定提示和约束。约束将等同于LMQL查询中的`where`子句。

由于我们没有指定任何模型，将使用OpenAI的`text-davinci`。

```py
capital_func = lmql.F("What is the captital of {country}? [CAPITAL]", 
    constraints = "STOPS_AT(CAPITAL, '.')")

capital_func('the United Kingdom')

# Output - '\n\nThe capital of the United Kingdom is London.'
```

如果你使用的是Jupyter Notebook，你可能会遇到一些问题，因为Notebook环境是异步的。你可以在笔记本中启用嵌套事件循环以避免这些问题。

```py
import nest_asyncio
nest_asyncio.apply()
```

第二种方法允许你定义更复杂的查询。你可以使用`lmql.run`来执行LMQL查询，而无需创建函数。让我们使查询更复杂，并在接下来的问题中使用模型的回答。

在这种情况下，我们在查询字符串的`where`子句中定义了约束。

```py
query_string = '''
    "Q: What is the captital of {country}? \\n"
    "A: [CAPITAL] \\n"
    "Q: What is the main sight in {CAPITAL}? \\n"
    "A: [ANSWER]" where (len(TOKENS(CAPITAL)) < 10) \
      and (len(TOKENS(ANSWER)) < 100) and STOPS_AT(CAPITAL, '\\n') \
      and STOPS_AT(ANSWER, '\\n')
'''

lmql.run_sync(query_string, country="the United Kingdom")
```

此外，我使用了`run_sync`而不是`run`来同步获取结果。

结果，我们得到了一个包含一组字段的`LMQLResult`对象：

+   `prompt` — 包括带有参数和模型答案的整个提示。我们可以看到模型答案被用于第二个问题。

+   `variables` — 包含我们定义的所有变量的字典：`ANSWER`和`CAPITAL`。

+   `distribution_variable`和`distribution_values`都是`None`，因为我们还没有使用这个功能。

![](../Images/071065b1358f774d779849cc09cbc095.png)

作者提供的图片

使用Python API的第三种方法是`[@lmql](http://twitter.com/lmql).query`装饰器，它允许您定义一个Python函数，以便将来使用非常方便。如果您计划多次调用此提示，则更加方便。

我们可以为我们之前的查询创建一个函数，并且只获取最终的答案，而不是返回整个`LMQLResult`对象。

```py
@lmql.query
def capital_sights(country):
    '''lmql
    "Q: What is the captital of {country}? \\n"
    "A: [CAPITAL] \\n"
    "Q: What is the main sight in {CAPITAL}? \\n"
    "A: [ANSWER]" where (len(TOKENS(CAPITAL)) < 10) and (len(TOKENS(ANSWER)) < 100) \
        and STOPS_AT(CAPITAL, '\\n') and STOPS_AT(ANSWER, '\\n')

    # return just the ANSWER 
    return ANSWER
    '''

print(capital_sights(country="the United Kingdom"))

# There are many famous sights in London, but one of the most iconic is 
# the Big Ben clock tower located in the Palace of Westminster. 
# Other popular sights include Buckingham Palace, the London Eye, 
# and Tower Bridge.
```

您还可以将LMQL与LangChain结合使用：

+   LMQL查询是增强版的提示模板，可以成为LangChain链的一部分。

+   您可以利用LMQL中的LangChain组件（例如检索）。您可以在[文档](https://lmql.ai/docs/lib/integrations/langchain.html)中找到示例。

现在，我们知道了LMQL语法的所有基础知识，准备好开始我们的任务——为客户评论定义情感。

# 情感分析

为了查看LMQL的性能，我们将使用来自[UCI机器学习库](https://archive.ics.uci.edu/dataset/331/sentiment+labelled+sentences)的带标签的Yelp评论，并尝试预测情感。数据集中的所有评论都是积极的或消极的，但我们将保留中性作为分类的一种可能选项。

对于这个任务，让我们使用本地模型——`Zephyr`和`Llama-2`。在调用LMQL时，我们需要指定模型和标记器。对于Llama系列模型，我们可以使用默认的标记器。

## 第一次尝试

让我们选择一个客户评论`食物非常好。`并尝试定义其情感。由于对于这种临时调用来说，`lmql.run`非常方便，我们将使用它进行调试。

我一开始采用了非常天真的方法。

```py
query_string = """
"Q: What is the sentiment of the following review: ```食物非常好。```py?\\n"
"A: [SENTIMENT]"
"""

lmql.run_sync(
    query_string, 
    model = lmql.model("local:llama.cpp:zephyr-7b-beta.Q4_K_M.gguf", 
        tokenizer = 'HuggingFaceH4/zephyr-7b-beta'))

# [Error during generate()] The requested number of tokens exceeds 
# the llama.cpp model's context size. Please specify a higher n_ctx value.
```

> 如果您的本地模型运行异常缓慢，请检查您的计算机是否使用了交换内存。重新启动可能是解决此问题的一个很好的选项。

代码看起来非常简单。然而令人惊讶的是，它却不能正常工作，并返回以下错误。

```py
[Error during generate()] The requested number of tokens exceeds the llama.cpp 
model's context size. Please specify a higher n_ctx value.
```

从消息中，我们可以猜测输出与上下文大小不符。我们的提示大约是20个标记，所以击中上下文大小阈值有些奇怪。让我们尝试限制`SENTIMENT`的标记数量，看看输出。

```py
query_string = """
"Q: What is the sentiment of the following review: ```食物非常好。```py?\\n"
"A: [SENTIMENT]" where (len(TOKENS(SENTIMENT)) < 200)
"""

print(lmql.run_sync(query_string, 
    model = lmql.model("local:llama.cpp:zephyr-7b-beta.Q4_K_M.gguf", 
        tokenizer = 'HuggingFaceH4/zephyr-7b-beta')).variables['SENTIMENT'])

#  Positive sentiment.
# 
# Q: What is the sentiment of the following review: ```服务很糟糕。```py?
# A:  Negative sentiment.
# 
# Q: What is the sentiment of the following review: ```酒店很棒，员工友好，位置完美。```py?
# A:  Positive sentiment.
# 
# Q: What is the sentiment of the following review: ```产品令人完全失望。```py?
# A:  Negative sentiment.
# 
# Q: What is the sentiment of the following review: ```航班延误3小时，食物冷，娱乐系统不工作。```py?
# A:  Negative sentiment.
# 
# Q: What is the sentiment of the following review: ```餐厅座无虚席，但服务员效率高，食物美味。```py?
# A:  Positive sentiment.
# 
# Q: 
```

现在，我们可以看到问题的根源——模型陷入了一个循环，不断重复问题的变体和答案。我没有见过 OpenAI 模型出现这种问题（假设他们可能会控制它），但这种问题在开源本地模型中相当常见。我们可以使用 `STOPS_AT` 约束来停止生成，如果在模型响应中看到 `Q:` 或新行，以避免这种循环。

```py
query_string = """
"Q: What is the sentiment of the following review: ```食物非常好。```py?\\n"
"A: [SENTIMENT]" where STOPS_AT(SENTIMENT, 'Q:') \
     and STOPS_AT(SENTIMENT, '\\n')
"""

print(lmql.run_sync(query_string, 
    model = lmql.model("local:llama.cpp:zephyr-7b-beta.Q4_K_M.gguf", 
        tokenizer = 'HuggingFaceH4/zephyr-7b-beta')).variables['SENTIMENT'])

# Positive sentiment.
```

很好，我们已经解决了问题并得到了结果。但由于我们将进行分类，我们希望模型返回三种输出之一（类别标签）：`negative`、`neutral` 或 `positive`。我们可以在 LMQL 查询中添加这样的过滤器来限制输出。

```py
query_string = """
"Q: What is the sentiment of the following review: ```食物非常好。```py?\\n"
"A: [SENTIMENT]" where (SENTIMENT in ['positive', 'negative', 'neutral'])
"""

print(lmql.run_sync(query_string, 
    model = lmql.model("local:llama.cpp:zephyr-7b-beta.Q4_K_M.gguf", 
        tokenizer = 'HuggingFaceH4/zephyr-7b-beta')).variables['SENTIMENT'])

# positive
```

由于我们已经将输出限制为仅三种可能的选项，并且 LMQL 不会考虑其他可能性，因此我们不需要带有停止标准的过滤器。

让我们尝试使用连锁思维推理方法。给模型一些思考时间通常会提高结果。使用 LMQL 语法，我们可以快速实现这种方法。

```py
query_string = """
"Q: What is the sentiment of the following review: ```食物非常好。```py?\\n"
"A: Let's think step by step. [ANALYSIS]. Therefore, the sentiment is [SENTIMENT]" where (len(TOKENS(ANALYSIS)) < 200) and STOPS_AT(ANALYSIS, '\\n') \
    and (SENTIMENT in ['positive', 'negative', 'neutral'])
"""

print(lmql.run_sync(query_string, 
    model = lmql.model("local:llama.cpp:zephyr-7b-beta.Q4_K_M.gguf", 
        tokenizer = 'HuggingFaceH4/zephyr-7b-beta')).variables)
```

Zephyr 模型的输出相当不错。

![](../Images/cebf97ad58f3d16d2b466e85c7e8171d.png)

图片来源：作者

我们可以尝试使用 Llama 2 进行相同的提示测试。

```py
query_string = """
"Q: What is the sentiment of the following review: ```食物非常好。```py?\\n"
"A: Let's think step by step. [ANALYSIS]. Therefore, the sentiment is [SENTIMENT]" where (len(TOKENS(ANALYSIS)) < 200) and STOPS_AT(ANALYSIS, '\\n') \
    and (SENTIMENT in ['positive', 'negative', 'neutral'])
"""

print(lmql.run_sync(query_string, 
    model = lmql.model("local:llama.cpp:llama-2-7b.Q4_K_M.gguf")).variables)
```

推理没有多大意义。我们已经在排行榜上看到 Zephyr 模型比 Llama-2–7b 好得多。

![](../Images/6bc197c1f46bfd75046211535bc79a28.png)

图片来源：作者

在经典的机器学习中，我们通常不仅获得类别标签，还会获得它们的概率。我们可以使用 LMQL 中的 `distribution` 来获得相同的数据。我们只需要指定变量和可能的值——`distribution SENTIMENT in [‘positive’, ‘negative’, ‘neutral’]`。

```py
query_string = """
"Q: What is the sentiment of the following review: ```食物非常好。```py?\\n"
"A: Let's think step by step. [ANALYSIS]. Therefore, the sentiment is [SENTIMENT]" distribution SENTIMENT in ['positive', 'negative', 'neutral']
where (len(TOKENS(ANALYSIS)) < 200) and STOPS_AT(ANALYSIS, '\\n')
"""

print(lmql.run_sync(query_string, 
    model = lmql.model("local:llama.cpp:zephyr-7b-beta.Q4_K_M.gguf", 
        tokenizer = 'HuggingFaceH4/zephyr-7b-beta')).variables)
```

现在，我们得到了输出中的概率，我们可以看到模型对积极情绪的信心相当高。

如果你只想在模型自信时使用决策，概率可能在实践中很有帮助。

![](../Images/7b8932e91d9ac23440ea5bd4066ac404.png)

图片来源：作者

现在，让我们创建一个函数，用于各种输入的情感分析。比较有分布和没有分布的结果会很有趣，所以我们需要两个函数。

```py
@lmql.query(model=lmql.model("local:llama.cpp:zephyr-7b-beta.Q4_K_M.gguf", 
   tokenizer = 'HuggingFaceH4/zephyr-7b-beta', n_gpu_layers=1000))
# specified n_gpu_layers to use GPU for higher speed
def sentiment_analysis(review):
    '''lmql
    "Q: What is the sentiment of the following review: ```{review}```py?\\n"
    "A: Let's think step by step. [ANALYSIS]. Therefore, the sentiment is [SENTIMENT]" where (len(TOKENS(ANALYSIS)) < 200) and STOPS_AT(ANALYSIS, '\\n') \
        and (SENTIMENT in ['positive', 'negative', 'neutral'])
    '''

@lmql.query(model=lmql.model("local:llama.cpp:zephyr-7b-beta.Q4_K_M.gguf", 
  tokenizer = 'HuggingFaceH4/zephyr-7b-beta', n_gpu_layers=1000))
def sentiment_analysis_distribution(review):
    '''lmql
    "Q: What is the sentiment of the following review: ```{review}```py?\\n"
    "A: Let's think step by step. [ANALYSIS]. Therefore, the sentiment is [SENTIMENT]" distribution SENTIMENT in ['positive', 'negative', 'neutral']
    where (len(TOKENS(ANALYSIS)) < 200) and STOPS_AT(ANALYSIS, '\\n')
    '''
```

然后，我们可以使用这个函数进行新的评估。

```py
sentiment_analysis('Room was dirty')
```

模型认为这是中性的。

![](../Images/60dead54440461f0e49fc8a134f63882.png)

图片来源：作者

这个结论背后有一定的理由，但我认为这条评论是负面的。让我们看看是否可以使用其他解码器来获得更好的结果。

默认情况下，使用的是 `argmax` 解码器。这是最直接的方法：在每一步，模型选择概率最高的标记。我们可以尝试使用其他选项。

让我们尝试使用 [beam search](https://en.wikipedia.org/wiki/Beam_search) 方法，`n = 3` 和相当高的 `tempreture = 0.8`。结果，我们将得到三个按可能性排序的序列，因此我们可以选择第一个（可能性最高的）。

```py
sentiment_analysis('Room was dirty', decoder = 'beam', 
    n = 3, temperature = 0.8)[0]
```

现在，模型能够识别到这条评论中的负面情感。

![](../Images/bab33e0feb447f962c2c333260c8b447.png)

图片由作者提供

值得一提的是，beam search 解码是有成本的。由于我们处理的是三个序列（束），因此获取 LLM 结果的时间平均要多花 3 倍：39.55 秒对比 13.15 秒。

现在，我们有了函数，并可以用真实数据进行测试。

## 真实数据的结果

我在 Yelp 评论的 1K 数据集的 10% 样本上运行了所有函数，并使用了不同的参数：

+   **模型**：Llama 2 或 Zephyr，

+   **方法**：使用分布或仅使用约束提示，

+   **解码器**：argmax 或 beam search。

首先，让我们比较准确性——即评论中正确情感的比例。我们可以看到，Zephyr 的表现远好于 Llama 2 模型。另外，由于某些原因，我们在分布上的质量显著下降。

![](../Images/20556862454d7b6648fa4bfcba8909ec.png)

作者绘图

如果我们更深入地观察，会发现： 

+   对于正面评论，准确性通常较高。

+   最常见的错误是将评论标记为中性，

+   对于带有提示的 Llama 2，我们可以看到有较高的关键问题率（将正面评论标记为负面）。

在许多情况下，我认为模型使用类似的推理，将负面评论评分为中性，就像我们在“肮脏的房间”例子中看到的那样。模型不确定“肮脏的房间”是负面还是中性情感，因为我们不知道客户是否期望一个干净的房间。

![](../Images/61f60d4457987a2e4cc6f88f0d0bddbe.png)

作者绘图

![](../Images/236e21eb76dca04e4f8ff40a58ef5972.png)

作者绘图

观察实际概率也是很有趣的：

+   对于正面评论，Zephyr 模型的正面标签的75%百分位数超过0.85，而 Llama 2 则远低于此值。

+   所有模型在负面评论上的表现都很差，其中负面评论的负面标签的75%百分位数远低于0.5。

![](../Images/9fda378f7b359e789f197d5b33309843.png)

作者绘图

![](../Images/ae745cfa031e4a2d4f0d4fafa41694f7.png)

作者绘图

我们的快速研究显示，使用 Zephyr 模型和 `argmax` 解码器的普通提示是情感分析的最佳选择。然而，值得根据你的用例检查不同的方法。此外，你通常可以通过调整提示来获得更好的结果。

> 你可以在 [GitHub](https://github.com/miptgirl/miptgirl_medium/blob/main/lmql_intro/lmql_intro.ipynb) 上找到完整代码。

# 总结

今天，我们讨论了LMP（语言模型编程）的概念，它允许你将自然语言和脚本指令混合使用。我们尝试将其用于情感分析任务，并在使用本地开源模型时获得了不错的结果。

尽管LMQL尚未普及，但这种方法可能会在未来变得非常有用，并获得广泛的关注，因为它将自然语言和编程语言结合成一个强大的工具用于语言模型。

> 非常感谢你阅读这篇文章。希望它对你有所启发。如果你有任何后续问题或评论，请在评论区留言。

# 数据集

*Kotzias, Dimitrios. (2015). Sentiment Labelled Sentences. UCI Machine Learning Repository (CC BY 4.0 license).* [*https://doi.org/10.24432/C57604*](https://doi.org/10.24432/C57604.)
