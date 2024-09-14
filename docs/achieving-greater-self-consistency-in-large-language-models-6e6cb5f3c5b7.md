# 实现大型语言模型的更大自我一致性

> 原文：[`towardsdatascience.com/achieving-greater-self-consistency-in-large-language-models-6e6cb5f3c5b7`](https://towardsdatascience.com/achieving-greater-self-consistency-in-large-language-models-6e6cb5f3c5b7)

[](https://medium.com/@alcarazanthony1?source=post_page-----6e6cb5f3c5b7--------------------------------)![安东尼·阿尔卡拉斯](https://medium.com/@alcarazanthony1?source=post_page-----6e6cb5f3c5b7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6e6cb5f3c5b7--------------------------------)![面向数据科学](https://towardsdatascience.com/?source=post_page-----6e6cb5f3c5b7--------------------------------) [安东尼·阿尔卡拉斯](https://medium.com/@alcarazanthony1?source=post_page-----6e6cb5f3c5b7--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e6cb5f3c5b7--------------------------------) ·8 分钟阅读·2023 年 12 月 1 日

--

*人工智能软件用于增强本文文本的语法、流畅性和可读性。*

当 LLM 用于评估文本的正确性、准确性或相关性等质量时，一致性至关重要。如果 LLM 表现出不一致的判断，那么其评估就会变得不可靠和不可信。

如果 LLM 评估论证的推理质量，但通过将无效的论证评为比完全有效的论证更有逻辑，从而自相矛盾，那么它就无法作为理性的仲裁者。由于模型自身缺乏逻辑一致性，其评估失去可信度。

当出现这种不一致性时，就没有稳定的基础来比较 LLM 对不同文本的评估。如果模型随意自相矛盾，那么句子无法根据模型的不一致评分可靠地排序。

本质上，不一致性破坏了评估旨在提供的比较基础。如果一个 LLM 不能展示一致的评估标准应用，那么使用它来评估文本就失去了所有的有效性和实用性。

因此，对于被用于评分或评判文本质量和特征的 LLM 来说，一致性是必需的。如果评估没有高水平的稳定性，并且基于对被评估概念的一致理解，那么在将 LLM 输出作为评估或评分形式时，比较的基础就会崩溃。

采样多个解决方案表明，输出之间的一致性与质量有很强的相关性。然而，现有的一致性技术依赖于提取和匹配封闭形式的答案，限制了其适用性。本文探讨了在没有这些限制的情况下提高自我一致性的方法，同时也将决策建立在现实世界知识上。

![](img/8f1bb2f51d372d80a08c389810cc3436.png)

作者提供的图像

## **自洽性的需求**

尽管取得了迅速的进展，逻辑失败和虚假信息仍然阻碍了最先进模型中的可靠推理。对于复杂的多步骤分析或自由形式生成，模型往往会自相矛盾或编造没有支持的事实。

这表现为两个关键方面——不一致的开放式生成和不连贯的推理。在执行开放式任务时，模型在同一输入上多次采样时生成冲突的输出。与此同时，对于链式推理，模型得出违反基本传递性质的非理性结论。

例如，模型可能在排名比较中确定 A > B 和 B > C，但随后不准确地评估 C > A，导致循环矛盾。这种未能保持传递一致性的问题从根本上削弱了可靠性。

像自洽性这样的技术通过采样多个候选解决方案并选择显示共识的输出来帮助解决不一致性。关键的见解是共识作为质量和连贯性的有效代理度量。

然而，现有的自洽性方法依赖于严格的答案格式，以便在响应之间进行提取和比较。这大大限制了它们在开放式任务中的适用性。

提升内部自洽性需要双管齐下的方法。首先，一致性技术如 USC 消除了格式限制，允许开放式选择。同时，对比排序方法对潜在表示施加逻辑约束，确保模型在多步骤推理过程中保持顺序关系。

然而，仅仅增强内部一致性而不依赖于结构化的外部知识仍不足以实现准确推理。语言模型缺乏动态更新、逻辑表达能力和经验验证，这些都是解释所需的关键因素。

因此，提升可靠性需要在各种任务中提高自洽性，同时整合结构化的世界知识库。一致性技术应在自由形式输出中灵活运作，并结合利用丰富语义表示的框架，以逻辑驱动的、外部验证的事实来指导决策。

这种模型内部共识与结构化外部基础的结合补充了每种方法的优势，以复制多方面的人类推理。只有它们的协同作用才能克服固有的局限性，逐步推进 AI 能力，以匹配多样且流动的认知。

## 引入通用自洽性

[## 通用自一致性用于大型语言模型生成](https://arxiv.org/abs/2311.17311?source=post_page-----6e6cb5f3c5b7--------------------------------#:~:text=Universal%20Self%2DConsistency%20for%20Large%20Language%20Model%20Generation,-Xinyun%20Chen%2C%20Renat&text=Self%2Dconsistency%20with%20chain%2Dof,large%20language%20models%20%28LLMs%29.)

### 使用链式思维提示（CoT）的自一致性在各种任务上表现出显著的性能提升……

arxiv.org](https://arxiv.org/abs/2311.17311?source=post_page-----6e6cb5f3c5b7--------------------------------#:~:text=Universal%20Self%2DConsistency%20for%20Large%20Language%20Model%20Generation,-Xinyun%20Chen%2C%20Renat&text=Self%2Dconsistency%20with%20chain%2Dof,large%20language%20models%20%28LLMs%29.)

陈 2023 等人提出了**通用自一致性**（USC），以在不同应用场景中实现自一致性，而无需从中提取答案。USC 依赖于语言模型自身的能力，从其最初生成的多个候选解中选择最一致的回答。

具体而言，USC 将所有采样的回答连接成一个上下文，然后构建一个提示，要求模型选择其中共识度最高的回答。通过消除专业的聚合逻辑，USC 即使在自由形式生成任务中也适用。

实验表明，USC 在允许提取答案的数学推理和代码生成基准测试中的表现与标准自一致性相匹配。至关重要的是，USC 还改善了开放式问答、总结和创意写作，这些领域现有技术存在不足。

## 通过外部知识增强推理

尽管 USC 提供了与框架无关的一致性，但外部知识对于准确和稳健的推理仍然至关重要。语言模型缺乏策划知识库的动态更新、事实完整性和逻辑表现力。

知识图谱允许结合经验基础的细节和相互关联的事实关系，根据最新信息提供更一致的情境解读。它们有助于访问上下文的经验证据，以支持决策，而不是仅依赖于模型权重中嵌入的固有偏见。

此外，知识图谱管理着事实随时间的演变，确保推理依赖于最新的信息。它们还封装了领域逻辑，以显式编码规则、约束和本体分类法。这使得合理的推理无法仅从模型训练数据中辨别。

经验上，利用外部知识图谱的检索增强生成通过将回答建立在已验证的事实上而不是幻觉上，展示了更一致的输出。知识图谱的并行查询——即使是重复副本——通过从多个角度积累证据进一步提高了一致性。

通过使用 USC 的结构化推理与检索增强系统的协调，可以利用外部知识来形成一个模块化的混合架构。USC 提供了推理的“骨干”，而并行检索则从可信的知识库中提供相关的事实细节，增强了解释的一致性。

## 使用种子采样增强一致性

[](https://github.com/openai/openai-cookbook/blob/main/examples/Deterministic_outputs_with_the_seed_parameter.ipynb?source=post_page-----6e6cb5f3c5b7--------------------------------) [## openai-cookbook/examples/Deterministic_outputs_with_the_seed_parameter.ipynb 在 main 分支下 ·…](https://github.com/openai/openai-cookbook/blob/main/examples/Deterministic_outputs_with_the_seed_parameter.ipynb?source=post_page-----6e6cb5f3c5b7--------------------------------)

### 使用 OpenAI API 的示例和指南。通过创建账户来贡献 openai/openai-cookbook 的开发…

github.com](https://github.com/openai/openai-cookbook/blob/main/examples/Deterministic_outputs_with_the_seed_parameter.ipynb?source=post_page-----6e6cb5f3c5b7--------------------------------)

在开放式生成任务中，解决方案之间的一致性与质量强相关。然而，大型语言模型展示了固有的随机性，这会导致输出的变异。

为了缓解这一问题，OpenAI 的 Chat Completions API 提供了一个种子参数，该参数用于种子随机数生成器以进行确定性采样。使用相同的种子和参数，每次将产生相同或非常相似的输出。

这使得将通用自我一致性（Universal Self-Consistency），即模型从候选中选择最一致的响应，与种子采样结合起来，以在请求之间评估完全相同的响应成为可能。

计算机中仍然存在因非确定性带来的微小变异机会。此外，对系统 _fingerprint 的更改表示后端更新影响了请求的确定性。

除了种子采样外，较低的温度和仔细的提示工程也能减少模型生成文本的变异性。总体而言，种子采样使我们能够利用一致性来提高可靠性。

通过与 USC 基于选择的种子候选进行编排，我们系统地构建了既基于外部知识又基于采样确定性的解决方案。这体现了超越单一技术的涌现能力。

## 对比一致性排名（CCR）

尽管前面的部分侧重于分类和开放式生成，但从语言模型中引出的排名一致性也是至关重要的。最近的工作探索了对比一致排名（CCR），它适应了对比一致搜索（CCS）来对映射项目表示施加逻辑约束，以实现一致的尺度。

CCR 探测项目向量，以无监督的方式找到一致的排名方向。对提示基线和 CCR 变体的实验显示出改进的一致性和泛化能力。

通过将基于一致性的技术如 USC 扩展到排名，CCR 提供了一种限制不可预测性并使模型输出在多样任务中对齐的方法。两者都旨在通过选择模型自身认为内部一致的解决方案或排名来提高可靠性。

[](https://arxiv.org/abs/2309.06991?source=post_page-----6e6cb5f3c5b7--------------------------------) [## 无监督对比一致性排名与语言模型

### 语言模型包含基于排名的知识，并且是处理上下文排名任务的强大解决器。例如……

arxiv.org](https://arxiv.org/abs/2309.06991?source=post_page-----6e6cb5f3c5b7--------------------------------)

CCR 探测训练一个额外的“探测”模型，以在固定的语言模型的向量空间中找到潜在的排名方向。

+   它不会直接提示或微调语言模型本身。

+   语言模型仅用于通过输入提示生成项目的向量表示。

+   这些项目向量随后被输入到 CCR 探测器中，该探测器在无监督损失函数上进行训练，将表示映射到一致的排名尺度上。

总结来说，CCR 探测引入了一个外部探测模型，该模型将语言模型包装起来，以在其向量空间中进行排序。语言模型保持静态。只有额外的探测模型在对比目标下进行训练，以揭示内在的排名。

## 技术架构

[](/achieving-structured-reasoning-with-llms-in-chaotic-contexts-with-thread-of-thought-prompting-and-a4b8018b619a?source=post_page-----6e6cb5f3c5b7--------------------------------) ## 在混乱上下文中通过思维线程提示和……实现结构化推理

### 大型语言模型（LLMs）展现了令人印象深刻的少样本学习能力，能够快速适应新任务……

towardsdatascience.com

我们实现了一个 RAG 系统，通过向量搜索访问多个知识图以进行快速事实检索。查询引擎接口到索引，并封装段落搜索。辅助工具包装引擎，促进集成。独立代理负责工具的接口，与 LLM 进行对接。超级代理负责工具协调。

系统利用思维线程（ToT）提示进行结构化推理，对检索的段落进行分析。ToT 指导模型通过逐步分析，增强理解。并行异步检索允许同时查询所有图，加速上下文积累。

使用多种算法和嵌入的多模态知识图提供了不同的视角。个性化 PageRank 遍历支持沿间接连接进行灵活推理。近似最近邻搜索实现了高效的查找。嵌入通过向量运算实现类比推理。

我们实施了一种分阶段的检索方法，首先查询领域本体以建立与问题相关的基本概念和术语。本体提供了正式的定义和实体之间的高层次关系，在检索完整的知识图谱之前实现了基础理解。

本体结果附加到原始问题中以提供背景。这个增强的查询随后用于从多个知识图谱中检索段落。通过本体信息种子检索，可以为 RAG 系统提供更一致和相关的段落，这些段落是针对具体基础方面量身定制的。

这一技术将本体的自上而下的层级推理与知识图谱中互补的自下而上的事实细节相结合。本体澄清了模糊或广泛的术语，而知识图谱提供了对细化查询焦点的深入关系信息。

以这种分阶段的方式编排不同的知识来源，可以在抽象层次之间平滑过渡，增强一致性。检索从高层次的本体概念流向与问题相关的特定背景段落。

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

## 影响

结合 USC 和 RAG 将基于一致性的思维与外部知识的基础结合起来。USC 提供了推理结构，而 RAG 扩展了信息的广度。这些结合起来弥补了 LLM 的局限性，更好地复制了人类认知。

这种编排还提高了准确性、速度和覆盖范围。检索到的事实填补了知识空白，以做出合理的决策。并行知识访问加速了理解。不同的知识图谱拓宽了考虑的概念连接。

通过模块化增强，我们优雅地扩展了新兴能力超越固有模型的适应性。随着 LLM 和知识库的发展，这种可组合的范式将促进 AI 推理能力的逐步提升。
