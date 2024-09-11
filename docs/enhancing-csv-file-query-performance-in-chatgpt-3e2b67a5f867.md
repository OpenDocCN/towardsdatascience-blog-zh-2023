# 提升 ChatGPT 中 CSV 文件查询性能

> 原文：[https://towardsdatascience.com/enhancing-csv-file-query-performance-in-chatgpt-3e2b67a5f867?source=collection_archive---------1-----------------------#2023-07-31](https://towardsdatascience.com/enhancing-csv-file-query-performance-in-chatgpt-3e2b67a5f867?source=collection_archive---------1-----------------------#2023-07-31)

## 基于自定义 CSV 加载器的 LangChain 自查询

[](https://medium.com/@vanillaxiangshuyang?source=post_page-----3e2b67a5f867--------------------------------)[![Shuyang Xiang](../Images/36a5fd18fd9b7b88cb41094f09b83882.png)](https://medium.com/@vanillaxiangshuyang?source=post_page-----3e2b67a5f867--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3e2b67a5f867--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3e2b67a5f867--------------------------------) [Shuyang Xiang](https://medium.com/@vanillaxiangshuyang?source=post_page-----3e2b67a5f867--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9b74bc8c860d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-csv-file-query-performance-in-chatgpt-3e2b67a5f867&user=Shuyang+Xiang&userId=9b74bc8c860d&source=post_page-9b74bc8c860d----3e2b67a5f867---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3e2b67a5f867--------------------------------) · 9分钟阅读 · 2023年7月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3e2b67a5f867&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-csv-file-query-performance-in-chatgpt-3e2b67a5f867&user=Shuyang+Xiang&userId=9b74bc8c860d&source=-----3e2b67a5f867---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3e2b67a5f867&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fenhancing-csv-file-query-performance-in-chatgpt-3e2b67a5f867&source=-----3e2b67a5f867---------------------bookmark_footer-----------)

高级语言模型的出现，如 ChatGPT，为查询表格数据带来了新颖而有前途的方法。然而，由于令牌限制，直接执行查询变得具有挑战性，尤其是在没有像检索器这样的 API 协助的情况下。因此，查询的准确性严重依赖于查询的质量，标准检索器往往无法返回所需的确切信息。

在这篇文章中，我将*深入探讨*传统检索方法在某些使用案例中失败的原因。此外，我们提出了一种革命性解决方案，即一种自定义的CSV数据加载器，它整合了元数据。通过利用[LangChain](https://python.langchain.com/docs/get_started/introduction.html)的自查询API及新CSV数据加载器，我们可以以显著提高的性能和精度提取信息。

有关本文中使用的详细代码，请查看[这里](https://github.com/ShuyangenFrance/ChatGPTCSV/blob/main/self-querying.ipynb)的笔记本。我想强调的是，这个笔记本展示了**使用LLM查询大型表格数据可以实现显著准确性**的可能性。

# 在疾病人口数据集上的检索

我们希望查询作者创建的以下合成SIR数据集：我们基于一个简单的[SIR模型](https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology)模拟10个城市在90天内的三组人口。为了简单起见，我们假设每个城市的人口范围从5e3到2e4，并且城市之间没有人口流动。此外，我们生成了十个介于500到2000之间的随机整数作为原始感染人数。

![](../Images/07fc21dc197c3e04df872fb527f72224.png)

作者提供的图像：10个城市的疾病人口

表格的形式如下，有五列：“time”表示测量人口的时间，“city”表示测量数据的城市，“susceptible”、“infectious”和“removed”表示人口的三组。为了简单起见，数据已保存为本地CSV文件。

```py
time susceptible infectious removed city
0 2018-01-01 8639 8639 0 city0
1 2018-01-02 3857 12338 1081 city0
2 2018-01-03 1458 13414 2405 city0
3 2018-01-04 545 12983 3749 city0
4 2018-01-05 214 12046 5017 city0
```

我们希望向ChatGPT提出与数据集相关的问题。为了让ChatGPT与这种表格数据交互，我们按照以下标准步骤使用LangChain：

1.  使用CSVLoader加载数据，

1.  创建一个vectorstore（我们这里使用Chroma）来存储带有OpenAI嵌入的嵌入数据，

1.  使用检索器返回与给定非结构化查询相关的文档。

你可以使用以下代码实现上述步骤。

```py
# load data fron local path 
loader = CSVLoader(file_path=LOCAL_PATH)
data = loader.load()

# Create embedding
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(data, embeddings)

# Create retriever 
retriever=vectorstore.as_retriever(search_kwargs={"k": 20})
```

我们现在可以定义一个*ConversationalRetriverChain*来查询SIR数据集。

```py
llm=ChatOpenAI(model_name="gpt-4",temperature=0)

# Define the system message template
system_template = """The provided {context} is a tabular dataset containing Suspectible, infectious and removed population during 90 days in 10 cities.
The dataset includes the following columns:
'time': time the population was meseaured,
'city': city in which the popoluation was measured,
"susceptible": the susceptible population of the disease, 
"infectious": the infectious population of the disease, 
"removed": the removed popolation of the disease. 
----------------
{context}"""

# Create the chat prompt templates
messages = [
    SystemMessagePromptTemplate.from_template(system_template),
    HumanMessagePromptTemplate.from_template("{question}")
]
qa_prompt = ChatPromptTemplate.from_messages(messages)
memory = ConversationBufferMemory(memory_key="chat_history", return_messages=True)
qa = ConversationalRetrievalChain.from_llm(llm=llm, retriever=vectorstore.as_retriever(), return_source_documents=False,combine_docs_chain_kwargs={"prompt": qa_prompt},memory=memory,verbose=True)
```

在上述代码中，我定义了一个对话链，它将使用在上一步中定义的检索器在SIR数据集中搜索查询的相关信息，并根据检索到的信息给出答案。为了给ChatGPT更清晰的指示，我还给出了一个提示，说明了数据集中所有列的定义。

现在我们来问一个简单的问题：“2018年2月3日哪个城市的感染人数最多？”

出乎意料的是，我们的聊天机器人说：“提供的数据集中不包括2018年2月3日的数据。”

这怎么可能？

# 为什么检索失败了？

为了调查聊天机器人为何未能回答一个其答案仅存在于提供的数据集中的问题，我查看了它用问题“2018年2月3日哪个城市的传染病人数最多？”检索到的相关文档。我得到了以下几行：

```py
[Document(page_content=': 31\ntime: 2018-02-01\nsusceptible: 0\ninfectious: 1729\nremoved: 35608\ncity: city3', metadata={'source': 'sir.csv', 'row': 301}),
 Document(page_content=': 1\ntime: 2018-01-02\nsusceptible: 3109\ninfectious: 9118\nremoved: 804\ncity: city8', metadata={'source': 'sir.csv', 'row': 721}),
 Document(page_content=': 15\ntime: 2018-01-16\nsusceptible: 1\ninfectious: 2035\nremoved: 6507\ncity: city7', metadata={'source': 'sir.csv', 'row': 645}),
 Document(page_content=': 1\ntime: 2018-01-02\nsusceptible: 3481\ninfectious: 10873\nremoved: 954\ncity: city5', metadata={'source': 'sir.csv', 'row': 451}),
 Document(page_content=': 23\ntime: 2018-01-24\nsusceptible: 0\ninfectious: 2828\nremoved: 24231\ncity: city9', metadata={'source': 'sir.csv', 'row': 833}),
 Document(page_content=': 1\ntime: 2018-01-02\nsusceptible: 8081\ninfectious: 25424\nremoved: 2231\ncity: city6', metadata={'source': 'sir.csv', 'row': 541}),
 Document(page_content=': 3\ntime: 2018-01-04\nsusceptible: 511\ninfectious: 9733\nremoved: 2787\ncity: city8', metadata={'source': 'sir.csv', 'row': 723}),
 Document(page_content=': 24\ntime: 2018-01-25\nsusceptible: 0\ninfectious: 3510\nremoved: 33826\ncity: city3', metadata={'source': 'sir.csv', 'row': 294}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 1413\nremoved: 35924\ncity: city3', metadata={'source': 'sir.csv', 'row': 303}),
 Document(page_content=': 25\ntime: 2018-01-26\nsusceptible: 0\ninfectious: 3173\nremoved: 34164\ncity: city3', metadata={'source': 'sir.csv', 'row': 295}),
 Document(page_content=': 1\ntime: 2018-01-02\nsusceptible: 3857\ninfectious: 12338\nremoved: 1081\ncity: city0', metadata={'source': 'sir.csv', 'row': 1}),
 Document(page_content=': 23\ntime: 2018-01-24\nsusceptible: 0\ninfectious: 1365\nremoved: 11666\ncity: city8', metadata={'source': 'sir.csv', 'row': 743}),
 Document(page_content=': 16\ntime: 2018-01-17\nsusceptible: 0\ninfectious: 2770\nremoved: 10260\ncity: city8', metadata={'source': 'sir.csv', 'row': 736}),
 Document(page_content=': 3\ntime: 2018-01-04\nsusceptible: 487\ninfectious: 6280\nremoved: 1775\ncity: city7', metadata={'source': 'sir.csv', 'row': 633}),
 Document(page_content=': 14\ntime: 2018-01-15\nsusceptible: 0\ninfectious: 3391\nremoved: 9639\ncity: city8', metadata={'source': 'sir.csv', 'row': 734}),
 Document(page_content=': 20\ntime: 2018-01-21\nsusceptible: 0\ninfectious: 1849\nremoved: 11182\ncity: city8', metadata={'source': 'sir.csv', 'row': 740}),
 Document(page_content=': 28\ntime: 2018-01-29\nsusceptible: 0\ninfectious: 1705\nremoved: 25353\ncity: city9', metadata={'source': 'sir.csv', 'row': 838}),
 Document(page_content=': 23\ntime: 2018-01-24\nsusceptible: 0\ninfectious: 3884\nremoved: 33453\ncity: city3', metadata={'source': 'sir.csv', 'row': 293}),
 Document(page_content=': 16\ntime: 2018-01-17\nsusceptible: 1\ninfectious: 1839\nremoved: 6703\ncity: city7', metadata={'source': 'sir.csv', 'row': 646}),
 Document(page_content=': 15\ntime: 2018-01-16\nsusceptible: 1\ninfectious: 6350\nremoved: 20708\ncity: city9', metadata={'source': 'sir.csv', 'row': 825})]
```

令人惊讶的是，尽管我明确要求了解2018年2月3日发生了什么，但没有返回该日期的任何行。由于该日期的信息从未发送给ChatGPT，因此当然毫无疑问它无法回答这样的问题。

深入研究检索器的[源代码](https://github.com/langchain-ai/langchain/blob/df40cd233f0690c1fc82d6fc0a1d25afdd7fdd42/langchain/vectorstores/base.py#L393-L398)，我们可以看到*get_relevant_dcouments*默认调用*similarity_search*。该方法根据计算的距离度量（默认使用[余弦距离](https://en.wikipedia.org/wiki/Cosine_similarity)）返回前n个片段（默认4个，但我在代码中将数量设置为20），这些片段的距离范围从0到1，用于测量查询向量与文档片段向量之间的“相似性”。

回到SIR数据集，我们注意到**每行几乎讲述了相同的故事**：哪个日期、哪个城市、以及多少人被标记为哪个组。毫不奇怪，代表这些行的向量彼此相似。快速检查相似度得分发现很多行的得分接近0.29。

因此，相似度得分不足以区分行与查询的相关性：这在表格数据中总是如此，因为行之间没有显著差异。**我们需要更强的过滤器来处理元数据。**

# 带有自定义元数据的CSVLoader

显然，聊天机器人的性能提升在很大程度上依赖于检索器的效率。为此，我们首先定义了一个自定义的CSVLoader，它可以将元数据与检索器进行通信。

我们编写了以下代码：

```py
class MetaDataCSVLoader(BaseLoader):
    """Loads a CSV file into a list of documents.

    Each document represents one row of the CSV file. Every row is converted into a
    key/value pair and outputted to a new line in the document's page_content.

    The source for each document loaded from csv is set to the value of the
    `file_path` argument for all doucments by default.
    You can override this by setting the `source_column` argument to the
    name of a column in the CSV file.
    The source of each document will then be set to the value of the column
    with the name specified in `source_column`.

    Output Example:
        .. code-block:: txt

            column1: value1
            column2: value2
            column3: value3
    """

    def __init__(
        self,
        file_path: str,
        source_column: Optional[str] = None,
        metadata_columns: Optional[List[str]] = None,   
        content_columns: Optional[List[str]] =None ,  
        csv_args: Optional[Dict] = None,
        encoding: Optional[str] = None,
    ):
        #  omitted (save as original code)
        self.metadata_columns = metadata_columns        # < ADDED

    def load(self) -> List[Document]:
        """Load data into document objects."""

        docs = []
        with open(self.file_path, newline="", encoding=self.encoding) as csvfile:
           #  omitted (save as original code)
                # ADDED CODE 
                if self.metadata_columns:
                    for k, v in row.items():
                        if k in self.metadata_columns:
                            metadata[k] = v
                # END OF ADDED CODE
                doc = Document(page_content=content, metadata=metadata)
                docs.append(doc)
        return docs
```

为节省空间，我省略了与原始API相同的代码，仅包含了主要用于将需要特别注意的某些列写入元数据的附加几行代码。确实，在上面的打印数据中，你可以注意到两个部分：页面内容和元数据。标准CSVLoader将表格的所有列写入页面内容，只将数据资源和行号写入元数据。定义的“MetaDataCSVLoader”允许我们将其他列写入元数据。

现在我们重新创建向量存储，步骤与上述章节相同，只是数据加载器中添加了两个元数据列“time”和“city”。

```py
# Load data and set embeddings 
loader = MetaDataCSVLoader(file_path="sir.csv",metadata_columns=['time','city']) #<= modified 
data = loader.load()
```

# 自查询元数据通知的向量存储

现在我们准备使用LangChain的[SelfQuerying API](https://js.langchain.com/docs/modules/data_connection/retrievers/how_to/self_query/)：

根据 LangChain 的文档：自查询检索器顾名思义，就是可以对自身进行查询的检索器。… 这使得检索器不仅可以使用用户输入的查询与存储文档的内容进行语义相似性比较，还可以从用户查询中提取关于存储文档的元数据的过滤器，并执行这些过滤器。

![](../Images/c80238ccb42251d10b5d1c3e53533d3c.png)

图片来源于 LangChain：自查询示意图

你现在可以理解为什么我在上一章强调元数据：这是基于元数据，ChatGPT 或其他 LLM 能够在其上构建过滤器，以从数据集中获取最相关的信息。我们使用以下代码构建这样的自查询检索器，通过描述元数据和文档内容描述来构建一个性能良好的过滤器，以提取准确的信息。特别地，我们给检索器提供一个*metadata_field_info*，指定我们希望检索器更多关注的两列的类型和描述。

```py
llm=ChatOpenAI(model_name="gpt-4",temperature=0)
metadata_field_info=[
     AttributeInfo(
        name="time",
        description="time the population was meseaured", 
        type="datetime", 
    ),
    AttributeInfo(
        name="city",
        description="city in which the popoluation was measured", 
        type="string", 
    ),
]
document_content_description = "Suspectible, infectious and removed population during 90 days in 10 cities "
retriever = SelfQueryRetriever.from_llm(
    llm, vectorstore, document_content_description, metadata_field_info, search_kwargs={"k": 20},verbose=True
)
```

我们现在可以定义一个类似的*ConversationalRetriverChain*来查询SIR数据集，但这次使用*SelfQueryRetriever*。让我们看看当我们问同样的问题时会发生什么：“2018年2月3日哪个城市传染人数最多？”

聊天机器人说：“2018年2月3日传染人数最多的城市是 city3，传染人数为1413。”

各位女士们先生们，结果是正确的！聊天机器人在使用更好的检索器时表现更好！

这次查看检索器返回了哪些相关文档是没有害处的，结果是：

```py
[Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 1413\nremoved: 35924\ncity: city3', metadata={'source': 'sir.csv', 'row': 303, 'time': '2018-02-03', 'city': 'city3'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 822\nremoved: 20895\ncity: city4', metadata={'source': 'sir.csv', 'row': 393, 'time': '2018-02-03', 'city': 'city4'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 581\nremoved: 14728\ncity: city5', metadata={'source': 'sir.csv', 'row': 483, 'time': '2018-02-03', 'city': 'city5'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 1355\nremoved: 34382\ncity: city6', metadata={'source': 'sir.csv', 'row': 573, 'time': '2018-02-03', 'city': 'city6'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 496\nremoved: 12535\ncity: city8', metadata={'source': 'sir.csv', 'row': 753, 'time': '2018-02-03', 'city': 'city8'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 1028\nremoved: 26030\ncity: city9', metadata={'source': 'sir.csv', 'row': 843, 'time': '2018-02-03', 'city': 'city9'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 330\nremoved: 8213\ncity: city7', metadata={'source': 'sir.csv', 'row': 663, 'time': '2018-02-03', 'city': 'city7'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 1320\nremoved: 33505\ncity: city2', metadata={'source': 'sir.csv', 'row': 213, 'time': '2018-02-03', 'city': 'city2'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 776\nremoved: 19753\ncity: city1', metadata={'source': 'sir.csv', 'row': 123, 'time': '2018-02-03', 'city': 'city1'}),
 Document(page_content=': 33\ntime: 2018-02-03\nsusceptible: 0\ninfectious: 654\nremoved: 16623\ncity: city0', metadata={'source': 'sir.csv', 'row': 33, 'time': '2018-02-03', 'city': 'city0'})]
```

你可能会立刻注意到在检索到的文档中现在有“时间”和“城市”这些“元数据”。

# 结论

在这篇博客文章中，我探讨了 ChatGPT 在查询 CSV 格式数据集时的局限性，以10个城市的SIR数据集在90天内为例。为了应对这些局限性，我提出了一种新方法：**一种感知元数据的CSV数据加载器，使我们能够利用自查询API，显著提高聊天机器人的准确性和性能。**
