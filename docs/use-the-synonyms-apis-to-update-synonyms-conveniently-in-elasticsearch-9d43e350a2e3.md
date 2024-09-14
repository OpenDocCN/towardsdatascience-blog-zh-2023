# 使用同义词 API 方便地更新 Elasticsearch 中的同义词

> 原文：[`towardsdatascience.com/use-the-synonyms-apis-to-update-synonyms-conveniently-in-elasticsearch-9d43e350a2e3`](https://towardsdatascience.com/use-the-synonyms-apis-to-update-synonyms-conveniently-in-elasticsearch-9d43e350a2e3)

## 学习一种新的方法来更新 Elasticsearch 同义词，而无需重新索引或重新加载

[](https://lynn-kwong.medium.com/?source=post_page-----9d43e350a2e3--------------------------------)![Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----9d43e350a2e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9d43e350a2e3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9d43e350a2e3--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----9d43e350a2e3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9d43e350a2e3--------------------------------) ·阅读时间：5 分钟·2023 年 11 月 25 日

--

![](img/b3193155f751500d3dee5b98319411e9.png)

图片来源：Tumisu，Pixabay

Elasticsearch 的同义词功能非常强大，当正确使用时，可以显著提高搜索引擎的效率。使用同义词功能时，一个常见问题是更新同义词集。

在索引的设置中定义的内联同义词不能直接更新，我们需要关闭索引、更新设置，然后重新打开索引以使更改生效。另一种方法是使用可以通过重新加载索引来更新的同义词文件。然而，当 Elasticsearch 服务器分布式部署或托管在云端时，使用索引文件会很难管理。这是因为我们需要将文件放在所有集群节点上。

好消息是，现在还有第三种方法，比之前的两种方法要方便得多。我们现在可以使用同义词 API 来管理同义词。尽管在撰写本文时这仍然是 Elasticsearch 的一个测试功能，但我认为它很快会被采用，因为开发者对该功能的需求很高，而且它可以非常方便地解决更新同义词集的棘手问题。我们将探讨在本文中同义词 API 的常见用法。

## 准备工作

我们将使用以下 `docker-compose.yaml` 文件在本地启动 Elasticsearch 和 Kibana 以进行演示。

```py
version: "3.9"
services:
  elasticsearch:
    image: elasticsearch:8.11.1
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
      - xpack.security.enabled=false
    ports:
      - target: 9200
        published: 9200
    networks:
      - elastic

  kibana:
    image: kibana:8.11.1
    ports:
      - target: 5601
        published: 5601
    depends_on:
      - elasticsearch
    networks:
      - elastic      

networks:
  elastic:
    name: elastic
    driver: bridge
```

请注意，你需要至少使用 8.10.0 版本的 Elasticsearch 才能使用同义词 API。最新版本会是最佳选择，因为那个时候该功能应该已经更加成熟。

## 创建同义词集

当 Elasticsearch 和 Kibana 使用上述 `docker-compose.yaml` 文件启动时，我们可以前往 [`localhost:5601`](http://localhost:5601) 来管理 Elasticsearch 索引和同义词。

要使用同义词 API 管理同义词，我们需要先创建一个同义词集，然后才能在 Elasticsearch 索引中使用。

我们可以使用`_synonyms`端点通过`PUT`请求创建或更新同义词集：

```py
PUT _synonyms/inventory-synonyms-set
{
  "synonyms_set": [
    {
      "id": "synonym-1",
      "synonyms": "ps => playstation"
    },
    {
      "synonyms": "javascript,ecmascript,js"
    }
  ]
}
```

+   `products-synonyms-set`是用户定义的同义词集名称。

+   `synonyms_set`是请求体中必需的键，包括一个同义词规则数组。

+   每个同义词规则是一个包含可选`id`键和必需`synonyms`键的对象。如果未提供`id`，Elasticsearch 将创建一个标识符。`synonyms`的值是以前一篇文章中所用的 Solr 格式定义的规则。

## 使用同义词集创建 Elasticsearch 索引

当创建同义词集时，它可以在创建索引时用于`synonym`或`synonym_graph`令牌过滤器：

```py
 PUT /inventory
{
  "settings": {
    "index": {
      "analysis": {
        "analyzer": {
          "index_analyzer": {
            "tokenizer": "standard",
            "filter": [
              "lowercase"
            ]
          },
          "search_analyzer": {
            "tokenizer": "standard",
            "filter": [
              "lowercase",
              "synonym_filter"
            ]
          }
        },
        "filter": {
          "synonym_filter": {
            "type": "synonym_graph",
            "synonyms_set": "inventory-synonyms-set",
            "updateable": true
          }
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "name": {
        "type": "text",
        "analyzer": "index_analyzer",
        "search_analyzer": "search_analyzer"
      }
    }
  }
}
```

如我们所见，使用同义词集的配置与使用同义词文件非常相似，如这篇文章中详细说明的那样。我们只需将`synonyms_path`更改为`synonyms_set`。

## 测试同义词集

我们可以使用`_analyze`端点来分析一些文本，并测试在上一步中添加的同义词，使用 API 进行测试：

```py
GET /inventory/_analyze
{
  "analyzer": "search_analyzer",
  "text": "PS"
}

{
  "tokens": [
    {
      "token": "playstation",
      "start_offset": 0,
      "end_offset": 2,
      "type": "SYNONYM",
      "position": 0
    }
  ]
}

GET /inventory/_analyze
{
  "analyzer": "search_analyzer",
  "text": "JS"
}

{
  "tokens": [
    {
      "token": "javascript",
      ......
    },
    {
      "token": "ecmascript",
      ......
    },
    {
      "token": "js",
      ......
    }
  ]
}
```

这表明 API 添加的同义词工作正常。

## 更新同义词集

现在让我们使用同义词 API 更新同义词集。这正是同义词 API 真正出色的地方，因为我们不需要关闭、打开或重新加载相应的 Elasticsearch 索引，这极大地减轻了开发者的痛苦。

我们可以使用`PUT`方法整体更新同义词集。在这里必须非常小心，否则你会用只包含新同义词规则的新集替换原始集合，这在生产环境中非常具有破坏性。

让我们向`inventory-synonyms-set`添加一个新的同义词规则：

```py
PUT _synonyms/inventory-synonyms-set
{
  "synonyms_set": [
    {
      "id": "synonym-1",
      "synonyms": "ps => playstation"
    },
    {
      "synonyms": "javascript,ecmascript,js"
    },
    {
      "synonyms": "py => python"
    }
  ]
}
```

请注意，原始的同义词集也应在此处添加。

当现有的同义词集更新时，所有使用该同义词集的搜索分析器会自动重新加载，这可以在上面的`PUT`请求响应中看到：

```py
{
  "result": "updated",
  "reload_analyzers_details": {
    "_shards": {
      "total": 2,
      "successful": 1,
      "failed": 0
    },
    "reload_details": [
      {
        "index": "inventory",
        "reloaded_analyzers": [
          "search_analyzer"
        ],
        "reloaded_node_ids": [
          "MD0GUTvcQAOvsHdUIBunNw"
        ]
      }
    ]
  }
}
```

我们可以使用`_analyze`端点来测试新添加的同义词：

```py
GET /inventory/_analyze
{
  "analyzer": "search_analyzer",
  "text": "py"
}

{
  "tokens": [
    {
      "token": "python",
      "start_offset": 0,
      "end_offset": 2,
      "type": "SYNONYM",
      "position": 0
    }
  ]
}
```

是的，同义词集已成功更新，并且实时生效，无需关闭、打开或重新加载相应的索引。

此外，我们还可以通过直接添加规则来进行增量更新。在这种情况下，我们需要在路径中为规则指定一个`id`：

```py
PUT _synonyms/inventory-synonyms-set/synonym-ipod
{
  "synonyms": "i-pod, i pod ⇒ ipod"
}
```

你也可以通过其 id 删除同义词规则：

```py
 DELETE _synonyms/inventory-synonyms-set/synonym-ipod
```

单独更新同义词规则的工作方式与整体更新同义词集时相同。

## 监控同义词集

我们可以使用`GET`方法直接检索同义词集的内容：

```py
GET _synonyms/inventory-synonyms-set

{
  "count": 3,
  "synonyms_set": [
    {
      "id": "qP8w_osBr1bbhRIfbz1c",
      "synonyms": "javascript,ecmascript,js"
    },
    {
      "id": "qf8w_osBr1bbhRIfbz1c",
      "synonyms": "py => python"
    },
    {
      "id": "synonym-1",
      "synonyms": "ps => playstation"
    }
  ]
}
```

实际操作中，我们希望编写一些脚本来统计索引中的同义词数量，以确保如上所示的同义词没有被意外删除。下面的 Python 代码片段可以统计同义词集合中的同义词数量：

```py
from elasticsearch import Elasticsearch

es_client = Elasticsearch("http://localhost:9200")

es_client.synonyms.get_synonyms_sets()
# ObjectApiResponse({'count': 1, 'results': [{'synonyms_set': 'inventory-synonyms-set', 'count': 3}]})
```

在这项工作中，我们介绍了 Elasticsearch 同义词 API 的基础知识，并演示了如何方便地使用它来管理同义词。通过此功能，我们不再需要像处理内联同义词那样关闭和重新打开索引，也不需要像处理同义词文件那样手动重新加载索引。它可以使我们的搜索引擎更稳定，也让我们作为开发者的工作变得更加轻松。

## 相关文章

+   如何正确使用 Elasticsearch 的同义词功能

+   Elasticsearch 8 在 Python 中的重要语法更新
