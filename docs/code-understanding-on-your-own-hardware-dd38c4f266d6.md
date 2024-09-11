# 在你的本地硬件上理解代码

> 原文：[https://towardsdatascience.com/code-understanding-on-your-own-hardware-dd38c4f266d6?source=collection_archive---------8-----------------------#2023-07-05](https://towardsdatascience.com/code-understanding-on-your-own-hardware-dd38c4f266d6?source=collection_archive---------8-----------------------#2023-07-05)

## 设置一个LLM来讨论你的代码——使用LangChain和本地硬件

[](https://medium.com/@doriandrost?source=post_page-----dd38c4f266d6--------------------------------)[![Dorian Drost](../Images/1795395ad0586eafd83d3e2f7b975ca8.png)](https://medium.com/@doriandrost?source=post_page-----dd38c4f266d6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dd38c4f266d6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dd38c4f266d6--------------------------------) [Dorian Drost](https://medium.com/@doriandrost?source=post_page-----dd38c4f266d6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1d49ea537d1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcode-understanding-on-your-own-hardware-dd38c4f266d6&user=Dorian+Drost&userId=1d49ea537d1c&source=post_page-1d49ea537d1c----dd38c4f266d6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dd38c4f266d6--------------------------------) ·7分钟阅读·2023年7月5日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdd38c4f266d6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcode-understanding-on-your-own-hardware-dd38c4f266d6&source=-----dd38c4f266d6---------------------bookmark_footer-----------)![](../Images/14ea7eeae6ca02ac579d218a93a066c4.png)

我保证你的代码不会离开你的本地硬件。照片由[Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

在今天，大型语言模型（LLMs）能够执行的各种任务中，代码理解可能对你尤其感兴趣，如果你是一名软件开发者或数据科学家。拥有一个你可以询问代码问题的聊天机器人不是很好吗？*数据预处理在哪里实现的？* *是否已经有验证用户身份的函数？calculate_vector_dim 和 calculate_vector_dimension 函数之间有什么区别？* 你不必自己搜索正确的文件，只需问机器人，它会给你答案，并指向包含相关代码片段的文件。这种机制叫做语义搜索，你可以想象它的实用性。

在这个教程中，我将展示如何实现一个完全符合要求的 LangChain 机器人。此外，我将关注具体的数据隐私问题，即不把你的代码交出去。你或你的公司生产的代码是私有财产，可能包含敏感信息或宝贵的知识。你可能不希望，或者公司政策可能不允许你将其发送到另一个公司托管的 LLM，那个公司可能位于外国。因此，在本教程中，我将展示如何设置一个运行在本地硬件上的代码理解机器人，以便你的代码不会离开你的基础设施。

我们现在就开始吧！首先，我会给你简要介绍一下语义搜索的一般过程，然后我们再实现一个用于代码理解的机器人。

## 语义搜索简介

![](../Images/d2a2495d247f2a0c813df9e0d7df3d07.png)

在语义搜索中，关键是找到相关的文档。照片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

首先，让我简要说明一下语义搜索的一般思路。这种方法包括两个主要步骤：检索和 LLM 本身生成答案。在检索步骤中，选择包含相关信息的文档，然后将这些文档输入 LLM 以生成自然语言答案。例如，如果你问一个关于名为*transform_vectors*的函数的问题，检索步骤会选择那些与回答该问题相关的文件。这可能包括实现*transform_vectors*函数的文件，也包括使用它的文件或提及它的文档部分。在第二步，这些文件的内容会被作为提示提供给 LLM，提示可能如下所示：

```py
"""Answer the question below given the context. 
<document 1>
<document 2>
...
<document n>

Question: <user question>
Answer:
"""
```

LLM 使用来自提供的文档的信息生成自然语言答案。

这就是语义搜索的主要思想。现在我们开始实现吧！首先，我们需要安装我们的要求并读取数据。

## 安装要求

在我们开始之前，请确保你已经设置了运行 Python 的环境，并安装了以下软件包：

```py
pip install langchain==0.0.191
pip install transformers
```

## 读取文档

现在我们需要读取数据并将其转换为 LangChain 可以使用的格式。在这个演示中，我将下载 LangChain 本身的代码，但你当然可以使用你自己的代码库：

```py
import os

folder_name = "sample_code"
os.system(f"git clone https://github.com/hwchase17/langchain {folder_name}")
```

我们加载所有文件并将其转换为一个 *Document*，即每个 *Document* 将包含代码库中的一个文件。

```py
from langchain.docstore.document import Document

documents = []
for root, dirs, files in os.walk(folder_name):
    for file in files:
        try:
            with open(os.path.join(root, file), "r", encoding="utf-8") as o:
                code = o.readlines()
                d = Document(page_content="\n".join(code), metadata={"source": os.path.join(root, file)})
                documents.append(d)
        except UnicodeDecodeError:
            # some files are not utf-8 encoded; let's ignore them for now.
            pass
```

## 检索

![](../Images/b141a8ee6f486c209b660f19068f37ec.png)

哪些与回答我们的提问相关？这是检索的任务来决定的。照片由 [Ed Robertson](https://unsplash.com/@eddrobertson?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

现在我们已经创建了 *Documents*，我们需要对其进行索引以使其可搜索。对 *Document* 进行索引意味着计算一个数值向量，该向量捕捉 *Document* 中最相关的信息。与纯文本不同，数字向量可以用于进行数值计算，这意味着我们可以轻松计算相似度，然后用来确定哪些 *Documents* 对回答给定问题是相关的。

从技术层面来看，我们将利用嵌入创建的索引，并将其存储在 *VectorStore* 中。已有一些作为服务提供的 *VectorStores*（例如 [DeepLake](https://www.deeplake.ai)），这些服务有一些方便的优势，但在我们的场景中，我们不希望将代码交给他人，所以我们在本地机器上创建一个 *VectorStore*。最简单的方法是使用 *Chroma*，它在内存中创建一个 *VectorStore* 并允许我们持久化它。

```py
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.vectorstores import Chroma

hfemb = HuggingFaceEmbeddings(model_name="krlvi/sentence-t5-base-nlpl-code-x-glue")
persist_directory = "db"
db = Chroma.from_documents(documents, hfemb, persist_directory=persist_directory)
db.persist()
```

在 *from_documents* 函数中，索引被计算并存储在 *Chroma* 数据库中。下次，我们可以加载持久化的 *Chroma* 数据库，而不是再次调用 *from_documents* 函数：

```py
db = Chroma(persist_directory=persist_directory, embedding_function=hfemb)
```

正如你在上面看到的，作为一个嵌入，我使用了[*krlvi/sentence-t5-base-nlpl-code-x-glue*](https://huggingface.co/krlvi/sentence-t5-base-nlpl-code_search_net)，这是一个在开源 GitHub 库的代码上训练的嵌入。可以想象，我们使用的嵌入必须在代码（以及其他数据）上进行过训练，以便能够利用我们提供的数据。仅在自然语言上训练的嵌入，表现可能会较差。

现在我们有了 *VectorStore* 和嵌入，我们可以直接从 *Chroma* 数据库创建检索器：

```py
retriever = db.as_retriever()
```

## LLM

![](../Images/05fb0674b4aaeda3e15588bc43d0967e.png)

LLM 必须对文档进行推理，并给出用户问题的答案。照片由 [Tingey Injury Law Firm](https://unsplash.com/@tingeyinjurylawfirm?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我们需要的最后一个组件是一个LLM。最简单的解决方案是使用托管的LLM，例如通过使用OpenAI接口。然而，我们不想将我们的代码发送到这样的托管服务。相反，我们将在自己的硬件上运行LLM。为此，我们使用[HuggingFacePipeline](https://python.langchain.com/docs/modules/model_io/models/llms/integrations/huggingface_pipelines)，它允许我们在LangChain框架中使用来自HuggingFace的模型。

```py
from langchain import HuggingFacePipeline
import transformers

model_id = "mosaicml/mpt-7b-instruct"
config = transformers.AutoConfig.from_pretrained(model_id,trust_remote_code=True)
tokenizer = transformers.AutoTokenizer.from_pretrained(model_id)
model = transformers.AutoModelForCausalLM.from_pretrained(model_id, config=config, trust_remote_code=True)
pipe = transformers.pipeline("text-generation", model=model, tokenizer=tokenizer, max_new_tokens=100)
llm = HuggingFacePipeline(pipeline=pipe)
```

正如你所见，我使用了[*mosaic mpt-7b*](https://huggingface.co/mosaicml/mpt-7b)模型，它只需要~16GB的GPU内存。我创建了一个*AutoModelForCausalLM*，它被传递到*transformers.pipeline*中，最终被转换成一个*HuggingFacePipeline*。*HuggingFacePipeline*实现了与LangChain中典型LLM对象相同的接口。也就是说，你可以像使用OpenAI LLM接口一样使用它。

如果你的机器上有多个GPU，你需要指定使用哪个。在这种情况下，我想使用索引为0的GPU：

```py
config.init_device="cuda:0"
model.to(device='cuda:0')
pipe = transformers.pipeline("text-generation", model=model, tokenizer=tokenizer, max_new_tokens=100, device=0)
```

我设置的一些额外参数可以解释如下：

+   *trust_remote_code*：这必须设置为true，以允许运行来自LangChain之外的模型。

+   *max_new_tokens*：这定义了模型在回答中可能生成的最大token数量。如果这个值太低，模型的回答可能会在完全回答问题之前被截断。

## 将所有内容连接起来

![](../Images/a1a78263500282ac4514caf874b3f3c6.png)

我们拥有所需的所有组件。我们只需将它们连接起来。照片由[John Barkiple](https://unsplash.com/@barkiple?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

现在我们拥有了所有需要的组件，并可以将它们组合成一个*ConversationalRetrievalChain*。

```py
from langchain.chains import ConversationalRetrievalChain

qa_chain = ConversationalRetrievalChain.from_llm(llm=llm, retriever=retriever, return_source_documents=True)
```

最终，我们可以查询链以回答我们的问题。结果对象将包括一个自然语言答案和一个*source_documents*的列表，这些文档被查阅以得出答案。

```py
result = qa_chain({"question":"What is the return type of the create_index function in the KNNRetriever?", "chat_history":[]})
print(f"Answer: {result['answer']}")
print(f"Sources: {[x.metadata['source'] for x in result['source_documents']]}")
```

这是答案：

```py
Answer:  The return type of the create_index function in the KNNRetriever is np.ndarray.
Sources: ['sample_code/langchain/retrievers/knn.py', 'sample_code/langchain/vectorstores/elastic_vector_search.py', 'sample_code/langchain/vectorstores/elastic_vector_search.py', 'sample_code/langchain/vectorstores/opensearch_vector_search.py']
```

# 总结

完成了！嗯，差不多。通过上述代码，我们现在可以提出关于源代码的问题。然而，根据你的需求，可能需要更改一些步骤。

+   使用你自己的源代码作为*Documents*，而不是LangChain的代码。

+   尝试不同的嵌入。如果嵌入不合适，检索器可能无法找到正确的文档，最终问题可能无法准确回答。

+   尝试不同的模型。外面有更大、更强大的模型，但有些可能太大而无法在你的硬件上运行。你需要找到性能不错但仍能以令人满意的方式运行模型的最佳平衡点。

+   尝试不同的*Documents*预处理方式以促进检索步骤*。*一个常见的例子是[将它们分成相等长度的块](https://python.langchain.com/docs/modules/data_connection/document_transformers/)。

我相信还有更多可以尝试的，以获得更好的性能。只需动手尝试并根据你的需求调整机器人。

## 进一步阅读

有关使用 LangChain 进行代码理解的更多示例，请查看他们的文档：

+   [https://python.langchain.com/docs/use_cases/code/](https://python.langchain.com/docs/use_cases/code/#)

在 HuggingFace 上，你可以找到可以在 LangChain 中轻松使用的模型和嵌入：

+   [https://huggingface.co/models](https://huggingface.co/models)

*喜欢这篇文章吗？* [*关注我*](/@doriandrost) *以便获取我未来的文章更新。*
