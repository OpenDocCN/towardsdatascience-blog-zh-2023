# 如何创建有价值的数据测试

> 原文：[`towardsdatascience.com/how-to-create-valuable-data-tests-850e778718e1`](https://towardsdatascience.com/how-to-create-valuable-data-tests-850e778718e1)

## 重要的不是数量，而是质量。

[](https://medium.com/@xiaoxugao?source=post_page-----850e778718e1--------------------------------)![Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----850e778718e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----850e778718e1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----850e778718e1--------------------------------) [Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----850e778718e1--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----850e778718e1--------------------------------) ·9 分钟阅读·2023 年 7 月 3 日

--

![](img/a6433b06eebe7358fd99a93d34ff62f8.png)

照片由 [Shubham Dhage](https://unsplash.com/@theshubhamdhage) 提供，来源于 [Unsplash](https://unsplash.com/)

在过去的一年中，数据质量已经被广泛讨论。数据合同、数据产品和数据可观测性工具的日益采用无疑显示了数据从业者对向消费者提供高质量数据的承诺。我们都喜欢看到这一点！

数据解决方案中的一个关键组成部分是数据测试。这是验证数据质量的最基本和实用的方法之一，并且明确或隐含地嵌入在许多数据解决方案中。

尽管其有效性为数据团队带来了显著的好处，但它也提出了如何最大化其潜在价值的问题，因为更多的测试并不一定意味着更高的数据质量。在本文中，我希望展示一些设计数据测试的方法。希望这些方法能够提供一些启示。

> 值得注意的是，建议你结合这些方法，找到最适合自己的平衡点。

## 质量 > 数量

![](img/50cbde299cf98bb64a915f7b5a14d136.png)

质量 > 数量（作者创建）

我是那些喜欢创建测试的人之一，因为它们让我对我的解决方案更加自信。作为一名软件工程背景的从业者，我曾经信奉“测试越多越好”的座右铭。我总是对数据框架提供简单的数据测试创建方法感到兴奋。

然而，我低估了过多的数据测试所带来的副作用。（真的会有副作用吗？有的！）首先，让我们理解数据测试和单元测试（即逻辑测试）之间的区别。简而言之，单元测试旨在验证我们编写的代码逻辑的正确性。单元测试越多，我们对处理边界情况的信心就越强。但数据测试不仅限于代码逻辑，它还检查源数据的质量、数据管道配置、上游依赖关系等等。度量标准无穷无尽，可能会令人不知所措。创建大量测试以防万一是很诱人的，但这些测试不一定带来价值，可能会引入不必要的噪声。例如，面对现实吧，如果某些数据管道只偶尔被用户使用，那它们不需要每日的新鲜度测试，并且数据管道中的所有阶段也不需要相同的测试。重复的数据测试只会导致冗余的警报。

曾经，我在创建数据测试时迷失了方向，结果产生了许多冗余测试，导致了错误的警报。**我认识到，为了确保数据质量，我们首先需要确保数据测试的质量。** 数量不一定与质量成正比。

## 适用性

![](img/dbb105199c04b434c3b2eaa313b13ccf.png)

捕捉客户的全部声音（作者创建）

数据可以视为一种产品。数据管道可以被看作是数据制造系统，接收原始数据输入并生产数据产品。虽然数据消费者不购买数据，但他们可以决定是否使用这些数据，并相应地提供需求和反馈。

“适用性”这一概念在衡量质量时被广泛采用。它强调了**捕捉客户的全部声音**的重要性，因为最终是消费者决定产品的成功。例如，在建设大学图书馆时，必须考虑到学生和教师，这意味着图书馆应有一系列覆盖多样兴趣和教育需求的书籍和资源。

在数据产品方面，一个好的例子是报告数据。为了被认为有价值，工程师应该与利益相关者紧密合作，了解有关准确性、可靠性、完整性、及时性等方面的监管要求，然后据此创建数据测试。

这种方法是测试创建过程的良好开端，尤其是当数据使用者对其需求有清晰的愿景，并且对数据质量有良好理解时。这可以大大简化数据工程师的工作。然而，单纯依赖利益相关者的需求往往是不够的，因为他们从外部视角评估数据，而没有考虑上游依赖关系和技术错误。在这种情况下，下一节中的数据质量框架将帮助我们覆盖大多数场景。

## 数据质量维度

从消费者的角度来看数据质量无疑是一个有价值的初步步骤。但它可能无法涵盖测试范围的完整性。大量文献回顾已经为我们解决了这个问题，提供了[一系列数据质量维度](https://dl.acm.org/doi/pdf/10.1145/240455.240479)，这些维度与大多数使用案例相关。建议与数据消费者一起审查列表，共同确定适用的维度，并相应创建测试。

```py
| Accuracy     | Format           | Comparability     |
| Reliability  | Interpretability | Conciseness       |
| Timeliness   | Content          | Freedom from bias |
| Relevance    | Efficiency       | Informativeness   |
| Completeness | Importance       | Level of detail   |
| Currency     | Sufficiency      | Quantitativeness  |
| Consistency  | Usableness       | Scope             |
| Flexibility  | Usefulness       | Understandability |
| Precision    | Clarity          |                   |
```

你可能会发现这个列表太长，不知道从何开始。数据产品或任何信息系统可以从两个角度观察或分析：外部视角和内部视角。

***外部视角***

![](img/52918b43295ed070325b0cea5f1198cc.png)

外部视角的维度（由作者创建）

外部视角关注数据的使用及其与组织的关系。它通常被视为一个“黑箱”，具有表示现实世界系统的功能。属于外部视角的维度高度依赖业务驱动。有时，对这些维度的评估可能是主观的，因此不容易为其创建自动化测试。但让我们看看一些知名的维度：

+   **相关性：** 数据在分析中的适用性和有用性。例如，考虑一个旨在推广新产品的市场活动。所有数据属性应直接促进活动的成功，例如客户人口统计数据和购买数据。像城市天气或股票市场价格这样的数据在这种情况下是无关的。另一个例子是细节层级（粒度）。如果业务希望市场数据按天级别提供，但实际以周级别提供，那么它就不相关也不实用。

+   **表示层：** 数据对数据消费者的可解释程度以及数据格式的一致性和描述性。在访问数据质量时，表示层的重要性往往被忽视。它包括数据的格式——保持一致且用户友好，以及数据的意义——易于理解。例如，考虑一个场景，其中数据预计以 CSV 文件的形式提供，列描述是详尽的，且值预计使用 EUR 货币而非分。

+   **时效性：** 数据对数据消费者的新鲜程度。例如，业务需要销售交易数据，其延迟时间不得超过 1 小时。这表明数据管道应频繁刷新。

+   **准确性：** 数据遵守业务规则的程度。数据指标通常与复杂的业务规则相关，如数据映射、四舍五入模式等。强烈建议对数据逻辑进行自动化测试，越多越好。

在四个维度中，创建数据测试时，及时性和准确性更为直接。及时性通过将时间戳列与当前时间戳进行比较来实现。准确性测试可以通过客户查询来进行。

***内部视角***

![](img/bb14c702bc68ab045979558b81807d50.png)

内部视角的维度（由作者创建）

相对而言，内部视角关注的是独立于特定需求的操作。无论当前的使用案例是什么，这些操作都是至关重要的。内部视角的维度更具技术驱动性，而外部视角的维度则更具业务驱动性。这也意味着数据测试对消费者的依赖较小，大多数情况下可以实现自动化。以下是一些关键视角：

+   **数据源质量：** 数据源的质量对最终数据的整体质量有显著影响。数据合同是确保源数据质量的一个很好的措施。作为源数据的消费者，我们可以采用类似的方法来监控源数据，就像数据利益相关者在评估数据产品时一样。

+   **完整性：** 信息保留的程度。随着数据管道复杂性的增加，中间阶段发生信息丢失的可能性也会增加。我们可以考虑一个存储客户交易数据的金融系统。完整性测试确保所有交易成功经过整个生命周期，而不会被遗漏或遗漏。例如，最终账户余额应该准确反映实际情况，捕捉每一笔交易而没有遗漏。

+   **唯一性：** 这个维度与完整性测试密切相关。虽然完整性保证了没有信息丢失，但唯一性确保了数据中没有重复。

+   **一致性：** 数据在内部系统中每天的一致性。差异是一种常见的数据问题，通常源于数据孤岛或不一致的指标计算方法。另一个一致性问题的方面发生在数据预期有稳定增长模式的日期之间。任何偏差都应引起进一步调查的警报。

值得注意的是，每个维度可以与一个或多个数据测试相关联。关键在于理解维度在特定表格或指标上的适用性。只有这样，测试使用得越多，效果越好。

到目前为止，我们已经讨论了外部视角和内部视角的维度。在未来的数据测试设计中，考虑外部和内部视角都很重要。通过向合适的人提出正确的问题，我们可以提高效率并减少沟通误差。

## 数据测试的更多提示

在最后一节中，我想分享一些创建数据测试的实用技巧。这些技巧来源于我的日常工作，但你也可以在评论中分享更多。

+   **错误数据与无数据：** 在许多数据解决方案中，数据测试通常是在模型更新后进行的。这意味着当我们识别到问题时，数据已经变得“损坏”。如果你更倾向于“无数据”而非“错误数据”，你可以首先将表格生成到临时位置，然后进行测试。只有当测试成功通过时，管道才会继续将表格复制到原始目标；否则，过程将被中止。

+   **对账测试：** 对账测试是一种数据验证过程，用于比较两个或多个系统之间的数据一致性和准确性，通常是在源数据集和目标数据集之间。例如，设计了两个管道来处理事务时，比较两个系统和源的总交易金额是值得的。任何差异的存在可能表明数据管道中存在缺陷。

+   **带有容差的测试：** 我们可能会从利益相关者那里收到这样的陈述：“这个列中的 0 是可以接受的，但应避免过量。”这意味着他们想捕捉到偏离一致模式的情况。许多现代数据监控工具提供异常检测功能，但如果这对你来说不是一个选项，你可以开始创建带有容差的测试。例如，总行数中不应有超过 5%的值为 0。

## 结论

一如既往，我希望你觉得这篇文章有用且启发。关键是避免过分关注你创建的数据测试的数量。因为在每一列上添加相同的测试没有意义，因为这只会制造大量噪音并降低你的生产力。

在与数据消费者和数据提供者交谈之前，务必考虑数据质量框架。聪明地提问，并利用框架激发利益相关者考虑数据的额外视角。干杯！

## 参考

+   [将数据质量维度锚定在本体论基础上](https://dl.acm.org/doi/pdf/10.1145/240455.240479)

+   [超越准确性：数据质量对数据消费者的意义](http://mitiq.mit.edu/documents/publications/tdqmpub/14_beyond_accuracy.pdf)
