# 重新创建 Andrej Karpathy 的周末项目 — 电影搜索引擎

> 原文：[https://towardsdatascience.com/recreating-andrej-karpathys-weekend-project-a-movie-search-engine-9b270d7a92e4?source=collection_archive---------3-----------------------#2023-11-07](https://towardsdatascience.com/recreating-andrej-karpathys-weekend-project-a-movie-search-engine-9b270d7a92e4?source=collection_archive---------3-----------------------#2023-11-07)

## 使用 OpenAI 嵌入和向量数据库构建电影推荐系统

[](https://medium.com/@iamleonie?source=post_page-----9b270d7a92e4--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----9b270d7a92e4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9b270d7a92e4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9b270d7a92e4--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----9b270d7a92e4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecreating-andrej-karpathys-weekend-project-a-movie-search-engine-9b270d7a92e4&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----9b270d7a92e4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b270d7a92e4--------------------------------) ·9分钟阅读·2023年11月7日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9b270d7a92e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecreating-andrej-karpathys-weekend-project-a-movie-search-engine-9b270d7a92e4&source=-----9b270d7a92e4---------------------bookmark_footer-----------)![](../Images/e9469f6553eb0ffb4b927e45e7e6705c.png)

[最终电影推荐演示](https://awesome-moviate.weaviate.io/) 的风格化截图（图片由作者提供）

在2023年4月，[Andrej Karpathy](https://karpathy.ai/)，OpenAI 的创始成员之一及前特斯拉人工智能总监，分享了这个有趣的周末项目，一个 [电影搜索和推荐引擎](https://awesome-movies.life/)：

用户界面非常简单，主要有两个关键功能。首先，你有一个搜索框，可以通过电影标题进行搜索。然后点击任何电影，你会得到一份该电影的 40 部最相似电影的推荐列表。

![](../Images/6cfd01c37e7c5bf45d28f50e5949f88d.png)

演示网站：[https://awesome-movies.life/](https://awesome-movies.life/)

尽管很受欢迎，Karpathy 不幸地没有公开分享该项目的源代码。

![](../Images/211d985776d7aa12c6d29ae98eb6334c.png)

原推文下的 [评论截图](https://twitter.com/karpathy/status/1647644308647071745)（截图由作者提供）

所以，拿上一些爆米花，让我们自己动手重建吧！

# 前提条件

该项目基于四个主要组件：

+   OpenAI 嵌入模型生成嵌入

+   使用 Weaviate [向量数据库](https://medium.com/towards-data-science/explaining-vector-databases-in-3-levels-of-difficulty-fc392e48ab78) 来存储嵌入，数据通过 Python 脚本填充

+   前端：HTML、CSS、Js

+   后端：NodeJs

因此，要跟随本教程，你需要以下内容：

+   **Python** 用于数据处理和填充向量数据库

+   **Docker 和 Docker-Compose** 用于在本地运行向量数据库。

+   **Node.js 和 npm** 用于在本地运行应用程序。

+   **OpenAI API 密钥** 用于访问 OpenAI 嵌入模型

# 实现一个电影搜索引擎

本节分析 Karpathy 的周末黑客活动，并旨在以一些小变化重建它。要构建一个简单的电影搜索引擎，请按照以下步骤操作：

+   [准备工作：电影数据集](#f305)

+   [步骤 1：生成和存储嵌入](#553d)

+   [步骤 2：搜索电影](#d1dc)

+   [步骤 3：获取类似的电影推荐](#5fc7)

+   [步骤 4：运行演示](#86ba)

完整代码是开源的，你可以在 [GitHub](https://github.com/weaviate-tutorials/awesome-moviate) 上找到它。

## 准备工作：电影数据集

Karpathy 的项目索引了自 1970 年以来的 11,762 部电影，包括来自 Wikipedia 的情节和摘要。

若想在不手动抓取 Wikipedia 的情况下实现类似功能，你可以使用以下两个来自 Kaggle 的数据集：

+   [48,000+ 电影数据集](https://www.kaggle.com/datasets/yashgupta24/48000-movies-dataset)（许可：[CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)）包括 `'id'`、`'name'`、`'PosterLink'`、`'Genres'`、`'Actors'`、`'Director'`、`'Description'`、`'Keywords'` 和 `'DatePublished'` 列。

+   [Wikipedia 电影情节](https://www.kaggle.com/datasets/jrobischon/wikipedia-movie-plots)（许可：[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)），用于 `'plot'` 列。

两个数据集在电影标题和发行年份上合并，然后筛选出1970年后发行的电影。你可以在`[add_data.py](https://github.com/weaviate-tutorials/awesome-moviate/blob/main/add_data.py)`文件中找到详细的预处理步骤。结果数据框包含大约35,000部电影，其中约8,500部电影有情节描述，数据框如下所示：

![](../Images/a1bcfc5105b915beac48139e6b91844a.png)

预处理后的电影数据框（截图由作者提供）

## 步骤1：生成并存储嵌入

这个演示项目的核心是电影数据对象的嵌入，这些嵌入主要用于通过情节相似度推荐电影。在Karpathy的项目中，为电影摘要和情节生成了向量嵌入。生成向量嵌入有两个选项：

+   术语频率-逆文档频率（TF-IDF），这是一种简单的二元组，应该用于单个词的使用。

+   `[text-embedding-ada-002](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)` [OpenAI的嵌入模型](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)，应该用于语义相似度。

此外，相似度是基于每部电影的维基百科摘要和情节计算的，具有两种相似度排序选择：

+   k-最近邻（kNN）使用余弦相似度

+   支持向量机

Karpathy建议将`[text-embedding-ada-002](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)`和kNN结合使用，以获得一个好的快速默认设置。

最后但同样重要的是，如这条臭名昭著的回应所述，向量嵌入存储在`np.array`中：

![](../Images/2bd195ca70bea5862e395d4a06782b09.png)

原推文下的[评论](https://twitter.com/karpathy/status/1647374645316968449)的截图（截图由作者提供）

在这个项目中，我们还将使用来自OpenAI的`[text-embedding-ada-002](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)`嵌入模型，但将向量嵌入存储在向量数据库中。

也就是说，我们将使用[Weaviate](https://weaviate.io/)*，一个开源的向量数据库。虽然我可以争论说，向量数据库比将嵌入存储在`np.array`中要快得多，因为它们使用向量索引，但坦率地说：在这个规模（数千个）下，你不会注意到速度上的任何差异。我使用向量数据库的主要原因是Weaviate有许多方便的内置功能可以立即使用，比如使用嵌入模型的自动向量化。

首先，如`[add_data.py](https://github.com/weaviate-tutorials/awesome-moviate/blob/main/add_data.py)`文件所示，你需要设置你的Weaviate客户端，它连接到本地的Weaviate数据库实例，如下所示。此外，你将定义你的OpenAI API密钥，以启用集成OpenAI模块的使用。

```py
# pip weaviate-client
import weaviate
import os

openai_key = os.environ.get("OPENAI_API_KEY", "")

# Setting up client
client = weaviate.Client(
    url = "http://localhost:8080",
    additional_headers={
         "X-OpenAI-Api-Key": openai_key,
    })
```

接下来，你将定义一个名为`Movies`的数据集合来存储电影数据对象，这类似于在关系数据库中创建一个表。在这一步中，你定义`text2vec-openai`模块作为向量化器，它允许在导入和查询时自动进行数据向量化，并在模块设置中定义使用`[text-embedding-ada-002](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)`嵌入模型。此外，你可以定义余弦距离作为相似性度量。

```py
movie_class_schema = {
    "class": "Movies",
    "description": "A collection of movies since 1970.",
    "vectorizer": "text2vec-openai",
    "moduleConfig": {
        "text2vec-openai": {
            "vectorizeClassName": False,
            "model": "ada",
            "modelVersion": "002",
            "type": "text"
        },
    },
    "vectorIndexConfig": {"distance" : "cosine"},
}
```

接下来，你定义电影数据对象的属性以及为哪些属性生成向量嵌入。在下面的简化代码片段中，你可以看到，对于属性`movie_id`和`title`，没有生成向量嵌入，因为向量化模块的设置为`"skip" : True`。这是因为，我们只希望为`description`和`plot`生成向量嵌入。

```py
movie_class_schema["properties"] = [
        {
            "name": "movie_id",
            "dataType": ["number"],
            "description": "The id of the movie", 
            "moduleConfig": {
                "text2vec-openai": {  
                    "skip" : True,
                    "vectorizePropertyName" : False
                }
            }        
        },
        {
            "name": "title",
            "dataType": ["text"],
            "description": "The name of the movie", 
            "moduleConfig": {
                "text2vec-openai": {  
                    "skip" : True,
                    "vectorizePropertyName" : False
                }
            }   
        },
        # shortened for brevity ...
        {
            "name": "description",
            "dataType": ["text"],
            "description": "overview of the movie", 
        },
        {
            "name": "Plot",
            "dataType": ["text"],
            "description": "Plot of the movie from Wikipedia", 
        },
    ]

# Create class
client.schema.create_class(movie_class_schema)
```

最后，你定义一个批处理过程来填充向量数据库：

```py
# Configure batch process - for faster imports 
client.batch.configure(batch_size=10)

# Importing the data
for i in range(len(df)):
    item = df.iloc[i]

    movie_object = {
        'movie_id':float(item['id']),
        'title': str(item['Name']).lower(),
        # shortened for brevity ...
        'description':str(item['Description']),
        'plot': str(item['Plot']),
    }

    client.batch.add_data_object(movie_object, "Movies")
```

## 第二步：搜索电影

在 Karpathy 的项目中，搜索栏是一个简单的基于关键字的搜索，尝试逐字匹配你的确切查询与电影标题。当一些人表示他们希望搜索能够进行语义搜索时，Karpathy 同意这可能是项目的一个很好的扩展：

![](../Images/5c0db08fd09133d39658b214d1ae353d.png)

原始推文下的[评论](https://twitter.com/karpathy/status/1647376961902366720)截图（作者截图）

在这个项目中，你将在`[queries.js](https://github.com/weaviate-tutorials/awesome-moviate/blob/main/queries.js)`文件中启用三种类型的搜索：

+   基于关键字的搜索 ([BM25](https://weaviate.io/developers/weaviate/search/bm25)),

+   语义搜索，以及

+   [混合搜索](https://weaviate.io/developers/weaviate/search/hybrid)，它是基于关键字的搜索和语义搜索的结合。

每个搜索都会返回`num_movies = 20`部电影，属性为`['title', 'poster_link', 'genres', 'year', 'director', 'movie_id']`。

要启用**基于关键字的搜索**，你可以使用`.withBm25()`搜索查询，涉及的属性为`['title', 'director', 'genres', 'actors', 'keywords', 'description', 'plot']`。你可以通过指定`'title^3'`来给属性`'title'`更大的权重。

```py
async function get_keyword_results(text) {
    let data = await client.graphql
        .get()
        .withClassName('Movies')
        .withBm25({query: text,
            properties: ['title^3', 'director', 'genres', 'actors', 'keywords', 'description', 'plot'],
        })
        .withFields(['title', 'poster_link', 'genres', 'year', 'director', 'movie_id'])
        .withLimit(num_movies)
        .do()
        .then(info => {
            return info
        })
        .catch(err => {
            console.error(err)
        })
    return data;
}
```

要启用**语义搜索**，你可以使用`.withNearText()`搜索查询。这将自动将搜索查询向量化，并检索其在向量空间中最接近的电影。

```py
async function get_semantic_results(text) {
    let data = await client.graphql
        .get()
        .withClassName('Movies')
        .withFields(['title', 'poster_link', 'genres', 'year', 'director', 'movie_id'])
        .withNearText({concepts: [text]})
        .withLimit(num_movies)
        .do()
        .then(info => {
            return info
        })
        .catch(err => {
            console.error(err)
        });
        return data;
}
```

要启用**混合搜索**，你可以使用`.withHybrid()`搜索查询。`alpha : 0.5`表示关键字搜索和语义搜索的权重相等。

```py
async function get_hybrid_results(text) {
    let data = await client.graphql
        .get()
        .withClassName('Movies')
        .withFields(['title', 'poster_link', 'genres', 'year', 'director', 'movie_id'])
        .withHybrid({query: text, alpha: 0.5})
        .withLimit(num_movies)
        .do()
        .then(info => {
            return info
        })
        .catch(err => {
            console.error(err)
        });
    return data;
}
```

## 第三步：获取类似电影推荐

要获取类似的电影推荐，你可以执行 `.withNearObject()` 搜索查询，如 `[queries.js](https://github.com/weaviate-tutorials/awesome-moviate/blob/main/queries.js)` 文件中所示。通过传递电影的 `id`，查询返回向量空间中与给定电影最接近的 `num_movies = 20` 部分电影。

```py
async function get_recommended_movies(mov_id) {
    let data = await client.graphql
        .get()
        .withClassName('Movies')
        .withFields(['title', 'genres', 'year', 'poster_link', 'movie_id'])
        .withNearObject({id: mov_id})
        .withLimit(20)
        .do()
        .then(info => {
            return info;
        })
        .catch(err => {
            console.error(err)
        });
    return data;
}
```

## 第4步：运行演示

最后，将一切包装成一个具有2000年代标志性 [GeoCities](https://en.wikipedia.org/wiki/GeoCities) 美学的网页应用（我不打算让你厌倦前端的内容），然后就完成了！你已准备好！

要在本地运行演示，克隆 [GitHub 仓库](https://github.com/weaviate-tutorials/awesome-moviate)。

```py
git clone git@github.com:weaviate-tutorials/awesome-moviate.git
```

导航到演示的目录并设置一个虚拟环境。

```py
python -m venv .venv             
source .venv/bin/activate
```

确保 [在你的虚拟环境中设置环境变量 $OPENAI_API_KEY。](https://stackoverflow.com/questions/9554087/setting-an-environment-variable-in-virtualenv) 此外，在目录中运行以下命令，以安装虚拟环境中所需的所有依赖项。

```py
pip install -r requirements.txt
```

接下来，在 `docker-compose.yml` 文件中设置你的 OPENAI_API_KEY，并运行以下命令通过 Docker 本地运行 Weaviate。

```py
docker compose up -d
```

一旦你的 Weaviate 实例启动并运行，运行 `add_data.py` 文件以填充你的向量数据库。

```py
python add_data.py
```

在你运行应用程序之前，安装所有必需的节点模块。

```py
npm install
```

最后，运行以下命令在本地启动你的电影搜索引擎应用程序。

```py
npm run start
```

现在，导航到 [http://localhost:3000/](http://localhost:3000/) 并开始尝试你的应用程序。

# 摘要

本文重新创建了 Andrej Karpathy 的有趣周末项目，即电影搜索引擎/推荐系统。下面，你可以看到完成的 [实时演示](https://awesome-moviate.weaviate.io/) 的简短视频：

![](../Images/2b3edc4058d918016f21473cf8ff12bf.png)

演示网址 [https://awesome-moviate.weaviate.io/](https://awesome-moviate.weaviate.io/)

与原始项目相比，该项目使用向量数据库来存储嵌入。此外，搜索功能扩展到了语义和混合搜索。

如果你进行尝试，你会注意到它并不完美，但正如 Karpathy 所说：

> “它运行得~还不错，哈哈，需要再调试一下。”

你可以在 [GitHub](https://github.com/weaviate-tutorials/awesome-moviate) 上找到项目的开源代码，并根据需要进行修改。一些进一步改进的建议包括尝试向量化不同的属性、调整关键词搜索与语义搜索之间的权重，或用开源替代品替换嵌入模型。

# 喜欢这个故事吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时收到通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----9b270d7a92e4--------------------------------) [## 每当 Leonie Monigatti 发布时获取电子邮件。

### 每当Leonie Monigatti发布新内容时，会收到一封电子邮件。通过注册，如果你还没有Medium账户，你将创建一个…

[medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----9b270d7a92e4--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)、[*Twitter*](https://twitter.com/helloiamleonie) *和* [*Kaggle*](https://www.kaggle.com/iamleonie) *上找到我*！

# 免责声明

+   在撰写本文时，我是一名[Weaviate](https://weaviate.io/)的开发者倡导者，这是一个开源的[向量数据库](https://weaviate.io/blog/what-is-a-vector-database)。

+   这个项目并非原创想法：该项目的灵感来源于[Andrej Karpathy的周末项目](https://twitter.com/karpathy/status/1647372603907280896)，实现基于一个[旧的电影搜索引擎演示项目](https://github.com/weaviate/weaviate-examples/tree/main/movies-search-engine)。
