# 朝向绿色 AI：如何在生产中提高深度学习模型的效率

> 原文：[`towardsdatascience.com/towards-green-ai-how-to-make-deep-learning-models-more-efficient-in-production-3b1e7430a14`](https://towardsdatascience.com/towards-green-ai-how-to-make-deep-learning-models-more-efficient-in-production-3b1e7430a14)

## The Kaggle Blueprints

## 从学术界到工业界：在机器学习实践中寻找预测性能与推理运行时间之间的最佳权衡以实现可持续性

[](https://medium.com/@iamleonie?source=post_page-----3b1e7430a14--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----3b1e7430a14--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3b1e7430a14--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b1e7430a14--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----3b1e7430a14--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b1e7430a14--------------------------------) ·14 分钟阅读·2023 年 8 月 8 日

--

![](img/2c688f3626d020d10afe04975cc8d466.png)

在 GPU 篝火中制作 s’mEARTHs（图片由作者手绘）

*本文最初发表于* [*Kaggle*](https://www.kaggle.com/code/iamleonie/towards-green-ai) *，作为* [*“2023 Kaggle AI Report” 竞赛*](https://www.kaggle.com/competitions/2023-kaggle-ai-report) *的参赛作品，* [*在“ Kaggle 竞赛”类别中获得第 1 名*](https://www.kaggle.com/competitions/2023-kaggle-ai-report/discussion/429989)*。由于它回顾了 Kaggle 竞赛的文章，因此它是“**The Kaggle Blueprints**”系列的特别版。*

# 介绍

“我认为我们已经进入了一个不再依赖这些巨大的模型的时代。[…] 我们将通过其他方式让它们变得更好。”， [OpenAI 首席执行官 Sam Altman](https://www.wired.com/story/openai-ceo-sam-altman-the-age-of-giant-ai-models-is-already-over/) 在 GPT-4 发布后不久说道。这一声明让许多人感到惊讶，因为 GPT-4 的规模预计比前身 GPT-3 大十倍（[1.76 万亿参数](https://wandb.ai/byyoung3/ml-news/reports/AI-Expert-Speculates-on-GPT-4-Architecture---Vmlldzo0NzA0Nzg4)），而 GPT-3 具有 [1750 亿参数](https://arxiv.org/abs/2005.14165)。

> “我认为我们已经到了这样一个时代，即这些巨大的模型将会结束。[……] 我们会通过其他方式使它们变得更好。” — [Sam Altman](https://www.wired.com/story/openai-ceo-sam-altman-the-age-of-giant-ai-models-is-already-over/)

2019 年，Strubell 等人[1]估计，训练一个自然语言处理（NLP）管道，包括调优和实验，产生约 35 吨的二氧化碳当量，超过了美国公民年均消费的两倍。

让我们更具体地来看一下：据报道，[2019 年信息技术产生了全球 3.7%的 CO2 排放](https://theshiftproject.org/en/article/lean-ict-our-new-report/)。这比[全球航空（1.9%）和航运（1.7%）](https://ourworldindata.org/emissions-by-sector)的排放总和还要多！

深度学习模型在不同领域推动了最先进的表现。这些性能提升通常是更大模型的结果。但构建更大的模型需要在训练和推理阶段进行更多计算。而更多的计算需要更大的硬件和更多的电力，从而排放更多的 CO2，导致更大的碳足迹，这对环境不利。

Strubell 等人[1]的震撼论文促成了 2020 年“绿色人工智能（Green AI）”这一新研究领域的诞生。这个术语由 Schwartz 等人[2]提出，用来描述“在不增加计算成本的情况下产生新结果的研究，理想情况下还要减少计算成本”。自那时起，许多领域的论文涌现出来，旨在减少人工智能，尤其是深度学习模型的碳足迹。

![](img/326a38296360c389199f719b73c9c4e3.png)

为了减少深度学习模型的碳足迹，[Kaggle](https://www.kaggle.com/)这一个数据科学竞赛平台，在他们的一些比赛中引入了“效率奖”：

> 我们正在主办一个第二个专题，专注于模型效率，因为高精度模型通常计算负担较重。这些模型具有更强的碳足迹，并且在实际世界的[…]环境中通常难以使用。

尽管碳足迹在深度学习模型的整个生命周期中产生，但 Kaggle 无法影响每个阶段参赛者的行动。但是，通过在运行时间和预测性能上评估提交，Kaggle 可以鼓励参赛者构建更高效的解决方案，以减少碳足迹，至少在推理阶段。

![](img/fecf54d67a319440ab6ba293713a2319.png)

今年年初，发布了一项调查，声称绿色人工智能（Green AI）研究领域已经达到成熟水平，现在是将众多有前景的学术成果转化为工业实践以在实验室环境之外评估技术的时候了[7]。

虽然 Kaggle 不能直接作为行业实践的代理，但 Kaggle 是在实验室环境之外测试新技术的理想场所。因此，本文的主题是：

**在过去两年中，社区在平衡 Kaggle 竞赛中的模型性能和推理时间方面学到了什么，以减少生产中深度学习模型的碳足迹？**

我们将首先回顾“绿色 AI”文献中的有前景的学术成果。然后，我们将审查这些成果中的哪些已经被 Kaggle 社区采纳并取得成功，通过查看效率奖获胜者的文章来进行分析。

# 背景

碳足迹在整个机器学习操作（MLOps）生命周期中产生。自 2021 年以来，一些调查 [3, 4, 5, 6, 7] 已按 MLOps 阶段对减少碳足迹的技术进行了分类。尽管各阶段略有不同，但主要的三个步骤是模型设计、开发和推理 [3, 4, 5, 6]。其他分类包括数据存储和使用 [3, 5, 6] 或硬件 [4]。

![](img/abf1c0ef88cdbbc11177f49aaa7b3e29.png)

在 2019 年，AWS [21] 和 NVIDIA [22] 都估计大约 90% 的机器学习工作负载是推理。在“效率奖”的背景下，本文的主题集中在提升推理阶段的效率。

请注意，模型设计也会显著影响推理阶段的效率。然而，模型设计可能会针对特定用例，因此需要更多数据来有效分析哪些模型设计技术可以帮助减少推理阶段的碳足迹。因此，我们将首先关注推理阶段的模型无关技术，以保持分析简洁。鼓励未来的工作在效率奖被引入更广泛的竞赛范围时分析模型设计（见 讨论）。

Xu 等人 [3] 在 2021 年提出的分类法也已被采用，并在最近的调查 [6] 中使用。因此，我们将根据这一分类法来结构化我们的分析。以下技术旨在减少模型大小以提高延迟，因为模型大小直接影响延迟：

+   **剪枝**通过去除冗余元素（如权重、滤波器、通道甚至层）来减少神经网络的大小，而不损失性能。它首次由 LeCun 等人 [8] 于 1989 年提出。

+   **低秩分解**通过将权重矩阵分解为两个低维矩阵（矩阵/张量分解）来减少卷积层或全连接层的复杂性。

+   **量化**通过降低权重和激活值的精度（通常从 32 位浮点值降到 8 位无符号整数）来减少模型的大小。这种精度的降低可能会导致轻微的性能损失。你可以在训练期间或训练后对模型进行量化。量化有静态和动态之分。对于本文，你只需要知道动态量化比静态量化更为准确。然而，动态量化也比静态量化需要更多的计算。

+   **知识蒸馏**是一种技术，通过使用从大型高性能模型/集合（教师网络）中伪标记的额外数据来训练较小的神经网络（学生网络），从而将知识蒸馏到紧凑的神经网络中。这些伪标签也称为软标签，包含所谓的“黑暗知识”，这有助于学生网络学习模仿教师网络。这个想法由 Hinton 等人在 2015 年提出[9]。

# 方法论与数据收集

为了评估哪些有前途的学术成果已经在实验室环境外进行过尝试和验证，我们将回顾带有效率奖的 Kaggle 比赛的解决方案报告。

不幸的是，Kaggle 原始比赛数据集的写作内容并未涵盖所有的效率相关报告，因为一些团队为效率奖写了单独的报告。因此，我们决定为此次比赛创建一个自定义数据集。

1.  确定所有带有效率奖的 Kaggle 比赛。

1.  根据[效率排行榜笔记本](https://www.kaggle.com/code/ryanholbrook/curriculum-recommendation-efficiency-leaderboard)确定效率奖的前 10 支团队。

1.  通过搜索讨论论坛和排行榜，手动收集每个顶级效率解决方案的报告。如果一个团队写了两份报告，请选择效率奖的那一份（例如，来自同一团队的[标准排行榜报告](https://www.kaggle.com/competitions/feedback-prize-effectiveness/discussion/347536)与[效率排行榜报告](https://www.kaggle.com/competitions/feedback-prize-effectiveness/discussion/347537)）。

在 50 个前 10 名的效率解决方案中，我们能够收集到**25 份可用报告**。

![](img/e8b253caf20d534bc1b66ef6515cf311.png)

这些报告分布在以下五场比赛中。

![](img/67d0efb11c307e030f7f62b38fba1e0a.png)

# 结果

本节回顾了 Kaggle 效率奖的顶级解决方案中收集的报告，查看是否使用了包括剪枝、低秩分解、量化和知识蒸馏在内的减少推理成本的技术，以及这些技术是否成功。

对于每一种技术，我们手动审查了这 25 份报告，以回答以下问题：

1.  竞争者们对这种技术进行了实验吗？它是如何应用的？

1.  该技术是否有效？

1.  是否有任何迹象表明该技术为何有效/无效？

## 剪枝

在 25 篇顶级效率报告中，只有一篇[20]提到了剪枝。

![](img/44a32a5f8df42b06f600596e0971ddcf.png)

然而，他们无法报告任何成功的案例[20]：

> 开始探索剪枝以及硬量化显示出性能损失会很显著（在生产中可以接受，但在竞赛中则不行），因此我们选择了简单的 TF Lite 转换。

这篇报告的有趣之处在于它提到，发现的性能损失在生产中是可以接受的，但在此竞赛设置中则不然，这表明，权衡是否良好的标准在 Kaggle 竞赛和工业界之间可能有所不同（见讨论）。

## 低秩分解

25 篇顶级效率报告中没有提到低秩分解。

![](img/8c6e91e5a278817d6166063f89881670.png)

由于报告中没有提到，我们不知道是否没有竞争者尝试过，或者是否有竞争者尝试过但效果不佳。

由于低秩分解在 Feedback Prize——英语语言学习竞赛的[讨论帖](https://www.kaggle.com/competitions/feedback-prize-english-language-learning/discussion/365851)中被提到，我们可以假设一些竞争者知道这一技术。然而，最近的调查[3]已经得出结论，低秩分解在计算上复杂，减少计算成本和推理时间的效果不如其他压缩方法。

## 量化

在 Feedback Prize——英语语言学习竞赛之前没有提到量化。但在后者中，超过一半的报告提到过量化，但报告显示效果不佳。虽然在学习平等——课程推荐竞赛中仅有一篇报告提到量化，但报告称在该竞赛中效果良好。

![](img/7e9e60152ea8a35238d9576b6f62f63c.png)

量化首次出现在 Feedback Prize——英语语言学习竞赛中。然而，提到量化的报告表示它没有效果。两篇报告[13, 17]特别提到他们尝试了动态量化，导致性能损失且没有运行时改进：

> 由于量化`nn.Linear`层直接影响模型的输出，因此量化层不适合回归任务。

在学习平等——课程推荐比赛中，一位竞争者报告说，训练后动态量化成功，虽然性能略有下降，但推理速度提高了[19]：

> 如果在变换器的 Feed Forward 部分的中间上采样和输出层也使用`qint8`，得分下降会更大，因此我最终只在注意力层使用了`qint8`。

根据参赛者的解决方案[19]，我们可以看到量化单独的层几乎和量化整个神经网络一样容易。

```py
# Code snippet taken from <https://github.com/KonradHabel/learning_equality/blob/master/eval_cpu.py>
from torch.quantization import quantize_dynamic

model.transformer.embeddings = quantize_dynamic(model.transformer.embeddings, None, dtype=torch.float16)

for i in range(config.num_hidden_layers):
    model.transformer.encoder.layer[i].attention = quantize_dynamic(model.transformer.encoder.layer[i].attention, None, dtype=torch.qint8)
    model.transformer.encoder.layer[i].intermediate = quantize_dynamic(model.transformer.encoder.layer[i].intermediate, None, dtype=torch.float16)
    model.transformer.encoder.layer[i].output = quantize_dynamic(model.transformer.encoder.layer[i].output, None, dtype=torch.float16)
```

因此，量化的成功取决于神经网络中哪些层被量化。

## 知识蒸馏

在 25 篇总结中，知识蒸馏在五场竞赛中的三场中提到了九次。

*注意：知识蒸馏通常与伪标签一起提及。由于许多参赛者使用伪标签重新训练现有模型，我们仅关注那些明确提到使用伪标签训练新小模型的总结。*

![](img/fa3645ca6494f205c0b77bfd97133ace.png)

有趣的是，所有提到知识蒸馏的总结都报告了其有效性。许多参赛者甚至报告知识蒸馏对效率奖[13, 14, 16, 17]具有很高的影响，仅有最小的性能损失[10, 16, 19]。

一位参赛者描述了他们的知识蒸馏过程，并成功应用于反馈奖——有效论点预测[10]和反馈奖——英语语言学习竞赛[15]，如下所述[10]：

> 1\. 我们通过使用我们的大型集成模型为之前的反馈竞赛生成伪标签数据[…]。
> 
> 2\. 我们还为给定的训练数据生成了折外伪标签
> 
> 3\. 现在我们将这两个数据集与软伪标签结合在一起，并训练一个没有任何原始标签的新模型

尽管其实现简单，但我们需要注意知识蒸馏依赖于额外数据的可用性。许多参赛者[10, 13, 15]提到在反馈奖竞赛系列中从之前的竞赛中整理数据集以进行伪标签化。

有趣的是，知识蒸馏在自然语言处理竞赛中成功应用，但在计算机视觉和表格数据竞赛中却失败了。然而，它们也没有类似的竞赛数据作为额外数据。

# 讨论

本节讨论了社区对在 Kaggle 竞赛中平衡模型性能和推理时间的理解，以减少深度学习模型在生产中的碳足迹，特别是我们讨论了这些有前景的学术成果在实验室环境之外是否具有实用性。

## Kaggle 竞赛中的效率奖对更广泛的机器学习社区有什么影响？机器学习社区是否从中受益？

Verdecchia 等人[7]声称绿色人工智能研究领域已达到成熟水平，现在是“将大量有前景的学术成果移植到工业实践中”的时候，以评估其在实验室环境之外的实用性。

为此，我们回顾了获得效率奖的 Kaggle 比赛的解决方案总结。我们看到效率奖鼓励参赛者尝试不同技术，以减少推理阶段的碳排放。

分析表明，许多学术技术在 Kaggle 社区中得到了尝试。Kaggle 社区无法确认剪枝和低秩分解是实现效率与性能良好平衡的实用技术。然而，量化和知识蒸馏的细致应用被证明是实际可行的，因为它们简单易用且效果显著。

## 这些比赛是否推动了机器学习领域的任何重要进展？

尽管这项分析的重点是评估剪枝、低秩分解、量化和知识蒸馏这四种技术，但我们遇到了竞争者发现有效的不同技术，例如将模型转换为 ONNX 格式[15, 16, 18]，在不牺牲预测性能的情况下减少运行时间。

因此，接下来的步骤是从相反的角度分析比赛总结，看看哪些技术在 Kaggle 社区中被确立为提高深度学习模型推理速度的有效技术。

## 这些比赛的影响有哪些限制？

碳足迹在整个 MLOps 生命周期中产生。在这项分析中，我们特别关注了文献中归类于推理阶段的技术。然而，从我们设计模型的起点开始已经影响了推理时的碳排放，这也应在未来的工作中加以分析。

此外，尽管效果已被证明有效，知识蒸馏仍需训练一个大规模的教师网络，然后将知识蒸馏到较小的网络中。因此，虽然知识蒸馏减少了推理阶段的碳排放，但必须指出，这项技术在训练阶段会产生额外的碳排放。

因此，虽然效率奖有助于评估减少推理时碳足迹的技术，但我们需要一种更全面的方法来鼓励参赛者在比赛过程中减少碳排放，以迈向绿色 AI。

# 结论

本次分析旨在了解在 MLOps 生命周期的推理阶段减少碳足迹的学术上有前景的技术是否在获得效率奖的 Kaggle 比赛中得到了有效应用。

为此，我们首先回顾了现有文献，并发现最近的调查将减少推理阶段碳排放的技术分为四类：剪枝、低秩分解、量化和知识蒸馏。然后，我们分析了效率奖总结的自定义数据集。

我们发现 Kaggle 社区尝试了许多学术提案：

+   剪枝被报告为在实现令人满意的折中方面无效。

+   低秩分解在前 10 篇写作中没有提到。我们假设这一技术可能在实际应用中更具计算复杂性。

+   量化仅在一个案例中成功报告，其中竞争者没有量化整个模型，而是策划了哪些层进行量化以及量化的程度。

+   知识蒸馏在有类似竞赛的额外数据可用时，已被证明对自然语言处理竞赛有效。

我们得出结论，Kaggle 社区帮助评估了绿色人工智能文献中有前景的学术成果，以其实际可行性快速脱离实验室设置。

因此，**Kaggle 应该继续推进效率奖**，以迈向绿色人工智能。作为下一步，Kaggle 可以**将其添加到所有竞赛中**，理想情况下，甚至可以不再作为单独的赛道，而**作为主要指标**。

# 喜欢这个故事吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时收到通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----3b1e7430a14--------------------------------) [## 每当 Leonie Monigatti 发布时获取电子邮件。

### 每当 Leonie Monigatti 发布时获取电子邮件。通过注册，如果您还没有 Medium 账户，将会创建一个…

medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----3b1e7430a14--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)，[*Twitter*](https://twitter.com/helloiamleonie)*和* [*Kaggle*](https://www.kaggle.com/iamleonie)*上找到我！*

# 参考文献

## 数据集

[A] arXiv.org 提交者。（2023）。[arXiv 数据集](https://www.kaggle.com/datasets/Cornell-University/arxiv)。Kaggle。 [`doi.org/10.34740/KAGGLE/DSV/6141267`](https://doi.org/10.34740/KAGGLE/DSV/6141267)

许可：[CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/)

[B] iamleonie（2023）。[Kaggle 效率写作](https://www.kaggle.com/datasets/iamleonie/kaggle-efficiency-writeups) 在 Kaggle 数据集中。

许可： [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

## 图像参考

除非另有说明，所有图像均由作者创建。有关代码，请参见[Kaggle 笔记本](https://www.kaggle.com/code/iamleonie/towards-green-ai)。

# 参考文献

## 文献

[1] 斯特拉贝尔，E.，加奈，A.，& 麦考勒姆，A.（2019）。深度学习在自然语言处理中的能源和政策考虑。*arXiv 预印本 arXiv:1906.02243*。

[2] 施瓦茨，R.，道奇，J.，史密斯，N. A.，& 埃提奥尼，O.（2020）。绿色人工智能。*ACM 通讯*，*63*(12)，54–63。

[3] 许，J.，周，W.，傅，Z.，周，H.，& 李，L.（2021）。绿色深度学习的调查。*arXiv 预印本 arXiv:2111.05193*。

[4] 孟哈尼，G.（2021）。高效深度学习：关于让深度学习模型更小、更快、更好的调查。*ACM 计算调查*，*55*(12)，1–37。

[5] Wu, C. J., 等（2022）。 可持续人工智能：环境影响、挑战与机遇。 *机器学习与系统会议论文集*，*4*，795–813。

[6] Mehlin, V., Schacht, S., & Lanquillon, C.（2023）。 朝着节能深度学习的方向：深度学习生命周期中节能方法的概述。 *arXiv 预印本 arXiv:2303.01980*。

[7] Verdecchia, R., Sallou, J., & Cruz, L.（2023）。 绿色人工智能的系统综述。 *Wiley 跨学科评论：数据挖掘与知识发现*，e1507。

[8] LeCun, Y., Denker, J., & Solla, S.（1989）。 最优脑损伤。 神经信息处理系统进展，2。

[9] Hinton, G., Vinyals, O., & Dean, J.（2015）。 提炼神经网络中的知识。 *arXiv 预印本 arXiv:1503.02531*。

## Kaggle 写作

[10] 氢团队（2022）。 [Feedback Prize 预测有效论点中的第 1 名分析](https://www.kaggle.com/competitions/feedback-prize-effectiveness/discussion/347537)

[11] 你现在看到了我（2022）。 [Feedback Prize 预测有效论点中的第 2 名效率分析](https://wandb.ai/darek/fbck/reports/How-To-Build-an-Efficient-NLP-Model--VmlldzoyNTE5MDEx?accessToken=pmm41mpdkxif0lsbm927tfxj947to4gbd0nrgjjq9rdoq1c4jr3kruf993ys5kpg)

[12] Darjeeling Tea（2022）。 [Feedback Prize 预测有效论点中的第 5 名效率分析](https://www.kaggle.com/competitions/feedback-prize-effectiveness/discussion/347433)

[13] 图灵团队（2022）。 [Feedback Prize 英语语言学习中的第 1 名效率分析](https://www.kaggle.com/competitions/feedback-prize-english-language-learning/discussion/369646)

[14] 用尽创意💨（2022）。 [Feedback Prize 英语语言学习中的第 2 名效率分析](https://www.kaggle.com/competitions/feedback-prize-english-language-learning/discussion/369623)

[15] Psi（2022）。 [Feedback Prize 英语语言学习中的第 5 名效率分析](https://www.kaggle.com/competitions/feedback-prize-english-language-learning/discussion/370020)

[16] william.wu（2022）。 [Feedback Prize 英语语言学习中的第 6 名效率分析](https://www.kaggle.com/competitions/feedback-prize-english-language-learning/discussion/369587)

[17] ktm（2022）。 [Feedback Prize 英语语言学习中的第 7 名效率分析](https://www.kaggle.com/competitions/feedback-prize-english-language-learning/discussion/369540)

[18] Shobhit Upadhyaya（2022）。 [Feedback Prize 英语语言学习中的第 9 名效率分析](https://www.kaggle.com/competitions/feedback-prize-english-language-learning/discussion/370046)

[19] Konni（2023）。 [Learning Equality 课程推荐中的第 2 名效率分析](https://www.kaggle.com/competitions/learning-equality-curriculum-recommendations/discussion/395110)

[20] French Touch (2023). [在游戏玩法中预测学生表现的第五名效率总结](https://www.kaggle.com/competitions/learning-equality-curriculum-recommendations/discussion/395110)

## Web

[21] Barr, J. (2019). [Amazon EC2 更新 — Inf1 实例配备 AWS Inferentia 芯片用于高性能且具有成本效益的推理](https://aws.amazon.com/de/blogs/aws/amazon-ec2-update-inf1-instances-with-aws-inferentia-chips-for-high-performance-cost-effective-inferencing/) 见于 AWS News Blog（访问于 2023 年 7 月 16 日）

[22] Leopold, G. (2019). [AWS 提供 Nvidia 的 T4 GPU 用于 AI 推理](https://www.hpcwire.com/2019/03/19/aws-upgrades-its-gpu-backed-ai-inference-platform/) 见于 HPC Wire（访问于 2023 年 7 月 16 日）
