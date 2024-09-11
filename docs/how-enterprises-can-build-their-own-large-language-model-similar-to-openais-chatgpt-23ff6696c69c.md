# 企业如何构建类似于 OpenAI 的 ChatGPT 的大型语言模型

> 原文：[https://towardsdatascience.com/how-enterprises-can-build-their-own-large-language-model-similar-to-openais-chatgpt-23ff6696c69c?source=collection_archive---------2-----------------------#2023-07-01](https://towardsdatascience.com/how-enterprises-can-build-their-own-large-language-model-similar-to-openais-chatgpt-23ff6696c69c?source=collection_archive---------2-----------------------#2023-07-01)

## 想要构建你自己的 ChatGPT 吗？这里有三种方法可以实现

[](https://pronojit.medium.com/?source=post_page-----23ff6696c69c--------------------------------)[![Pronojit Saha](../Images/a15a3546470293cc1bec858af63c4515.png)](https://pronojit.medium.com/?source=post_page-----23ff6696c69c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----23ff6696c69c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----23ff6696c69c--------------------------------) [Pronojit Saha](https://pronojit.medium.com/?source=post_page-----23ff6696c69c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa9c37f94155a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-enterprises-can-build-their-own-large-language-model-similar-to-openais-chatgpt-23ff6696c69c&user=Pronojit+Saha&userId=a9c37f94155a&source=post_page-a9c37f94155a----23ff6696c69c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----23ff6696c69c--------------------------------) · 10 分钟阅读 · 2023年7月1日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F23ff6696c69c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-enterprises-can-build-their-own-large-language-model-similar-to-openais-chatgpt-23ff6696c69c&source=-----23ff6696c69c---------------------bookmark_footer-----------)![](../Images/57660313805e8cf4021ef4fc6a82c1a5.png)

图 1：构建自定义 LLM 的三种方式（图像来源：作者）

TL;DR。企业应该构建自己的定制LLM，因为它提供了定制、控制、数据隐私和透明度等各种好处。为了简化构建定制LLM的过程，建议遵循三个级别的方法——L1、L2和L3。这些级别从低模型复杂度、准确性和成本（L1）到高模型复杂度、准确性和成本（L3）。企业必须在这些折衷中取得平衡，以满足他们的需求并从LLM计划中获得投资回报。

# 介绍

语言模型近年来获得了显著关注，彻底改变了自然语言处理、内容生成和虚拟助手等多个领域。一个最突出的例子是OpenAI的ChatGPT，这是一种大型语言模型，可以生成类人文本并进行互动对话。这引发了企业的好奇心，促使他们探索构建自己大型语言模型（LLM）的想法。

然而，决定是否开始构建LLM需要仔细审视。它需要大量的资源，包括计算能力和数据可用性。企业必须权衡收益与成本，评估所需的技术专长，并判断这是否符合他们的长期目标。

在这篇文章中，我们向你展示了构建自己LLM的三种方法，类似于OpenAI的ChatGPT。在文章结束时，你将对构建自己的大型语言模型相关的挑战、需求和潜在回报有更清晰的了解。让我们深入探讨吧！

# 企业应该构建自己的LLM吗？

要了解企业是否应该构建自己的LLM，让我们探索他们可以利用这些模型的三种主要方式。

![](../Images/a0b7115bf05e1a73ab4495a8fcbe2417.png)

图2：利用大型语言模型的不同方式（作者提供的图像）

**1\. 闭源LLM：** 企业可以利用现有的LLM服务，如OpenAI的ChatGPT、Google的Bard，或其他提供商的类似服务。这些服务提供了现成的解决方案，使企业能够利用LLM的强大功能，而无需进行大量基础设施投资或具备技术专长。

优势：

+   快速、简便的部署，节省时间和精力。

+   在一般文本生成任务上的良好表现。

缺点：

+   对模型行为和响应的控制有限

+   在特定领域或企业特定数据上的准确性较低

+   数据隐私问题，因为数据被发送到托管服务的第三方

+   依赖第三方供应商和可能的价格波动。

**2\. 使用领域特定LLM：** 另一种方法是使用领域特定的语言模型，例如用于金融的BloombergGPT、用于生物医学应用的BioMedLM、用于营销应用的MarketingGPT、用于电子商务应用的CommerceGPT等。这些模型在领域特定的数据上进行训练，从而在各自领域中提供更准确和量身定制的响应。

优势：

+   由于在相关数据上进行训练，特定领域的准确性得到了提升。

+   具有针对特定行业量身定制的预训练模型的可用性。

缺点：

+   在超出指定领域的适应性方面灵活性有限。

+   依赖于供应商的更新和领域特定模型的可用性。

+   稍微提高了准确性，但仍受限于非特定于您企业数据的情况

+   数据隐私问题，因为数据被发送到托管服务的第三方

**3\. 构建和托管自定义 LLM**：最全面的选项是企业使用其特定数据构建和托管自己的 LLM。这种方法提供了最高级别的定制和隐私控制，确保与其品牌声音的一致性和领域特定的准确性。

优点：

+   **完全定制和控制**：自定义模型使企业能够生成与其品牌声音、行业特定术语和独特要求完全一致的响应。

+   **成本效益**：如果设置得当（微调成本在几百美元左右）

+   **透明**：整个数据和模型对企业是已知的

+   **最佳准确性**：通过在企业特定数据和要求上训练模型，它可以更好地理解和回应企业特定的查询，从而产生更准确和上下文相关的输出。

+   **隐私友好**：数据和模型保留在您的环境中。拥有自定义模型可以使企业控制其敏感数据，从而减少与数据隐私和安全漏洞相关的担忧。

+   **竞争优势**：在个性化和准确语言处理起着关键作用的行业中，自定义大型语言模型可以成为显著的区分因素。

缺点：

+   需要显著的机器学习和 LLM 专业知识来构建自定义大型语言模型

需要注意的是，自定义 LLM 的方法取决于各种因素，包括企业的预算、时间限制、所需准确性和期望的控制级别。然而，正如您从上述内容中看到的，基于企业特定数据构建自定义 LLM 提供了许多好处。

> 自定义大型语言模型为特定领域、使用案例和企业需求提供了无与伦比的定制、控制和准确性。因此，企业应考虑构建自己的企业特定自定义大型语言模型，以解锁量身定制的无限可能，满足其需求、行业和客户群体。

# 构建自定义大型语言模型的三种方式

您可以通过三种方式构建自定义 LLM，这些方式的复杂性从低到高，如下图所示。

![](../Images/57660313805e8cf4021ef4fc6a82c1a5.png)

图 3：构建自定义 LLM 的三种方式（图片来源：作者）

**L1\. 利用调整的 LLM**

利用预训练 LLM 的一种常见方法是制定有效的提示技术以应对各种任务。一个常见的提示方法是 [上下文学习（ICL）](http://ai.stanford.edu/blog/understanding-incontext/)，其包括用自然语言文本表达任务描述和/或示例。此外，通过在提示中加入一系列中间推理步骤，[思维链（CoT）](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html?m=1) 的使用可以增强上下文学习。要构建一个 L1 LLM，

![](../Images/fbe243ef4d5a45eb7ae36dc594bff9fa.png)

图 4：ICL 和 CoT 提示的比较示意图。（图片来自论文《大型语言模型调查》——参考文献 №7）

要构建一个 L1 LLM，

1.  首先选择一个合适的预训练 LLM（可以在 [Hugging Face 模型库](https://huggingface.co/models) 或其他在线资源中找到），通过查看许可证确保其适用于商业用途。

1.  接下来，确定与你的特定领域或用例相关的数据源，组建一个多样化且全面的数据集，涵盖广泛的主题和语言变体。对于 L1 LLM，不需要标记数据。

1.  在定制过程中，所选预训练 LLM 的模型参数保持不变。相反，采用提示工程技术来调整 LLM 的响应以适应数据集。

1.  如上所述，上下文学习和思维链提示是两种流行的提示工程方法。这些技术统称为资源高效调优（RET），提供了一种简化的方式来获得回应，而无需大量基础设施资源。

**L2. 指令调优 LLM**

指令调优是对预训练 LLM 进行微调的一种方法，其形式为自然语言中的格式化实例，这与监督微调和多任务提示训练高度相关。通过指令调优，LLM 能够在没有显式示例的情况下（类似于零-shot 能力）遵循任务指令，从而具备更好的泛化能力。要构建这种指令调优的 L2 LLM，

1.  首先选择一个合适的预训练 LLM（可以在 [Hugging Face 模型库](https://huggingface.co/models) 或其他在线资源中找到），通过查看许可证确保其适用于商业用途。

1.  接下来，确定与你的目标领域或用例相关的数据源。需要一个包含与你的领域或用例相关的各种指令的标记数据集。例如，你可以参考Databricks提供的[dolly-15k数据集](https://huggingface.co/datasets/databricks/databricks-dolly-15k)，它提供了不同格式的指令，如闭合问答、开放问答、分类、信息检索等。这个数据集可以作为构建你自己指令数据集的模板。

1.  进入监督微调过程，我们向第1步中选择的原始基础LLM引入新的模型参数。通过添加这些参数，我们可以训练模型进行特定的训练轮次，以微调其对给定指令的响应。这个方法的优点在于它避免了更新基础LLM中数十亿个参数的需要，而是集中在较少数量的附加参数（数千或数百万）上，同时仍能在所需任务中取得准确的结果。这种方法还帮助减少了成本。

1.  下一步是进行微调。各种微调技术，如前缀调优、适配器、低秩注意力等，将在未来的文章中详细阐述。上述第3点中讨论的添加新模型参数的过程也依赖于这些技术。有关更多详细信息，请参见参考文献部分。这些技术属于参数高效微调（PEFT）类别，因为它们能够在不更新基础LLM的所有参数的情况下进行定制。

**L3. 对齐调优的LLM**

由于LLM（大语言模型）是通过捕捉预训练语料库的数据特征进行训练的（包括高质量和低质量的数据），它们可能会生成有毒、偏见甚至对人类有害的内容。因此，可能有必要将LLM与人类价值观对齐，例如，有帮助、诚实和无害。为了这一对齐目的，我们使用了与人类反馈的强化学习（RLHF）技术，这是一种有效的调优方法，可以使LLM遵循预期的指令。它通过精心设计的标记策略将人类纳入训练循环。要构建这个对齐调优的L3 LLM，

1.  首先选择一个开源的预训练LLM（可以在[Hugging Face模型库](https://huggingface.co/models)或其他在线资源中找到），或者选择你的L2 LLM作为基础模型。

1.  构建对齐调整LLM的主要技术是RLHF，它结合了监督学习和强化学习。首先，从第1步的特定领域或指令语料库的微调LLM开始，使用它生成回应。然后，这些回应由人类注释，用于训练监督奖励模型（通常使用另一个预训练LLM作为基础模型）。最后，通过使用奖励模型进行强化学习（[PPO](https://huggingface.co/learn/deep-rl-course/unit8/introduction)），对LLM（来自第1步）进行再次微调，以生成最终回应。

1.  因此，训练了两个LLM：一个用于奖励模型，另一个用于微调LLM以生成最终回应。在这两种情况下，基本模型参数可以有选择地更新，取决于所需的响应准确性。例如，在一些RLHF方法中，仅更新涉及强化学习的特定层或组件的参数，以避免过拟合，并保留预训练LLM捕获的一般知识。

这个过程的一个有趣的现象是，到目前为止，成功的RLHF系统使用了相对于文本生成的不同大小的奖励语言模型（例如，OpenAI 175B LM，6B奖励模型，Anthropic使用从10B到52B的LM和奖励模型，DeepMind使用70B Chinchilla模型作为LM和奖励模型）。一种直觉是，这些偏好模型需要有类似于生成文本所需的模型的能力，以理解所给定的文本。

还有RLAIF（带有AI反馈的强化学习），可以替代RLHF。主要区别在于，AI模型作为评估者或评论者，在强化学习过程中向AI代理提供反馈，而不是人类反馈。

# 结论

企业可以利用定制LLM的非凡潜力，实现与其特定领域、用例和组织需求相符的卓越定制、控制和准确性。建立一个特定于企业的定制LLM，使企业能够解锁大量量身定制的机会，完全符合其独特需求、行业动态和客户基础。

> 建立自定义LLM的过程有三个层级，从低模型复杂性、准确性和成本到高模型复杂性、准确性和成本。企业必须在这些权衡之间找到平衡，以最适合其需求并从LLM计划中获取投资回报。

![](../Images/8ff13e199df2d9c7494e07484dc97106.png)

图5：三个LLM层级之间的权衡（图像由作者提供）

**参考文献**

1.  [什么是提示工程](https://medium.com/r?url=https%3A%2F%2Fwww.hackerrank.com%2Fblog%2Fwhat-is-prompt-engineering%2F)?

1.  In-Context Learning (ICL) — Q. Dong, L. Li, D. Dai, C. Zheng, Z. Wu, B. Chang, X. Sun, J. Xu, L. Li, 和 Z. Sui, “关于上下文学习的调查，” CoRR, 卷. abs/2301.00234, 2023。

1.  [上下文学习如何工作？理解与传统监督学习差异的框架 | SAIL博客 (stanford.edu)](http://ai.stanford.edu/blog/understanding-incontext/)

1.  思维链提示 — J. Wei、X. Wang、D. Schuurmans、M. Bosma、E. H. Chi、Q. Le 和 D. Zhou，“思维链提示在大型语言模型中引发推理，”CoRR，第 abs/2201.11903 卷，2022年。

1.  [语言模型通过思维链进行推理 — Google AI 博客 (googleblog.com)](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html?m=1)

1.  指令调整 — J. Wei、M. Bosma、V. Y. Zhao、K. Guu、A. W. Yu、B. Lester、N. Du、A. M. Dai 和 Q. V. Le，“微调语言模型是零样本学习者，”发表于第十届国际学习表征会议，ICLR 2022，虚拟会议，2022年4月25–29日\. OpenReview.net，2022年。

1.  一项关于大型语言模型的调查 — Wayne Xin Zhao、Kun Zhou*、Junyi Li*、Tianyi Tang、Xiaolei Wang、Yupeng Hou、Yingqian Min、Beichen Zhang、Junjie Zhang、Zican Dong、Yifan Du、Chen Yang、Yushuo Chen、Zhipeng Chen、Jinhao Jiang、Ruiyang Ren、Yifan Li、Xinyu Tang、Zikang Liu、Peiyu Liu、Jian-Yun Nie 和 Ji-Rong Wen，arXiv:2303.18223v4 [cs.CL]，2023年4月12日
