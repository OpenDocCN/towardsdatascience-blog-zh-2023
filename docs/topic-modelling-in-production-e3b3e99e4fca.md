# 生产中的主题建模

> 原文：[https://towardsdatascience.com/topic-modelling-in-production-e3b3e99e4fca?source=collection_archive---------0-----------------------#2023-10-30](https://towardsdatascience.com/topic-modelling-in-production-e3b3e99e4fca?source=collection_archive---------0-----------------------#2023-10-30)

## 利用 LangChain 从临时的 Jupyter Notebook 迁移到生产模块化服务

[](https://miptgirl.medium.com/?source=post_page-----e3b3e99e4fca--------------------------------)[![Mariya Mansurova](../Images/b1dd377b0a1887db900cc5108bca8ea8.png)](https://miptgirl.medium.com/?source=post_page-----e3b3e99e4fca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e3b3e99e4fca--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e3b3e99e4fca--------------------------------) [Mariya Mansurova](https://miptgirl.medium.com/?source=post_page-----e3b3e99e4fca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a29a4fc6ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftopic-modelling-in-production-e3b3e99e4fca&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=post_page-15a29a4fc6ad----e3b3e99e4fca---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----e3b3e99e4fca--------------------------------) ·22分钟阅读·2023年10月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe3b3e99e4fca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftopic-modelling-in-production-e3b3e99e4fca&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=-----e3b3e99e4fca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe3b3e99e4fca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftopic-modelling-in-production-e3b3e99e4fca&source=-----e3b3e99e4fca---------------------bookmark_footer-----------)![](../Images/afdbbe895f01c14cdde29dadbd785120.png)

图片由 DALL-E 3 制作

在 [上一篇文章](/topic-modelling-using-chatgpt-api-8775b0891d16) 中，我们讨论了如何使用 ChatGPT 进行主题建模，并取得了出色的结果。任务是查看酒店连锁的客户评价，并定义评论中提到的主要主题。

在之前的迭代中，我们使用了标准的 [ChatGPT 完成 API](https://platform.openai.com/docs/guides/gpt/chat-completions-api) 并且自己发送了原始提示。这种方法在我们进行一些临时分析研究时效果很好。

然而，如果你的团队正在积极使用和监控客户评论，值得考虑一些自动化。良好的自动化不仅会帮助你构建一个自主的管道，还会更加方便（即使是不熟悉 LLM 和编码的团队成员也能访问这些数据）和更具成本效益（你只需将所有文本发送给 LLM，并只需支付一次费用）。

假设我们正在构建一个可持续的生产就绪服务。在这种情况下，值得利用现有框架来减少胶水代码的数量，并拥有一个更模块化的解决方案（例如，我们可以轻松地从一个 LLM 切换到另一个）。

在这篇文章中，我想告诉你一个最受欢迎的 LLM 应用框架之一——[LangChain](https://www.langchain.com)。此外，我们将详细了解如何评估你模型的性能，因为这是商业应用中的关键步骤。

# 生产过程的细节

## 修订初始方法

首先，让我们回顾一下之前使用 ChatGPT 进行的即席话题建模方法。

**步骤 1：获取代表性样本。**

我们想确定将用于标记的话题列表。最直接的方法是发送所有评论，并要求 LLM 定义我们评论中提到的 20–30 个话题列表。不幸的是，我们不能做到这一点，因为这不符合上下文大小。我们可以使用 map-reduce 方法，但这可能会很昂贵。因此，我们希望定义一个代表性样本。

为此，我们[构建了](/topic-modelling-using-chatgpt-api-8775b0891d16)一个[BERTopic](https://maartengr.github.io/BERTopic/getting_started/quickstart/quickstart.html) 话题模型，并获得了每个话题中最具代表性的评论。

**步骤 2：确定我们将用于标记的话题列表。**

下一步是将所有选定的文本传递给 ChatGPT，并要求它定义这些评论中提到的话题列表。然后，我们可以使用这些话题进行后续标记。

**步骤 3：批量进行话题标记。**

最后一步是最直接的——我们可以将符合上下文大小的客户评论分批发送，并要求 LLM 返回每个客户评论的话题。

最后，通过这三步，我们可以确定我们文本的相关话题列表并对其进行分类。

这在一次性研究中效果很好。然而，我们还缺少一些要素来实现一个出色的生产就绪解决方案。

## 从即席到生产

让我们讨论一下我们可以对最初的即席方法进行哪些改进。

+   在之前的方法中，我们有一个静态的话题列表。但在实际情况中，新的话题可能会随着时间出现，例如，如果你推出了新功能。因此，我们需要一个反馈循环来更新我们使用的话题列表。最简单的方法是捕捉没有分配话题的评论列表，并定期对其进行话题建模。

+   如果我们进行一次性研究，我们可以手动验证主题分配的结果。但对于在生产环境中运行的过程，我们需要考虑持续评估。

+   如果我们正在构建一个客户评论分析的管道，我们应该考虑更多潜在的用例，并存储我们可能需要的其他相关信息。例如，存储客户评论的翻译版本是有帮助的，这样我们的同事就不必一直使用Google翻译。此外，情感和其他特征（例如客户评论中提到的产品）可能对分析和过滤很有价值。

+   目前LLM行业进展迅速，一切都在不断变化。值得考虑一种模块化的方法，我们可以在不从头开始重写整个服务的情况下，快速迭代并尝试新的方法。

![](../Images/3a85e2f56aeca48465378a421f27fb2f.png)

作者提供的服务方案

我们有很多关于如何使用主题建模服务的想法。但让我们专注于主要部分：模块化方法而非API调用和评估。LangChain框架将帮助我们解决这两个问题，所以让我们深入了解一下它。

# LangChain基础知识

[LangChain](https://python.langchain.com/docs/get_started/introduction)是一个用于构建由语言模型驱动的应用程序的框架。以下是LangChain的主要[组件](https://docs.langchain.com/docs/category/components)：

+   **Schema**是最基本的类，如文档、聊天消息和文本。

+   **Models**。LangChain提供对LLMs、聊天模型和文本嵌入模型的访问，您可以轻松地在应用程序中使用这些模型，并根据需要在它们之间切换。不用说，它支持像ChatGPT、Anthropic和Llama这样的流行模型。

+   **Prompts**是一种帮助处理提示的功能，包括提示模板、输出解析器和少样本提示的示例选择器。

+   **Chains**是LangChain的核心（正如你从名字中可以猜到的）。Chains帮助您构建一系列将被执行的块。如果您正在构建复杂的应用程序，您会真正欣赏到这个功能。

+   **Indexes**：文档加载器、文本分割器、向量存储和检索器。此模块提供了帮助LLMs与您的文档交互的工具。如果您正在构建问答用例，这些功能会很有价值。我们今天的示例中不会使用这些功能。

+   LangChain提供了一整套管理和限制**内存**的方法。这些功能主要用于聊天机器人场景。

+   最新和最强大的功能之一是**代理**。如果您是ChatGPT的重度用户，您一定听说过插件。这与您可以为LLM配置一组自定义或预定义工具（如Google搜索或维基百科）的想法相同，然后代理可以在回答您的问题时使用它们。在这种设置下，LLM就像一个推理代理，决定为实现结果需要做什么以及何时获得最终答案以分享。这是一个令人兴奋的功能，因此绝对值得单独讨论。

因此，LangChain 可以帮助我们构建模块化应用程序，并能够在不同组件之间切换（例如，从ChatGPT到Anthropic，或者从CSV作为数据输入到Snowflake DB）。LangChain 拥有超过[190个集成](https://python.langchain.com/docs/integrations/providers)，因此可以帮助您节省大量时间。

此外，我们可以重用现成的链式结构来处理[某些用例](https://python.langchain.com/docs/use_cases)，而不是从头开始。

在手动调用ChatGPT API时，我们必须管理大量的Python胶水代码来使其正常工作。当您处理一个小型、简单的任务时，这并不是问题，但是当您需要构建更复杂和复杂的东西时，可能会变得难以管理。在这种情况下，LangChain 可能会帮助您消除这些胶水代码，并创建更易于维护的模块化代码。

然而，LangChain 也有其局限性：

+   它主要集中在OpenAI模型上，因此可能与本地开源模型不太兼容。

+   便利的反面是，不容易理解底层发生了什么，以及何时以及如何执行您支付的ChatGPT API。您可以使用调试模式，但需要指定并查看完整日志，以获得更清晰的视角。

+   尽管有相当好的文档，我偶尔还是会在寻找答案时遇到困难。除了官方文档外，互联网上几乎没有其他教程和资源，通常只能看到Google的官方页面。

+   Langchain 库正在取得很大进展，团队不断发布新功能。因此，这个库还不够成熟，您可能需要从您正在使用的功能切换。例如，`SequentialChain`类现在被认为是遗留的，并且可能会在未来被弃用，因为他们已经引入了[LCEL](https://python.langchain.com/docs/expression_language/) — 我们稍后将更详细地讨论它。

我们已经对LangChain功能有了总体的了解，但实践出真知。让我们开始使用LangChain。

# 增强主题分配

让我们重构主题分配，因为它将是我们日常流程中最常见的操作，并且将帮助我们了解如何在实践中使用LangChain。

首先，我们需要安装这个包。

```py
!pip install --upgrade langchain
```

## 加载文档

要处理客户的评论，我们首先需要加载它们。为此，我们可以使用 [文档加载器](https://python.langchain.com/docs/modules/data_connection/document_loaders/)。在我们的案例中，客户评论被存储在一个目录中的一组 .txt 文件中，但你可以轻松地从第三方工具加载文档。例如，还有一个与 Snowflake 的 [集成](https://python.langchain.com/docs/integrations/document_loaders/snowflake)。

我们将使用 `DirectoryLoader` 加载目录中的所有文件，因为我们有来自不同酒店的单独文件。对于每个文件，我们将指定 `TextLoader` 作为加载器（默认情况下，使用用于非结构化文档的加载器）。我们的文件使用 `ISO-8859–1` 编码，因此默认调用会返回错误。然而，LangChain 可以自动检测使用的编码。使用这样的设置，它工作得很好。

```py
from langchain.document_loaders import TextLoader, DirectoryLoader

text_loader_kwargs={'autodetect_encoding': True}
loader = DirectoryLoader('./hotels/london', show_progress=True, 
    loader_cls=TextLoader, loader_kwargs=text_loader_kwargs)

docs = loader.load()
len(docs)
82
```

## 文档分割

现在，我们希望分割我们的文档。我们知道每个文件由一组以 `\n` 分隔的客户评论组成。由于我们的案例非常简单，我们将使用最基本的 `CharacterTextSplitter`，按字符分割文档。当处理实际文档（而不是独立的短评论）时，最好使用 [按字符递归分割](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/recursive_text_splitter)，因为它允许你更智能地将文档分割成块。

然而，LangChain 更适合模糊文本分割。因此，我不得不稍微修改它，使其按我想要的方式工作。

它是如何工作的：

+   你指定 `chunk_size` 和 `chunk_overlap`，它会尽量减少分割次数，以使每个块小于 `chunk_size`。如果无法创建足够小的块，它会在 Jupyter Notebook 输出中打印一条消息。

+   如果你指定的 `chunk_size` 太大，一些评论将不会被分开。

+   如果你指定的 `chunk_size` 太小，你的输出中会为每个评论打印语句，导致 Notebook 重新加载。不幸的是，我找不到任何参数来关闭这个功能。

为了解决这个问题，我将 `length_function` 指定为等于 `chunk_size` 的常量。然后我得到了标准的按字符分割。LangChain 提供了足够的灵活性来实现你的需求，但只有以一种有点 hacky 的方式。

```py
from langchain.text_splitter import CharacterTextSplitter

text_splitter = CharacterTextSplitter(
    separator = "\n",
    chunk_size = 1,
    chunk_overlap  = 0,
    length_function = lambda x: 1, # usually len is used 
    is_separator_regex = False
)
split_docs = text_splitter.split_documents(docs)
len(split_docs) 
12890
```

另外，让我们将文档 ID 添加到元数据中——我们稍后会用到它。

```py
for i in range(len(split_docs)):
    split_docs[i].metadata['id'] = i
```

使用文档的优点在于我们现在拥有了自动数据源，并且可以根据它来过滤数据。例如，我们可以仅过滤与 Travelodge Hotel 相关的评论。

```py
list(filter(
    lambda x: 'travelodge' in x.metadata['source'],
    split_docs
))
```

接下来，我们需要一个模型。正如我们之前在 LangChain 中讨论的那样，有 LLMs 和聊天模型。主要区别在于 LLMs 接受文本并返回文本，而聊天模型更适合对话式用例，可以将一组消息作为输入。在我们的案例中，我们将使用 OpenAI 的 ChatModel，因为我们也希望传递系统消息。

```py
from langchain.chat_models import ChatOpenAI

chat = ChatOpenAI(temperature=0.0, model="gpt-3.5-turbo", 
  openai_api_key = "your_key")
```

## 提示

让我们进入最重要的部分——我们的提示。在LangChain中，有一个提示模板的概念。它们帮助重复使用由变量参数化的提示。这很有帮助，因为在实际应用中，提示可能非常详细和复杂。因此，提示模板可以成为一种有用的高级抽象，帮助你有效地管理代码。

由于我们将使用聊天模型，我们需要[ChatPromptTemplate.](https://python.langchain.com/docs/modules/model_io/prompts/prompt_templates/#chatprompttemplate)

但在深入探讨提示之前，让我们简要讨论一个有用的功能——输出解析器。令人惊讶的是，它们可以帮助我们创建一个有效的提示。我们可以定义所需的输出，生成一个输出解析器，然后使用该解析器为提示创建指令。

让我们定义一下我们希望在输出中看到的内容。首先，我们希望能够将客户评论的列表传递给提示，以批量处理它们，因此在结果中，我们希望得到一个具有以下参数的列表：

+   用于识别文档的ID，

+   从预定义列表中获取主题列表（我们将使用我们上一个迭代中的列表），

+   情感（负面、中性或正面）。

让我们指定我们的输出解析器。由于我们需要一个相当复杂的JSON结构，我们将使用[Pydantic Output Parser](https://python.langchain.com/docs/modules/model_io/output_parsers/pydantic)而不是最常用的[Structured Output Parser](https://python.langchain.com/docs/modules/model_io/output_parsers/structured)。

为此，我们需要创建一个继承自`BaseModel`的类，并指定我们所需的所有字段及其名称和描述（以便LLM可以理解我们对响应的期望）。

```py
from langchain.output_parsers import PydanticOutputParser
from langchain.pydantic_v1 import BaseModel, Field
from typing import List

class CustomerCommentData(BaseModel):
    doc_id: int = Field(description="doc_id from the input parameters")
    topics: List[str] = Field(description="List of the relevant topics \
        for the customer review. Please, include only topics from \
        the provided list.")
    sentiment: str = Field(description="sentiment of the comment (positive, neutral or negative")

output_parser = PydanticOutputParser(pydantic_object=CustomerCommentData)
```

现在，我们可以使用这个解析器为我们的提示生成格式化指令。这是一个绝佳的例子，展示了如何使用提示最佳实践，从而减少提示工程的时间。

```py
format_instructions = output_parser.get_format_instructions()
print(format_instructions)
```

![](../Images/223f83dd83f0e516f1a09c0acbc47478.png)

然后，进入我们的提示部分。我们取了一批评论并将其格式化为预期格式。接着，我们创建了一个包含多个变量的提示消息：`topics_descr_list`、`format_instructions`和`input_data`。之后，我们创建了由一个常量系统消息和一个提示消息组成的聊天提示消息。最后一步是用实际值格式化聊天提示消息。

```py
from langchain.prompts import ChatPromptTemplate

docs_batch_data = []
for rec in docs_batch:
    docs_batch_data.append(
        {
            'id': rec.metadata['id'],
            'review': rec.page_content
        }
    )

topic_assignment_msg = '''
Below is a list of customer reviews in JSON format with the following keys:
1\. doc_id - identifier for the review
2\. review - text of customer review
Please, analyse provided reviews and identify the main topics and sentiment. Include only topics from the provided below list.

List of topics with descriptions (delimited with ":"):
{topics_descr_list}

Output format:
{format_instructions}

Customer reviews:
```

{input_data}

```py
'''

topic_assignment_template = ChatPromptTemplate.from_messages([
    ("system", "You're a helpful assistant. Your task is to analyse hotel reviews."),
    ("human", topic_assignment_msg)
])

topics_list = '\n'.join(
    map(lambda x: '%s: %s' % (x['topic_name'], x['topic_description']), 
      topics))

messages = topic_assignment_template.format_messages(
    topics_descr_list = topics_list,
    format_instructions = format_instructions,
    input_data = json.dumps(docs_batch_data)
)
```

现在，我们可以将这些格式化的消息传递给LLM，并查看响应。

```py
response = chat(messages)
type(response.content)
str

print(response.content)
```

![](../Images/19c75f5b1930416d6db2123198a6c974.png)

我们得到了一个字符串对象的响应，但我们可以利用我们的解析器，将其结果转换为一组`CustomerCommentData`类对象。

```py
response_dict = list(map(lambda x: output_parser.parse(x), 
  response.content.split('\n')))
response_dict
```

![](../Images/39bbe90b0978935dfefd2f9ffe552981.png)

因此，我们利用了LangChain及其一些功能，已经构建了一个更智能的解决方案，可以将主题分配给批量评论（这将节省我们的成本），并开始定义不仅仅是主题，还有情感。

# 添加更多逻辑

到目前为止，我们只构建了没有任何关系和顺序的单一LLM调用。然而，在现实生活中，我们通常希望将任务拆分成多个步骤。为此，我们可以使用链。链是LangChain的基本构建块。

## LLMChain

最基本的链类型是LLMChain。它是LLM和提示的组合。

所以我们可以将我们的逻辑重写成一个链。这段代码将给我们与之前完全相同的结果，但拥有一个定义所有内容的方法是非常方便的。

```py
from langchain.chains import LLMChain

topic_assignment_chain = LLMChain(llm=chat, prompt=topic_assignment_template)
response = topic_assignment_chain.run(
    topics_descr_list = topics_list,
    format_instructions = format_instructions,
    input_data = json.dumps(docs_batch_data)
) 
```

## 顺序链

LLM链非常基础。链的力量在于构建更复杂的逻辑。让我们尝试创建一些更高级的东西。

[顺序链](https://python.langchain.com/docs/modules/chains/foundational/sequential_chains)的理念是将一个链的输出用作另一个链的输入。

在定义链时，我们将使用[LCEL](https://python.langchain.com/docs/expression_language)（LangChain表达语言）。这种新语言刚刚推出几个月，现在所有旧的`SimpleSequentialChain`或`SequentialChain`方法都被视为遗留方法。因此，花时间了解LCEL的概念是值得的。

让我们用LCEL重写之前的链。

```py
chain = topic_assignment_template | chat
response = chain.invoke(
    {
        'topics_descr_list': topics_list,
        'format_instructions': format_instructions,
        'input_data': json.dumps(docs_batch_data)
    }
)
```

如果你想亲身体验，建议你观看[这个视频](https://www.youtube.com/watch?v=9M8x485j_lU)，了解LangChain团队讲解的LCEL。

## 使用顺序链

在某些情况下，拥有[多个顺序调用](https://python.langchain.com/docs/expression_language/cookbook/multiple_chains)可能会很有帮助，以便将一个链的输出用作其他链的输入。

在我们的案例中，我们可以先将评论翻译成英语，然后进行主题建模和情感分析。

![](../Images/685f59a665a81ef918b94ae53fa56394.png)

```py
from langchain.schema import StrOutputParser
from operator import itemgetter

# translation

translate_msg = '''
Below is a list of customer reviews in JSON format with the following keys:
1\. doc_id - identifier for the review
2\. review - text of customer review

Please, translate review into English and return the same JSON back. Please, return in the output ONLY valid JSON without any other information.

Customer reviews:
```

{input_data}

```py
'''

translate_template = ChatPromptTemplate.from_messages([
    ("system", "You're an API, so you return only valid JSON without any comments."),
    ("human", translate_msg)
])

# topic assignment & sentiment analysis

topic_assignment_msg = '''
Below is a list of customer reviews in JSON format with the following keys:
1\. doc_id - identifier for the review
2\. review - text of customer review
Please, analyse provided reviews and identify the main topics and sentiment. Include only topics from the provided below list.

List of topics with descriptions (delimited with ":"):
{topics_descr_list}

Output format:
{format_instructions}

Customer reviews:
```

{translated_data}

```py
'''

topic_assignment_template = ChatPromptTemplate.from_messages([
    ("system", "You're a helpful assistant. Your task is to analyse hotel reviews."),
    ("human", topic_assignment_msg)
])

# defining chains

translate_chain = translate_template | chat | StrOutputParser()
topic_assignment_chain = {'topics_descr_list': itemgetter('topics_descr_list'), 
                          'translated_data': translate_chain, 
                          'format_instructions': itemgetter('format_instructions')} 
                        | topic_assignment_template | chat 

# execution

response = topic_assignment_chain.invoke(
    {
        'topics_descr_list': topics_list,
        'format_instructions': format_instructions,
        'input_data': json.dumps(docs_batch_data)
    }
)
```

我们类似地为翻译和主题分配定义了提示模板。然后，我们确定了翻译链。这里唯一的新事物是`StrOutputParser()`的使用，它将响应对象转换为字符串（没什么高深的技术）。

然后，我们定义了完整的链，指定了输入参数、提示模板和LLM。对于输入参数，我们从`translate_chain`的输出中获取`translated_data`，而其他参数则使用`itemgetter`函数从调用输入中提取。

然而，在我们的情况下，使用组合链可能不是那么方便，因为我们还希望保存第一个链的输出以获得翻译值。

使用链时，一切变得有些复杂，我们可能需要一些调试功能。调试有两个选项。

第一个选项是你可以在本地启用调试。

```py
import langchain
langchain.debug = True
```

另一个选择是使用LangChain平台 — [LangSmith](https://blog.langchain.dev/announcing-langsmith/)。不过，它仍处于测试阶段，所以你可能需要等待才能获得访问权限。

## 路由

链中最复杂的案例之一是[路由](https://python.langchain.com/docs/expression_language/how_to/routing)，当你为不同的用例使用不同的提示时。例如，我们可以根据情感保存不同的客户评论参数：

+   如果评论是负面的，我们将存储客户提到的问题列表。

+   否则，我们将从评论中获得好点的列表。

要使用路由链，我们需要逐个传递评论，而不是像以前那样批量处理。

所以我们高层的链将会是这样的。

![](../Images/99a869839ba12e1e401eca16ab094b04.png)

首先，我们需要定义主要的链来确定情感。这条链由提示、LLM和已经熟悉的`StrOutputParser()`组成。

```py
sentiment_msg = '''
Given the customer comment below please classify whether it's negative. If it's negative, return "negative", otherwise return "positive".
Do not respond with more than one word.

Customer comment:
```

{input_data}

```py
'''

sentiment_template = ChatPromptTemplate.from_messages([
    ("system", "You're an assistant. Your task is to markup sentiment for hotel reviews."),
    ("human", sentiment_msg)
])

sentiment_chain = sentiment_template | chat | StrOutputParser()
```

对于正面评论，我们将要求模型提取好点，而对于负面评论 — 问题。所以，我们将需要两个不同的链。

我们将使用与以前相同的Pydantic输出解析器来指定预期的输出格式并生成指令。

我们在通用主题分配提示消息上使用了`partial_variables`来为正面和负面案例指定不同的格式说明。

```py
from langchain.prompts import PromptTemplate, ChatPromptTemplate, HumanMessagePromptTemplate, SystemMessagePromptTemplate

# defining structure for positive and negative cases 
class PositiveCustomerCommentData(BaseModel):
    topics: List[str] = Field(description="List of the relevant topics for the customer review. Please, include only topics from the provided list.")

    advantages: List[str] = Field(description = "List the good points from that customer mentioned")
    sentiment: str = Field(description="sentiment of the comment (positive, neutral or negative")

class NegativeCustomerCommentData(BaseModel):
    topics: List[str] = Field(description="List of the relevant topics for the customer review. Please, include only topics from the provided list.")

    problems: List[str] = Field(description = "List the problems that customer mentioned.")
    sentiment: str = Field(description="sentiment of the comment (positive, neutral or negative")

# defining output parsers and generating instructions
positive_output_parser = PydanticOutputParser(pydantic_object=PositiveCustomerCommentData)
positive_format_instructions = positive_output_parser.get_format_instructions()

negative_output_parser = PydanticOutputParser(pydantic_object=NegativeCustomerCommentData)
negative_format_instructions = negative_output_parser.get_format_instructions()

general_topic_assignment_msg = '''
Below is a customer review delimited by ```.

请分析提供的评论，并识别主要主题和情感。仅包括下列列表中的主题。

带有描述的主题列表（用“:”分隔）：

{topics_descr_list}

输出格式：

{format_instructions}

客户评论：

```py
{input_data}
```

'''

# 定义提示模板

positive_topic_assignment_template = ChatPromptTemplate(

    messages=[

        SystemMessagePromptTemplate.from_template("你是一个有帮助的助手。你的任务是分析酒店评论。"),

        HumanMessagePromptTemplate.from_template(general_topic_assignment_msg)

    ],

    input_variables=["topics_descr_list", "input_data"],

    partial_variables={"format_instructions": positive_format_instructions} )

negative_topic_assignment_template = ChatPromptTemplate(

    messages=[

        SystemMessagePromptTemplate.from_template("你是一个有帮助的助手。你的任务是分析酒店评论。"),

        HumanMessagePromptTemplate.from_template(general_topic_assignment_msg)

    ],

    input_variables=["topics_descr_list", "input_data"],

    partial_variables={"format_instructions": negative_format_instructions} )

```py

So, now we need just to build the full chain. The main logic is defined using `RunnableBranch` and condition based on sentiment, an output of `sentiment_chain`.

```

from langchain.schema.runnable import RunnableBranch

branch = RunnableBranch(

(lambda x: "negative" in x["sentiment"].lower(), negative_chain),

positive_chain

)

full_route_chain = {

    "sentiment": sentiment_chain,

    "input_data": lambda x: x["input_data"],

    "topics_descr_list": lambda x: x["topics_descr_list"]

} | branch

full_route_chain.invoke({'input_data': review,

'topics_descr_list': topics_list})

```py

Here are a couple of examples. It works pretty well and returns different objects depending on the sentiment.

![](../Images/47cd0e4548c96667425a56e62d254b10.png)

We’ve looked in detail at the modular approach to do Topic Modelling using LangChain and introduce more complex logic. Now, it’s time to move on to the second part and discuss how we could assess the model’s performance.

# Evaluation

The crucial part of any system running in production is [evaluation](https://python.langchain.com/docs/guides/evaluation). When we have an LLM model running in production, we want to ensure quality and keep an eye on it over time.

In many cases, you could use not only human-in-the-loop (when people are checking the model results for a small sample over time to control performance) but also leverage LLM for this task as well. It could be a good idea to use a more complex model for runtime checks. For example, we used ChatGPT 3.5 for our topic assignments, but we could use GPT 4 for evaluation (similar to the concept of supervision in real life when you are asking more senior colleagues for a code review).

Langchain can help us with this task as well since it provides some tools to evaluate results:

*   [String Evaluators](https://python.langchain.com/docs/guides/evaluation/string/) help to evaluate results from your model. There is quite a broad set of tools, from validating the format to assessing correctness based on provided context or reference. We will talk about these methods in detail below.
*   The other class of evaluators are [Comparison evaluators](https://python.langchain.com/docs/guides/evaluation/comparison/). They will be handy if you want to assess the performance of 2 different LLM models (A/B testing use case). We won’t go into their details today.

## Exact match

The most straightforward approach is to compare the model’s output to the correct answer (i.e. from experts or a training set) using an exact match. For that, we could use `ExactMatchStringEvaluator`, for example, to assess the performance of our sentiment analysis. In this case, we don’t need LLMs.

```

from langchain.evaluation import ExactMatchStringEvaluator

evaluator = ExactMatchStringEvaluator(

    ignore_case=True,

    ignore_numbers=True,

    ignore_punctuation=True,

)

evaluator.evaluate_strings(

    prediction="positive.",

    reference="Positive"

)

# {'score': 1}

evaluator.evaluate_strings(

    prediction="negative",

    reference="Positive"

)

# {'score': 0}

```py

You can build your own [custom String Evaluator](https://python.langchain.com/docs/guides/evaluation/string/custom) or match output to [a regular expression](https://python.langchain.com/docs/guides/evaluation/string/regex_match).

Also, there are helpful tools to validate structured output, whether the output is a valid JSON, has the expected structure and is close to the reference by distance. You can find more details about it in [the documentation](https://python.langchain.com/docs/guides/evaluation/string/json).

## Embeddings distance evaluation

The other handy approach is to look at [the distance between embeddings](https://python.langchain.com/docs/guides/evaluation/string/embedding_distance). You will get a score in the result: the lower the score — the better, since answers are closer to each other. For example, we can compare found good points by Euclidean distance.

```

from langchain.evaluation import load_evaluator

from langchain.evaluation import EmbeddingDistance

evaluator = load_evaluator(

    "embedding_distance", distance_metric=EmbeddingDistance.EUCLIDEAN

)

evaluator.evaluate_strings(

prediction="设计良好的房间，干净，位置优越",

reference="设计良好的房间，干净，位置优越，氛围很好"

)

{'score': 0.20732719121627757}

```py

We got a distance of 0.2\. However, the results of such evaluation might be more difficult to interpret since you will need to look at your data distributions and define thresholds. Let’s move on to approaches based on LLMs since we will be able to interpret their results effortlessly.

## Criteria evaluation

You can use [LangChain](https://python.langchain.com/docs/guides/evaluation/string/criteria_eval_chain) to validate LLM’s answer against some rubric or criteria. There’s a list of predefined criteria, or you can create a custom one.

```

from langchain.evaluation import Criteria

list(Criteria)

[<Criteria.CONCISENESS: '简洁性'>,

<Criteria.RELEVANCE: '相关性'>,

<Criteria.CORRECTNESS: '正确性'>,

<Criteria.COHERENCE: '连贯性'>,

<Criteria.HARMFULNESS: '有害性'>,

<Criteria.MALICIOUSNESS: '恶意'>,

<Criteria.HELPFULNESS: '有用性'>,

<Criteria.CONTROVERSIALITY: '争议性'>,

<Criteria.MISOGYNY: '厌女'>,

<Criteria.CRIMINALITY: '犯罪性'>,

<Criteria.INSENSITIVITY: '不敏感性'>,

<Criteria.DEPTH: '深度'>,

<Criteria.CREATIVITY: '创造力'>,

<Criteria.DETAIL: '细节'>]

```py

Some of them don’t require reference (for example, `harmfulness` or `conciseness`). But for `correctness`, you need to know the answer.
Let’s try to use it for our data.

```

evaluator = load_evaluator("criteria", criteria="简洁性")

eval_result = evaluator.evaluate_strings(

    prediction="设计良好的房间，干净，位置优越",

    input="列出客户提到的优点",

)

```py

As a result, we got the answer (whether the results fit the specified criterion) and chain-of-thought reasoning so that we could understand the logic behind the result and potentially tweak the prompt.

![](../Images/8714d05f4b2e5cfac200311caf698c5c.png)

If you’re interested in how it works, you could switch on `langchain.debug = True` and see the prompt sent to LLM.

![](../Images/0da298b09e09569e208799682be0e438.png)

Let’s look at the correctness criterion. To assess it, we need to provide a reference (the correct answer).

```

evaluator = load_evaluator("labeled_criteria", criteria="正确性")

eval_result = evaluator.evaluate_strings(

    prediction="设计良好的房间，干净，位置优越",

    input="列出客户提到的优点",

    reference="设计良好的房间，干净，位置优越，氛围很好",

)

```py

![](../Images/53ca5ff20f20000706d084f3caee8754.png)

You can even create your own custom criteria, for example, whether multiple points are mentioned in the answer.

```

custom_criterion = {"multiple": "输出是否包含多个点？"}

evaluator = load_evaluator("criteria", criteria=custom_criterion)

eval_result = evaluator.evaluate_strings(

    prediction="设计良好的房间，干净，位置优越",

    input="列出客户提到的优点",

)

```py

![](../Images/267f33d58f6493ce44003b49671882f0.png)

## Scoring evaluation

With criteria evaluation, we got only a Yes or No answer, but in many cases, it is not enough. For example, in our example, the prediction has 3 out of 4 mentioned points, which is a good result, but we got N when evaluating it for correctness. So, using this approach, answers “well-designed rooms, clean, great location” and “fast internet” will be equal in terms of our metrics, which won’t give us enough information to understand the model’s performance.

There’s another pretty close [technique of scoring](https://python.langchain.com/docs/guides/evaluation/string/regex_match) when you’re asking LLM to provide the score in the output, which might help to get more granular results. Let’s try it.

```

from langchain.chat_models import ChatOpenAI

accuracy_criteria = {

    "accuracy": """

分数 1: 答案没有提到任何相关点。

分数 3: 答案只提到了一些相关点，但有重大不准确或包含几个不相关选项。

分数 5: 答案有适量相关选项，但可能存在不准确或错误的点。

分数 7: 答案与参考一致，展示了大部分相关点，并且没有完全错误的选项。

分数 10: 答案完全准确，并与参考完全一致。

}

evaluator = load_evaluator(

    "labeled_score_string",

    criteria=accuracy_criteria,

    llm=ChatOpenAI(model="gpt-4"),

)

eval_result = evaluator.evaluate_strings(

    prediction="设计良好的房间，干净，位置优越",

    input="""以下是由```py. Provide the list the good points that customer mentioned in the customer review.
    Customer review:
    ```限定的客户评论

    小而设计良好的房间，干净，位置优越，氛围很好。我会再次入住。欧式早餐一般但可以接受。

    ```py
    """,
    reference="well designed rooms, clean, great location, good atmosphere"
)
```

![](../Images/9a85f3ea6a6ec65b20ef73b036dec829.png)

我们得到了七分，这看起来很有效。让我们看看实际使用的提示。

![](../Images/95be175868f753919929dd772eed5abe.png)

不过，我会对 LLM 的分数持保留态度。记住，这不是一个回归函数，分数可能相当主观。

我们一直在使用带有参考的评分模型。但在许多情况下，我们可能没有正确的答案，或者获取答案的成本可能很高。即使没有参考评分，你也可以使用评分评估器让模型评估答案。使用 GPT-4 以获得更有信心的结果是值得的。

```py
accuracy_criteria = {
    "recall": "The asisstant's answer should include all mentioned in the question. If information is missing, score answer lower.",
    "precision": "The assistant's answer should not have any points not present in the question."
}

evaluator = load_evaluator("score_string", criteria=accuracy_criteria,
   llm=ChatOpenAI(model="gpt-4"))

eval_result = evaluator.evaluate_strings(
    prediction="well designed rooms, clean, great location",
    input="""Below is a customer review delimited by ```。提供客户在评价中提到的优点列表。

    客户评价：

    ```py
    Small but well designed rooms, clean, great location, good atmosphere. I would stay there again. Continental breakfast is weak but ok.
    ```

    """

)

```

![](../Images/1afcfe80ddb39f4c0a6255e0ef3a574a.png)

我们得到了与之前相当接近的分数。

我们探讨了很多可能的方式来验证你的输出，所以希望你现在已经准备好测试你的模型结果。

# 总结

在本文中，我们讨论了一些如果我们想要将 LLM 用于生产过程时需要考虑的细微差别。

+   我们探讨了使用 LangChain 框架使我们的解决方案更加模块化，以便我们可以轻松地迭代并使用新方法（例如，从一种 LLM 切换到另一种）。此外，框架通常有助于使我们的代码更易于维护。

+   我们讨论的另一个重要话题是评估模型性能的不同工具。如果我们在生产中使用 LLM，我们需要进行一些持续监控，以确保服务质量，并且值得花些时间基于 LLM 或人机协作创建评估流程。

> 非常感谢你阅读本文。希望它对你有启发。如果你有任何后续问题或评论，请在评论区留言。

# 数据集

*Ganesan, Kavita 和 Zhai, ChengXiang. (2011). OpinRank 评价数据集。

UCI 机器学习库。*[https://doi.org/10.24432/C5QW4W](https://doi.org/10.24432/C5QW4W)*

# 参考

本文基于 DeepLearning.AI 和 LangChain 的课程 [“LangChain for LLM Application Development”](https://www.deeplearning.ai/short-courses/langchain-for-llm-application-development/)。
