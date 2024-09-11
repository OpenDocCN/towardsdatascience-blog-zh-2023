# 数据网格的前奏

> 原文：[https://towardsdatascience.com/a-prequel-to-data-mesh-2233c0c56311?source=collection_archive---------3-----------------------#2023-12-03](https://towardsdatascience.com/a-prequel-to-data-mesh-2233c0c56311?source=collection_archive---------3-----------------------#2023-12-03)

## 我个人对数据网格存在性的理由的看法

[](https://medium.com/@rohan_paithankar?source=post_page-----2233c0c56311--------------------------------)[![Rohan Paithankar](../Images/113fc1e72dcb73ab023a60c5043dd191.png)](https://medium.com/@rohan_paithankar?source=post_page-----2233c0c56311--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2233c0c56311--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2233c0c56311--------------------------------) [Rohan Paithankar](https://medium.com/@rohan_paithankar?source=post_page-----2233c0c56311--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2165b853c675&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-prequel-to-data-mesh-2233c0c56311&user=Rohan+Paithankar&userId=2165b853c675&source=post_page-2165b853c675----2233c0c56311---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2233c0c56311--------------------------------) · 4分钟阅读 · 2023年12月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2233c0c56311&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-prequel-to-data-mesh-2233c0c56311&user=Rohan+Paithankar&userId=2165b853c675&source=-----2233c0c56311---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2233c0c56311&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-prequel-to-data-mesh-2233c0c56311&source=-----2233c0c56311---------------------bookmark_footer-----------)

我在一个项目中的一位高级利益相关者提到，他们希望去中心化他们的数据平台架构，并在组织内实现数据民主化。

当我听到“去中心化数据架构”这个词时，我最初感到非常困惑！在我当时有限的数据工程经验中，我只接触过中心化的数据架构，而且它们似乎运行得非常好。因此，我不禁思考，我们想要通过去中心化的数据架构解决什么问题？还是说我们正在创造一个根本不存在的新问题？

# 我在哪里寻找？

显而易见的答案 — 由Zhamak Dehghani提出的数据网格。

一本伟大的书，带你了解一个实施这一概念并克服一些独特挑战的组织。强烈推荐给那些希望深入了解的人。

但为了说明这个概念为何出现，我认为回顾一下历史并理解数据景观的演变会很有帮助。以下是我过于简化的看法。

# **数据景观的演变**

## **1980年代——起源**

+   关系型数据库诞生了。

+   组织开始将关系型数据库用于“一切”。

+   数据库被事务性和分析性负载压垮了。

结果：

+   数据仓库诞生了。

![](../Images/6e1e772f00e39c97bb01e8f7fc8368d8.png)

作者提供的图片

## **1990年代早期——规模**

+   分析工作负载开始变得复杂。

+   数据量开始增长。

+   性能需要改进。

结果：

+   引入了大规模并行处理（MPP）概念——数据在集群之间分布。

![](../Images/f17da3d1f35319ad361e5a8cbf6e672d.png)

作者提供的图片

## **1990年代末到2000年代初——产品化**

+   对报告的需求不断增长。

+   架构变得复杂。

+   业务部门需要与其分析相关的数据。

结果：

+   公司开始将预配置的数据仓库作为产品出售。

+   引入了`数据集市`的概念。

![](../Images/767eeb4085818e12060860c779e26edb.png)

作者提供的图片

## **2004年至2010年——大象进入房间**

+   新一波的应用出现了——社交媒体、软件可观察性等。

+   新的数据格式出现了——JSON、Avro、Parquet、XML等。

结果：

+   Hadoop和NoSQL框架出现了。

+   引入了数据湖以存储新数据格式。

![](../Images/c77372d7b67937a14d954779d415470e.png)

作者提供的图片

## **2010年至2020年——云数据仓库**

+   企业现在希望快速的数据分析，而不受昨天灵活性、处理能力和规模的限制。

结果：

+   云数据仓库解决方案作为关系型和半结构化数据的首选解决方案出现了。

+   示例包括：Amazon Redshift、Google BigQuery、Snowflake、Azure Synapse Analytics、Databricks等。

# 那么缺少了什么？

![](../Images/f7cb6ca514c4185c8270292bbcaff4b3.png)

作者提供的图片

如果我们查看一个组织中使用集中式数据架构的数据通用流程，我们会意识到数据有3个接触点：

+   数据生产者

+   中央数据团队

+   数据消费者

现在让我们先问自己几个问题：

+   谁管理数据仓库？

+   哪个团队响应数据请求？

+   哪个团队负责确保数据质量？

+   哪个团队被期望成为数据的SME？

当我向一群人提问这些问题时，我得到的一个共同答案是（与其他答案结合）——选项B，中央数据团队。

所以我们可以推断中央数据团队需要：

+   管理数据仓库

+   服务数据请求

+   确保数据质量

+   成为数据仓库中数据的SME

还有更多问题待解决。

**那么，缺少了什么？**

随着企业的持续增长，中央数据团队往往成为从数据中获得可操作见解的瓶颈。

中央数据团队最终会承担大量的知识负担，并面临越来越大的交付压力。

这为我提供了支持去证明去中心化数据架构——通常被称为数据网（Data Mesh）——存在的合理性。

> 数据网是一种分析架构，但更重要的是，它是一种将分析数据的所有权转移给最了解和拥有数据的团队——即数据生产者和消费者——的运营模型。

该图展示了数据网架构的高层次视图：

[https://martinfowler.com/articles/data-monolith-to-mesh/data-mesh.png](https://martinfowler.com/articles/data-monolith-to-mesh/data-mesh.png)

我不会详细讨论数据网的原则或逻辑架构，因为有很多文章对此做了详细解释。以下是我最喜欢的一些：

+   [https://martinfowler.com/articles/data-mesh-principles.html](https://martinfowler.com/articles/data-mesh-principles.html)

+   [https://www.datamesh-architecture.com/](https://www.datamesh-architecture.com/)

# **参考文献：**

+   [https://www.oreilly.com/library/view/data-mesh/9781492092384/](https://www.oreilly.com/library/view/data-mesh/9781492092384/)

+   [https://martinfowler.com/articles/data-monolith-to-mesh.html](https://martinfowler.com/articles/data-monolith-to-mesh.html)

+   [https://www.snowflake.com/wp-content/uploads/2017/09/Past-Present-Future-DW-FINAL.pdf](https://www.snowflake.com/wp-content/uploads/2017/09/Past-Present-Future-DW-FINAL.pdf)
