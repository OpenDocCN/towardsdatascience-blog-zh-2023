# 考虑微调 LLM？在你开始之前，这里有 3 个考虑因素

> 原文：[`towardsdatascience.com/thinking-about-fine-tuning-an-llm-heres-3-considerations-before-you-get-started-c1f483f293`](https://towardsdatascience.com/thinking-about-fine-tuning-an-llm-heres-3-considerations-before-you-get-started-c1f483f293)

[](https://medium.com/@smsmith714?source=post_page-----c1f483f293--------------------------------)![Sean Smith](https://medium.com/@smsmith714?source=post_page-----c1f483f293--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1f483f293--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1f483f293--------------------------------) [Sean Smith](https://medium.com/@smsmith714?source=post_page-----c1f483f293--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1f483f293--------------------------------) ·11 分钟阅读·2023 年 6 月 15 日

--

![](img/fabd8f334f6e560c8750dadc546fdd8e.png)

照片由[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

目前 LLM（大型语言模型）和生成 AI 正当红。来自[IBM](https://www.ibm.com/thought-leadership/institute-business-value/report/generative-ai-data-story)的惊人统计数据显示，近 2/3 的 C-Suite 高管感受到来自投资者的压力，要求加快生成 AI 的采纳。这种压力自然会传递给数据科学和机器学习团队，他们负责应对炒作并创建成功的实施方案。

随着环境的发展，LLM 的生态系统在开源模型和行业模型之间发生了分化，形成了一个[迅速填补的护城河](https://www.semianalysis.com/p/google-we-have-no-moat-and-neither)。这一新兴场景促使许多团队考虑以下问题：我们如何使 LLM 更具体地适用于我们的用例？

在这篇文章中，我们探讨了在考虑投入时间和工程周期来构建一个细分 LLM 时需要重点关注的一些关键因素。在这段旅程中，了解一些关于潜在限制和最佳实践的最新研究至关重要，这些研究涉及如何构建微调的语言模型。阅读完这篇文章后，你将会有更多的想法来指导你的组织做出正确的决定——是训练还是不训练，以及如何训练。

# 你可能无法用开源模型模拟 GPT

任何人都知道，OpenAI 正在通过其最新的 GPT 版本引领 LLM 的发展。因此，许多利益相关者可能会要求开发团队部署一个模型，以模仿更强大模型的结果，原因可能包括（速率限制、数据隐私、成本等）。这自然引发了开发者的疑问：我们能否从 GPT 生成输出，并利用这些输出来微调一个模型？

这个问题的答案仍然不确定，因为它似乎取决于几个因素。这个特定任务，称为模仿学习，涉及通过使用来自更先进模型（如 GPT）的目标观察来微调训练一个新的语言模型。虽然这似乎是从下游模型中获得良好性能的一个好方法，但也存在一些潜在的问题。

![](img/76cb4e5b4240d03930df9e5d59d3dce0.png)

图源自参考文献 Gudibande 等人 [1]。

一篇题为“模仿专有 LLM 的虚假承诺” [1] 的最近论文对这种方法可能遇到的陷阱提供了一些见解。作者展示了一些实验，表明增加更多模仿数据可能会导致模型性能的下降。查看上图，我们可以看到在中心图表中，基准任务上的准确性随着令牌数量的增加而下降。那么这是为什么呢？

作者建议出现这种情况的原因是，模仿模型学习的是它们模仿的模型的风格，而不是学习和理解模型的内容。查看上图左侧的窗格，人工评审员更喜欢模仿模型的结果而非 ChatGPT 的结果。经过探索，明显看到评审员喜欢模仿模型的风格，但并未仔细审查内容。注意到模仿模型产生的内容通常缺乏事实准确性，作者总结道：“模仿模型实际上体现了 AI 助手的一些最糟糕的方面：它们的回答听起来很自信，但比 ChatGPT 的回答更不准确。”

重要的是要注意，有些情况下模仿模型可以取得出色的表现。作者指出，模仿模型可以在本地任务或复制教师模型非常特定行为的任务上表现良好。在为研究创建的任务 NQ-Synthetic 中，作者要求语言模型生成与给定背景相关的 10 个问题和答案。值得注意的是，模仿模型的得分接近 GPT。这表明，更具体的模型在尝试模仿教师模型的行为时可能会取得有利的结果。

论文中的一个有趣的推论是，使用教师模型对模型进行微调实际上可以帮助减少模仿模型的毒性评分。这对于希望快速发布开源大语言模型的公司非常有用，而无需进行繁琐的过滤器构建任务。公司可以选择训练一个从精心挑选的教师模型中获得的输出，以获得一个良好的起点，而不是手动尝试构建过滤器。

值得一提的是最近发布的 Orca，这是一款由微软研究院开发的模型，它在训练数据中融入了 GPT 的信号。不同之处在于用于模型训练的数据量。Orca 在 500 万示例上进行了微调，而广覆盖的模仿模型则在大约 15.1 万条观察数据上进行了调整。由于我认为大多数受众不会花费 16000 美元来训练大语言模型作为随意实验，我倾向于发表更接近于模仿建模论文的声明，而非 Orca。话虽如此，我们还需要等待更多研究，以确定模仿学习在更广泛任务中成为可行选项所需的最小示例数。

**结论：根据任务的复杂性，尝试用较弱的模型模仿 GPT 或任何复杂模型的输出可能会导致模型性能较差。**

# 上下文学习是你所需的一切吗？

上下文学习（In-Context Learning），或称为少量样本学习（Few Shot Learning），是将任务特定的示例包含在提示中的过程。这种方法对于复杂的语言模型特别适用，因为开源模型尚未具备处理上下文学习所需的灵活性。通常，这种方法可以取得很好的结果，但你是否曾经想过为什么会这样？

Dai 等人[3]的论文探讨了在提示中加载示例与使用相同示例进行微调之间的数学联系。作者展示了提示示例产生的元梯度在推理时的前向传播中得到体现。对于微调，示例实际产生的是真实梯度，用于更新权重。因此，上下文学习似乎达到了与微调类似的结果。为了更深入理解这些发现，我建议阅读该论文，其中详细阐述了数学联系。

尽管上下文学习的方法很棒，但存在一个在精细调整中不明显的限制。如果我们有大量的训练数据，精细调整的模型将通过在训练过程中更新模型的真实梯度来利用所有这些数据。在上下文学习中，我们只能提供有限数量的观察。因此，出现了一个问题：鉴于大量的训练语料库，我们如何利用最相关的示例来实现最佳结果？

解决这个问题的一种方法是使用启发式方法选择示例，幸运的是，LangChain 提供了支持。LangChain 是一个 Python 模块，基本上包含了简化与语言模型工作相关的预构建提示。我们现在关注的 LangChain 工具是 [ExampleSelector](https://python.langchain.com/docs/modules/model_io/prompts/example_selectors/)。

```py
def get_similarity(seq_a: str, seq_b: str) -> Union[float, int]:
    """ 
    Make a similarity heuristic,
    here we use Jaccard similarity or IOU

    seq_a: First sequence to compare
    seq_b: Second sequence to compare

    Returns:
    Similarity score (float or int)
    """
    # Tokenize
    set_a = set(seq_a.split(' '))
    set_b = set(seq_b.split(' ')) 

    # Calculate IOU/Jaccard similarity
    return len(set_a.intersection(set_b)) / len(set_a.union(set_b))

def example_selector(examples: List[str], input: str, examples2use: int) -> List[str]:
    """ 
    Pseudo code for an example selector

    examples: List of training corpus
    input: Target sequence to translate
    examples2use: Number of examples to use

    Returns:
    List of selected examples
    """
    scores = [get_similarity(example, input) for example in examples]
    sorted_idx = [i for i, _ in sorted(enumerate(scores), key=lambda x: x[1], reverse=True)]
    return examples[sorted_idx[:examples2use]]
```

ExampleSelectors 是一种提示操控器，允许我们在推理过程中动态改变使用的示例。有很多启发式方法可以使用。上面我创建了一些 LangChain 的选择器如何工作的伪代码。我使用了输入序列和示例序列之间的 jaccard 相似度。在 LangChain 中还有更多选项，请查看 [这里](https://python.langchain.com/docs/modules/model_io/prompts/example_selectors/)。

采用这种方法有两个主要好处。首先，你可以使你的 LLM 数据高效，通过选择最相关的示例来处理给定的输入。这与为所有观察加载少量静态示例相对。第二个好处是节省成本，如果通过托管服务进行调整的话。写作时，使用精细调整的基础 Davinci 模型的费用为每 1,000 个 tokens $0.12。相比之下，使用 instruct Davinci 的费用为 $0.02，这是一种 500% 的价格增长！这些价格也不包括培训的费用。

需要注意的是，这些价格可能会发生变化，因为 OpenAI 目前尚未使用 LoRa 或适配器，这在一篇现已删除的 [博客文章](https://web.archive.org/web/20230531203946/https://humanloop.com/blog/openai-plans) [5] 中有透露。然而，由于需要为每个用户维护自定义权重，精细调整的模型仍然可能更昂贵。这也不包括上下文中示例的费用。你的团队需要评估从成本和准确性的角度来看，ICL 还是精细调整更适合你的任务。

**要点：使用动态示例加载的上下文学习可能会在没有来自托管服务的额外成本的情况下实现与精细调整相同的结果。**

# 你的任务是否从最终推理之前的一个或多个中间步骤中受益？

假设你试图回答关于长文档的复杂问题。这项任务从根本上要求语言模型具备良好的语言掌握和理解能力。这引出了一个问题：如果我们帮助语言模型将推理过程分解为子任务，类似于人类如何分析文档并按顺序执行任务呢？

![](img/01c095f6e54a62ae6b8ac5a97e01078e.png)

来自 Sun 等 [4] 的图示。

这正是微软研究人员设定的目标，他们的解决方案是 PEARL [4]。PEARL 代表规划和执行行动以推理长文档。整体框架分为三个步骤：

1.  **行动挖掘：** 语言模型首先被提示阅读文档，并提取可能用于回答领域特定问题的行动。为了提取这些行动，语言模型会提供几个示例行动。以下是一个行动可能的示例。

1.  **计划生成：** 在生成一组任务特定行动后，LLM 现在被要求根据问题和上下文生成一个后续的行动列表。LLM 会提供其他任务的计划示例，这有助于构建高质量的计划。有关技术细节的更多信息，请参阅论文。

1.  **计划执行：** 现在模型已经有了计划。我们现在将输入提供给模型并执行计划。

![](img/2ef172c199bd6de8133e08c945179820.png)

来自 Sun 等 [4] 的示例行动。

在各个阶段之间使用了一些中间步骤以确保质量。作者包括一个自我校正步骤，确保计划符合所需格式。还有一个自我精炼步骤，确定计划是否可以在以后作为少量示例使用。

![](img/5787834bfb64a1c7a69c673d9e4c6ce5.png)

来自 Sun 等 [4] 的表格。

在评估中，PEARL 展示了相较于其他 GPT 模型的显著改进，尤其是在处理长文档时。这个过程的关键收获是，在某些情况下，拥有多个步骤可以显著帮助模型。

另一个中间步骤证明有益的场景是当要包含在上下文中的文档数量超过语言模型支持的范围时。当前，OpenAI 使用的注意力机制的规模为 O(n²)，尚无解决方案来克服这一点 [5]。这引起了对将上下文缩减到最小形式的强烈兴趣。

根据你的任务，有处理此问题的方法。例如，如果你的任务完全围绕实体展开，可以提取相关实体及其相关属性。你可以将这种方法视为一种有损压缩，允许你向 LLM 提供更多上下文。这个中间步骤的另一个好处是，你将非结构化数据转换为结构化格式，这使得你能够在没有 LLM 的情况下做出明智的决策。下面的图示来自 Fei 等人[6]展示了这一任务的一个示例。

![](img/65d4b19ef247c9172802e564af8a99fb.png)

图示摘自 Fei 等人[6]

**总结：将任务分解为较小的后续问题可以帮助将较大问题简化为更易管理的部分。你还可以利用这些较小的任务来解决与模型限制相关的瓶颈。**

# 总结思考

这些是关于研究人员在 LLM 性能和效率的新前沿中探索的一些一般想法。这不是微调模型时需要考虑的所有事项的详尽列表，但在考虑这段旅程时是一个很好的起点。

进一步阅读，Hugging Face 关于训练 LLMs 的[这篇文章](https://huggingface.co/blog/trl-peft)非常有趣，是探索本地问题模仿模型的一个好起点。对[LangChain](https://python.langchain.com/en/latest/index.html)有一个具体的理解也是极其有帮助的。尽管大部分库可以为你的用例重写，但主要好处在于，如果其他人为你编写代码，你可以更容易跟上研究进展！

再次总结要点：

1.  **根据任务的复杂性，试图用较弱的模型模仿 GPT 或任何复杂模型的输出可能会导致模型性能差。**

1.  **通过动态示例加载进行上下文学习可能会达到与微调相同的效果，而不会带来额外的管理服务成本。**

1.  **将任务分解为较小的后续问题可以帮助将较大问题简化为更易管理的部分。你还可以利用这些较小的任务来解决与模型限制相关的瓶颈。**

*感谢阅读这篇文章！我的主要兴趣领域是为用户创建个性化互动。请考虑互动文章并关注我在* [*Medium*](https://medium.com/@smsmith714)*！如果你想联系我以讨论文章中的技术错误或保持联系，请通过* [*LinkedIn*](https://www.linkedin.com/in/sms714/)*联系我。*

## 参考文献

[1] Arnav Gudibande, Eric Wallace, Charlie Snell, Xinyang Geng, Hao Liu, Pieter Abbeel, Sergey Levine, & Dawn Song. (2023). 模仿专有 LLMs 的虚假承诺。

[2] Mukherjee, S., Mitra, A., Jawahar, G., Agarwal, S., Palangi, H., & Awadallah, A.（2023）。Orca：从 GPT-4* 复杂解释轨迹中逐步学习。arXiv: 计算与语言*。

[3] Damai Dai, Yutao Sun, Li Dong, Yaru Hao, Shuming Ma, Zhifang Sui, & Furu Wei.（2023）。为什么 GPT 能在上下文中学习？语言模型隐式地作为元优化器执行梯度下降。

[4] Simeng Sun, Yang Liu, Shuohang Wang, Chenguang Zhu, & Mohit Iyyer.（2023）。PEARL：引导大型语言模型规划和执行长文档上的动作。

[5] Habib, R..（2023）。根据 Sam Altman 的说法，OpenAI 的计划。

[6] Hao Fei, Fei Li, Chenliang Li, Shengqiong Wu, Jingye Li 和 Donghong Ji，（2022）。继承前人的智慧：一个用于统一基于方面的情感分析的多层级级联框架
