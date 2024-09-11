# 如何在 Neo4j 图数据科学库中使用 Cypher 聚合

> 原文：[https://towardsdatascience.com/how-to-use-cypher-aggregations-in-neo4j-graph-data-science-library-5d8c40c2670c?source=collection_archive---------11-----------------------#2023-03-27](https://towardsdatascience.com/how-to-use-cypher-aggregations-in-neo4j-graph-data-science-library-5d8c40c2670c?source=collection_archive---------11-----------------------#2023-03-27)

## 利用 Cypher 聚合功能，在内存图中展示所有 Cypher 查询语言的灵活性和表现力

[](https://bratanic-tomaz.medium.com/?source=post_page-----5d8c40c2670c--------------------------------)[![Tomaz Bratanic](../Images/d5821aa70918fcb3fc1ff0013497b3d5.png)](https://bratanic-tomaz.medium.com/?source=post_page-----5d8c40c2670c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d8c40c2670c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5d8c40c2670c--------------------------------) [Tomaz Bratanic](https://bratanic-tomaz.medium.com/?source=post_page-----5d8c40c2670c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F57f13c0ea39a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-cypher-aggregations-in-neo4j-graph-data-science-library-5d8c40c2670c&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=post_page-57f13c0ea39a----5d8c40c2670c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d8c40c2670c--------------------------------) ·7 min 阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5d8c40c2670c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-cypher-aggregations-in-neo4j-graph-data-science-library-5d8c40c2670c&user=Tomaz+Bratanic&userId=57f13c0ea39a&source=-----5d8c40c2670c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5d8c40c2670c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-cypher-aggregations-in-neo4j-graph-data-science-library-5d8c40c2670c&source=-----5d8c40c2670c---------------------bookmark_footer-----------)

Cypher 聚合是 Neo4j 图数据科学库的一个强大功能，允许用户使用灵活而表达力强的方法来投影内存中的图。尽管使用 Cypher 投影可以在很长一段时间内投影内存中的图，但它缺乏一些功能，最显著的是无法投影无向关系。因此，GDS 中添加了一种新的内存图投影方法，称为 Cypher 聚合。这篇博客将探讨 Neo4j 图数据科学库中 Cypher 聚合投影选项的语法和常见用法。

## 环境设置

如果你想跟随示例，可以打开一个 [Neo4j Sandbox 中的图数据科学项目](https://sandbox.neo4j.com/?usecase=graph-data-science2)。该项目包含有关机场、其位置和航班路线的小数据集。

我们可以使用以下 Cypher 语句可视化图模式：

```py
CALL db.schema.visualization()
```

![](../Images/4acad275cd607ceb29bbfb61ae524cd1.png)

图模式。图片由作者提供。

## 使用 Cypher 聚合投影内存中的图

首先，让我们快速回顾一下 Neo4j 图数据科学库的操作方式。

![](../Images/c329d740ce532c99b5eb95d5c9075543.png)

图数据科学库工作流。图片由作者提供。

在我们执行任何图算法之前，首先必须投影内存中的图。内存图不必是数据库中存储图的精确副本。我们可以选择图的子集，或者如你稍后将学到的，也可以投影未存储在数据库中的虚拟关系。投影内存图后，我们可以执行任意多的图算法，然后直接将结果流式传输给用户，或将其写回数据库。

## 使用 Cypher 聚合投影内存中的图

Cypher 聚合功能是图数据科学工作流的第一步，即在内存中投影图。它提供了 Cypher 查询语言的完全灵活性，可以在投影过程中选择、过滤或转换图。Cypher 聚合函数的语法如下：

```py
gds.alpha.graph.project(
    graphName: String,
    sourceNode: Node or Integer,
    targetNode: Node or Integer,
    nodesConfig: Map,
    relationshipConfig: Map,
    configuration: Map
)
```

只有前两个参数（graphName 作为 sourceNode）是必需的，但你需要指定 sourceNode 和 relationshipNode 参数以定义单个关系。我们将详细介绍你可能需要的大部分选项，以帮助你使用 Cypher 聚合来投影图。

我们将从一个简单的示例开始。假设我们想要投影所有 **Airport** 节点及其之间的 **HAS_ROUTE** 关系。

```py
MATCH (source:Airport)-[:HAS_ROUTE]->(target:Airport)
WITH gds.alpha.graph.project('airports', source, target) AS graph
RETURN graph.nodeCount AS nodeCount, 
       graph.relationshipCount AS relationshipCount
```

Cypher 语句以 MATCH 子句开始，该子句选择相关的图。要使用 Cypher 聚合定义关系，我们输入 **source** 和 **target** 节点。

当然，Cypher 查询语言提供了选择图的任何子集的灵活性。例如，我们可以只投影大洋洲的机场及其航班路线。

```py
MATCH (source:Airport)-[:HAS_ROUTE]->(target:Airport)
WHERE EXISTS {(source)-[:ON_CONTINENT]->(:Continent {name:"OC"})}
  AND EXISTS {(target)-[:ON_CONTINENT]->(:Continent {name:"OC"})}
WITH gds.alpha.graph.project('airports-oceania', source, target) AS graph
RETURN graph.nodeCount AS nodeCount, 
       graph.relationshipCount AS relationshipCount
```

在这个例子中，匹配的 Cypher 语句变得稍微复杂了一些，但 Cypher 聚合函数保持不变。**airports-oceania** 图包含 272 个节点和 973 条关系。如果你对 Cypher 有经验，你可能会注意到上述 Cypher 语句不会捕获任何在大洋洲没有航班路线的机场。

假设我们也想在投影中显示孤立机场。在这种情况下，我们需要稍微修改 Cypher 匹配语句。

```py
MATCH (source:Airport)
WHERE EXISTS {(source)-[:ON_CONTINENT]->(:Continent {name:"OC"})}
OPTIONAL MATCH (source)-[:HAS_ROUTE]->(target:Airport)
WHERE EXISTS {(target)-[:ON_CONTINENT]->(:Continent {name:"OC"})}
WITH gds.alpha.graph.project('airports-isolated', source, target) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

关系数量保持不变，而节点数量增加到 304。因此，32 个大洋洲的机场没有任何航班路线通往其他大洋洲机场。

在处理图中的多个节点和关系类型时，我们可能希望在投影过程中保留节点标签和关系类型的信息。在图投影过程中定义节点和关系类型使我们可以在算法执行时对其进行筛选。

```py
CALL {
    MATCH (source:Airport)-[r:HAS_ROUTE]->(target:Airport)
    RETURN source, target, r
    UNION
    MATCH (source:Airport)-[r:IN_CITY]->(target:City)
    RETURN source, target, r
}
WITH gds.alpha.graph.project('airports-labels', source, target,
  {sourceNodeLabels: labels(source),
   targetNodeLabels: labels(target)},
  {relationshipType:type(r)}) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

当投影多个不同的图模式时，我更倾向于使用`UNION`子句。然而，具体使用什么 Cypher 匹配语句完全取决于你。由于我们投影的是两种类型的节点和关系，保留它们的标签和类型信息可能是个好主意。因此，我们使用了**sourceNodeLabels**、**targetNodeLabels**和**relationshipType**参数。在这个例子中，我们使用了现有的节点标签和关系类型。

然而，有时我们可能希望在投影过程中使用自定义标签或关系类型。

```py
CALL {
    MATCH (source:Airport)-[r:HAS_ROUTE]->(target:Airport)
    RETURN source, target, r
    UNION
    MATCH (source:Airport)-[r:IN_CITY]->(target:City)
    RETURN source, target, r
}
WITH gds.alpha.graph.project('airports-labels-custom', source, target,
{sourceNodeLabels: CASE WHEN source.city = 'Miami' 
                       THEN 'Miami' ELSE 'NotMiami' END,
 targetNodeLabels: ['CustomLabel']},
{relationshipType: CASE WHEN type(r) = 'HAS_ROUTE' 
                        THEN 'FLIGHT' ELSE 'NOT_FLIGHT' END}) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

正如你所见，我们可以使用 Cypher 动态定义节点或关系类型，也可以直接硬编码它。如果自定义的节点标签或关系类型比较复杂，也可以在 Cypher 匹配语句中计算。

```py
CALL {
    MATCH (source:Airport)-[r:HAS_ROUTE]->(target:Airport)
    RETURN source, target, r,
         CASE WHEN source.city = target.city 
              THEN 'INTRACITY' ELSE 'INTERCITY' END as rel_type
    UNION
    MATCH (source:Airport)-[r:IN_CITY]->(target:City)
    RETURN source, target, r, type(r) as rel_type
}
WITH gds.alpha.graph.project('airports-labels-precalculated', source, target,
  {sourceNodeLabels: labels(source),
   targetNodeLabels: labels(target)},
  {relationshipType: rel_type}) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

有时，我们也希望投影节点或关系属性。

```py
MATCH (source:Airport)-[r:HAS_ROUTE]->(target:Airport)
WITH gds.alpha.graph.project('airports-properties', source, target,
  {sourceNodeLabels: labels(source),
   targetNodeLabels: labels(target),
   sourceNodeProperties: {runways: source.runways},
   targetNodeProperties: {runways: target.runways}},
  {relationshipType: type(r), properties: {distance: r.distance}}) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

节点或关系属性被定义为一个映射对象（对于 Python 或 JS 开发者来说是字典或 JSON 对象），其中键表示投影的属性，值表示投影的值。这种语法允许我们投影在投影过程中计算出的属性。

```py
MATCH (source:Airport)-[r:HAS_ROUTE]->(target:Airport)
WITH gds.alpha.graph.project('airports-properties-custom', source, target,
  {sourceNodeLabels: labels(source),
   targetNodeLabels: labels(target),
   sourceNodeProperties: {runways10: source.runways * 10},
   targetNodeProperties: {runways10: target.runways * 10}},
  {relationshipType: type(r),
  properties: {inverseDistance: 1 / r.distance}}) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

同样，我们可以利用 Cypher 的所有灵活性来计算任何节点或关系属性。与节点标签类似，我们也可以在`MATCH`子句中计算自定义属性。

一个重要的注意事项是当前的投影行为是引擎在第一次遇到节点时存储节点属性。然而，在随后的节点遇到中，它会完全忽略节点属性。因此，你必须小心为源节点和目标节点计算相同的节点属性。否则，投影结果可能与预期存在差异。

Neo4j 图数据科学库中的一些图算法期望无向关系。关系不能以无向方式存储在数据库中，必须在图投影期间明确地定义。

假设你想将所有投影关系视为无向关系。

```py
MATCH (source:Airport)-[r:HAS_ROUTE]->(target:Airport)
WITH gds.alpha.graph.project('airports-undirected', source, target,
  {}, // nodeConfiguration
  {}, // relationshipConfiguration
  {undirectedRelationshipTypes: ['*']}
) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

我们可以使用 **undirectedRelationshipType** 来指定哪些关系应被投影为无向关系。在实际操作中，你可以观察到，当我们投影无向图时，关系数量翻了一倍。

有时你可能希望将某一类关系投影为无向关系，而将其他关系视为有向关系。

```py
CALL {
    MATCH (source:Airport)-[r:HAS_ROUTE]->(target:Airport)
    RETURN source, target, r
    UNION ALL
    MATCH (source:Airport)-[r:IN_CITY]->(target:City)
    RETURN source, target, r
}
WITH gds.alpha.graph.project('airports-undirected-specific', source, target,
  {},
  {relationshipType:type(r)},
  {undirectedRelationshipTypes: ['IN_CITY']}) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

在这个示例中，**HAS_ROUTE** 关系被视为有向关系，而 **IN_CITY** 关系被视为无向关系。当我们想要指定特定的关系类型被视为无向关系时，必须在关系配置中包含 **relationshipType** 参数。

最后，我们还可以投影虚拟关系。虚拟关系是一种未存储在数据库中的关系。

![](../Images/15a13565efa295f5ce25b7d9ed4b6020.png)

虚拟关系。作者提供的图像。

假设你想根据城市的航班连接来检查城市。数据库中没有城市之间的航班关系。你可以在图投影期间计算这些关系，而不是在数据库中创建它们。

```py
MATCH (sourceCity)<-[:IN_CITY]-(:Airport)-[:HAS_ROUTE]->(:Airport)-[:IN_CITY]->(targetCity)
WITH sourceCity, targetCity, count(*) AS countOfRoutes
WITH gds.alpha.graph.project('airports-virtual', sourceCity, targetCity,
  {},
  {relationshipType:'VIRTUAL_ROUTE'},
  {}) AS graph
RETURN graph.nodeCount AS nodeCount, graph.relationshipCount AS relationshipCount
```

正如你所观察到的，使用 Cypher 聚合投影投影虚拟关系非常简单。我们计算了各个城市之间的路线数量，并将其添加为投影图中的关系属性。

让我们基于 PageRank 算法计算最重要的城市，以完成这篇博客文章。

```py
CALL gds.pageRank.stream('airports-virtual')
YIELD nodeId, score
RETURN gds.util.asNode(nodeId).name AS city, score
ORDER BY score DESC
LIMIT 5
```

*结果*

## 总结

Cypher 聚合是使用 Cypher 语句在 Neo4j 图数据科学库中投影内存图的新选项。具体来说，它可以用于投影无向关系，这在旧的 Cypher 投影中是不可能的。然而，随着在投影期间选择和转换图的灵活性增加，性能成本也随之增加。因此，如果可能的话，你应该出于性能考虑使用原生投影。另一方面，当你有特定的用例需要投影图的某一特定子集、计算自定义属性或投影虚拟关系时，Cypher 聚合是你的好帮手。
