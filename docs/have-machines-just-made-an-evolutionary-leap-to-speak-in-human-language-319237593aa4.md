# 机器是否刚刚实现了在人的语言中进行进化性的飞跃？

> 原文：[https://towardsdatascience.com/have-machines-just-made-an-evolutionary-leap-to-speak-in-human-language-319237593aa4?source=collection_archive---------15-----------------------#2023-04-17](https://towardsdatascience.com/have-machines-just-made-an-evolutionary-leap-to-speak-in-human-language-319237593aa4?source=collection_archive---------15-----------------------#2023-04-17)

![](../Images/d5cbdf6bebeefae99fe4cb4903ef20f3.png)

图片来源：Adobe Stock。

## **评估我们在实现人与人工智能之间深层次、有意义的沟通的旅程中的位置**

[](https://gadi-singer.medium.com/?source=post_page-----319237593aa4--------------------------------)[![Gadi Singer](../Images/293941f11306a6e2100c2375ccb1a85a.png)](https://gadi-singer.medium.com/?source=post_page-----319237593aa4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----319237593aa4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----319237593aa4--------------------------------) [Gadi Singer](https://gadi-singer.medium.com/?source=post_page-----319237593aa4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F51de1f48d0b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhave-machines-just-made-an-evolutionary-leap-to-speak-in-human-language-319237593aa4&user=Gadi+Singer&userId=51de1f48d0b&source=post_page-51de1f48d0b----319237593aa4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----319237593aa4--------------------------------) · 12 min read · 2023年4月17日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F319237593aa4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhave-machines-just-made-an-evolutionary-leap-to-speak-in-human-language-319237593aa4&user=Gadi+Singer&userId=51de1f48d0b&source=-----319237593aa4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F319237593aa4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhave-machines-just-made-an-evolutionary-leap-to-speak-in-human-language-319237593aa4&source=-----319237593aa4---------------------bookmark_footer-----------)

人们与对话型人工智能（AI）系统互动时，清晰的沟通是获得最佳效果的关键因素，这将最有利于提升我们的生活。从更广泛的角度来看，应使用什么语言来控制系统和与机器对话？在这篇博客文章中，我们评估了基于最近技术创新的方法，例如OpenAI的ChatGPT和GPT-4，来引导和对话机器的发展，并探讨对话AI在掌握自然对话方面的下一步所需的步骤。机器已经从提示工程跃升到“人类语言”，但其他智能方面仍在等待发现。

直到2022年之前，让AI做出正确响应并发挥其优势需要专业知识，如复杂的提示工程。ChatGPT的推出是机器对话能力的重大进步，使得即使是高中生也能与高效能AI聊天并获得令人印象深刻的结果。这是一个重要的里程碑。然而，我们也需要评估在人与机器沟通的旅程中，我们处于什么位置，以及还需要什么来实现与AI的有意义对话。

人与机器的互动有两个主要目标：一是指导机器完成所需任务，二是在任务执行过程中交换信息和指导。第一个目标传统上是通过编程来实现，但现在正在演变为用户对话可以定义新任务，例如请求AI创建一个Python脚本来完成任务。在任务执行中的交流是通过自然语言处理（NLP）或自然语言理解（NLU）结合机器响应生成来实现的。我们可以假设，人机互动进展的核心特征——如果不是终点——是当人们可以像与老朋友一样与机器沟通，包括所有的自由形式的语法、语义、隐喻和文化方面的内容。在AI系统要全面参与这种自然交流时，需要创造什么？

> 机器已经从提示工程跃升到“人类语言”，但其他智能方面仍在等待发现。

## **过去的对话AI：变换器架构重新定义了NLP性能**

![](../Images/0e2d58942092f9d0995c0cfbc4f7588f.png)

*图1\. 机器编程和指令的发展。图片来源：经Intel Labs许可。*

在计算初期，人类和机器只能通过机器码进行通信，这是一种低级的二进制计算机语言——由0和1组成的字符串，与人类通信几乎没有任何相似之处。过去一个世纪以来，我们逐渐实现了让与机器的交流更接近人类语言的旅程。如今，我们能够让机器生成一张猫在下棋的图片，这是巨大进步的证明。随着编程语言从低级到高级代码的演进，从汇编语言到C语言再到Python，以及引入像if-then语句这样类似人类语言的结构，这种交流逐渐改善。现在的最终步骤是消除输入措辞微调的敏感性，使机器和人类可以以自然的方式互动。人机对话应该允许根据过去的“保存点”进行增量引用以继续对话。

自然语言处理（NLP）处理计算机与人类语言之间的交互，以处理和分析大量自然语言数据，而自然语言理解（NLU）则承担了检测用户意图的困难任务。像Alexa和Google这样的虚拟助手使用NLP、NLU和机器学习（ML）在运行时获取新知识。通过使用预测性智能和分析，AI可以根据用户偏好个性化对话和响应。虽然虚拟助手像是人们家中的可信朋友，但它们目前仍然受限于基本的命令语言循环。人们已经适应了这一点，通过说出“关键字语言”来获得最佳结果，但他们的对话AI在理解自然语言交互方面仍然存在差距。当与虚拟助手出现沟通中断时，人们会使用[修复策略](https://www.frontiersin.org/articles/10.3389/fcomp.2022.791704/full)，如简化话语、调整信息量的变化、语义和句法上的查询调整以及重复命令。

![](../Images/688b6b2b167b5169a5dee9f582f9e07b.png)

*图2。自然语言理解中的六个意图层级。图片来源：经由Intel Labs许可使用。*

在理解意图方面，自然语言理解（NLU）至关重要。NLU 分析文本和语音以确定其含义（见图 2）。使用对人类语言的语义和语用定义的数据模型，NLU 专注于意图和实体识别。2018 年引入的 Transformer 神经网络架构使虚拟助手的自然语言处理（NLP）性能得到了提升。这些类型的网络利用自注意力机制来处理输入数据，从而有效捕捉人类语言中的依赖关系。由 [Google AI Language 的研究人员](https://arxiv.org/abs/1810.04805) 提出的 BERT 解决了一个模型中的 11 个最常见的 NLP 任务，改进了传统的为每个特定任务使用独立模型的方法。BERT 通过在大规模文本语料库（如维基百科）上训练通用语言理解模型，来预训练语言表示，然后将模型应用于下游 NLP 任务，如问答和语言推理。

除了虚拟助手之外，ChatGPT 的进步与 Transformer 模型在 NLP 性能上的提升是相辅相成的。GPT-3 Transformer 技术于 2021 年引入，但其在流行度和使用上的重大突破是通过 ChatGPT 实现的，其在人类对话界面的创新得以通过 [来自人类反馈的强化学习（RLHF）](https://en.wikipedia.org/wiki/Reinforcement_learning_from_human_feedback) 的应用实现。ChatGPT 使大型语言模型能够处理和理解自然语言输入，并生成尽可能接近人类的输出。

## **当前：大型语言模型主导对话式人工智能**

自 2022 年 11 月由 OpenAI 发布以来，[ChatGPT](https://openai.com/blog/chatgpt) 以其似乎写得很好的语言生成能力和成功通过医学执照及 MBA 考试的表现主导了新闻。ChatGPT 在没有任何训练或强化的情况下通过了 [美国医学执照考试](https://www.medrxiv.org/content/10.1101/2022.12.19.22283643v2.full)（USMLE）的所有三个考试。这导致研究人员得出结论：“大型语言模型可能有潜力协助医学教育，并有可能辅助临床决策。” 一位 [宾夕法尼亚大学沃顿商学院的教授](https://mackinstitute.wharton.upenn.edu/2023/would-chat-gpt3-get-a-wharton-mba-new-white-paper-by-christian-terwiesch/) 对 ChatGPT 进行了运营管理 MBA 期末考试测试，其成绩为 B 到 B-。ChatGPT 在基于案例研究的基础运营管理和过程分析问题上表现良好，提供了正确的答案和充分的解释。当 ChatGPT 未能将问题与正确的解决方法匹配时，人类专家的提示帮助模型纠正了答案。尽管这些结果令人鼓舞，但 ChatGPT 在达到人类水平的对话方面仍有局限性（我们将在下一节讨论）。

-   作为一个自回归语言模型，ChatGPT 拥有 1750 亿个参数，其庞大的模型尺寸帮助其在理解用户意图方面表现出色。基于图 2 中的意图水平，ChatGPT 可以通过分析文本提示的目标来处理用户包含开放参数集和灵活结构的实用请求。ChatGPT 能够撰写高度详细的回答和清晰的答案，展示出对医学、业务运营、计算机编程等不同领域的广泛和深刻的知识。GPT-4 也显示出了令人印象深刻的优势，例如添加多模态能力和在高级人类测试中提高得分。据报道，GPT-4 在统一巴尔考试中的得分为 90 分位数（ChatGPT 为 10 分位数），在美国生物奥林匹克竞赛中的得分为 99 分位数（ChatGPT 为 31 分位数）。

[ChatGPT 也可以进行机器编程](https://www.zdnet.com/article/how-to-use-chatgpt-to-write-code/)，尽管程度有限。它可以创建多种语言的程序，包括 [Python, JavaScript, C++, Java](https://seo.ai/blog/how-many-languages-does-chatgpt-support)，等等。它还可以 [分析代码中的错误和性能问题](https://www.zdnet.com/article/chatgpt-can-write-code-now-researchers-say-its-good-at-fixing-bugs-too/)。然而，目前看来，最好的利用方式似乎是作为程序员和人工智能的联合组合的一部分。

-   尽管 OpenAI 的模型吸引了很多关注，其他模型也在类似的方向上取得进展，例如 Google Brain 的开源 1.6 万亿参数的 [Switch Transformer](https://arxiv.org/abs/2101.03961)，该模型在 2021 年首次亮相，以及使用 LaMDA（用于对话应用的语言模型）技术的 [Google Bard](https://blog.google/technology/ai/bard-google-ai-search-updates/)。Bard 目前仅对测试用户开放，因此其与 ChatGPT 的表现尚不为人知。

-   尽管大语言模型在与人类进行自然对话方面取得了巨大进展，但仍需要解决关键增长领域。

> -   要达到下一个智能水平和人类级沟通，关键领域需要能力飞跃 —— 知识重组，多技能整合和提供上下文适应。

## -   **未来：人机对话缺少什么？**

-   在将会话型人工智能推向下一个水平的过程中，仍然存在四个关键元素的缺失，这些元素包括达成自然对话的亲密性和共享目的。要达到这一水平，机器需要理解个体的象征性沟通的含义，并用有意义的、可信赖的定制回应来回应。

**1) 生成可信的回应。** AI 系统不应该产生幻觉！认识论问题影响了 AI 构建知识的方式，以及区分已知和未知信息的能力。当机器在不了解的事物上提供答案时，可能会出错，产生有偏见的结果，甚至是幻觉。ChatGPT 在捕捉来源归因和信息出处方面存在困难。它可以生成听起来合理但错误或荒谬的答案。此外，它在处理物理、空间和时间问题以及数学推理方面缺乏事实的正确性和常识。据 OpenAI 称，它在处理“如果我把奶酪放进冰箱，它会融化吗？”这样的问题时表现不佳。在 [MBA 期末考试](https://mackinstitute.wharton.upenn.edu/2023/would-chat-gpt3-get-a-wharton-mba-new-white-paper-by-christian-terwiesch/) 上测试时，它在六年级水平的数学中出现了令人惊讶的错误，可能导致操作上的重大错误。研究发现，“Chat GPT 在处理更复杂的流程分析问题时能力不足，即使这些问题基于标准模板。这包括具有多种产品和需求变异等随机效应的流程流程。”

**2) 对人类符号和特殊习惯的深刻理解。** AI 需要在人类的完整符号世界内工作，包括能够进行抽象、定制和理解部分引用。机器必须能够解释人们言辞中的模棱两可和不完整的句子，以便进行有意义的对话。

![](../Images/fa99e2701e7ffe3b7b6712a01aee2544.png)

*图 3\. 从* [*Switchboard 收集*](https://catalog.ldc.upenn.edu/LDC97S62) *中的一个例子，展示了来自美国各地发言者之间的双向电话对话。图片来源：经过 Intel Labs 的许可。*

正如图 3 所示，人类的言语模式经常是难以理解的。AI 是否应该像人类一样说话？实际上，它可以与朋友一样使用松散结构的语言进行交流，包括合理数量的“嗯嗯”、“喜欢”、“不完整或不完整的句子”、“模棱两可”、“语义抽象”、“个人引用” 和常识推断。但这些人类言语的特殊习惯不应该使沟通变得难以理解。

**3) 提供定制化响应。** AI 需要具备定制能力并熟悉用户的世界。ChatGPT 经常猜测用户的意图，而不是提出澄清问题。此外，作为一个完全封装的信息模型，ChatGPT 不具备浏览或搜索互联网以提供定制答案的能力。根据 OpenAI 的说法，[ChatGPT 的定制答案有限](https://arxiv.org/abs/2005.14165)，因为“它对每个标记的权重相等，缺乏对预测中最重要和较不重要内容的概念。通过自我监督目标，任务规格依赖于将期望任务强制变成预测问题，而最终，像虚拟助手这样的有用语言系统可能更应被视为采取目标导向的行动，而不仅仅是进行预测。”此外，拥有多会话的人工与机器对话的上下文，以及[用户的心理理论模型](/beyond-input-output-reasoning-four-key-properties-of-cognitive-ai-3f82cde8cf1e)会很有帮助。

**4) 成为目标驱动。** 当人们与伴侣合作时，协调不仅仅基于文本交换，而是基于共同的目标。AI 需要超越上下文答案，变得目标驱动。在不断发展的人工与机器关系中，双方需要成为实现目标、避免或减轻问题，或共享信息的过程的一部分。ChatGPT 和其他 LLM 尚未达到这种交互水平。正如我在之前的博客中探讨的那样，智能机器需要[超越输入输出回复和对话](https://medium.com/towards-data-science/beyond-input-output-reasoning-four-key-properties-of-cognitive-ai-3f82cde8cf1e)作为聊天机器人。

为了达到下一水平的智能和人类级别的沟通，关键领域需要在能力上实现飞跃——知识的重组、技能的整合以及提供上下文适应。

## **通往对话 AI 下一水平的道路**

像 ChatGPT 这样的 LLM 仍在认知技能上存在差距，这些技能是将对话 AI 提升到下一水平所必需的。缺失的能力包括逻辑推理、时间推理、数值推理以及整体上驱动目标的能力和定义子任务以完成更大任务的能力。ChatGPT 和其他 LLM 的知识相关限制可以通过[Thrill-K 方法](https://community.intel.com/t5/Blogs/Tech-Innovation/Artificial-Intelligence-AI/Thrill-K-A-Blueprint-for-The-Next-Generation-of-Machine/post/1397951?wapkw=gadi+singer+thrill-K)来解决，通过增加检索和持续学习。知识对于 AI 存在于三个地方：

**1) 即时知识.** 常用的知识和连续功能，可以在参数化内存中的最快最昂贵的层或其他ML处理的工作内存中有效近似，ChatGPT目前使用这种端到端的深度学习系统，但需要扩展以包括其他知识来源，以便作为人类伴侣更有效。

**2) 待机知识.** 对AI系统有价值但不常用的知识，通过需要时从相邻的结构化知识库提取。它需要增强对离散实体的表示能力，或者需要保持广泛而灵活，以适应各种新的使用方式。基于待机知识的行动或结果需要处理和内部解决，使得AI能够像人类伴侣一样学习和适应。

**3) 检索的外部知识.** 来自广阔在线存储库的信息，供AI系统外检索时使用。这使得AI能够根据人类伴侣的需求定制答案，提供理性分析，并解释信息来源和达到结论的路径。

## **总结**

从机器语言到人类语言的旅程已经从人类输入简单的二进制数字到计算机，发展到在家里使用虚拟助手执行简单任务，再到向像ChatGPT这样的LLM询问并接收明确答案。尽管LLM在近期的创新取得了巨大进展，但要达到下一个水平的对话型AI，需要知识重组、多重智能和上下文适应，以构建真正的人类伴侣。

## **参考文献**

1.  Mavrina, L., Szczuka, J. M., Strathmann, C., Bohnenkamp, L., Krämer, N. C., & Kopp, S. (2022). “Alexa, You’re Really Stupid”: A Longitudinal Field Study on Communication Breakdowns Between Family Members and a Voice Assistant. *计算机科学前沿*, *4*. [https://doi.org/10.3389/fcomp.2022.791704](https://doi.org/10.3389/fcomp.2022.791704)

1.  Devlin, J. (2018年, 10月11日). BERT: 深度双向转换器的预训练用于语言理解. arXiv.org. [https://arxiv.org/abs/1810.04805](https://arxiv.org/abs/1810.04805)

1.  Wikipedia贡献者. (2023年). 从人类反馈中的强化学习. 维基百科. [https://en.wikipedia.org/wiki/Reinforcement_learning_from_human_feedback](https://en.wikipedia.org/wiki/Reinforcement_learning_from_human_feedback)

1.  介绍ChatGPT. (无日期). [https://openai.com/blog/chatgpt](https://openai.com/blog/chatgpt)

1.  Kung, T. H., Cheatham, M., Medenilla, A., Sillos, C., De Leon, L., Elepaño, C., Madriaga, M., Aggabao, R., Diaz-Candido, G., Maningo, J., & Tseng, V. (2022)。ChatGPT 在 USMLE 上的表现：利用大型语言模型进行 AI 辅助医学教育的潜力。medRxiv（冷泉港实验室）。 [https://doi.org/10.1101/2022.12.19.22283643](https://doi.org/10.1101/2022.12.19.22283643)

1.  Needleman, E. (2023)。Chat GPT 能获得沃顿 MBA 吗？Christian Terwiesch 新白皮书。Mack 创新管理研究所。 [https://mackinstitute.wharton.upenn.edu/2023/would-chat-gpt3-get-a-wharton-mba-new-white-paper-by-christian-terwiesch/](https://mackinstitute.wharton.upenn.edu/2023/would-chat-gpt3-get-a-wharton-mba-new-white-paper-by-christian-terwiesch/)

1.  OpenAI. (2023)。GPT-4 技术报告。arXiv（康奈尔大学）。 [https://doi.org/10.48550/arxiv.2303.08774](https://doi.org/10.48550/arxiv.2303.08774)

1.  Gewirtz, D. (2023年4月6日)。如何使用 ChatGPT 编写代码。ZDNET。 [https://www.zdnet.com/article/how-to-use-chatgpt-to-write-code/](https://www.zdnet.com/article/how-to-use-chatgpt-to-write-code/)

1.  ChatGPT 支持多少种语言？完整的 ChatGPT 语言列表。 (n.d.). [https://seo.ai/blog/how-many-languages-does-chatgpt-support](https://seo.ai/blog/how-many-languages-does-chatgpt-support)

1.  Tung, L. (2023年2月2日)。ChatGPT 能编写代码。现在研究人员说它也擅长修复漏洞。ZDNET。 [https://www.zdnet.com/article/chatgpt-can-write-code-now-researchers-say-its-good-at-fixing-bugs-too/](https://www.zdnet.com/article/chatgpt-can-write-code-now-researchers-say-its-good-at-fixing-bugs-too/)

1.  Fedus, W., Zoph, B., & Shazeer, N. (2021)。Switch Transformers：以简单有效的稀疏性扩展到万亿参数模型。arXiv（康奈尔大学）。 [https://doi.org/10.48550/arxiv.2101.03961](https://doi.org/10.48550/arxiv.2101.03961)

1.  Pichai, S. (2023年2月6日)。我们 AI 旅程中的一个重要下一步。Google。 [https://blog.google/technology/ai/bard-google-ai-search-updates/](https://blog.google/technology/ai/bard-google-ai-search-updates/)

1.  Dickson, B. (2022年7月31日)。大型语言模型无法计划，即使它们写出花哨的文章。TNW | Deep-Tech。 [https://thenextweb.com/news/large-language-models-cant-plan](https://thenextweb.com/news/large-language-models-cant-plan)

1.  Brown, T., Mann, B. F., Ryder, N., Subbiah, M., Kaplan, J., Dhariwal, P., Neelakantan, A., Shyam, P., Sastry, G., Askell, A., Agarwal, S., Herbert-Voss, A., Krueger, G., Henighan, T., Child, R., Ramesh, A., Ziegler, D. M., Wu, J. C., Winter, C., . . . Amodei, D. (2020)。语言模型是少样本学习者。arXiv（康奈尔大学）。 [https://doi.org/10.48550/arxiv.2005.14165](https://doi.org/10.48550/arxiv.2005.14165)

1.  Singer, G. (2022年8月17日). 超越输入输出推理：认知人工智能的四个关键特性。Medium. [https://towardsdatascience.com/beyond-input-output-reasoning-four-key-properties-of-cognitive-ai-3f82cde8cf1e](/beyond-input-output-reasoning-four-key-properties-of-cognitive-ai-3f82cde8cf1e)

1.  Singer, G. (2022年1月6日). Thrill-K：下一代机器智能的蓝图。Medium. [https://towardsdatascience.com/thrill-k-a-blueprint-for-the-next-generation-of-machine-intelligence-7ddacddfa0fe](/thrill-k-a-blueprint-for-the-next-generation-of-machine-intelligence-7ddacddfa0fe)
