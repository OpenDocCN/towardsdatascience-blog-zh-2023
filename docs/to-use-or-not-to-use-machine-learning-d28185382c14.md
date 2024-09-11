# 是否使用机器学习

> 原文：[https://towardsdatascience.com/to-use-or-not-to-use-machine-learning-d28185382c14?source=collection_archive---------6-----------------------#2023-07-28](https://towardsdatascience.com/to-use-or-not-to-use-machine-learning-d28185382c14?source=collection_archive---------6-----------------------#2023-07-28)

## 如何决定是否使用机器学习，以及这在生成式AI的影响下是如何变化的

[](https://annaviaba.medium.com/?source=post_page-----d28185382c14--------------------------------)[![Anna Via](../Images/7e8fe5c1a485a789edad3a6d118bcf45.png)](https://annaviaba.medium.com/?source=post_page-----d28185382c14--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d28185382c14--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d28185382c14--------------------------------) [Anna Via](https://annaviaba.medium.com/?source=post_page-----d28185382c14--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc1a8933ed8b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-use-or-not-to-use-machine-learning-d28185382c14&user=Anna+Via&userId=c1a8933ed8b&source=post_page-c1a8933ed8b----d28185382c14---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d28185382c14--------------------------------) ·8分钟阅读·2023年7月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd28185382c14&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-use-or-not-to-use-machine-learning-d28185382c14&user=Anna+Via&userId=c1a8933ed8b&source=-----d28185382c14---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd28185382c14&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fto-use-or-not-to-use-machine-learning-d28185382c14&source=-----d28185382c14---------------------bookmark_footer-----------)![](../Images/8d5c08adb67df49de8a44d9e52766051.png)

图片来源于 [Ivan Aleksic](https://unsplash.com/es/@ivalex) 在 [Unsplash](https://unsplash.com/es/fotos/2RRq1BHPq4E?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

机器学习擅长解决某些复杂问题，通常涉及特征和结果之间的难以用启发式规则或if-else语句编码的复杂关系。然而，在决定是否使用机器学习作为解决方案时，有一些局限性或需考虑的因素。在这篇文章中，我们将深入探讨**“是否使用机器学习”**这一话题，首先理解“传统”机器学习模型，然后讨论随着生成式AI的进展这一图景如何变化。

为了澄清一些要点，我将以以下倡议为例：*“作为一家公司，我想知道我的客户是否满意以及不满的主要原因”*。解决这个问题的“传统”机器学习方法可能是：

+   获取客户对你的评论（应用程序或商店、推特或其他社交网络、你的网站…）

+   使用情感分析模型将评论分类为积极/中性/消极。

+   对预测的“消极情绪”评论使用主题建模，以了解其内容。

![](../Images/ffc441f8fc3babbbb67a76fdab1f0e13.png)

评论的分类分为积极、中性和消极情绪（图像来源于作者）

# 我的数据是否有足够的质量和量？

在监督式机器学习模型中，训练数据对于模型学习预测所需的内容（在这个例子中是评论的情感）是必要的。如果数据质量较低（如拼写错误、缺失数据、错误等），模型很难表现良好。

> 这通常被称为“垃圾进，垃圾出”问题：如果你的数据是垃圾，你的模型和预测也会是垃圾。

同样，你需要有足够的数据量，以便模型学习影响预测所需内容的不同情况。在这个例子中，如果你只有一个带有“无用”、“失望”或类似概念的消极评论标签的案例，模型将无法学习这些词通常在标签为“消极”时出现。

足够的训练数据量也有助于确保你对需要进行预测的数据有良好的代表性。例如，如果你的训练数据中没有某个特定地理区域或特定人群的代表，那么模型在预测这些评论时表现不佳的可能性会更大。

对于某些使用案例，拥有足够的历史数据也很重要，以确保我们能够计算相关的滞后特征或标签（例如“客户是否在下一年偿还信用”）。

# 标签是否明确易于定义和获取？

再次，对于传统的监督式机器学习模型，你需要一个标记数据集：即你知道最终预测结果的示例，以便训练你的模型。

标签的定义是关键。在这个例子中，我们的标签将是与评论相关的情感。我们可能认为只有“积极”或“消极”评论，然后争论是否可能有“中性”评论。在这种情况下，从给定评论中，通常会清楚标签是否需要是“积极”、“中性”或“消极”。但如果我们有“非常积极”、“积极”、“中性”、“消极”或“非常消极”的标签呢？对于给定的评论，是否那么容易决定它是“积极”还是“非常积极”？这种标签定义不明确的情况需要避免，因为训练时使用噪声标签会使模型学习更困难。

现在标签的定义已经明确，我们需要能够为一个足够数量且质量高的示例获取这些标签，这些标签将构成我们的训练数据。在我们的例子中，我们可以考虑手动标记一组评论，无论是在公司或团队内部，还是将标记工作外包给专业注释员（是的，有专门的人全职从事机器学习数据集的标记工作！）。获取这些标签的成本和可行性需要被考虑。

![](../Images/55e0ce3919aa814f6b65c54efab18ce4.png)

对于一个示例的标签，在我们的例子中，是评论（作者提供的图片）

# 解决方案的部署是否可行？

为了实现最终的影响，机器学习模型的预测结果需要是可用的。根据用例，使用预测结果可能需要特定的基础设施（例如机器学习平台）和专家（例如机器学习工程师）。

在我们的例子中，由于我们希望将模型用于分析目的，我们可以离线运行它，并且利用预测结果会相当简单。然而，如果我们希望在评论发布后的5分钟内自动回应负面评论，那将是另一回事：模型需要被部署和集成才能实现这一目标。总体而言，明确使用预测结果的要求非常重要，以确保团队和现有工具能够实现这些要求。

# 风险是什么？

机器学习模型的预测总会存在一定程度的误差。实际上，机器学习领域有一个经典的说法：

> 如果模型没有错误，那么数据或模型肯定存在问题

理解这一点很重要，因为如果用例不允许这些错误发生，那么使用机器学习可能不是一个好主意。在我们的例子中，假设我们不是处理评论和情感，而是使用模型将客户的电子邮件分类为“是否起诉”。拥有一个可能误分类对公司提出起诉的电子邮件的模型并不是一个好主意，因为这可能对公司造成严重后果。

# 使用机器学习是否符合伦理？

许多经验证的预测模型已经因为基于性别、种族和其他敏感个人属性进行歧视而受到质疑。因此，机器学习团队需要谨慎选择用于项目的数据和特征，同时也需要质疑自动化某些类型决策是否从伦理角度来看是合理的。你可以查看我之前的[博客文章](https://medium.com/glovo-engineering/ml-bias-intro-risks-and-solutions-to-discriminatory-predictive-models-9335b7709818)以获取更多细节。

# 我需要解释性吗？

机器学习模型在某种程度上充当了黑箱：你输入一些信息，它们神奇地输出预测。模型背后的复杂性就是这个黑箱的本质，特别是与统计学中的简单算法相比。在我们的例子中，我们可能不必完全理解为什么一个评论被预测为“积极”或“消极”。

在其他用例中，解释性可能是必须的。例如，在保险或银行等高度监管的领域。即使决定基于评分预测模型，银行也需要能够解释为什么给予（或不给予）某个人信用。

这个话题与伦理问题有很强的关系：如果我们不能完全理解模型的决策，就很难知道模型是否学会了具有歧视性。

# 所有这些都随着生成式AI的出现而改变了吗？

随着生成式AI的进步，各种公司提供了网页和API来使用强大的模型。这是如何改变我之前提到的关于机器学习的限制和考虑的？

+   **数据相关话题（质量、数量和标签）：** 对于能够利用现有生成式AI模型的用例，这种情况正在发生变化。大量数据已经用于训练生成式AI模型。大多数模型的数据质量未被控制，但这似乎被其使用的大量数据所弥补。由于这些模型，这可能会成为事实（再次强调，非常具体的用例），我们可能不再需要训练数据。这被称为零样本学习（例如，*“询问ChatGPT给定评论的情感”*）和少样本学习（例如，*“向ChatGPT提供一些积极、中性和消极评论的例子，然后要求它对新评论提供情感分析”*）。有关此内容的良好解释可以在[deeplearning.ai新闻通讯](https://www.deeplearning.ai/the-batch/how-prompting-is-changing-machine-learning-development/)中找到。

+   **部署可行性：** 对于能够利用现有生成式AI模型的用例，部署变得容易得多，因为许多公司和工具提供了易于使用的API。如果这些模型需要微调或因隐私原因引入内部，那么部署自然会变得更困难。

![](../Images/e5b0a50fec9dd97ae0f24a3b96b08a06.png)

“传统”机器学习与利用生成式AI模型（图像由作者提供）

其他限制或考虑因素不会改变，无论是否利用生成式AI：

+   **高风险：** 这个问题仍然存在，因为生成式AI模型在预测中也有一定的误差。谁没见过GhatGPT出现幻觉或给出不合理的答案？更糟糕的是，评估这些模型变得更难，因为无论模型的准确度如何，响应总是听起来很自信，评估变得主观（例如，*“这个回答对我来说是否有意义？”*）。

+   **伦理：** 依然像以前一样重要。已有证据表明，生成式人工智能模型可能因用于训练的数据输入而存在偏见 ([link](https://news.mit.edu/2023/large-language-models-are-biased-can-logic-help-save-them-0303))。随着越来越多的公司和功能开始使用这些类型的模型，明确它们可能带来的风险是很重要的。

+   **可解释性：** 由于生成式人工智能模型比“传统”机器学习更大、更复杂，其预测的可解释性变得更加困难。虽然目前正在进行研究以理解如何实现这种可解释性，但仍然非常不成熟 ([link](https://techcrunch.com/2023/05/09/openais-new-tool-attempts-to-explain-language-models-behaviors/))。

# 总结

在这篇博客文章中，我们讨论了在决定是否**使用或不使用机器学习**时需要考虑的主要因素，以及随着生成式人工智能模型的进展这些因素如何变化。讨论的主要主题包括数据的质量和数量、标签获取、部署、风险、伦理和可解释性。我希望这个总结对你考虑下一个机器学习（或不使用）项目时有所帮助！

**参考文献**

[1] [机器学习偏见：介绍、风险及解决方案](https://medium.com/glovo-engineering/ml-bias-intro-risks-and-solutions-to-discriminatory-predictive-models-9335b7709818)，由我自己提供

[2] [超越测试集：提示如何改变机器学习开发](https://www.deeplearning.ai/the-batch/how-prompting-is-changing-machine-learning-development/)，由 deeplearning.ai 提供

[3] [大型语言模型存在偏见，逻辑能帮助拯救它们吗？](https://news.mit.edu/2023/large-language-models-are-biased-can-logic-help-save-them-0303)，由 MIT News 提供

[4] [OpenAI 解释语言模型行为的尝试](https://techcrunch.com/2023/05/09/openais-new-tool-attempts-to-explain-language-models-behaviors/)，TechCrunch
