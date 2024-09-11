# 召唤所有函数

> 原文：[https://towardsdatascience.com/calling-all-functions-a421434ae0a4?source=collection_archive---------7-----------------------#2023-12-07](https://towardsdatascience.com/calling-all-functions-a421434ae0a4?source=collection_archive---------7-----------------------#2023-12-07)

![](../Images/eba8ad88a5fb2ef4e5bb4bfbe563ef50.png)

图片由作者使用 Dall-E 创建

## 基准测试 OpenAI 的函数调用与解释

[](https://medium.com/@amber.roberts?source=post_page-----a421434ae0a4--------------------------------)[![Amber Roberts](../Images/ee686891eeedca7f33a63e147cc2c086.png)](https://medium.com/@amber.roberts?source=post_page-----a421434ae0a4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a421434ae0a4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a421434ae0a4--------------------------------) [Amber Roberts](https://medium.com/@amber.roberts?source=post_page-----a421434ae0a4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffef13eb0f830&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcalling-all-functions-a421434ae0a4&user=Amber+Roberts&userId=fef13eb0f830&source=post_page-fef13eb0f830----a421434ae0a4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a421434ae0a4--------------------------------) ·9 分钟阅读·2023年12月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa421434ae0a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcalling-all-functions-a421434ae0a4&user=Amber+Roberts&userId=fef13eb0f830&source=-----a421434ae0a4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa421434ae0a4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcalling-all-functions-a421434ae0a4&source=-----a421434ae0a4---------------------bookmark_footer-----------)

*感谢* [*Roger Yang*](https://www.linkedin.com/in/roger-y-a35595114/) *对这篇文章的贡献*

对第三方大型语言模型（LLMs）的可观察性主要通过基准测试和评估来实现，因为像Anthropic的Claude、OpenAI的GPT模型和Google的PaLM 2这样的模型是专有的。在这篇博客文章中，我们将OpenAI的GPT模型的函数调用和解释与各种性能指标进行基准测试。我们特别关注GPT模型和OpenAI功能在正确分类幻觉和相关响应方面的表现。下面的结果展示了不同LLM应用系统在速度和性能之间的权衡，以及如何利用这些带有解释的结果进行数据标注、LLM辅助评估和质量检查。我们使用的实验框架也在下面提供，以便从业者可以在默认分类模板上进行迭代和改进。

# OpenAI函数调用和解释

使用OpenAI函数调用，你可以描述函数给各种GPT模型，这些函数可以用来输出包含调用这些函数的参数的JSON对象。函数调用本质上作为一种工具或代理，用于以给定格式记录响应，以可靠地将OpenAI GPT模型的能力与外部工具和API连接。关于解释，由于许多情况下很难理解LLM为何以特定方式响应，这些解释旨在促使LLM解释输出是否正确。然后，你可以附上一个输出标签（‘相关’或‘无关’）和LLM的解释。下面是一个带有解释的‘相关’评估示例，用于调试LLM响应。

![](../Images/16b573487f1f3fa16913ea72627dd754.png)

图片由作者提供

在下面的“结果与权衡”部分，提供了两个用例的比较表格：预测相关性和预测幻觉（分别为表1和表2）。每个表格比较了GPT-4-turbo、GPT-4、GPT-3.5和GPT-3.5-turbo在特定提示指令和LLM属性下的各种分类指标（准确性、精确度、召回率和[F1分数](https://arize.com/blog-course/f1-score/)）的表现。

测试的四个基础模型在这些情况下使用上述指标（除了中位处理时间）进行评估。

+   无_function_calling & 无_explanations

+   有_function_calling & 无_explanations

+   有_function_calling & 有_explanations

+   无_function_calling & 有_explanations

因此，LLM中的提示模板保持不变（即普通的提示完成），例如**without_function_calling & without_explanations**。具有功能调用但无解释的样本要求LLM将其答案放在仅接受枚举作为输入的JSON对象中，而具有功能调用和解释的样本要求LLM在同一个JSON对象中提供解释（详见[Google colab notebook](https://colab.research.google.com/github/Arize-ai/phoenix/blob/benchmarking-function-calling-and-explanations/tutorials/internal/benchmarking_function_calling_and_explanations.ipynb))。

# 基准数据集和评估指标

正确识别LLM输出中的幻觉和相关响应是我们客户当前实施LLM应用中最大的两个痛点。[优化LLM评估系统](https://arize.com/blog-course/llm-evaluation-the-definitive-guide/)对于幻觉意味着正确识别所有虚构响应，同时跟踪事实输出。对于利用搜索和推荐作为其用户体验的一部分的用例，与用户满意度相关的最重要因素是结果的速度和相关性。为了评估LLM系统在相关和无关输出上的性能，您应该了解关键指标：

+   *精确度和召回率*：检索到的信息有多相关？

+   *准确性*：回应在语境上的准确度有多高？

+   *延迟*：系统提供响应需要多长时间？

+   *用户反馈*：结果的相关性和响应时间如何影响用户体验？

# 结果与权衡

这里是关于基准测试LLM系统在无关和相关预测上的结果：

![](../Images/c960caad6033db8f8681ca5d59cfdbc6.png)

表1 作者提供

这里是基准测试LLM系统在幻觉和事实预测上的结果：

![](../Images/6c0d3c56fd9c82c9fa530037a2cfa2b7.png)

表2 作者提供

在分析结果之前，你可以在这个[Google colab notebook](https://colab.research.google.com/github/Arize-ai/phoenix/blob/benchmarking-function-calling-and-explanations/tutorials/internal/benchmarking_function_calling_and_explanations.ipynb)中自行复现这些结果。请注意，通常情况下，由于LLM的非确定性特性，你无法完全复现这些表中的数字，但对于这个notebook，我们已经添加了一个种子以保证每次采样结果都相同。此外，我们还添加了分层采样，使得二进制类别确实是完全50/50。**请注意，运行此notebook需要消耗计算资源与您的OpenAI API密钥相关。** 默认采样数量设置为2，但如果您希望复制这篇博文中的结果，可以将数字更改为100。

## 中等处理时间

为了清晰起见，这些比较（使用了100个样本）是在 Google Colab 上使用标准 OpenAI API 帐户和密钥进行的。因此，虽然在不同的设置下运行时延迟值不太可能完全准确，但最慢和最快的模型将会被重现。

此外，在评估中使用解释可能需要花费 3 到 20 倍的时间来编译（这与函数调用无关）。

关于整体相关性的模型预测能力

+   *延迟*：GPT-3.5-instruct > GPT-3.5-turbo > GPT-4-turbo > GPT-4

关于模型预测能力对幻觉的表现

+   延迟：GPT-3.5-instruct > GPT-3.5-turbo ~ GPT-4-turbo > GPT-4

带有函数调用的 GPT 模型的延迟通常略高于不带函数调用的 LLM，但请注意一些警告。首先，延迟是从 OpenAI 返回的 HTTP 头信息中提取的，因此根据你的帐户和请求方式，延迟值可能会有所变化，因为这些值是由 OpenAI 内部计算的。函数调用的权衡也取决于你的用例。例如，若没有函数调用，你需要通过提供示例和详细描述来精确指定输出的结构。然而，如果你的用例是 [结构化数据提取](https://arize.com/blog-course/structured-data-extraction-openai-function-calling/?utm_campaign=Q323+Content&utm_source=Content)，那么直接使用 OpenAI 函数调用 API 是最简单的。

总的来说，带有函数调用的 LLM 的表现与不使用函数调用而使用普通提示完成的 LLM 相当。你是否决定使用 OpenAI 函数调用 API 而不是提示工程，应该取决于你的用例和输出的复杂性。

## GPT 模型性能比较

关于整体相关性的模型预测能力：

+   性能：GPT-4 ~ GPT-4-turbo ~ GPT-3.5-turbo >>> GPT-3.5-instruct

关于模型预测能力对幻觉的表现：

+   性能：GPT-4 ~ GPT-4-turbo > GPT-3.5-turbo > GPT-3.5-instruct

有趣的是，在这两种用例中，使用解释并不总是能提高性能。更多内容见下文。

## 评估指标

如果你在决定使用哪个 LLM 来预测相关性，你可以选择 GPT-4、GPT-4-turbo 或 GPT-3.5-turbo。

GPT-4-turbo 能够准确识别输出的相关性，但在召回所有50个示例上有所牺牲，实际上，即使使用解释，其召回率也不比抛硬币好。

GPT-3.5-turbo 也面临相同的权衡，尽管它的延迟较低且准确性较低。从这些结果来看，GPT-4 拥有最高的 F1 分数（精确率和召回率的调和平均值）和总体最佳表现，同时运行时间与 GPT4-turbo 相当。

GPT-3.5-instruct 预测所有内容都是相关的，因此不是一个可行的预测相关性的 LLM。有趣的是，当使用解释时，预测性能显著提高，尽管它仍然低于其他 LLM。同时，GPT-3.5-instruct 无法使用 OpenAI 函数调用 API，并且可能会在 [2024 年初被弃用](https://platform.openai.com/docs/deprecations)。

如果你正在决定选择哪个 LLM 来预测虚假信息，你可以使用 GPT-4、GPT-4-turbo 或 GPT-3.5-turbo。

结果显示，在精确度、准确性、召回率和 F1 分数方面，GPT-4 在识别虚假和事实输出的正确率上比 GPT-4-turbo 高出约 3%。

尽管 GPT-4 和 GPT-4-turbo 的表现略高于 GPT-3.5-turbo（*请注意，得出小差距不代表噪声的结论时应使用更多样本*），如果你计划使用解释，可能值得使用 GPT-3.5-turbo。

用于预测虚假和事实的解释在 GPT-3.5-turbo 上的返回速度比 GPT-4 和 GPT-4-turbo 快三倍以上，然而，当比较 GPT-4 模型预测虚假信息的召回率时，两个 GPT-3.5 模型的召回率有所下降。

# 讨论与影响

在决定使用哪个 LLM 进行应用时，需要进行一系列实验和迭代。同样，在决定是否将 LLM 用作评估器时，也需要进行基准测试和实验。基本上，这两种方法是基准 LLM 的主要方法：LLM 模型评估（评估基础模型）和通过可观察性进行 LLM 系统评估。

![](../Images/7156337ea757257f7cd51b751dc47b39.png)

图片由作者提供 | 在单一基础模型上评估两种不同的提示模板。我们正在测试相同的数据集，并观察它们的精确度和召回率等指标如何对比。

最终，当决定一个 LLM 是否适合你的用例作为性能评估器时，你需要考虑系统的延迟以及相关预测指标的性能。在本文中，我们总结了这些模型的开箱即用表现——没有增加提高速度和性能的技术。请记住，开箱即用时，这些 LLM 都使用零样本模板，因此没有将示例添加到 LLM 提示模板中以改善模型输出。由于这些数据作为基准，团队可以通过提示工程、提示模板（和存储库）、代理、微调以及像 RAG 和 HyDE 这样的搜索和检索应用来提高 LLM 系统性能。

# 潜在的未来工作：数据标注的解释

通过这种基准测试，我们发现了一些有趣的示例，其中提供解释会改变预测标签。以下示例在没有使用解释时预测为“relevant”，即使真实标签也是“relevant”。由于即使是“黄金数据集”也可能存在标注错误（尤其是在更主观的任务中），LLM提供的充分解释可能足以编辑真实标签。这可以视为LLM辅助的评估或质量检查。

以下是来自维基数据集的一个示例，用于相关性说明。请注意，‘D’列是数据集提供的真实标签，‘E’列是没有调用函数和解释的预测标签，而‘F’列是创建的预测标签（未调用函数），并带有‘G’列中的解释。因此，‘E’列和‘F’与‘G’列是两个独立LLM调用的响应。F&G是从同一次调用中生成的，如下图1所示。

![](../Images/f794b0059301154c29e8a9b7bdd9919c.png)

图1（作者提供）：脚本截图（代码见colab）。在这里，标签和解释一起返回，但我们要求首先提供解释（见提示）。

下表展示了当我们拥有真实标签为‘relevant’，LLM预测标签在没有调用函数的情况下为‘relevant’，但当LLM被要求首先提供解释时，标签变为‘irrelevant’的情况。与我们遇到的几个类似示例一样，LLM对将检索到的答案标记为‘irrelevant’提出了有效的论据。虽然我们许多人经常考虑罗马帝国，但我们可以同意，对于“罗马帝国持续了多久？”的多段回答既不简洁也不够相关，不足以引起最终用户的积极反馈。LLM辅助的评估有很多可能性，包括为需要数据标记的公司节省成本和时间。这以及解释所提供的可见性，以及LLM返回其参考文献（用于预测的文档），都是LLM可观测性领域的重大进展。

![](../Images/be2961b71844d8b67ab3d7ddf8c4cae6.png)

作者示例

# 结论

希望[这篇文章](https://arize.com/blog/calling-all-functions-benchmarking-openai-function-calling-and-explanations/)能为那些希望更好地理解并平衡新LLM应用系统的性能与速度权衡的团队提供一个良好的开端。正如往常一样，生成式AI和LLMOps领域正在迅速发展，因此观察这些发现和领域如何随时间变化将是很有趣的。

有问题吗？请在此处或在[LinkedIn](https://www.linkedin.com/in/amber-roberts42/)、[X](https://twitter.com/astronomeramber)或[Slack](https://join.slack.com/t/arize-ai/shared_invite/zt-26zg4u3lw-OjUNoLvKQ2Yv53EfvxW6Kg)与我联系！
