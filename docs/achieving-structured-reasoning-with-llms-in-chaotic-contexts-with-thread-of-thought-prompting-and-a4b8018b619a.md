# 在混乱背景下通过思路引导和并行知识图谱检索实现结构化推理

> 原文：[`towardsdatascience.com/achieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a`](https://towardsdatascience.com/achieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a)

![Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----a4b8018b619a--------------------------------) [Anthony Alcaraz](https://medium.com/@alcarazanthony1?source=post_page-----a4b8018b619a--------------------------------) ![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4b8018b619a--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a4b8018b619a--------------------------------) ·阅读时长 8 分钟·2023 年 11 月 18 日

--

*人工智能软件被用来增强本文的语法、流畅性和可读性。*

大型语言模型（LLMs）展现了令人印象深刻的少样本学习能力，能够通过少量示例迅速适应新任务。

[## 为什么 RAG（检索增强生成）将成为使用 LLM 系统设计的基石](https://ai.plainenglish.io/why-rag-retrieval-augmented-generation-will-become-a-cornerstone-of-system-design-using-llm-719657fbfebe?source=post_page-----a4b8018b619a--------------------------------)

### 最近在大型语言模型（LLMs）如 GPT-3 [1]中的进展显示了强大的少样本学习能力——

[ai.plainenglish.io](https://ai.plainenglish.io/why-rag-retrieval-augmented-generation-will-become-a-cornerstone-of-system-design-using-llm-719657fbfebe?source=post_page-----a4b8018b619a--------------------------------)

然而，尽管有这些进展，大型语言模型（LLMs）在涉及充满不相关事实的复杂推理中仍面临局限。为了解决这一挑战，研究人员探索了如链式思维引导等技术，这些技术引导模型逐步分析信息。然而，这些方法单独使用时，难以全面捕捉广泛背景中的所有关键细节。

本文提出了一种将线程思维（Thread-of-Thought, ToT）提示与检索增强生成（Retrieval Augmented Generation, RAG）框架相结合的技术，该框架并行访问多个知识图谱。ToT 作为思维的“骨架”来构建推理，而 RAG 系统则扩展了可用知识以填补空白。与顺序检索相比，信息来源的并行查询提高了效率和覆盖面。综合来看，这一框架旨在提升 LLM 在混乱情境中的理解和解决问题的能力，更接近于人类认知。

我们首先概述了在相关和无关事实混合的混乱环境中对结构化推理的需求。接着，我们介绍了 RAG 系统设计及其如何扩展 LLM 的可访问知识。然后，我们解释了如何集成 ToT 提示，以系统地引导 LLM 进行逐步分析。最后，我们讨论了如并行检索等优化策略，以高效地同时查询多个知识来源。

通过概念解释和 Python 代码示例，本文阐明了一种将 LLM 的优势与互补外部知识相结合的新技术。这种创造性的整合突显了克服模型固有限制并提升 AI 推理能力的有希望方向。所提出的方法旨在提供一个通用的框架，可随着 LLM 和知识库的发展进一步增强。

# 结构化推理的需求

尽管 LLM 在语言理解上取得了进展，但其推理能力仍然有限。当面临包含多个不相关事实的复杂情境时，模型往往会迷失或遗漏关键信息。

1.  混乱的情境包括处理问题/任务所需的相关事实，以及推理过程中不必要的无关信息。

1.  相关和无关的内容混合在一起，而不是分隔成清晰的部分。

1.  事实没有明确的逻辑进展或分组。顺序可能完全随机，而不是以结构化方式构建。

1.  相关和无关的细节深度交织。例如，一个关键的相关句子可能夹在两个无关句子之间。

1.  结构缺失意味着模型不能依赖信息的位置或顺序来确定相关性。相关事实可能出现在任何地方。

1.  上下文中包含的信息密度和体量很高。许多事实跳出来，但很少有明确突出的相关信息。

1.  信息过载的混乱混合使得哪些细节以有意义的方式相互关联变得模糊。它破坏了逻辑线索。

1.  在没有连贯线索的情况下，模型很容易丧失关键点，被混乱所掩盖。

1.  干扰破坏了推理过程，使模型错过了需要连接多个分散细节的洞察。

1.  这使得基于相关信息有条理地构建理解变得具有挑战性。推理变得随机。

从本质上讲，混乱的背景使模型过载于信号和噪声的复杂交集。这种纠缠即使对结构化推理技术也造成了压力，显示出需要额外解决方案的差距。

为了解决这个问题，引入了线索提示（ToT）策略。受人类认知的启发，ToT 引导模型逐步分析上下文。这一技术提高了理解和推理能力，而无需重新训练模型。

[](https://paperswithcode.com/paper/thread-of-thought-unraveling-chaotic-contexts?source=post_page-----a4b8018b619a--------------------------------) [## Papers with Code - 线索解锁混乱背景]

### 目前还没有代码。

paperswithcode.com](https://paperswithcode.com/paper/thread-of-thought-unraveling-chaotic-contexts?source=post_page-----a4b8018b619a--------------------------------)

然而，ToT 提示在极端混乱的背景下单独存在局限性。本文提出将其与访问多样知识源的 RAG 系统互补。

# 介绍 RAG 系统

检索增强生成（RAG）结合了 LLM 的能力和可扩展检索。索引的知识源允许模型快速适应，利用少量示例学习，而不是静态编码所有信息。

我们设计了一个具有多个知识图谱（KGs）的 RAG 系统，这些知识图谱通过 LLama 索引进行索引。对于每个 KG，我们：

1.  创建一个查询引擎来检索相关段落作为上下文

1.  构建一个工具来封装查询引擎

1.  为工具构建一个与 LLM 互动的代理

1.  将所有代理组合成一个超级代理，协调 KG 工具

并行查询不同的 KGs 扩展了可用知识，而少量示例学习实现了快速适应。

```py
from llama_index.indices.knowledge_graph import KnowledgeGraphRAGRetriever
from llama_index.agent import OpenAIAgent
from llama_index.tools import QueryEngineTool, ToolMetadata
from llama_index.llms import OpenAI
from llama_index.memory import ChatMemoryBuffer

memory = ChatMemoryBuffer.from_defaults(token_limit=1000)

# Your knowledge graphs
my_kgs = {'kg1': kg_index, 'kg2': kg_index2}

# Dictionary to store the agents
kg_agents = {}

# List to store the tools
kg_tools = []

for kg_name, kg in my_kgs.items():
    # Create a query engine for the KG
    query_engine = kg.as_query_engine(
        include_text=True,
        response_mode="tree_summarize",
        embedding_mode="hybrid",
        similarity_top_k=20,
    )

    # Create a tool for the query engine
    tool = QueryEngineTool(
        query_engine=query_engine,
        metadata=ToolMetadata(
            name=f"tool_{kg_name}",
            description=f"Useful for questions related to {kg_name}",
        ),
    )

    # Add the tool to the list of KG tools
    kg_tools.append(tool)

    # Create an agent for the tool
    agent = OpenAIAgent.from_tools([tool], system_prompt="Walk me through this context in manageable parts step by step, summarizing and analyzing as we go.")

    # Add the agent to the dictionary of KG agents
    kg_agents[kg_name] = agent

# Create the super agent
llm = OpenAI(model="gpt-3.5-turbo-1106")
super_agent = OpenAIAgent.from_tools(kg_tools, llm=llm, verbose=True, memory=memory, system_prompt="Therefore, the answer.")
```

# 整合 ToT 提示

为了启用结构化推理，我们将 ToT 提示集成到 RAG 系统中。

超级代理提示 LLM：

*“逐步引导我通过这个上下文，将其分解为可管理的部分，并在过程中总结和分析”*

这启动了对检索段落的增量分析。

然后，代理使用以下方式提取结论：

*“因此，答案是：”*

通过 ToT 推理和多样知识的引导增强理解，避免忽视关键事实。

# 实施并行检索

为了高效查询多个 KGs，我们利用异步并行检索：

```py
import nest_asyncio
nest_asyncio.apply()

response = await super_agent.astream_chat("How to optimize value of an asset")

# Collect all tokens into a string
response_text = ""
async for token in response.async_response_gen():
    response_text += token

# Print the response text
print(response_text)
```

与顺序查询相比，同时查询 KGs 可以加速检索。

不仅知识范围，还包括所使用的底层图算法和嵌入可以提供互补的推理能力。例如：

+   优化用于遍历的图算法，如个性化 PageRank，可以推断概念之间的间接联系。这支持更灵活的联想推理。

+   为搜索调优的算法（如近似最近邻）可以有效地找到相关实体。这有助于精确的事实查找。

+   知识图谱嵌入将符号转换为适合数学操作的向量空间。这使得类比推理和向量运算成为可能。

+   多模态嵌入结合了文本、图像、音频等。这支持视觉语义推理。

同样，嵌入的约束和训练目标也会影响推理：

+   传递嵌入通过事实链改善逻辑推理。

+   等价类嵌入将同义词汇总在一起。这有助于泛化。

+   层次嵌入捕捉类之间的分类关系。这允许基于继承的推理。

通过协调不同的算法和嵌入策略，我们在同一个总体框架内创造了不同的推理模式。语言模型可以根据每个查询灵活地使用适当的推理风格。这种增强的推理能力接近于人类认知的多面推理能力。

RAG 系统的灵活架构使得知识图谱和推理引擎可以根据不同的用途进行交织。这种紧密的框架将混合推理整合在一个屋檐下，同时保持知识来源的模块化和可扩展性。

例如，在医学知识图谱中，我们可以利用：

+   传递嵌入通过逻辑推理改善临床决策。这允许从已知交互链推断新的药物交互。

+   等价类嵌入将同义医学术语分组。这有助于不同术语（如普通术语和专家术语）之间的泛化。

+   层次嵌入用于捕捉疾病、症状和治疗的分类。这使得基于继承的推理成为可能，比如从某个特定癌症推断其母类癌症的特性。

# 首先查询本体

```py
# Initial question 
initial_question = "Define assets? And optimize them"

# Query the ontology with the initial question
ontology_results = ontology_engine.query(initial_question)
# Combine the initial question with the ontology results
combined_question = f"{initial_question}. {ontology_results}"
# Query the super agent with the combined question
response = await super_agent.astream_chat(combined_question) 
# Collect all tokens into a string
response_text = ""
async for token in response.async_response_gen():
  response_text += token
# Print the response text
print(response_text)
```

这允许首先利用本体获取关于查询的定义和高层次知识。本体的结果被附加到原始问题上，以提供背景。

增强的问题包含本体信息，然后输入到超级代理。这在从知识图谱中检索之前，为 RAG 系统提供初步背景。

首先查询本体提供了 RAG 系统的“热启动”，通过建立基本概念和背景。知识图谱可以随后填充更具体的关系事实，以适应具体的查询。

这种技术将本体的自上而下的层次推理与自下而上的详细知识检索结合起来。本体澄清了模糊或广泛的术语，而知识图谱则提供了关于精细查询方面的深入信息。

以这种阶段性的方法编排不同的知识源，能够在不同的抽象层次之间平滑流动。这从高层次概念逐渐推进到与问题相关的上下文细节。

# 结论

本文提出了一种方法，通过将思维线程（Thread-of-Thought）提示与检索增强生成系统（RAG）相结合，来增强大型语言模型在复杂背景下的推理能力。

思维线程技术提供了结构化的思维序列，用于系统地分析混乱的信息。与此同时，RAG 框架通过并行查询不同的知识图谱来扩展可获取的知识。

这种混合技术旨在协调 LLMs 与互补的外部知识源的优势。引导模型在从丰富的上下文数据中汲取信息的同时逐步推理，增强了其理解能力。

所提出的方法提供了一个通用框架来解决固有的模型限制。随着 LLMs 与知识库的不断发展，这种技术提供了一条逐步增加结构化推理能力的途径。

尽管在复制多面的人的推理方面仍面临挑战，但在提示策略、知识基础和并行架构方面的创新揭示了有前景的方向。

本文强调了创造性地结合现有方法的优势，以实现超越部分之和的 emergent 能力的价值。随着我们对 LLMs 能力的编排了解的深入，类似于本提案的元架构可能成为主流范式。

向前发展，适应性地利用大型语言模型（LLMs）以及结构化知识和推理技术仍然是推动人工智能进步的关键支柱。随着持续研究探索这些交集，人类语言理解的梦想可能逐渐接近现实。

![](img/4db009311e011b5157b3d3d1eb5225f3.png)

作者提供的图像
