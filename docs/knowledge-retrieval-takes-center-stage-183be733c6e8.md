# 知识检索占据了中心舞台

> 原文：[https://towardsdatascience.com/knowledge-retrieval-takes-center-stage-183be733c6e8?source=collection_archive---------2-----------------------#2023-11-16](https://towardsdatascience.com/knowledge-retrieval-takes-center-stage-183be733c6e8?source=collection_archive---------2-----------------------#2023-11-16)

![](../Images/e902f2d1bc5402065cee49b02394f230.png)

*图片来源：Adobe Stock。*

## **GenAI 架构从 RAG 转向解释性检索为中心的生成（RCG）模型**

[](https://gadi-singer.medium.com/?source=post_page-----183be733c6e8--------------------------------)[![Gadi Singer](../Images/293941f11306a6e2100c2375ccb1a85a.png)](https://gadi-singer.medium.com/?source=post_page-----183be733c6e8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----183be733c6e8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----183be733c6e8--------------------------------) [Gadi Singer](https://gadi-singer.medium.com/?source=post_page-----183be733c6e8--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F51de1f48d0b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fknowledge-retrieval-takes-center-stage-183be733c6e8&user=Gadi+Singer&userId=51de1f48d0b&source=post_page-51de1f48d0b----183be733c6e8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----183be733c6e8--------------------------------) ·13 分钟阅读·2023 年 11 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F183be733c6e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fknowledge-retrieval-takes-center-stage-183be733c6e8&user=Gadi+Singer&userId=51de1f48d0b&source=-----183be733c6e8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F183be733c6e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fknowledge-retrieval-takes-center-stage-183be733c6e8&source=-----183be733c6e8---------------------bookmark_footer-----------)

要使 GenAI 从消费者向业务部署过渡，解决方案应主要围绕使用检索为中心的生成（RCG）所需的模型外部信息构建。

随着生成型人工智能（GenAI）开始在各行业部署，公司需要提供效率、准确性、安全性和可追溯性的模型。ChatGPT等模型的原始架构显示出无法满足这些关键需求的主要差距。在早期的GenAI模型中，检索被用作事后补救，以解决依赖参数化内存中记忆信息的模型的缺点。当前的模型通过在解决方案平台上增强[检索增强生成（RAG）](https://www.techtarget.com/searchenterpriseai/definition/retrieval-augmented-generation)前端已在此问题上取得显著进展，以允许提取模型外的信息。也许现在是进一步重新思考生成型人工智能架构的时候了，并且从RAG系统转向以检索为核心访问信息的检索中心生成（RCG）模型。

检索中心生成模型可定义为一种生成型人工智能解决方案，专为系统设计，其中绝大多数数据存储在模型参数化内存之外，并且在预训练或微调中大多数情况下并未见过。使用RCG时，GenAI模型的主要角色是解释从公司索引数据语料库或其他策划内容中检索到的丰富信息。模型不是记忆数据，而是专注于对目标结构、关系和功能的精细调整。生成输出的数据质量预期达到100%的准确性和及时性。适当解释和使用大量未在预训练中看到的数据要求模型更多地抽象化，并使用模式作为识别信息中复杂模式和关系的关键认知能力。这种检索要求与模式自动学习相结合，将推动大型语言模型的预训练和微调进一步演进。

![](../Images/f59a7dc4a68fb4565a6ec7030a198b8d.png)

*图 1\. 检索中心生成（RCG）与检索增强生成（RAG）的优势和挑战。图片来源：Intel Labs.*

大幅减少在生成式人工智能模型中使用记忆中的数据，而是依赖可验证的索引源，将改善来源，并在提升准确性和性能方面发挥重要作用。到目前为止，生成式人工智能架构中的普遍假设是模型中的数据越多越好。基于这一目前主流的结构，预计大多数标记和概念已被吸收和交叉映射，以便模型可以从其参数记忆中生成更好的答案。然而，在常见的商业场景中，生成输出所用的大部分数据预计来自检索的输入。我们现在观察到，模型中有更多数据而依赖检索知识会导致信息冲突，或包括无法追溯或验证其来源的数据。正如我在上一篇博客中概述的，[适者生存](/survival-of-the-fittest-compact-generative-ai-models-are-the-future-for-cost-effective-ai-at-scale-6bbdc138f618)，设计为使用RCG的小型灵活目标模型不需要在参数记忆中存储大量数据。

在数据主要来自检索的商业环境中，目标系统需要在解释未见相关信息以满足公司要求方面表现出色。此外，大型向量数据库的普及和上下文窗口大小的增加（例如，OpenAI最近将[GPT-4 Turbo的上下文窗口](https://openai.com/blog/new-models-and-developer-products-announced-at-devday)从32K增加到128K）正在推动模型向推理和解释未见复杂数据的方向发展。现在，模型需要智能化地将广泛的数据转化为有效的知识，通过结合复杂的检索和微调来实现。随着模型变得以检索为中心，创建和利用模式的认知能力将成为焦点。

## **消费者与商业使用的生成式人工智能**

在经历了十年的人工智能模型规模和复杂度的快速增长后，2023年标志着一个关注效率和生成式人工智能有针对性应用的转变。从消费者焦点转向业务使用是推动这种变化的关键因素之一，涉及三个层面：数据质量、数据来源和目标用途。

● **数据质量：** 在为公司生成内容和分析时，95%的准确性是不够的。企业需要接近或达到完全准确。为了确保输出质量，需要对特定任务进行高性能的微调，并管理所用数据的质量。此外，数据需要可追溯和可验证。来源很重要，而检索对于确定内容来源至关重要。

-   **数据来源：** 期望在商业应用程序中的绝大多数数据都是从可信的外部来源以及专有的业务/企业数据中策划出来的，包括产品信息、资源、客户、供应链、内部操作等等。检索对于访问未在模型中预先训练的最新和最广泛的专有数据至关重要。无论模型大小如何，当使用来自其内部存储器的数据与从业务来源提取的可验证、可追溯数据时，数据的来源都可能存在问题。如果数据有冲突，可能会使模型混淆。

-   **目标用途：** 公司模型的构造和功能往往专门用于一组用途和数据类型。当GenAI功能部署在特定的工作流程或业务应用程序中时，不太可能需要全能功能。由于数据主要来自检索，因此目标系统需要在解释公司特定方式所需的相关信息方面表现出色，这些信息对模型来说是看不见的。

例如，如果一个金融或医疗公司追求GenAI模型来改善其服务，它将专注于一系列用于其预期用途所需的函数族。他们可以选择从头开始预训练一个模型，并尝试包含所有他们的专有信息。然而，这样的努力可能会非常昂贵，需要深厚的专业知识，并且很可能随着技术的发展和公司数据的不断变化而迅速落后。此外，它无论如何都需要依赖检索来访问最新的具体信息。更有效的路径是采用现有的预训练基础模型（例如[Meta的Llama 2](https://ai.meta.com/llama/)）并通过微调和索引来定制它以用于检索。微调只使用了信息和任务的一小部分来改进模型的行为，但广泛的业务专有信息本身可以被索引，并根据需要进行检索。随着基础模型更新到最新的GenAI技术，刷新目标模型应该是一个相对简单的过程，只需重复微调流程。

## **转向以检索为中心的生成：围绕索引信息提取进行架构设计**

[Meta AI及其大学合作伙伴](https://arxiv.org/pdf/2005.11401v4.pdf)在2021年引入了检索增强生成（RAG），以解决LLMs中的来源和更新世界知识的问题。研究人员使用RAG作为一种通用方法，将非参数记忆添加到经过预训练的参数记忆生成模型中。非参数记忆使用由预训练检索器访问的Wikipedia密集向量索引。在一个数据记忆较少的紧凑模型中，非常重视由向量数据库引用的索引数据的广度和质量，因为模型不能依赖于记忆信息来满足业务需求。RAG和RCG都可以使用相同的检索器方法，通过在推理时实时提取相关知识（见图2）。它们在GenAI系统放置信息的方式以及对以前未见数据的解释期望上存在差异。在RAG中，模型本身是主要的信息来源，检索到的数据作为辅助。而在RCG中，大部分数据存在于模型的参数记忆之外，使得解释未见数据成为模型的主要角色。

需要注意的是，目前许多RAG解决方案依赖于诸如[LangChain](https://www.langchain.com/)或[Haystack](https://haystack.deepset.ai/)等流程，将前端检索与独立的向量存储连接到一个未经过检索预训练的GenAI模型。这些解决方案提供了一个用于索引数据源、选择模型和模型行为训练的环境。其他方法，如[Google Research的REALM](https://arxiv.org/pdf/2002.08909.pdf)，则尝试通过集成检索进行端到端的预训练。目前，OpenAI正在优化其检索GenAI路径，而不是让生态系统为ChatGPT创建流程。该公司最近发布了[Assistants API](https://openai.com/blog/new-models-and-developer-products-announced-at-devday)，该API可以检索模型外的专有领域数据、产品信息或用户文档。

![](../Images/4f37941f9f583e66a33f712a78dd368f.png)

*图2\. RCG和RAG在推理过程中检索公共和私人数据，但它们在如何放置和解释未见数据方面存在差异。图像来源：Intel Labs。*

在其他例子中，像 [Intel Labs 的 fastRAG](https://github.com/IntelLabs/fastRAG) 这样的快速检索模型使用预训练的小型基础模型从知识库中提取请求的信息，而无需额外训练，提供了更可持续的解决方案。fastRAG 作为开源 Haystack GenAI 框架的扩展，使用 [检索模型](https://medium.com/@daniel.fleischer/open-domain-q-a-using-dense-retrievers-in-fastrag-65f60e7e9d1e) 通过从外部知识库中检索当前文档来生成对话回答。此外，Meta 的一组研究人员最近发布了一篇论文，介绍了 [检索增强双重指令微调](https://arxiv.org/pdf/2310.01352.pdf)（RA-DIT），一种“轻量级微调方法，通过为任何大型语言模型增加检索能力提供第三种选项”。

从 RAG 到 RCG 模型的转变挑战了信息在训练中的作用。在 RAG 中，信息既是信息的存储库，又是对提示的解释者，而在 RCG 中，模型的功能主要转变为对检索到的（通常是商业策划的）信息进行上下文解释。这可能需要对预训练和微调方法进行修改，因为目前用于训练语言模型的目标可能不适用于这种学习方式。RCG 对模型要求不同的能力，如更长的上下文、数据的可解释性、数据的策划以及其他新挑战。

在学术界或工业界，RCG 系统的实例仍然相当少。在一个例子中，Kioxia Corporation 的研究人员创建了开源的 [SimplyRetrieve](https://arxiv.org/abs/2308.03983)，该系统使用 RCG 架构通过分离上下文解释和知识记忆来提升 LLM 的性能。在 Wizard-Vicuna-13B 模型上实现时，研究人员发现 RCG 能准确回答关于某组织工厂位置的查询。相比之下，RAG 尝试将检索到的知识库与 Wizard-Vicuna 对该组织的知识进行整合，这导致了部分错误信息或幻觉。这只是一个例子——RAG 和检索关闭生成（ROG）在其他情况下可能会提供正确的回答。

![](../Images/e593642774258ad91a8c041c2d1ce5c3.png)

*图 3\. 检索中心生成（RCG）、检索增强生成（RAG）和检索关闭生成（ROG）的比较。正确的回答以蓝色显示，而幻觉以红色显示。图片来源：* [*Kioxia Corporation*](https://arxiv.org/pdf/2308.03983.pdf)*。*

从RAG过渡到RCG可以类比于在编程中使用常量（RAG）和变量（RCG）的区别。当一个AI模型回答关于一辆可转换的福特野马的问题时，大型模型将熟悉许多与汽车相关的细节，例如引入年份和发动机规格。大型模型还可以添加一些最近检索到的更新，但它主要会根据特定的内部已知术语或常量来回答。然而，当一个模型被部署在准备其下一款车型发布的电动车公司时，该模型需要进行推理和复杂解释，因为大部分数据都是未见过的。模型需要理解如何使用这类信息，例如变量的值，来理解数据。

## **架构：推理过程中的泛化与抽象能力**

在商业环境中检索的大部分信息（商业组织与人员、产品与服务、内部流程和资产）在预训练期间相应的GenAI模型可能没有看到，并且在精调期间可能仅仅是抽样。这意味着变压器架构不会将“已知”的单词或术语（即模型先前摄取的内容）作为生成输出的一部分。相反，该架构需要在适当的上下文解释中放置未见过的术语。这在某种程度上类似于上下文学习如何使LLM中的一些新推理能力成为可能，而无需额外的训练。

随着这一变化，进一步提升泛化和抽象能力变得不可或缺。需要增强的关键能力是，当在推理过程中遇到未见过的术语或标记时，使用学到的模式进行解释和应用的能力。[认知科学中的模式](https://en.wikipedia.org/wiki/Schema_(psychology))“描述了一种组织信息类别及其关系的思维或行为模式。” [心理模式](https://en.wikipedia.org/wiki/Mental_schema)“可以被描述为一种心理结构，代表世界的某个方面。”类似地，在GenAI模型中，模式是解释未见过的标记、术语和数据所需的基本抽象机制。如今的模型已经展示了对新兴模式构建和解释的相当理解，否则它们无法在复杂的未见过的提示上下文数据上执行生成任务。随着模型检索到之前未见过的信息，它需要识别与数据最匹配的模式。这使得模型能够通过与模式相关的知识来解释未见过的数据，而不仅仅是利用上下文中包含的显性信息。值得注意的是，在这次讨论中，我指的是学习和抽象模式作为新兴能力的神经网络模型，而不是依赖于知识图谱中显式模式并在推理时参考的解决方案类别。

从[模型能力](/survival-of-the-fittest-compact-generative-ai-models-are-the-future-for-cost-effective-ai-at-scale-6bbdc138f618)（认知能力、功能技能和信息获取）这三种类型的视角来看，抽象和模式使用完全属于认知能力类别。特别是，如果小型模型能在构建和使用模式来解释数据的技能上有所提高，它们应能与更大模型的表现相当（在获得适当的数据的前提下）。预计与模式相关的基于课程的预训练将提升模型的认知能力。这包括模型构建多种模式的能力、根据生成过程识别合适的模式，以及插入/利用模式构建的信息以创造最佳结果。

例如，研究人员展示了当前的LLM如何使用[假设到理论](https://arxiv.org/pdf/2310.07064.pdf)（HtT）框架来学习基本的模式。研究人员发现，LLM可以用来生成规则，然后按照这些规则解决数值和关系推理问题。GPT-4发现的规则可以视为理解家庭关系的详细模式（见图4）。未来的家庭关系模式可能会更加简洁和强大。

![](../Images/6f3a5e4db5d251891d0491cc1f8b9b2e.png)

*图 4\. 使用 CLUTRR 数据集进行关系推理，Hypotheses-to-Theories 框架促使 GPT-4 生成类似模式的规则供 LLM 在回答测试问题时遵循。图像来源:* [*Zhu et al*](https://arxiv.org/pdf/2310.07064.pdf)*.*

将此应用于一个简单的商业案例，GenAI 模型可以使用一种模式来理解公司供应链的结构。例如，知道“B 是 A 的供应商”和“C 是 B 的供应商”意味着在分析文档以评估潜在的供应链风险时，“C 是 A 的二级供应商”是重要的。

在一个更复杂的案例中，比如教会 GenAI 模型记录患者就医情况的变化和细微差别，在预训练或微调期间建立的紧急模式将为理解检索的信息提供结构，以生成报告或支持医疗团队的问题和答案。该模式可以在模型中通过更广泛的患者护理案例训练/微调中出现，包括预约以及测试和程序等其他复杂元素。当 GenAI 模型接触到所有示例时，它应该创建解读在推理期间提供的部分患者数据的专业知识。模型对过程、关系和变化的理解将使其能够正确解释以前未见过的患者案例，而无需在提示中提供过程信息。相反，它不应尝试记忆在预训练或微调过程中接触到的特定患者信息。这种记忆是不利的，因为患者信息不断变化。模型需要学习构造，而不是特定的案例。这种设置还可以最小化潜在的隐私问题。

## **总结**

随着 GenAI 在各行各业的大规模部署，企业对高质量专有信息的依赖以及对可追溯性和可验证性的要求显著增加。这些关键要求，加上对成本效率和集中的应用的压力，推动了对小型、针对性的 GenAI 模型的需求，这些模型旨在解释局部数据，这些数据在预训练过程中大多未曾见过。以检索为中心的系统需要提升一些深度学习 GenAI 模型可以掌握的认知能力，例如构建和识别合适的模式。通过使用 RCG 并指导预训练和微调过程，以创建反映认知结构的泛化和抽象，GenAI 可以在理解模式和从检索中理解未见数据的能力上取得飞跃。精炼的抽象（例如基于模式的推理）和高效的认知能力似乎是下一个前沿。

## **了解更多：GenAI 系列**

[生存之道：紧凑生成AI模型是规模化成本效益AI的未来](/survival-of-the-fittest-compact-generative-ai-models-are-the-future-for-cost-effective-ai-at-scale-6bbdc138f618)

## **参考资料**

1.  Gillis, A. S. (2023年10月5日)。 检索增强生成。 Enterprise AI。 [https://www.techtarget.com/searchenterpriseai/definition/retrieval-augmented-generation](https://www.techtarget.com/searchenterpriseai/definition/retrieval-augmented-generation)

1.  Singer, G. (2023年7月28日)。 生存之道：紧凑生成AI模型是规模化成本效益AI的未来。 Medium。 [https://towardsdatascience.com/survival-of-the-fittest-compact-generative-ai-models-are-the-future-for-cost-effective-ai-at-scale-6bbdc138f618](/survival-of-the-fittest-compact-generative-ai-models-are-the-future-for-cost-effective-ai-at-scale-6bbdc138f618)

1.  在DevDay宣布的新模型和开发者产品。 (n.d.)。 [https://openai.com/blog/new-models-and-developer-products-announced-at-devday](https://openai.com/blog/new-models-and-developer-products-announced-at-devday)

1.  Meta AI。 (n.d.)。 介绍Llama 2。 [https://ai.meta.com/llama/](https://ai.meta.com/llama/)

1.  Lewis, P. (2020年5月22日)。 检索增强生成用于知识密集型自然语言处理任务。 arXiv.org。 [https://arxiv.org/abs/2005.11401](https://arxiv.org/abs/2005.11401)

1.  LangChain。 (n.d.)。 [https://www.langchain.com](https://www.langchain.com)

1.  Haystack。 (n.d.)。 Haystack。 [https://haystack.deepset.ai/](https://haystack.deepset.ai/)

1.  Guu, K. (2020年2月10日)。 REALM：检索增强语言模型预训练。 arXiv.org。 [https://arxiv.org/abs/2002.08909](https://arxiv.org/abs/2002.08909)

1.  Intel Labs。 (n.d.)。 GitHub — Intel Labs/FastRAG：高效检索增强和生成框架。 GitHub。 [https://github.com/IntelLabs/fastRAG](https://github.com/IntelLabs/fastRAG)

1.  Fleischer, D. (2023年8月20日)。 在fastRAG中使用稠密检索器的开放域问答 — Daniel Fleischer — Medium。 [https://medium.com/@daniel.fleischer/open-domain-q-a-using-dense-retrievers-in-fastrag-65f60e7e9d1e](https://medium.com/@daniel.fleischer/open-domain-q-a-using-dense-retrievers-in-fastrag-65f60e7e9d1e)

1.  Lin, X. V. (2023年10月2日)。 RA-DIT：检索增强双指令调整。 arXiv.org。 [https://arxiv.org/abs/2310.01352](https://arxiv.org/abs/2310.01352)

1.  Ng, Y. (2023年8月8日)。 SimplyRetrieve：私密轻量级检索中心生成AI工具。 arXiv.org。 [https://arxiv.org/abs/2308.03983](https://arxiv.org/abs/2308.03983)

1.  Wikipedia contributors。 (2023年9月27日)。 模式（心理学）。 Wikipedia。 [https://en.wikipedia.org/wiki/Schema_(psychology)](https://en.wikipedia.org/wiki/Schema_(psychology))

1.  Wikipedia contributors。 (2023年8月31日)。 心理模型。 Wikipedia。 [https://en.wikipedia.org/wiki/Mental_schema](https://en.wikipedia.org/wiki/Mental_schema)

1.  Zhu, Z. (2023年10月10日). 大型语言模型可以学习规则. arXiv.org. [https://arxiv.org/abs/2310.07064](https://arxiv.org/abs/2310.07064)
