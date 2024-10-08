# 使用 LLM 编译器框架有效协调知识图谱的推理

> 原文：[`towardsdatascience.com/orchestrating-efficient-reasoning-over-knowledge-graphs-with-llm-compiler-frameworks-749d36dc32b9`](https://towardsdatascience.com/orchestrating-efficient-reasoning-over-knowledge-graphs-with-llm-compiler-frameworks-749d36dc32b9)

[](https://medium.com/@alcarazanthony1?source=post_page-----749d36dc32b9--------------------------------)![安东尼·阿尔卡拉兹](https://medium.com/@alcarazanthony1?source=post_page-----749d36dc32b9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----749d36dc32b9--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----749d36dc32b9--------------------------------) [安东尼·阿尔卡拉兹](https://medium.com/@alcarazanthony1?source=post_page-----749d36dc32b9--------------------------------)

·发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----749d36dc32b9--------------------------------) ·6 分钟阅读·2023 年 12 月 29 日

--

*人工智能软件被用来增强本文文本的语法、流畅性和可读性。*

最近，大型语言模型（LLM）设计中的创新推动了少样本学习和推理能力的快速进步。然而，尽管取得了进展，LLM 在处理涉及大量相互关联知识的复杂现实世界情境时仍面临限制。

为了应对这一挑战，一种有前景的方法在*检索增强生成*（RAG）系统中出现。RAG 结合了 LLM 的自适应学习优势与从外部知识源（如知识图谱（KGs））中进行可扩展检索的能力。与其试图将所有信息静态编码在模型中，RAG 允许根据需要从索引知识图谱中动态查询所需的上下文。

然而，有效地协调跨互联知识源的推理和检索带来了自身的挑战。那些简单地在离散步骤中检索和拼接信息的天真方法，往往无法充分捕捉密集知识图谱中的细微差别。概念的相互关联性意味着，如果没有相互关系的分析，关键的上下文细节可能会被遗漏。

最近，一个名为 LLM Compiler 的有趣框架在优化 LLM 中多个函数调用的协调方面展示了早期成功，自动处理依赖关系并允许并行执行。

[](https://arxiv.org/abs/2312.04511?source=post_page-----749d36dc32b9--------------------------------) [## 一个用于并行函数调用的 LLM 编译器

### 大型语言模型（LLM）在各种复杂推理基准上表现出了显著的成果。推理...

arxiv.org](https://arxiv.org/abs/2312.04511?source=post_page-----749d36dc32b9--------------------------------) [](https://github.com/run-llama/llama-hub/blob/main/llama_hub/llama_packs/agents/llm_compiler/llm_compiler.ipynb?source=post_page-----749d36dc32b9--------------------------------) [## llama-hub/llama_hub/llama_packs/agents/llm_compiler/llm_compiler.ipynb at main ·…

### 一个由社区创建的用于大语言模型的数据加载器库——可与 LlamaIndex 和/或 LangChain 一起使用…

github.com](https://github.com/run-llama/llama-hub/blob/main/llama_hub/llama_packs/agents/llm_compiler/llm_compiler.ipynb?source=post_page-----749d36dc32b9--------------------------------)

在本文中，我们探讨了将大语言模型编译器技术更广泛地应用于知识图谱检索和推理的潜力。在论文发布之前，我们已经做了一个工作原型：

[](/achieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a?source=post_page-----749d36dc32b9--------------------------------) ## 实现混乱背景下的结构化推理：思路提示和…

### 大语言模型（LLMs）展示了令人印象深刻的少量学习能力，迅速适应新任务…

towardsdatascience.com

我们分析了诸如自动规划、依赖管理和并行执行等技术如何使对互联知识的分析更加高效和结构化。

## 计划：

## 1\. 大规模知识图谱推理的挑战

## 2\. 知识图谱作为模块化大语言模型工具

## 3\. 由大语言模型编译器驱动的结构化推理

## 4\. 知识吸收的操作系统

—

# 1\. 大规模知识图谱推理的挑战

[](https://github.com/tomasonjo/blogs/blob/master/llm/devops_rag.ipynb?source=post_page-----749d36dc32b9--------------------------------) [## blogs/llm/devops_rag.ipynb at master · tomasonjo/blogs

### 支持我图数据科学博客文章的 Jupyter 笔记本，网址为 https://bratanic-tomaz.medium.com/ …

github.com](https://github.com/tomasonjo/blogs/blob/master/llm/devops_rag.ipynb?source=post_page-----749d36dc32b9--------------------------------)

在解决大规模知识图谱推理的挑战时，必须整合先进的技术和方法来优化数据检索和处理。这涉及利用各种计算策略来平衡分析大规模互联数据集中的效率、准确性和完整性。以下部分详细介绍了这些方法：

## a. 优化 Cypher 查询以进行数学运算

**目标：** 提高 Cypher 查询的性能，特别是针对图数据的数学聚合。

**方法论：**

**- 识别机会：** 识别可以高效应用数学聚合的图数据场景。

**- 模块化步骤：** 将过程分解为并行检索组、并发执行聚合函数和高效连接。

**- 示例应用：** 计算分配给 A 队的高优先级任务数量。这涉及到检索任务节点、按优先级和团队进行筛选，并汇总计数。

## b. 规划并行向量搜索

**目标：** 实现并行向量搜索，以高效导航图基数据。

**方法论：**

- 分析种子实体问题：剖析查询以确定作为向量搜索起点的关键实体。

- 探索并发向量空间：从不同的种子实体启动搜索，同时探索图。

- 持续节点检索：基于向量相似性动态检索节点，扩展有针对性的搜索。

- 示例应用：通过探索以不同实体为根的向量空间来识别描述类似于‘PaymentService’的服务。

## c. 协调图算法的使用

**目标：** 选择并应用最合适的图算法以处理特定查询。

**方法论：**

- 检查算法选择问题：分析查询的性质，以选择相关算法（如遍历、社区检测）。

- 模块化应用：以模块化的方式应用算法，每个算法针对特定目标进行优化（例如效率、准确性）。

- 解决依赖关系：确保算法之间的协调，特别是在一个算法的输出需要用于另一个算法的执行时。

- 示例应用：使用社区检测和遍历算法找到紧密连接的服务子图。

## LLM 编译器协调

一个 LLM 编译器可以作为这些任务的中央协调者。它将管理数学运算的 Cypher 查询的执行，监督并行向量搜索，并协调各种图算法的应用。LLM 编译器在此背景下的主要目标是确保知识图谱推理的整个过程不仅高效和准确，而且全面。这需要对数据结构有深入的理解，能够预测计算需求，并能根据实时数据分析结果动态调整策略。

# 2\. 知识图谱作为模块化 LLM 工具

Kim 等人 2023 年提出的 LLM 编译器系统包括 3 个关键组件：

1.  **LLM 规划器：** 分解任务并构建依赖图

1.  **任务提取单元：** 分派处理依赖的任务

1.  **执行者：** 并行运行工具函数

值得注意的是，LLM 编译器将工具视为可以并发执行的模块化函数（当可能时）。

在这一范式的基础上，一个引人入胜的观点是将**知识图谱本身视为模块化工具**，由 LLM 进行协调：

+   **查询引擎**通过知识图谱（KGs）成为 LLM 访问的工具

+   **图算法和嵌入**提供了工具级定制

+   **LLM 规划器**确定最佳的多图探索策略

通过这种框架，LLM 编译器的自动化规划和结构化执行技术对于复杂知识检索显得非常有前景。

# 3\. 由 LLM 编译器驱动的结构化推理

通过专门化 LLM 编译器技术来整合多个工具和执行图，我们可以提升知识图谱推理的多个方面：

## a. 并行探索

规划分解将允许从多个“入口点”同时查询知识图谱的不同区域。多跳路径可以并行遍历，加速探索。

## b. 模块化检索

针对独特子图的查询引擎使用独特的算法和嵌入，实际上充当了独立的工具。LLM 编译器在整合不同的模块能力方面表现出色。

## c. 依赖管理

中间查询结果通常会影响后续的搜索。LLM 编译器对工具间依赖关系的自动处理能够实现实体和关系的无缝传播。

## d. 递归重新规划

对于复杂问题，后续的检索阶段高度依赖于之前的阶段。LLM 编译器允许在更新的上下文揭示新依赖时进行递归重新规划。

## e. 本体辅助规划

提供关键实体高层次模式的本体可以进一步协助规划模块。元级知识指导更加结构化的任务分解，符合领域概念。

## e. 多样化数据源

规划器可以将额外的数据源，如 SQL 数据库、图像和互联网搜索引擎，整合为 LLM 可访问的模块化工具。

通过在本体知识驱动的结构化规划框架下协调知识检索模块，LLM 编译器可以实现对广泛信息空间的显著高效和精准的导航。

# 4\. 知识吸收的操作系统

从宏观上看，我们可以设想 LLM 编译器技术会产生一个新范式——LLM 作为操作系统来监督各种知识功能。就像操作系统平衡线程和内存一样，LLM 可能会在知识存储中调度检索、推理和学习。基于 ML 的优化器将自动调整各种“工具”模块的路由和使用。

在这个生态系统中，LLM 编译器将充当关键的工作流协调框架，接口连接操作系统和工具。它将处理 LLM 操作系统代表下的所有依赖解析、并发优化和资源分配的复杂性。

## LLM 作为操作系统

LLM 作为协调者，监督并在各个模块之间分配资源：

+   为检索、推理和学习功能之间的最佳并发安排计划

+   将查询和决策路由到最适合的组件

+   监控执行以动态优化分配

+   随着新能力的上线，实现模块化扩展性

## 语言模型作为语义粘合剂

语言模型提供了将不同组件联系在一起的共同语义表示：

+   将查询、检索的上下文、生成的决策嵌入到共享向量空间

+   在组件接口之间翻译查询和指令

+   通过将输入/输出编码为可传输的嵌入，在线路之间传播信号

## 语言模型作为推理引擎

语言模型在专业组件的输入上进行元推理：

+   通过基于更广泛的理解解决歧义来进行信号的语境化

+   将低层次的输出抽象为更高层次的见解

+   在概念基础上评估假设和解释

+   确定解释性推论，连接相互关联的输出

语言模型因此在计算和语义上充当了狭义和广义 AI 之间的连接纽带。

![](img/c0d43e2911cd543b4c638bbda1270e0f.png)

由 Dall-E-3 生成的图像

# 结论

随着语言模型不断成熟为更通用的推理引擎，围绕它们的架构创新仍然至关重要。特别是，有效利用外部知识为克服固有模型局限性提供了一条途径。语言模型编译器及其协调模块功能的技术提供了一个有前景的范式，用于构建可扩展的知识检索结构。

在开发这些连接和大规模整合检索能力方面还有很多工作要做。

但通过将知识框定为由语言模型编译器协调的模块化工具，我们为在复杂推理中实现平衡的效率、简洁性和连贯性打开了有趣的可能性。

在这些方面的进展仍然至关重要，因为我们期望实现真正的系统智能。
