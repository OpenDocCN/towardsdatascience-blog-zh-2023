# LLM 评估指南：如何构建和基准测试你的评估

> 原文：[https://towardsdatascience.com/llm-evals-setup-and-the-metrics-that-matter-2cc27e8e35f3?source=collection_archive---------0-----------------------#2023-10-13](https://towardsdatascience.com/llm-evals-setup-and-the-metrics-that-matter-2cc27e8e35f3?source=collection_archive---------0-----------------------#2023-10-13)

![](../Images/0141ad936d35fc1e8736f72b7ec61ce8.png)

图片由作者使用 Dalle-3 通过 Bing Chat 创建

## 如何构建和运行 LLM 评估——以及为何在基准测试你的 LLM 提示模板时使用精确度和召回率

[](https://aparnadhinak.medium.com/?source=post_page-----2cc27e8e35f3--------------------------------)[![Aparna Dhinakaran](../Images/e431ee69563ecb27c86f3428ba53574c.png)](https://aparnadhinak.medium.com/?source=post_page-----2cc27e8e35f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2cc27e8e35f3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2cc27e8e35f3--------------------------------) [Aparna Dhinakaran](https://aparnadhinak.medium.com/?source=post_page-----2cc27e8e35f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff32f85889f3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-evals-setup-and-the-metrics-that-matter-2cc27e8e35f3&user=Aparna+Dhinakaran&userId=f32f85889f3a&source=post_page-f32f85889f3a----2cc27e8e35f3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2cc27e8e35f3--------------------------------) ·12 分钟阅读·2023年10月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2cc27e8e35f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-evals-setup-and-the-metrics-that-matter-2cc27e8e35f3&user=Aparna+Dhinakaran&userId=f32f85889f3a&source=-----2cc27e8e35f3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2cc27e8e35f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fllm-evals-setup-and-the-metrics-that-matter-2cc27e8e35f3&source=-----2cc27e8e35f3---------------------bookmark_footer-----------)

*这篇文章由* [*Ilya Reznik*](https://ibreznik.com/) *共同创作*

大型语言模型（LLM）是开发者和商业领袖为消费者创造新价值的不可思议的工具。它们可以提供个人化推荐、在非结构化数据和结构化数据之间进行翻译、总结大量信息，等等。

随着应用程序的增加，衡量基于LLM的应用程序的性能变得越来越重要。这是一个非平凡的问题，原因有几个：用户反馈或任何其他**“真实来源”极其有限且往往不存在**；即使可能，**人工标记仍然很昂贵**；而且这些应用程序很容易变得**复杂**。

这种复杂性通常被代码的抽象层隐藏，只有当事情出错时才会显现出来。一行代码可以触发一连串的调用（跨度）。**每个跨度需要不同的评估**，从而使问题倍增。例如，下面的简单代码片段会触发多个子LLM调用。

![](../Images/5300072b0e27ee545a3c0ded1c702754.png)

作者绘制的图表

幸运的是，我们可以利用LLM的力量来自动化评估。在这篇文章中，我们将深入探讨如何设置这一过程并确保其可靠性。

**LLM评估的核心是AI评估AI。**

虽然这听起来有些循环，但我们一直以来都是由人类智慧来评估人类智慧（例如，在面试或大学期末考试中）。现在，AI系统终于可以对其他AI系统进行同样的评估。

这里的过程是让LLM生成可以用于评估另一个系统的合成基准。这引出了一个问题：为什么不直接使用人工反馈？简单来说，因为你永远不会拥有足够的人工反馈。

对于你输入/输出对中的哪怕只有百分之一获得人工反馈，这也是一项巨大的成就。大多数团队甚至无法做到这一点。但是，为了使这一过程真正有用，重要的是对每个LLM子调用进行评估，我们已经看到，LLM子调用可能会有很多。

让我们来探讨一下如何做到这一点。

# LLM模型评估与LLM系统评估

```py
LLM_model_evals != LLM_System_evals
```

## LLM模型评估

你可能听说过LLM评估。这个术语在许多不同的情况下使用，听起来很相似，但实际上非常不同。一个更常见的用法是我们所称的**LLM模型评估**。LLM模型评估关注于基础模型的整体性能。推出原始面向客户的LLM的公司需要一种量化其在各种任务中的有效性的方法。

![](../Images/ecfd3204dba3144c65b2e87736695bc1.png)

作者绘制的图表 | 在这种情况下，我们正在评估两个不同的开源基础模型。我们在这两个模型上测试相同的数据集，看看它们的指标，例如hellaswag或mmlu，如何表现。

一个流行的库是[OpenAI Eval库](/how-to-best-leverage-openais-evals-framework-c38bcef0ec47)，最初专注于模型评估使用案例。现在有许多指标，如[HellaSwag](https://arxiv.org/abs/1905.07830)（评估LLM完成句子的能力）、[TruthfulQA](https://arxiv.org/abs/2109.07958)（衡量模型回应的真实性）和[MMLU](https://arxiv.org/abs/2009.03300)（评估LLM的多任务能力）。甚至还有[LLM排行榜](https://arize.com/blog-course/llm-leaderboards-benchmarks/)查看开源LLM的表现如何。

## LLM系统评估

到目前为止，我们讨论了LLM模型评估。相比之下，**LLM系统评估**是对你在系统中可控组件的完整评估。这些组件中最重要的是提示（或[提示模板](https://arize.com/blog/prompt-templates-functions-and-prompt-window-management/)）和上下文。LLM系统评估评估你的输入如何决定你的输出。

例如，LLM系统评估可以保持LLM不变并更改提示模板。由于提示是系统中更动态的部分，这种评估在项目生命周期中非常有意义。例如，LLM可以评估你的聊天机器人响应的有用性或礼貌性，并且相同的评估可以提供关于生产环境中性能变化的信息。

![](../Images/f45a254033d1cb9a594b71670cd7f58d.png)

作者图示 | *在这种情况下，我们正在评估在单一基础模型上使用的两种不同提示模板。我们在这两个模板上测试相同的数据集，并查看它们的精确度和召回率等指标的表现。*

## 使用哪种？这取决于你的角色

使用LLM评估的角色有区别。一种是负责调整核心LLM的模型开发者或工程师，另一种是组装用户系统的实践者。

LLM模型开发者非常少，他们通常在OpenAI、Anthropic、Google、Meta等公司工作。**模型开发者关心LLM模型评估，** 因为他们的工作是提供一个适用于各种使用案例的模型。

对于ML实践者，任务同样从模型评估开始。开发LLM系统的第一步之一是选择一个模型（即GPT 3.5 vs 4 vs Palm等）。然而，对于这一群体，LLM模型评估通常是一个一次性的步骤。一旦确定了哪个模型在你的使用案例中表现最好，应用生命周期的大部分将由LLM系统评估决定。因此，**ML实践者关心LLM模型评估和LLM系统评估，但可能在后者上花费更多时间**。

# LLM系统评估指标因使用案例而异

在与其他机器学习系统合作后，你首先会问这个问题：“结果指标应该是什么？”答案取决于你要评估的内容。

+   **提取结构化信息**：你可以查看LLM提取信息的效果。例如，你可以查看完整性（输入中是否有输出中没有的信息？）。

+   **问答**：系统回答用户问题的效果如何？你可以查看答案的准确性、礼貌性或简洁性——或者以上所有方面。

+   **检索增强生成（RAG）**：检索到的文档和最终答案是否相关？

作为系统设计师，你最终负责系统性能，因此你需要了解系统中哪些方面需要评估。例如，如果你有一个与儿童互动的LLM，比如一个辅导应用，你会希望确保回答是适龄的，并且不含有毒内容。

目前一些常用的评估包括相关性、幻觉、问答准确性和毒性。每一个评估工具都会有不同的模板，具体取决于你要评估的内容。以下是一个关于相关性的示例：

这个示例为了简单起见使用开源的[Phoenix工具](https://github.com/Arize-ai/phoenix)（完全披露：我在开发Phoenix的团队中）。在Phoenix工具中，存在适用于大多数常见用例的默认模板。以下是我们将用于本示例的模板：

```py
You are comparing a reference text to a question and trying to determine if the reference text contains information relevant to answering the question. Here is the data:
    [BEGIN DATA]
    ************
    [Question]: {query}
    ************
    [Reference text]: {reference}
    [END DATA]
Compare the Question above to the Reference text. You must determine whether the Reference text
contains information that can answer the Question. Please focus on whether the very specific
question can be answered by the information in the Reference text.
Your response must be single word, either "relevant" or "irrelevant",
and should not contain any text or characters aside from that word.
"irrelevant" means that the reference text does not contain an answer to the Question.
"relevant" means the reference text contains an answer to the Question.
```

我们还将使用OpenAI的GPT-4模型和scikit-learn的精确度/召回率指标。

首先，我们将导入所有必要的依赖项：

```py
from phoenix.experimental.evals import (
   RAG_RELEVANCY_PROMPT_TEMPLATE_STR,
   RAG_RELEVANCY_PROMPT_RAILS_MAP,
   OpenAIModel,
   download_benchmark_dataset,
   llm_eval_binary,
)
from sklearn.metrics import precision_recall_fscore_support
```

现在，让我们引入数据集：

```py
# Download a "golden dataset" built into Phoenix
benchmark_dataset = download_benchmark_dataset(
   task="binary-relevance-classification", dataset_name="wiki_qa-train"
)
# For the sake of speed, we'll just sample 100 examples in a repeatable way
benchmark_dataset = benchmark_dataset.sample(100, random_state=2023)
benchmark_dataset = benchmark_dataset.rename(
   columns={
       "query_text": "query",
       "document_text": "reference",
   },
)
# Match the label between our dataset and what the eval will generate
y_true = benchmark_dataset["relevant"].map({True: "relevant", False: "irrelevant"})
```

现在让我们进行评估：

```py
# Any general purpose LLM should work here, but it is best practice to keep the temperature at 0
model = OpenAIModel(
   model_name="gpt-4",
   temperature=0.0,
)
# Rails will define our output classes
rails = list(RAG_RELEVANCY_PROMPT_RAILS_MAP.values())

benchmark_dataset["eval_relevance"] = \
   llm_eval_binary(benchmark_dataset,
                   model,
                   RAG_RELEVANCY_PROMPT_TEMPLATE_STR,
                   rails)
y_pred = benchmark_dataset["eval_relevance"]

# Calculate evaluation metrics
precision, recall, f1, support = precision_recall_fscore_support(y_true, y_pred)
```

## 使用LLM对LLM基础系统进行评估

使用LLM对LLM基础系统进行评估的过程包括两个不同的步骤。首先，为你的LLM评估指标建立基准。为此，你需要组建一个专门的LLM评估工具，其唯一任务是将数据标注得尽可能像人工标注你的“黄金数据集”一样有效。然后，你将你的指标与该评估工具进行基准测试。接着，将这个LLM评估指标应用于你的LLM应用结果（下面会详细说明）。

# 如何构建LLM评估工具

首先，如上所述，构建评估的基准是第一步。

为此，你必须从**最适合你用例的指标**开始。然后，你需要**黄金数据集**。这应该能够代表你期望LLM评估工具看到的数据类型。黄金数据集应该有“真实标签”，以便我们可以衡量LLM评估模板的性能。这些标签通常来自人工反馈。构建这样的数据集是费力的，但你通常可以找到针对最常见用例的标准化数据集（正如我们在上面的代码中所做的）。

![](../Images/e8d4068cf295bc602a23ab7d4d565b6c.png)

作者绘制的图表

然后你需要决定**使用哪个 LLM** 进行评估。这可能是与用于应用的 LLM 不同的另一个 LLM。例如，你可能在应用中使用 Llama，而在评估中使用 GPT-4。这个选择通常受到成本和准确性问题的影响。

![](../Images/747ecc27cf13c132829bcb912ceabb24.png)

作者提供的图示

现在来到我们试图基准化和改进的核心组件：**评估模板**。如果你使用的是像 OpenAI 或 Phoenix 这样的现有库，你应该从现有的模板开始，看看那个提示的表现如何。

如果有特定的细节你想要加入，可以相应地调整模板或从头开始构建自己的模板。

请记住，模板应该有一个清晰的结构，就像我们在前面部分使用的那样。对以下内容要明确：

+   **输入是什么？** 在我们的例子中，它是检索到的文档/上下文和用户的查询。

+   **我们在问什么？** 在我们的例子中，我们要求 LLM 告诉我们文档是否与查询相关。

+   **可能的输出格式是什么？** 在我们的例子中，它是二元相关/不相关，但也可以是多类（例如，完全相关、部分相关、不相关）。

![](../Images/bebbe4a5ea7985283fb654af5381d1a1.png)

作者提供的图示

你现在需要在你的黄金数据集上运行评估。然后你可以**生成指标**（总体准确性、精确度、召回率、F1 等）来确定基准。重要的是要查看的不仅仅是总体准确性。我们将在下面详细讨论。

如果你对 LLM 评估模板的性能不满意，你需要更改提示以提高其表现。这是一个由硬指标驱动的迭代过程。像往常一样，重要的是避免将模板过度拟合到黄金数据集。确保有一个代表性的保留集或进行 k 折交叉验证。

![](../Images/e160e9a9dee0af2ad439b7df0361e42c.png)

作者提供的图示

最终，你到达了你的**基准**。在黄金数据集上的优化性能代表了你对 LLM 评估的信心。它可能不会像你的真实数据那样准确，但足够准确，而且成本远低于每个示例都需要人工标注的情况。

准备和自定义你的提示模板可以让你设置测试用例。

# 为什么在基准测试你的 LLM 提示模板时应该使用精确度和召回率

行业尚未完全标准化 LLM 评估的最佳实践。团队通常不知道如何建立正确的基准指标。

总体准确性经常被使用，但这还不够。

这是数据科学中最常见的问题之一：非常显著的类别不平衡使得准确性成为一个不切实际的指标。

从相关性指标的角度考虑是有帮助的。假设你经历了所有麻烦和费用，组建了你能组建的最相关的聊天机器人。你选择了一个适合用例的 LLM 和模板。这应该意味着你的示例中会有显著更多的被评估为“相关”的。让我们选择一个极端的数字来说明这个观点：99.99% 的所有查询返回相关结果。太棒了！

现在从 LLM 评估模板的角度来看。如果输出在所有情况下都是“相关”，即使不查看数据，它的正确率也会是99.99%。但它同时会错过所有（可以说是最重要的）案例——即模型返回无关结果的那些，我们必须捕捉这些结果。

在这个例子中，准确率会很高，但[精确率和召回率](https://arize.com/blog-course/precision-vs-recall/)（或者它们的组合，比如[F1 分数](https://arize.com/blog-course/f1-score/)）会非常低。在这里，精确率和召回率更能衡量你模型的表现。

另一个有用的可视化是混淆矩阵，它基本上让你看到相关和无关示例的正确和错误预测百分比。

![](../Images/e0713aae1a956a4766d5a56b2b5e8d2c.png)

作者图表 | *在这个例子中，我们看到最高比例的预测是正确的：在黄金数据集中，一个相关的例子被我们的评估标记为相关的机会是88%。然而，我们看到评估在“无关”示例上的表现明显较差，错误标记的概率超过27%*。

# 如何在你的应用程序上运行 LLM 评估

此时你应该拥有模型和经过测试的 LLM 评估。你已经证明了评估有效，并对其相对于真实情况的表现有了可量化的理解。是时候建立更多信任了！

现在我们可以实际使用我们的评估来评估应用程序。这将帮助我们衡量我们的 LLM 应用程序的表现，并找出如何改进。

![](../Images/4f86ea814b4142eb391e3b536b290bfb.png)

作者图表

LLM 系统评估在你的整个系统中运行一个额外的步骤。例如：

+   你检索输入文档并将它们添加到你的提示模板中，以及样本用户输入。

+   你将这个提示提供给 LLM 并接收答案。

+   你将提示和答案提供给你的评估，询问答案是否与提示相关。

最佳实践是不使用一次性代码来进行 LLM 评估，而是使用具有内置提示模板的库。这增加了可重复性，并允许更灵活的评估，可以更换不同的部分。

这些评估需要在三种不同的环境中有效：

## 预生产

当你进行基准测试时。

## 预生产

当你在测试你的应用程序时。这在某种程度上类似于传统机器学习中的离线评估概念。其目的是在你将系统交付给客户之前了解系统的性能。

## 生产

当系统投入使用时，现实生活是混乱的。数据漂移、用户漂移、模型漂移，所有这些都是不可预测的。仅仅因为你的系统曾经运行良好，并不意味着它在周二下午7点也会如此。评估有助于你在部署后持续了解系统的性能。

![](../Images/98b24eb1c65e16c0d7245e4643e68083.png)

作者图示

# 需要考虑的问题

## **你应该抽取多少行？**

LLM评估LLM的范式并不是魔法。你不能评估你曾经遇到的每一个示例——那样会代价高昂。然而，你在人工标记期间已经需要抽样数据，更多的自动化只会使这变得更容易和便宜。因此，你可以抽取比人工标记更多的行。

## **你应该使用什么评估？**

这在很大程度上取决于你的使用案例。对于搜索和检索，相关性类型的评估效果最好。毒性和幻觉有特定的评估模式（上述已有提及）。

其中一些评估在故障排查流程中很重要。问答准确性可能是一个很好的总体指标，但如果你深入挖掘为什么这个指标在你的系统中表现不佳，你可能会发现这可能是由于检索不良等原因。通常有很多可能的原因，你可能需要多个指标来找到根本原因。

## **你应该使用什么模型？**

不可能说哪种模型适用于所有情况。相反，你应该进行模型评估，以了解哪个模型适合你的应用程序。你还可能需要考虑召回率与精度的权衡，具体取决于对你的应用程序来说哪种更有意义。换句话说，做一些数据科学来了解你的特定情况。

![](../Images/3fcd84c78cd24147d2effdb88b6893e2.png)

作者图示

# 结论

能够评估应用程序的性能在生产代码中是非常重要的。在LLM时代，问题变得更加复杂，但幸运的是，我们可以使用LLM技术来帮助我们进行评估。[LLM评估](https://arize.com/blog-course/llm-evaluation-the-definitive-guide/)应该测试整个系统，而不仅仅是基础LLM模型——考虑一下提示模板对用户体验的重要性。最佳实践、标准化工具和精心策划的数据集简化了LLM系统的开发工作。
