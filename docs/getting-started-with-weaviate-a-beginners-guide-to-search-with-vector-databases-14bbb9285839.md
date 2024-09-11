# 开始使用 Weaviate：初学者的向量数据库搜索指南

> 原文：[https://towardsdatascience.com/getting-started-with-weaviate-a-beginners-guide-to-search-with-vector-databases-14bbb9285839?source=collection_archive---------1-----------------------#2023-07-18](https://towardsdatascience.com/getting-started-with-weaviate-a-beginners-guide-to-search-with-vector-databases-14bbb9285839?source=collection_archive---------1-----------------------#2023-07-18)

## 如何在 Python 中使用 OpenAI 和 Weaviate 进行语义搜索、问答和生成搜索

[](https://medium.com/@iamleonie?source=post_page-----14bbb9285839--------------------------------)[![Leonie Monigatti](../Images/4044b1685ada53a30160b03dc78f9626.png)](https://medium.com/@iamleonie?source=post_page-----14bbb9285839--------------------------------)[](https://towardsdatascience.com/?source=post_page-----14bbb9285839--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----14bbb9285839--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----14bbb9285839--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-weaviate-a-beginners-guide-to-search-with-vector-databases-14bbb9285839&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----14bbb9285839---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----14bbb9285839--------------------------------) ·12分钟阅读·2023年7月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F14bbb9285839&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-weaviate-a-beginners-guide-to-search-with-vector-databases-14bbb9285839&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----14bbb9285839---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F14bbb9285839&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgetting-started-with-weaviate-a-beginners-guide-to-search-with-vector-databases-14bbb9285839&source=-----14bbb9285839---------------------bookmark_footer-----------)![](../Images/6485587f0e926a7c26edc3221c1441f3.png)

如果你看到这篇文章，我假设你已经开始尝试构建使用大语言模型（LLM）的应用，并遇到了“向量数据库”这一术语。

伴随着构建 LLM 应用的工具快速增长，诸如[LangChain](https://python.langchain.com/docs/get_started/introduction.html)和[LlamaIndex](https://www.llamaindex.ai/)等工具正变得越来越受欢迎。

在[最近的一篇文章中，我描述了如何开始使用 LangChain](/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c)，在这篇文章中，我想通过玩弄[Weaviate](https://weaviate.io/)来继续探索 LLM 工具的景观。

# 什么是 Weaviate？

[Weaviate](https://weaviate.io/) 是一个开源向量数据库。它使你能够存储数据对象和向量嵌入，并根据相似性度量查询它们。

[## GitHub - weaviate/weaviate: Weaviate 是一个开源向量数据库，既存储对象也存储向量…](https://github.com/weaviate/weaviate?source=post_page-----14bbb9285839--------------------------------)

### Weaviate 是一个开源向量数据库，存储对象和向量，允许进行向量搜索的结合…

[github.com](https://github.com/weaviate/weaviate?source=post_page-----14bbb9285839--------------------------------)

向量数据库自从 LLM 媒体关注度上升以来，受到了大量关注。可能最流行的向量数据库在 LLM 上下文中的应用是“为 LLM 提供长期记忆”。

如果你需要重新了解向量数据库的概念，你可能想查看我之前的文章：

[## 解释向量数据库的三种难度级别](https://explaining-vector-databases-in-3-levels-of-difficulty-fc392e48ab78?source=post_page-----14bbb9285839--------------------------------)

### 从新手到专家：解密不同背景下的向量数据库

[towardsdatascience.com](/explaining-vector-databases-in-3-levels-of-difficulty-fc392e48ab78?source=post_page-----14bbb9285839--------------------------------)

在本教程中，我们将演示如何用数据集的嵌入填充 Weaviate 向量数据库。然后，我们将介绍三种从中检索信息的方法：

+   [向量搜索](#abaf)

+   [问答系统](#cf85)

+   [生成搜索](#a30a)

# 前提条件

要跟随本教程，你需要具备以下条件：

+   Python 3 环境

+   [OpenAI](https://openai.com/) API 密钥（或选择 Hugging Face、Cohere 或 PaLM 的 API 密钥）

*关于 API 密钥的说明：* 在本教程中，我们将通过一个推理服务（在这种情况下是[OpenAI](https://openai.com/)）从文本中生成嵌入。根据你使用的推理服务，请确保检查提供商的定价页面以避免意外费用。例如，使用的[阿达模型（版本 2）在撰写时的费用为每 1,000 个令牌 $0.0001](https://openai.com/pricing)，在本教程中推理费用不到 1 美分。

# 设置

你可以在自己的实例上运行 Weaviate（使用 Docker、Kubernetes 或嵌入式 Weaviate），也可以通过 Weaviate Cloud Services (WCS) 作为托管服务运行。对于本教程，我们将使用 WCS 运行 Weaviate 实例，因为这是推荐的最简单的方法。

## 如何使用 Weaviate Cloud Services (WCS) 创建集群

要能够使用该服务，你首先需要 [注册 WCS](https://console.weaviate.cloud/)。

注册后，你可以通过点击“创建集群”按钮来创建一个新的 Weaviate 集群。

![](../Images/cac79d7af6e30a06e44d88de4ee8bee6.png)

Weaviate Cloud Services 的截图

对于本教程，我们将使用免费的试用计划，该计划为你提供 14 天的沙盒。（你无需添加任何支付信息。相反，沙盒将在试用期结束后过期。但你可以随时创建一个新的免费试用沙盒。）

在“免费沙盒”标签下，进行以下设置：

1.  输入集群名称

1.  启用身份验证（设置为“是”）

![](../Images/db24341de07f83075b91e11d83f80aef.png)

Weaviate Cloud Services 计划的截图

最后，点击“创建”以创建你的沙盒实例。

## 如何在 Python 中安装 Weaviate

最后，但同样重要的是，使用 `pip` 将 `weaviate-client` 添加到你的 Python 环境中

```py
$ pip install weaviate-client
```

并导入库：

```py
import weaviate
```

## **如何通过客户端访问 Weaviate 集群**

对于下一步，你将需要以下两项信息来访问你的集群：

+   集群 URL

+   Weaviate API 密钥（在“启用 — 身份验证”下）

![](../Images/5f272ff38d248ed5e5f866d4e61f3c37.png)

Weaviate Cloud Services 沙盒的截图

现在，你可以实例化一个 Weaviate 客户端以访问你的 Weaviate 集群，如下所示。

```py
auth_config = weaviate.AuthApiKey(api_key="YOUR-WEAVIATE-API-KEY")  # Replace w/ your Weaviate instance API key

# Instantiate the client
client = weaviate.Client(
    url="https://<your-sandbox-name>.weaviate.network", # Replace w/ your Weaviate cluster URL
    auth_client_secret=auth_config,
    additional_headers={
        "X-OpenAI-Api-Key": "YOUR-OPENAI-API-KEY", # Replace with your OpenAI key
        }
)
```

如你所见，我们使用`additional_headers`下的 OpenAI API 密钥来访问嵌入模型。如果你使用的是不同于 OpenAI 的提供者，请将密钥参数更改为以下之一：`X-Cohere-Api-Key`，`X-HuggingFace-Api-Key` 或 `X-Palm-Api-Key`。

要检查是否一切设置正确，请运行：

```py
client.is_ready()
```

如果返回`True`，你已为下一步做好准备。

# 如何创建和填充 Weaviate 向量数据库

现在，我们准备在 Weaviate 中创建一个向量数据库并用一些数据填充它。

对于本教程，我们将使用 Kaggle 上的 [200,000+ Jeopardy 问题](https://www.kaggle.com/datasets/ulrikthygepedersen/200000-jeopardy-questions) 数据集的前 100 行 [1]。

```py
import pandas as pd

df = pd.read_csv("your_file_path.csv", nrows = 100)
```

![](../Images/0537b31ea6ffbae1f3adae0b122ec306.png)

Kaggle 上的 [200,000+ Jeopardy 问题](https://www.kaggle.com/datasets/ulrikthygepedersen/200000-jeopardy-questions) 数据集的前几行 [1]。

*关于令牌数量和相关成本的说明：在以下示例中，我们将为前 100 行嵌入“category”、“question”和“answer”列。根据与* `*tiktoken*` *库的计算，这将产生大约 3,000 个令牌用于嵌入，这* [*大约会导致 OpenAI 的 Ada 模型（版本 2）在 2023 年 7 月的推理成本为 $0.0003*](https://openai.com/pricing)*。*

## 第一步：创建模式

首先，我们需要定义底层数据结构和一些配置：

+   `class`：这个向量空间中的对象集合将被称为什么？

+   `properties`：对象的属性，包括属性名称和数据类型。在Pandas Dataframe类比中，这些就是DataFrame中的列。

+   `vectorizer`：生成嵌入的模型。对于文本对象，通常你会选择一个`[text2vec](https://weaviate.io/developers/weaviate/modules/retriever-vectorizer-modules)`模块（`text2vec-cohere`、`text2vec-huggingface`、`text2vec-openai`或`text2vec-palm`），具体选择取决于你使用的提供商。

+   `moduleConfig`：在这里，你可以定义所用模块的详细信息。例如，向量化器是一个模块，你可以定义使用哪个模型和版本。

```py
class_obj = {
    # Class definition
    "class": "JeopardyQuestion",

    # Property definitions
    "properties": [
        {
            "name": "category",
            "dataType": ["text"],
        },
        {
            "name": "question",
            "dataType": ["text"],
        },
        {
            "name": "answer",
            "dataType": ["text"],
        },
    ],

    # Specify a vectorizer
    "vectorizer": "text2vec-openai",

    # Module settings
    "moduleConfig": {
        "text2vec-openai": {
            "vectorizeClassName": False,
            "model": "ada",
            "modelVersion": "002",
            "type": "text"
        },
    },
}
```

在上述模式中，你可以看到我们将创建一个名为`"JeopardyQuestion"`的类，具有三个文本属性`"category"`、`"question"`和`"answer"`。我们使用的向量化器是OpenAI的Ada模型（版本2）。所有属性都将被向量化，但类名（`"vectorizeClassName" : False`）不会。如果你有不希望嵌入的属性，可以指定这一点（见[文档](https://weaviate.io/developers/academy/zero_to_mvp/schema_and_imports/schema)）。

一旦定义了模式，你可以使用`create_class()`方法创建类。

```py
client.schema.create_class(class_obj)
```

要检查类是否已成功创建，你可以如下查看其模式：

```py
client.schema.get("JeopardyQuestion")
```

创建的模式如下所示：

```py
{
  "class": "JeopardyQuestion",
  "invertedIndexConfig": {
    "bm25": {
      "b": 0.75,
      "k1": 1.2
    },
    "cleanupIntervalSeconds": 60,
    "stopwords": {
      "additions": null,
      "preset": "en",
      "removals": null
    }
  },
  "moduleConfig": {
    "text2vec-openai": {
      "model": "ada",
      "modelVersion": "002",
      "type": "text",
      "vectorizeClassName": false
    }
  },
  "properties": [
    {
      "dataType": [
        "text"
      ],
      "indexFilterable": true,
      "indexSearchable": true,
      "moduleConfig": {
        "text2vec-openai": {
          "skip": false,
          "vectorizePropertyName": false
        }
      },
      "name": "category",
      "tokenization": "word"
    },
    {
      "dataType": [
        "text"
      ],
      "indexFilterable": true,
      "indexSearchable": true,
      "moduleConfig": {
        "text2vec-openai": {
          "skip": false,
          "vectorizePropertyName": false
        }
      },
      "name": "question",
      "tokenization": "word"
    },
    {
      "dataType": [
        "text"
      ],
      "indexFilterable": true,
      "indexSearchable": true,
      "moduleConfig": {
        "text2vec-openai": {
          "skip": false,
          "vectorizePropertyName": false
        }
      },
      "name": "answer",
      "tokenization": "word"
    }
  ],
  "replicationConfig": {
    "factor": 1
  },
  "shardingConfig": {
    "virtualPerPhysical": 128,
    "desiredCount": 1,
    "actualCount": 1,
    "desiredVirtualCount": 128,
    "actualVirtualCount": 128,
    "key": "_id",
    "strategy": "hash",
    "function": "murmur3"
  },
  "vectorIndexConfig": {
    "skip": false,
    "cleanupIntervalSeconds": 300,
    "maxConnections": 64,
    "efConstruction": 128,
    "ef": -1,
    "dynamicEfMin": 100,
    "dynamicEfMax": 500,
    "dynamicEfFactor": 8,
    "vectorCacheMaxObjects": 1000000000000,
    "flatSearchCutoff": 40000,
    "distance": "cosine",
    "pq": {
      "enabled": false,
      "bitCompression": false,
      "segments": 0,
      "centroids": 256,
      "encoder": {
        "type": "kmeans",
        "distribution": "log-normal"
      }
    }
  },
  "vectorIndexType": "hnsw",
  "vectorizer": "text2vec-openai"
}
```

## 步骤2：将数据导入Weaviate

在此阶段，向量数据库已有模式但仍为空。因此，让我们用数据集填充它。这一过程也称为“upserting”。

我们将以每批200条的方式进行数据的upsert。如果你注意到了，你会知道这里不需要这样做，因为我们只有100行数据。但一旦你准备好进行更大规模的数据upsert，你会希望以批量方式进行。这就是为什么我在这里留下批处理代码：

```py
from weaviate.util import generate_uuid5

with client.batch(
    batch_size=200,  # Specify batch size
    num_workers=2,   # Parallelize the process
) as batch:
    for _, row in df.iterrows():
        question_object = {
            "category": row.category,
            "question": row.question,
            "answer": row.answer,
        }
        batch.add_data_object(
            question_object,
            class_name="JeopardyQuestion",
            uuid=generate_uuid5(question_object)
        )
```

尽管Weaviate会自动生成一个通用唯一标识符（`uuid`），我们将通过`question_object`的`generate_uuid5()`函数手动生成`uuid`，以避免导入重复的项目。

进行完整性检查时，你可以使用以下代码片段查看导入对象的数量：

```py
client.query.aggregate("JeopardyQuestion").with_meta_count().do()
```

```py
{'data': {'Aggregate': {'JeopardyQuestion': [{'meta': {'count': 100}}]}}}
```

# 如何查询Weaviate向量数据库

你最常做的操作是检索对象。要检索对象，你可以使用`get()`函数查询Weaviate向量数据库：

```py
client.query.get(
    <Class>,
    [<properties>]
).<arguments>.do()
```

+   `Class`：指定要检索的对象类的名称。这里是`"JeopardyQuestion"`。

+   `properties`：指定要检索的对象属性。这里包括`"category"`、`"question"`和`"answer"`中的一个或多个。

+   `arguments`：指定检索对象的搜索标准，例如限制条件或聚合。在以下示例中，我们将介绍一些这些标准。

让我们用`get()`函数从`JeopardyQuestion`类中检索一些条目，以查看它们的样子。在Pandas的类比中，你可以将以下内容视为`df.head(2)`。由于`get()`函数的响应是JSON格式的，我们将导入相关库以以视觉上吸引人的格式显示结果。

```py
import json

res = client.query.get("JeopardyQuestion", 
                      ["question", "answer", "category"])
                  .with_additional(["id", "vector"])
                  .with_limit(2)
                  .do()

print(json.dumps(res, indent=4))
```

```py
{
    "data": {
        "Get": {
            "JeopardyQuestion": [
                {
                    "_additional": {
                        "id": "064fee53-f8fd-4513-9294-432170cc9f77",
                        "vector": [ -0.02465364, ...] # Vector is trunkated for better readability 
                    },
                    "answer": "(Lou) Gehrig",
                    "category": "ESPN's TOP 10 ALL-TIME ATHLETES",
                    "question": "No. 10: FB/LB for Columbia U. in the 1920s; MVP for the Yankees in '27 & '36; \"Gibraltar in Cleats\""
                },
                {
                    "_additional": {
                        "id": "1041117a-34af-40a4-ad05-3dae840ad6b9",
                        "vector": [ -0.031970825, ...] # Vector is trunkated for better readability
                    },
                    "answer": "Jim Thorpe",
                    "category": "ESPN's TOP 10 ALL-TIME ATHLETES",
                    "question": "No. 2: 1912 Olympian; football star at Carlisle Indian School; 6 MLB seasons with the Reds, Giants & Braves"
                },
            ]
        }
    }
}
```

在上述代码片段中，你可以看到我们从`"JeopardyQuestion"`类中检索对象。我们指定了检索属性`"category"`、`"question"`和`"answer"`。

我们指定了两个额外的`arguments`：首先，我们使用`.with_additional()`参数指定检索关于对象ID和向量嵌入的额外信息。然后，使用`.with_limit(2)`参数，我们指定只检索两个对象。这种限制很重要，你将在后续示例中再次看到。这是因为从矢量数据库中检索对象不会返回完全匹配的结果，而是基于相似性返回对象，这需要通过阈值来限制。

## 矢量搜索

现在，我们准备进行一些矢量搜索！从矢量数据库中检索信息的酷炫之处在于，你可以例如要求它检索与`"concepts"`相关的Jeopardy问题。

为此，我们可以使用`.with_near_text()`参数，并传入我们感兴趣的`"concepts"`，如下所示：

```py
res = client.query.get(
    "JeopardyQuestion",
    ["question", "answer", "category"])\
    .with_near_text({"concepts": "animals"})\
    .with_limit(2)\
    .do()
```

指定的`vectorizer`将输入文本（`"animals”`）转换为向量嵌入，并检索两个最接近的结果：

```py
{
    "data": {
        "Get": {
            "JeopardyQuestion": [
                {
                    "answer": "an octopus",
                    "category": "SEE & SAY",
                    "question": "Say the name of <a href=\"http://www.j-archive.com/media/2010-07-06_DJ_26.jpg\" target=\"_blank\">this</a> type of mollusk you see"
                },
                {
                    "answer": "the ant",
                    "category": "3-LETTER WORDS",
                    "question": "In the title of an Aesop fable, this insect shared billing with a grasshopper"
                }
            ]
        }
    }
}
```

你可以看到这有多酷：我们看到矢量搜索返回了两个问题，其答案都是动物，但来自两个完全不同的类别。使用传统的关键词搜索，你必须首先定义一个动物列表，然后检索所有包含其中一个定义动物的问题。

## 问答功能

问答是将LLMs与矢量数据库结合使用时最受欢迎的示例之一。

要启用问答功能，你需要在模块配置中指定一个矢量化器（你应该已经有了）和一个问答模块，如下例所示：

```py
# Module settings
    "moduleConfig": {
        "text2vec-openai": {
          ...
        },
        "qna-openai": {
          "model": "text-davinci-002"
        }
    },
```

对于问答功能，你需要添加`with_ask()`参数，并且检索`_additional`属性。

```py
ask = {
  "question": "Which animal was mentioned in the title of the Aesop fable?",
  "properties": ["answer"]
}

res = (
  client.query
  .get("JeopardyQuestion", [
      "question",
      "_additional {answer {hasAnswer property result} }"
  ])
  .with_ask(ask)
  .with_limit(1)
  .do()
)
```

上述代码会查找所有可能包含问题`"Which animal was mentioned in the title of the Aesop fable?"`答案的提问，并返回答案`"The ant"`。

```py
{
    "JeopardyQuestion": [
        {
            "_additional": {
                "answer": {
                    "hasAnswer": true,
                    "property": "",
                    "result": " The ant"
                }
            },
            "question": "In the title of an Aesop fable, this insect shared billing with a grasshopper"
        }
    ]
}
```

## 生成搜索

通过结合LLMs，你还可以在返回搜索结果之前转换数据。这一概念称为生成搜索。

要启用生成搜索，你需要在模块配置中指定一个生成模块，如下例所示：

```py
# Module settings
    "moduleConfig": {
        "text2vec-openai": {
          ...
        },
        "generative-openai": {
          "model": "gpt-3.5-turbo"
        }
    },
```

对于生成搜索，你只需在之前的矢量搜索代码中添加`with_generate()`参数，如下所示：

```py
res = client.query.get(
    "JeopardyQuestion", 
    ["question", "answer"])\
  .with_near_text({"concepts": ["animals"]})\
  .with_limit(1)\
  .with_generate(single_prompt= "Generate a question to which the answer is {answer}")\
  .do()
```

上述代码做了以下操作：

1.  搜索最接近`"animals"`概念的问题

1.  返回问题 `"说出你看到的这种软体动物的名字"`，答案是 `"章鱼"`

1.  为提示 `"生成一个答案是章鱼的问题"` 生成一个完成的结果：

```py
{
    "generate": {
        "error": null,
        "singleResult": "What sea creature has eight arms and is known for its intelligence and camouflage abilities?"
    }
}
```

# 摘要

LLM 领域的流行不仅带来了许多有趣的新开发者工具，如 LangChain 或 LLaMaIndex，还展示了如何利用现有工具（如向量数据库）来增强 LLM 驱动的应用程序的潜力。

在这篇文章中，我们开始尝试使用 Weaviate，不仅利用向量数据库进行向量搜索，还结合 LLM 进行问答和生成搜索。

如果你对更深入的教程感兴趣，我推荐查看这门全面的四部分课程，内容涉及向量数据库和 Weaviate：

[](https://weaviate.io/developers/academy/zero_to_mvp?source=post_page-----14bbb9285839--------------------------------) [## 1\. 从零到 MVP | Weaviate — 向量数据库

### 课程概述

weaviate.io](https://weaviate.io/developers/academy/zero_to_mvp?source=post_page-----14bbb9285839--------------------------------)

*免责声明：撰写本文时，我是 Weaviate 的开发者倡导者。*

# 喜欢这个故事吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以在我发布新故事时收到通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----14bbb9285839--------------------------------) [## 每当 Leonie Monigatti 发布时获取电子邮件通知。

### 每当 Leonie Monigatti 发布时获取电子邮件通知。通过注册，如果你还没有 Medium 账户，将创建一个…

medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----14bbb9285839--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)、[*Twitter*](https://twitter.com/helloiamleonie) *和* [*Kaggle*](https://www.kaggle.com/iamleonie) *上找到我！*

# 参考资料

## 数据集

[1] Ulrik Thyge Pedersen (2023). [200,000+ Jeopardy 问题](https://www.kaggle.com/datasets/ulrikthygepedersen/200000-jeopardy-questions) 在 Kaggle 数据集中。

**许可证：** 4.0 国际版（CC BY 4.0）

## 图片参考

如果没有另行说明，所有图片均由作者创作。

## 网络与文献

[2] Weaviate (2023). [Weaviate 文档](https://weaviate.io/developers/weaviate)（访问日期：2023年7月14日）
