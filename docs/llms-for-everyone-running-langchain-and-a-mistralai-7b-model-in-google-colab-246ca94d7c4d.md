# 每个人的 LLM：在 Google Colab 中运行 LangChain 和 MistralAI 7B 模型

> 原文：[`towardsdatascience.com/llms-for-everyone-running-langchain-and-a-mistralai-7b-model-in-google-colab-246ca94d7c4d`](https://towardsdatascience.com/llms-for-everyone-running-langchain-and-a-mistralai-7b-model-in-google-colab-246ca94d7c4d)

## 免费实验大型语言模型

[](https://dmitryelj.medium.com/?source=post_page-----246ca94d7c4d--------------------------------)![Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----246ca94d7c4d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----246ca94d7c4d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----246ca94d7c4d--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----246ca94d7c4d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----246ca94d7c4d--------------------------------) ·阅读时间 10 分钟·2023 年 12 月 5 日

--

![](img/260367e3c69b2a0784e0677fcb3983bb.png)

LangChain 的艺术表现，照片由 Ruan Richard Rodrigues 提供，[Unsplash](https://unsplash.com/@heeybooy)

大家都知道大型语言模型的定义就是“大”。甚至在不久前，它们仅对高端硬件拥有者开放，或者至少对付费获取云访问或每次 API 调用的人开放。现在，情况正在改变。在这篇文章中，我将展示如何在 Google Colab 中完全免费运行 LangChain Python 库、FAISS 向量数据库和 Mistral-7B 模型，并进行一些有趣的实验。

## 组件

这里有很多关于在 Python 中使用大型语言模型的文章，但它们往往不容易重现。例如，许多使用 LangChain 库的示例使用了一个 [OpenAI](https://python.langchain.com/docs/integrations/llms/openai) 类，其第一个参数（猜猜是什么？）是 OPENAI_API_KEY。一些其他 RAG（检索增强生成）和向量数据库的示例使用了 Weaviate；我们打开其网站后首先看到的是“定价”。在这里，我将使用一组可以完全免费使用的开源库：

+   [LangChain](https://github.com/langchain-ai/langchain)。它是一个用于开发语言模型驱动应用程序的 Python 框架。它也是模型无关的，相同的代码可以与不同的模型重复使用。

+   [FAISS](https://github.com/facebookresearch/faiss)（Facebook AI 相似性搜索）。这是一个用于高效相似性搜索和稠密向量存储的库，我将使用它进行检索增强生成（Retrieval Augmented Generation）。

+   [Mistral 7B](https://mistral.ai/news/announcing-mistral-7b/) 是一个拥有 7.3B 参数的大型语言模型（在 Apache 2.0 许可证下发布），根据作者的说法，它在所有基准测试中超越了 13B Llama2。它也可以在 [HuggingFace](https://huggingface.co/mistralai/Mistral-7B-v0.1) 上使用，因此使用起来非常简单。

+   最后但同样重要的是，[Google Colab](https://colab.research.google.com/)也是这次测试的重要组成部分。它提供了对由 CPU、16 GB NVIDIA Tesla T4 或甚至 80 GB NVIDIA A100（尽管我从未见过免费实例中提供最后一个）的 Python 笔记本的免费访问。

现在，让我们开始吧。

## 安装

首先，我们需要打开 Google Colab 并创建一个新的笔记本。需要的库可以在第一个单元格中使用`pip`安装：

```py
!pip install bitsandbytes accelerate xformers einops langchain faiss-cpu transformers sentence-transformers
```

在运行代码之前，我们需要**选择运行时**类型：

![](img/befe23bd5b3d3bb9ed2a9a7c2edb01d9.png)

Google Colab，作者截图

现在，让我们导入这些库：

```py
from typing import List
import transformers
from transformers import AutoModelForCausalLM, AutoTokenizer, AutoConfig, BitsAndBytesConfig
import torch
from langchain.prompts import PromptTemplate
from langchain.llms import HuggingFacePipeline
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.text_splitter import CharacterTextSplitter
from langchain.chains import ConversationalRetrievalChain, RetrievalQA
from langchain.callbacks.tracers import ConsoleCallbackHandler
from langchain_core.vectorstores import VectorStoreRetriever
from langchain.memory import ConversationBufferMemory
from langchain.vectorstores import FAISS

device = 'cuda' if torch.cuda.is_available() else 'cpu'
print("Device:", device)
if device == 'cuda':
    print(torch.cuda.get_device_name(0))

# >>> Device: cuda
# >>> Tesla T4
```

如果一切正常，输出应显示“cuda”设备和选定的“Tesla T4”显卡。

下一步是最重要和最耗资源的：让我们**加载语言模型**：

```py
orig_model_path = "mistralai/Mistral-7B-Instruct-v0.1"
model_path = "filipealmeida/Mistral-7B-Instruct-v0.1-sharded"
bnb_config = BitsAndBytesConfig(
                                load_in_4bit=True,
                                bnb_4bit_use_double_quant=True,
                                bnb_4bit_quant_type="nf4",
                                bnb_4bit_compute_dtype=torch.bfloat16,
                               )
model = AutoModelForCausalLM.from_pretrained(model_path, trust_remote_code=True, quantization_config=bnb_config, device_map="auto")
tokenizer = AutoTokenizer.from_pretrained(orig_model_path)
```

在这里，我选择了 4 位量化模式，这允许模型适应 GPU 内存。还有另一个棘手的部分。加载原始“mistralai/Mistral-7B-Instruct-v0.1”模型会导致 Colab 实例崩溃。令人惊讶的是，GPU 内存足够用于 4 位量化，但模型文件的大小约为 16 GB，免费 Colab 实例上没有足够的“正常”内存来在将其加载到 GPU 之前对模型进行量化！作为变通方法，我使用了一个“分片”版本，它被拆分成 2GB 的块（如果您的 PC 或 Colab 实例有超过 16GB 的内存，则不需要这样做）。

另外，想了解更多关于 4 位量化如何工作的读者欢迎阅读另一篇文章：

[](/16-8-and-4-bit-floating-point-formats-how-does-it-work-d157a31ef2ef?source=post_page-----246ca94d7c4d--------------------------------) ## 16、8 和 4 位浮点格式——它是如何工作的？

### 让我们深入探讨

towardsdatascience.com

如果一切正常，Colab 的输出应如下所示：

![](img/99178efab22a0b8427cc6b16db98f545.png)

加载 Mistral 7B 模型，作者截图

从图片中可以看出，需要下载的文件非常庞大，因此如果在本地运行此代码（而不是在 Colab 中），请确保您的网络流量没有限制。

现在，让我们**创建 LLM 管道**：

```py
text_generation_pipeline = transformers.pipeline(
    model=model,
    tokenizer=tokenizer,
    task="text-generation",
    eos_token_id=tokenizer.eos_token_id,
    pad_token_id=tokenizer.eos_token_id,
    repetition_penalty=1.1,
    return_full_text=True,
    max_new_tokens=100,
)
mistral_llm = HuggingFacePipeline(pipeline=text_generation_pipeline)
```

恭喜！我们的安装已准备好，我们成功加载了一个 7B 语言模型。为了进行简单测试，让我们看看 LLM 是否有效：

```py
text = "What is Mistral? Write a short answer."
mistral_llm.invoke(text)

#> Mistral is a type of cold front that forms over the Mediterranean 
#> Sea and moves eastward across southern Europe, bringing strong winds 
#> and sometimes severe weather conditions such as heavy rainfall, hail, 
#> and even tornadoes.
```

如果一切正常，我们准备好享受乐趣并进行进一步的测试。

## LangChain

[LangChain](https://github.com/langchain-ai/langchain) 是一个专门为与语言模型配合使用而设计的 Python 框架。作为热身，让我们测试一个**提示模板**：

```py
from langchain.prompts import PromptTemplate

prompt = PromptTemplate.from_template(
    "Tell me a {adjective} joke about {content}."
)
prompt.format(adjective="funny", content="chickens")

llm_chain = prompt | mistral_llm
llm_chain.invoke({"adjective": "funny", "content": "chickens"})

#> Why don't chickens like to tell jokes? They might crack each other
#> up and all their eggs will scramble!
```

有趣的是，LangChain 是“跨平台”的，我们可以在不更改代码的情况下使用不同的语言模型。这个示例取自官方 [库文档](https://python.langchain.com/docs/modules/model_io/prompts/prompt_templates/)，其中 OpenAI 用于提示，但我使用了相同的模板来进行 Mistral。

它是如何工作的？可以将 `ConsoleCallbackHandler` 添加到配置中，这样我们就可以看到所有的中间步骤：

```py
llm_chain = prompt | mistral_llm
llm_chain.invoke({"adjective": "funny", "content": "chickens"},
                 config={'callbacks': [ConsoleCallbackHandler()]})
```

然后，输出将如下所示：

```py
[1:chain:RunnableSequence] Entering Chain run with input:
{
  "adjective": "funny",
  "content": "chickens"
}

[1:chain:RunnableSequence > 2:prompt:PromptTemplate] Entering Prompt run with input:
{
  "adjective": "funny",
  "content": "chickens"
}

[1:chain:RunnableSequence > 3:llm:HuggingFacePipeline] Entering LLM run with input:
{
  "prompts": [
    "Tell me a funny joke about chickens."
  ]
}

[1:chain:RunnableSequence > 3:llm:HuggingFacePipeline] [3.60s] Exiting LLM run with output:
{
  "generations": [
    [
      {
        "text": "\n\nWhy don't chickens like to tell jokes? They might crack each other up and all their eggs will scramble!",
        "generation_info": null,
        "type": "Generation"
      }
    ]
  ],
  "llm_output": null,
  "run": null
}
```

作为另一个示例，让我们尝试 **ChatPromptTemplate** 类：

```py
chat_prompt = ChatPromptTemplate.from_messages(
    [
        ("system", "You are a helpful AI bot. Your name is {name}. Answer with short sentences."),
    ]
)

llm_chain = chat_prompt | mistral_llm
llm_chain.invoke({"name": "Mistral", "user_input": "What is your name?"})

#> Mistral: Yes, I am Mistral. How can I assist you today?
```

在我看来，回答“是的，我是 Mistral”是可以接受的，但从语言学角度来看，对于“你叫什么名字？”这个问题并不是最好的回答。显然，对于大型神经网络来说，可解释性可能是一个问题，我们无法告诉*为什么*模型以这种或那种方式做出回应。这可能是 4 位量化的副作用（这会稍微降低模型质量）或只是 7B 模型能力的根本限制（显然，其他 33B 或 70B 模型可能表现更好，但需要更多计算资源）。

## 检索增强生成（RAG）

现在，RAG 是一个热门研究话题。它允许我们自动将外部文档添加到 LLM 提示中，并在不微调模型的情况下添加更多信息。让我们看看如何将其与 LangChain 和 Mistral 一起使用。

首先，我们需要创建一个单独的**嵌入模型**：

```py
from langchain.embeddings import HuggingFaceEmbeddings

embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-l6-v2",
    model_kwargs={"device": "cuda"},
)
```

这个小型 [sentence-transformer 模型](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) 能够将文本字符串转换为向量表示；我们将用它来构建我们的向量数据库。作为一个玩具示例，我将仅向数组中添加一个“文档”：

```py
db_docs = [
    "Airbus's registered headquarters is located in Leiden, Netherlands.",
]
```

然后我们需要创建一个向量数据库和一个 `VectorStoreRetriever` 对象：

```py
from langchain.vectorstores import FAISS
from langchain_core.vectorstores import VectorStoreRetriever

vector_db = FAISS.from_texts(db_docs, embeddings)
retriever = VectorStoreRetriever(vectorstore=vector_db)
```

现在，我们可以创建一个 [RetrievalQA](https://api.python.langchain.com/en/latest/chains/langchain.chains.retrieval_qa.base.RetrievalQA.html#) 对象，这个对象专门用于问答：

```py
template = """You are a helpful AI assistant. Use the following pieces of context to answer the question at the end.
              {context}
              If you don't know the answer, just say that you don't know, don't try to make up an answer.
              Chat history: {history}
              Question: {question}
              Write your answers short. Helpful Answer:"""

prompt = PromptTemplate(
        template=template, input_variables=["history", "context", "question"]
    )
qa = RetrievalQA.from_chain_type(
        llm=mistral_llm,
        chain_type="stuff",
        retriever=retriever,
        chain_type_kwargs={
            "verbose": False,
            "prompt": prompt,
            "memory": ConversationBufferMemory(
                memory_key="history",
                input_key="question"),
        }
    )
```

我将询问模型有关空客的问题；这些问题模型很可能不知道：

```py
qa.run("Hi, who are you?")
#> I am an AI assistant.

qa.run("What is the range of Airbus A380?")
#> The range of Airbus A380 is approximately 12,497 nautical miles.

qa.run("What is the tire diameter of Airbus A380 in centimeters?")
#> I don't know.
```

我对这些回答感到积极惊讶。首先，Mistral 7B 模型已经知道了空客 A380 的航程（我用 Google 查了一下，结果看起来是正确的）。其次，正如我所预期的，模型并不知道 A380 轮胎的直径，但它“诚实地”回答了“我不知道”，而不是提供“虚构的”错误回答。

现在，让我们向我们的“向量数据库”中添加一个额外的字符串：

```py
db_docs = [
    "Airbus's registered headquarters is located in Leiden, Netherlands.",
    "The Airbus A380 has the largest commercial plane tire size in the world at 56 inches in diameter."
]
```

然后我们可以再试一次：

```py
qa.run("What is the tire diameter of Airbus A380 in centimeters? Write a short answer.")
#> 142 cm
```

这真是太棒了！模型不仅找到了 A380 轮胎直径为 56 英寸的信息，还正确地将其转换为厘米（56*2.54 确实是 142）。我们知道数学任务通常对 LLM 来说很困难，所以这种准确性令人惊讶。

我们还可以要求模型逐步解释答案：

```py
qa.run("What is the tire diameter of Airbus A380 in centimeters? Explain the answer in three steps.")
#> 1\. The tire diameter of Airbus A380 is 56 inches in diameter.
#> 2\. To convert 56 inches to centimeters, we need to multiply it by 2.54 (the conversion factor from inches to centimeters).
#> 3\. Therefore, the tire diameter of Airbus A380 in centimeters is 142.16 cm.
```

这太棒了！好吧，我们已经习惯于大型语言模型如 GPT3 或 GPT4 在云端超级计算机上运行并产生惊人结果的事实。但在你的本地 GPU 上看到这一点（我在 Google Colab 和我的家用 PC 上都测试了这段代码）是完全不同的感觉。

细心的读者可能会问，Mistral 和 Retriever 模型如何协同工作？确实，我创建了一个“Mistral-7B-Instruct-v0.1”模型和一个“all-MiniLM-l6-v2”句子嵌入模型。它们的向量空间是否兼容？答案是“否”。当我们进行查询时，`VectorStoreRetriever` 会先进行自己的搜索，找到向量库中最好的文档，并以纯文本格式返回这些文档。如果我们将 `verbose` 参数设置为 True，就可以看到最终的提示：

```py
template = """You are a helpful AI assistant. Use the following pieces of context to answer the question at the end.
              {context}
              If you don't know the answer, just say that you don't know, don't try to make up an answer.
              Chat history: {history}
              Question: {question}
              Write your answers short. Helpful Answer:"""

prompt = PromptTemplate(
        template=template, input_variables=["history", "context", "question"]
    )
qa = RetrievalQA.from_chain_type(
        llm=mistral_llm,
        chain_type="stuff",
        retriever=retriever,
        chain_type_kwargs={
            "verbose": True,
            "prompt": prompt,
            "memory": ConversationBufferMemory(
                memory_key="history",
                input_key="question"),
        }
    )
```

在运行相同的代码后，我们可以看到 LangChain 发送给模型的实际提示：

```py
You are a helpful AI assistant. Use the following pieces of context to answer the question at the end.
The Airbus A380 has the largest commercial plane tire size in the world at 56 inches in diameter.
Airbus's registered headquarters is located in Leiden, Netherlands.
If you don't know the answer, just say that you don't know, don't try to make up an answer.
Chat history. Human: Hi, who are you?
AI:  I am an AI assistant.
Human: What is the range of Airbus A380?
AI:  The range of Airbus A380 is approximately 12,497 nautical miles.
Question: What is the tire diameter of Airbus A380 in centimeters? Explain the answer in three steps.
Write your answers short. Helpful Answer:
```

在这种情况下，这两个文档都与“空客”问题相关，`VectorStoreRetriever` 将它们放在 `context` 占位符中。

## 结论

在这篇文章中，我们成功在免费的 Google Colab 实例上运行了一个 7.3B 的 Mistral 大型语言模型，完全依赖于免费的开源组件。这是一项伟大的成就，同时也是 Google 的慷慨举措，因为在撰写本文时，Amazon 上最便宜的 16GB 显卡至少需要 $500（不过我必须承认，Google Colab 服务并非纯粹的慈善，免费的 GPU 后端可能并不总是可用；需要经常使用的人应该考虑购买付费订阅）。我们还能够使用检索增强生成来为 LLM 提供额外的信息。如果像这样的模型已经准备好投入生产，是否真的合适仍然是一个悬而未决的问题，也永远是“购买还是自己动手”的难题。Mistral 7B 模型有时仍会“幻觉”并产生错误的答案；它也可能被更大的模型超越。无论如何，能够免费测试这样的模型对于学习、自我教育和原型设计都是极好的。

接下来的部分，我将展示如何在 Google Colab 中运行 LLaMA-13B 模型和 LangChain 框架：

[](/llms-for-everyone-running-the-llama-13b-model-and-langchain-in-google-colab-68d88021cf0b?source=post_page-----246ca94d7c4d--------------------------------) ## 适合所有人的 LLM：在 Google Colab 中运行 LLaMA-13B 模型和 LangChain

### 免费实验大型语言模型（第二部分）

[towardsdatascience.com

对使用语言模型和自然语言处理感兴趣的人也欢迎阅读其他文章：

+   自然语言处理绝对初学者指南

+   16、8 和 4 位浮点格式——如何运作？

+   [Python 数据分析：我们对流行歌曲了解多少？](https://blog.devgenius.io/python-data-analysis-what-do-we-know-about-pop-songs-b6197d85d4)

如果你喜欢这个故事，欢迎[订阅](https://medium.com/@dmitryelj/membership)Medium，你将获得新文章发布的通知，并且可以完全访问其他作者的成千上万篇故事。也欢迎通过[LinkedIn](https://www.linkedin.com/in/dmitrii-eliuseev/)与我联系。如果你想获得此文及其他文章的完整源代码，可以访问我的[Patreon 页面](https://www.patreon.com/deliuseev)。

感谢阅读。
