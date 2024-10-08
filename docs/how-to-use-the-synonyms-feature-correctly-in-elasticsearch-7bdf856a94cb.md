# 如何在 Elasticsearch 中正确使用同义词功能

> 原文：[`towardsdatascience.com/how-to-use-the-synonyms-feature-correctly-in-elasticsearch-7bdf856a94cb`](https://towardsdatascience.com/how-to-use-the-synonyms-feature-correctly-in-elasticsearch-7bdf856a94cb)

## 学习简单但强大的同义词功能，以提高您的搜索质量

[](https://lynn-kwong.medium.com/?source=post_page-----7bdf856a94cb--------------------------------)![Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----7bdf856a94cb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7bdf856a94cb--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7bdf856a94cb--------------------------------) [Lynn G. Kwong](https://lynn-kwong.medium.com/?source=post_page-----7bdf856a94cb--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7bdf856a94cb--------------------------------) ·9 分钟阅读·2023 年 1 月 7 日

--

![](img/13d37f90a8d511dae87864d18c83d3ca.png)

图片由 Tumisu 在 Pixabay 上

同义词用于提高搜索质量并扩大匹配范围。例如，搜索“英格兰”的用户可能希望找到包含“英国”或“英国”等内容的文档，尽管这三个词完全不同。

Elasticsearch 中的同义词功能非常强大，如果正确实现，可以使您的搜索引擎更加健壮和强大。在本文中，我们将介绍实践中实现同义词功能的基本要点，并附上简单的代码片段。特别是，我们将介绍如何更新现有索引的同义词，这是一个相对高级的话题。

## 准备工作

我们将在本文中使用`docker-compose.yaml`文件在本地启动 Elasticsearch 服务器，并使用 Kibana 来管理索引和运行命令。

我们将在本文中使用的`docker-compose.yaml`文件具有以下内容，稍后我们将添加更多功能：

```py
version: "3.9"
services:
  elasticsearch:
    image: elasticsearch:8.5.3
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
      - xpack.security.enabled=false
    volumes:
      - type: volume
        source: es_data
        target: /usr/share/elasticsearch/data
    ports:
      - target: 9200
        published: 9200
    networks:
      - elastic

  kibana:
    image: kibana:8.5.3
    ports:
      - target: 5601
        published: 5601
    depends_on:
      - elasticsearch
    networks:
      - elastic      

volumes:
  es_data:
    driver: local

networks:
  elastic:
    name: elastic
    driver: bridge
```

下载此文件或创建一个名为`docker-compose.yaml`的新文件，并将上面的内容粘贴到其中。然后，您可以使用以下命令之一启动 Elasticsearch 和 Kibana：

```py
# In the same folder where docker-compose.yaml is located (Recommended).
docker-compose up -d

# If you are in a different folder or name the YAML file differently, 
# you would need to specify the path or the name, for example:
docker-compose -f ~/Downloads/docker-compose.yaml up -d
docker-compose -f docker-compose-elasticsearch.yaml up -d
```

## 使用标准的同义词标记过滤器和同义词列表

让我们首先使用标准的同义词标记过滤器和同义词列表创建一个索引。在 Kibana 中运行以下命令，我们将很快解释细节：

```py
PUT /inventory_synonym
{
  "settings": {
    "index": {
      "analysis": {
        "analyzer": {
          "index_analyzer": {
            "tokenizer": "standard",
            "filter": [
              "lowercase",
              "synonym_filter"
            ]
          }
        },
        "filter": {
          "synonym_filter": {
            "type": "synonym",
            "synonyms": [
              "PS => PlayStation",
              "Play Station => PlayStation"
            ]
          }
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "name": {
        "type": "text",
        "analyzer": "index_analyzer"
      }
    }
  }
}
```

关键点在这里：

1.  请注意`settings`的嵌套键级别。`settings` => `index` => `analysis` => `analyzer`/`filter`都是内置关键字。但是，`index_analyzer`和`synonym_filter`分别是自定义分析器和过滤器的自定义名称。

1.  我们需要创建一个`type`为`synonym`的自定义过滤器。使用`synonyms`选项明确提供了一系列同义词。通常情况下，这仅用于测试，因为更新同义词列表不方便，稍后我们将看到。

1.  此文章中使用了 Solr 同义词。对于此示例，使用了显式映射，这意味着`=>`左侧的标记将被替换为右侧的标记。稍后我们将使用等效的同义词，这意味着提供的标记被视为等效。

1.  `synonym_filter`被添加到名为`index_analyzer`的新自定义分析器的过滤器列表中。通常情况下，过滤器的顺序很重要。然而，对于同义词过滤器来说，它有点特殊，可能会让我们感到惊讶。在这个例子中，即使`synonym_filter`过滤器放在`lowercase`过滤器之后，该过滤器返回的标记也会传递给`lowercase`过滤器，从而也会变成小写。因此，您不需要在同义词列表或同义词文件中提供小写标记。

1.  最后，在文档的映射中，为`name`字段指定了自定义分析器。

要测试在索引中创建的分析器，我们可以调用`_analyze`端点：

```py
GET /inventory_synonym/_analyze
{
  "analyzer": "index_analyzer",
  "text": "PS 3"
}
```

我们可以看到，“PS”的标记被替换为指定的同义词，并且是小写的：

```py
{
  "tokens": [
    {
      "token": "playstation",
      "start_offset": 0,
      "end_offset": 2,
      "type": "SYNONYM",
      "position": 0
    },
    {
      "token": "3",
      "start_offset": 3,
      "end_offset": 4,
      "type": "<NUM>",
      "position": 1
    }
  ]
}
```

让我们向索引中添加一些文档，并测试其在搜索中是否正常工作：

```py
PUT /inventory_synonym/_doc/1
{
  "name": "PS 3"
}

PUT /inventory_synonym/_doc/2
{
  "name": "PlayStation 4"
}

PUT /inventory_synonym/_doc/3
{
  "name": "Play Station 5"
}
```

我们可以使用`match`关键字进行简单的搜索：

```py
GET /inventory_synonym/_search
{
  "query": {
    "match": {
      "name": "PS"
    }
  }
}
```

如果一切正常，应该返回所有三个文档，并且得分相同。

## 索引时间与搜索时间的同义词

如您所见，在上面的示例中，只创建了一个分析器，并且它用于索引和搜索。

在索引步骤中将同义词应用于所有文档是不鼓励的，因为它有一些主要的缺点：

+   在不重新索引所有内容的情况下，无法更新同义词列表，这在实践中非常低效。

+   搜索得分也会受到同义词标记的影响。

+   索引过程变得更耗时，索引会变得更大。对于小数据集来说可以忽略不计，但对于大数据集来说非常重要。

因此，最好只在搜索步骤中应用同义词，这样可以克服所有三个缺点。为此，我们需要为搜索创建一个新的分析器。

## 使用`search_analyzer`并应用搜索时同义词

在 Kibana 中运行以下命令以创建一个带有搜索时同义词的新索引：

```py
PUT /inventory_synonym_graph
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
            "synonyms": [
              "PS => PlayStation",
              "Play Station => PlayStation"
            ]
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

关键要点：

+   类型现在更改为`synonym_graph`，它是一个更复杂的同义词过滤器，专为仅用作搜索分析器的一部分而设计。它可以更好地处理多词同义词，并建议在搜索时使用。但是，您仍然可以继续使用原始的`synonym`类型，在本文中它的行为相同。

+   将同义词过滤器从索引时间分析器中移除，并添加到搜索时间分析器中。

+   `search_analyzer`明确指定了 name 字段。如果没有指定，将使用相同的分析器（`index_analyzer`）用于索引和搜索。

分析器应该返回与之前相同的标记。然而，当您使用[这些命令](https://gist.github.com/lynnkwong/d47c77d7fabc48e2d7022ea5c7830caa)索引了这三个文档并再次执行相同的搜索时，结果将会有所不同：

```py
GET /inventory_synonym_graph/_search
{
  "query": {
    "match": {
      "name": "PS"
    }
  }
}
```

这次只返回了“PlayStation 4”。甚至“PS 3”都没有返回！

原因是同义词过滤器仅在搜索时应用。搜索查询“ps”被替换为同义词标记“playstation”。然而，索引中的文档没有经过同义词过滤器的筛选，因此“PS”只是被分词为“ps”，而没有被替换为“playstation”。对于“Play Station”也是一样。结果只有“PlayStation 4”能匹配。

要使其像前面的示例一样正常工作，我们需要将同义词规则从显式映射更改为等效的同义词。让我们将同义词过滤器更新如下：

```py
......
  "filter": {
    "synonym_filter": {
      "type": "synonym_graph",
        "synonyms": [
           "PS, PlayStation, Play Station"
        ]
    }
  }
......
```

要更改现有索引的同义词，我们可以重新创建索引并重新索引所有文档，这是愚蠢和低效的。

更好的方法是更新索引的设置。但是，在更新设置之前，我们需要关闭索引，然后重新打开它以便可以访问：

```py
 POST /inventory_synonym_graph/_close

PUT inventory_synonym_graph/_settings
{
  "settings": {
    "index.analysis.filter.synonym_filter.synonyms": [
      "PS, PlayStation, Play Station"
    ]
  }
}

POST /inventory_synonym_graph/_open
```

注意更新索引设置的特殊语法。

运行上述命令后，让我们使用`_analyzer`端点测试`search_analyzer`并查看生成的标记：

```py
GET /inventory_synonym_graph/_analyze
{
  "analyzer": "search_analyzer",
  "text": "PS 3"
}
```

这是结果：

```py
{
  "tokens": [
    {
      "token": "playstation",
      "start_offset": 0,
      "end_offset": 2,
      "type": "SYNONYM",
      "position": 0,
      "positionLength": 2
    },
    {
      "token": "play",
      "start_offset": 0,
      "end_offset": 2,
      "type": "SYNONYM",
      "position": 0
    },
    {
      "token": "ps",
      "start_offset": 0,
      "end_offset": 2,
      "type": "<ALPHANUM>",
      "position": 0,
      "positionLength": 2
    },
    {
      "token": "station",
      "start_offset": 0,
      "end_offset": 2,
      "type": "SYNONYM",
      "position": 1
    },
    {
      "token": "3",
      "start_offset": 3,
      "end_offset": 4,
      "type": "<NUM>",
      "position": 2
    }
  ]
}
```

它显示了“PS”搜索查询被替换并扩展为三个同义词的标记（由`[expand](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-synonym-graph-tokenfilter.html)`选项控制）。它还证明了如果等效的同义词在索引时间应用，结果索引的大小可能会显著增加。

然后我们再次执行相同的搜索：

```py
GET /inventory_synonym_graph/_search
{
  "query": {
    "match": {
      "name": "PS"
    }
  }
}
```

所有三个文档都将被返回。

## 使用同义词文件

在上面的示例中，我们一直在创建索引时直接指定同义词列表。然而，当你有大量的同义词时，将所有同义词都添加到索引中将会很麻烦。更好的方法是将它们存储在一个文件中，并动态加载到索引中。使用同义词文件有很多好处，包括：

+   方便维护大量的同义词。

+   可以被不同的索引使用。

+   可以在不关闭索引的情况下动态重新加载。

要开始，我们需要先将同义词放入一个文件中。每一行都是一个同义词规则，与上面演示的相同。更多[详细信息](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-synonym-graph-tokenfilter.html#_solr_synonyms_2)可以在官方文档中找到。

我们将创建的同义词文件称为`synonyms.txt`，但它可以被称为任何名称。它的内容如下：

```py
# This is a comment! The file is named synonyms.txt.
PS, PlayStation, Play Station
```

然后我们需要将同义词文件绑定到 Docker 容器。更新`[docker-compose.yaml](https://gist.github.com/lynnkwong/c84bbe872dad32e2222bd2c242e5d2ab)`如下：

```py
......
    volumes:
      - type: volume
        source: es_data
        target: /usr/share/elasticsearch/data
      - type: bind
        source: ./synonyms.txt
        target: /usr/share/elasticsearch/config/synonyms.txt
......
```

请注意，同义词文件加载到容器中的`config`文件夹中。您可以使用以下两个命令之一进入容器并检查它：

```py
# User docker
docker exec -it synonyms-elasticsearch-1  bash

# User docker-compose
docker-compose exec elasticsearch bash
```

现在我们需要停止并重新启动服务使更改生效。请注意，仅重新启动服务不起作用。

```py
docker-compose stop elasticsearch
docker-compose up -d elasticsearch
```

然后我们可以使用同义词文件创建一个新的索引：

```py
PUT /inventory_synonym_graph_file
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
            "synonyms_path": "synonyms.txt",
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

关键点：

+   对于`synonyms_path`，它是相对于 Elasticsearch 服务器中的`config`文件夹的同义词文件的路径。

+   添加了一个新的`updateable`字段，指定相应的过滤器是否可更新。我们将很快看到如何重新加载搜索分析器而不关闭和打开索引。

这个新索引`inventory_synonym_graph_file`的行为应该与之前的`inventory_synonym_graph`相同。

现在让我们向同义词文件添加更多同义词，然后它的内容将如下所示：

```py
# This is a comment! The file is named synonyms.txt.
PS, Play Station, PlayStation
JS => JavaScript
TS => TypeScript
Py => Python
```

当同义词已经添加，我们可以关闭并打开索引使其生效。然而，由于我们将同义词过滤器标记为可更新，我们可以重新加载搜索分析器，使更改立即生效，而无需关闭索引，因此没有停机时间。

要重新加载索引的搜索分析器，我们需要调用`_reload_search_analyzers`端点：

```py
POST /inventory_synonym_graph_file/_reload_search_analyzers
```

现在当我们分析“JS”字符串时，我们将看到返回“javascript”标记：

```py
GET /inventory_synonym_graph_file/_analyze
{
  "analyzer": "search_analyzer",
  "text": "JS"
}

// You will see the "javascript" token returned.
```

这里需要注意两件重要的事情：

+   如果对于同义词过滤器设置了`updateable`为`true`，那么相应的分析器只能用作*search_analyzer*，不能用于索引，即使类型是`synonym`。

+   `updateable`选项只能在使用`synonym_path`选项与同义词文件一起使用时使用，而不能在直接使用`synonyms`选项提供同义词时使用。

恭喜你到达这里！我们已经涵盖了在 Elasticsearch 中使用同义词功能的所有要点。

我们已经介绍了如何在索引时间和搜索时间分析步骤中使用同义词。此外，还介绍了如何直接提供同义词列表以及如何通过文件提供它们。最后但并非最不重要的是，介绍了更新现有索引的同义词列表的不同方法。建议重新加载索引的搜索分析器，因为它不会给服务带来停机时间。

## 相关文章：

+   [从实际例子中学习 Elasticsearch](https://lynn-kwong.medium.com/learn-elasticsearch-from-practical-examples-495f2f8db83e)

+   [如何在本地开发中使用 Docker 运行 Elasticsearch 8](https://levelup.gitconnected.com/how-to-run-elasticsearch-8-on-docker-for-local-development-401fd3fff829)
