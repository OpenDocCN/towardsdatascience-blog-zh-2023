# 专家系统是否已经过时？

> 原文：[https://towardsdatascience.com/are-expert-systems-dead-87c8d6c26474?source=collection_archive---------4-----------------------#2023-03-16](https://towardsdatascience.com/are-expert-systems-dead-87c8d6c26474?source=collection_archive---------4-----------------------#2023-03-16)

## 对近期趋势、使用案例和技术的回顾

[](https://simon-j-preis.medium.com/?source=post_page-----87c8d6c26474--------------------------------)[![Simon J. Preis 教授，博士](../Images/ea0586e8d1fd6ba434fa2cfb9a71e4a5.png)](https://simon-j-preis.medium.com/?source=post_page-----87c8d6c26474--------------------------------)[](https://towardsdatascience.com/?source=post_page-----87c8d6c26474--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----87c8d6c26474--------------------------------) [Simon J. Preis 教授，博士](https://simon-j-preis.medium.com/?source=post_page-----87c8d6c26474--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8c9dbf8eb438&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fare-expert-systems-dead-87c8d6c26474&user=Professor+Simon+J.+Preis%2C+Ph.D.&userId=8c9dbf8eb438&source=post_page-8c9dbf8eb438----87c8d6c26474---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----87c8d6c26474--------------------------------) ·12分钟阅读·2023年3月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F87c8d6c26474&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fare-expert-systems-dead-87c8d6c26474&user=Professor+Simon+J.+Preis%2C+Ph.D.&userId=8c9dbf8eb438&source=-----87c8d6c26474---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F87c8d6c26474&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fare-expert-systems-dead-87c8d6c26474&source=-----87c8d6c26474---------------------bookmark_footer-----------)![](../Images/91a28d8351f021ca364795f72731892c.png)

[Nghia Le](https://unsplash.com/@lephunghia?utm_source=medium&utm_medium=referral)的照片，来自于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

每个人都在谈论机器学习。这不仅仅是一种感觉：如果你在Google上搜索“机器学习”，你将获得近7亿条结果。但专家系统呢？传统上，它是AI的另一面。好吧，Google至少返回了大约700万条结果，但这两个概念之间存在明显差距。这种不匹配也适用于Google搜索趋势：尽管机器学习（橙色线）的兴趣近年来显著增加，但可以得到的印象是专家系统（蓝色线）似乎已经被遗忘了。

![](../Images/86912c0000ffc0c2fe1391b698bfbc0a.png)

Google搜索趋势百分比（作者，数据来源于Google）

所以这是否只是AI进化的结果（适者生存，希望不是“过于适者”……对不起，那个AI笑话；-）？专家系统真的过时了吗？还是在某些研究领域中，专家系统仍有存在的空间？我认为收集和分享一些发现和经验，以便为专家系统提供立场，是有意义的！

内容：

1.  什么是专家系统？

1.  专家系统在专题研究中是否相关？

1.  专家系统的应用案例有哪些？

1.  开发专家系统的现代技术有哪些？

# 什么是专家系统？

专家系统（ES）是一种软件，至少由一个知识（数据）库、一个与领域相关的规则集和一个能够推断新公理的推理引擎组成。之所以称之为ES，是因为它在特定领域的表现类似于人类专家，例如能够回答与其专业领域相关的棘手问题。从应用角度来看，ES有两类主要用户：a）持续审查和修改知识库的人类专家；b）寻求领域相关问题答案的最终用户。与通常称为“黑箱AI”的深度学习方法相比，ES的结果总是透明的：推理引擎通常提供逐步的结果解释，用户能够理解其逻辑结构。

# 专家系统在专题研究中是否相关？

为了澄清这个问题，如果专家系统确实已经过时，值得查看一下使用或至少提及专家系统的文献数量。为此，我在Google Scholar上做了一个简单的搜索，检查了2005年至2022年间包含“专家系统”术语的出版物数量。让我们提出以下工作假设：在选定的年份中，专家系统的出版趋势是负面的（以确认“专家系统已经过时”）。

现在让我们来看看数据显示了什么。我制作了一张图表，显示了每个时间段的出版物绝对数量和趋势线。该图表清楚地显示出出版趋势仍然是积极的。尽管文献检索非常高级且嘈杂，但我的期望是，如果该术语已经过时，ES在专题文章中不会被提及；所以我们可以拒绝上述的假设。我们可以推断出ES仍然是重要的，并在研究中发挥着关键作用。

![](../Images/a7ccebfabdeb34e77993eba4aadf249a.png)

专家系统的研究趋势（作者）

# 专家系统的使用案例是什么？

那么，开发和提出专家系统的这些专题研究学科是什么？它们为哪些领域提供服务？它们满足哪些要求？专家系统在这些项目中的主要价值是什么？当然，这些都是通过系统文献综述来回答的重要问题，但现在，我只会展示三个已选项目，以展示专家系统的广度。

## 网络安全

ES可以用于IoT生态系统的自动化安全评估。Rak等人（2022）提出了一个专家系统，为每个识别的威胁生成威胁模型和攻击计划列表。专家系统提供的结果可以供渗透测试人员对目标IoT基础设施进行系统安全测试。

## 项目管理

Bhattacharya等人（2022）提出了一个用于在复杂的监管和技术实施项目中进行决策的专家系统。例如，该专家系统验证项目是否满足启动条件，并根据项目类型和复杂性来定制项目计划。

## 临床决策支持

Chrimes（2023）提出了一个用于支持COVID-19临床决策的专家系统。该专家系统通过与用户进行交互（通过聊天机器人）来确定COVID-19感染的潜在严重程度或可能导致严重病例发展的生物系统反应和共病症。

# 专家系统的现代技术

特别是在纯研究项目中，我们经常看到Protégé作为专家系统开发工具，也用于专题项目。该工具结合了知识存储（实体和对象属性）、规则（SWRL）、事实（个体）并提供预安装插件（HermiT）的推理引擎。我仍然认为Protégé在教育和研究中很重要，特别是因为它是免费且易于入门的。但我不建议在工业项目中使用它，因为它在系统集成和用户界面设计方面有限制。在我看来，它是大学使用的工具，而不是公司使用的工具。

然而，现代工具中有一些具有 Protégé 部分功能的行业验证工具。最重要的是图形数据库，它们允许语义存储和语义分析信息。因此，在一定程度上，我们可以将本体表示为知识图谱。为此，neo4j 提供了一个名为“neosemantics”的 RDF 插件，它是 Protégé 中本体的数据模型标准。因此，我们可以在 Protégé 中创建本体并将其导出到 neo4j 以便进一步的软件集成。插件 neosemantics 也提供了推理引擎，不过，我们也可以通过 Cypher 来进行推理，Cypher 是 neo4j 的标准查询语言。

## 示例

那么推理或推断是什么？让我们看看以下由人员和性别组成的数据库。我们看到有一些关系指示一个人的性别（在简化的二元世界中）为“IS_A”，以及人员节点之间的家庭关系为“IS_Parent”。

![](../Images/b95fd7eeac570d340712acbb4b12b7be.png)

初始数据库（作者）

现在我们可以区分显式存储的知识和传递知识。显式知识是我们在图中看到的内容，以及我在上面的段落中描述的内容。然而，从我们日常经验中我们知道，根据这些可用的信息，这些人员之间还有更多关系：兄弟姐妹、祖父母、叔叔等。与其手动搜索和/或输入数据库中的所有这些关系，不如使用推理技术自动创建这些知识。

## 祖父母

让我们从一个简单的开始：祖父母是一个人的父母的父母。因此在我们的图形数据库中，我们寻找的是多级的“IS_Parent”关系，如 PersonA →PersonB →PersonC。在这种情况下，我们可以说 PersonC 是 PersonA 的孙辈，而 PersonA 是 PersonC 的祖父母。我们可以通过以下通用语句推断出这一信息：

```py
MATCH (a:Person)-[:IS_Parent]->(b:Person)-[:IS_Parent]->(c:Person) 
return a.name as grandparent, c.name as grandchild 
```

这个声明的结果显示在下面的截图中。我们看到 Max 和 Anna 都是 Betty 的孙辈。为什么？因为 Betty 是 Franz 的父母，而 Franz 是 Max 和 Anna 的父母。

![](../Images/4428ddc7ea5047888693595028bf0fe4.png)

祖父母结果（作者）

## 兄弟姐妹

另一种类型的传递知识是兄弟姐妹关系。我们在寻找有相同父母的人员，这意味着我们在寻找 PersonA ←PersonB 和 PersonC ←PersonB 的共同“IS_Parent”关系。我们可以通过以下语句查询这些信息：

```py
MATCH (a:Person)<-[:IS_Parent]-(b:Person),
 (c:Person)<-[:IS_Parent]-(b:Person)
return distinct a.name as sibling_a, c.name as sibling_b
```

这个声明的结果显示在下面的截图中。我们看到 Max 和 Anna 是兄弟姐妹。由于匹配查询适用于两种视角，我们得到两个结果记录。这在技术上也是有效的，因为 neo4j 不支持双向或非定向关系。如果 Anna 是 Max 的兄弟姐妹，而 Max 又是 Anna 的兄弟姐妹，那么我们就有两个独立的关系。

![](../Images/86e5b140c8e59d8aba6374ca84b9143a.png)

兄弟结果（作者）

## 持久化传递知识

对于这种按需查询的用例是存在的，但也有其他用例中，我们希望将新的传递知识存储到数据库中。如果多个用户同时使用数据库，这特别有用——在这种情况下，隐藏任何可能对业务相关的传递知识是没有意义的。在这种情况下，我们可以使用合并子句创建节点之间的新（唯一）关系，如以下语句所示：

```py
Match (a:Person)<-[:IS_Parent]-(b:Person),
 (c:Person)<-[:IS_Parent]-(b:Person) 
Merge  (a)-[r:IS_SIBLING]->(c)
```

*注意：除了新的关系之外，我们还可以推断并保存新的节点、标签和属性。*

如果我们查询整个数据库，我们会看到安娜和马克斯之间额外的“IS_SIBLING”关系：

![](../Images/5fce84203c94a303ffa2da1f34caebb4.png)

修改后的数据库（作者）

应该强调的是，这种手动推理类型不考虑前向/后向推理，这些通常是推理引擎所需的功能，例如支持复杂的定理证明。因此，如果你对这些功能感兴趣，可以从[这里](https://www.geeksforgeeks.org/difference-between-backward-and-forward-chaining/)（一般介绍）和[这里](https://neo4j.com/labs/neosemantics/4.0/inference/)（使用 neo4j 进行推理）开始阅读。

## 自学习专家系统

现在的问题是，我们作为人类是否总是需要事先知道这些规则，或者是否需要手动准备我们想要查询的规则以提取传递知识。简短的答案是：不！这使我们看到数据驱动的机器学习和基于规则的专家系统这两个通常分开的领域之间的有趣联系：我们可以应用决策树从经验数据中建立规则，这些规则仍然是“白盒”的，因为它们对用户是透明的。决策树不限于图数据库，Python 中的 scikit-learn 库需要将数据结构从图结构转换为扁平结构，例如 pandas 数据框——所以我们现在忽略图模型，直接跳到我们有一个包含选定数字特征的扁平数据框的地方。

假设我们为一个汽车销售商工作，我们想要建立一个专家系统，告诉销售人员某个访客是否可能购买汽车。我们可以使用历史销售数据来推导出从以前购买决定中得到的规则，而不是根据自己的经验开发规则。这是我们在讨论决策树时应注意的：我们处理的是概率。即使树预测一个人可能会买车，也不意味着这个人真的会买车。我们可以在下面的图中看到汽车销售商的决策树分类的可能结果。

![](../Images/55c2f205d8462460b6b69cdfa2e45a55.png)

汽车销售商的决策树分类（作者）

除了图形表示，我们还可以提取新的决策规则，以逻辑“if-then”格式呈现，如[这篇文章](https://mljar.com/blog/extract-rules-decision-tree/)所述。

```py
if (Age <= 44.5) and (Annual Salary <= 90750.0) and (Annual Salary <= 69750.0) then class: No Purchase (proba: 100.0%) | based on 258 samples
if (Age <= 44.5) and (Annual Salary <= 90750.0) and (Annual Salary > 69750.0) then class: No Purchase (proba: 89.02%) | based on 173 samples
if (Age > 44.5) and (Age > 47.5) and (Annual Salary > 41750.0) then class: Purchase (proba: 82.78%) | based 
on 151 samples
if (Age > 44.5) and (Age > 47.5) and (Annual Salary <= 41750.0) then class: Purchase (proba: 98.46%) | based on 65 samples
if (Age <= 44.5) and (Annual Salary > 90750.0) and (Annual Salary <= 119750.0) then class: Purchase (proba: 
67.35%) | based on 49 samples
if (Age <= 44.5) and (Annual Salary > 90750.0) and (Annual Salary > 119750.0) then class: Purchase (proba: 97.67%) | based on 43 samples
if (Age > 44.5) and (Age <= 47.5) and (Annual Salary > 53250.0) then class: No Purchase (proba: 50.0%) | based on 34 samples
if (Age > 44.5) and (Age <= 47.5) and (Annual Salary <= 53250.0) then class: Purchase (proba: 85.19%) | based on 27 samples
```

假设我们已经验证了这个决策树模型，并将其集成到用户界面中，那么汽车销售员可以输入一些个人访客信息，以获得一个响应，告诉他这个访客是否可能会买车。让我们查看以下示例：

a) 如果一个潜在客户进入商店，具有以下属性：女性，年龄=28岁，年薪=78,000 — 这个人可能会买车吗？→ 我们的决策树说：不（标记为“0”）。

b) 如果一个潜在客户进入商店，具有以下属性：男性，年龄=28岁，年薪=118,000 — 这个人可能会买车吗？→ 我们的决策树说：是的（标记为“1”）。

我们看到，这样的系统将提供类似于传统专家系统的功能，适用于这个汽车销售领域。好处在于，这种基于决策树的专家系统能够从新数据中学习，从而不断改进规则。然而，我们需要记住，这种类型的专家系统依赖于概率，因此响应通常不是100%准确的。与传统专家系统不同，传统专家系统由于应用了逻辑推理，响应在逻辑上是准确的（至少如果基本公理正确的话）。但我们需要考虑建立和维护知识库和规则的高手动成本，同时也需要考虑即使是专家也可能犯错，这需要额外的验证工作。因此，最终这是一种维护效率与预测质量之间的权衡。

# 结论

在定义了什么是专家系统之后，我们看到专家系统仍在使用、开发或至少在研究出版物中被引用，且出版趋势甚至是积极的。从这个角度来看，我们可以清楚地回答最初的问题：专家系统已经死了吗？没有！我们讨论了几个主题项目及其领域，以了解专家系统的广度。最后，我们讨论了一些图形数据库的特性，由neo4j代表，来执行通常与专家系统相关的任务。这些关键任务一方面是显性知识的语义存储，另一方面是基于规则的新传递知识推理。当然，neo4j在数据存储和分析方面提供了更多功能，但我认为，上述简单示例足以理解neo4j作为专家系统一部分的知识库的能力。此外，我们还简要讨论了如何通过决策树从数据中构建规则，从而允许开发自学习的专家系统。

那么，我们什么时候应该考虑应用专家系统（ES）？

1.  在我们**尚未拥有数据**的情况下无法使用机器学习模型。例如：在没有可靠参考产品的情况下，开发和验证新产品发布的资格计划。

1.  在我们**受规则驱动**且已知规则已经静态且不由数据驱动的情况下。例如：根据其他详细信息推断业务定义的分类主数据，例如生产机器的特定结构和设置。

1.  在我们**拥有数据并希望使用和理解决策规则**的情况下，并且我们知道**规则可能会随时间变化**。我们可以使用决策树，而不是手动开发和维护规则。例如：基于经验数据理解和推断客户行为。

结尾附上一个有趣的事实：我问了ChatGPT，它*不*认为自己是一个专家系统，因为它没有针对特定领域进行训练以提供专家级建议。

# 来源

Bhattacharya, K., Gangopadhyay, S., DeBrule, C. (2021). 复杂的监管和技术实施项目中的决策专家系统设计。收录于：Chakrabarti, A., Poovaiah, R., Bokil, P., Kant, V. (编辑) 未来设计——第3卷。智能创新、系统与技术，第223卷。Springer，Singapore。[https://doi.org/10.1007/978-981-16-0084-5_50](https://doi.org/10.1007/978-981-16-0084-5_50)

Chrimes, D. (2023). 将决策树作为COVID-19临床决策支持的专家系统。Interact J Med Res 2023; 12:e42540

URL：[https://www.i-jmr.org/2023/1/e42540](https://www.i-jmr.org/2023/1/e42540)。 DOI: 10.2196/42540

Rak, Massimiliano & Salzillo, Giovanni & Granata, Daniele. (2022). ESSecA：一个用于物联网生态系统的威胁建模和渗透测试的自动化专家系统，《计算机与电气工程》，第99卷，2022，107721，ISSN 0045–7906，[https://doi.org/10.1016/j.compeleceng.2022.107721](https://doi.org/10.1016/j.compeleceng.2022.107721)。
