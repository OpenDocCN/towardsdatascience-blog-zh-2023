# 掌握 Elasticsearch：强大搜索与精确性的初学者指南 — 第1部分

> 原文：[https://towardsdatascience.com/mastering-elasticsearch-a-beginners-guide-to-powerful-searches-and-precision-part-1-87686fec9808?source=collection_archive---------2-----------------------#2023-11-21](https://towardsdatascience.com/mastering-elasticsearch-a-beginners-guide-to-powerful-searches-and-precision-part-1-87686fec9808?source=collection_archive---------2-----------------------#2023-11-21)

## 解锁 Elasticsearch 的力量：深入理解 Elasticsearch，掌握基本的搜索查询，并探索词汇搜索

[](https://medium.com/@sanilkhurana7?source=post_page-----87686fec9808--------------------------------)[![Sanil Khurana](../Images/b6aea8dd0366a0659fcf3828fc745aea.png)](https://medium.com/@sanilkhurana7?source=post_page-----87686fec9808--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87686fec9808--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----87686fec9808--------------------------------) [Sanil Khurana](https://medium.com/@sanilkhurana7?source=post_page-----87686fec9808--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bda56b80bb9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-elasticsearch-a-beginners-guide-to-powerful-searches-and-precision-part-1-87686fec9808&user=Sanil+Khurana&userId=2bda56b80bb9&source=post_page-2bda56b80bb9----87686fec9808---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----87686fec9808--------------------------------) ·19 分钟阅读·2023年11月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87686fec9808&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-elasticsearch-a-beginners-guide-to-powerful-searches-and-precision-part-1-87686fec9808&user=Sanil+Khurana&userId=2bda56b80bb9&source=-----87686fec9808---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87686fec9808&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmastering-elasticsearch-a-beginners-guide-to-powerful-searches-and-precision-part-1-87686fec9808&source=-----87686fec9808---------------------bookmark_footer-----------)![](../Images/b489d34d4f65f6863cced325c6f575e0.png)

# 目录

· [介绍](#90ef)

· [从我们离开的地方开始，Elasticsearch](#a78d)

∘ [示例数据集](#67ab)

∘ [理解 ElasticSearch 查询](#95a7)

∘ [理解响应](#a1e5)

∘ [一个基本搜索查询](#1b1f)

· [词汇搜索](#88c4)

· [我们当前搜索查询中的问题](#64f7)

∘ [相似词返回不同结果](#11df)

∘ [对用户需求理解不足](#3dd1)

∘ [相似词未被返回](#bfca)

∘ [忽略打字错误](#aa13)

∘ [不同的词组合有不同的含义](#ad80)

· [改进我们的搜索](#21f0)

∘ [提升更相关的字段](#2bc3)

∘ [基于函数的提升](#1e6e)

∘ [模糊查询](#ac5f)

· [结论](#da2c)

# 介绍

是否曾想过你如何在网上轻松找到完美的鞋子，或在广阔的社交媒体领域中偶然发现朋友的帖子？这都要归功于数字体验中的无名英雄：搜索系统。

回想一下你最近的一次在线购物——无论是时尚的鞋子还是为朋友挑选的贴心书籍。你是如何准确找到你想要的东西的？你很可能是通过搜索栏在大量选项中导航！这就是搜索系统的魔力，它在默默塑造我们的在线体验，并使我们在数字通道中轻松发现完美的商品。在充满选择的世界中，快速而轻松地找到我们想要的东西，证明了强大而直观的搜索系统对于我们喜爱的产品的重要性。

在我最近的 Elasticsearch 探索中（[查看我关于其架构和术语的入门介绍](https://medium.com/better-programming/system-design-series-elasticsearch-architecting-for-search-5d5e61360463)），我们揭示了驱动这些发现的引擎。本文深入探讨搜索——导航 ElasticSearch 查询，理解响应，并创建一个基本的查询以设定基础。

我们的目标：建立一个简单的搜索查询，发现问题，并通过实际例子加以改进。加入我们，一起认识到当前搜索系统中的挑战，并在这个数字世界的通道中探索改进的路径。”

# 从我们停下的地方继续，Elasticsearch

## 示例数据集

为了演示我们如何改进搜索，让我们设置 Elasticsearch 并加载一些数据。对于这篇文章，我将使用 [我在 Kaggle 上找到的新闻数据集](https://www.kaggle.com/datasets/rmisra/news-category-dataset)。这个数据集非常简单，包含大约 210,000 篇新闻文章，包括它们的标题、简短描述、作者，以及一些我们不太关心的其他字段。我们不需要所有 210,000 个文档，所以我会在 ES 中加载大约 10,000 个文档并开始搜索。

这些是数据集中几个文档的示例 —

```py
[
  {
    "link": "https://www.huffpost.com/entry/new-york-city-board-of-elections-mess_n_60de223ee4b094dd26898361",
    "headline": "Why New York City’s Board Of Elections Is A Mess",
    "short_description": "“There’s a fundamental problem having partisan boards of elections,” said a New York elections attorney.",
    "category": "POLITICS",
    "authors": "Daniel Marans",
    "country": "IN",
    "timestamp": 1689878099
  },
  ....
]
```

每个文档代表一篇新闻文章。每篇文章包含一个`link`、`headline`、一个`short_description`、一个`category`、`authors`、`country`（随机值，由我添加）和`timestamp`（同样是随机值，由我添加）。

我添加了`country`和`timestamp`字段，以使接下来的示例更加有趣，所以让我们开始吧！

## 理解 ElasticSearch 查询

Elasticsearch 查询是用 JSON 编写的。我们不深入探讨所有不同的语法来创建搜索查询，而是从简单的开始，并在此基础上构建。

最简单的全文查询是 `match` 查询。其思想很简单，您编写一个查询，Elasticsearch 对特定字段执行全文搜索。例如，

```py
GET news/_search
{
  "query": {
    "match": {
      "headline": "robbery"
    }
  }
}
```

上述查询找到了所有标题中出现单词“抢劫”的文章。这些是我收到的结果 -

```py
{
  "_index" : "news",
  "_id" : "RzrouIsBC1dvdsZHf2cP",
  "_score" : 7.4066687,
  "_source" : {
    "link" : "https://www.huffpost.com/entry/guard-cat-hailed-as-hero_n_62e9a515e4b00f4cf2352a6f",
    "headline" : "Bandit The 'Guard Cat' Hailed As Hero After Thwarting Would-Be Robbery",
    "short_description" : "When at least two people tried to break into a Tupelo, Mississippi, home last week, the cat did everything she could to alert its owner.",
    "category" : "WEIRD NEWS",
    "authors" : "",
    "country" : "US",
    "timestamp" : 1693070640
  }
},
{
  "_index" : "news",
  "_id" : "WTrouIsBC1dvdsZHp2wd",
  "_score" : 7.4066687,
  "_source" : {
    "link" : "https://www.huffpost.com/entry/san-francisco-news-crew-security-guard-shot-killed_n_61a2a9d8e4b0ae9a42af278a",
    "headline" : "News Crew Security Guard Dies After Being Shot While Helping Robbery Coverage",
    "short_description" : "Kevin Nishita was shot in the abdomen while doing his job amid an uptick in organized retail crime.",
    "category" : "CRIME",
    "authors" : "Daisy Nguyen, AP",
    "country" : "US",
    "timestamp" : 1692480894
  }
}
```

但是，如果您希望在多个字段上执行全文搜索，该怎么办？您可以通过 `multi_match` 查询来实现，

```py
GET news/_search
{
  "query": {
    "multi_match": {
      "query": "robbery",
      "fields": ["headline", "short_description"]
    }
  }
}
```

这执行了类似的操作，但不再仅限于单个字段，而是查看所有文档的 `headine` 和 `short_description`，并对它们进行全文搜索。

## 理解响应

这是我们上次查询的一个示例响应 -

```py
{
  "took" : 5,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 29.626675,
    "hits" : [
      {
        "_index" : "news",
        "_id" : "RzrouIsBC1dvdsZHf2cP",
        "_score" : 29.626675,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/guard-cat-hailed-as-hero_n_62e9a515e4b00f4cf2352a6f",
          "headline" : "Bandit The 'Guard Cat' Hailed As Hero After Thwarting Would-Be Robbery",
          "short_description" : "When at least two people tried to break into a Tupelo, Mississippi, home last week, the cat did everything she could to alert its owner.",
          "category" : "WEIRD NEWS",
          "authors" : "",
          "country" : "US",
          "timestamp" : 1693070640
        }
      },
      .....
    ]
  }
}
```

`took` 字段和 `timed_out` 字段很容易理解，它们简单地表示 Elasticsearch 返回响应所花费的毫秒数，以及查询是否超时。

`_shards` 字段告诉我们在此搜索操作中涉及了多少分片，其中多少返回成功，多少失败，以及多少被跳过。

`hits` 字段包含从搜索返回的文档。每个文档根据其与搜索相关性而得分。hits 字段还包含一个 `total` 字段，提到返回的文档总数，以及文档的最大分数。

最后，在嵌套字段 `hits` 中，我们获取所有相关文档，包括它们的 `_id` 和它们的 `score`。文档按它们的分数排序。

## 一个基本的搜索查询

让我们开始构建我们的搜索查询。我们可以从一个简单的查询开始，并剖析其中的问题 -

```py
GET news/_search
{
  "query": {
    "multi_match": {
      "query": "robbery",
      "fields": ["headline", "short_description"]
    }
  }
}
```

这是一个非常简单的查询，它只是找到所有包含单词“抢劫”的文档，无论其出现在哪些字段中，即 `headline` 或 `short_description`。

它返回了一些结果，我们可以看到它们中都有单词“抢劫”。

```py
{
  .....
  "hits" : {
    "total" : {
      "value" : 3,
      "relation" : "eq"
    },
    "max_score" : 8.164355,
    "hits" : [
      {
        "_index" : "news",
        "_id" : "hjrouIsBC1dvdsZHgWdm",
        "_score" : 8.164355,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/lady-gaga-dog-walker-reward_n_62d82efee4b000da23fafad7",
          "headline" : "$5K Reward For Suspect In Shooting Of Lady Gaga’s Dog Walker",
          "short_description" : "One of the men involved in the violent robbery was mistakenly released from custody in April and remains missing.",
          "category" : "U.S. NEWS",
          "authors" : "STEFANIE DAZIO, AP",
          "country" : "IN",
          "timestamp" : 1694863246
        }
      },
      {
        "_index" : "news",
        "_id" : "RzrouIsBC1dvdsZHf2cP",
        "_score" : 7.4066687,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/guard-cat-hailed-as-hero_n_62e9a515e4b00f4cf2352a6f",
          "headline" : "Bandit The 'Guard Cat' Hailed As Hero After Thwarting Would-Be Robbery",
          "short_description" : "When at least two people tried to break into a Tupelo, Mississippi, home last week, the cat did everything she could to alert its owner.",
          "category" : "WEIRD NEWS",
          "authors" : "",
          "country" : "US",
          "timestamp" : 1693070640
        }
      },
      {
        "_index" : "news",
        "_id" : "WTrouIsBC1dvdsZHp2wd",
        "_score" : 7.4066687,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/san-francisco-news-crew-security-guard-shot-killed_n_61a2a9d8e4b0ae9a42af278a",
          "headline" : "News Crew Security Guard Dies After Being Shot While Helping Robbery Coverage",
          "short_description" : "Kevin Nishita was shot in the abdomen while doing his job amid an uptick in organized retail crime.",
          "category" : "CRIME",
          "authors" : "Daisy Nguyen, AP",
          "country" : "US",
          "timestamp" : 1692480894
        }
      }
    ]
  }
}
```

# 词法搜索

到目前为止，我们所涉及的被称为“词法搜索”。在这种类型的搜索中，系统寻找文档中给定单词或短语的精确匹配。实质上，当用户输入“抢劫”时，我们的搜索查询识别出所有包含确切术语“抢劫”的文档。尽管这种方法最初可能看起来直观，但它的局限性很快就会显现出来，正如我们马上就会发现的那样。

# 我们当前搜索查询中的问题

## 相似的词语会返回不同的结果

让我们举几个例子，看看当用户搜索“被抢劫”时我们会得到什么 -

```py
GET news/_search
{
  "query": {
    "multi_match": {
      "query": "robbed",
      "fields": ["headline", "short_description"]
    }
  }
}
```

这些是我收到的结果 -

```py
{
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 7.9044275,
    "hits" : [
      {
        "_index" : "news",
        "_id" : "YTrouIsBC1dvdsZHh2jf",
        "_score" : 7.9044275,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/multiple-guns-robbery-wellston-market-missouri_n_62994cbee4b05fe694f296ad",
          "headline" : "Man Robbed Of Assault Rifle At Gunpoint Opens Fire With Second Gun",
          "short_description" : "The accused robber was struck multiple times, and two bystanders were wounded in the St. Louis shootout.",
          "category" : "CRIME",
          "authors" : "Mary Papenfuss",
          "country" : "IN",
          "timestamp" : 1691458552
        }
      },
      {
        "_index" : "news",
        "_id" : "YDrouIsBC1dvdsZH73UQ",
        "_score" : 7.8303137,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/michigan-militia-training-video-gretchen-whitmer_n_5f8b6e26c5b6dc2d17f78e0a",
          "headline" : "Chilling Training Videos Released Of Militia Men Charged In Michigan Gov. Kidnap Plot",
          "short_description" : "\"I’m sick of being robbed and enslaved by the state ... they are the enemy. Period,\" says one suspect in a video.",
          "category" : "POLITICS",
          "authors" : "Mary Papenfuss",
          "country" : "IN",
          "timestamp" : 1692613291
        }
      }
    ]
  }
```

为了简单起见，这些是我收到的文档的标题 -

```py
1\. "Man Robbed Of Assault Rifle At Gunpoint Opens Fire With Second Gun"
2\. "Chilling Training Videos Released Of Militia Men Charged In Michigan Gov. Kidnap Plot"
```

这两篇文档中都包含“被抢劫”一词，无论是在标题还是描述中。但是如果用户搜索的是“抢劫”，那么我们将会在结果中看到完全不同的文档 -

```py
[
      {
        "_index" : "news",
        "_id" : "hjrouIsBC1dvdsZHgWdm",
        "_score" : 8.164355,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/lady-gaga-dog-walker-reward_n_62d82efee4b000da23fafad7",
          "headline" : "$5K Reward For Suspect In Shooting Of Lady Gaga’s Dog Walker",
          "short_description" : "One of the men involved in the violent robbery was mistakenly released from custody in April and remains missing.",
          "category" : "U.S. NEWS",
          "authors" : "STEFANIE DAZIO, AP",
          "country" : "IN",
          "timestamp" : 1694863246
        }
      },
      {
        "_index" : "news",
        "_id" : "YTrouIsBC1dvdsZHh2jf",
        "_score" : 8.079888,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/multiple-guns-robbery-wellston-market-missouri_n_62994cbee4b05fe694f296ad",
          "headline" : "Man Robbed Of Assault Rifle At Gunpoint Opens Fire With Second Gun",
          "short_description" : "The accused robber was struck multiple times, and two bystanders were wounded in the St. Louis shootout.",
          "category" : "CRIME",
          "authors" : "Mary Papenfuss",
          "country" : "IN",
          "timestamp" : 1691458552
        }
      },
      {
        "_index" : "news",
        "_id" : "RzrouIsBC1dvdsZHf2cP",
        "_score" : 7.4066687,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/guard-cat-hailed-as-hero_n_62e9a515e4b00f4cf2352a6f",
          "headline" : "Bandit The 'Guard Cat' Hailed As Hero After Thwarting Would-Be Robbery",
          "short_description" : "When at least two people tried to break into a Tupelo, Mississippi, home last week, the cat did everything she could to alert its owner.",
          "category" : "WEIRD NEWS",
          "authors" : "",
          "country" : "US",
          "timestamp" : 1693070640
        }
      },
      {
        "_index" : "news",
        "_id" : "WTrouIsBC1dvdsZHp2wd",
        "_score" : 7.4066687,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/san-francisco-news-crew-security-guard-shot-killed_n_61a2a9d8e4b0ae9a42af278a",
          "headline" : "News Crew Security Guard Dies After Being Shot While Helping Robbery Coverage",
          "short_description" : "Kevin Nishita was shot in the abdomen while doing his job amid an uptick in organized retail crime.",
          "category" : "CRIME",
          "authors" : "Daisy Nguyen, AP",
          "country" : "US",
          "timestamp" : 1692480894
        }
      }
    ]
```

```py
1\. "$5K Reward For Suspect In Shooting Of Lady Gaga's Dog Walker"
2\. "Man Robbed Of Assault Rifle At Gunpoint Opens Fire With Second Gun"
3\. "Bandit The 'Guard Cat' Hailed As Hero After Thwarting Would-Be Robbery"
4\. "News Crew Security Guard Dies After Being Shot While Helping Robbery Coverage"
```

简而言之，如果用户搜索“抢劫”和“被抢劫”，我们会得到不同的结果。如果用户搜索了这些（或任何与“抢劫”相关的内容），我们应该显示所有包含“抢劫”不同形式的文档（称为[“屈折”形式](https://en.wikipedia.org/wiki/Inflection)）的文档。

## 对用户需求的理解不足

> “设计师的目标是倾听、观察、理解、同情、共鸣、综合，并汲取洞察力，使他或她能够使不可见的变得可见。” — 希尔曼·柯蒂斯

我们正在尝试检索与用户查询相符的文档，这一任务超出了用户输入的范围。通过*深入探讨*附加参数，我们可以获得对用户偏好和需求的更丰富理解。

例如，当用户搜索新闻时，他们的兴趣可能不仅仅在于相关性；新闻文章的时效性往往也很重要。为了提高我们的搜索精度，我们可以微调评分机制。

此外，我们的文章中还有一个`location`字段。该字段表示新闻的地理来源，提供了进一步优化结果的机会。我们可以利用这一点来提高来自用户所在国家的文章的排名。

## 相似词未被返回

由于我们仅返回与用户查询完全匹配的文章，我们很可能会错过包含类似词的相关文档。例如，如果我搜索“盗窃”，我会得到以下文章，

```py
[
      {
        "_index" : "news",
        "_id" : "dzrouIsBC1dvdsZHiGh1",
        "_score" : 8.079888,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/ap-us-church-theft-beheaded-statue_n_629504abe4b0933e7376f2fa",
          "headline" : "Angel Statue Beheaded At Church, $2 Million Relic Stolen",
          "short_description" : "The New York City church says its stolen 18-carat gold relic was guarded by its own security system and is irreplaceable due to its historical and artistic value.",
          "category" : "CRIME",
          "authors" : "Michael R. Sisak, AP",
          "country" : "IN",
          "timestamp" : 1699477455
        }
      },
      {
        "_index" : "news",
        "_id" : "ATrouIsBC1dvdsZHrG2n",
        "_score" : 7.4066687,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/joseph-sobolewski-charge-dropped-mountain-dew-felony_n_617a15e0e4b0657357447ee2",
          "headline" : "Prosecutors Drop Felony Charge Against Man Accused Of 43 Cent Soda Theft",
          "short_description" : "Joseph Sobolewski faced up to seven years in prison after paying $2 for a Mountain Dew that cost $2.29 plus tax.",
          "category" : "U.S. NEWS",
          "authors" : "Nick Visser",
          "country" : "IN",
          "timestamp" : 1698883200
        }
      },
      {
        "_index" : "news",
        "_id" : "ZDrouIsBC1dvdsZH73Uq",
        "_score" : 7.153779,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/missing-lemur-found_n_5f8b2c33c5b6dc2d17f76bdb",
          "headline" : "'There's A Lemur!' 5-Year-Old Helps Crack San Francisco Zoo Theft Case",
          "short_description" : """The arthritic, 21-year-old lemur is "agitated" but safe, zoo staff say.""",
          "category" : "U.S. NEWS",
          "authors" : "",
          "country" : "IN",
          "timestamp" : 1698560597
        }
      }
    ]
```

“抢劫”一词可能与“盗窃”一词意义不同，但它仍然是一个相关的词，用户可能也对包含“抢劫”一词的文章感兴趣（虽然相关性评分低于包含用户搜索的确切词的文档）。

盗窃的类似词可能有很多，每个词的相似度不同。例如，“盗窃”可能与“商店盗窃”更相似，而与“入室盗窃”的相似度较低。但在某些上下文中，它们可能是同义词，并且具有一定的相关性，尽管不如查询中的确切词汇“盗窃”相关。

我们当前的搜索没有考虑文档和查询中的词语相似性。如果用户搜索“盗窃”，只会返回包含“盗窃”一词的文章，而我们还应该返回包含与“盗窃”类似词汇（如“入室盗窃”或“抢劫”）的文章。

## 错别字被忽略

> “如果用户不能使用它，那它就不起作用。” — 苏珊·德雷

另一个问题是，用户的任何错别字会导致返回空结果。我们知道用户可能会不小心输入错别字，我们不希望返回空结果。例如，在 Google 新闻上搜索“robbey”仍然会返回与“抢劫”相关的结果。

![](../Images/2e1ded7685eacd2857f18ec016254422.png)

## 不同的词语组合有不同的含义

让我们来看一个例子，假设用户提出了这个查询 —

```py
GET news/_search
{
  "query": {
    "multi_match": {
      "query": "new jersey covid virus"
    }
  }
}
```

对于你和我来说，显然用户想要搜索与“covid”或“virus”相关的新闻，“New Jersey”。但对于我们的搜索引擎来说，每个词的含义是相同的，它无法理解词语的顺序是否重要（例如，“New”和“Jersey”在“New Jersey”中的顺序）。

让我们看一下前三个结果，

```py
{
        "_index" : "news",
        "_id" : "0jrouIsBC1dvdsZH03FH",
        "_score" : 15.199991,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/covid-new-york-new-jersey-trend_n_60611769c5b6531eed0621da",
          "headline" : "Virus Fight Stalls In Early Hot Spots New York, New Jersey",
          "short_description" : "New Jersey has been reporting about 647 new cases for every 100,000 residents over the past 14 days. New York has averaged 548.",
          "category" : "U.S. NEWS",
          "authors" : "Marina Villeneuve and Mike Catalini, AP",
          "country" : "US",
          "timestamp" : 1697056489
        }
      },
      {
        "_index" : "news",
        "_id" : "zzrouIsBC1dvdsZH23Ig",
        "_score" : 12.708103,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/new-variants-raise-worry-about-covid-19-virus-reinfections_n_602193e6c5b689330e31dcc4",
          "headline" : "New Variants Raise Worry About COVID-19 Virus Reinfections",
          "short_description" : "Scientists discovered a new version of the virus in South Africa that’s more contagious and less susceptible to certain treatments.",
          "category" : "WORLD NEWS",
          "authors" : "Marilynn Marchione, AP`",
          "country" : "IN",
          "timestamp" : 1693063095
        }
      },
      {
        "_index" : "news",
        "_id" : "fTrouIsBC1dvdsZH6HQF",
        "_score" : 11.707885,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/new-york-covid-19-religious-gatherings_n_5fbf42b8c5b66bb88c6430ac",
          "headline" : "Supreme Court Blocks New York COVID-19 Restrictions On Religious Gatherings",
          "short_description" : "It was the first major decision since Justice Amy Coney Barrett joined the nation's highest court.",
          "category" : "POLITICS",
          "authors" : "Lawrence Hurley, Reuters",
          "country" : "IN",
          "timestamp" : 1693362371
        }
      },
```

如果你仔细看上面的结果，你会注意到第二个结果，“新变种引发对COVID-19病毒再感染的担忧”，与新泽西毫无关系。事实上，在阅读描述后，它似乎与南非的COVID-19感染更相关！

这是因为“COVID”、“virus”和“New”这些词是文档的一部分，因此文档得分较高。然而，这与用户查询完全不相关。我们的搜索系统无法理解“New”和“Jersey”应被视为一个单独的词组。

# 改进我们的搜索。

## 提升更相关的字段。

> “言辞有重量，因此如果你要说重要的事情，请确保选择正确的词。” — Lang Leav

我们可以决定提升某些字段或某些值，这些可能更有助于理解文章的内容。例如，文章的标题可能比文章的描述更有意义。

让我们来看一个示例查询。假设用户试图搜索选举，这将是我们的 Elasticsearch 查询 —

```py
GET news/_search
{
  "query": {
    "multi_match": {
      "query": "elections",
      "type": "most_fields", 
      "fields": ["short_description", "headline"]
    }
  }
}
```

这些是我们得到的结果 -

```py
{
        "_index" : "news",
        "_id" : "qDrouIsBC1dvdsZHwW-a",
        "_score" : 15.736175,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/new-york-city-board-of-elections-mess_n_60de223ee4b094dd26898361",
          "headline" : "Why New York City’s Board Of Elections Is A Mess",
          "short_description" : "“There’s a fundamental problem having partisan boards of elections,” said a New York elections attorney.",
          "category" : "POLITICS",
          "authors" : "Daniel Marans",
          "country" : "IN",
          "timestamp" : 1689878099
        }
      },
      {
        "_index" : "news",
        "_id" : "8zrouIsBC1dvdsZH63Si",
        "_score" : 7.729385,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/20-funniest-tweets-from-women-oct-31-nov-6_n_5fa209fac5b686950033b3e7",
          "headline" : "The 20 Funniest Tweets From Women This Week (Oct. 31-Nov. 6)",
          "short_description" : "\"Hear me out: epidurals, but for elections.\"",
          "category" : "WOMEN",
          "authors" : "Caroline Bologna",
          "country" : "IN",
          "timestamp" : 1694723723
        }
      },
      {
        "_index" : "news",
        "_id" : "zzrouIsBC1dvdsZH8nVe",
        "_score" : 7.353842,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/childrens-books-elections-voting_l_5f728844c5b6f622a0c368a1",
          "headline" : "25 Children's Books That Teach Kids About Elections And Voting",
          "short_description" : "Parents can use these stories to educate their little ones about the American political process.",
          "category" : "PARENTING",
          "authors" : "Caroline Bologna",
          "country" : "IN",
          "timestamp" : 1697290393
        }
      },
```

如果你看第二篇文章“本周20条最有趣的女性推文（10月31日至11月6日）”，你会发现它似乎与选举无关。然而，由于描述中出现了“election”一词，Elasticsearch认为这是一个相关的结果。也许我们还有改进的空间。直觉上来说，与用户查询匹配的标题相符的文章更相关。为了实现这一点，我们可以指示Elasticsearch提升`heading`字段，从而在评分计算中赋予它更大的重要性。

在我们的查询中，这很简单 -

```py
GET news/_search
{
  "query": {
    "multi_match": {
      "query": "elections",
      "type": "most_fields", 
      "fields": ["headline^4", "short_description"]
    }
  }
}
```

注意我在`fields`中放置的`heading^4`。这意味着“heading”字段被提升了4倍。现在让我们看看结果，

```py
{
        "_index" : "news",
        "_id" : "qDrouIsBC1dvdsZHwW-a",
        "_score" : 37.7977,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/new-york-city-board-of-elections-mess_n_60de223ee4b094dd26898361",
          "headline" : "Why New York City’s Board Of Elections Is A Mess",
          "short_description" : "“There’s a fundamental problem having partisan boards of elections,” said a New York elections attorney.",
          "category" : "POLITICS",
          "authors" : "Daniel Marans",
          "country" : "IN",
          "timestamp" : 1689878099
        }
      },
      {
        "_index" : "news",
        "_id" : "zzrouIsBC1dvdsZH8nVe",
        "_score" : 29.415367,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/childrens-books-elections-voting_l_5f728844c5b6f622a0c368a1",
          "headline" : "25 Children's Books That Teach Kids About Elections And Voting",
          "short_description" : "Parents can use these stories to educate their little ones about the American political process.",
          "category" : "PARENTING",
          "authors" : "Caroline Bologna",
          "country" : "IN",
          "timestamp" : 1697290393
        }
      },
      {
        "_index" : "news",
        "_id" : "_jrouIsBC1dvdsZH3HKZ",
        "_score" : 29.415367,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/shirley-weber-first-black-california-secretary-of-state_n_6014651ec5b6aa4bad33e87b",
          "headline" : "Shirley Weber Sworn In As California's First Black Elections Chief",
          "short_description" : "She vacates her Assembly seat to be the new secretary of state, replacing Alex Padilla, who last week became the first Latino U.S. senator for California.",
          "category" : "POLITICS",
          "authors" : "Sarah Ruiz-Grossman",
          "country" : "IN",
          "timestamp" : 1697300728
        }
      },
      {
        "_index" : "news",
        "_id" : "NzrouIsBC1dvdsZHnWvd",
        "_score" : 26.402336,
        "_source" : {
          "link" : "https://www.huffpost.com/entry/josh-hawley-democrats-dont-accept-elections_n_61ea1949e4b01440a689bedc",
          "headline" : "Sen. Josh Hawley Says, Without Irony, That Democrats Don't Accept Elections They Lose",
          "short_description" : "The Missouri Republican led the charge on Jan. 6 to object to Joe Biden's win -- right after he saluted pro-Trump protesters gathering at the U.S. Capitol.",
          "category" : "POLITICS",
          "authors" : "Josephine Harvey",
          "country" : "IN",
          "timestamp" : 1692046727
        }
      },
```

现在我们可以看到所有前几个结果的标题中都包含“election”一词，因此返回的文章更相关。

## 基于函数的提升

虽然我们已经提升了某些字段，但我们也希望根据用户在搜索新闻时想要的内容引入两种新的提升类型。

1.  我们希望提升用户所在国家的文章。我们不仅仅想基于国家进行过滤，因为这可能导致无关紧要的结果出现在顶部，但我们也不希望完全忽略它。简而言之，我们希望给用户所在国家的文章更多的权重。

1.  我们希望提升最新新闻的排名。我们不仅仅是希望根据时效性进行排序，因为这也可能导致无关的结果出现在顶部。相反，我们希望将时效性与相关性进行平衡。

让我们看看如何实现这一点。

在Elasticsearch中，我们可以使用`function_score`查询来应用自定义评分函数，包括提升评分。`function_score`查询允许你根据各种函数修改文档的分数。简而言之，我们可以根据条件提升某些文档的评分。

让我们从提升用户所在国家的权重开始。假设用户的国家是“US”，然后将其插入到发送给Elasticsearch的查询中。为此，我们需要添加一个`function_score`块，它允许将自定义评分函数应用于查询结果。我们可以为给定的查询定义多个`functions`，指定匹配文档的条件和提升值。

我们可以定义一个函数来提升用户所在国家的权重 —

```py
{
  "filter": {
    "term": {
      "country.keyword": "US"
    }
  },
  "weight": 2
}
```

这将提升国家为“US”的文章的分数2倍。

接下来，让我们尝试将最新的新闻置顶。我们可以通过使用`field_value_factor`来实现这一点。`field_value_factor`函数允许我们使用文档中的一个字段来影响其分数，这正是我们所需要的。让我们看看效果如何 —

```py
{
  "field_value_factor": {
    "field": "timestamp",
    "factor": 2
  }
}
```

`factor`术语指定了指定字段的值应如何影响分数的乘数或因子。使用这个函数，具有较新时间戳的文档将被给予更高的分数。

我们的完整查询变成 —

```py
GET news/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "multi_match": {
            "query": "covid",
            "fields": ["headline^4", "short_description"]
          }
        },
        {
          "function_score": {
            "query": {
              "multi_match": {
                "query": "covid",
                "fields": ["headline^4", "short_description"]
              }
            },
            "functions": [
              {
                "filter": {
                  "term": {
                    "country.keyword": "US"
                  }
                },
                "weight": 2
              }, {
                "field_value_factor": {
                  "field": "timestamp",
                  "factor": 2
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```

现在，最新的文档和来自用户所在国家的文档将获得更高的分数。我们可以通过配置`weight`和`factor`字段的值来调整这种平衡。

## 模糊查询

接下来，让我们修正搜索查询中的拼写错误。

在Elasticsearch中，我们可以执行模糊搜索，以检索即使存在拼写或字符的轻微变化也匹配指定术语的文档。为此，我们只需在查询中添加一个`fuzziness`字段。我们的最终查询变成 —

```py
GET news/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "multi_match": {
            "query": "covi",
            "fields": ["headline^4", "short_description"],
            "fuzziness": 1
          }
        },
        {
          "function_score": {
            "query": {
              "multi_match": {
                "query": "covi",
                "fields": ["headline^4", "short_description"],
                "fuzziness": 1
              }
            },
            "functions": [
              {
                "filter": {
                  "term": {
                    "country.keyword": "US"
                  }
                },
                "weight": 2
              }, {
                "field_value_factor": {
                  "field": "timestamp",
                  "factor": 2
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```

拼写纠正不仅仅是简单地添加`fuzziness`。[如果你想了解更多，请查看这篇博客文章](https://queryunderstanding.com/spelling-correction-471f71b19880)。

# 结论

在这篇博客文章中，我们深入探讨了Elasticsearch的细节，从实际操作一个样本数据集和构建搜索查询的基础知识开始。我们揭开了Elasticsearch响应的神秘面纱，并通过一个基本的搜索查询，为有效探索奠定了基础。

在我们探索词汇搜索的过程中，我们认识到当前搜索方法的一些怪癖。为了解决这些挑战，我们引入了提升和模糊度——这些都是微调搜索和处理现实世界数据复杂性的有用工具。

在我们结束这里的讨论时，视此为我们追求搜索卓越之旅中的一次短暂停留。在下一部分，我们将**深入探讨**解决当前搜索方法中特定问题的高级策略。准备好迎接语义搜索的迷人世界吧，在这里，重点不再仅仅是匹配关键词，而是理解其背后的含义，从而为更直观、更具上下文意识的搜索体验铺平道路。准备好将你的Elasticsearch冒险提升到一个新的水平吧！

喜欢这次Elasticsearch的旅程吗？在Medium上关注我，获取更多文章。想要更快的知识点（例如我正在阅读的内容、速查表等），[在LinkedIn上关注我](https://www.linkedin.com/in/sanil-khurana-a2503513b/)，获取定期的短内容（例如，在阅读Elasticsearch时，我简要讨论了一个特定的评分函数tf-idf的工作原理，在这篇[5分钟的帖子](https://www.linkedin.com/posts/sanil-khurana-a2503513b_searchengine-softwaredevelopment-data-activity-7127859782736089088-fi3H?utm_source=share&utm_medium=member_desktop)中）。让我们在技术和数据的探索中保持联系吧！
