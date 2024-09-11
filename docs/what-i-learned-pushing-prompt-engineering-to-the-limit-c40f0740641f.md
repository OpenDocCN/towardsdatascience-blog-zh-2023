# 我在推动提示工程极限时的所学

> 原文：[`towardsdatascience.com/what-i-learned-pushing-prompt-engineering-to-the-limit-c40f0740641f?source=collection_archive---------0-----------------------#2023-06-12`](https://towardsdatascience.com/what-i-learned-pushing-prompt-engineering-to-the-limit-c40f0740641f?source=collection_archive---------0-----------------------#2023-06-12)

[](https://medium.com/@jacob_marks?source=post_page-----c40f0740641f--------------------------------)![Jacob Marks, Ph.D.](https://medium.com/@jacob_marks?source=post_page-----c40f0740641f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c40f0740641f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c40f0740641f--------------------------------) [Jacob Marks, Ph.D.](https://medium.com/@jacob_marks?source=post_page-----c40f0740641f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff7dc0c0eae92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-i-learned-pushing-prompt-engineering-to-the-limit-c40f0740641f&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=post_page-f7dc0c0eae92----c40f0740641f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c40f0740641f--------------------------------) ·10 分钟阅读·2023 年 6 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc40f0740641f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-i-learned-pushing-prompt-engineering-to-the-limit-c40f0740641f&user=Jacob+Marks%2C+Ph.D.&userId=f7dc0c0eae92&source=-----c40f0740641f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc40f0740641f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-i-learned-pushing-prompt-engineering-to-the-limit-c40f0740641f&source=-----c40f0740641f---------------------bookmark_footer-----------)![](img/ad4b84de610367e97b341392a5b74e5e.png)

对提示工程的讽刺描绘。具有讽刺意味的是，DALL-E2 生成的图像是作者使用提示工程生成的，提示为“一个疯狂的科学家把卷轴递给一个人工智能机器人，以复古风格生成”，加上一些变化，以及扩展画布。

我花了过去两个月时间构建一个大型语言模型（LLM）驱动的应用程序。这是一次令人兴奋、智力激发且有时令人沮丧的经历。整个项目过程中，我对提示工程的理解——以及 LLM 的可能性——发生了变化。

我很乐意与你分享一些我最大的收获，旨在揭示一些常被忽视的提示工程方面。我希望在阅读了我的经历后，你能做出更明智的提示工程决策。如果你已经尝试过提示工程，我希望这能帮助你在自己的旅程中更进一步！

为了提供背景，这里是我们将要学习的项目的 TL;DR：

+   我和我的团队构建了 [VoxelGPT](https://github.com/voxel51/voxelgpt/tree/main)，这是一个将 LLM 与 [FiftyOne](https://github.com/voxel51/fiftyone) 计算机视觉查询语言结合的应用，旨在通过自然语言在图像和视频数据集中进行搜索。VoxelGPT 还回答有关 FiftyOne 本身的问题。

+   VoxelGPT 是开源的（FiftyOne 也是！）。所有代码都可以在 [GitHub](https://github.com/voxel51/voxelgpt) 上找到。

+   你可以在 gpt.fiftyone.ai 免费试用 VoxelGPT。

+   如果你对我们如何构建 VoxelGPT 感到好奇，可以在 [TDS 上阅读更多](https://medium.com/towards-data-science/how-i-turned-chatgpt-into-an-sql-like-translator-for-image-and-video-datasets-7b22b318400a)。

现在，我已经将提示工程课程分为四个类别：

1.  一般经验

1.  提示技术

1.  示例

1.  工具

# 一般经验

## 科学？工程？黑魔法？

提示工程既是实验也是工程。从你提问的具体措辞，到你输入的上下文的内容和格式，有无穷无尽的提示写作方式。这可能会让人不知所措。我发现最简单的方法是从简单开始，建立直觉——然后测试假设。

在计算机视觉中，每个数据集都有自己独特的模式、标签类型和类别名称。VoxelGPT 的目标是能够处理 *任何* 计算机视觉数据集，但我们一开始只用了一个数据集：MS COCO。保持所有额外自由度固定使我们能够集中精力验证 LLM 在写出语法正确的查询方面的能力。

一旦你确定了在有限上下文中成功的公式，就可以找出如何将其概括和扩展。

## 使用哪个模型？

人们说，大型语言模型最重要的特征之一是它们是相对可互换的。理论上，你应该能够在不显著改变连接结构的情况下，替换一个 LLM 为另一个。

虽然改变你使用的 LLM 通常像更换 API 调用一样简单，但在实际操作中确实会遇到一些困难。

+   一些模型的上下文长度比其他模型短得多。切换到上下文较短的模型可能需要重大重构。

+   开源是很棒的，但开源 LLM 的性能（目前）还不如 GPT 模型。而且，如果你要部署一个使用开源 LLM 的应用，你需要确保运行模型的容器有足够的内存和存储。这可能比直接使用 API 端点更麻烦（也更昂贵）。

+   如果你开始使用 GPT-4 然后因为成本问题切换到 GPT-3.5，你可能会对性能的下降感到震惊。对于复杂的代码生成和推理任务，GPT-4 要好得多。

## 何时使用 LLM？

大型语言模型是强大的。但仅仅因为它们可能能够完成某些任务，并不意味着你需要——甚至应该——将它们用于这些任务。思考 LLM 的最佳方式是将其视为*赋能工具*。LLM 不是解决方案的全部：它们只是其中的一部分。不要指望大型语言模型能做所有事情。

举个例子，可能你使用的 LLM 可以（在理想情况下）生成格式正确的 API 调用。但如果你知道 API 调用的结构应是什么样的，并且实际上对填充 API 调用的部分（变量名、条件等）感兴趣，那么只需使用 LLM 来完成这些任务，然后自己使用（经过适当后处理的）LLM 输出生成结构化的 API 调用。这将更便宜、更高效，也更可靠。

拥有 LLM 的完整系统肯定会有大量的连接部分和经典逻辑，以及一系列传统的软件工程和 ML 工程组件。找到最适合你应用的解决方案。

## LLMs 有偏见

语言模型既是推理引擎也是知识存储库。通常，LLM 的知识存储库方面对用户非常有吸引力——许多人将 LLM 用作搜索引擎的替代品！到现在，任何使用过 LLM 的人都知道它们容易编造虚假的“事实”——这现象被称为*幻觉*。

然而，有时 LLM 遇到的是相反的问题：它们过于固执于其训练数据中的事实。

在我们的案例中，我们尝试促使 GPT-3.5 确定将用户的自然语言查询转换为有效的 FiftyOne Python 查询所需的适当 ViewStages（逻辑操作的管道）。问题在于 GPT-3.5 了解 `Match` 和 `FilterLabels` ViewStages，这些在 FiftyOne 中存在了一段时间，但其训练数据不包括最近添加的功能，其中 `SortBySimilarity` ViewStage 可以用来找到与文本提示相似的图像。

我们尝试传入 `SortBySimilarity` 的定义、使用细节和示例。我们甚至尝试指示 GPT-3.5 绝对不能使用 `Match` 或 `FilterLabels` ViewStages，否则会受到惩罚。不管我们尝试什么，LLM 仍然倾向于其*已知的*内容，无论是否是正确的选择。我们在与 LLM 的本能作斗争！

我们最终不得不在后处理中处理这个问题。

## 痛苦的后处理是不可避免的

无论你的示例有多好；无论你的提示有多严格——大型语言模型总会产生幻觉，给出格式错误的响应，当它们无法理解输入信息时，还会发脾气。LLM 最可预测的特性就是其输出的不可预测性。

我花费了大量的时间编写例程来匹配模式并纠正幻觉语法。最终，[后处理文件](https://github.com/voxel51/voxelgpt/blob/main/links/dataset_view_generator.py)包含了近 1600 行 Python 代码！

这些子例程中有些非常简单，比如添加括号，或在逻辑表达式中将“and”和“or”更改为“&”和“|”。有些子例程则复杂得多，比如验证 LLM 响应中的实体名称，如果满足某些条件，将一个 ViewStage 转换为另一个，确保方法的参数数量和类型有效。

如果你在一个相对封闭的代码生成环境中使用提示工程，我推荐以下方法：

1.  使用抽象语法树（Python 的[ast](https://docs.python.org/3/library/ast.html)模块）编写你自己的自定义错误解析器。

1.  如果结果在语法上无效，将生成的错误消息输入到 LLM 中，让它再试一次。

这种方法未能解决更隐蔽的情况，即语法有效但结果不正确。如果有人对这个问题有好的建议（超出 AutoGPT 和“展示你的工作”风格的方法），请告诉我！

# 提示技术

## 人越多越好

为了构建 VoxelGPT，我使用了几乎所有的提示技术：

+   “你是一个专家”

+   “你的任务是”

+   “你必须”

+   “你将会受到惩罚”

+   “以下是规则”

任何这种短语的组合都无法*确保*某种行为。巧妙的提示是不够的。

话虽如此，你在提示中使用的这些技术越多，就越能将大型语言模型（LLM）引导到正确的方向！

## 示例 > 文档

现在已经是常识（也是常识！），示例和其他上下文信息如文档可以帮助引导大型语言模型生成更好的响应。我发现 VoxelGPT 确实是这样。

一旦你添加了所有直接相关的示例和文档，如果上下文窗口中还有额外的空间，你应该怎么做？根据我的经验，我发现间接相关的示例比间接相关的文档更重要。

## 模块化 >> 大一统

你将一个总体问题拆解成更小的子问题的程度越高，效果越好。与其提供数据集模式和端到端示例列表，不如识别单独的选择和推理步骤（选择-推理提示），并在每个步骤中仅提供相关信息。

这有三个优点：

1.  LLM 在一次处理一个任务时表现得比同时处理多个任务更好。

1.  步骤越小，清理输入和输出就越容易。

1.  对你作为工程师来说，理解应用逻辑是一个重要的练习。LLM 的目的不是让世界变成黑匣子，而是启用新的工作流程。

# 例子

## 我需要多少个例子？

提示工程的一个重要部分是确定你需要多少个例子来完成特定任务。这是*高度*依赖于问题的。

对于一些任务（[有效查询生成](https://github.com/voxel51/voxelgpt/blob/main/prompts/effective_prompt_generator_prefix.txt) 和 [根据 FiftyOne 文档回答问题](https://github.com/voxel51/voxelgpt/blob/main/prompts/docs_qa_template.txt)），我们能够做到没有*任何*例子。而对于其他任务（[标签选择](https://github.com/voxel51/voxelgpt/blob/main/examples/tag_selection_examples.csv)，[聊天记录是否相关](https://github.com/voxel51/voxelgpt/blob/main/examples/history_relevance_examples.csv)和 [标签类命名实体识别](https://github.com/voxel51/voxelgpt/blob/main/examples/label_class_examples.csv)），我们只需要几个例子就能完成工作。然而，我们的主要推理任务几乎有 400 个例子（这仍然是整体性能的限制因素），因此我们仅在推理时传入最相关的例子。

当你生成例子时，尝试遵循两个指南：

1.  尽可能全面。如果你有一个有限的可能性空间，那么尝试为每种情况提供至少一个例子。对于 VoxelGPT，我们至少尝试为每种语法正确的 ViewStage 使用方式提供一个例子——通常每种方式会有几个例子，以便 LLM 进行模式匹配。

1.  尽可能保持一致。如果你将任务拆分为多个子任务，确保例子在一个任务与下一个任务之间保持一致。你可以重用例子！

## 合成例子

生成例子是一个繁琐的过程，手工制作的例子只能做到有限程度。事先想到每种可能的情境几乎是不可能的。当你部署应用时，可以记录用户查询，并利用这些查询来改进你的例子集。

然而，在部署之前，你最好的选择可能是生成*合成*例子。

这里有两种生成合成例子的方式，你可能会发现有用：

1.  使用 LLM 生成例子。你可以让 LLM 变换语言，甚至模仿潜在用户的风格！虽然这对我们没用，但我相信它对许多应用可能有效。

1.  基于输入查询中的元素程序性地生成示例——可能带有随机性。对于 VoxelGPT，这意味着基于用户数据集中的字段生成示例。我们正在将其纳入我们的管道中，目前为止看到的结果很有希望。

# 工具

## LangChain

LangChain 之所以受欢迎是有原因的：这个库使得以复杂的方式连接 LLM 的输入和输出变得简单，抽象掉了繁琐的细节。特别是模型和提示模块表现出色。

话虽如此，LangChain 绝对是一个不断进步的项目：他们的记忆、索引和链模块都有显著的限制。这只是我在尝试使用 LangChain 时遇到的一些问题。

1.  文档加载器和文本拆分器：在 LangChain 中，[文档加载器](https://python.langchain.com/en/latest/modules/indexes/document_loaders.html) 应将不同文件格式的数据转换为文本，而 [文本拆分器](https://python.langchain.com/en/latest/modules/indexes/text_splitters.html) 则应将文本拆分成语义上有意义的块。VoxelGPT 通过检索文档中最相关的块并将其传入提示中来回答有关 FiftyOne 文档的问题。为了生成有意义的答案，我不得不有效地构建自定义加载器和拆分器，因为 LangChain 没有提供适当的灵活性。

1.  向量存储：LangChain 提供了 [Vectorstore](https://python.langchain.com/en/latest/modules/indexes/vectorstores.html) 集成和基于 Vectorstore 的 [检索器](https://python.langchain.com/en/latest/modules/indexes/retrievers.html)，以帮助找到相关信息并纳入 LLM 提示中。这在理论上很棒，但实现缺乏灵活性。我不得不使用 ChromaDB 编写自定义实现，以便预先传递嵌入向量，而不是每次运行应用程序时都重新计算它们。我还不得不编写自定义检索器，以实现所需的自定义预筛选。

1.  带来源的问答：在构建对 FiftyOne 文档的问答时，我找到了一种合理的解决方案，利用了 LangChain 的 `RetrievalQA` Chain。当我想添加来源时，我以为只需将该链替换为 LangChain 的 `RetrievalQAWithSourcesChain` 就可以了。然而，不良的提示技术导致该链表现出一些不幸的行为，例如 [关于迈克尔·杰克逊的虚假信息](https://github.com/hwchase17/langchain/issues/2510)。我再次不得不 [自己动手](https://github.com/voxel51/voxelgpt/blob/main/links/utils.py)。

这意味着什么呢？也许自己构建这些组件更简单！

## 向量数据库

向量搜索可能正处于 🔥🔥🔥 的热潮中，但这并不意味着你的项目必须使用它。我最初使用 ChromaDB 实现了类似的示例检索程序，但由于我们只有几百个示例，最终我转向了精确的最近邻搜索。我确实需要自己处理所有的元数据过滤，但结果是一个更快的程序，依赖性更少。

## TikToken

将 [TikToken](https://github.com/openai/tiktoken) 纳入计算是非常简单的。总的来说，TikToken 给项目增加了不到 10 行代码，但使我们在计数令牌和尝试将尽可能多的信息融入上下文长度时变得更加精确。这在工具选择上是唯一的明智选择。

# 结论

选择的 LLM（大语言模型）有很多，各种新工具琳琅满目，还有一堆“提示工程”技巧。这一切既令人兴奋又可能让人感到不知所措。使用提示工程构建应用程序的关键在于：

1.  将问题分解；构建解决方案

1.  将 LLM 视为 *赋能工具*，而不是端到端的解决方案

1.  仅在工具能让你的生活更轻松时使用它们

1.  拥抱实验精神！

去创造一些很酷的东西吧！
