# 机器学习模型的性能评估技术

> 原文：[https://towardsdatascience.com/performance-estimation-techniques-for-machine-learning-models-aaa83463bfa3?source=collection_archive---------7-----------------------#2023-03-02](https://towardsdatascience.com/performance-estimation-techniques-for-machine-learning-models-aaa83463bfa3?source=collection_archive---------7-----------------------#2023-03-02)

[](https://felipe-p-adachi.medium.com/?source=post_page-----aaa83463bfa3--------------------------------)[![Felipe de Pontes Adachi](../Images/58c9544ae85f43548c5e5b56fda31bb4.png)](https://felipe-p-adachi.medium.com/?source=post_page-----aaa83463bfa3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aaa83463bfa3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----aaa83463bfa3--------------------------------) [Felipe de Pontes Adachi](https://felipe-p-adachi.medium.com/?source=post_page-----aaa83463bfa3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa038269245d5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperformance-estimation-techniques-for-machine-learning-models-aaa83463bfa3&user=Felipe+de+Pontes+Adachi&userId=a038269245d5&source=post_page-a038269245d5----aaa83463bfa3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aaa83463bfa3--------------------------------) ·6分钟阅读·2023年3月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faaa83463bfa3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperformance-estimation-techniques-for-machine-learning-models-aaa83463bfa3&user=Felipe+de+Pontes+Adachi&userId=a038269245d5&source=-----aaa83463bfa3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faaa83463bfa3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fperformance-estimation-techniques-for-machine-learning-models-aaa83463bfa3&source=-----aaa83463bfa3---------------------bookmark_footer-----------)![](../Images/90680db7d0ae093f76c7997e6d1a97ee.png)

图片由 [Isaac Smith](https://unsplash.com/@isaacmsmith?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

一旦你的模型部署完成，监控其性能在确保机器学习系统质量方面扮演着至关重要的角色。为了计算如准确率、精确率、召回率或f1-score等指标，需要标签。然而，在许多情况下，标签可能不可用、部分可用或有延迟。在这些情况下，估计模型性能的能力会很有帮助。

在这篇文章中，我想讨论一些在没有真实数据的情况下估计性能的方法。

# NannyML

[NannyML](https://nannyml.readthedocs.io/en/stable/index.html) 是一个用于检测模型静默失败、在没有标记数据的情况下估计部署后性能和检测数据漂移的Python包。目前，NannyML有两种性能估计方法：**基于置信度的性能估计** (**CBPE**) 和 **直接损失估计** (**DLE**)。有关这些方法的详细描述，请参阅 [NannyML原始文档](https://nannyml.readthedocs.io/en/stable/how_it_works/performance_estimation.html#)。

**a. 基于置信度的性能估计**

正如名称所示，这种方法利用模型预测的置信度分数来进行性能估计。

**注意事项：**使用这种方法时需要注意一些要求和假设。

+   **置信度作为概率：**置信度分数应该代表概率——例如，如果对于一大组观察对象分数为0.9，那么它大约会正确90%的时间。

+   **良好校准的概率：**另一个要求是分数应该经过良好的校准，但这并不总是能做到。好消息是NannyML会在需要时内部进行校准。

+   **没有对之前未见过的空间区域的协变量偏移：**例如，如果你的模型是在10-70岁的人群上训练的，而在生产中你的观察对象是70岁以上的人，这种方法可能无法提供可靠的估计。

+   **没有概念漂移：**如果模型的输入与目标之间的关系发生变化，这种方法可能无法提供可靠的估计（我个人不知晓任何方法能做到这一点）。

+   **对回归模型适配不佳：**回归模型通常不会内在地输出置信度分数，仅提供实际预测，因此这种方法在这些情况下的使用并不简单。

**b. 直接损失估计**

这种方法的直觉是训练一个额外的机器学习模型，其任务是估计监控模型的损失。这个额外模型称为**保姆模型**，监控模型称为**子模型**。

**注意事项：**

+   **额外模型：**需要训练一个额外的模型来估计原始模型的损失，这增加了系统的复杂性。然而，这个模型不必比原始模型更好，在许多情况下，这个过程可以很直接。

+   **适用于回归：**这种方法非常适合回归任务。例如，保姆模型可以被训练来预测均方误差（MSE）或平均绝对误差（MAE）。

+   **没有对之前未见过的空间区域的协变量偏移：**对CBPE的相同考虑也适用于这种方法。

+   **没有概念漂移：**对CBPE的相同考虑也适用于这种方法。

+   **不同性能区域：** 监控的模型在不同区域应该表现出不同的性能。例如，如果你的模型在一天的不同时间段或不同季节的表现有所不同。

# 重要性加权

我首次了解这种方法是通过参加了一个O’Reilly课程，名为[实时机器学习性能监控](https://learning.oreilly.com/live-events/monitor-real-time-machine-learning-performance/0636920075104/0636920075102/)，讲师是[Shreya Shankar](https://twitter.com/sh_reya)。直观上，你可以利用一个你已经有标签的参考数据集来估计未标记目标数据集的性能。这可以是你在部署前阶段使用的数据集，比如你最初训练模型时的测试集。为此，我们首先定义具有明确标准的分段，然后计算每个数据段的性能，比如准确性。这可以是根据年龄、职业或产品类别进行分段。例如，要估计目标数据集的准确性，你需要对数据应用相同的分段规则，并根据目标数据集的分段比例加权原始参考分段的准确性。

我非常喜欢这种方法，因为直观上非常清晰，实施也很简单。

我们在这里考虑的主要原因是参考数据集与目标数据集之间性能差异的原因是输入数据分布的变化。这被称为**协变量漂移**。

**考虑事项：**

+   **无概念漂移：** 如果模型输入与目标之间的关系发生变化，这种方法可能不会提供可靠的估计。再说一次——我不知道任何能做到这一点的方法。

+   **协变量漂移到特征空间未知区域：** 对前述方法的考虑也适用于这种方法。

+   **分段的重要性：** 选择适当的数据分段方式非常重要。目标数据集中的分段必须是参考数据集的子集，因为未见过的分段将没有关联的准确性。分段还应在训练准确性上具有较高的方差：如果所有分段的准确性相同，那么加权它们就没有太大意义。你还应该能够在生产过程中轻松进行分段，而无需手动标记——这正是我们想要避免的！

+   **互斥且穷尽的分段：** 据我所知，这种方法仅适用于互斥且穷尽的分段。这意味着我们的分段彼此不重叠，并且所有分段的总和等于完整的数据集。

# Mandoline

[Mandoline](https://arxiv.org/abs/2107.00643) 是一个用于评估 ML 模型的框架，利用标记的参考数据集和用户的先验知识来估计未标记数据集的性能。这里的关键见解是，用户可以利用他们的理解来创建“切片函数”，这些函数捕捉分布可能变化的轴向。这些函数可以通过编程或使用元数据来对数据进行分组。

与前一节中介绍的方法有相似之处，因为两者都使用了类似的桶/切片/段的概念，并且都利用这些分组来重新加权源数据。在 Mandoline 中，创建的切片将帮助指导后续的密度估计过程，这些估计结果然后用于重新加权源数据集，并输出性能估计。

![](../Images/8643e31a6e64f49d9b9423de9da0207f.png)

Mandoline: 分布转移下的模型评估 — [https://arxiv.org/abs/2107.00643](https://arxiv.org/abs/2107.00643)

[这篇论文](https://arxiv.org/abs/2107.00643)非常有趣，值得一读，结果看起来也很有前景。他们还提供了一个[框架的 Python 实现](https://github.com/HazyResearch/mandoline)，我计划在未来的文章中深入探讨。

**考虑因素**

+   **没有概念漂移：** 再次强调，问题的表述假设分布之间没有概念漂移。

+   **适用于噪声较大、未指定的切片：** 即使你的切片存在噪声和/或未指定的情况，这种方法仍然有效。举个例子，如论文中所述 — 使用 [CivilComments 数据集](https://wilds.stanford.edu/datasets/#civilcomments) 检测毒性，并且你想根据人口统计信息进行分段，比如男性、女性、基督教徒、LGBTQ 等。你可以使用正则表达式函数来查找关键词，如“man, male, female”来对数据进行分组。这种方法不会每次都准确地获得切片，但即便如此，这种方法效果很好。此外，据我了解，这种方法适用于重叠的段 — 在这种情况下，每个评论可能提到一个或多个人口统计信息。

+   **切片的重要性：** 设计切片函数时，准确性至关重要。它们应与当前任务相关，并能有效捕捉分布中可能发生的不同轴向的变化。

# 未来实验

在未来的帖子中，我计划在实际应用案例中实验每种方法。目标不是比较每种方法的性能，而是简单展示我们如何在实际场景中使用这些方法。由于每种方法有不同的需求集，我们可能会使用不同的数据集和用例。例如，Mandoline 支持使用重叠片段。另一方面，Importance Weighting 方法适用于互斥且全面的分箱。CBPE 和 DLE 方法都不假设任何形式的分割，但我们确实需要一个具有良好校准的置信度分数的模型（CBPE），或者训练一个额外的模型（DLE）。

感谢阅读！

*“*[*机器学习模型的性能估计技术*](https://datatravelogues.substack.com/p/performance-estimation-pt-1)*”最初发表于* [*作者的个人通讯*](https://datatravelogues.substack.com/)*。
