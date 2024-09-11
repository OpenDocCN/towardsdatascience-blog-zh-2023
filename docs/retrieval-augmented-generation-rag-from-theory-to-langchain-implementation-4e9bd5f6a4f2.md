# 检索增强生成（RAG）：从理论到LangChain实现

> 原文：[https://towardsdatascience.com/retrieval-augmented-generation-rag-from-theory-to-langchain-implementation-4e9bd5f6a4f2?source=collection_archive---------0-----------------------#2023-11-14](https://towardsdatascience.com/retrieval-augmented-generation-rag-from-theory-to-langchain-implementation-4e9bd5f6a4f2?source=collection_archive---------0-----------------------#2023-11-14)

## 从原始学术论文的理论到其在OpenAI、Weaviate和LangChain中的Python实现

[](https://medium.com/@iamleonie?source=post_page-----4e9bd5f6a4f2--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----4e9bd5f6a4f2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4e9bd5f6a4f2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4e9bd5f6a4f2--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----4e9bd5f6a4f2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-rag-from-theory-to-langchain-implementation-4e9bd5f6a4f2&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----4e9bd5f6a4f2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4e9bd5f6a4f2--------------------------------) ·7分钟阅读·Nov 14, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4e9bd5f6a4f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-rag-from-theory-to-langchain-implementation-4e9bd5f6a4f2&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----4e9bd5f6a4f2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4e9bd5f6a4f2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fretrieval-augmented-generation-rag-from-theory-to-langchain-implementation-4e9bd5f6a4f2&source=-----4e9bd5f6a4f2---------------------bookmark_footer-----------)![](../Images/ac6c086f3ecee48e8c43eaf4439e8442.png)

检索增强生成工作流程

自意识到可以通过专有数据来强化大型语言模型（LLM）以来，关于如何最有效地弥合LLM的一般知识和您的专有数据之间差距的讨论已经进行了一些讨论。关于[微调与检索增强生成（RAG）哪个更适合此目的的讨论](/rag-vs-finetuning-which-is-the-best-tool-to-boost-your-llm-application-94654b1eaba7)已有很多辩论（剧透：两者都是）。

本文首先关注RAG的概念，并首先涵盖其理论。接着，展示如何使用[LangChain](https://www.langchain.com/)进行简单的RAG流水线编排，使用[OpenAI](https://openai.com/)语言模型和[Weaviate](https://weaviate.io/)向量数据库。

# 什么是检索增强生成

检索增强生成（RAG）是向LLM提供来自外部知识源的额外信息的概念。这使它们能够生成更准确和上下文相关的答案，同时减少幻觉。

## 问题

最先进的LLM是通过大量数据训练的，以实现存储在神经网络权重（参数化记忆）中的广泛的一般知识。然而，促使LLM生成需要其训练数据中未包含的知识的完成，例如更新的、专有的或特定领域的信息，可能导致事实错误（幻觉），如下截图所示：

![](../Images/e4a0faeee6e536b3729d70d00f41a34c.png)

ChatGPT对问题“总统对贝里尔法官说了什么？”的回答

因此，重要的是弥合LLM的一般知识与任何附加背景之间的差距，以帮助LLM生成更准确和上下文相关的完成，同时减少幻觉。

## 解决方案

传统上，神经网络通过微调模型来适应特定领域或专有信息。虽然这种技术有效，但也计算密集、昂贵，并且需要技术专业知识，使其适应不断变化的信息变得不那么灵活。

2020年，Lewis等人在论文[知识密集型自然语言处理任务的检索增强生成](https://arxiv.org/abs/2005.11401) [1]中提出了一种更灵活的技术，称为检索增强生成（RAG）。在这篇论文中，研究人员将生成模型与检索模块结合起来，以提供来自外部知识源的额外信息，这些信息可以更容易地更新。

**简而言之，** RAG对LLM来说就像对人类开放书籍考试一样。在开放书籍考试中，学生被允许携带参考材料，例如教科书或笔记，他们可以用来查找相关信息来回答问题。开放书籍考试背后的思想是，考试侧重于学生的推理能力，而不是他们记忆特定信息的能力。

类似地，事实知识与LLM的推理能力分离，并存储在可以轻松访问和更新的外部知识源中：

+   **参数化知识：** 在训练期间学习，隐含存储在神经网络的权重中。

+   **非参数化知识：** 存储在外部知识源中，例如向量数据库。

（顺便说一句，这个天才比较不是我想出来的。据我所知，这个比较是[JJ在Kaggle - LLM科学考试竞赛期间首次提到的](https://www.kaggle.com/code/jjinho/open-book-llm-science-exam)。）

Vanilla RAG 工作流程如下所示：

![](../Images/ac6c086f3ecee48e8c43eaf4439e8442.png)

检索增强生成工作流程

1.  **检索：** 使用用户查询从外部知识源中检索相关内容。为此，用户查询嵌入到与向量数据库中的额外上下文相同的向量空间中。这允许执行相似性搜索，并从向量数据库返回前k个最接近的数据对象。

1.  **增强：** 用户查询和检索的额外内容被填充到提示模板中。

1.  **生成：** 最后，将检索增强提示输入LLM。

# 使用LangChain进行检索增强生成实现

本节使用OpenAI LLM结合Weaviate向量数据库和OpenAI嵌入模型实现Python中的RAG管道。[LangChain](https://www.langchain.com/) 用于编排。

如果您对LangChain或Weaviate不熟悉，您可能希望查看以下两篇文章：

[](/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c?source=post_page-----4e9bd5f6a4f2--------------------------------) [## 开始使用LangChain：构建LLM驱动应用程序的初学者指南

### 用Python构建大型语言模型的LangChain教程

towardsdatascience.com](/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c?source=post_page-----4e9bd5f6a4f2--------------------------------) [](/getting-started-with-weaviate-a-beginners-guide-to-search-with-vector-databases-14bbb9285839?source=post_page-----4e9bd5f6a4f2--------------------------------) [## 开始使用Weaviate：使用向量数据库进行搜索的初学者指南

### 如何使用OpenAI和Python中的向量数据库进行语义搜索、问答和生成搜索

towardsdatascience.com](/getting-started-with-weaviate-a-beginners-guide-to-search-with-vector-databases-14bbb9285839?source=post_page-----4e9bd5f6a4f2--------------------------------)

## 先决条件

确保您已安装所需的Python包：

+   `langchain` 用于编排

+   `openai` 用于嵌入模型和LLM

+   `weaviate-client` 用于向量数据库

```py
#!pip install langchain openai weaviate-client
```

此外，在根目录下的 .env 文件中定义相关的环境变量。要获取 OpenAI API 密钥，你需要一个 OpenAI 账户，然后在[API 密钥](https://platform.openai.com/account/api-keys)下“创建新密钥”。

```py
OPENAI_API_KEY="<YOUR_OPENAI_API_KEY>"
```

然后，运行以下命令以加载相关的环境变量。

```py
import dotenv
dotenv.load_dotenv()
```

## 准备工作

作为准备步骤，你需要准备一个作为外部知识来源的向量数据库，它包含所有附加信息。通过以下步骤填充这个向量数据库：

1.  收集和加载数据

1.  分割文档

1.  嵌入和存储片段

第一步是**收集和加载你的数据** — 对于这个示例，你将使用[拜登总统 2022 年国情咨文](https://www.whitehouse.gov/state-of-the-union-2022/)作为额外的上下文。原始文本文件可在[LangChain 的 GitHub 仓库](https://raw.githubusercontent.com/langchain-ai/langchain/master/docs/docs/modules/state_of_the_union.txt)中获得。要加载数据，你可以使用 LangChain 提供的众多内置的`[DocumentLoader](https://api.python.langchain.com/en/latest/api_reference.html#module-langchain.document_loaders)`之一。`Document` 是一个包含文本和元数据的字典。要加载文本，你将使用 LangChain 的 `TextLoader`。

```py
import requests
from langchain.document_loaders import TextLoader

url = "https://raw.githubusercontent.com/langchain-ai/langchain/master/docs/docs/modules/state_of_the_union.txt"
res = requests.get(url)
with open("state_of_the_union.txt", "w") as f:
    f.write(res.text)

loader = TextLoader('./state_of_the_union.txt')
documents = loader.load()
```

接下来，**分割你的文档 —** 由于 `Document` 在其原始状态下过长，无法适配 LLM 的上下文窗口，你需要将其分割成更小的片段。LangChain 提供了许多内置的[文本分割器](https://python.langchain.com/docs/modules/data_connection/document_transformers/)用于此目的。对于这个简单的示例，你可以使用 `CharacterTextSplitter`，其 `chunk_size` 大约为 500，`chunk_overlap` 为 50，以保持片段之间的文本连续性。

```py
from langchain.text_splitter import CharacterTextSplitter
text_splitter = CharacterTextSplitter(chunk_size=500, chunk_overlap=50)
chunks = text_splitter.split_documents(documents)
```

最后，**嵌入和存储片段** — 为了在文本片段中启用语义搜索，你需要为每个片段生成向量嵌入，然后将它们与嵌入一起存储。为了生成向量嵌入，你可以使用 OpenAI 嵌入模型，而为了存储它们，你可以使用 Weaviate 向量数据库。通过调用 `.from_documents()`，向量数据库会自动填充片段。

```py
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Weaviate
import weaviate
from weaviate.embedded import EmbeddedOptions

client = weaviate.Client(
  embedded_options = EmbeddedOptions()
)

vectorstore = Weaviate.from_documents(
    client = client,    
    documents = chunks,
    embedding = OpenAIEmbeddings(),
    by_text = False
)
```

## 步骤 1：检索

一旦向量数据库被填充，你可以将其定义为检索组件，它根据用户查询和嵌入片段之间的语义相似性来获取额外的上下文。

```py
retriever = vectorstore.as_retriever()
```

## 步骤 2：增强

接下来，为了增强提示内容，你需要准备一个提示模板。提示可以很容易地从提示模板中定制，如下所示。

```py
from langchain.prompts import ChatPromptTemplate

template = """You are an assistant for question-answering tasks. 
Use the following pieces of retrieved context to answer the question. 
If you don't know the answer, just say that you don't know. 
Use three sentences maximum and keep the answer concise.
Question: {question} 
Context: {context} 
Answer:
"""
prompt = ChatPromptTemplate.from_template(template)

print(prompt)
```

## 步骤 3：生成

最后，你可以为 RAG 流程管道构建一个链，将检索器、提示模板和 LLM 链接在一起。一旦定义了 RAG 链，你可以调用它。

```py
from langchain.chat_models import ChatOpenAI
from langchain.schema.runnable import RunnablePassthrough
from langchain.schema.output_parser import StrOutputParser

llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)

rag_chain = (
    {"context": retriever,  "question": RunnablePassthrough()} 
    | prompt 
    | llm
    | StrOutputParser() 
)

query = "What did the president say about Justice Breyer"
rag_chain.invoke(query)
```

```py
"The president thanked Justice Breyer for his service and acknowledged his dedication to serving the country. 
The president also mentioned that he nominated Judge Ketanji Brown Jackson as a successor to continue Justice Breyer's legacy of excellence."
```

你可以在下面看到这个特定示例的 RAG 流程管道示意图：

![](../Images/b939dd150f26bf0a9d0200a3748a64cb.png)

检索增强生成工作流

# 总结

本文介绍了RAG的概念，该概念在2020年的论文 [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) [1] 中提出。在介绍概念的理论背景、动机和问题解决方案后，本文使用Python实现了其内容。本文使用了一个 [OpenAI](https://openai.com/) LLM 结合一个 [Weaviate](https://weaviate.io/) 向量数据库和一个 [OpenAI](https://openai.com/) 嵌入模型实现了一个RAG流水线。使用 [LangChain](https://www.langchain.com/) 进行了编排。

# 喜欢这个故事吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时收到通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----4e9bd5f6a4f2--------------------------------) [## 每当Leonie Monigatti发布新文章时，收到邮件通知。

### 每当Leonie Monigatti发布新文章时，收到邮件通知。通过注册，如果你还没有Medium账号…

[medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----4e9bd5f6a4f2--------------------------------)

*找到我在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/),[*Twitter*](https://twitter.com/helloiamleonie)*，和* [*Kaggle*](https://www.kaggle.com/iamleonie)*！*

# 免责声明

在写这篇文章时，我是Weaviate的开发者倡导者。除了本文外，我还在 [LangChain文档中的Weaviate笔记本](https://python.langchain.com/docs/integrations/vectorstores/weaviate) 中添加了相同的示例。或者，你可以从 [LangChain的rag-weaviate模板](https://github.com/langchain-ai/langchain/tree/master/templates/rag-weaviate) 开始。

# 参考文献

## 文献

[1] Lewis, P., et al. (2020). 为知识密集型自然语言处理任务增强检索生成。*Advances in Neural Information Processing Systems*, *33*, 9459–9474.

## 图片

除非另有说明，所有图片均由作者创建。
