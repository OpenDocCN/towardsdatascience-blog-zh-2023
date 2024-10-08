# 用 RAG 增强 LLM

> 原文：[`towardsdatascience.com/augmenting-llms-with-rag-f79de914e672`](https://towardsdatascience.com/augmenting-llms-with-rag-f79de914e672)

## 查看一个大型语言模型（LLM）模型如何回答 Amazon SageMaker 相关问题的端到端示例

[](https://ram-vegiraju.medium.com/?source=post_page-----f79de914e672--------------------------------)![Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----f79de914e672--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f79de914e672--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f79de914e672--------------------------------) [Ram Vegiraju](https://ram-vegiraju.medium.com/?source=post_page-----f79de914e672--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f79de914e672--------------------------------) ·9 分钟阅读·2023 年 10 月 10 日

--

![](img/553cc94765b38c8f5437dbc15e3856a6.png)

图片来源 [Unsplash](https://unsplash.com/photos/lUSFeh77gcs)

我在 Medium 上写了不少关于不同技术主题的博客，特别是关于 [在 Amazon SageMaker 上托管机器学习（ML）模型](https://ram-vegiraju.medium.com/list/amazon-sagemaker-f1b06f720fba)。最近，我也对日益增长的生成式 AI/大型语言模型生态系统产生了兴趣（就像行业中的其他人一样，哈哈）。

这两个不同的领域让我产生了一个有趣的问题。**我的 Medium 文章在教授 Amazon SageMaker 方面表现如何？** 为了回答这个问题，我决定实现一个利用 [检索增强生成（RAG）](https://docs.aws.amazon.com/sagemaker/latest/dg/jumpstart-foundation-models-customize-rag.html) 的生成式 AI 解决方案，并访问我的一些文章，以查看它能够多好地回答一些与 SageMaker 相关的问题。

在这篇文章中，我们将构建一个端到端的生成式 AI 解决方案，并利用一些流行的工具来实现这个工作流：

+   [**LangChain**](https://www.langchain.com/): LangChain 是一个流行的 Python 框架，通过提供现成的模块来帮助简化生成式 AI 应用程序，这些模块有助于提示工程、RAG 实现和 LLM 工作流编排。

+   [**OpenAI**](https://openai.com/): LangChain 将负责我们生成式 AI 应用程序的编排，但大脑仍然是模型。在这种情况下，我们使用了 OpenAI 提供的 LLM，但 LangChain 也可以与不同的模型源集成，如 SageMaker 端点、Cohere 等。

**注意**：本文假设读者具有中级 Python 了解和对 LangChain 的基本了解。我建议参考这篇 文章 来更好地理解 LangChain 和构建生成性 AI 应用。

**免责声明**：我在 AWS 担任机器学习架构师，我的观点仅代表我个人。

# 问题概述

大型语言模型（LLMs）本身非常强大，通常可以在不依赖微调或额外知识/上下文的情况下回答许多问题。

然而，当你需要访问其他特定的数据源，尤其是最新数据时，这可能会成为瓶颈。例如，虽然 OpenAI 已经在大量数据语料库上进行了训练，但它并不知道我最近在 Medium 上写的文章。

在这种情况下，我们想要检查我的 Medium 文章在回答关于 Amazon SageMaker 的问题方面能提供多大的帮助。OpenAI 的模型已经从它们训练过的语料库中获得了一些关于 Amazon SageMaker 的知识。我们想要看到的是，通过让这些大型语言模型（LLMs）访问我的 Medium 文章，我们能提升多少性能。这些文章几乎可以作为一个为已经拥有大量知识库的 LLMs 提供的速查表。

我们如何为这些 LLMs 提供访问这些额外知识和信息的途径？

# 为什么我们需要 RAG

这就是 [检索增强生成（RAG）](https://aws.amazon.com/blogs/machine-learning/question-answering-using-retrieval-augmented-generation-with-foundation-models-in-amazon-sagemaker-jumpstart/) 发挥作用的地方。通过 RAG，我们提供了一个信息检索系统，使我们能够访问所需的额外数据。这将帮助我们回答更高级的 SageMaker 问题，并增强我们的 LLM 知识库。要实现一个基本的 RAG 系统，我们需要几个组件：

+   [**嵌入模型**](https://medium.com/@ryanntk/choosing-the-right-embedding-model-a-guide-for-llm-applications-7a60180d28e3)：对于我们提供访问的数据，这些数据不能仅仅是一些文本或图像，而是需要以数值/向量格式进行捕捉，以便所有自然语言处理模型（包括 LLMs）能够理解。为了转换我们的数据，我们利用了 [OpenAI 嵌入模型](https://platform.openai.com/docs/guides/embeddings)，但也有多种不同的选择，如 Cohere、Amazon Titan 等，你可以评估它们的性能。

+   [**向量存储**](https://aws.amazon.com/blogs/database/the-role-of-vector-datastores-in-generative-ai-applications/)：一旦我们拥有了嵌入，我们需要利用一个向量数据存储，不仅存储这些向量，还提供一种高效的方式来索引和检索相关数据。当用户有查询时，我们希望返回包含与该输入相似性的任何相关上下文。这些向量存储大多数由 KNN 和其他最近邻算法提供支持，以为初始问题提供相关上下文。在这个解决方案中，我们使用了 Facebook 的 [FAISS](https://github.com/facebookresearch/faiss) 库，该库可用于高效的相似性搜索和向量聚类。

+   **LLM 模型**：在这种情况下，我们有两个模型，一个是用于创建嵌入的嵌入模型，但我们仍然需要主要的 LLM，它接收这些嵌入和用户输入以返回输出。在这种情况下，我们也使用默认的 [ChatOpenAI 模型](https://platform.openai.com/docs/guides/gpt)。

![](img/52f11376cdf9d37a40cb69cc03dc207c.png)

RAG 流程（作者创建）

从本质上讲，你可以将 RAG 视为 LLM 的性能增强器，通过提供基础 LLM 可能尚未拥有的额外知识。在下一部分中，我们将探讨如何利用 LangChain 和 OpenAI 实现这些概念。

# 生成式 AI 应用与示例推理

要开始，你需要一个 OpenAI API 密钥，你可以在以下 [链接](https://platform.openai.com/docs/api-reference) 找到并安装。注意费用和 API 限制，以便了解定价结构。对于开发，我们在 [SageMaker Classic Notebook 实例](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) 中工作，但任何安装了 OpenAI 和 LangChain 的环境都应足够。

```py
import os
os.environ['OPENAI_API_KEY'] = 'Enter your API Key here'

# necessary langchain imports
import langchain
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.embeddings.cache import CacheBackedEmbeddings
from langchain.vectorstores import FAISS
from langchain.storage import LocalFileStore
from langchain.document_loaders import PyPDFDirectoryLoader
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI
```

设置 LangChain 和 OpenAI 后，我们创建一个本地目录，存储十篇我的热门 Medium 文章作为 PDF。这将是我们为 LLM 提供的额外数据/信息。

![](img/6236ef526322058a90cadb9927e65e01.png)

Medium 文章（作者截图）

下一步，我们需要能够加载这些数据，并创建一个目录以存储我们生成的嵌入。LangChain 提供了许多工具，可以自动加载并拆分/切块你的数据。 [切块](https://www.pinecone.io/learn/chunking-strategies/) 尤其重要，因为我们不希望生成的嵌入数据集过大。数据越大，可能引入的噪声也越多。

在这种情况下，我们使用 LangChain 提供的 [PDF 加载器](https://python.langchain.com/docs/modules/data_connection/document_loaders/pdf) 来加载和拆分我们的数据。

```py
# where our embeddings will be stored
store = LocalFileStore("./cache/")

# instantiate a loader: this loads our data, use PDF in this case
loader = PyPDFDirectoryLoader("sagemaker-articles/")

# by default the PDF loader both loads and splits the documents for us
pages = loader.load_and_split()
print(len(pages))
```

然后，我们实例化我们的 OpenAI 嵌入模型。我们使用嵌入模型创建我们的嵌入，并填充我们创建的本地缓存目录。

```py
# instantiate embedding model
embeddings_model = OpenAIEmbeddings()

embedder = CacheBackedEmbeddings.from_bytes_store(
    embeddings_model,
    store
)
```

![](img/40cb3e81281f6644ffa9e9b5ee5447c0.png)

嵌入生成（作者截图）

然后我们创建了 FAISS 向量存储并推送了嵌入的文档。

```py
# create vector store, we use FAISS in this case
vector_store = FAISS.from_documents(pages, embedder)
```

然后我们使用[检索问答链](https://python.langchain.com/docs/use_cases/question_answering/how_to/vector_db_qa)来将这些移动部件汇聚在一起。我们指定了上面创建的向量存储，并将 ChatOpenAI 默认 LLM 作为模型，该模型将接收输入和相关文档以获取上下文。

```py
# this is the entire retrieval system
medium_qa_chain = RetrievalQA.from_chain_type(
    llm=ChatOpenAI(),
    retriever=vector_store.as_retriever(),
    return_source_documents=True,
    verbose=True
)
```

然后我们可以通过传入相同的提示并观察结果，比较模型在没有 RAG 的情况下与基于 RAG 的链的性能。让我们运行一系列不同难度的示例提示。

```py
sample_prompts = ["What does Ram Vegiraju write about?",
                 "What is Amazon SageMaker?",
                 "What is Amazon SageMaker Inference?",
                 "What are the different hosting options for Amazon SageMaker?",
                 "What is Serverless Inference with Amazon SageMaker?",
                 "What's the difference between Multi-Model Endpoints and Multi-Container Endpoints?",
                 "What SDKs can I use to work with Amazon SageMaker?"]

for prompt in sample_prompts:

    #vanilla OpenAI Response
    response = openai.Completion.create(
        engine="text-davinci-003",
        prompt=prompt,
        max_tokens = 500)

    # RAG Augmented Response
    response_rag = medium_qa_chain({"query":prompt})
```

我们可以看到第一个问题本身非常具体于我的写作。我们知道 OpenAI 模型没有访问或了解我的文章，因此它给出的描述非常随机且不准确。

![](img/877944bd4385e258faa94879d793f6b7.png)

OpenAI 回应（作者截图）

另一方面，我们的 RAG 链接访问了一些我的 Medium 文章，并生成了对我写作内容的相当准确的总结。

![](img/cf7aeab9513869d39110eec827a8a987.png)

RAG 回应（作者截图）

我们可以通过提出一些 SageMaker 相关的问题来测试这两种方法。我们从一个非常基本的问题开始：什么是 Amazon SageMaker？由于 OpenAI LLM 对此有了解，它会给出一个相当准确并且可以与我们基于 RAG 的方法相媲美的回答。

![](img/7afbc2d5452eb56315db8d8e37ce6e14.png)

OpenAI 与 RAG 回应（作者截图）

当问题开始变得更加具体和困难时，我们开始看到 RAG 的真正好处。一个例子是比较两种高级托管选项的提示：[多模型端点 (MME)](https://docs.aws.amazon.com/sagemaker/latest/dg/multi-model-endpoints.html) 和 [多容器端点 (MCE)](https://aws.amazon.com/blogs/machine-learning/part-5-model-hosting-patterns-in-amazon-sagemaker-cost-efficient-ml-inference-with-multi-framework-models-on-amazon-sagemaker/)。

![](img/30fcd243656d7712a3246e49bb5f1451.png)

MME 与 MCE（作者截图）

在这里我们看到，Vanilla OpenAI 的回答给出了一个完全不准确的答案，它对这两个最近的功能没有了解。然而，我的关于 MCE 与 MME 的具体 Medium 文章为模型提供了这些功能的背景，因此它能够准确回答查询。

借助 RAG，我们可以扩展我们 LLM 已经掌握的关于 SageMaker 的基础知识。在下一部分，我们可以查看不同的方法来改进我们所构建的原型。

# 我们如何提高性能？

虽然这是一个很好的解决方案，但还有很多改进空间以便于扩展。你可以用来改进基于 RAG 的性能的一些潜在方法包括以下几点：

+   **数据大小和质量**：在这个案例中，我们仅提供了十篇 Medium 文章，仍然看到了良好的性能。为了提升效果，我们还可以提供我所有 Medium 文章的访问权限，或者任何带有“ SageMaker”标签的内容。我们还直接复制了我的文章而没有任何格式化，PDF 本身也非常无结构，清理数据格式可以帮助更好地分块，从而优化性能。**注意：** 使用的数据也必须仅依赖于你被允许用于目的的资源/文章。在这个例子中，以我的 Medium 文章作为来源没有问题，但始终确保你以授权的方式使用数据。

+   **向量存储优化**：在这个案例中，我们使用了默认的 FAISS 向量存储设置。你可以调整的项包括向量存储索引的速度，以及检索和提供给 LLM 的文档数量。

+   **微调与 RAG**：虽然 RAG 有助于获得领域特定的知识，但微调也是帮助 LLM 获得特定知识集的另一种方法。你需要在这里评估你的使用案例，以确定是微调更有意义，还是两者的结合。一般来说，如果你有优质的数据，微调表现非常出色。在这种情况下，我们甚至没有对数据进行格式化或调整，但仍能取得良好的结果。对于微调，数据的可用性和质量至关重要。有关这两种选项的详细分析，请参阅以下文章。

# 附加资源与结论

[](https://github.com/RamVegiraju/LangChain-Samples/tree/master/Medium-SageMaker-Analyzer?source=post_page-----f79de914e672--------------------------------) [## LangChain-Samples/Medium-SageMaker-Analyzer 在 master · RamVegiraju/LangChain-Samples]

### 示例展示了如何将 LangChain 与 GenAI 集成。通过创建一个…贡献于 RamVegiraju/LangChain-Samples 的开发。

github.com](https://github.com/RamVegiraju/LangChain-Samples/tree/master/Medium-SageMaker-Analyzer?source=post_page-----f79de914e672--------------------------------)

整个示例的代码可以在上述链接中找到。这是一个有趣的项目，用于评估我的文章的价值，同时展示如何将 RAG 集成到你的生成式 AI 应用程序中。在接下来的文章中，我们将继续探索更多生成式 AI 和 LLM 驱动的功能。

一如既往，感谢你的阅读，欢迎随时留下反馈。

*如果你喜欢这篇文章，请随时通过* [*LinkedIn*](https://www.linkedin.com/in/ram-vegiraju-81272b162/) *与我联系，并订阅我的 Medium* [*新闻通讯*](https://ram-vegiraju.medium.com/subscribe)*。*
