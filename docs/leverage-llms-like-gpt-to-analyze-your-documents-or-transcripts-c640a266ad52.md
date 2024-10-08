# 利用像 GPT 这样的 LLMs 分析你的文档或记录

> 原文：[`towardsdatascience.com/leverage-llms-like-gpt-to-analyze-your-documents-or-transcripts-c640a266ad52`](https://towardsdatascience.com/leverage-llms-like-gpt-to-analyze-your-documents-or-transcripts-c640a266ad52)

## 使用提示工程，通过 langchain 和 openai 以类似 ChatGPT 的方式分析你的文档

[](https://konstantin-rink.medium.com/?source=post_page-----c640a266ad52--------------------------------)![Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----c640a266ad52--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c640a266ad52--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c640a266ad52--------------------------------) [Konstantin Rink](https://konstantin-rink.medium.com/?source=post_page-----c640a266ad52--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c640a266ad52--------------------------------) ·阅读时间 6 分钟·2023 年 3 月 31 日

--

![](img/7dd6687f2e85da1c40832aaf26002fc7.png)

（原始）照片由[Laura Rivera](https://unsplash.com/@laurar1vera?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/de/fotos/9ZQzrLWV52M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

ChatGPT 无疑是最受欢迎的大型语言模型（LLMs）之一。自 2022 年底发布其测试版以来，大家都可以使用便捷的聊天功能提问或与语言模型互动。

**但如果我们想要向 ChatGPT 提问关于我们自己的文档或我们刚刚听过的播客内容呢？**

本文的目标是向你展示如何利用像 GPT 这样的 LLM（大型语言模型）来分析我们的文档或记录，然后以 ChatGPT 的方式提问并获得有关文档内容的回答。

# 简要说明

+   本文使用了 OpenAI 的 ChatGPT `gpt-3.5-turbo`模型，这需要一个[API 密钥](https://platform.openai.com/account/api-keys)。

+   `langchain`包是一个围绕 LLM 构建的框架，用于加载和处理我们的文档（提示工程）以及与模型进行交互。

+   包含本文全部代码的 colab 笔记本可以在[这里](https://colab.research.google.com/drive/1oG6TXgXJTd8qyCj_ncD0Uy1pGEDMlly3?usp=sharing)找到。

# 前提条件

在编写所有代码之前，我们必须确保所有必要的包已安装、API 密钥已创建且配置已设置。

## API 密钥

要使用 ChatGPT，首先需要创建一个 OpenAI API 密钥。可以通过这个[链接](https://platform.openai.com/account/api-keys)创建密钥，然后点击

`+ 创建新的密钥` 按钮。

> **没有什么是免费的**：一般来说，OpenAI 会对每 1,000 个 tokens 收费。Tokens 是处理文本的结果，可以是单词或字符块。每 1,000 个 tokens 的价格因模型而异（例如，gpt-3.5-turbo 为$0.002 / 1K tokens）。关于定价选项的更多细节可以在[这里](https://openai.com/pricing)找到。
> 
> 好的一点是，OpenAI 允许你免费试用$18，无需提供任何付款信息。你当前的使用概况可以在你的[账户](https://platform.openai.com/account/usage)中查看。

## 安装 OpenAI 包

我们还需要通过运行以下命令来安装官方 OpenAI 包

```py
pip install openai
```

由于 OpenAI 需要（有效的）API 密钥，我们还需要将密钥设置为环境变量：

```py
import os
os.environ["OPENAI_API_KEY"] = "<YOUR-KEY>"
```

## 安装 langchain 包

随着 2022 年末对大型语言模型（LLMs）的兴趣急剧上升（Chat-GPT 发布），一个名为 LangChain 的包在[同一时间出现](https://github.com/hwchase17/langchain/graphs/commit-activity)。

[LangChain](https://github.com/hwchase17/langchain)是一个围绕 LLM（如 ChatGPT）构建的框架。该包的目标是帮助开发将 LLM 与其他计算或知识来源相结合的应用程序。它涵盖了如*特定文档的问答*（**本文的目标**）*、聊天机器人*和*代理*等应用领域。更多信息可以在[文档](https://langchain.readthedocs.io/en/latest/)中找到。

可以使用以下命令安装该包：

```py
pip install langchain
```

## Prompt Engineering

你可能会想知道*Prompt Engineering*是什么。可以通过创建一个在你希望分析的文档上训练的自定义模型来[微调](https://platform.openai.com/docs/guides/fine-tuning) GPT-3。然而，除了训练费用外，我们还需要大量的高质量示例，理想情况下由人工专家审核（根据[文档](https://platform.openai.com/docs/guides/fine-tuning/general-best-practices)）。

这对于仅仅分析我们的文档或抄本来说会显得过于繁琐。因此，我们将我们希望分析的文本（通常称为提示）传递给它，而不是训练或微调模型。生产或创建如此高质量的提示称为*Prompt Engineering*。

> **注意**：有关 Prompt Engineering 的进一步阅读文章可以在[这里](https://docs.cohere.ai/docs/prompt-engineering)找到。

# 加载数据

根据你的使用案例，`langchain`为你提供了几个“[加载器](https://python.langchain.com/en/latest/modules/indexes/document_loaders.html)”，如`Facebook Chat`、`PDF`或`DirectoryLoader`，用于加载或读取你的（非结构化）文本（文件）。该包还包括一个`YoutubeLoader`用于转录 YouTube 视频。

以下示例集中在`DirectoryLoader`和`YoutubeLoader`上。

## 使用 DirectoryLoader 读取文本文件

```py
from langchain.document_loaders import DirectoryLoader

loader = DirectoryLoader("", glob="*.txt")
docs = loader.load_and_split()
```

`DirectoryLoader` 的第一个参数是**路径**，第二个参数是**模式**，用以查找我们所需要的文档或文档类型。在我们的案例中，我们将加载与脚本位于同一目录下的所有文本文件（*.txt*）。`load_and_split` 函数随后启动加载过程。

> 即使我们可能只加载一个文本文档，但如果文件较大，进行拆分也是有意义的，以避免`NotEnoughElementsException`（需要至少四个文档）。更多信息可以在[这里](https://github.com/hwchase17/langchain/issues/1793#issuecomment-1480765690)找到。

## 使用 YoutubeLoader 转录 YouTube 视频

LangChain 提供了一个 YoutubeLoader 模块，该模块使用 `youtube_transcript_api` [包](https://pypi.org/project/youtube-transcript-api/)。这个模块会收集给定视频的（生成的）字幕。

> 并非每个视频都自带字幕。在这些情况下，可以使用自动生成的字幕。然而，在某些情况下，这些字幕质量较差。在这些情况下，使用 Whisper 来转录音频文件可能是一个替代方案。

下面的代码将**视频 ID**和**语言**（默认为 en）作为参数。

```py
from langchain.document_loaders import YoutubeLoader

loader = YoutubeLoader(video_id="XYZ", language="en")
docs = loader.load_and_split()
```

## 在我们继续之前……

如果你决定使用**转录的 YouTube 视频**，请考虑**首先进行适当的清理**，例如，清除 Latin1 字符（*\xa0*）。我在*问答*部分体验到的答案差异取决于我使用的相同来源的格式。

# 处理数据

像 GPT 这样的 LLMs 只能处理一定数量的[令牌](https://platform.openai.com/docs/models/gpt-3-5)。这些限制在处理大型（或更大）文档时尤为重要。通常，有三种方法来应对这些限制。一种是利用嵌入或`vector space engine`。第二种方法是尝试不同的链式方法，如`map-reduce`或`refine`。第三种方法是两者的结合。

> 一篇提供有关不同链式方法和向量空间引擎使用的更多细节的精彩文章可以在[这里](https://dagster.io/blog/chatgpt-langchain)找到。还要记住：使用的令牌越多，收费也会越高。

在接下来的内容中，我们将`embeddings`与链式方法`stuff`结合起来，`stuff`会将所有文档“塞入”一个单一的提示中。

首先，我们通过使用`OpenAIEmbeddings`将转录文本（`docs`）导入向量空间。然后，这些嵌入被存储在一个称为[Chroma](https://github.com/chroma-core/chroma)的内存嵌入数据库中。

```py
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma

embeddings = OpenAIEmbeddings()
docsearch = Chroma.from_documents(docs, embeddings)
```

之后，我们定义我们希望用于分析数据的**model_name**。在这种情况下，我们选择`gpt-3.5-turbo`。可以在[这里](https://platform.openai.com/docs/models/gpt-3-5)找到可用模型的完整列表。**temperature**参数定义了采样温度。较高的值会导致输出更随机，而较低的值则会使答案更加集中和确定。

最后但同样重要的是，我们使用了`RetrievalQA` (**Q**uestion/**A**nswer) [检索器](https://blog.langchain.dev/retrieval/)并设置了相应的参数（`llm`，`chain_type`，`retriever`）。

```py
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI

llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0.2)

qa = RetrievalQA.from_chain_type(llm=llm, 
                                    chain_type="stuff",
                                    retriever=docsearch.as_retriever())
```

# 提问

现在我们准备向模型提问关于我们的文档。下面的代码显示了如何定义查询。

```py
query = "What are the three most important points in the text?"
qa.run(query)
```

## 对于不完整的回答该怎么办？

在某些情况下，你可能会遇到不完整的回答。回答文本在几句话后就停止了。

不完整回答的原因很可能是令牌限制。如果提供的提示非常长，模型剩余的令牌就不够给出一个（完整的）回答。处理此问题的一种方法是切换到不同的**chain-type**，如`refine`。

```py
llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0.2)

qa = RetrievalQA.from_chain_type(llm=llm, 
                                    chain_type="refine",
                                    retriever=docsearch.as_retriever())
```

然而，我发现当使用与*stuff*不同的`chain_type`时，我得到的结果不够具体。处理这些问题的另一种方法是重新表述问题，使其更加具体。

# 结论

多亏了 LangChain 包，只需要几行代码就可以分析 GPT 文本文档或转录内容。由于这个包相对较新，我预计很快会有很多更新和代码更改。这可能会影响本文中提供的代码片段。

如果你考虑在日常工作或较大的私人项目中使用 LLM，你应该关注数据的清理，优化使用的令牌数量，并使用最佳实践，如设置预算限制或警报。

我希望你喜欢阅读这篇文章。源代码的 Colab 笔记本可以在 [这里](https://colab.research.google.com/drive/1oG6TXgXJTd8qyCj_ncD0Uy1pGEDMlly3?usp=sharing) 找到。

# 来源

[](https://www.pinecone.io/learn/langchain-intro/?source=post_page-----c640a266ad52--------------------------------) [## LangChain：简介和入门 | Pinecone

### 大型语言模型（LLMs）在 2020 年 OpenAI 发布 GPT-3 后进入了世界舞台 [1]。从那时起…

[www.pinecone.io](https://www.pinecone.io/learn/langchain-intro/?source=post_page-----c640a266ad52--------------------------------) [](https://dagster.io/blog/chatgpt-langchain?source=post_page-----c640a266ad52--------------------------------) [## 使用 GPT3、LangChain 和 Python 构建 GitHub 支持机器人 | Dagster 博客

### 2023 年 1 月 9 日 * 13 分钟阅读 * ChatGPT，你听说过吗？ChatGPT 几个月前推出，震惊了所有人……

[dagster.io](https://dagster.io/blog/chatgpt-langchain?source=post_page-----c640a266ad52--------------------------------) [## 提示工程

### 在这里，我们讨论了一些编写提示（我们模型的输入）的原则和技巧，这将帮助你获得更好的结果……

[docs.cohere.ai](https://docs.cohere.ai/docs/prompt-engineering?source=post_page-----c640a266ad52--------------------------------) [](https://blog.langchain.dev/retrieval/?source=post_page-----c640a266ad52--------------------------------) [## 检索

### TL;DR：我们正在调整我们的抽象，以便除了 LangChain VectorDB 之外的其他检索方法也能轻松使用……

[blog.langchain.dev](https://blog.langchain.dev/retrieval/?source=post_page-----c640a266ad52--------------------------------)
