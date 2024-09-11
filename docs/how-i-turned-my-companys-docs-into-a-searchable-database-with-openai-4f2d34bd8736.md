# 如何将公司的文档转变为可搜索的数据库，利用OpenAI

> 原文：[https://towardsdatascience.com/how-i-turned-my-companys-docs-into-a-searchable-database-with-openai-4f2d34bd8736?source=collection_archive---------0-----------------------#2023-04-25](https://towardsdatascience.com/how-i-turned-my-companys-docs-into-a-searchable-database-with-openai-4f2d34bd8736?source=collection_archive---------0-----------------------#2023-04-25)

## 以及你如何用同样的方法处理你的文档

[](https://medium.com/@jacob_marks?source=post_page-----4f2d34bd8736--------------------------------)[![Jacob Marks, Ph.D.](../Images/94d9832b8706d1044e3195386613bfab.png)](https://medium.com/@jacob_marks?source=post_page-----4f2d34bd8736--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4f2d34bd8736--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4f2d34bd8736--------------------------------) [Jacob Marks, Ph.D.](https://medium.com/@jacob_marks?source=post_page-----4f2d34bd8736--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7dc0c0eae92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-turned-my-companys-docs-into-a-searchable-database-with-openai-4f2d34bd8736&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=post_page-f7dc0c0eae92----4f2d34bd8736---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4f2d34bd8736--------------------------------) ·15分钟阅读·2023年4月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4f2d34bd8736&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-turned-my-companys-docs-into-a-searchable-database-with-openai-4f2d34bd8736&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=-----4f2d34bd8736---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4f2d34bd8736&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-turned-my-companys-docs-into-a-searchable-database-with-openai-4f2d34bd8736&source=-----4f2d34bd8736---------------------bookmark_footer-----------)![](../Images/a7959bea4e365ac7dd00c74aaa9d1aff.png)

图片来自Unsplash。

在过去的六个月里，我一直在系列A初创公司Voxel51工作，并且是[开源计算机视觉工具包FiftyOne](https://github.com/voxel51/fiftyone)的创建者。作为一名机器学习工程师和开发者推广员，我的工作是倾听我们的开源社区，满足他们的需求——新的功能、集成、教程、研讨会，等等。

几周前，我们为 FiftyOne 添加了对向量搜索引擎和文本相似性查询的原生支持，以便用户可以通过简单的自然语言查询在他们（通常是庞大的——包含数百万或数千万样本的）数据集中找到最相关的图像。

这让我们处于一个奇特的位置：现在使用开源 FiftyOne 的人可以轻松地用自然语言查询搜索数据集，但使用我们的文档仍然需要传统的关键字搜索。

我们有大量文档，这有利有弊。作为用户，我有时发现，由于文档量庞大，找到我确切需要的信息比我希望的时间要长。

我不打算就此罢休……所以我在业余时间构建了这个：

![](../Images/a88b4645cde0b9861a7a24dcf2bf89e3.png)

从命令行语义化搜索公司文档。图片由作者提供。

所以，这是我如何将我们的文档转换为语义搜索的向量数据库：

+   [将所有文档转换为统一格式](#eb87)

+   [将文档拆分为块并添加了一些自动清理](#8ed1)

+   [为每个块计算了嵌入](#1a17)

+   [从这些嵌入生成了一个向量索引](#34d3)

+   [定义了索引查询](#87c5)

+   [将所有内容封装在用户友好的命令行界面和 Python API 中](#9d79)

你可以在 [voxel51/fiftyone-docs-search](https://github.com/voxel51/fiftyone-docs-search) 仓库中找到本帖的所有代码，并且可以通过 `pip install -e .` 在本地编辑模式中轻松安装包。

更好的是，如果你想使用这种方法为你自己的网站实现语义搜索，你可以跟着做！以下是你需要的材料：

+   *安装* [*openai*](https://github.com/openai/openai-python) *Python 包并创建一个账户：* 你将使用此账户将文档和查询发送到推断端点，该端点会返回每个文本片段的嵌入向量。

+   *安装* [*qdrant-client*](https://github.com/qdrant/qdrant_client) *Python 包并通过* [*Docker 启动 Qdrant 服务器*](https://qdrant.tech/)：你将使用 [Qdrant](https://qdrant.tech/) 为文档创建一个本地托管的向量索引，查询将对其进行运行。Qdrant 服务将在 Docker 容器中运行。

# 将文档转换为统一格式

我公司的文档都托管为 HTML 文档，地址是 [https://docs.voxel51.com](https://docs.voxel51.com)。一个自然的起点是使用 Python 的 [requests](https://pypi.org/project/requests/) 库下载这些文档，并用 [Beautiful Soup](https://beautiful-soup-4.readthedocs.io/en/latest/) 解析文档。

作为开发者（以及我们许多文档的作者），我认为自己可以做得更好。我已经在本地计算机上有一个 GitHub 仓库的工作副本，其中包含生成 HTML 文档所用的所有原始文件。我们的某些文档是用 [Sphinx ReStructured Text (RST)](https://www.sphinx-doc.org/en/master/) 编写的，而其他文档，如教程，则从 Jupyter notebooks 转换为 HTML。

我（错误地）认为，离 RST 和 Jupyter 文件的原始文本越近，事情就会越简单。

## RST

在 RST 文档中，部分由仅包含 `=`、`-` 或 `_` 字符的行来划分。例如，这是 FiftyOne 用户指南中的一个文档，其中包含所有三种划分符：

![](../Images/3188d8c4ac25ff6414eabdf8675fbdd9.png)

开源 FiftyOne 文档中的 RST 文档。图片由作者提供。

然后，我可以移除所有的 RST 关键字，如 `toctree`、`code-block` 和 `button_link`（还有许多其他的），以及随关键字、区块开始或区块描述符出现的 `:`、`::` 和 `..`。

链接也很容易处理：

```py
no_links_section = re.sub(r"<[^>]+>_?","", section)
```

当我想从 RST 文件中提取部分锚点时，情况开始变得棘手。我们许多部分明确指定了锚点，而其他部分则在转换为 HTML 时被推断出来。

这里有一个例子：

```py
.. _brain-embeddings-visualization:

Visualizing embeddings
______________________

The FiftyOne Brain provides a powerful
:meth:`compute_visualization() <fiftyone.brain.compute_visualization>` method
that you can use to generate low-dimensional representations of the samples
and/or individual objects in your datasets.

These representations can be visualized natively in the App's
:ref:`Embeddings panel <app-embeddings-panel>`, where you can interactively
select points of interest and view the corresponding samples/labels of interest
in the :ref:`Samples panel <app-samples-panel>`, and vice versa.

.. image:: /images/brain/brain-mnist.png
   :alt: mnist
   :align: center

There are two primary components to an embedding visualization: the method used
to generate the embeddings, and the dimensionality reduction method used to
compute a low-dimensional representation of the embeddings.

Embedding methods
-----------------

The `embeddings` and `model` parameters of
:meth:`compute_visualization() <fiftyone.brain.compute_visualization>`
support a variety of ways to generate embeddings for your data:
```

在我们用户指南文档中的 brain.rst 文件（上面展示的部分），*Visualizing embeddings* 部分有一个由 `.. _brain-embeddings-visualization:` 指定的锚点 `#brain-embeddings-visualization`。然而，紧随其后的 *Embedding methods* 子部分有一个自动生成的锚点。

另一个很快出现的困难是如何处理 RST 中的表格。[List tables](https://sublime-and-sphinx-guide.readthedocs.io/en/latest/tables.html#list-table-directive) 相对简单。例如，这是我们 View Stages 备忘单中的一个列表表格：

```py
.. list-table::

   * - :meth:`match() <fiftyone.core.collections.SampleCollection.match>`
   * - :meth:`match_frames() <fiftyone.core.collections.SampleCollection.match_frames>`
   * - :meth:`match_labels() <fiftyone.core.collections.SampleCollection.match_labels>`
   * - :meth:`match_tags() <fiftyone.core.collections.SampleCollection.match_tags>`
```

[Grid tables](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#grid-tables) 另一方面，很快就会变得凌乱。它们给文档作者提供了很大的灵活性，但这种灵活性使得解析起来非常麻烦。看看我们 Filtering 备忘单中的这张表格：

```py
+-----------------------------------------+-----------------------------------------------------------------------+
| Operation                               | Command                                                               |
+=========================================+=======================================================================+
| Filepath starts with "/Users"           |  .. code-block::                                                      |
|                                         |                                                                       |
|                                         |     ds.match(F("filepath").starts_with("/Users"))                     |
+-----------------------------------------+-----------------------------------------------------------------------+
| Filepath ends with "10.jpg" or "10.png" |  .. code-block::                                                      |
|                                         |                                                                       |
|                                         |     ds.match(F("filepath").ends_with(("10.jpg", "10.png"))            |
+-----------------------------------------+-----------------------------------------------------------------------+
| Label contains string "be"              |  .. code-block::                                                      |
|                                         |                                                                       |
|                                         |     ds.filter_labels(                                                 |
|                                         |         "predictions",                                                |
|                                         |         F("label").contains_str("be"),                                |
|                                         |     )                                                                 |
+-----------------------------------------+-----------------------------------------------------------------------+
| Filepath contains "088" and is JPEG     |  .. code-block::                                                      |
|                                         |                                                                       |
|                                         |     ds.match(F("filepath").re_match("088*.jpg"))                      |
+-----------------------------------------+-----------------------------------------------------------------------+
```

在表格中，行可以占用任意数量的行，列的宽度也可以变化。网格表格单元格中的代码块也很难解析，因为它们占用多行空间，因此它们的内容与其他列的内容交错在一起。这意味着这些表格中的代码块需要在解析过程中有效地重建。

这不是世界末日，但也不是理想状态。

## Jupyter

Jupyter notebooks 解析起来相对简单。我能够将 Jupyter notebook 的内容读取为字符串列表，每个单元格一个字符串：

```py
import json
ifile = "my_notebook.ipynb"
with open(ifile, "r") as f:
    contents = f.read()
contents = json.loads(contents)["cells"]
contents = [(" ".join(c["source"]), c['cell_type'] for c in contents]
```

此外，部分由以 `#` 开头的 Markdown 单元格划分。

尽管面临 RST 带来的挑战，我决定转向 HTML 并平等对待我们的所有文档。

## HTML

我通过`bash generate_docs.bash`从本地安装生成了 HTML 文档，并开始用 Beautiful Soup 解析它们。然而，我很快意识到，当 RST 代码块和包含内联代码的表格被转换为 HTML 时，虽然渲染正确，但 HTML 本身却非常难以处理。以我们的过滤器备忘单为例。

当在浏览器中渲染时，我们过滤器备忘单中*日期和时间*部分之前的代码块如下所示：

![](../Images/79ae2b0dd021cda1c5fd07f853789c0a.png)

开源 FiftyOne 文档中的备忘单截图。图片由作者提供。

然而，原始 HTML 看起来是这样的：

![](../Images/be7d519067dbbfc0ee85a947f7011790.png)

RST 备忘单转换为 HTML。图片由作者提供。

这不是不可能解析，但也远非理想。

## Markdown

幸运的是，我通过使用 [markdownify](https://pypi.org/project/markdownify/) 将所有 HTML 文件转换为 Markdown 解决了这些问题。Markdown 有几个关键优点，使其成为这个工作的最佳选择。

1.  **比 HTML 更干净**：代码格式从一串杂乱的`span`元素简化为用单个`` ` ``标记的内联代码片段，代码块则用三重引号标记 ```py` ``` ```py`before and after. This also made it easy to split into text and code.
2.  **Still contained anchors:** unlike raw RST, this Markdown included section heading anchors, as the implicit anchors had already been generated. This way, I could link not just to the page containing the result, but to the specific section or subsection of that page.
3.  **Standardization**: Markdown provided a mostly uniform formatting for the initial RST and Jupyter documents, allowing us to give their content consistent treatment in the vector search application.

## Note on LangChain

Some of you may know about the open source library [LangChain](https://python.langchain.com/en/latest/index.html) for building applications with LLMs, and may be wondering why I didn’t just use LangChain’s [Document Loaders](https://python.langchain.com/en/latest/modules/indexes/document_loaders.html) and [Text Splitters](https://python.langchain.com/en/latest/modules/indexes/text_splitters.html). The answer: I needed more control!

# Processing the documents

Once the documents had been converted to Markdown, I proceeded to clean the contents and split them into smaller segments.

## Cleaning

Cleaning most consisting in removing unnecessary elements, including:

*   Headers and footers
*   Table row and column scaffolding — e.g. the `|`’s in `|select()| select_by()|`
*   Extra newlines
*   Links
*   Images
*   Unicode characters
*   Bolding — i.e. `**text**` → `text`

I also removed escape characters that were escaping from characters which have special meaning in our docs: `_` and `*`. The former is used in many method names, and the latter, as usual, is used in multiplication, regex patterns, and many other places:

```

document = document.replace("\_", "_").replace("\*", "*")

```py

## Splitting documents into semantic blocks

With the contents of our docs cleaned, I proceeded to split the docs into bite-sized blocks.

First, I split each document into sections. At first glance, it seems like this can be done by finding any line that starts with a `#` character. In my application, I did not differentiate between h1, h2, h3, and so on (`#` , `##` , `###`), so checking the first character is sufficient. However, this logic gets us in trouble when we realize that `#` is also employed to allow comments in Python code.

To bypass this problem, I split the document into text blocks and code blocks:

```

text_and_code = page_md.split('```py')
text = text_and_code[::2]
code = text_and_code[1::2]
```

然后，我用 `#` 标记文本块中的新节的开始。我从这一行中提取了节标题和锚点：

```py
def extract_title_and_anchor(header):
    header = " ".join(header.split(" ")[1:])
    title = header.split("[")[0]
    anchor = header.split("(")[1].split(" ")[0]
    return title, anchor
```

然后将每个文本或代码块分配到适当的部分。

起初，我还尝试将文本块拆分成段落，假设因为一个部分可能包含许多不同主题的信息，该部分的嵌入可能与只涉及其中一个主题的文本提示的嵌入不相似。然而，这种方法导致大多数搜索查询的最佳匹配过于集中在单行段落上，结果显示这些搜索结果并不十分有用。

> *查看附带的* [*GitHub 仓库*](https://github.com/voxel51/fiftyone-docs-search) *，了解这些方法的实现，并尝试在您自己的文档中应用！*

# 使用 OpenAI 嵌入文本和代码块

将文档转换、处理并拆分为字符串后，我为这些块生成了嵌入向量。由于大型语言模型本质上灵活且通常能力强，我决定将文本块和代码块视为相同的文本，并用相同的模型进行嵌入。

我使用了 OpenAI 的 [text-embedding-ada-002 模型](https://platform.openai.com/docs/guides/embeddings/embedding-models)，因为它易于使用，且在所有 OpenAI 的嵌入模型中（在 [BEIR 基准](https://arxiv.org/pdf/2104.08663.pdf)上）表现最佳，并且也是最便宜的。实际上，它的费用非常低（$0.0004/1K 标记），以至于为 FiftyOne 文档生成所有嵌入仅花费了几美分！正如 OpenAI 自己所说，“我们建议几乎所有用例都使用 text-embedding-ada-002。它更好、更便宜、更简单。”

使用这个嵌入模型，你可以生成一个 1536 维的向量，表示任何输入提示，最多 8,191 个标记（大约 30,000 个字符）。

要开始使用，你需要创建一个 OpenAI 账户，在 [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys) 生成 API 密钥，使用以下命令将该 API 密钥导出为环境变量：

```py
export OPENAI_API_KEY="<MY_API_KEY>"
```

你还需要安装 [openai Python 库](https://github.com/openai/openai-python)：

```py
pip install openai
```

我编写了一个 OpenAI API 的包装器，该包装器接受文本提示并返回嵌入向量：

```py
MODEL = "text-embedding-ada-002"

def embed_text(text):
    response = openai.Embedding.create(
        input=text,
        model=MODEL
    )
    embeddings = response['data'][0]['embedding']
    return embeddings
```

为了生成所有文档的嵌入，我们只需对所有文档中的每个子部分——文本和代码块——应用此函数。

# 创建 Qdrant 向量索引

拥有嵌入后，我创建了一个向量索引用于搜索。我选择使用 Qdrant 的原因与我们选择在 FiftyOne 中添加本地 Qdrant 支持的原因相同：它是开源的、免费的，并且易于使用。

要开始使用 Qdrant，你可以拉取一个预构建的 Docker 镜像并运行容器：

```py
docker pull qdrant/qdrant
docker run -d -p 6333:6333 qdrant/qdrant
```

此外，你还需要安装 Qdrant Python 客户端：

```py
pip install qdrant-client
```

我创建了 Qdrant 集合：

```py
import qdrant_client as qc
import qdrant_client.http.models as qmodels

client = qc.QdrantClient(url="localhost")
METRIC = qmodels.Distance.DOT
DIMENSION = 1536
COLLECTION_NAME = "fiftyone_docs"

def create_index():
    client.recreate_collection(
    collection_name=COLLECTION_NAME,
    vectors_config = qmodels.VectorParams(
            size=DIMENSION,
            distance=METRIC,
        )
    )
```

然后我为每个子部分（文本或代码块）创建了一个向量：

```py
import uuid
def create_subsection_vector(
    subsection_content,
    section_anchor,
    page_url,
    doc_type
    ):

    vector = embed_text(subsection_content)
    id = str(uuid.uuid1().int)[:32]
    payload = {
        "text": subsection_content,
        "url": page_url,
        "section_anchor": section_anchor,
        "doc_type": doc_type,
        "block_type": block_type
    }
    return id, vector, payload
```

对于每个向量，你可以提供额外的上下文作为 [payload](https://qdrant.tech/documentation/payload/) 的一部分。在这种情况下，我包括了结果可以找到的 URL（及锚点）、文档的 *类型*，以便用户可以指定是否希望搜索所有文档，或只是某些类型的文档，以及生成嵌入向量的字符串内容。我还添加了块类型（文本或代码），以便如果用户正在寻找代码片段，他们可以将搜索定制为此目的。

然后我将这些向量一个页面一个页面地添加到索引中：

```py
def add_doc_to_index(subsections, page_url, doc_type, block_type):
    ids = []
    vectors = []
    payloads = []

    for section_anchor, section_content in subsections.items():
        for subsection in section_content:
            id, vector, payload = create_subsection_vector(
                subsection,
                section_anchor,
                page_url,
                doc_type,
                block_type
            )
            ids.append(id)
            vectors.append(vector)
            payloads.append(payload)

    ## Add vectors to collection
    client.upsert(
        collection_name=COLLECTION_NAME,
        points=qmodels.Batch(
            ids = ids,
            vectors=vectors,
            payloads=payloads
        ),
    )
```

# 查询索引

一旦创建了索引，可以通过使用相同的嵌入模型对查询文本进行嵌入，并在索引中搜索相似的嵌入向量来执行对索引文档的搜索。使用 Qdrant 向量索引，可以通过 Qdrant 客户端的 `search()` 命令执行基本查询。

为了使公司的文档可搜索，我希望允许用户按文档的部分以及编码的块类型进行筛选。在向量搜索术语中，筛选结果同时确保返回预定数量的结果（由`top_k`参数指定）被称为*预筛选*。

为了实现这一点，我编写了一个程序化过滤器：

```py
def _generate_query_filter(query, doc_types, block_types):
    """Generates a filter for the query.
    Args:
        query: A string containing the query.
        doc_types: A list of document types to search.
        block_types: A list of block types to search.
    Returns:
        A filter for the query.
    """
    doc_types = _parse_doc_types(doc_types)
    block_types = _parse_block_types(block_types)

    _filter = models.Filter(
        must=[
            models.Filter(
                should= [
                    models.FieldCondition(
                        key="doc_type",
                        match=models.MatchValue(value=dt),
                    )
                for dt in doc_types
                ],

            ),
            models.Filter(
                should= [
                    models.FieldCondition(
                        key="block_type",
                        match=models.MatchValue(value=bt),
                    )
                for bt in block_types
                ]  
            )
        ]
    )

    return _filter
```

内部的`_parse_doc_types()`和`_parse_block_types()`函数处理参数为字符串、列表或None的情况。

然后我写了一个`query_index()`函数，它接受用户的文本查询，进行预筛选，搜索索引，并从负载中提取相关信息。该函数返回一个元组列表，形式为`(url, contents, score)`，其中得分表示结果与查询文本的匹配程度。

```py
def query_index(query, top_k=10, doc_types=None, block_types=None):
    vector = embed_text(query)
    _filter = _generate_query_filter(query, doc_types, block_types)

    results = CLIENT.search(
        collection_name=COLLECTION_NAME,
        query_vector=vector,
        query_filter=_filter,
        limit=top_k,
        with_payload=True,
        search_params=_search_params,
    )

    results = [
        (
            f"{res.payload['url']}#{res.payload['section_anchor']}",
            res.payload["text"],
            res.score,
        )
        for res in results
    ]

    return results
```

# 编写搜索包装器

最后一步是为用户提供一个干净的界面，以便在这些“向量化”文档中进行语义搜索。

我编写了一个`print_results()`函数，它接受查询、`query_index()`的结果和一个`score`参数（是否打印相似度分数），并以易于理解的方式打印结果。我使用了[rich](https://rich.readthedocs.io/en/stable/introduction.html) Python包来格式化终端中的超链接，以便在支持超链接的终端中，点击超链接将打开默认浏览器中的页面。如果需要，我还使用了[webbrowser](https://docs.python.org/3/library/webbrowser.html)来自动打开最顶端结果的链接。

![](../Images/8e615542c724d5d85be45aba81a7b73a.png)

显示带有丰富超链接的搜索结果。图片由作者提供。

对于基于Python的搜索，我创建了一个`FiftyOneDocsSearch`类来封装文档搜索行为，因此一旦实例化了`FiftyOneDocsSearch`对象（可能使用默认的搜索参数设置）：

```py
from fiftyone.docs_search import FiftyOneDocsSearch
fosearch = FiftyOneDocsSearch(open_url=False, top_k=3, score=True)
```

你可以通过调用这个对象在Python中进行搜索。例如，要查询“如何加载数据集”，只需运行：

```py
fosearch(“How to load a dataset”)
```

![](../Images/3c701dc79f3dbded374c760c96741967.png)

在Python进程中对公司的文档进行语义搜索。图片由作者提供。

我还使用了[argparse](https://docs.python.org/3/library/argparse.html)来通过命令行提供此文档搜索功能。当包被安装后，可以通过CLI搜索文档：

```py
fiftyone-docs-search query "<my-query>" <args 
```

仅为有趣，因为`fiftyone-docs-search query`有点繁琐，我在我的`.zsrch`文件中添加了一个别名：

```py
alias fosearch='fiftyone-docs-search query'
```

使用这个别名，可以通过命令行搜索文档：

```py
fosearch "<my-query>" args
```

# 结论

在开始之前，我已经把自己看作是公司开源Python库FiftyOne的重度用户。我编写了许多文档，并且每天都在使用（并继续使用）这个库。但将我们的文档转变为可搜索的数据库的过程迫使我以更深刻的方式理解我们的文档。当你为他人构建某些东西时，它也能帮助到你，这总是很棒的！

这是我学到的：

+   **Sphinx RST使用繁琐**：它能生成美观的文档，但解析起来有点麻烦

+   **不要在预处理上过度**：OpenAI的text-embeddings-ada-002模型非常擅长理解文本字符串背后的意义，即使它有稍微不典型的格式。再也不用进行词干提取和费力地去除停用词及各种字符了。

+   **小的语义有意义的片段最好**：将文档分解成尽可能小的有意义的部分，并保留上下文。对于较长的文本，搜索查询与索引中文本部分的重叠更可能被段落中的不相关文本遮掩。如果你将文档拆分得过小，许多索引条目可能包含很少的语义信息。

+   **向量搜索非常强大**：只需最少的工作量，不需任何微调，我就能显著提高文档的可搜索性。从初步估计来看，这种改进的文档搜索比旧的关键字搜索方法返回相关结果的可能性提高了两倍以上。此外，这种向量搜索方法的语义特性意味着用户现在可以用任意措辞、任意复杂的查询进行搜索，并且保证得到指定数量的结果。

如果你发现自己（或他人）不断地挖掘或筛选大量文档以寻找特定信息，我鼓励你将这一过程调整到你的用例中。你可以修改它以适用于你的个人文档或公司的档案。如果你这样做，我保证你会从中得到新的启发！

这里有一些方法可以扩展到你自己的文档中！

+   [混合搜索](https://www.pinecone.io/learn/hybrid-search-intro/): 将向量搜索与传统的关键字搜索结合起来

+   全球化：使用[Qdrant Cloud](https://cloud.qdrant.io/)在云中存储和查询集合

+   引入网络数据：使用[requests](https://pypi.org/project/requests/)直接从网络下载HTML

+   自动更新：使用[Github Actions](https://docs.github.com/en/actions)在底层文档更改时触发嵌入的重新计算

+   嵌入：将其包装在一个Javascript元素中，替代传统的搜索框

构建包所用的所有代码都是开源的，可以在[voxel51/fiftyone-docs-search](https://github.com/voxel51/fiftyone-docs-search)仓库中找到。
