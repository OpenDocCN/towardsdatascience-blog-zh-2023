# RAG：如何与您的数据交流

> 原文：[https://towardsdatascience.com/rag-how-to-talk-to-your-data-eaf5469b83b0?source=collection_archive---------0-----------------------#2023-11-11](https://towardsdatascience.com/rag-how-to-talk-to-your-data-eaf5469b83b0?source=collection_archive---------0-----------------------#2023-11-11)

## 详细指南：如何使用 ChatGPT 分析客户反馈

[](https://miptgirl.medium.com/?source=post_page-----eaf5469b83b0--------------------------------)[![Mariya Mansurova](../Images/b1dd377b0a1887db900cc5108bca8ea8.png)](https://miptgirl.medium.com/?source=post_page-----eaf5469b83b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eaf5469b83b0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----eaf5469b83b0--------------------------------) [Mariya Mansurova](https://miptgirl.medium.com/?source=post_page-----eaf5469b83b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a29a4fc6ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frag-how-to-talk-to-your-data-eaf5469b83b0&user=Mariya+Mansurova&userId=15a29a4fc6ad&source=post_page-15a29a4fc6ad----eaf5469b83b0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eaf5469b83b0--------------------------------) ·21 分钟阅读·2023 年 11 月 11 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feaf5469b83b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frag-how-to-talk-to-your-data-eaf5469b83b0&source=-----eaf5469b83b0---------------------bookmark_footer-----------)![](../Images/07ab6bd0a42c6e657b2f5212e94c83c0.png)

由 DALL-E 3 提供的图像

在我的以前的文章中，我们讨论了如何使用 ChatGPT 进行主题建模。我们的任务是分析不同酒店连锁的客户评论，并确定每家酒店提到的主要主题。

通过这样的主题建模，我们知道每个客户评论的主题，可以轻松按主题筛选并深入了解。然而，在现实生活中，拥有能够涵盖所有可能用例的详尽主题集是不可能的。

例如，这是我们从客户反馈中先前识别出的主题列表。

![](../Images/617d605242d40f7a4640d3b393394cf5.png)

这些主题可以帮助我们获得客户反馈的高层次概述，并进行初步预筛选。但是，假设我们想了解客户对健身房或早餐饮品的看法。在这种情况下，我们将需要自己从“酒店设施”和“早餐”主题中浏览相当多的客户反馈。

幸运的是，LLMs 可以帮助我们进行这种分析，节省大量浏览客户评论的时间（尽管自己倾听客户的声音仍可能是有帮助的）。在本文中，我们将讨论这些方法。

我们将继续使用 LangChain（最流行的 LLM 应用框架之一）。你可以在[我之前的文章](https://medium.com/towards-data-science/topic-modelling-in-production-e3b3e99e4fca)中找到 LangChain 的基本概述。

# 幼稚的方法

获取与特定主题相关的评论最直接的方法就是在文本中寻找一些特定的词汇，比如“健身房”或“饮料”。在 ChatGPT 出现之前，我曾多次使用这种方法。

这种方法的问题是相当明显的：

+   你可能会得到很多不相关的关于附近健身房或酒店餐厅酒精饮料的评论。这种过滤器不够具体，不能考虑上下文，因此你会有很多假阳性。

+   另一方面，你可能也无法获得足够好的覆盖范围。人们往往对相同的事物使用略微不同的词汇（例如，饮料、茶点、饮品、果汁等）。可能还会有拼写错误。如果你的客户说不同的语言，这个任务可能会变得更加复杂。

因此，这种方法在精准度和召回率方面都有问题。它会给你对问题的粗略理解，但能力有限。

另一种潜在的解决方案是使用与主题建模相同的方法：将所有客户评论发送给 LLM，并让模型确定它们是否与我们的兴趣主题相关（早餐饮品或健身房）。我们甚至可以要求模型总结所有客户反馈并提供结论。

这种方法可能会工作得很好。然而，它也有其局限性：每次你想深入探讨一个特定话题时，你需要将所有文档发送给 LLM。即使基于我们定义的主题进行高水平过滤，传递给 LLM 的数据量也可能相当大，而且成本也会相当高。

幸运的是，还有另一种解决这个任务的方法，它被称为 RAG。

# 检索增强生成

我们有一组文档（客户评论），我们希望提出与这些文档内容相关的问题（例如，“客户喜欢早餐的哪些方面？”）。正如我们之前讨论的，我们不想将所有客户评论都发送给 LLM，因此我们需要一种方法来定义最相关的评论。然后，任务将变得非常简单：将用户问题和这些文档作为上下文传递给 LLM，就可以了。

这种方法称为[检索增强生成](https://python.langchain.com/docs/use_cases/question_answering/)或RAG。

![](../Images/eeacde37f8694c0b34865d586c1ff75b.png)

作者提供的方案

RAG的流水线包括以下几个阶段：

+   **加载文档**从我们拥有的数据源。

+   **将文档分割**为更容易进一步使用的块。

+   **存储：** 向量存储通常用于此用例，以有效处理数据。

+   **检索**与问题相关的文档。

+   **生成**是将问题和相关文档传递给LLM并获得最终答案**。**

您可能已经听说OpenAI本周推出了[助理API](https://platform.openai.com/docs/assistants/tools/function-calling)，它可以为您完成所有这些步骤。但我认为值得通过整个过程来理解它的工作原理及其特殊性。

因此，让我们逐步了解所有这些阶段。

## 加载文档

第一步是加载我们的文档。LangChain支持不同类型的文档，例如[CSV](https://python.langchain.com/docs/modules/data_connection/document_loaders/csv)或[JSON](https://python.langchain.com/docs/modules/data_connection/document_loaders/json)。

您可能会想知道使用LangChain处理这些基本数据类型的好处是什么。毫无疑问，您可以使用标准Python库解析CSV或JSON文件。但我建议使用LangChain数据加载器API，因为它返回包含内容和元数据的文档对象。稍后使用LangChain文档会更容易。

让我们看看一些更复杂的数据类型的例子。

我们经常需要分析网页内容，因此必须处理HTML。即使您已经掌握了[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)库，您可能会发现[BSHTMLLoader](https://python.langchain.com/docs/modules/data_connection/document_loaders/html)也很有帮助。

与LLM应用相关的HTML的有趣之处在于，很可能您需要对其进行大量预处理。如果您使用浏览器检查工具查看任何网站，您会注意到比网站上看到的文本要多得多。它用于指定布局、格式和样式等。

![](../Images/0dd75068e94af13d284fab15e72ad923.png)

作者提供的图片，[LangChain文档](https://python.langchain.com/docs/modules/data_connection/document_loaders/html)

在大多数实际情况下，我们不需要将所有这些数据传递给LLM。一个站点的整个HTML很容易超过200K标记（只有用户看到的文本约为10-20%），因此将其适应上下文大小将是一项挑战。而且，这些技术信息可能会让模型的工作变得更加困难。

因此，从HTML中提取文本并将其用于进一步分析是相当标准的做法。要做到这一点，你可以使用下面的命令。结果，你将得到一个文档对象，其中网页内容在`page_content`参数中。

```py
from langchain.document_loaders import BSHTMLLoader

loader = BSHTMLLoader("my_site.html")
data = loader.load()
```

另一个常用的数据类型是PDF。我们可以解析PDF，例如使用PyPDF库。让我们从DALL-E 3论文中加载文本。

```py
from langchain.document_loaders import PyPDFLoader
loader = PyPDFLoader("https://cdn.openai.com/papers/DALL_E_3_System_Card.pdf")
doc = loader.load()
```

在输出中，你会得到一组文档 — 每页一个文档。在元数据中，`source`和`page`字段都会被填充。

因此，正如你所见，LangChain允许你处理广泛的不同文档类型。

让我们回到我们最初的任务。在我们的数据集中，每个酒店都有一个单独的.txt文件，其中包含顾客的评论。我们需要解析目录中的所有文件并将它们整合在一起。我们可以使用`DirectoryLoader`来完成这个任务。

```py
from langchain.document_loaders import TextLoader, DirectoryLoader

text_loader_kwargs={'autodetect_encoding': True}
loader = DirectoryLoader('./hotels/london', show_progress=True, 
    loader_cls=TextLoader, loader_kwargs=text_loader_kwargs)

docs = loader.load()
len(docs)
82
```

我们的文本不是标准的UTF-8编码，所以我还使用了`'autodetect_encoding': True`。

结果，我们得到了文档列表 — 每个文本文件一个文档。我们知道每个文档由独立的客户评论组成。与其处理酒店所有顾客评论的大文本，我们更有效地使用较小的块来处理。因此，我们需要分割我们的文档。让我们继续下一阶段，详细讨论文档分割。

## 文档分割

下一步是分割文档。也许你会想为什么我们需要这样做。文档通常很长，涵盖多个主题，例如Confluence页面或文档。如果我们将这样的长文本传递给LLMs，我们可能会面临以下问题：要么LLM被无关信息分散注意力，要么文本不适合上下文大小。

因此，为了有效地处理LLMs，值得从我们的知识库（文档集合）中定义最相关的信息，并仅将此信息传递给模型。这就是为什么我们需要将文档分割成较小块的原因。

通常用于一般文本的最常见技术是[递归字符分割](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/recursive_text_splitter)。在LangChain中，它是由`RecursiveCharacterTextSplitter`类实现的。

让我们尝试理解它是如何工作的。首先，你需要定义一个优先级列表用于分割器（默认为`["\n\n", "\n", " ", ""]`）。然后，分割器会逐个字符地遍历这个列表，并尝试将文档分割成足够小的块。这意味着该方法试图保持语义上紧密相关的部分在一起（段落、句子、单词），直到我们需要分割它们以达到期望的块大小。

让我们使用[Python之禅](https://peps.python.org/pep-0020/#easter-egg)看看它是如何工作的。这段文字有824个字符，139个单词和21个段落。

> 如果你执行`import this`，你可以看到Python之禅。

```py
zen = '''
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one -- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
'''

print('Number of characters: %d' % len(zen))
print('Number of words: %d' % len(zen.replace('\n', ' ').split(' ')))
print('Number of paragraphs: %d' % len(zen.split('\n')))

# Number of characters: 825
# Number of words: 140
# Number of paragraphs: 21
```

让我们使用`RecursiveCharacterTextSplitter`，并从相对较大的块大小开始，设为300。

```py
from langchain.text_splitter import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size = 300,
    chunk_overlap  = 0,
    length_function = len,
    is_separator_regex = False,
)
text_splitter.split_text(zen)
```

我们将得到三个块：264、293和263个字符。我们可以看到所有的句子都保持在一起。

> 以下所有图像均由作者制作。

![](../Images/80a3551e4ca8caa9f264b0c457177aac.png)

你可能会注意到一个`chunk_overlap`参数，它允许你进行重叠分割。这很重要，因为我们将把一些块和问题一起传递给LLM，而拥有足够的上下文来仅根据每个块中提供的信息做出决策是至关重要的。

![](../Images/1ea599b673f69b36e3a2e164519cecfb.png)

作者方案

让我们尝试添加`chunk_overlap`。

```py
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size = 300,
    chunk_overlap  = 100,
    length_function = len,
    is_separator_regex = False,
)
text_splitter.split_text(zen)
```

现在，我们有四个分割块，字符数分别为264、232、297和263，我们可以看到我们的块有重叠。

![](../Images/efdab3a9394850a6aa0ed1b8be657a27.png)

让我们把块的大小稍微调小一点。

```py
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size = 50,
    chunk_overlap  = 10,
    length_function = len,
    is_separator_regex = False,
)
text_splitter.split_text(zen)
```

现在，我们甚至不得不分割一些较长的句子。这就是递归分割的工作原理：由于按段落（`"\n"`）分割后，块仍然不够小，因此分割器继续按`" "`分割。

![](../Images/a4799d99b63f977376473a7390690c76.png)

你可以进一步自定义分割。例如，你可以指定`length_function = lambda x: len(x.split("\n"))`来使用段落的数量作为块的长度，而不是字符的数量。按[标记分割](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/split_by_token)也很常见，因为LLM的上下文大小基于标记的数量。

另一种潜在的自定义方式是使用其他`separators`，而不是用`","`而是用`" "`来分隔。让我们尝试用几句话来使用它。

```py
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size = 50,
    chunk_overlap  = 0,
    length_function = len,
    is_separator_regex = False,
    separators=["\n\n", "\n", ", ", " ", ""]
)
text_splitter.split_text('''\
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.''')
```

它有效，但逗号的位置不对。

![](../Images/93224fc16ae8c68ae604642a25f39b4c.png)

为了解决这个问题，我们可以使用带回顾的正则表达式作为分隔符。

```py
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size = 50,
    chunk_overlap  = 0,
    length_function = len,
    is_separator_regex = True,
    separators=["\n\n", "\n", "(?<=\, )", " ", ""]
)
text_splitter.split_text('''\
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.''')
```

现在已修复。

![](../Images/b0d44768c377383b9f49024919ac2fab.png)

此外，LangChain 提供了[处理代码的工具](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/code_splitter)，可以根据编程语言特定的分隔符来分割文本。

然而，在我们的情况下，情况更简单。我们知道每个文件中有用`"\n"`分隔的独立评论，我们只需按此分隔即可。不幸的是，LangChain 不支持这种基本用例，因此我们需要进行一些黑客操作以使其按我们想要的方式工作。

```py
from langchain.text_splitter import CharacterTextSplitter

text_splitter = CharacterTextSplitter(
    separator = "\n",
    chunk_size = 1,
    chunk_overlap  = 0,
    length_function = lambda x: 1, # hack - usually len is used 
    is_separator_regex = False
)
split_docs = text_splitter.split_documents(docs)
len(split_docs) 
12890
```

> 你可以在[我之前关于 LangChain 的文章](https://medium.com/towards-data-science/topic-modelling-in-production-e3b3e99e4fca)中找到更多关于我们为什么需要这个 hack 的详细信息。

文档的重要部分是元数据，因为它可以提供有关该块来源的更多上下文。在我们的例子中，LangChain 自动填充了元数据的`source`参数，因此我们知道每条评论涉及哪个酒店。

![](../Images/ca49a0b04359615bfb5dc8939c97fcdc.png)

还有其他方法（例如用于[HTML](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/HTML_header_metadata)或[Markdown](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/markdown_header_metadata)的方法），它们在拆分文档时添加标题到元数据。如果您正在处理这些数据类型，这些方法可能非常有帮助。

## 向量存储

现在我们有评论文本，下一步是学习如何有效地存储它们，以便我们可以获得相关的文档来回答我们的问题。

我们可以将评论存储为字符串，但这对我们解决这个任务没有帮助——我们无法过滤与问题相关的客户评论。

更加功能强大的解决方案是存储文档的嵌入。

嵌入是高维向量。嵌入捕捉单词和短语之间的语义含义和关系，因此语义上接近的文本之间的距离较小。

我们将使用[OpenAI嵌入](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)，因为它们非常流行。OpenAI建议使用`text-embedding-ada-002`模型，因为它具有更好的性能、更广泛的上下文和更低的价格。像往常一样，它有[其风险和限制](https://platform.openai.com/docs/guides/embeddings/limitations-risks)：潜在的社会偏见和对最近事件的有限了解。

让我们尝试在玩具示例上使用嵌入来看看它的工作原理。

```py
from langchain.embeddings.openai import OpenAIEmbeddings
embedding = OpenAIEmbeddings()

text1 = 'Our room (standard one) was very clean and large.'
text2 = 'Weather in London was wonderful.'
text3 = 'The room I had was actually larger than those found in other hotels in the area, and was very well appointed.'

emb1 = embedding.embed_query(text1)
emb2 = embedding.embed_query(text2)
emb3 = embedding.embed_query(text3)

print('''
Distance 1 -> 2: %.2f
Distance 1 -> 3: %.2f
Distance 2-> 3: %.2f
''' % (np.dot(emb1, emb2), np.dot(emb1, emb3), np.dot(emb2, emb3)))
```

> 我们可以使用`*np.dot*`作为余弦相似度，因为OpenAI嵌入已经被归一化。

我们可以看到第一和第三个向量彼此接近，而第二个向量不同。第一和第三个句子有类似的语义含义（它们都是关于房间大小），而第二个句子不接近，讨论天气。因此，嵌入之间的距离实际上反映了文本之间的语义相似性。

![](../Images/42b71fa8f8b25c311ff5de86ee2ec70e.png)

现在，我们知道如何将评论转换为数值向量。下一个问题是如何存储这些数据，以便轻松访问。

让我们考虑一下我们的用例。我们的流程将是：

+   获取一个问题，

+   计算其嵌入，

+   找到与此问题相关的最相关的文档块（与此嵌入距离最小的文档块），

+   最后，将找到的块作为上下文与初始问题一起传递给LLM。

数据存储的常规任务是找到K个最近的向量（K个最相关的文档）。因此，我们需要计算我们问题的嵌入与我们拥有的所有向量之间的距离（在我们的情况下，[余弦相似度](https://zh.wikipedia.org/wiki/%E4%BD%99%E5%BC%A6%E7%9B%B8%E4%BC%BC%E5%BA%A6)）。

通用数据库（如Snowflake或Postgres）在这样的任务中表现不佳。但是有些数据库被优化，特别适合这种用例——向量数据库。

我们将使用一个开源嵌入数据库，[Chroma](https://www.trychroma.com/)。Chroma 是一个轻量级的内存数据库，非常适合原型设计。你可以在[这里](https://python.langchain.com/docs/integrations/vectorstores/)找到更多的向量存储选项。

首先，我们需要使用pip安装Chroma。

```py
pip install chromadb
```

我们将使用`persist_directory`来将数据本地存储并从磁盘重新加载。

```py
from langchain.vectorstores import Chroma
persist_directory = 'vector_store'

vectordb = Chroma.from_documents(
    documents=split_docs,
    embedding=embedding,
    persist_directory=persist_directory
)
```

为了在下次需要时能够从磁盘加载数据，请执行以下命令。

```py
embedding = OpenAIEmbeddings()
vectordb = Chroma(
    persist_directory=persist_directory,
    embedding_function=embedding
)
```

数据库初始化可能需要几分钟时间，因为Chroma需要加载所有文档并使用OpenAI API获取它们的嵌入。

我们可以看到所有文档已经加载完毕。

```py
print(vectordb._collection.count())
12890
```

现在，我们可以使用相似性搜索来查找关于员工礼貌的顶级客户评论。

```py
query_docs = vectordb.similarity_search('politeness of staff', k=3)
```

文档看起来与问题非常相关。

![](../Images/a7a65fa196ff784ba87b1a0ba09450a5.png)

我们已经以可访问的方式存储了客户评论，现在是时候更详细地讨论检索了。

## 检索

我们已经使用了`vectordb.similarity_search`来检索与问题最相关的块。在大多数情况下，这种方法将对你有效，但可能会有一些细节：

+   **多样性缺乏** — 模型可能会返回极其相似的文本（甚至重复），这不会给LLM带来多少新信息。

+   **未考虑元数据** — `similarity_search`不会考虑我们拥有的元数据。例如，如果我查询问题“Travelodge Farringdon的早餐”的前五条评论，结果中只有三条评论的来源等于`uk_england_london_travelodge_london_farringdon`。

+   **上下文大小限制** — 和往常一样，我们有有限的LLM上下文大小，需要将文档适配到其中。

让我们讨论一下可以帮助我们解决这些问题的技术。

**解决多样性问题 — MMR（最大边际相关性）**

相似性搜索返回与你的问题最接近的响应。但为了向模型提供完整的信息，你可能不想只关注最相似的文本。例如，对于问题“Travelodge Farringdon的早餐”，前五条客户评论可能都关于咖啡。如果我们仅查看这些评论，就会错过其他提到鸡蛋或员工行为的评论，从而对客户反馈有一定的局限性。

我们可以使用MMR（最大边际相关性）方法来增加客户评论的多样性。它的工作原理非常简单：

+   首先，我们使用`similarity_search`获取`fetch_k`与问题最相似的文档。

+   然后，我们选择了`k`中最具多样性的那些。

![](../Images/8caa8c4cf29380abbba2ec74ada30a1f.png)

作者方案

如果我们想使用MMR，我们应该使用`max_marginal_relevance_search`而不是`similarity_search`，并指定`fetch_k`数量。值得保持`fetch_k`相对较小，以便输出中不会有不相关的答案。就这些。

```py
query_docs = vectordb.max_marginal_relevance_search('politeness of staff', 
    k = 3, fetch_k = 30)
```

让我们来看一下相同查询的示例。这次我们收到了更多样化的反馈，甚至还有带有负面情绪的评论。

![](../Images/2eac3daac518b2051659eea0af9bbbd3.png)

**解决特异性问题 — LLM辅助检索**

另一个问题是我们在检索文档时没有考虑元数据。为了解决这个问题，我们可以让LLM将初始问题拆分为两部分：

+   基于文档文本的语义过滤器，

+   基于我们拥有的元数据进行过滤，

这种方法被称为[“自查询”](https://python.langchain.com/docs/modules/data_connection/retrievers/self_query)。

首先，让我们添加一个手动过滤器，指定与Travelodge Farringdon酒店相关的`source`参数的文件名。

```py
query_docs = vectordb.similarity_search('breakfast in Travelodge Farrigdon', 
  k=5,
  filter = {'source': 'hotels/london/uk_england_london_travelodge_london_farringdon'}
)
```

现在，让我们尝试使用LLM自动生成这样的过滤器。我们需要详细描述所有元数据参数，然后使用`SelfQueryRetriever`。

```py
from langchain.llms import OpenAI
from langchain.retrievers.self_query.base import SelfQueryRetriever
from langchain.chains.query_constructor.base import AttributeInfo

metadata_field_info = [
    AttributeInfo(
        name="source",
        description="All sources starts with 'hotels/london/uk_england_london_' \
          then goes hotel chain, constant 'london_' and location.",
        type="string",
    )
]

document_content_description = "Customer reviews for hotels"
llm = OpenAI(temperature=0.1) # low temperature to make model more factual
# by default 'text-davinci-003' is used

retriever = SelfQueryRetriever.from_llm(
    llm,
    vectordb,
    document_content_description,
    metadata_field_info,
    verbose=True
)

question = "breakfast in Travelodge Farringdon"
docs = retriever.get_relevant_documents(question, k = 5)
```

我们的情况很棘手，因为元数据中的`source`参数包含多个字段：国家、城市、酒店连锁和位置。在这种情况下，将如此复杂的参数拆分为更详细的子参数是值得的，以便模型可以更容易地理解如何使用元数据过滤器。

然而，通过详细的提示，它确实有效，并仅返回了与Travelodge Farringdon相关的文档。但我必须承认，这花了我几个迭代才达到这个结果。

让我们开启调试模式看看它的工作情况。要进入调试模式，只需执行下面的代码。

```py
import langchain 
langchain.debug = True
```

完整的提示非常长，所以让我们看看它的主要部分。这是提示的开头，给模型一个我们期望的概述和结果的主要标准。

![](../Images/dde2c1f718e302800303ff0a749009de.png)

然后，使用少量示例提示技术，模型提供了两个输入和期望输出的示例。这是其中一个示例。

![](../Images/9a18e1d83aa3cba03202f7f72f2e30c0.png)

我们并没有使用像ChatGPT这样的聊天模型，而是使用通用的LLM（没有针对指令进行微调）。它只是训练来预测文本的后续标记。这就是为什么我们以问题和字符串`Structured output:`结束提示，期待模型提供答案的原因。

![](../Images/5282f1b7a1d3110c851092eba1c448e3.png)

结果，我们从模型那里得到的初始问题被拆分为两部分：语义部分（`breakfast`）和元数据过滤器（`source = hotels/london/uk_england_london_travelodge_london_farringdon`）

![](../Images/5fcd1ac6561fb29d2dd90b4421eb65f2.png)

然后，我们使用了这种逻辑从我们的向量存储中检索文档，并仅获取了我们需要的文档。

**解决大小限制 — 压缩**

另一种可能有用的检索技术是压缩。尽管GPT 4 Turbo的上下文大小为128K标记，但它仍然有限。因此，我们可能需要预处理文档并仅提取相关部分。

主要优势有：

+   您将能够将更多文档和信息整合到最终提示中，因为它们将被压缩。

+   您将会得到更好、更集中的结果，因为在预处理期间将清除非相关的上下文。

这些好处是有代价的 — 您将需要更多的LLM调用来进行压缩，这意味着更慢的速度和更高的价格。

您可以在[文档](https://python.langchain.com/docs/modules/data_connection/retrievers/contextual_compression/)中找到有关此技术的更多信息。

![](../Images/158eac2e3af993c03b952a02ff8b848b.png)

作者提出的方案

实际上，我们甚至可以结合技术并在这里使用MMR。我们使用`ContextualCompressionRetriever`来获取结果。此外，我们指定了我们只想要三个文档作为返回结果。

```py
from langchain.retrievers import ContextualCompressionRetriever
from langchain.retrievers.document_compressors import LLMChainExtractor

llm = OpenAI(temperature=0)
compressor = LLMChainExtractor.from_llm(llm)
compression_retriever = ContextualCompressionRetriever(
    base_compressor=compressor,
    base_retriever=vectordb.as_retriever(search_type = "mmr",  
      search_kwargs={"k": 3})
)

question = "breakfast in Travelodge Farringdon"
compressed_docs = compression_retriever.get_relevant_documents(question)
```

像往常一样，了解其内部运作方式是最有趣的部分。如果我们看实际调用，可以看到有三次调用LLM来从文本中提取仅相关信息的情况。这里有一个例子。

![](../Images/81e83d6794873f70fbdf0c74d5390dbd.png)

在输出中，我们只得到了与早餐相关的部分句子，所以压缩有所帮助。

![](../Images/4568d36d28b10db7323cd8bd5bb86c55.png)

有许多更有利的检索方法，例如经典自然语言处理技术：[支持向量机（SVM）](https://python.langchain.com/docs/integrations/retrievers/svm)或者[TF-IDF](https://python.langchain.com/docs/integrations/retrievers/tf_idf)。不同的检索器可能在不同情况下有所帮助，因此我建议您为您的任务比较不同版本，并选择最适合您使用情况的版本。

## 生成

最后，我们来到了最后阶段：我们将所有内容合并并生成最终答案。

这里是它们将如何运作的一个方案：

+   我们收到了用户的一个问题，

+   我们从向量存储中使用嵌入检索了此问题的相关文档，

+   我们将初始问题与从嵌入中检索到的相关文档一起传递给LLM，并获得最终答案。

![](../Images/5e3e0cfc54ba45e0085e7fbca218baec.png)

作者提出的方案

在LangChain中，我们可以使用`RetrievalQA`链快速实现这一流程。

```py
from langchain.chains import RetrievalQA

from langchain.chat_models import ChatOpenAI
llm = ChatOpenAI(model_name='gpt-4', temperature=0.1)

qa_chain = RetrievalQA.from_chain_type(
    llm,
    retriever=vectordb.as_retriever(search_kwargs={"k": 3})
)

result = qa_chain({"query": "what customers like about staff in the hotel?"})
```

让我们看一下对ChatGPT的调用。正如您所见，我们将检索到的文档与用户查询一起传递。

![](../Images/123f5083f0c9c4e4bfedae15e9d50a1a.png)

这里是模型的输出。

![](../Images/f1b887953657f01038cc79a9bc1101c2.png)

我们可以调整模型的行为，定制提示。例如，我们可以要求模型更加简洁。

```py
from langchain.prompts import PromptTemplate

template = """
Use the following pieces of context to answer the question at the end. 
If you don't know the answer, just say that you don't know, don't try 
to make up an answer. 
Keep the answer as concise as possible. Use 1 sentence to sum all points up.
______________
{context}
Question: {question}
Helpful Answer:"""

QA_CHAIN_PROMPT = PromptTemplate.from_template(template)

qa_chain = RetrievalQA.from_chain_type(
    llm,
    retriever=vectordb.as_retriever(),
    return_source_documents=True,
    chain_type_kwargs={"prompt": QA_CHAIN_PROMPT}
)
result = qa_chain({"query": "what customers like about staff in the hotel?"})
```

这次我们得到了一个更短的答案。此外，由于我们指定了`return_source_documents=True`，我们得到了一组返回的文档。这对于调试可能有帮助。

![](../Images/ac6a6d4dd1e70ed16df741ceacce5658.png)

正如我们所见，所有检索到的文档默认都合并在一个提示中。这种方法既优秀又简单，因为只需要一个调用来执行语言模型。唯一的限制是您的文档必须符合上下文大小。如果不符合，您需要应用更复杂的技术。

让我们看看不同的链类型，它们可以让我们处理任意数量的文档。第一个是MapReduce。

这种方法类似于经典的[MapReduce](https://en.wikipedia.org/wiki/MapReduce)：我们根据每个检索到的文档生成答案（map阶段），然后将这些答案合并成最终答案（reduce阶段）。

![](../Images/94d4aa804326b3615e79dddf6a55fbe0.png)

作者的方案

所有这些方法的局限性在于成本和速度。你需要为每个检索到的文档进行一次调用，而不是一次调用LLM。

关于代码，我们只需要指定`chain_type="map_reduce"`以改变行为。

```py
qa_chain_mr = RetrievalQA.from_chain_type(
    llm,
    retriever=vectordb.as_retriever(),
    chain_type="map_reduce"
)
result = qa_chain_mr({"query": "what customers like about staff in the hotel?"})
```

结果，我们得到了以下输出。

![](../Images/035d0587917428467a88ed5dff0bfdd5.png)

让我们在调试模式下看看它是如何工作的。由于这是MapReduce，我们首先将每个文档发送到LLM，并根据这个块得到答案。以下是其中一个块的提示示例。

![](../Images/284f539a3a8f40a15ee11cd9fb855b3a.png)

然后，我们将所有结果结合起来，并要求LLM给出最终答案。

![](../Images/5a3a248a9afc2448760caf5dad93b6fd.png)

就这样。

MapReduce方法还有另一个特定的缺点。模型分别看到每个文档，而不是将它们全部放在同一上下文中，这可能导致更差的结果。

我们可以通过Refine链类型克服这个缺点。然后，我们将按顺序查看文档，并允许模型在每次迭代时细化答案。

![](../Images/f1e06fac8143b3a7f59e5395d3b27cbb.png)

作者的方案

再次，我们只需要更改`chain_type`以测试另一种方法。

```py
qa_chain_refine = RetrievalQA.from_chain_type(
    llm,
    retriever=vectordb.as_retriever(),
    chain_type="refine"
)
result = qa_chain_refine({"query": "what customers like about staff in the hotel?"})
```

使用Refine链，我们得到了一个更详细和完整的答案。

![](../Images/6335d9a32845b1e132d6419e17a4e4c2.png)

让我们使用调试模式看看它是如何工作的。对于第一个块，我们从头开始。

![](../Images/0929294c4835fcf65dcf04f5540fb070.png)

然后，我们传递当前的答案和一个新的块，并给模型一个机会来细化其答案。

![](../Images/b191a4c12c728851bbe8ca57f4840d9c.png)

然后，我们对每个剩余的检索文档重复细化提示，最终得到结果。

今天我想告诉你的就这些。让我们快速回顾一下。

# 总结

在本文中，我们详细介绍了检索增强生成的整个过程：

+   我们已经查看了不同的数据加载器。

+   我们讨论了数据拆分的可能方法及其潜在的细微差别。

+   我们了解了什么是嵌入，并建立了一个向量存储库以有效地访问数据。

+   我们找到了检索问题的不同解决方案，并学习了如何增加多样性、克服上下文大小限制以及使用元数据。

+   最后，我们使用了`RetrievalQA`链来生成基于我们数据的答案，并比较了不同的链类型。

这些知识应该足够你开始使用自己的数据构建类似的东西。

> 非常感谢您阅读本文。希望本文对您有所启发。如果您有任何后续问题或评论，请在评论部分留言。

# 数据集

*Ganesan, Kavita 和 Zhai, ChengXiang. (2011). OpinRank 评论数据集.

UCI 机器学习库 (CC BY 4.0).* [*https://doi.org/10.24432/C5QW4W*](https://doi.org/10.24432/C5QW4W.)

# 参考资料

本文基于以下课程信息：

+   [“LLM 应用开发的 LangChain”](https://www.deeplearning.ai/short-courses/langchain-for-llm-application-development/)由 DeepLearning.AI 和 LangChain 提供，

+   [“LangChain：与您的数据聊天”](https://www.deeplearning.ai/short-courses/langchain-chat-with-your-data/)由 DeepLearning.AI 和 LangChain 提供。
