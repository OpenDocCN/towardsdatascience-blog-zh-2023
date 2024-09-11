# 一位站点搜索工程师的日志：在 Elasticsearch 查询构建中应对相关性挑战

> 原文：[https://towardsdatascience.com/a-site-search-engineers-journal-approaching-relevance-challenges-in-elasticsearch-query-1eca29283da5?source=collection_archive---------17-----------------------#2023-06-13](https://towardsdatascience.com/a-site-search-engineers-journal-approaching-relevance-challenges-in-elasticsearch-query-1eca29283da5?source=collection_archive---------17-----------------------#2023-06-13)

## 与 Elasticsearch 的工作就像是在玩乐高

[](https://medium.com/@quy.dinh3195?source=post_page-----1eca29283da5--------------------------------)[![Quý Đinh](../Images/40c7da5cfcc56d79d2b6f45ea70a26ee.png)](https://medium.com/@quy.dinh3195?source=post_page-----1eca29283da5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1eca29283da5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1eca29283da5--------------------------------) [Quý Đinh](https://medium.com/@quy.dinh3195?source=post_page-----1eca29283da5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44a47bf76e4e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-site-search-engineers-journal-approaching-relevance-challenges-in-elasticsearch-query-1eca29283da5&user=Qu%C3%BD+%C4%90inh&userId=44a47bf76e4e&source=post_page-44a47bf76e4e----1eca29283da5---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1eca29283da5--------------------------------) · 15 分钟阅读 · 2023年6月13日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1eca29283da5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-site-search-engineers-journal-approaching-relevance-challenges-in-elasticsearch-query-1eca29283da5&user=Qu%C3%BD+%C4%90inh&userId=44a47bf76e4e&source=-----1eca29283da5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1eca29283da5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-site-search-engineers-journal-approaching-relevance-challenges-in-elasticsearch-query-1eca29283da5&source=-----1eca29283da5---------------------bookmark_footer-----------)![](../Images/f943af89e64f6e075864489a6e3eb919.png)

[Mourizal Zativa](https://unsplash.com/@mourimoto?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

在过去的22个月里，我一直担任站点搜索工程师，使用 Elasticsearch 来帮助提高我们餐饮平台的相关性。我总共部署了83个版本，包括3个主要版本。

每周大约发布一次，我可以说我们的搜索引擎不仅比两年前好得多，而且我也学到了很多。虽然仍然远未达到一个伟大的搜索引擎，但在我看来，有些事情值得分享。更重要的是，我非常希望得到反馈。

本博客文章旨在提供一种设计 Elasticsearch 查询模板的方法，以处理常见的网站搜索问题，包括跨不同字段的匹配、结果提升和测试。我们将共同识别默认方法的问题，然后逐步提出一种新的方法来解决这些问题。

这个 Github 仓库：[https://github.com/dvquy13/elasticsearch-sharing](https://github.com/dvquy13/elasticsearch-sharing) 包含了本文讨论的示例和代码。

# 主要

我们现在扮演餐厅平台搜索工程师的角色，该平台允许 diners 发现并预订他们的下一餐。我们经验不多，但幸运的是，该应用程序一开始并不要求像 Google 那样的准确度。关键是逐步取得可见的进展！

好的，让我们*深入探讨*。首先，我们确保用户可以通过名称搜索餐厅。在这里，我们可以依赖简单的默认 `query-match` 来完成这个工作。

```py
# Index our first two restaurants
POST _bulk
{ "index" : { "_index" : "restaurant", "_id" : "001sabichuong" } }
{ "restaurant_name": "Sa Bi Chuong", "cuisine": "Vietnamese", "rating": 5.0 }
{ "index" : { "_index" : "restaurant", "_id" : "002vietnamesephonoodle" } }
{ "restaurant_name": "Vietnamese Pho Noodle", "cuisine": "Vietnamese", "rating": 4.0 }

# Test searching for one
# Should return Vietnamese Pho Noodle
GET restaurant/_search
{
  "query" : {
    "match" : { "restaurant_name": "vietnamese" }
  }
}
```

上面的代码片段可以在 Kibana 的 Dev Tools > Console 中运行，如果你按照仓库的说明，你会在 `localhost:5601` 找到它。

代码是自解释的。我们要求 Elasticsearch 返回名称中包含 `vietnamese` 的餐厅。我们收到了一个 `Vietnamese Pho Noodle` 的结果。没有问题。

但我们很快发现，名称并不是我们在用户提交查询时想要查看的唯一地方。鉴于关键字 `vietnamese`，我们还应该返回餐厅 `Sa Bi Chuong`，因为它在 `cuisine` 中标记为越南餐厅。一个 `multi_match` 查询可以准确地做到这一点。

```py
# Matching multiple fields
# Should return all 2 Vietnamese restaurant with the Vietnamese Pho Noodle on top
GET restaurant/_search
{
  "query" : {
    "multi_match" : {
      "query": "vietnamese",
      "fields": [ "restaurant_name", "cuisine" ]
    }
  }
}
```

```py
# Result
"hits": {
    ...
    "hits": [
      {
        "_index": "restaurant",
        "_id": "002vietnamesephonoodle",
        "_score": 0.6931471,
        "_source": {
          "restaurant_name": "Vietnamese Pho Noodle",
          "cuisine": "Vietnamese",
          "rating": 4
        }
      },
      {
        "_index": "restaurant",
        "_id": "001sabichuong",
        "_score": 0.18232156,
        "_source": {
          "restaurant_name": "Sa Bi Chuong",
          "cuisine": "Vietnamese",
          "rating": 5
        }
      }
    ]
  }
```

## 默认 TFIDF 的问题

注意上述分数。第一个分数比第二个高出大约 4 倍，这表明它与查询 `vietnamese` 的相关性要高得多。有人可能会假设，因为在多个字段中匹配会使分数更高。

每当我们有疑问时，可以使用 Elasticsearch 的 `explain` 来获取详细的评分组成部分说明。

```py
# Let's use explain=true to see what happens under the hood
# Vietnamese Pho Noodle is on top because of the default implementation of TFIDF that penalizes the matching at cuisine field because there are multiple restaurants with cuisine=Vietnamese while there are only one restaurant with name=Vietnamese
# Question: But why having the name Vietnamese in its name makes it more Vietnamese than other restaurants?
GET restaurant/_search
{
  "query" : {
    "multi_match" : {
      "query": "vietnamese",
      "fields": [ "restaurant_name", "cuisine" ]
    }
  },
  "explain": true
}
```

```py
# Result
"hits": {
    "hits": [
      {
        "_id": "002vietnamesephonoodle",
        "_score": 0.6931471,
        "_source": {
          "restaurant_name": "Vietnamese Pho Noodle",
          "cuisine": "Vietnamese",
          "rating": 4
        },
        "_explanation": {
          "value": 0.6931471,
          "description": "max of:",
          "details": [
            # Matching in field `cuisine` yields score=0.18
            # Note that by default the score is calculated by TFIDF
            # More info about Elasticsearch TFIDF: https://www.elastic.co/guide/en/elasticsearch/reference/8.6/index-modules-similarity.html#bm25
            {
              "value": 0.18232156,
              "description": "weight(cuisine:vietnamese in 1) [PerFieldSimilarity], result of:",
              "details": [...]
            },
            # Matching in field `restaurant_name` yields score=0.69
            {
              "value": 0.6931471,
              "description": "weight(restaurant_name:vietnamese in 1) [PerFieldSimilarity], result of:",
              "details": [...]
            }
            # Because the final score is "max of:" those two above scores,
            # it is equal to the matching score with `restaurant_name`
          ]
        }
      },
      {
        "_id": "001sabichuong",
        "_score": 0.18232156,
        "_source": {
          "restaurant_name": "Sa Bi Chuong",
          "cuisine": "Vietnamese",
          "rating": 5
        },
        # Similarly since there's no matching with `restaurant_name`,
        # here the final score is equal to the matching score of `cuisine`
        "_explanation": {
          "value": 0.18232156,
          "description": "max of:",
          "details": [
            {
              "value": 0.18232156,
              "description": "weight(cuisine:vietnamese in 0) [PerFieldSimilarity], result of:",
              "details": [...]
            }
          ]
        }
      }
    ]
  }
```

上面我们可以看到 `Vietnamese Pho Noodle` 排在最前面，是因为默认实现的 TFIDF 处罚了在菜系字段中的匹配，因为有多个餐厅的 `cuisine=Vietnamese`，而只有一个餐厅的 `name=Vietnamese`。

深入到 `_explanation` 块，我们意识到分数差异源于 `restaurant_name` 的 TFIDF 匹配输出。这是预期的，因为该算法假设如果关键词不常见且通常在许多文档中找到（类似于自动处理停用词的解决方案），那么它是更好的信号。在我们的例子中，两家餐厅都有越南菜，因此根据 TFIDF，这种匹配并不能说明文档的相关性。

我们是否应该鼓励这种行为是一个问题。真的有越南人名在名称中会使一个餐厅比另一个更“越南”吗？

另一个 TFIDF 的问题是它考虑了字段的长度。

```py
# Let's add another restaurant
POST _bulk
{ "index" : { "_index" : "restaurant", "_id" : "003vietnamesepho" } }
{ "restaurant_name": "Vietnamese Pho", "cuisine": "Vietnamese", "rating": 3.0 }

# In the below example we see that the new Vietnamese Pho restaurant is ranked higher...
GET restaurant/_search
{
  "query" : {
    "multi_match" : {
      "query": "vietnamese pho",
      "fields": [ "restaurant_name", "cuisine" ]
    }
  },
  "explain": true
}
```

你可以在帖子末尾的附录 #1 中找到详细而冗长的结果。简而言之，我们看到结果将越南河粉餐厅排名第一，然后是越南河粉面。分析组成分数表明，主要区别在于越南河粉的 `length=2`（词）而越南河粉面的 `length=3`。这感觉不直观，因为我们知道第二家餐厅的评分更高，考虑到实际情况，两者匹配用户关键字一样。

## 使用 function_score 进行重新排序（提升）

当我们谈论 `rating` 时，我们可以用 `function_score` 包裹我们的查询，以将该信息纳入修改匹配分数，从而更好地控制我们的排名。

```py
GET restaurant/_search
{
  "query": {
    "function_score": {
      # Our main query is wrapped in a function_score clause
      "query": {
        "multi_match" : {
          "query": "vietnamese",
          "fields": [ "restaurant_name", "cuisine" ]
        }
      },
      # We define the functions that will be applied on top of the matching scores 
      # returned by our main query
      "functions": [
        {
          "field_value_factor": {
            "field": "rating",
            "modifier": "none",
            "missing": 1
          }
        }
      ],
      # Retrieve the max boosting defined inside `functions`
      # Above there is only one boosting so it's applied by default
      "score_mode": "max",
      # Multiply the matching score with the boosting calculated from functions
      "boost_mode": "multiply"
    }
  }
}
```

```py
# Result
{
  "hits": {
    "hits": [
      {
        "_index": "restaurant",
        "_id": "002vietnamesephonoodle",
        "_score": 1.7885544,
        "_source": {
          "restaurant_name": "Vietnamese Pho Noodle",
          "cuisine": "Vietnamese",
          "rating": 4
        }
      },
      {
        "_index": "restaurant",
        "_id": "003vietnamesepho",
        "_score": 1.5706451,
        "_source": {
          "restaurant_name": "Vietnamese Pho",
          "cuisine": "Vietnamese",
          "rating": 3
        }
      },
      {
        "_index": "restaurant",
        "_id": "001sabichuong",
        "_score": 0.66765696,
        "_source": {
          "restaurant_name": "Sa Bi Chuong",
          "cuisine": "Vietnamese",
          "rating": 5
        }
      }
    ]
  }
}
```

高评分的餐厅现在排在前面了。但是餐厅 `Sa Bi Chuong` 的 `rating=5` 怎么样？它作为最后的结果似乎我们没有“足够”提升。

我们可能会开始稍微调整 `function_score`，以使其发生。这是一种实现方式，以非线性方式模拟提升，从而有效地在评分为 `5` 的文档上应用强烈提升。

```py
GET restaurant/_search
{
  "query": {
    "function_score": {
      "query": {
        "multi_match" : {
          "query": "vietnamese",
          "fields": [ "restaurant_name", "cuisine" ]
        }
      },
      "functions": [
        # Apply a non-linear function to model that
        # a rating of 5 has much more weight than rating of 4 (not just 25% more)
        {
          "filter": {
            "range": {
              "rating": {
                "gte": 5,
                "lte": 5
              }
            }
          },
          "weight": 10
        },
        {
          "filter": {
            "range": {
              "rating": {
                "gte": 4,
                "lt": 5
              }
            }
          },
          "weight": 2
        }
      ],
      "score_mode": "max",
      "boost_mode": "multiply"
    }
  }
}
```

```py
# Result
{
  "hits": {
    "hits": [
      {
        "_index": "restaurant",
        "_id": "001sabichuong",
        "_score": 1.3353139,
        "_source": {
          "restaurant_name": "Sa Bi Chuong",
          "cuisine": "Vietnamese",
          "rating": 5
        }
      },
      {
        "_index": "restaurant",
        "_id": "002vietnamesephonoodle",
        "_score": 0.8942772,
        "_source": {
          "restaurant_name": "Vietnamese Pho Noodle",
          "cuisine": "Vietnamese",
          "rating": 4
        }
      },
      {
        "_index": "restaurant",
        "_id": "003vietnamesepho",
        "_score": 0.52354836,
        "_source": {
          "restaurant_name": "Vietnamese Pho",
          "cuisine": "Vietnamese",
          "rating": 3
        }
      }
    ]
  }
}
```

我们可能会思考：“现在的函数提升看起来不是太武断了吗？它对其他情况有效吗？”确实，这是我们应该问自己的问题。随着越来越多的需求，我们的查询模板将变得越来越复杂，导致我们所做的修改之间的冲突。

让我们来看下一个例子，来说明我所说的“冲突”。

## 复杂性随着模糊匹配而来

虽然不是必需的，但处理用户的拼写错误总是一个不错的功能，尤其是当他们现在熟悉像 Google 这样的智能搜索引擎时。Elasticsearch 有一个名为 `fuzzy matching` 的内置机制，可以通过选项 `fuzziness` 进行配置。

```py
# The use of `bool` query below is to implement the logic: At least one condition should match
PUT _scripts/01-default-fuzzy-search-template
{
  "script": {
    "lang": "mustache",
    "source": {
      "query": {
        "function_score": {
          "query": {
            "bool": {
              "must": [
                {
                  "bool": {
                    "should": [
                      {
                        "multi_match" : {
                          "query": "{{query_string}}",
                          "fields": [ "restaurant_name", "cuisine" ]
                        }
                      },
                      {
                        "multi_match" : {
                          "query": "{{query_string}}",
                          "fields": [ "restaurant_name", "cuisine" ],
                          # For the purpose of this demo, default behavior works well enough
                          "fuzziness": "AUTO"
                        }
                      }
                    ]
                  }
                }
              ]
            }
          },
          "functions": [
            {
              "filter": {
                "range": {
                  "rating": {
                    "gte": 5,
                    "lte": 5
                  }
                }
              },
              "weight": 10
            },
            {
              "filter": {
                "range": {
                  "rating": {
                    "gte": 4,
                    "lt": 5
                  }
                }
              },
              "weight": 2
            }
          ],
          "score_mode": "max",
          "boost_mode": "multiply"
        }
      }
    },
    "params": {
      "query_string": "My query string"
    }
  }
}
```

注意，我们只是创建了一个查询模板而不是运行一个查询。现在我们可以使用参数调用查询，这是 Elasticsearch 引入的一个很好的功能，可以使我们的代码看起来不那么令人畏惧。就像这样：

```py
GET /_search/template
{
  "id": "01-default-fuzzy-search-template",
  "params": {
    "query_string": "vietnames"
  }
}
```

上述查询返回了我们期望的越南餐馆，给定一个拼写错误的关键词`vietnames`。在后台，模糊匹配使用了[Levenshtein编辑距离](https://www.elastic.co/guide/en/elasticsearch/reference/8.7/query-dsl-fuzzy-query.html)，它通过修改一个字符串使其变成另一个字符串的次数来衡量字符串之间的相似度。在我们的例子中，我们只需要在末尾添加一个字母`e`，将`vietnames`变成`vietnamese`。对算法来说，这是一项非常简单的任务。也许有人会争辩说，这对我们的开发人员来说也很简单。2行代码和一个新的漂亮特性。

好吧，真正有趣的地方在于别处。一天，我们的销售团队突然向我们投诉搜索结果错误。即使人们明确搜索`kbbq`（这是`korean bbq`的常见缩写），他们也会得到日本烧烤餐馆而不是韩国烧烤餐馆。

餐馆如下：

```py
POST _bulk
{ "index" : { "_index" : "restaurant", "_id" : "004parkhangseokbbq" } }
{ "restaurant_name": "Park Hang-seo's KBBQ", "cuisine": "Korean", "rating": 2.0 }
{ "index" : { "_index" : "restaurant", "_id" : "005bestbbqintown" } }
{ "restaurant_name": "Best BBQ in town", "cuisine": "Japanese", "rating": 5.0 }
```

查询：

```py
{
  "id": "01-default-fuzzy-search-template",
  "params": {
    "query_string": "kbbq"
  }
}
```

结果：

```py
{
  "hits": {
    "hits": [
      {
        "_index": "restaurant",
        "_id": "005bestbbqintown",
        "_score": 8.384459,
        "_source": {
          "restaurant_name": "Best BBQ in town",
          "cuisine": "Japanese",
          "rating": 5
        }
      },
      {
        "_index": "restaurant",
        "_id": "004parkhangseokbbq",
        "_score": 2.5153382,
        "_source": {
          "restaurant_name": "Park Hang-seo's KBBQ",
          "cuisine": "Korean",
          "rating": 2
        }
      }
    ]
  }
}
```

为了理解发生了什么，我们需要启用`explain=true`来查看是什么因素贡献了最终的分数。由于这次输出过于冗长，以下是发现：

+   `Best BBQ in town`餐馆的关键词匹配分数（在提升之前）为0.8，低于`Park Hang-seo's KBBQ`的1.2

+   如果没有应用提升，我们会看到`Park Hang-seo's KBBQ`餐馆排在第一位

+   但随后，来自`rating`的提升修改了分数，导致我们看到的排序结果

一种框架化问题的方法是我们目前的提升算法不完美。假设我们有一个更好的公式，能够取得正确的平衡，那么问题应该可以解决。但几乎无法保证新公式不会引发其他问题。我们不希望这些问题在没有任何警示的情况下悄然进入系统，然后在某一天被利益相关者标记出来。我们希望成为第一个发现这些问题的人，特别是每当我们进行任何更改时。因此，在讨论潜在解决方案之前，我希望我们都同意，接下来我们应该做的非常重要的事情是（是的，你可能和我有一样的想法）建立一个测试/评估机制。

## 我们应该如何为这个搜索应用创建测试用例？

在我看来，第一个挑战是数据迁移。查询和文档都可能随着时间的推移而增长，因此静态模拟数据集可能在一个月后不再很好地代表搜索相关性。接下来的问题与我们的思维方式有关。有时我们可能需要考虑是否需要 100% 通过测试用例才能解决这个新的紧急问题。例如，有些情况下，如果你修复了一些问题，其他测试用例的搜索结果排序可能会稍有改变。如果我们硬编码排名，那么我们可能会为了调整查询模板而苦苦挣扎。但实际上，我们往往不需要排名完全预定义，也不完全确定哪个排序实际上是最优的。我们应该考虑使用一种软机制，通过量化系统的相关性并使用阈值来代替。

在这里我们将讨论如何使用 Elasticsearch 排名评估 API 来实现这种评估方案：

```py
GET restaurant/_rank_eval
{
  # Query template comes in really handy when used in conjunction with _rank_eval
  "templates": [
     {
        "id": "01-default-fuzzy-search-template",
        "template": { 
            "id": "01-default-fuzzy-search-template"
        }
     }
  ],
  "requests": [
    {
     "id": "kbbq_query",
     # Here we manually define the true positives with rating >= 1.0
     # The actual rating number helps when using metrics that takes into account
     # the ranking of the search results
     "ratings": [
        { "_index": "restaurant", "_id": "004parkhangseokbbq", "rating": 3 },
        { "_index": "restaurant", "_id": "005bestbbqintown", "rating": 1 }
      ],
     "template_id": "01-default-fuzzy-search-template",
     "params": { 
        "query_string": "kbbq"
      }
    },
    {
     "id": "vietnamese_query",
     "ratings": [
        { "_index": "restaurant", "_id": "001sabichuong", "rating": 3 },
        { "_index": "restaurant", "_id": "002vietnamesephonoodle", "rating": 3 },
        { "_index": "restaurant", "_id": "003vietnamesepho", "rating": 3 }
      ],
     "template_id": "01-default-fuzzy-search-template", 
     "params": { 
        "query_string": "vietnamese"
      }
    }
  ],
  "metric": {
    "dcg": {
      "k": 5,
      "normalize": true
    }
  }
}
```

结果：

```py
{
  "metric_score": 0.8549048706984328,  # This is the overall metric score, best is 1.0, worst is 0.0
  "details": {
    "kbbq_query": {
      # This kbbq_query has a imperfect score because it ranks the more relevant result lower
      "metric_score": 0.7098097413968655,
      "unrated_docs": [],
      "hits": [
        {
          "hit": {
            "_index": "restaurant",
            "_id": "005bestbbqintown",
            "_score": 8.384459
          },
          "rating": 1
        },
        {
          "hit": {
            "_index": "restaurant",
            "_id": "004parkhangseokbbq",
            "_score": 2.5153382
          },
          "rating": 3
        }
      ],
      "metric_details": {
        ...
      }
    },
    "vietnamese_query": {
      "metric_score": 1,
      "unrated_docs": [],
      "hits": [
        ...
      ],
      "metric_details": {
        ...
      }
    }
  },
  "failures": {}
}
```

让我们尝试通过引入一些变更来使评估分数更接近完美的 1.0。

## 我们修订后的搜索模型

在开始设计新的查询模板之前，我们可以先退一步，真正思考一下我们应该如何建模搜索引擎。以下是要点：

1.  精确匹配总是会排在模糊匹配等非精确匹配之上；

1.  精确匹配不考虑字段长度或词汇/文档频率。如果两个文档在某个字段中的精确匹配相同，它们应该具有相同的关键词匹配分数；

1.  在相同的匹配级别内（无论是精确匹配还是模糊匹配），虽然初始的关键词匹配分数应该是相同的，但可以通过距离、流行度等某些修正器进行重新排序。然而，修正后的分数不应使最终分数超过上层级别的基准分数，例如，修正后的模糊分数不应大于精确基准分数。这是为了确保本质上的 #1。

如果你看足球，这类似于英超等联赛如何排名他们的球队。不论球队 L 比球队 M 多进了多少球，或者两队之间的交锋结果如何，如果球队 M 的积分比球队 L 高，那么球队 M 排名会更高。其他措施仅用于决胜负。

这种理解可以转化为我们如何使用 Elasticsearch 来表达我们的模型。

一种方法是使用 `dis_max` 查询结合 `constant_score` 查询。其思路是将每种匹配类型分类到不同的分数级别，其中一个级别的分数是下一个级别的两倍。落入某一匹配级别的文档（平局）将由修正器重新排序，但最终新的分数不会超过上层基准分数。以下是新的查询模板：

```py
PUT _scripts/02-constant-score-search-template
{
  "script": {
    "lang": "mustache",
    "source": {
      "query": {
        "function_score": {
          "query": {
            "bool": {
              "must": [
                {
                  "bool": {
                    "should": [
                      {
                        # `dis_max` query gets the max score of an array of clauses
                        "dis_max": {
                          "queries": [
                            {
                              # `constant_score` says that if matches, return a constant score
                              "constant_score": {
                                "filter": {
                                  "multi_match" : {
                                    "query": "{{query_string}}",
                                    "fields": [ "restaurant_name", "cuisine" ]
                                  }
                                },
                                # This is the constant that is returned as score
                                # Note that the exact number is chosen intentionally
                                # Here the upper level will be twice the lower level
                                # and we will restrict the modifiers to be only
                                # able to boost by at most 100% the base score
                                # so that the lower level can not exceed the upper
                                "boost": 2
                              }
                            },
                            {
                              "constant_score": {
                                "filter": {
                                  "multi_match" : {
                                    "query": "{{query_string}}",
                                    "fields": [ "restaurant_name", "cuisine" ],
                                    "fuzziness": "AUTO"
                                  }
                                },
                                "boost": 1
                              }
                            }
                          ]
                        }
                      }
                    ]
                  }
                }
              ]
            }
          },
          "functions": [
            # Design the modifiers to be multiplier of maximum 1.9999 the base score
            {
              "weight": 1
            },
            {
              "field_value_factor": {
                "field": "rating",
                "modifier": "ln",
                "missing": 1
              },
              "weight": 0.1
            }
          ],
          "score_mode": "sum",
          "boost_mode": "multiply"
        }
      }
    },
    "params": {
      "query_string": "My query string"
    }
  }
}
```

当我们重新运行评估时，可以观察到标准化的 DCG 指标现在的分数等于 1.0，表示完美的准确性！

# 总结

本博客文章重点是让你置身于需要衍生适合站点搜索引擎需求的查询模板的Elasticsearch工程师角色。我们简要覆盖了以下主题：

+   多字段关键词匹配

+   理解默认Elasticsearch评分

+   默认TFIDF的问题

+   通过属性提升搜索结果

+   模糊匹配

+   Elasticsearch查询模板评估与排名评估API

+   使用`dis_max`和`constant_score`构造查询

虽然绝对不是最佳的，我希望博客中的某些部分能帮助你更接近利用Elasticsearch解决自己的问题。

我也非常感激任何评论或反馈。如果你想深入讨论，请在此帖子下评论或在Github仓库中打开一个问题：[https://github.com/dvquy13/elasticsearch-sharing](https://github.com/dvquy13/elasticsearch-sharing)。

谢谢大家！

# 附录

## #1：详细解析默认TFIDF匹配，其中字段值的长度影响整体匹配分数

```py
# Result
{
  "hits": {
    "hits": [
      {
        "_id": "003vietnamesepho",
        "_score": 1.0470967,
        "_source": {
          "restaurant_name": "Vietnamese Pho",
          "cuisine": "Vietnamese",
          "rating": 3
        },
        "_explanation": {
          "value": 1.0470967,
          "description": "max of:",
          "details": [
            {
              "value": 0.13353139,
              "description": "sum of:",
              "details": [
                {
                  "value": 0.13353139,
                  "description": "weight(cuisine:vietnamese in 0) [PerFieldSimilarity], result of:",
                  "details": [...]
                }
              ]
            },
            {
              "value": 1.0470967,
              "description": "sum of:",
              "details": [
                # Matching score with "vietnamese"
                {
                  "value": 0.52354836,
                  "description": "weight(restaurant_name:vietnamese in 0) [PerFieldSimilarity], result of:",
                  "details": [
                    {
                      "value": 0.52354836,
                      "description": "score(freq=1.0), computed as boost * idf * tf from:",
                      "details": [
                        {
                          "value": 2.2,
                          "description": "boost",
                          "details": []
                        },
                        {
                          "value": 0.47000363,
                          "description": "idf, computed as log(1 + (N - n + 0.5) / (n + 0.5)) from:",
                          ...
                        },
                        {
                          "value": 0.50632906,
                          "description": "tf, computed as freq / (freq + k1 * (1 - b + b * dl / avgdl)) from:",
                          "details": [
                            {
                              "value": 1,
                              "description": "freq, occurrences of term within document",
                              "details": []
                            },
                            {
                              "value": 1.2,
                              "description": "k1, term saturation parameter",
                              "details": []
                            },
                            {
                              "value": 0.75,
                              "description": "b, length normalization parameter",
                              "details": []
                            },
                            # Notice the length=2 here is in the denominator,
                            # which means that the higher the length the less
                            # the score
                            {
                              "value": 2,
                              "description": "dl, length of field",
                              "details": []
                            },
                            {
                              "value": 2.6666667,
                              "description": "avgdl, average length of field",
                              "details": []
                            }
                          ]
                        }
                      ]
                    }
                  ]
                },
                # Matching score with "pho"
                {
                  "value": 0.52354836,
                  "description": "weight(restaurant_name:pho in 0) [PerFieldSimilarity], result of:",
                  # Details are exactly like above
                  "details": [...]
                }
              ]
            }
          ]
        }
      },
      {
        "_id": "002vietnamesephonoodle",
        "_score": 0.8942772,
        "_source": {
          "restaurant_name": "Vietnamese Pho Noodle",
          "cuisine": "Vietnamese",
          "rating": 4
        },
        "_explanation": {
          "value": 0.8942772,
          "description": "max of:",
          "details": [
            {
              "value": 0.13353139,
              "description": "sum of:",
              "details": [...]
            },
            {
              "value": 0.8942772,
              "description": "sum of:",
              "details": [
                {
                  "value": 0.4471386,
                  "description": "weight(restaurant_name:vietnamese in 1) [PerFieldSimilarity], result of:",
                  "details": [
                    {
                      "value": 0.4471386,
                      "description": "score(freq=1.0), computed as boost * idf * tf from:",
                      "details": [
                        ...,
                        {
                          "value": 0.4324324,
                          "description": "tf, computed as freq / (freq + k1 * (1 - b + b * dl / avgdl)) from:",
                          "details": [
                            ...,
                            # Here the length=3 (greater than length=2 of the
                            # above restaurant)
                            {
                              "value": 3,
                              "description": "dl, length of field",
                              "details": []
                            },
                            ...
                          ]
                        }
                      ]
                    }
                  ]
                },
                {
                  "value": 0.4471386,
                  "description": "weight(restaurant_name:pho in 1) [PerFieldSimilarity], result of:",
                  "details": [...]
                }
              ]
            }
          ]
        }
      }
    ]
  }
}
```
