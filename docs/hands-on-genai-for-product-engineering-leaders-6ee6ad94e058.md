# 针对产品和工程领导者的动手 GenAI

> 原文：[https://towardsdatascience.com/hands-on-genai-for-product-engineering-leaders-6ee6ad94e058?source=collection_archive---------10-----------------------#2023-11-28](https://towardsdatascience.com/hands-on-genai-for-product-engineering-leaders-6ee6ad94e058?source=collection_archive---------10-----------------------#2023-11-28)

## 通过深入了解基于LLM的产品，做出更好的产品决策

[](https://medium.com/@ninadsohoni?source=post_page-----6ee6ad94e058--------------------------------)[![Ninad Sohoni](../Images/8d6ec40665bb85fb7b4ece99e6a40913.png)](https://medium.com/@ninadsohoni?source=post_page-----6ee6ad94e058--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ee6ad94e058--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6ee6ad94e058--------------------------------) [Ninad Sohoni](https://medium.com/@ninadsohoni?source=post_page-----6ee6ad94e058--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5ee93978501b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-genai-for-product-engineering-leaders-6ee6ad94e058&user=Ninad+Sohoni&userId=5ee93978501b&source=post_page-5ee93978501b----6ee6ad94e058---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ee6ad94e058--------------------------------) · 35 分钟阅读 · 2023年11月28日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6ee6ad94e058&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-genai-for-product-engineering-leaders-6ee6ad94e058&user=Ninad+Sohoni&userId=5ee93978501b&source=-----6ee6ad94e058---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6ee6ad94e058&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhands-on-genai-for-product-engineering-leaders-6ee6ad94e058&source=-----6ee6ad94e058---------------------bookmark_footer-----------)![](../Images/4b2f853b2bede6542039512f44e4c4c2.png)

图片由 [Bing Image Creator](https://www.bing.com/create) 根据提示“为机器学习驱动的应用程序工作中的产品所有者”生成

# 介绍

如果你是一个普通司机，你可能不在乎你车的引擎盖下是什么。然而，如果你是设计和执行链条中的一部分，负责打造更好的汽车，了解不同部件是什么以及它们如何协同工作，将帮助你打造更好的汽车。

同样，作为产品负责人、业务领导或负责创建新大型语言模型（LLM）驱动产品的工程师，或将LLM/生成性AI引入现有产品的工程师，了解LLM驱动产品的构建模块将帮助你解决与技术相关的战略和战术问题，例如，

1.  我们的使用案例是否适合LLM驱动的解决方案？也许传统的分析、监督式机器学习或其他方法更合适？

1.  如果LLM是可行的，我们的使用案例现在或在不久的将来是否可以通过现成的产品（比如ChatGPT Enterprise）来解决？这是经典的构建与购买决策。

1.  我们的LLM驱动产品的不同构建模块有哪些？其中哪些已经商品化，哪些可能需要更多时间来构建和测试？

1.  我们如何衡量解决方案的性能？有哪些杠杆可以提高我们产品输出的质量？

1.  我们的数据质量是否符合使用案例的要求？我们是否正确地组织了数据，并将相关数据传递给了LLM？

1.  我们能否确信LLM的回答始终是事实准确的？也就是说，我们的解决方案是否会在生成回答时偶尔出现“幻觉”？

虽然这些问题在文章后面会得到回答，但通过动手实践的目标是建立对LLM驱动解决方案的直观理解，这应该有助于你自己回答这些问题，或者至少让你更好地进行进一步研究。

在一篇[上一篇文章](/how-genai-solutions-revolutionize-business-automation-57747b0f11ce)中，我**深入探讨**了与构建LLM驱动产品相关的一些基础概念。但你不能仅仅通过阅读博客或观看视频来学会驾驶——这需要你亲自上路。好在我们生活的时代提供了免费的工具（这些工具的创建[花费了数百万美元](https://lambdalabs.com/blog/demystifying-gpt-3)），我们可以在不到一小时的时间里构建自己的LLM解决方案！因此，在这篇文章中，我建议我们就这样做。这比学习驾驶要容易得多😝。

**构建一个允许你与网站“聊天”的聊天机器人**

> 目标：构建一个基于提供的网站信息回答问题的聊天机器人，**以更好地理解**当前流行的GenAI解决方案的构建模块

我们将创建一个基于知识库信息回答问题的问答聊天机器人。这种解决方案模式，称为检索增强生成（RAG），已成为公司中的首选解决方案模式。RAG 之所以受欢迎的一个原因是，它不仅依赖于 LLM 自身的知识，还可以以自动化的方式将外部信息带入 LLM。在实际应用中，外部信息可以来自组织自己的知识库，包含专有信息，以使产品能够回答有关业务、产品、业务流程等问题。RAG 还减少了 LLM 的“幻觉”，即生成的响应是基于提供给 LLM 的信息的。根据 [最近的一次演讲](https://www.youtube.com/watch?v=xa7k9MUeIdk)，

> “RAG 将是企业使用 LLM 的默认方式”
> 
> -Dr. Waleed Kadous, Chief Scientist, AnyScale

在我们的实操练习中，我们将允许用户输入一个网站，我们的解决方案将“读取”该网站到其知识库中。然后，解决方案将能够根据网站上的信息回答问题。这个网站是一个占位符——实际上，可以调整为从任何数据源如 PDFs、Excel、其他产品或内部系统等获取文本。这种方法也适用于其他媒体——如图像——但它们需要一些不同的 LLM。目前，我们将重点关注来自网站的文本。

作为示例，我们将使用为本博客创建的示例书单网页：[Books I’d Pick Up — If There Were More Hours in the Day!](https://ninadsohoni.github.io/booklist/) 您也可以使用您选择的其他网站。

这就是我们的结果的样子：

![](../Images/781fc874901946c9c92cb4c6ab0f0984.png)

LLM 驱动的聊天机器人可以根据网站上的信息智能回答问题。（图像由作者提供）

以下是我们将遵循的步骤来构建我们的解决方案：

0\. 设置 — Google Colaboratory 和 OpenAI API 密钥

1\. 创建知识库

2\. 搜索与问题相关的上下文

3\. 使用 LLM 生成答案

4\. 添加“聊天”功能（可选）

5\. 添加一个简单的预编码 UI（可选）

## **0.1\. 设置 — Google Colaboratory 和 OpenAI API 密钥**

要构建一个 LLM 解决方案，我们需要一个编写和运行代码的地方，以及一个生成问题回答的 LLM。我们将使用 Google Colab 作为代码环境，并使用 ChatGPT 背后的模型作为我们的 LLM。

首先设置 [Google Colab](https://colab.google/)，这是一个由 Google 提供的免费服务，可以以易于阅读的格式运行 Python 代码 — 无需在计算机上安装任何东西。我发现将 Colab 添加到 Google Drive 中很方便，这样我就可以轻松找到 Colab 笔记本。

为此，导航至 **Google Drive**（使用浏览器） **> 新建 > 更多 > 连接更多应用 >** 在 Google Marketplace 中 **搜索 “Colaboratory”** **> 安装。**

要开始使用 Colab，你可以选择 **新建** > **更多** > **Google Colaboratory**。这将会在你的 Google 云端硬盘中创建一个新的笔记本，方便你返回继续使用。

![](../Images/ee514c154f891d1fd129b21195a698bd.png)

Google Colaboratory 可以在 Google 云端硬盘中访问。（图片由作者提供）

接下来，让我们获取对 LLM 的访问权限。有几个开源和专有选项可供选择。虽然开源 LLM 是免费的，但强大的 LLM 通常需要强大的 GPU 来处理输入和生成响应，且 GPU 的运行成本较低。在我们的示例中，我们将使用 OpenAI 的服务来使用 ChatGPT 所用的 LLM。为此，你需要一个 API 密钥，它类似于用户名/密码的组合，用以让 OpenAI 知道是谁在尝试访问 LLM。根据此时的信息，OpenAI 为新用户提供了 $5 的信用额度，足以用于本实践教程。以下是获取 API 密钥的步骤：

访问[**OpenAI 平台网站**](https://platform.openai.com/signup/)> **开始使用 > 注册**，使用电子邮件和密码进行注册，或使用 Google 或 Microsoft 帐户注册。你还可能需要一个电话号码进行验证。

登录后，点击右上角的个人资料图标 > **查看 API 密钥** > **创建新密钥**。密钥将类似于以下内容（仅供参考的假密钥）。请保存以备后用。

```py
sk-4f3a9b8e7c4f4c8f8f3a9b8e7c4f4c8f-UsH4C3vE64
```

现在，我们已经准备好构建解决方案了。

## 0.2\. 准备构建解决方案的笔记本

我们需要在 Colab 环境中安装一些软件包以方便我们的解决方案。只需在 Colab 中的文本框（称为“单元格”）中输入以下代码，然后按“Shift + Return（Enter）”。或者，直接点击单元格左侧的“播放”按钮或使用笔记本顶部的“运行”菜单。你可能需要使用菜单插入新的代码单元格以运行后续代码：

```py
# Install OpenAI & tiktoken packages to use the embeddings model as well as the chat completion model
!pip install openai tiktoken
# Install the langchain package to facilitate a most of the functionality in our solution, from processing documents to enabling "chat" using LLM
!pip install langchain
# Install ChromaDB - an in-memory vector database package - to save the "knowledge" relied on by our solution to answer questions
!pip install chromadb
# Install HTML to text package to transform webpage content to a more human readable format
!pip install html2text
# Install gradio to create a basic UI for our solution
!pip install gradio
```

接下来，我们应该从已安装的软件包中提取代码，以便在编写的代码中使用这些软件包。你可以使用新的代码单元格并再次按“Shift + Return” — 以这种方式继续进行每个后续的代码块。

```py
# Import packages needed to enable different functionality for the solution
from langchain.document_loaders import AsyncHtmlLoader # To load website content into a document
from langchain.text_splitter import MarkdownHeaderTextSplitter # To document into smaller chunks by document headings 
from langchain.document_transformers import Html2TextTransformer # To converrt HTML to Markdown text
from langchain.chat_models import ChatOpenAI # To use OpenAI's LLM
from langchain.prompts import PromptTemplate # To formulate instructions / prompts
from langchain.chains import RetrievalQA, ConversationalRetrievalChain # For RAG
from langchain.memory import ConversationTokenBufferMemory # To maintain chat history
from langchain.embeddings.openai import OpenAIEmbeddings # To convert text to numerical representation
from langchain.vectorstores import Chroma # To interact with vector database
import pandas as pd, gradio as gr # To show data as tables, and to build UI respectively
import chromadb, json, textwrap # Vector database, converting json to text, and prettify printing respectively
from chromadb.utils import embedding_functions # Setting up embedding function, following protocol required by Chroma
```

最后，将 OpenAI API 密钥添加到一个变量中。请注意，这个密钥类似于你的密码 — 请勿分享。此外，在分享你的 Colab 笔记本前，请务必先删除 API 密钥。

```py
# Add your OpenAI API Key to a variable
# Saving the key in a variable like so is bad practice. It should be loaded into environment variables and loaded from there, but this is okay for a quick demo
OPENAI_API_KEY='sk-4f3a9b8e7c4f4c8f8f3a9b8e7c4f4c8f-UsH4C3vE64' # Fake Key - use your own real key here
```

现在我们准备开始构建解决方案。以下是接下来步骤的高级视图：

![](../Images/f4773785ae6fd1047c8a8c724bb381bf.png)

构建 RAG 解决方案的核心步骤（图片由作者提供）

在编码时，我们将使用LangChain，它已经成为构建此类解决方案的流行框架。它有助于从连接数据源到发送和接收LLM信息的每个步骤。LlamaIndex是另一个简化构建LLM驱动应用的选项。虽然并不严格要求使用LangChain（或LlamaIndex），并且在某些情况下，高级抽象可能使团队对内部发生的事情一无所知，但我们将使用LangChain，但仍会经常查看内部情况。

请注意，由于创新的速度如此之快，可能会有代码中使用的包更新，有些更新可能会导致代码停止工作，除非相应地进行更新。我不打算保持代码的最新状态。然而，本文旨在作为演示，代码可以作为参考或起点，您可以根据需要进行调整。

## 1\. 创建知识库

**1.1\. 确定并读取文档** 让我们访问书单并将内容读取到我们的Colab环境中。内容最初以HTML格式加载，这对网页浏览器很有用。然而，我们将使用HTML转文本工具将其转换为更易读的格式。

```py
url = "https://ninadsohoni.github.io/booklist/" # Feel free to use any other website here, but note that some code might need to be edited to display contents properly

# Load HTML from URL and transform to a more readable text format
docs = Html2TextTransformer().transform_documents(AsyncHtmlLoader(url).load())

# Let's take a quick peek again to see what do we have now
print("\n\nIncluded metadata:\n", textwrap.fill(json.dumps(docs[0].metadata), width=100), "\n\n")
print("Page content loaded:")
print('...', textwrap.fill(docs[0].page_content[2500:3000], width=100, replace_whitespace=False), '...')
```

以下是运行代码在Google Colab上生成的内容：

![](../Images/de6416c74c20602de4edbb11a39d23f5.png)

执行上述代码的结果。网站内容被加载到Colab环境中。（图片由作者提供）

**1.2\. 将文档分解为较小的摘录** 在我们将博客的信息加载到我们的知识库（即我们选择的数据库）之前，还有一步。文本不应按原样加载到数据库中。它应首先分割成较小的块。这有几个原因：

1.  如果我们的文本太长，由于超出文本长度阈值（即*“上下文大小”*），则不能发送给LLM。

1.  较长的文本可能包含广泛且松散相关的信息。我们将依赖LLM挑选出相关部分——这可能不会总是如预期那样有效。使用较小的块，我们可以利用检索机制来识别仅相关的信息并发送给LLM，如我们后面将看到的。

1.  LLM对文本的开始和结束部分关注较强，因此较长的块可能导致LLM对后面更多的内容关注较少（称为*“在中间迷失”*）。

每个用例的适当块大小将根据用例的具体情况而有所不同，包括内容类型、使用的LLM及其他因素。明智的做法是尝试不同的块大小并在确定解决方案之前评估响应质量。在本演示中，我们将使用上下文感知的分块，其中书单中的每个书籍推荐都有自己的块。

```py
# Now we split the entire content of the website into smaller chunks
# Each book review will get its own chunk since we are splitting by headings
# The LangChain splitter used here will also create a set of metadata from headings and associate it with the text in each chunk
headers_to_split_on = [ ("#", "Header 1"), ("##", "Header 2"),
    ("###", "Header 3"), ("####", "Header 4"), ("#####", "Header 5") ]
splitter = MarkdownHeaderTextSplitter(headers_to_split_on = headers_to_split_on)
chunks = splitter.split_text(docs[0].page_content)

print(f"{len(chunks)} smaller chunks generated from original document")

# Let's look at one of the chunks
print("\nLooking at a sample chunk:")
print("Included metadata:\n", textwrap.fill(json.dumps(chunks[5].metadata), width=100), "\n\n")
print("Page content loaded:")
print(textwrap.fill(chunks[5].page_content[:500], width=100, drop_whitespace=False), '...')
```

![](../Images/ca21a3be9e3be635cf3f4fdb21f09bd4.png)

原始内容拆分后的众多文档块之一。（图片由作者提供）

请注意，如果迄今为止创建的文本块仍然比所需的长度长，可以使用其他文本拆分算法进一步拆分，这些算法可以通过 LangChain 或 LlamaIndex 轻松获得。例如，每本书的评论可以根据需要拆分为段落。

**1.3\. 将摘录加载到知识库** 文本块现在准备好加载到知识库中。这些文本块首先通过嵌入模型转换为一系列捕捉文本意义的数字。然后，实际的文本及其数值表示（即嵌入）将加载到向量数据库——我们的知识库中。请注意，嵌入也由大语言模型（LLMs）生成，只是与聊天 LLM 的类型不同。如果你想了解更多关于嵌入的内容，[上一篇文章](/how-genai-solutions-revolutionize-business-automation-57747b0f11ce)通过示例展示了这一概念。

我们将使用向量数据库来存储所有信息。这将实现我们的知识库。向量数据库是专门设计用于通过嵌入相似性进行搜索的。如果我们想从数据库中搜索某些内容，搜索词首先通过嵌入模型转换为数值表示，然后将问题的嵌入与数据库中的所有嵌入进行比较。与问题嵌入最接近的记录（在我们的例子中，就是关于每本书的文本块）作为搜索结果返回，只要它们超过了一个阈值。

```py
# We will get embeddings for each chunk (and subsequently questions) using an embeddings model from OpenAI
openai_embedding_func = embedding_functions.OpenAIEmbeddingFunction(api_key=OPENAI_API_KEY)

# Initialize vector DB and create a collection
persistent_chroma_client = chromadb.PersistentClient()
collection = persistent_chroma_client.get_or_create_collection("my_rag_demo_collection", embedding_function=openai_embedding_func)
cur_max_id = collection.count() # To not overwrite existing data

# Let's add data to our collection in the vector DB
collection.add(
    ids=[str(t) for t in range(cur_max_id+1, cur_max_id+len(chunks)+1)],
    documents=[t.page_content for t in chunks],
    metadatas=[None if len(t.metadata) == 0 else t.metadata for t in chunks]
    )

print(f"{collection.count()} documents in vector DB")
#  25 documents in vector DB

# Optional: We will write a scrappy helper function to print data slightly better -
#  it limits the length embeddings shown on the screen (since these are over a 1,000 long numbers).
#  Also shows a subset of text from documents as well as metadatas fields
def render_vectorDB_content(chromadb_collection):
    vectordb_data = pd.DataFrame(chromadb_collection.get(include=["embeddings", "metadatas", "documents"]))
    return pd.DataFrame({'IDs': [str(t) if len(str(t)) <= 10 else str(t)[:10] + '...'for t in vectordb_data.ids],
                         'Embeddings': [str(t)[:27] + '...' for t in vectordb_data.embeddings],
                         'Documents': [str(t) if len(str(t)) <= 300 else str(t)[:300] + '...' for t in vectordb_data.documents],
                         'Metadatas': ['' if not t else json.dumps(t) if len(json.dumps(t))  <= 90 else '...' + json.dumps(t)[-90:] for t in vectordb_data.metadatas]
                        })

# Let's take a look at what is in the vector DB using our helper function. We will look at the first 4 chunks
render_vectorDB_content(collection)[:4]
```

![](../Images/06255fabddafae0036919bdeaa6d2fdf.png)

加载到向量数据库中的前几个文本块及其数值表示（即嵌入）的视图。（图片由作者提供）

## 2\. 搜索问题相关的上下文

我们的终极目标是让我们的解决方案从向量数据库知识库中挑选相关信息，并将其与我们希望 LLM 回答的问题一起传递给 LLM。让我们尝试一下向量数据库搜索，询问问题“你能推荐几本侦探小说吗？”

```py
# Here we are linking to the previously created an instance of the ChromaDB database using a LangChain ChromaDB client
vectordb = Chroma(client=persistent_chroma_client, collection_name="my_rag_demo_collection", 
                  embedding_function=OpenAIEmbeddings(openai_api_key=OPENAI_API_KEY)
                  )

# Optional - We will define another scrappy helper function to print data slightly better
def render_source_documents(source_documents):
    return pd.DataFrame({'#': range(1, len(source_documents) + 1), 
                         'Documents': [t.page_content if len(t.page_content) <= 300 else t.page_content[:300] + '...' for t in source_documents],
                         'Metadatas': ['' if not t else '...' + json.dumps(t.metadata, indent=2)[-88:] for t in source_documents]
                         })

# Here's where we are compiling the question
question = "Can you recommend a few detective novels?"

# Running the search against the vector DB based on our question
relevant_chunks = vectordb.similarity_search(question)

# Printing results
print(f"Top {len(relevant_chunks)} search results")
render_source_documents(relevant_chunks)
```

![](../Images/15bebbaf8b218ae61b888fe54953a1f3.png)

问题“你能推荐几本侦探小说吗？”的搜索结果前 4 名（图片由作者提供）

默认情况下，我们得到前 4 个结果，除非我们明确设置不同的值。在这个例子中，排名第一的结果是一本福尔摩斯小说，直接提到了“侦探”一词。第二个结果（*《杰克尔的日子》*）虽然没有“侦探”一词，但提到了“警察机构”和“揭露阴谋”，这些与“侦探小说”在语义上相关。第三个结果（*《卧底经济学家》*）提到了“卧底”一词，尽管它是关于经济学的。我认为最后一个结果被获取是因为它与小说/书籍有关，而不是特定的“侦探小说”，因为请求了四个结果。

同样，使用向量数据库并不是绝对必要的。您可以加载嵌入并在其他存储形式中进行搜索。“普通”关系数据库甚至 Excel 都可以使用。但您需要在应用程序逻辑中处理“相似性”计算，当使用 OpenAI 嵌入时，这可以是点积。另一方面，向量数据库为您处理了这些。

请注意，如果我们想通过元数据预筛选一些搜索结果，我们可以这样做。为了演示，我们将根据元数据中从书单加载的“Header 2”过滤。

```py
# Let's try filtering and accessing records that match a specific metadata filter. This can probably be transformed into a pre-filter if needed
pd.DataFrame(vectordb.get(where = {'Header 2': 'Finance'}))
```

![](../Images/3e7381e8da847e76b51f202714d7e319.png)

基于应用元数据预筛选的搜索结果，仅显示关键列。（图像由作者提供）

LLM 提供了一个有趣的机会，即利用 LLM 本身来检查用户问题，审查可用的元数据，评估是否需要和可能进行基于元数据的预筛选，并制定预筛选查询代码，这些代码可以在向量数据库上实际预筛选数据。有关更多信息，请参见 LangChain 的 [自查询检索器](https://python.langchain.com/docs/modules/data_connection/retrievers/self_query/)。

## 3\. 使用 LLM 生成答案

接下来，我们将向 LLM 添加指令，基本上是说“我将给你一些信息片段和一个问题。请使用提供的信息片段回答问题”。然后，我们将这些指令、向量数据库中的搜索结果和我们的问题打包，并发送给 LLM 进行回应。所有这些步骤都由以下代码完成。

请注意，LangChain 提供了抽象一些代码的机会，因此您的代码不必像以下代码那样冗长。然而，以下代码的目的是展示发送到语言模型的指令。这里也是自定义这些指令的地方——例如，在这种情况下，默认指令被更改为请求 LLM 尽可能简洁地回应。如果默认设置适用于您的用例，您的代码可以完全跳过问题模板部分，LangChain 会在向 LLM 发送请求时使用其包中的默认提示。

```py
# Let's select the language model behind free version of ChatGPT: GPT-3.5-turbo
llm = ChatOpenAI(model_name = 'gpt-3.5-turbo', temperature = 0, openai_api_key = OPENAI_API_KEY)

# Let's build a prompt. This is what actually gets sent to the ChatGPT LLM, with the context from our vector database and question injected into the prompt
template = """Use the following pieces of context to answer the question at the end. 
If you don't know the answer, just say that the available information is not sufficient to answer the question. 
Don't try to make up an answer. Keep the answer as concise as possible, limited to five sentences.
{context}
Question: {question}
Helpful Answer:"""
QA_CHAIN_PROMPT = PromptTemplate.from_template(template)

# Define a Retrieval QA chain, which will take the question, get the relevant context from the vectorDB, and pass both to the language model for a response
qa_chain = RetrievalQA.from_chain_type(llm, 
                                       retriever=vectordb.as_retriever(),
                                       return_source_documents=True,
                                       chain_type_kwargs={"prompt": QA_CHAIN_PROMPT}
                                       )
```

现在，让我们再次请求侦探小说的推荐，看看我们会得到什么回应。

```py
# Let's ask a question and run our question-answering chain
question = "Can you recommend a few detective novels?"

result = qa_chain({"query": question})

# Let's look at the result
result["result"]
```

![](../Images/546144d38d406b1c88712edd0333f838.png)

推荐侦探小说的解决方案回应（图像由作者提供）

让我们确认模型是否回顾了我们从向量数据库中获得的所有四个搜索结果，还是仅仅获取了响应中提到的两个结果？

```py
# Let's look at the source documents used as context by the LLM
# We will use our helper function from before to limit the size of the information shown
render_source_documents(result["source_documents"])
```

![](../Images/5f951dd421f7adbca2edefd17e925e95.png)

与问题一起传递给 LLM 的上下文，以便于响应。（图像由作者提供）

我们可以看到，LLM 仍然访问了所有四个搜索结果，并推断出只有前两本书是侦探小说。

请注意，LLM 的响应每次提问时可能会有所不同，尽管发送的是相同的指令和来自向量数据库的相同信息。例如，在询问关于奇幻书籍推荐时，LLM 有时给出三本书籍推荐，有时则更多——尽管都是来自书单。在所有情况下，最推荐的书籍保持不变。请注意，这些变化发生在将一致性——创造力谱——即“温度”参数配置为 0 以最小化差异的情况下。

## 4\. 添加“聊天”功能（可选）

现在，解决方案具备了必要的核心功能——它能够从网站中读取信息并根据这些信息回答问题。但目前它并未提供“对话式”的用户体验。感谢 ChatGPT，“聊天界面”已成为主流设计：我们现在期望这成为与生成式 AI 尤其是 LLM 互动的“自然”方式 😅。实现聊天界面的第一步涉及向解决方案中添加“内存”。

这里的“内存”是一种假象，LLM 实际上并不记住到目前为止的对话——它需要在每次回合中展示完整的对话记录。因此，如果用户向 LLM 提问后续问题，解决方案将打包原始问题、LLM 的原始答案以及后续问题，并将其发送给 LLM。LLM 阅读整个对话并生成有意义的响应以继续对话。

在问答聊天机器人中，如我们正在构建的这个，通常需要进一步扩展此方法，因为存在中间步骤需要从向量数据库中提取相关信息以制定对用户后续问题的响应。在问答聊天机器人中，“内存”是这样模拟的：

1.  将所有问题和响应保留（在一个变量中）作为“聊天记录”

1.  当用户提问时，将聊天记录和新问题发送给 LLM，并要求生成一个独立的问题

1.  此时，聊天记录不再需要。使用独立的问题在向量数据库上进行新的搜索

1.  将独立问题和搜索结果传递给 LLM，并附上指令以获得最终答案。这个步骤类似于我们在之前阶段“使用 LLM 生成答案”中实施的步骤

虽然我们可以使用简单的变量来跟踪聊天记录，但我们将使用 LangChain 的一种内存类型。我们将使用的特定内存对象提供了一个很好的功能，即在达到你指定的大小限制时自动截断较旧的聊天记录，通常是选定 LLM 可以接受的文本大小。在我们的案例中，LLM 应该能够接受略超过 4,000 个“tokens”（即词汇部分），这大约是 3,000 个单词或 ~5 页的 Word 文档。OpenAI 提供了相同 ChatGPT LLM 的 16k 变体，可以接受 4 倍的输入。因此，需要配置内存大小。

这是实现这些步骤的代码。同样，LangChain提供了更高层次的抽象，代码不必如此明确。这个版本只是为了展示发送到LLM的底层指令——首先将聊天记录浓缩成一个独立的单一问题，然后用于向量数据库搜索，其次根据向量数据库搜索结果生成对生成的独立问题的响应。

```py
# Let's create a memory object to track chat history. This will start accumulating human messages and AI responses.
# Here a "token" based memory is used to restrict the length of chat history to what can be passed into the selected LLM. 
# Generally, the maximum token length configured will depend on the LLM. Assuming we are using the 4K version of the LLM, 
#  we will set the token maximum to 3K, to allow some room for the question prompt.
#  The LLM parameter is to make LangChain aware of the tokenization scheme of the selected LLM. 
memory = ConversationTokenBufferMemory(memory_key="chat_history", return_messages=True, input_key="question", output_key="answer", max_token_limit=3000, llm=llm)

# While LangChain includes a default prompt to generate a standalone question based on the users' latest question and
# any context from the conversation up to that point, we will extend the default prompt with additional instructions.
standalone_question_generator_template = """Given the following conversation and a follow up question, 
rephrase the follow up question to be a standalone question, in its original language.
Be as explicit as possible in formulating the standalone question, and 
include any context necessary to clarify the standalone question.

Conversation:
{chat_history}
Follow Up Question: {question}
Standalone question:"""
updated_condense_question_prompt = PromptTemplate.from_template(standalone_question_generator_template)

# Let's rebuild the final prompt (again, optional since LangChain uses a default prompt, though it might be a little different)
final_response_synthesizer_template = """Use the following pieces of context to answer the question at the end. 
If you don't know the answer, just say that the available information is not sufficient to answer the question. 
Don't try to make up an answer. Keep the answer as concise as possible, limited to five sentences.
{context}
Question: {question}
Helpful Answer:"""
custom_final_prompt = PromptTemplate.from_template(final_response_synthesizer_template)

qa = ConversationalRetrievalChain.from_llm(
    llm=llm, 
    retriever=vectordb.as_retriever(), 
    memory=memory,
    return_source_documents=True,
    return_generated_question=True,
    condense_question_prompt= updated_condense_question_prompt,
    combine_docs_chain_kwargs={"prompt": custom_final_prompt})

# Let's again ask the question we previously asked the retrieval QA chain
query = "Can you recommend a few detective novels?"
result = qa({"question": query})
print(textwrap.fill(result['answer'], width=100))
```

![](../Images/e472c9c36dc137b348e73db60cfafa78.png)

解决方案的侦探小说推荐。与之前仅使用“问答”能力而没有“记忆”时收到的响应相同（图片由作者提供）

让我们提出一个后续问题，并查看响应以验证解决方案现在具有“记忆”并且可以对后续问题进行对话式回答：

```py
query = "Tell me more about the second book"
result = qa({"question": query})
print(textwrap.fill(result['answer'], width=100))
```

![](../Images/fed2232a1253700a345d4e1bb406e967.png)

对“第二本书”的更多信息的后续问题的响应。解决方案返回更多关于同一本书的信息（图片由作者提供）

让我们看看在引擎盖下发生了什么，以验证解决方案确实经过了本节开头概述的四个步骤。让我们从聊天记录开始，以验证解决方案确实记录了到目前为止的对话：

```py
# Let's look at chat history upto this point
result['chat_history']
```

![](../Images/5eb9194b5133cbb28fd5070319810bf1.png)

提问第二个问题后的聊天记录。注意此时响应也包括在对话中。（图片由作者提供）

让我们看看解决方案除了聊天记录外还跟踪了什么：

```py
# Let's print the other parts of the results
print("Here is the standalone question generated by the LLM based on chat history:")
print(textwrap.fill(result['generated_question'], width=100 ))
print("\nHere are the source documents the model referenced:")
display(render_source_documents(result['source_documents']))
print(textwrap.fill(f"\nGenerated Answer: {result['answer']}", width=100, replace_whitespace=False) )
```

![](../Images/396886e2b0b33096284054126381fe35.png)

提问第二个问题后，除聊天记录之外的输出。（图片由作者提供）

解决方案内部使用LLM将问题*“告诉我更多关于第二本书的事”*转换为*“你能提供更多关于弗雷德里克·福赛斯的《贼日》‘The Day of the Jackal’的信息吗？”*。有了这个问题，解决方案能够在向量数据库中搜索任何相关信息，并这次首先检索到《贼日》的信息。虽然注意到也包括了一些关于其他书籍的无关搜索结果。

## 快速的可选侧边栏讨论潜在问题

**潜在问题 #1 — 独立问题生成不佳：** 在我的测试中，聊天解决方案在生成一个好的独立问题时并不总是成功，直到调整了问题生成器提示。例如，对于一个后续问题，“告诉我关于第二本书的事”，生成的后续问题往往是“你能告诉我关于第二本书的什么？”这本身并没有特别的意义，并导致随机的搜索结果，因此LLM的生成响应看起来也是随机的。

**潜在问题 #2 — 原始问题与跟进问题之间的搜索结果变化：** 值得注意的是，即使第二个生成的问题明确提到感兴趣的书籍，向量数据库搜索返回的结果中也包括其他书籍的结果，更重要的是，这些搜索结果与原始问题的结果不同！在这个例子中，这种搜索结果的变化是期望的，因为问题从“侦探小说推荐”变成了特定的小说。然而，当用户提出跟进问题以深入探讨某个主题时，问题表述的变化或LLM生成的独立问题可能会导致不同的搜索结果或搜索结果的不同排序，这可能*不*是期望的。

这个问题可能会在一定程度上自动得到缓解，至少是通过对向量数据库进行更广泛的初步搜索——返回更多的结果，而不仅仅是我们示例中的4-5个——并对结果进行重新排序，以确保最相关的结果上升到顶部并始终发送到LLM以生成最终响应（参见[Cohere的“重新排序”](https://docs.cohere.com/docs/reranking)）。此外，应用程序应相对容易识别搜索结果是否发生了变化。可能可以应用一些启发式方法来判断搜索结果的变化程度（通过排序和重叠度量）以及问题变化的程度（通过余弦相似度等距离度量）是否匹配。至少在搜索结果在聊天轮次中出现意外波动的情况下，可以根据用例的关键性以及最终用户的培训或素养，提醒最终用户并让他们参与更详细的检查。

控制这种行为的另一个想法是利用LLM来决定跟进问题是否需要再次访问向量数据库，或者这个问题是否可以用之前获取的结果有意义地回答。一些用例可能希望生成两组搜索结果和回答，让LLM在答案之间裁定，另一些用例可能通过赋予用户冻结上下文的能力来将控制上下文的责任转交给用户（这取决于用例、用户培训或素养以及其他考虑因素），还有一些用例可能对跟进问题中搜索结果的变化持宽容态度。

正如你可能已经看出，获得一个基本解决方案是相对简单的，但做到完美——这才是难点。这里提到的问题只是冰山一角。好了，回到主要任务 …

## 5\. 添加预编码UI

最后，聊天机器人的功能已经准备好。现在，我们可以添加一个漂亮的用户界面以改善用户体验。这是（某种程度上）由于Python库如Gradio和Streamlit，使得基于Python编写的指令构建前端小部件成为可能。在这里，我们将使用Gradio快速创建一个用户界面。

以使任何尚未执行代码的人能够跟上进度，并演示一些达到相同结果的变体，以下两个代码块是自包含的，可以在全新的Colab笔记本中运行，以生成完整的聊天机器人。

```py
# Initial setup - Install necessary software in code environment
!pip install openai tiktoken langchain chromadb html2text gradio   # Uncomment by removing '#' at the beginning if you're starting here and haven't yet installed anything

# Import packages needed to enable different functionality for the solution
from langchain.document_loaders import AsyncHtmlLoader # To load website content into a document
from langchain.text_splitter import MarkdownHeaderTextSplitter # To document into smaller chunks by document headings 
from langchain.document_transformers import Html2TextTransformer # To converrt HTML to Markdown text
from langchain.chat_models import ChatOpenAI # To use OpenAI's LLM
from langchain.prompts import PromptTemplate # To formulate instructions / prompts
from langchain.chains import RetrievalQA, ConversationalRetrievalChain # For RAG
from langchain.memory import ConversationTokenBufferMemory # To maintain chat history
from langchain.embeddings.openai import OpenAIEmbeddings # To convert text to numerical representation
from langchain.vectorstores import Chroma # To interact with vector database
import pandas as pd, gradio as gr # To show data as tables, and to build UI respectively
import chromadb, json, textwrap # Vector database, converting json to text, and prettify printing respectively
from chromadb.utils import embedding_functions # Setting up embedding function, following protocol required by Chroma

# Add the OpenAI API Key to a variable
# Saving the key in a variable like so is bad practice. It should be loaded into environment variables and loaded from there, but this is okay for a quick demo
OPENAI_API_KEY='sk-4f3a9b8e7c4f4c8f8f3a9b8e7c4f4c8f-UsH4C3vE64' # Fake Key - use your own real key here
```

在运行下一组代码以呈现聊天机器人UI之前，请注意，当通过Colab渲染时，应用程序对于任何拥有链接的人公开可访问3天（链接在Colab笔记本单元输出中提供）。理论上，可以通过将代码中的最后一行更改为*demo.launch(share=False)*来保持应用的私密性，但那时我无法使应用正常工作。相反，我更倾向于在Colab中以“调试”模式运行，这样Colab单元会“运行”直到停止，然后终止聊天机器人。或者，在不同的Colab单元中运行下面的代码，以终止聊天机器人并删除在Colab中加载到Chroma向量数据库中的内容。

```py
# To be run at the end to terminate the demo chatbot
demo.close() # To end the chat session and terminate the shared demo

# Retrieve and delete the vector DB collection created for the chatbot
vectordb = Chroma(client=persistent_chroma_client, collection_name="my_rag_demo_collection", embedding_function=openai_embedding_func_for_langchain)
vectordb.delete_collection()
```

以下是将聊天机器人作为应用运行的代码。大部分代码重复了到目前为止的文章中的代码，所以应该很熟悉。请注意，与之前的代码相比，下面的代码存在一些差异，包括但不限于没有使用之前用过的LangChain的‘token’内存对象进行内存管理。这意味着随着对话的继续，历史记录将变得过长，无法传递给语言模型的上下文，应用需要重启。

```py
# Initiate OpenAI embedding functions. There are two because the function protocol is different when passing the function to Chroma DB directly vs using it with Chroma DB via LangChain
openai_embedding_func_for_langchain = OpenAIEmbeddings(openai_api_key=OPENAI_API_KEY)
openai_embedding_func_for_chroma = embedding_functions.OpenAIEmbeddingFunction(api_key=OPENAI_API_KEY)

# Initiate the LangChain chat model object using the GPT 3.5 turbo model
llm = ChatOpenAI(model_name='gpt-3.5-turbo', temperature=0, openai_api_key=OPENAI_API_KEY)

# Initialize vector DB and create a collection
persistent_chroma_client = chromadb.PersistentClient()
collection = persistent_chroma_client.get_or_create_collection("my_rag_demo_collection", embedding_function=openai_embedding_func_for_chroma)

# Function to load website content into vector DB
def load_content_from_url(url):
  # Load HTML from URL and transform to a more readable text format
  docs = Html2TextTransformer().transform_documents(AsyncHtmlLoader(url).load())
  # Split docs by section
  headers_to_split_on = [ ("#", "Header 1"), ("##", "Header 2"), ("###", "Header 3"), ("####", "Header 4"), ("#####", "Header 5") ]
  chunks = MarkdownHeaderTextSplitter(headers_to_split_on = headers_to_split_on).split_text(docs[0].page_content)
  # Here we are linking to the previously created an instance of the ChromaDB database
  vectordb_collection = persistent_chroma_client.get_or_create_collection("my_rag_demo_collection", embedding_function=openai_embedding_func_for_chroma)
  # Let's add data to the vector DB; specifically to our collection in the vector DB
  cur_max_id = vectordb_collection.count()
  vectordb_collection.add(ids=[str(t) for t in range(cur_max_id+1, cur_max_id+len(chunks)+1)],
                           documents=[t.page_content for t in chunks],
                           metadatas=[None if len(t.metadata) == 0 else t.metadata for t in chunks]
                           )
  # Alert user that content is loaded and ready to be queried
  gr.Info(f"Website content loaded. Vector DB now has {vectordb_collection.count()} chunks")
  return

# Define the UI and the chat function
with gr.Blocks() as demo:

  # Function to chat with language model using documents for context
  def predict(message, history):
    # Here we are linking to the previously created an instance of the ChromaDB database using a LangChain ChromaDB client
    langchain_chroma = Chroma(client=persistent_chroma_client, collection_name="my_rag_demo_collection", embedding_function=openai_embedding_func_for_langchain)
    # Convert to langchain chat history format - list of tuples rather than list of lists
    langchain_history_format = []
    for human, ai in history:
      langchain_history_format.append((human, ai))
    # We are now defining the ConversationalRetrieval chain, starting with the prompts used
    standalone_question_generator_template = """Given the following conversation and a follow up question, 
    rephrase the follow up question to be a standalone question, in its original language. Be as explicit as possible 
    in formulating the standalone question, and include any context necessary to clarify the standalone question.

    Conversation: {chat_history}
    Follow Up Question: {question}
    Standalone question:"""
    updated_condense_question_prompt = PromptTemplate.from_template(standalone_question_generator_template)
    # Let's rebuild the final prompt (again, optional since LangChain uses a default prompt, though it might be a little different)
    final_response_synthesizer_template = """Use the following pieces of context to answer the question at the end. 
    If you don't know the answer, just say that the available information is not sufficient to answer the question. 
    Don't try to make up an answer. Keep the answer as concise as possible, limited to five sentences.
    {context}
    Question: {question}
    Helpful Answer:"""
    custom_final_prompt = PromptTemplate.from_template(final_response_synthesizer_template)
    # Define the chain
    qa_chain = ConversationalRetrievalChain.from_llm(llm, retriever=langchain_chroma.as_retriever(), 
                                                     return_source_documents=True, return_generated_question=True, 
                                                     condense_question_prompt= updated_condense_question_prompt,
                                                     combine_docs_chain_kwargs={"prompt": custom_final_prompt})
    # Execute the chain
    gpt_response = qa_chain({"question": message, "chat_history": langchain_history_format})
    # Add human message and LLM response to chat history
    langchain_history_format.append((message, gpt_response['answer']))
    return gpt_response['answer'] 

  gr.Markdown(
      """
      # Chat with Websites
      ### Enter URL to extract content from website and start question-answering using this Chatbot
      """
  )
  with gr.Row():
    url_text = gr.Textbox(show_label=False, placeholder='Website URL to load content', scale = 5)
    url_submit = gr.Button(value="Load", scale = 1)
    url_submit.click(fn=load_content_from_url, inputs=url_text)

  with gr.Row():
    gr.ChatInterface(fn=predict)

demo.launch(debug=True)
```

你可以通过给应用提供不同的URL来加载内容进行尝试。无需多言：这不是一个生产级应用，只是用来演示基于RAG的GenAI解决方案的构建块。这充其量是一个早期原型，如果要将其转换为常规产品，大部分的软件工程工作还在前面。

## 重新审视介绍中的常见问题

根据我们创建的聊天机器人背景和知识，让我们重新审视在*介绍*中提出的一些问题，并深入探讨一下。

1.  *我们的用例是否适合LLM驱动的解决方案？也许传统的分析方法、监督学习或其他方法更合适？*

    LLM 在“理解”语言相关任务以及遵循指令方面表现出色。因此，LLM 的早期使用案例包括问答、总结、生成（在这里是文本）、提供更好的基于意义的搜索、情感分析、编码等。LLM 还获得了解决问题和推理的能力。例如，LLM 可以充当学生作业的自动评分员，只要你提供答案键，甚至有时不提供。

    另一方面，基于大量数据点的预测或分类、用于营销优化的多臂赌博机实验、推荐系统、强化学习系统（如 Roomba、Nest 温控器、优化能源消耗或库存水平等）是其他类型分析或机器学习的强项……至少目前是这样。传统 ML 模型向 LLM 提供信息和反向传递信息的混合方法也应考虑作为解决核心业务问题的整体方案。

1.  *如果 LLM 是可行的解决方案，我们的使用案例是否可以通过现成的产品（例如，ChatGPT Enterprise）现在或在不久的将来得到解决？经典的构建与购买决策。*

    OpenAI、AWS 和其他公司提供的服务和产品将变得更加广泛、更好，可能还更便宜。例如，ChatGPT 允许用户上传文件进行分析，Bing Chat 和 Google 的 Bard 让你指向外部网站进行问答，AWS Kendra 将语义搜索引入企业的信息中，Microsoft Copilot 让你在 Word、Powerpoint、Excel 等应用中使用 LLM。正如公司不会自己构建操作系统或数据库一样，公司也应该考虑是否需要构建可能被当前和未来的现成产品所取代的 AI 解决方案。另一方面，如果公司的使用案例非常具体或在某种程度上受限，例如由于敏感性或法规指导不能将敏感数据发送给任何供应商，那么可能需要在公司内部构建生成性 AI 产品以解决这些使用案例。使用 LLM 推理能力但承担的任务或生成的输出与现成解决方案差异太大的产品可能需要内部开发。例如，监控工厂车间、制造过程或库存水平的系统可能需要定制开发，特别是当没有好的领域特定产品时。此外，如果应用需要专业领域知识，那么在领域特定数据上微调的 LLM 可能会优于 OpenAI 的通用 LLM，内部开发也可以考虑。

1.  *我们的 LLM 驱动产品的不同构建模块有哪些？这些模块中哪些已经商品化，哪些可能需要更多时间来构建和测试？*

    像我们构建的RAG解决方案的高级构建块包括数据管道、向量数据库、检索、生成，当然还有LLM。LLM和向量数据库有很多优秀的选择。数据管道、检索、生成的提示工程将需要一些传统的数据科学实验来针对具体用例进行优化。一旦初步解决方案到位，生产化将需要大量工作，这在任何数据科学/机器学习管道中都是真实的。本讲座提供了有关生产化的宝贵经验：[LLMs in Production: Learning from Experience, Dr. Waleed Kadous, Chief Scientist, AnyScale](https://youtu.be/xa7k9MUeIdk?si=LQizYwFt4m-XOYpk)

1.  *我们如何衡量解决方案的性能？有哪些杠杆可以改善我们产品的输出质量？* 与任何技术（或非技术）解决方案一样，商业影响应使用领先的关键绩效指标来衡量。一些直接度量难以测量，通常会被替代指标如每日活跃用户数（DAU）和其他产品指标所取代。

    商业指标应补充技术指标，以评估RAG解决方案的性能。可以使用一系列测试信息量、事实准确性、相关性、毒性等的指标来评估回应的整体质量——系统的回应与专家或如GPT-4（当前）的最先进模型的回应相比如何。这有助于深入了解各个组件的性能，以便迭代和改进每个组件：解决方案将用作上下文的信息质量、检索和生成。

    i. **数据质量**如何？如果组织可用的数据存储在向量数据库中没有所需的信息，则没有人或LLM能够基于这些信息构造回应。

    ii. **检索**效果如何？假设信息可用，系统在找到和提取相关信息方面有多成功？

    iii. **生成（即合成）**效果如何？假设信息可用、检索正确，并传递给LLM生成最终回应，LLM是否按预期使用这些信息？

    每个领域都可以单独评估并同时改进，以提升整体输出。

    **改善数据质量：** 公司需要在数据管道上进行工作，以向系统提供良好的信息。如果向量数据库中的信息质量差，拥有优秀的LLM也不会显著改善输出。除了采用传统的数据质量和治理框架外，公司还应考虑改善分块的质量（下一问题的回应中将更多讨论此事）。

    **改善检索：** 通过尝试不同的检索算法、语义重排序、结合语义搜索和关键词搜索的混合搜索，以及微调嵌入，可以改善检索。改善指令/提示也应有助于提升检索质量。

    **改善生成：** 随着LLM的进步，合成步骤将得到改善，检索可能也会因为嵌入模型的改进而得到提升。另一种选择是进行微调，前提是资源和时间充足，这可以提高特定领域和任务的响应质量。例如，针对特定医疗条件进行微调的小模型可能在任务上优于像GPT-4这样的通用模型，同时也更快、更便宜。

1.  *我们的数据质量是否适合用例？我们是否正确组织了数据，并将相关数据传递给LLM？*

    数据质量可以通过传统的数据质量与治理框架进行评估。此外，对于LLM驱动的解决方案，LLM回答用户问题或执行任务所需的信息应在解决方案可用的数据中存在。

    假设数据是可用的，数据应根据用例和所使用的LLM进行适当的拆分。块不应过于宽泛，以免稀释与特定主题相关的连贯性，也不应过于狭窄，以免遗漏所有必要的上下文。数据不应以将必要的上下文分割在块之间并且在这种分隔下毫无意义的方式进行拆分。例如，如果下面的两个句子被拆分成两个块，

    *“OpenAI的GPT-3.5是一个强大的LLM。它可以支持高达16K令牌的上下文大小。”*

    像“告诉我关于GPT 3.5 LLM的情况”这样的问题可能不会获取第二句，因为它没有提到GPT 3.5，而这条信息可能不会提供给用户，仅仅是由于子优化块的原因。更危险的是，由于上下文大小和令牌与LLM的语义关联，当询问完全不同的LLM时，可能仍会获取该句子，且回答可能是其他重点模型的上下文大小高达16K，这将是不准确的。这是一个在生产环境中不太可能遇到的简化示例，但这个想法是成立的。

    改善内容质量的一种可能方法是使用上下文感知的文本拆分，例如按逻辑部分拆分（如我们书单的示例）。如果任何逻辑块过大——例如，维基百科上关于某些主题的页面可能非常长——它们可以进一步按逻辑部分或按语义单元（如段落）拆分，同时在块之间保持有意义的重叠，并确保将整体元数据和块特定的元数据传递给LLM。

1.  *我们能否确信LLM的回答总是事实准确的？也就是说，我们的解决方案会不会在生成回答时偶尔‘幻觉’？*

    RAG的一个关键卖点是推动事实准确性。GPT 3.5和GPT-4在遵循指令方面表现良好：“仅从提供的上下文中回应，或说‘基于提供的信息无法回答该问题’”。这被推测是由于OpenAI进行了大量的基于人类反馈的强化学习（RLHF）。作为推论，其他LLM可能当前在遵循指令方面表现不佳。对于生产应用，特别是面向外部的应用，进行大量测试以验证生成的输出是否忠实于从向量数据库检索的可用上下文，即使LLM相信这是正确的，也是明智的。方法包括对样本进行手动测试，使用强大的模型如GPT-4测试检索到的上下文样本和其他模型生成的响应，或使用如[Galileo](https://www.rungalileo.io/)这样的服务和产品，专注于实时检测LLM幻觉。

## 结论

如果你在11个月前就知道这些内容，这将值得与你公司首席执行官进行一次演示，甚至可能有一个TED演讲向更广泛的观众介绍。今天，这已成为AI素养的基本要求，特别是如果你参与生成式AI产品的交付。希望通过这次练习，你能比较跟得上！👍

一些结束语，

+   这项技术具有巨大的潜力——还有多少其他技术能“思考”到这种程度，并且能作为“推理引擎”（用安德鲁·吴博士的话说，参见[这里](https://learn.deeplearning.ai/langchain/lesson/7/agents)）？

+   虽然前沿模型（目前为GPT-4）将继续进步，但开源模型及其领域特定和任务特定的微调变体在许多任务中将具有竞争力，并找到许多应用。

+   无论好坏，这项耗资数百万（甚至数亿？）美元开发的前沿技术现在是免费的——你可以填写一个表格，下载Meta功能强大的Llama2模型，拥有非常宽松的许可。HuggingFace的模型中心几乎有30万个基础LLM或其微调变体。硬件也已经商品化。

+   OpenAI模型现在能够识别并使用“工具”（功能、API等），使得解决方案不仅能与人类和数据库接口，还能与其他程序接口。LangChain和其他软件包已经展示了如何将LLM作为“智能体”的“大脑”，这些智能体能够接受输入、决定采取的行动并执行，重复这些步骤直到智能体实现目标。我们的简单聊天机器人在确定性序列中使用了两个LLM调用——生成独立问题，并将搜索结果合成成连贯的自然语言响应。想象一下，数百次对快速发展的LLM进行的调用会取得什么成果！

+   这些快速进展是由于GenAI周围的巨大动力，它将通过我们的设备渗透到企业和日常生活中。最初是以更简单的方式，但随后将在利用技术的推理和决策能力的越来越复杂的应用中体现，与传统AI融合。

+   最终，现在是一个绝佳的时机来参与，因为应用这项技术的竞争环境相对公平——自2022年12月ChatGPT的爆发以来，每个人基本上都在同一时间学习这项技术。当然，研发方面情况有所不同，大型科技公司已经投入了多年和数十亿美元来开发这项技术。尽管如此，为了将来构建更复杂的解决方案，现在正是开始的最佳时机！

## 额外资源

+   **LangChain：** [Deeplearning.ai](http://deeplearning.ai/)课程：[LangChain：与您的数据对话](https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/) | LangChain [文档](https://python.langchain.com/docs/get_started/introduction)

+   **Gradio：** [Deeplearning.ai](https://www.deeplearning.ai/)课程 - [使用Gradio构建生成性AI应用](https://www.deeplearning.ai/short-courses/building-generative-ai-applications-with-gradio/) | Gradio文档和[指南](https://www.gradio.app/guides)

+   我发现[Shawhin Talebi](https://medium.com/u/f3998e1cd186?source=post_page-----6ee6ad94e058--------------------------------)的文章非常具有启发性。请参阅[破解OpenAI (Python) API](/cracking-open-the-openai-python-api-230e4cae7971)，[破解Hugging Face Transformers库](/cracking-open-the-hugging-face-transformers-library-350aa0ef0161)以及其他近期文章。

+   [LLMs在生产中的应用：从经验中学习，AnyScale首席科学家Dr. Waleed Kadous](https://youtu.be/xa7k9MUeIdk?si=LQizYwFt4m-XOYpk)

+   由LlamaIndex的联合创始人Jerry Liu所做的演讲概述了输出评估的各种方法：[构建生产就绪的LLM应用的实用数据考虑](https://youtu.be/xbeFAZl3uCk?si=XBpo6cWt0Z9P9v_w)
