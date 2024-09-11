# 如何将任何文本转换为概念图谱

> 原文：[https://towardsdatascience.com/how-to-convert-any-text-into-a-graph-of-concepts-110844f22a1a?source=collection_archive---------0-----------------------#2023-11-10](https://towardsdatascience.com/how-to-convert-any-text-into-a-graph-of-concepts-110844f22a1a?source=collection_archive---------0-----------------------#2023-11-10)

## 使用 Mistral 7B 将任何文本语料库转换为知识图谱的方法

[](https://medium.com/@rahul.nyk?source=post_page-----110844f22a1a--------------------------------)[![Rahul Nayak](../Images/9f8aa2f9af4e02b31c114222756489e5.png)](https://medium.com/@rahul.nyk?source=post_page-----110844f22a1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----110844f22a1a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----110844f22a1a--------------------------------) [Rahul Nayak](https://medium.com/@rahul.nyk?source=post_page-----110844f22a1a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F473e87f4b733&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-convert-any-text-into-a-graph-of-concepts-110844f22a1a&user=Rahul+Nayak&userId=473e87f4b733&source=post_page-473e87f4b733----110844f22a1a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----110844f22a1a--------------------------------) ·12 分钟阅读·2023年11月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F110844f22a1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-convert-any-text-into-a-graph-of-concepts-110844f22a1a&user=Rahul+Nayak&userId=473e87f4b733&source=-----110844f22a1a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F110844f22a1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-convert-any-text-into-a-graph-of-concepts-110844f22a1a&source=-----110844f22a1a---------------------bookmark_footer-----------)![](../Images/fb08cad7edf14d0bbf132f706493fdbf.png)

作者使用本文中分享的项目生成的图像。

几个月前，基于知识的问答（KBQA）还是一种新颖的技术。现在，带有检索增强生成（RAG）的 KBQA 对任何 AI 爱好者来说都已经易如反掌。看到 NLP 领域的可能性因 LLMs 而迅速扩展，令人着迷。而且，情况每天都在变得更好。

在我的上一篇文章中，我分享了一种递归 RAG 方法，用于实现带有多跳推理的问答，以基于大量文本语料库回答复杂查询。

[](/the-research-agent-4ef8e6f1b741?source=post_page-----110844f22a1a--------------------------------) [## 研究代理：应对基于大文本语料库回答问题的挑战

### 我制作了一个自主AI研究代理，它可以通过深度多跳推理能力回答困难的问题。

[towardsdatascience.com](/the-research-agent-4ef8e6f1b741?source=post_page-----110844f22a1a--------------------------------)

许多人尝试了它并提供了反馈。感谢大家的反馈。我已将这些贡献汇总，并对代码做了一些改进，以解决原始实现中的一些问题。我计划撰写一篇单独的文章。

在这篇文章中，我想分享另一个可能有助于创建超级研究代理的想法，当它与递归RAG结合时。这个想法源于我对递归RAG和较小LLM的实验，以及我在Medium上阅读的一些其他想法——特别是**知识图谱增强生成**。

## **摘要**

知识图谱（KG）或任何图形，由节点和边组成。KG的每个节点代表一个概念，每个边表示一对概念之间的关系。在这篇文章中，我将分享一种将任何文本语料库转换为概念图的方法。我将“概念图”（GC）与KG术语交替使用，以更好地描述我在这里展示的内容。

我在这个实现中使用的所有组件都可以在本地设置，因此这个项目可以很容易地在个人计算机上运行。我在这里采用了非GPT的方法，因为我相信较小的开源模型。我正在使用出色的Mistral 7B Openorca instruct和Zephyr模型。这些模型可以通过Ollama在本地设置。

像Neo4j这样的数据库使得存储和检索图数据变得容易。在这里，我使用内存中的Pandas数据框和NetworkX Python库，以保持简单。

我们的目标是将任何文本语料库转换为概念图（GC），并像这篇文章的美丽横幅图像一样进行可视化。我们甚至可以通过移动节点和边缘、缩放以及根据我们的心愿改变图形的物理属性来与网络图进行交互。这里是展示我们正在构建的结果的Github页面链接。

[https://rahulnyk.github.io/knowledge_graph/](https://rahulnyk.github.io/knowledge_graph/)

但首先，让我们*深入探讨*知识图谱的基本概念以及我们为什么需要它们。如果你已经熟悉这个概念，可以跳过下一部分。

# **知识图谱**

考虑以下文本。

*玛丽有只小羊，

你可能以前听过这个故事；

但你知道她把盘子递过来了吗，

还多了一点！*

（我希望孩子们没有在读这个 😝）

这是文本作为KG的一种可能表示方式。

![](../Images/4fd39148664c94a81f884489e8151fa3.png)

图表由作者使用draw.io创建

IBM 的以下文章恰当地解释了知识图谱的基本概念。

[## 什么是知识图谱？ | IBM](https://www.ibm.com/topics/knowledge-graph?source=post_page-----110844f22a1a--------------------------------)

### 了解知识图谱，语义元数据的网络，表示一组相关实体。

[www.ibm.com](https://www.ibm.com/topics/knowledge-graph?source=post_page-----110844f22a1a--------------------------------)

引用文章中的摘录来总结这一观点：

> 知识图谱，也称为语义网络，表示一个现实世界实体的网络——即对象、事件、情况或概念——并说明它们之间的关系。这些信息通常存储在图数据库中，并以图形结构的形式可视化，因此得名知识“图谱”。

## **为什么选择知识图谱？**

知识图谱在多种方面都非常有用。我们可以运行图算法并计算任何节点的中心性，以了解一个概念（节点）在整体工作中的重要性。我们可以分析连接和不连接的概念集，或者计算概念社区，以深入理解主题内容。我们还可以理解看似不相关的概念之间的联系。

我们还可以使用知识图谱来实现图检索增强生成（GRAG 或 GAG），并与我们的文档进行对话。这可以比简单的 RAG 版本获得更好的结果，后者存在若干缺陷。例如，仅通过简单的语义相似性搜索来检索与查询最相关的上下文并不总是有效。尤其是当查询没有提供足够的上下文来表明其真实意图，或者上下文在大量文本中是零散的情况下。

例如，考虑这个查询 —

***告诉我《百年孤独》中何塞·阿尔卡迪奥·布恩迪亚的家谱。***

这本书记录了*何塞·阿尔卡迪奥·布恩迪亚*的七代人，其中一半的角色都叫做*何塞·阿尔卡迪奥·布恩迪亚*。如果通过一个简单的 RAG 管道来回答这个查询，将会是相当具有挑战性的，甚至可能是不可能的。

RAG 的另一个缺陷是它不能告诉你该问什么。通常，提出正确的问题比得到答案更为重要。

图增强生成（GAG）在一定程度上可以解决 RAG 的这些缺陷。更好的是，我们可以混合使用，建立一个图增强检索增强生成的管道，从而获得两者的最佳效果。

所以现在我们知道，图谱非常有趣，它们可以极其有用，而且看起来也很美观。

# **创建概念图谱**

如果你询问 GPT，如何从给定的文本中创建知识图谱？它可能会建议以下过程。

1.  从文献中提取概念和实体。这些就是节点。

1.  提取概念之间的关系。这些就是边。

1.  在图形数据结构或图形数据库中填充节点（概念）和边（关系）。

1.  可视化，以获得一些艺术上的满足感，哪怕仅此而已。

第 3 步和第 4 步听起来很容易理解。但你如何实现第 1 步和第 2 步？

这是我设计的从任何给定文本语料库中提取概念图的方法的流程图。它类似于上述方法，但有一些细微的差别。

![](../Images/df5cd67aa84360e05c33020c840d23dc.png)

图表由作者使用 draw.io 创建

1.  将文本语料库分割成多个块。给每个块分配一个 chunk_id。

1.  对于每个文本块，使用 LLM 提取概念及其语义关系。我们将这种关系的权重定为 W1。相同概念对之间可以有多个关系。每种这样的关系都是概念对之间的一条边。

1.  考虑到出现在同一文本块中的概念也通过其上下文的接近性相关联。我们将这种关系的权重定为 W2。请注意，同一对概念可能出现在多个块中。

1.  对相似的对进行分组，求和它们的权重，并连接它们的关系。现在，我们只有一个边连接任何不同的概念对。该边具有一定的权重和一个作为其名称的关系列表。

你可以在我在本文中分享的 GitHub 仓库中看到该方法的 Python 实现。接下来，我们将简要回顾实现的关键思想。

为了演示该方法，我使用了在 PubMed/Cureus 上发布的以下评论文章，该文章遵循知识共享署名许可证。感谢文章末尾的作者。

[](https://www.cureus.com/articles/158868-indias-opportunity-to-address-human-resource-challenges-in-healthcare?source=post_page-----110844f22a1a--------------------------------#!/) [## 印度解决医疗保健中人力资源挑战的机会

### 印度的健康指标最近有所改善，但仍落后于其同行国家。…

www.cureus.com](https://www.cureus.com/articles/158868-indias-opportunity-to-address-human-resource-challenges-in-healthcare?source=post_page-----110844f22a1a--------------------------------#!/)

## **Mistral 和 Prompt**

上述流程图中的第 1 步很简单。Langchain 提供了大量的文本分割器，我们可以用来将文本分成多个块。

第 2 步是真正有趣的开始。为了提取概念及其关系，我使用了 Mistral 7B 模型。在确定最适合我们目的的模型变体之前，我尝试了以下几种：

[Mistral Instruct](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.1)

[Mistral OpenOrca](https://huggingface.co/Open-Orca/Mistral-7B-OpenOrca)，以及

[Zephyr（Hugging Face 版本，源自 Mistral）](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)

我使用了这些模型的4位量化版本——以防我的Mac开始讨厌我——在本地由Ollama托管。

[](https://ollama.ai/?source=post_page-----110844f22a1a--------------------------------) [## Ollama

### 在本地启动和运行大型语言模型。

ollama.ai](https://ollama.ai/?source=post_page-----110844f22a1a--------------------------------)

这些模型都是经过指令调整的模型，带有系统提示和用户提示。只要我们告诉它们，它们都会相当好地遵循指令，并将答案整齐地格式化为JSON。

经过几轮的试验和错误，我终于确定了**Zephyr模型**，并使用了以下提示。

```py
SYS_PROMPT = (
    "You are a network graph maker who extracts terms and their relations from a given context. "
    "You are provided with a context chunk (delimited by ```) 你的任务是提取本体“

    “在给定上下文中提到的术语。这些术语应代表上下文中的关键概念。 \n”

    “想法 1：在遍历每个句子时，考虑其中提到的关键术语。\n”

        “\t术语可能包括对象、实体、位置、组织、人物、\n”

        “\t条件、缩写、文档、服务、概念等。\n”

        “\t术语应尽可能原子化\n\n”

    “想法 2：考虑这些术语如何与其他术语一对一关联。\n”

        “\t在同一句话或同一段落中提到的术语通常是相互关联的。\n”

        “\t术语可以与许多其他术语相关联\n\n”

    “想法 3：找出每对相关术语之间的关系。\n\n”

    “将输出格式化为JSON列表。列表的每个元素包含一对术语”

    “以及它们之间的关系，如下所示：\n”

    “[\n”

    “   {\n”

    '       "node_1": "从提取的本体中得到的概念",\n'

    '       "node_2": "从提取的本体中相关的概念",\n'

    '       "edge": "节点_1 和节点_2 之间的关系，用一两句话描述"\n'

    “   }, {...}\n”

    “]”

)

USER_PROMPT = f"上下文: ```py{input}``` \n\n 输出: "

```py

If we pass our (not fit for) nursery rhyme with this prompt, here is the result.

```

[

{

    “node_1”: “玛丽”,

    “node_2”: “羊肉”,

    “edge”: “由……拥有”

},

{

    “node_1”: “盘子”,

    “node_2”: “食物”,

    “edge”: “包含”

}, . . .

]

```py

Notice, that it even guessed ‘food’ as a concept, which was not explicitly mentioned in the text chunk. Isn’t this wonderful!

If we run this through every text chunk of our example article and convert the json into a Pandas data frame, here is what it looks like.

![](../Images/b90d053228d8ea0b384e52a8e2ff819a.png)

Every row here represents a relation between a pair of concepts. Each row is an edge between two nodes in our graph, and there can be multiple edges or relationships between the same pair of concepts. The count in the above data frame is the weight that I arbitrarily set to 4.

## **Contextual Proximity**

I assume that the concepts that occur close to each other in the text corpus are related. Let’s call this relation ‘contextual proximity’.

To calculate the contextual proximity edges, we melt the dataframe so that node_1 and node_2 collapse into a single column. Then we create a self-join of this dataframe using the chunk_id as the key. So nodes that have the same chunk_id will pair with each other to form a row.

But this also means that each concept will also be paired with itself. This is called a self-loop, where an edge starts and ends on the same node. To remove these self-loops, we will drop every row where node_1 is the same as node_2 from the dataframe.

In the end, we get a dataframe very similar to our original dataframe.

![](../Images/8898559a2fcb5732570bd12f60e205e2.png)

The count column here is the number of chunks where node_1 and node_2 occur together. The column chunk_id is a list of all these chunks.

So we now have two dataframes, one with the semantic relation, and another with the contextual proximity relation between concepts mentioned in the text. We can combine them to form our network graph dataframe.

We are done building a graph of concepts for our text. But to leave it at this point will be quite an ungratifying exercise. Our goal is to visualise the Graph just like the featured image at the beginning of this article, and we are not far from our goal.

# **Creating a Network of Concepts**

NetworkX is a Python library that makes dealing with graphs super easy. If you are not already familiar with the library, click their logo below to learn more

 [## NetworkX - NetworkX documentation

### NetworkX is a Python package for the creation, manipulation, and study of the structure, dynamics, and functions of…

networkx.org](https://networkx.org/?source=post_page-----110844f22a1a--------------------------------) 

Adding our dataframe to a NetworkX graph is just a few lines of code.

```

G = nx.Graph()

## 向图中添加节点

for node in nodes:

    G.add_node(str(node))

## 向图中添加边

for index, row in dfg.iterrows():

    G.add_edge(

        str(row["node_1"]),

        str(row["node_2"]),

        title=row["edge"],

        weight=row['count']

    )

```py

This is where we can start harnessing the power of Network Graph. NetworkX provides a plethora of network algorithms out of the box for us to use. Here is a link to the list of algorithms we can run on our Graph.

 [## Algorithms - NetworkX 3.2.1 documentation

### Edit description

networkx.org](https://networkx.org/documentation/stable/reference/algorithms/index.html?source=post_page-----110844f22a1a--------------------------------) 

Here, I use a community detection algorithm to add colours to the nodes. Communities are groups of nodes that are more tightly connected with each other, than with the rest of the graph. Communities of concepts can give us a good idea of broad themes discussed in the text.

The Girvan Newman algorithm detected 17 communities of concept in the Review Article we are working with. Here is one such community.

```

[

'数字技术',

'EVIN',

'医疗设备',

'在线培训管理信息系统',

'可穿戴、可追踪技术'

]

```

这立刻让我们对审查论文中讨论的健康技术的广泛主题有了一个大致的了解，并使我们能够提出问题，然后用我们的RAG管道回答。这不是很棒吗？

让我们还计算一下图中每个概念的度。一个节点的度是它连接的边的总数。所以在我们的案例中，概念的度越高，它在文本主题中就越中心。我们将使用度作为我们可视化中节点的大小。

# **图形可视化**

可视化是此练习中最有趣的部分。它具有某种特质，给予你艺术上的满足感。

我使用 PiVis 库创建交互图表。[Pyvis 是一个用于可视化网络的 Python 库](https://github.com/WestHealth/pyvis/tree/master#pyvis---a-python-library-for-visualizing-networks)。这里有一篇中等文章展示了该库的易用性和强大功能。

[](/pyvis-visualize-interactive-network-graphs-in-python-77e059791f01?source=post_page-----110844f22a1a--------------------------------) [## Pyvis: 用 Python 可视化交互网络图

### 只需几行代码

[towardsdatascience.com](/pyvis-visualize-interactive-network-graphs-in-python-77e059791f01?source=post_page-----110844f22a1a--------------------------------)

Pyvis 内置了 NetworkX Helper，可以将我们的 NetworkX 图转换为 PyVis 对象。因此，我们不再需要额外编码……耶！！

记住，我们已经计算了每条边的权重以确定边的厚度、节点的社区以确定颜色，以及每个节点的度数以确定大小。

所以，带着这些花里胡哨的东西，这是我们的图表。

![](../Images/fe3e0691e930bea4d3bfe6249c4f9582.png)

动图由作者使用本文讨论的项目生成。

交互图表链接: [*https://rahulnyk.github.io/knowledge_graph/*](https://rahulnyk.github.io/knowledge_graph/)

我们可以随意缩放和移动节点及边缘。页面底部还有滑块面板，可以改变图的物理效果。看看这个图表如何帮助我们提出正确的问题，更好地理解主题！

我们可以进一步讨论我们的图如何帮助构建图增强检索以及这如何帮助我们构建更好的 RAG 管道。但我认为最好留待另一天。我们已经达成了本文的最终目标！

## GitHub 仓库

[](https://github.com/rahulnyk/knowledge_graph?source=post_page-----110844f22a1a--------------------------------) [## GitHub - rahulnyk/knowledge_graph: 将任何文本转换为知识图谱。这可以用于...

### 将任何文本转换为知识图谱。这可以用于图增强生成或基于知识图谱的问答...

[github.com](https://github.com/rahulnyk/knowledge_graph?source=post_page-----110844f22a1a--------------------------------)

欢迎贡献和建议

我使用了以下文章来演示我的代码。

**Saxena S G, Godfrey T (2023年6月11日) 印度应对医疗资源挑战的机会。Cureus 15(6): e40274\. DOI 10.7759/cureus.40274**

我对作者们的精彩作品表示感激，并感谢他们在知识共享署名许可下发布这部作品。

**关于我**

我是一个建筑学的学习者（不是建筑物……而是技术类型的）。过去，我曾涉及半导体建模、数字电路设计、电子接口建模以及物联网。目前，我在沃尔玛健康与保健部门从事数据互操作性和数据仓库架构方面的工作。
