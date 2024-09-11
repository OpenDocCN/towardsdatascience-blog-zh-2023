# 使用ChatGPT查询你的Neo4j数据库

> 原文：[https://towardsdatascience.com/use-chatgpt-to-query-your-neo4j-database-78680a05ec2?source=collection_archive---------4-----------------------#2023-01-31](https://towardsdatascience.com/use-chatgpt-to-query-your-neo4j-database-78680a05ec2?source=collection_archive---------4-----------------------#2023-01-31)

## 一种简单的方法让ChatGPT学习你的图谱结构，并帮助你构建Cypher查询

[](https://bratanic-tomaz.medium.com/?source=post_page-----78680a05ec2--------------------------------)[![Tomaz Bratanic](../Images/d5821aa70918fcb3fc1ff0013497b3d5.png)](https://bratanic-tomaz.medium.com/?source=post_page-----78680a05ec2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----78680a05ec2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----78680a05ec2--------------------------------) [Tomaz Bratanic](https://bratanic-tomaz.medium.com/?source=post_page-----78680a05ec2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F57f13c0ea39a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-chatgpt-to-query-your-neo4j-database-78680a05ec2&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=post_page-57f13c0ea39a----78680a05ec2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----78680a05ec2--------------------------------) ·8分钟阅读·2023年1月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F78680a05ec2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-chatgpt-to-query-your-neo4j-database-78680a05ec2&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=-----78680a05ec2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F78680a05ec2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-chatgpt-to-query-your-neo4j-database-78680a05ec2&source=-----78680a05ec2---------------------bookmark_footer-----------)

ChatGPT已经席卷了整个世界。几乎不可能不被各种关于它的信息轰炸。我想，AI生成内容泛滥只是时间问题。

我一直在使用ChatGPT，并尝试在空闲时间生成Cypher查询。我发现了一篇[利用ChatGPT帮助建模图谱并在Neo4j中创建一些基本查询的博客文章](https://neo4j.com/developer-blog/create-neo4j-database-model-with-chatgpt/)。因此，我决定记录一下使用ChatGPT作为question2cypher或english2cypher引擎的经验。

## Neo4j Aura 设置

我们将使用 [Neo4j Aura](https://neo4j.com/cloud/platform/aura-graph-database/)，它提供了一个带有预填充数据的 Neo4j 数据库的免费云实例，进行这次实验。登录后，点击 **New Instance** 并选择 **Graph based Recommendations** 数据集。

![](../Images/0fb9bf2f8cf4daee749ee178e646c67c.png)

选择 Graph Based Recommendations 数据集。图片由作者提供。

在数据库实例启动后，点击 **Query** 按钮以打开 Neo4j Browser。

## ChatGPT 设置

接下来，如果你还没有设置 ChatGPT，你需要进行设置。打开 [ChatGPT 网站](https://chat.openai.com/) 并按照注册说明进行操作。完成后，你应该能看到 ChatGPT 用户界面。

![](../Images/9fc679acec161f85fc0ded96db527814.png)

ChatGPT 用户界面。图片由作者提供。

## 定义图 schema

ChatGPT 对使用 Cypher 和属性图模型有一定经验。然而，它并不了解你的图的 schema。因此，为了避免 ChatGPT 猜测并产生不准确的节点标签和属性，提供相关信息是至关重要的。

我认为告知 ChatGPT 节点标签和关系类型的最佳方式是使用以下 APOC 过程。

```py
CALL apoc.meta.stats
YIELD labels, relTypes
```

*结果*

![](../Images/ddd3138090361a8ec2bb7c4fcf24ec84.png)

Apoc schema 结果。图片由作者提供。

假设你在 Neo4j Browser 中运行了上述 APOC schema 过程。在这种情况下，你可以简单地复制响应的文本版本并将其粘贴到 ChatGPT 中，并附上额外的信息，即这是 Neo4j 数据库的 schema 定义。

> 我有一个具有以下 schema 的 Neo4j 数据库：
> 
> —APOC schema 过程的结果—

![](../Images/0eacebf8d0df07d380d3adfbfcde0c7c.png)

图片由作者提供。

我得到以下响应，但不同的执行可能会有显著不同的反应。接下来，你需要提供关于节点和关系属性的信息，因为这些在 APOC schema 过程中并不可用。我们可以使用以下 Cypher 查询来提取所有属性键及其节点和关系类型。

```py
MATCH (n)
UNWIND keys(n) AS key
RETURN labels(n)[0] AS label, collect(distinct key) AS propertyKeys
UNION
MATCH ()-[r]->()
UNWIND keys(r) AS key
RETURN type(r) AS label, collect(distinct key) AS propertyKeys
```

你可以通过告知 ChatGPT 你正在提供有关节点和关系类型的信息来将这些信息输入 ChatGPT。

![](../Images/61d309b0a38de35839533a17b988abe5.png)

提供节点和关系属性键。图片由作者提供。

## 使用 ChatGPT 生成 Cypher 查询

现在我们可以继续测试 ChatGPT 基于各种提示生成 Cypher 查询的效果。我们需要告知 ChatGPT 它将生成 Cypher 查询。我发现需要在初始提示中说明的是，ChatGPT 不应对来自先前提示的查询做任何假设，否则它会尝试将来自不相关的先前提示的信息合并到新生成的 Cypher 查询中。你也可以选择省略查询解释。

> 现在你将根据提示生成Cypher查询。除非明确指定，否则不要发布任何解释。同时也不要对上下文做任何假设。只查看给定提示的上下文，不要从之前的提示中假设任何其他内容。

*我已经明确要求它不要在之前的提示中做任何假设，因为最初它假设了我的电影偏好，因为我经常询问基努·里维斯和喜剧。生成的查询在你要求它忘记或不忘记时似乎大致相同。唯一的区别是当我要求推荐时，它假设我喜欢基努·里维斯和喜剧，因此采用了这一点。此外，图谱模式的遗忘在这两种情况下都会发生，无论你是否要求它忘记之前的提示。*

我们可以从简单的开始。

![](../Images/d3b2c085318b53739526525f4704b463.png)

图片由作者提供。

提供的查询按预期工作。我们现在可以尝试一些更复杂的。

![](../Images/0e16155430620246afd8e37539eed3ce.png)

图片由作者提供。

再次，查询按预期工作。ChatGPT在基本的图形模式匹配和聚合方面相当出色。它大多数时候也能选择正确的节点属性。然而，它在所谓的存在性查询上存在问题，这类查询可能会返回true（如果模式存在）或false（如果模式不匹配）。

![](../Images/07415f4c4742b8c91d593c8125ef44cb.png)

图片由作者提供。

它似乎更喜欢`MATCH`子句，并且尚未掌握`OPTIONAL MATCH`或其他允许空值的方法。然而，在一些帮助下，它能够完成任务。

![](../Images/17371062b3a516b7350d065ba51aa46d.png)

图片由作者提供。

唯一的问题是它忘记了基努·里维斯。

所以它只是检查一个演员是否出现在《黑客帝国》中。

![](../Images/a8cc0b156c1645c385903b4d11fc6cfa.png)

图片由作者提供。

感觉就像是和一个有很多创造力但注意力几乎为零的人进行橡皮鸭编程，因为查询再次忘记了`OPTIONAL MATCH`。也许ChatGPT只是有无法治愈的注意力缺陷综合症。如果你希望它生成允许空值或0值的查询，它绝对需要一些微调。

接下来，我们可以检查ChatGPT的创造力。我们可以让它找到最受欢迎的喜剧，而不指定“受欢迎”的定义。

![](../Images/5e5524c2624de3d878d315af14b9dfc1.png)

图片由作者提供。

生成的查询非常出色。ChatGPT可能可以从许多电影推荐教程中学习。此外，它正确地假设了**imdbRating**属性包含某种流行度评分值。

我们可以要求它在数据库中包括用户评分。

![](../Images/3ad08fcb4dfa2e33fa5db016d3799c1e.png)

图片由作者提供。

起初，我感到印象深刻。尽管它没有将IMDB评分和用户评分结合起来，但它为用户评分提供了一个不错的开始。然而，它忘记了流派是一个独立的节点，而不是节点属性。

![](../Images/4199ecf0521084e0945c685cb2d07d04.png)

图片由作者提供。

尽管修正后的 Cypher 查询现在正确地将类型识别为单独的节点，但由于某种原因，它决定将流行度定义为 IMDB 投票数。因此，即使你获得一个有效的 Cypher 查询，也不能保证下次会得到相同的 Cypher 语句，尽管它声称一致性很重要。

让我们尝试另一个对 Cypher 创意有要求的提示。

![](../Images/22c24e1b4ac210d542bdc766ef0028b1.png)

作者提供的图片。

尽管它抱怨无法提供好的推荐，但它仍然提供了一个好的起点。让我们看看是否可以尝试改进推荐。

![](../Images/c383d321c37895f512f2b3b10e700dff.png)

作者提供的图片。

似乎 ChatGPT 有一些关于使用共同演员的平均电影评分的想法，然后提供与基努·里维斯在评分最高的电影中共同出演的演员作为推荐。

唯一的问题是上述查询无效。再次出现了 ChatGPT 虚构的节点属性**imdb_rating**而不是**imdbRating**。此外，它在第四行中收集电影评分，然后尝试对其取平均值。不幸的是，Cypher 不允许使用 `avg()` 操作符对数组进行平均。因此，我们需要去掉 `collect` 或使用 APOC。

![](../Images/77c8711ddc988f785953517382e6facd.png)

作者提供的图片。

现在，ChatGPT 的 ADHD 决定我们应该继续使用它最初的基本推荐。然而，如果你使用的是 Neo4j v5，还会遇到另一个问题。[在 v5 中，几种 Cypher 语法已被弃用](/how-cypher-changed-in-neo4j-v5-d0f10cbb60bf)，而 ChatGPT 对此一无所知，需要用最新的 Cypher 语法进行训练。在上述示例中，`size()` 操作符在 Neo4j v5 中不能再用于计算图模式。

最后，我想尝试一下 ChatGPT 是否能作为图数据科学家。

![](../Images/ae603eff693eb719afffa381282a9019.png)

作者提供的图片。

默认情况下，ChatGPT 想要使用已弃用的图算法库。将其修正为使用图数据科学库可以稍微改善查询，但效果不完全。GDS 库自 2.0 版本以来不支持匿名图。因此，你需要单独投影一个内存图，然后在其上运行 PageRank。

![](../Images/1a94c35c5eabd22eed6267355a88a377.png)

作者提供的图片。

这是一些好的想法，但语法仍然无法使用。不过，它可以作为一个好的起点。

## 结论

ChatGPT 展现出作为 Neo4j 数据库查询接口的巨大潜力。它在基本的图匹配和聚合方面表现良好。不幸的是，它有时会忘记确切的属性名称或图模式，但在一点帮助下，它能够记住。

但是，当你想生成复杂的查询时，该查询是否有效的可能性是相等的。调试复杂查询感觉像是与一个注意力短暂的 ADHD 人士进行橡皮鸭编程。虽然它可能解决你明确提到的 bug，但也可能改变其他变量或引入新的 bug。ChatGPT 的一致性不是它的强项。此外，它没有接受 Neo4j v5 或图数据科学库中使用的更新 Cypher 语法的训练。因此，需要额外的微调才能正确。

总的来说，我感觉现阶段的 ChatGPT 可以用来生成简单的查询和模式。对于更复杂的任务，比如提供推荐，它可以用来提示 Cypher 语法或如何解决问题，但绝对不应被依赖来提供有效的查询。我很期待看到基于更新数据训练的新版 ChatGPT 的表现。我也可能尝试微调 ChatGPT 或 GPT-3，看看效果如何。

*p.s. 文章由* [*黄思行*](https://medium.com/u/ff9d63e09a67?source=post_page-----78680a05ec2--------------------------------) *撰写，关于* [*使用 GPT-3 生成 Cypher 查询*](/gpt-3-for-doctor-ai-1396d1cd6fa5)*.*
