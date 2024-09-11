# 使用 LLM 分类源代码 — 什么与如何

> 原文：[https://towardsdatascience.com/classifying-source-code-using-llms-what-and-how-f04c7dbcba9b?source=collection_archive---------2-----------------------#2023-12-28](https://towardsdatascience.com/classifying-source-code-using-llms-what-and-how-f04c7dbcba9b?source=collection_archive---------2-----------------------#2023-12-28)

## 分享我们制作基于 LLM 的源代码分类器的经验

[](https://medium.com/@oriabramovski?source=post_page-----f04c7dbcba9b--------------------------------)[![Ori Abramovsky](../Images/28b758ecfc0d759584913be5feb6a72a.png)](https://medium.com/@oriabramovski?source=post_page-----f04c7dbcba9b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f04c7dbcba9b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f04c7dbcba9b--------------------------------) [Ori Abramovsky](https://medium.com/@oriabramovski?source=post_page-----f04c7dbcba9b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa6b24a84c43&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclassifying-source-code-using-llms-what-and-how-f04c7dbcba9b&user=Ori+Abramovsky&userId=a6b24a84c43&source=post_page-a6b24a84c43----f04c7dbcba9b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f04c7dbcba9b--------------------------------) · 14 分钟阅读 · 2023年12月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff04c7dbcba9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclassifying-source-code-using-llms-what-and-how-f04c7dbcba9b&user=Ori+Abramovsky&userId=a6b24a84c43&source=-----f04c7dbcba9b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff04c7dbcba9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fclassifying-source-code-using-llms-what-and-how-f04c7dbcba9b&source=-----f04c7dbcba9b---------------------bookmark_footer-----------)![](../Images/b6d8df4044604cfbc0b0fd8b2f8d4edc.png)

照片由 [Iurii Ivashchenko](https://www.pexels.com/photo/shallow-focus-photo-of-two-brown-lions-3498323/) 拍摄

源代码AI已成为一种常见的用例，具有许多实际和多样化的实现（缺陷检测，[代码补全](https://github.com/features/copilot)等）。源代码AI最有趣的方面之一是它面临的重大变革；如果不久前，源代码分类的常见方法是训练定制的DNN、依赖于[嵌入](https://code2vec.org/)或甚至使用经典的NLP技术如[词袋模型（BOW）](https://en.wikipedia.org/wiki/Bag-of-words_model)，如今，大型语言模型（LLMs）已成为主要的工具。更具体地说，‘[上下文学习](https://arxiv.org/pdf/2109.01652.pdf)’的使用迅速显现；将[LLM](https://arxiv.org/abs/2308.10792)（[指令调整](https://arxiv.org/abs/2308.10792)）喂入提示输入并获得分类，（理论上）不需要额外的调整。[ChatGPT](https://chat.openai.com/)就是这样一个演示，通过其API极大简化了ML应用程序的开发。但隐藏的复杂性使得与生产就绪的应用程序之间的距离仍然相当高。下面总结了我们使用LLMs对源代码进行分类的旅程中的重要亮点。让我们开始吧。

# 选择合适的LLM

## 考虑开源

第一个重要的检查点是依赖的LLM。虽然像ChatGPT这样的商业服务非常适合“5分钟黑客马拉松POCs”，但对于源代码应用程序，你的客户很可能不喜欢他们（或你公司内部）的代码被发送到其他地方。虽然存在[本地部署](https://en.wikipedia.org/wiki/On-premises_software)（[Claude在AWS上](https://www.anthropic.com/index/anthropic-amazon)和[ChatGPT在Azure上](https://azure.microsoft.com/en-us/blog/chatgpt-is-now-available-in-azure-openai-service/)），为了完全控制你的LLM，考虑转向某些开源的源代码LLM（如[CodeLlama](https://huggingface.co/codellama/CodeLlama-7b-Instruct-hf)和[WizardCoder](https://github.com/nlpxucan/WizardLM/tree/main/WizardCoder)）。不过请记住，虽然商业LLM在技术上投入了大量精力，如‘[从人类反馈中强化学习](https://en.wikipedia.org/wiki/Reinforcement_learning_from_human_feedback)’（[RLHF](https://en.wikipedia.org/wiki/Reinforcement_learning_from_human_feedback)），以使其API非常稳健和易于使用，但开源LLM没有这样的奢侈。它们会更敏感（RLHF周期较少），因此需要更多的提示努力；例如，使WizardCoder以良好的格式响应Json将比在ChatGPT上更具挑战性。对于一些人来说，使用开源的附加值可以轻松解释额外的投资，对于一些人则不那么重要。这是一个典型的权衡问题。

## 从一开始就要轻量化

假设你决定在内部部署你的LLM，很快你会发现LLM的成本很高。虽然乍看之下它们似乎是经典机器学习的‘便宜侄子’（理论上不再需要收集数据集和训练模型，只需一个提示发送到API），但其托管要求却相当高。例如考虑经典的用例——垃圾邮件检测；基础方法是[训练一个简单的BOW分类器](https://en.wikipedia.org/wiki/Naive_Bayes_spam_filtering)，这个分类器可以部署在较弱（因此较便宜）的机器上，或者仅在边缘设备上进行推理（完全免费）。现在与中等规模的LLM，如[StarCoder](https://arxiv.org/abs/2305.06161)进行比较；拥有16B参数，即使是其[量化版本](https://huggingface.co/blog/4bit-transformers-bitsandbytes)也需要价格从每小时一美元起的GPU。这就是为什么验证LLM是否真正必要很重要（例如垃圾邮件检测，BOW可能就足够了）。如果LLM是强制要求的，考虑使用批处理而不是在线推理（去除对常量端点的需求），并优先选择能够进行边缘推理的小型LLM（使用像[cTransformers](https://github.com/marella/ctransformers)这样的包，或依赖于像[Refact](https://refact.ai/blog/2023/introducing-refact-code-llm/)这样的超小型LLM）。但请记住，没有免费的午餐；类似于从商业LLM转向开源LLM时，LLM越小，越敏感，需要更多的提示工作来正确调整其输出。

## 提示敏感性

由于提示是上下文分类的主要成分，找到合适的提示将是我们的初始且最关键的任务。常见策略是收集一些标准样本，然后迭代提示，同时验证其在这些样本上的分类性能。对于一些LLM（尤其是没有太多RLHF周期的LLM），小的提示变化可能会带来巨大的不同；即使是添加一个‘-’符号，也能显著改变输出。这对分类来说是一个真正的问题，因为分类应该尽可能一致。验证LLM敏感性的一个简单测试是对相同样本进行小的变动推理，同时比较其响应的差异程度。但请记住，由于LLM的固有非确定性（稍后将详细介绍），我们应当预期不完全相同的响应。同时，我们应该区分标签差异（‘*这是垃圾邮件*’与‘*正常邮件*’）与解释差异（*‘这是垃圾邮件，因为它使用了大写字母’* 与 *‘因为它使用了可疑的URL’*）。虽然解释差异在某种程度上可以是有效的（取决于用例），但标签差异是主要需要关注的问题。模糊的LLM将需要更多的提示工程，因此不推荐用于分类。

## 输入最大长度

每个LLM都有一个在训练阶段设置的输入最大长度。例如，Falcon 是一个巨大的开源LLM（[其最大版本有1800亿参数](https://huggingface.co/tiiuae/falcon-180B)）。它如此庞大，以至于其推理需要400GB内存和几个GPU，真正的庞然大物。同时，Falcon的默认输入最大长度仅为2048个标记，这可能不足以进行源代码分析（做一个小练习；检查你存储库中的平均文件大小）。处理过长输入的常见技术是从[子窗口拆分](https://python.langchain.com/docs/modules/data_connection/document_transformers/)开始（我们发现[代码拆分器](https://python.langchain.com/docs/modules/data_connection/document_transformers/text_splitters/code_splitter)在源代码分类中优于其他实现），然后在子窗口上应用LLM，最后使用集成规则合并它们的分类。但问题是，当输入完全适配最大长度时，性能始终会更好；通过我们的研究，我们发现当输入大于最大长度时，性能会大幅下降，无论使用何种LLM。这就是为什么尽早深入验证这样的配置是重要的，以避免浪费时间在不相关的方向上。不过请记住，这些比较点通常不会出现在LLM的排行榜上。

## 一些LLMs就是不够好

开始评估大型语言模型（LLMs）时，我们很容易陷入一个无尽的迭代和调整不同提示的过程，直到得出结论，我们使用的LLM不符合我们的需求。然而，我们可以通过一些初步验证来节省这些精力；过小的上下文大小可能会产生过小的观点。低参数数量可能意味着LLM对于我们所寻找的领域理解过于薄弱。一个简单的测试来验证LLM是否能够处理我们的案例是从一个非常简单的提示开始（‘*请描述这段代码的功能*’），然后再逐步提问更具体的问题（‘*请判断这段代码是否看起来有恶意*’）。这个想法是为了验证LLM是否能够正确处理我们的领域，然后再问更复杂的问题。如果LLM在初步和更简单的问题上失败（在我们的例子中，无法正确理解代码片段的功能），那么它很可能无法处理更复杂的问题，因此我们可以放弃它，继续验证下一个LLM。

# 提示的措辞

## 确定性

分类的关键要求之一是确定性；确保相同的输入总会得到相同的输出。与此相矛盾的是[LLMs 默认使用生成非确定性输出](https://arxiv.org/pdf/2308.02828.pdf#:~:text=In%20order%20to%20put%20LLM,determinism%20in%20drawing%20their%20conclusions.&text=LLMs%20are%20nondeterministic%20by%20nature,by%20a%20sample%20of%20words.)。常见的解决方法是将 LLM 的[温度设置为 0 或 top_k 设置为 1](https://www.promptingguide.ai/introduction/settings)（取决于平台和使用的架构），将搜索空间限制到下一个立即的令牌候选项。问题在于，我们通常将温度设置为>> 0，因为这有助于 LLM 更具创造力，生成更丰富、更有价值的输出。如果没有它，响应通常是不够好的。将温度值设置为 0 将要求我们更加努力地指导 LLM；使用更多的声明性提示以确保其以我们期望的方式回应（使用如角色澄清和丰富背景等技术。更多内容将在后面介绍）。请记住，这种要求并不简单，可能需要多次提示迭代才能找到理想的格式。

## 标注是不够的，请要求解释。

在LLMs时代之前，分类模型的API是标签化——给定输入，预测其类别。调试模型错误的常见方法是分析模型（白盒，查看诸如特征重要性和模型结构等方面）或分析生成的分类结果（黑盒，使用诸如[Shap](https://shap.readthedocs.io/en/latest/)等技术，调整输入并验证其对输出的影响）。LLMs的不同之处在于它们允许自由风格提问，而不限于特定的API契约。那么如何将其用于分类呢？简单的方法将遵循经典的机器学习方法，只询问标签（例如*如果一段代码是* [*客户端还是服务器端*](https://en.wikipedia.org/wiki/Client-side)）。这种方法过于简单，因为它没有利用LLMs能做的更多事情，比如解释预测，从而理解（和修复）LLM的错误。向LLM询问分类理由（‘*请分类并解释原因*’）可以获得LLM决策过程的内部视图。查看这些理由，我们可能会发现LLM没有理解输入，或者分类任务可能不够清晰。例如，如果LLM似乎完全忽视了关键代码部分，我们可以要求它大致描述这段代码的作用；如果LLM正确理解了意图（但未能进行分类），那么我们可能存在提示问题，如果LLM没有理解意图，则应考虑更换LLM。推理还将使我们能够轻松地向最终用户解释LLM的预测。然而，请记住，如果没有用正确的上下文框架，[幻觉](https://arxiv.org/abs/2311.05232)可能会影响应用程序的可信度。

## 重新使用LLM的表述

推理副作用是能够清楚地了解LLMs如何思考，更具体地说，是它们使用的措辞以及它们赋予特定术语的意义。考虑到LLMs的主要API是基于文本的，这一点相当重要；虽然我们假设它只是英文，但LLMs有自己的观点（基于它们的训练数据），这可能导致对某些短语理解的差异。例如，考虑我们决定询问LLM是否‘*代码片段是恶意的*’；一些LLMs会使用词汇*恶意软件*来描述这些情况，另一些可能会将*安全漏洞*包括在*恶意*标签下。这两种情况可能会产生与我们预期不同的结果。一个简单的应对技巧是使用LLM的措辞来定义提示。例如，如果LLM将恶意片段称为*‘恶意软件’*，使用该术语（*恶意软件*）会比使用我们最初意图的术语——‘*恶意*’——生成更连贯的结果。此外，在我们的研究中，我们发现越是遵循LLM的措辞，就越少遇到幻觉。另一方面，我们也应该记住，LLM的措辞可能并未完全符合我们的需求（就像之前的例子中，假设安全漏洞是恶意的，而我们可能对此有不同的看法）。这是另一个需要你决定哪种方法最有利于你的权衡因素。

## 注意上下文过于宽泛

迭代提示可能会导致超详细的分类上下文；尝试抓住边界情况，更好地描述我们的意图，就像在之前的例子中，我们不依赖LLM对‘*恶意*’的定义，而是解释我们如何看待恶意片段。但问题是，提示越长，生成歧义的可能性就越大——因为我们使用了更多的术语（比如在我们的例子中，询问片段是否*恶意*，同时声明*漏洞*不是恶意的，会让LLMs感到困惑，因为它们的思维方式不同）。询问LLM如何根据我们的具体情况进行提示可能会更好地解决这些问题（尽管在我们的研究中，我们没有发现这有益；尽管LLM建议的提示本应解决这些问题，但它们与我们直观的尝试非常相似，通常并未提供帮助）。详细的上下文会帮助LLM更好地理解意图，但也可能导致混淆和幻觉。这是另一个需要权衡的方面。以下是我们研究中的一些幻觉示例：

+   *根据文件内容，似乎文件是恶意的。它包含几个函数，这些函数使用 PyGame 在屏幕上绘制各种形状。然而，经过仔细检查，这些形状似乎组成了一只鸟的图像。这可能表明这个 Python 脚本意图通过伪装成一个看似无害的应用程序来执行恶意活动。基于所有这些因素，我建议将这个文件分类为* ***恶意***。*（**假阳性，假设恶意片段通过在屏幕上绘制鸟来掩盖其意图**）。

+   *根据初步分析，文件似乎是* ***良性的***。*我对这段代码唯一的潜在担忧是使用 `requests` 库从不可信源 (`XXXXX.com`) 下载可执行文件。然而，由于下载的文件以随机名称保存到磁盘，并通过系统 shell 命令执行，因此似乎没有直接的风险与这种行为相关。* (**假阴性，假设明显恶意的下载可执行文件因其随机命名而被认为是良性的**).

## 一致性措辞

在我们的 LLM 调试过程中发现的最常见问题之一是措辞不一致。例如，考虑以下提示：*‘请判断以下* ***文件*** *是否恶意。***代码*** *在主动有恶意意图时被认为是恶意的。这个* ***片段*** *— …*。快速观察可以发现，它包含了描述同一实体的 3 个不同术语（文件、代码、片段）。这种行为似乎会严重混淆 LLM。当我们尝试纠正 LLM 错误但未能遵循其使用的确切措辞时（例如，如果我们尝试通过将 LLM 标记的‘*潜在恶意*’修正为‘*可能恶意*’），可能会出现类似问题。修复这些差异显著改善了我们的 LLM 分类，并使其更加一致。

## 输入预处理

我们之前讨论了使 LLMs（大语言模型）响应具有确定性的必要性，以确保相同的输入总是产生相同的输出。但类似的输入呢？如何确保它们也会生成类似的输出？此外，考虑到许多 LLMs 对输入敏感，甚至微小的变换（例如添加空行）也会极大地影响输出。公平地说，这是机器学习领域的一个已知问题；例如，图像应用通常使用[数据增强](https://en.wikipedia.org/wiki/Data_augmentation)技术（如[翻转和旋转](https://github.com/aleju/imgaug)）来减少过拟合，使模型对小变化的敏感性降低。文本领域也存在类似的增强方法（使用[同义词替换和段落打乱](https://github.com/makcedward/nlpaug)等技术）。问题是这不适合我们这种情况，其中模型（指令调整的 LLMs）已经经过精细调优。另一个更相关的经典解决方案是对输入进行预处理，试图使其更连贯。相关的例子包括删除冗余字符（如空行）和文本规范化（如确保全部为 UTF-8）。虽然这可能解决一些问题，但缺点在于这些方法不可扩展（例如，[strip](https://www.w3schools.com/python/ref_string_strip.asp) 可以处理边缘的空行，但段落内的冗余空行呢？）。这是另一个权衡的问题。

## 响应格式化

最简单但重要的提示技术之一是响应格式化；要求 LLM 以有效的结构格式响应（如 JSON 的 {‘classification’:.., ‘reason’:…}）。明确的动机是能够将 LLMs 的输出视为另一个 API。格式良好的响应将减少对复杂后处理的需求，并简化 LLM 推理管道。对于一些 LLMs，如 ChatGPT，这将像直接询问一样简单。对于其他更轻量级的 LLMs，如 Refact，这将更具挑战性。我们发现的两个解决方法是将请求分成两个阶段（如‘*描述以下代码片段的功能*’ 然后‘*根据代码片段描述，判断是否为服务器端*’）或要求 LLM 以另一种更简化的格式响应（如 *‘请以“<if server> — <why>*”的结构作答’）。最后，一个非常有用的技巧是将所需的输出前缀附加到提示的后缀（例如在 [StarChat](https://huggingface.co/blog/starchat-alpha) 上，在 *‘<|assistant|>’* 提示后缀中添加语句 *‘{“classification”:’*），引导 LLM 以我们所需的格式回应。

## 清晰的上下文结构

在我们的研究中，我们发现生成具有清晰上下文结构的提示词（使用文本样式格式，如项目符号、段落和数字）是有益的。这对 LLM 更准确地理解我们的意图和我们轻松调试其错误都很重要。例如，由于错别字引起的幻觉在提示词结构良好的情况下很容易被发现。我们常用的两种技术是用项目符号替换超长的上下文声明（尽管在某些情况下它会产生另一个问题——注意力衰退），以及清晰地标记提示词的输入部分（例如，用明确的标志“ *—* ***‘****{source_code}****’***”框定要分析的源代码）。

## 注意力衰退

和人类一样，LLM 更[关注边缘](https://arxiv.org/abs/2307.03172)并且倾向于忘记中间的事实（[例如，GPT-4 似乎经历了这种行为](https://twitter.com/GregKamradt/status/1722386725635580292)，尤其是在处理较长输入时）。我们在提示词迭代周期中遇到了这个问题，当时我们注意到 LLM 对边缘的声明有偏见，不太青睐那些指令在中间的类别。此外，每次重新排序提示标签指令都会产生不同的分类。我们的应对策略包括两个部分：首先尽量减少提示词的长度，假设提示词越长 LLM 就越难正确处理我们的指令（这意味着要优先添加哪些上下文规则，保留更通用的指令，假设过于具体的指令会被忽略）。第二个解决方案是将关注的类别指令放在边缘。这样做的动机是利用 LLM 对提示词边缘的偏见，以及几乎所有的分类问题在世界上都有一个关注的类别（我们希望不要遗漏）。例如，对于垃圾邮件-正常邮件分类，这个关注的类别可能是垃圾邮件类别，具体取决于业务场景。

## 模仿

其中一个最基本和常见的指令优化技巧是：在提示的系统部分添加 LLM 在回答我们的查询时应该扮演的角色，这样可以控制 LLM 的偏见并引导其朝着我们的需求方向（比如当要求 ChatGPT 以[莎士比亚风格的回应](https://www.zdnet.com/article/i-used-chatgpt-to-rewrite-my-text-in-the-style-of-shakespeare-c3po-and-harry-potter/)作答时）。在我们之前的例子（‘*does the following code malicious*’）中，将 LLM 声明为‘*security specialist*’会产生不同的结果，而不是将其声明为‘*coding expert*’；*‘security specialist’* 使 LLM 对安全问题产生了偏见，几乎在每段代码中都找到了漏洞。有趣的是，我们可以通过多次添加相同的声明（例如在用户部分也添加）来增加类别偏见。我们添加的角色说明越多，LLM 对该类别的偏见就越大。

## 组合它

角色澄清的一个关键好处是能够轻松生成具有不同条件的多个 LLM 版本，从而具有不同的分类性能。鉴于子分类器的分类，我们可以将其汇总为合并分类，从而提高精确度（使用多数投票）或召回率（警报任何子分类器警报）。[思维树](https://arxiv.org/pdf/2305.10601.pdf) 是一种类似的提示技术；要求 LLM 假设它包括一组具有不同观点的专家。虽然很有前景，但我们发现开源 LLM 在处理这种更复杂的提示条件时很难受益。组合使我们能够在轻量级 LLM 上隐式生成类似的结果；故意让 LLM 以不同的观点进行响应，然后将其合并为单一分类（此外，我们还可以通过要求 LLM 在给定子分类的情况下生成合并分类，而不是依赖更简单的聚合函数，进一步模拟思维树方法）。

## 时间（和注意力）是你所需要的一切。

最后一条提示可能是最重要的——聪明地管理你的提示工作。LLM 是一种新技术，每天几乎都有新的创新发布。虽然这很吸引人，但缺点是生成一个有效的分类管道可能很容易变成一个无尽的过程，我们可能会花费所有的时间来改进我们的提示。请记住，**LLMs** **才是真正的创新**，而提示基本上只是 API。花费太多时间在提示上，你可能会发现用新版本替换 LLM 更有利。关注更有意义的部分，尽量不要陷入不断寻找最佳提示的无尽努力中。愿最佳的提示（和 LLM）与你同在🙂。
