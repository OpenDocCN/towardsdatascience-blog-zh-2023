# AI驱动的洞察：利用LangChain和Pinecone与GPT-4

> 原文：[https://towardsdatascience.com/ai-driven-insights-for-product-managers-vol-1-leveraging-langchain-and-pinecone-with-gpt-4-7755485019f9?source=collection_archive---------6-----------------------#2023-06-19](https://towardsdatascience.com/ai-driven-insights-for-product-managers-vol-1-leveraging-langchain-and-pinecone-with-gpt-4-7755485019f9?source=collection_archive---------6-----------------------#2023-06-19)

## 赋能下一代产品经理

[](https://elengabrielyan.medium.com/?source=post_page-----7755485019f9--------------------------------)[![Elen Gabrielyan](../Images/a28280d051c33841e870f57101a731b2.png)](https://elengabrielyan.medium.com/?source=post_page-----7755485019f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7755485019f9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7755485019f9--------------------------------) [Elen Gabrielyan](https://elengabrielyan.medium.com/?source=post_page-----7755485019f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9f456c2bb76&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-driven-insights-for-product-managers-vol-1-leveraging-langchain-and-pinecone-with-gpt-4-7755485019f9&user=Elen+Gabrielyan&userId=9f456c2bb76&source=post_page-9f456c2bb76----7755485019f9---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7755485019f9--------------------------------) ·10分钟阅读·2023年6月19日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7755485019f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-driven-insights-for-product-managers-vol-1-leveraging-langchain-and-pinecone-with-gpt-4-7755485019f9&user=Elen+Gabrielyan&userId=9f456c2bb76&source=-----7755485019f9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7755485019f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fai-driven-insights-for-product-managers-vol-1-leveraging-langchain-and-pinecone-with-gpt-4-7755485019f9&source=-----7755485019f9---------------------bookmark_footer-----------)![](../Images/fb2dd3b8f691b939ee14e08d2c00b81e.png)

高效处理定性数据是产品经理最重要的技能之一；收集数据、分析数据并有效地传达这些数据，提供可行且有价值的洞察。

你可以从许多地方获取定性数据——用户访谈、竞争对手反馈，或使用你产品的人的评论。根据你想要实现的目标，你可能会立即分析这些数据，或者将它们保存起来以备后用。有时，你可能只需要几个用户访谈来验证一个假设。其他时候，你可能需要从一千个用户那里获取反馈以发现趋势或测试想法。因此，你分析这些数据的方法可以根据情况的不同而有所变化。

借助像 GPT-4 这样的**大型语言模型**和像 LangChain 及 Pinecone 这样的 AI 工具，我们可以更有效地处理各种情况和大量数据。在本指南中，我将分享我与这些工具的经验。我的目标是向产品经理以及其他处理定性数据的人员展示如何利用这些 AI 工具从数据中获取更有用的见解。

在这篇指南风格的文章中，你会发现什么？

1.  我将首先向你介绍这些 AI 工具，以及大型语言模型（LLMs）的当前限制。

1.  我将讨论你如何充分利用这些工具来处理现实生活中的使用案例。

1.  以用户反馈分析为例，我将提供代码片段和示例，以展示这些工具在实践中的应用。

请注意：要使用像 GPT-4、LangChain 和 Pinecone 这样的工具，你需要对数据感到舒适，并具备一些基本的编码技能。了解你的客户并能够将数据洞察转化为实际行动也很重要。对 AI 和机器学习的了解是加分项，但不是必须的。

# 了解 AI 工具：为什么你可能需要 LangChain 和 Pinecone

假设你已经对 GPT-4 熟悉，了解一些概念是很重要的，因为我们讨论与 LLMs 相关的工具时会用到这些概念。当前 LLMs（如 GPT-4）的一个主要挑战是它们的‘上下文窗口’——这就是它们一次可以处理和记住多少信息。

目前有两个版本的 GPT-4。标准版具有 8k 的上下文，而扩展版具有 32k 的上下文窗口。给你一个概念，32k token 大约是 24,000 个单词，相当于大约 48 页的文本。但请记住，即使你可以访问 GPT-4，32k 版本也不是对所有人开放的。

此外，OpenAI 最近宣布了一个新的 ChatGPT 模型，称为 gpt-3.5-turbo-16k，它的上下文长度是 gpt-3.5-turbo 的 4 倍。在进行洞察分析时，我建议使用 gpt-4，因为它的推理能力优于 GPT-3.5。但你可以尝试一下，看看哪个模型更适合你的使用案例。

## 为什么我要提到这个？

在处理洞察分析时，如果你有大量数据，或者对不止一个提示感兴趣，那么会遇到一个大挑战。假设你有一个用户访谈。你想更深入地挖掘并从中获取更多的洞察，使用 GPT-4。在这种情况下，你可以直接拿访谈记录并交给 ChatGPT，选择 GPT-4。你可能需要分割一次文本，但仅此而已。你不需要其他复杂的工具。因此，实际上当处理大量定性数据时，你才会需要这些复杂的新工具。让我们了解这些工具是什么，然后我们将转向一些具体的使用案例和示例。

## 那么 LangChain 是什么？

LangChain 是一个围绕 LLM 的框架，提供了各种功能，如聊天机器人、生成式问答（GQA）和总结。它的多功能性在于将不同组件连接在一起，包括提示模板、LLMs、代理和记忆系统。

提示模板是针对不同情况预制的提示，而 LLMs 处理和生成响应。代理帮助根据 LLM 的输出做出决策，记忆系统则存储信息以供后续使用。

在这篇文章中，我将分享一些我例子中的功能。

![](../Images/3da1aa9ce4639b4a93a09d1e73526dca.png)

*LangChain 模块的高级概述*

## Pinecone 是什么？

Pinecone.ai 是一个强大的工具，旨在简化高维数据表示（即向量）的管理。

向量在处理大量文本数据时特别有用，比如当你试图从中提取信息时。考虑一个你在分析反馈并想要了解产品各种细节的情况。仅靠“great”、“improve”或“i suggest”等关键词搜索无法实现这种深度洞察，因为你可能会错过很多上下文。

现在，我不会深入探讨文本向量化的技术细节（这可以是基于词的、基于句子的等）。你需要理解的关键点是，单词通过机器学习模型转换成数字，这些数字然后存储在数组中。

让我们举个例子：

单词 “seafood” 可能被转换为一系列数字，如：[1.2, -0.2, 7.0, 19.9, 3.1, …, 10.2]。

当我搜索另一个词时，该词也会被转换为一系列数字（或向量）。如果我们的机器学习模型正常工作，与“seafood”有类似上下文的单词，其数字系列应该接近于“seafood”的系列。这里有一个例子：

“shrimp” 可能被转换为：[1.1, -0.3, 7.1, 19.8, 3.0, …, 10.5]，其中这些数字接近于“seafood”拥有的数字。

使用 Pinecone.ai，你可以高效地存储和搜索这些向量，实现快速而准确的相似性比较。

通过利用其功能，你可以组织和索引来自 LLM 模型的向量，从而开启更深层次的见解，并在广泛的数据集中发现有意义的模式。

简而言之，Pinecone.ai 允许你以便捷的方式存储你的定性数据的向量表示。你可以轻松地在这些向量中进行搜索，并应用 LLM 模型从中提取有价值的见解。它简化了管理数据和从中获取有意义信息的过程。

![](../Images/81c46b6619a12375dab3bb4390791059.png)

向量数据库的表示

## 你什么时候实际需要像 LangChain 和 Pinecone 这样的工具？

简短回答：当你处理大量的定性数据时。

让我分享一些我的使用案例，以给你一个了解：

+   想象一下你有来自产品渠道的数千条书面反馈。你想识别数据中的模式，并跟踪反馈随时间的演变。

+   假设你有不同语言的评论，你想将它们翻译成你偏好的语言，然后提取见解。

+   你打算通过分析客户评论、反馈和对竞争对手产品的情感来进行竞争分析。

+   你的公司进行调查或用户研究，生成大量的定性反馈。提取有意义的见解，发现趋势，并为产品或服务改进提供信息是你的目标。

这些只是 LangChain 和 Pinecone 等工具在处理定性数据的产品经理工作中的一些宝贵应用示例。

# 示例项目：反馈分析

作为产品经理，我的工作包括改进我们的会议记录和转录功能。为此，我们听取用户对这些功能的评价。

对于我们的[会议记录功能](https://krisp.ai/ai-meeting-assistant/)，用户给我们质量打分1到5分，告诉我们他们使用了哪个模板，并发送他们的评论。流程如下：

![](../Images/e82324a307d9a09ba751a071fe1c457a.png)

在这个项目中，我密切关注了两件事：用户对我们功能的反馈和他们使用了哪些模板。我处理了大量的数据——超过 20,000 字，当我使用一个特殊工具将其拆分时，变成了超过 38,000 个“令牌”（或数据片段）。这数据量之大，超出了某些高级模型一次性处理的能力！

为了帮助我分析这些大量的数据，我转向了两个高级工具：LangChain 和 Pinecone，并辅以 GPT-4。借助这些工具，让我们更深入地探讨这个项目，看看这些高科技工具使我们能够做些什么。

这个项目的主要目标是从收集的数据中提取见解，这需要：

1.  创建与数据集相关的特定查询的能力。

1.  使用 LLMs 处理大量信息。

首先，我会给你一个项目实施的概述。之后，我会分享一些我使用的代码示例。

我们从一组文本文件开始。每个文件包含用户反馈和他们使用的模板名称。你可以处理这些数据以适应你的需求——我在我的项目中需要做一些后处理。记住，你的文件和数据可能不同，所以可以根据自己的项目进行调整。

例如，你想了解用户对会议记录结构的反馈：

```py
query =  "Please list all feedback regarding sentence structures in a table \
in markdown and get a single insight for each one, and give a general summary for all."
```

这里是一个高层次的图示，展示了在使用LLM和Pinecone时的流程。你向GPT-4提问，或者我们称之为“查询”。与此同时，Pinecone，我们的反馈库，提供了你的查询的上下文，当你将问题本身发送给它（“嵌入查询”）时。它们一起帮助我们高效地理解数据：

![](../Images/cc2c3092924645f8cbe8d8a3a06ac72d.png)

以下是图示的简化版本：

![](../Images/dbae8365b9ef674d83a32d23103964c8.png)

让我们开始吧！在这个脚本中，我们设置了一个管道来使用OpenAI的GPT-4、Pinecone和LangChain分析用户反馈数据。本质上，它导入了必要的库，设置了反馈数据的路径，并建立了处理数据的OpenAI API密钥。

```py
import os
import openai
import pinecone
import certifi
import nltk
from tqdm.autonotebook import tqdm
from langchain.document_loaders import DirectoryLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Pinecone
from langchain.llms import OpenAI
from langchain.chains.question_answering import load_qa_chain

directory = 'path to your directory with text files, containing feedback'
OPENAI_API_KEY = "your key"
```

然后我们定义并调用一个函数`load_docs()`，该函数使用LangChain的DirectoryLoader从指定目录加载用户反馈文档。然后，它会统计并显示加载的文档总数。

```py
def load_docs(directory):
  loader = DirectoryLoader(directory)
  documents = loader.load()
  return documents

documents = load_docs(directory)
len(documents)
```

接下来定义并执行`split_docs()`函数，该函数使用LangChain的RecursiveCharacterTextSplitter将加载的文档分割成特定大小和重叠的小块。然后，它会统计并打印出结果块的总数。

```py
def split_docs(documents, chunk_size=500, chunk_overlap=20):
  text_splitter = RecursiveCharacterTextSplitter(chunk_size=chunk_size, chunk_overlap=chunk_overlap)
  docs = text_splitter.split_documents(documents)
  return docs

docs = split_docs(documents)
print(len(docs))
```

要使用Pinecone，这基本上是一个向量数据库，我们需要从文档中获取嵌入，因此我们应该为此引入一个函数。有很多方法可以做到这一点，但我们使用OpenAI的嵌入函数：

```py
# Assuming OpenAIEmbeddings class is imported above
embeddings = OpenAIEmbeddings()

# Let's define a function to generate an embedding for a given query
def generate_embedding(query):
    query_result = embeddings.embed_query(query)
    print(f"Embedding length for the query is: {len(query_result)}")
    return query_result
```

要将这些向量存储到Pinecone中，你需要在那里创建一个帐户并创建一个索引。这个过程非常简单。然后你将从那里获得API密钥、环境名称和索引名称。

```py
MY_API_KEY_p= "the_key"
MY_ENV_p= "the_environment"

pinecone.init(
    api_key=MY_API_KEY_p,
    environment=MY_ENV_p
)

index_name = "your_index_name"

index = Pinecone.from_documents(docs, embeddings, index_name=index_name)
```

下一步是能够找到答案。这就像在可能的答案领域中找到离你的问题最近的点，从而给出最相关的结果。

```py
def get_similiar_docs(query, k=40, score=False):
  if score:
    similar_docs = index.similarity_search_with_score(query, k=k)
  else:
    similar_docs = index.similarity_search(query, k=k)
  return similar_docs
```

在这段代码中，我们使用OpenAI的GPT-4模型和LangChain设置了一个问答系统。`get_answer()`函数接受一个问题作为输入，查找类似的文档，并使用问答链生成一个答案。

```py
from langchain.chat_models import ChatOpenAI
model_name = "gpt-4"

llm = OpenAI(model_name=model_name, temperature =0)

chain = load_qa_chain(llm, chain_type="stuff")

def get_answer(query):
  similar_docs = get_similiar_docs(query)
  answer = chain.run(input_documents=similar_docs, question=query)
  return answer
```

我们来到了问题！或者问题们。你可以问任意多的问题。

```py
query =  "Please list all feedback regarding sentence structures in a table \
in markdown and get a single insight for each one, and give a general summary for all."

answer = get_answer(query)
print(answer) 
```

实现检索问答链：

要实现检索式问答系统，我们使用 LangChain 中的 RetrievalQA 类。它利用 OpenAI LLM 来回答问题，并依赖于一种“stuff”链类型。检索器连接到一个先前创建的索引，并存储在‘qa’变量中。为了更好地理解，你可以了解更多关于[检索技术](https://blog.langchain.dev/retrieval/)的信息。

```py
from langchain.chains import RetrievalQA
retriever = index.as_retriever()

qa_stuff = RetrievalQA.from_chain_type(
    llm=llm, 
    chain_type="stuff", 
    retriever=retriever, 
    verbose=True
)

response = qa_stuff.run(query)
```

所以我们得到了响应，让我们用 Markdown 文本以视觉上更吸引人的格式展示响应变量中存储的内容。这样可以使显示的文本更加有序且易于阅读。

```py
from IPython.display import display, Markdown

display(Markdown(response))
```

![](../Images/354be8b9180c8499f71421e758707b06.png)

示例输出

尝试使用输入文件和查询以获得最佳效果，充分利用这一方法和工具。

# 结论

总而言之，GPT-4、LangChain 和 Pinecone 使处理大量定性数据变得容易。它们帮助我们深入挖掘这些数据并找到有价值的见解，从而指导更好的决策。本文提供了对它们使用的简要介绍，但它们的功能远不止于此。

随着这些工具的不断进步和普及，现在学习使用它们将为你未来带来显著的优势。因此，继续探索和学习这些工具，因为它们正在塑造数据分析的现在和未来。

请继续关注未来探索这些实用工具的更多方法！

> *所有图片，除非另有说明，均由作者提供*

## 参考文献

[LangChain 文档](https://api.python.langchain.com/en/latest/)

[Pinecone 文档](https://docs.pinecone.io/docs/overview)

[LangChain 在 LLM 应用开发中的短期课程](https://learn.deeplearning.ai/langchain/lesson/1/introduction)
