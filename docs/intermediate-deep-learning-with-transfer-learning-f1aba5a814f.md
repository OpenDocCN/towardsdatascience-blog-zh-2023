# 中级深度学习与迁移学习

> 原文：[`towardsdatascience.com/intermediate-deep-learning-with-transfer-learning-f1aba5a814f`](https://towardsdatascience.com/intermediate-deep-learning-with-transfer-learning-f1aba5a814f)

## 实用指南，用于调整深度学习模型以进行计算机视觉和自然语言处理

[](https://medium.com/@iamleonie?source=post_page-----f1aba5a814f--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----f1aba5a814f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f1aba5a814f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1aba5a814f--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----f1aba5a814f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f1aba5a814f--------------------------------) ·阅读时间 11 分钟·2023 年 2 月 22 日

--

![](img/77c0a9277a5199f8f3fbdf7450e3992a.png)

作者提供的图像

入门深度学习很简单。你只需几行代码就可以设置并训练一个神经网络。然而，当你从初学者进阶到中级水平时，可能会感到不知所措。你会遇到许多新术语，比如“EfficientNet”或“DeBERTa”。这些术语是什么意思？它们与神经网络有什么关系？

初学者教程没有告诉你，实际上只有少数人从头开始训练神经网络。这是因为神经网络在足够大的数据集上进行训练需要大量的时间和计算资源。因此，使用预训练模型作为起点是一种常见的做法。这种做法称为迁移学习 [2]。

> 初学者教程没有告诉你，实际上只有少数人从头开始训练神经网络。

## 前提条件

本文将指导你从初学者成长为中级深度学习领域的专家。因此，本文假设你已经对深度学习概念有一些基本了解。

## 免责声明

本文**灵感来源于我最近接触的四个关于深度学习最佳实践的资源**，并将其关键点总结成一篇文章。本文中的原始观点并非我所创 — 可以将这篇文章视为学习笔记。

+   V. Godbole, G. E. Dahl, J. Gilmer, C. J. Shallue 和 Z. Nado：[深度学习调整手册](https://github.com/google-research/tuning_playbook) [4]（格式：Git 仓库）

+   S. Bhutani 和 H20.ai：[机器学习模型训练最佳实践](https://www.youtube.com/watch?v=_mzrfMA8Qx4) [1]（视频格式）

+   P. Singer 和 Y. Babakhin：[深度迁移学习的实用技巧](https://drive.google.com/drive/folders/1VtJF-zPbXc-V-UDl2bDgWJp05DnKZpQH) 于 2022 年 11 月在 Kaggle Days 巴黎展示 [11]（演示文稿格式）

+   D. Kłeczek 与慕尼黑 NLP：[提高你的 NLP 模型训练的十种有效技巧](https://www.youtube.com/watch?v=UbL1QMwDpec) [9]（视频格式）

# 迁移学习简要介绍

迁移学习描述了重新使用预训练神经网络而不是从头训练网络以节省时间和计算资源的实践。因此，我们将预先学习的权重和知识从一个任务转移到另一个任务。预训练模型也被称为**骨干网络**。

![](img/a27087d9ae9dee6d870e82b63acb2ebf.png)

迁移学习（图片由作者提供）

当你的数据集较小或与骨干网络训练的数据集相似时，迁移学习是实用的 [2]。但即使你的数据集足够大或你的任务不同于骨干网络训练的任务，使用迁移学习也无妨 [8]，因为通常，前几层包含通用信息。

尽管你可以保持转移模型的权重并仅重新训练分类器，但通常会**微调**整个模型的权重。

当前计算机视觉（CV）问题的一些流行骨干网络包括：

+   ResNet（残差网络） [6]

+   DenseNet [7]

+   EfficientNet [12]

分别，针对自然语言处理（NLP）问题的一些流行骨干网络有：

+   BERT（双向编码器表示从变换器） [3]

+   RoBERTa（强健优化 BERT 预训练方法） [10]

+   DeBERTa（解码增强 BERT 与解耦注意力） [5]

当然，迁移学习的可能性仅仅因为研究人员共享他们的模型检查点以造福他人 [2]。

处理 CV 或 NLP 问题的主要步骤包括：

1.  建立基线

1.  逐步增加复杂性和提高性能

1.  挤压最后一点性能

# 第一步：建立基线

作为第一步，你应该建立一个基线，用于比较你在第二步中进行的任何实验。你不应该在这一步花费太多时间。只需确保你有一个足够好的工作基线。

## 骨干网络

骨干网络描述了你作为起点使用的预训练模型。不要花太多时间选择完美的骨干网络。你应该在第二步中更换骨干网络并尝试其他网络（见模型复杂性）——所以只需选择一个即可。**如何选择 EfficientNet 用于 CV 问题和 DeBERTa 用于（英语）NLP 问题** [1, 9, 11]？

## 批量大小

在模型开发的这一阶段决定一个批量大小。除非必要，否则不要改变批量大小，因为这需要重新开始调优过程 [4, 11]。

通常，增加批量大小会提高训练速度。因此，常见做法是使用内存中最大可容纳的 2 的幂次的批量大小（例如 16, 32, 64 等）[4]。

## 训练步骤数量（轮次）

**训练轮次** — 微调预训练模型时不需要很多额外的训练步骤。通常，**5 个轮次是一个好的起点**作为初始基线[11]。

**提前停止** — 尽管提前停止在过去很受欢迎[5]，但现在不再如此[1, 4, 11]。提前停止是一种技术，当验证指标不再改善时停止训练，以避免对训练数据的过拟合。然而，这种技术可能导致你对验证集进行过拟合并泄漏信息[1, 11]。

此外，提前停止总是需要一个验证集。因此，你将无法在整个数据集上重新训练最终模型以获得额外的性能提升[1]（见在完整数据集上重新训练）。因此，**不推荐使用提前停止**。

**最佳检查点选择** — 关于最佳检查点选择的最佳实践没有共同的理解。最佳检查点选择是指训练神经网络固定次数（不使用提前停止）后，选择具有最佳验证指标的运行。尽管[深度学习调优手册](https://github.com/google-research/tuning_playbook) [4] 推荐使用最佳检查点选择，但 Kaggle 大师们不推荐，因为它需要使用验证集[1, 11]（见在完整数据集上重新训练）。

## 优化器及其学习率

**优化器** — [深度学习调优手册](https://github.com/google-research/tuning_playbook) [4] 推荐从以下之一开始…

+   随机梯度下降（SGD）或

+   Adam。

然而，Kaggle 大师们表示，SGD 需要更多的调优工作才能达到与 Adam 相似的性能，因此推荐 Adam[1]。他们特别推荐**AdamW 作为优化器**[1]。

**学习率** — 虽然在从头训练神经网络时你需要使用较大的学习率，但在微调时，我们可以设置**较小的初始学习率，例如 1e-3**[11]。这是因为我们假设预训练模型的权重已经很好，不应过快地进行大幅调整[2]。

如果你不确定选择什么学习率，也可以查看你使用的模型架构的论文，以了解他们使用了什么学习率[9]。

**学习率调度器** — 虽然在从头训练神经网络时建议使用恒定学习率作为初始基线[4]，但在微调时应该使用学习率调度器作为初始基线[1, 4, 11]。

类似于优化器，人们喜欢争论最佳的学习率调度器。虽然当验证指标达到平台期时，减少学习率的方法曾经很流行，但这种方法依赖于验证集[1]。Kaggle 大师**推荐余弦退火学习率调度器**[1, 11]，如下所示。

![](img/2bf822f9e00d4b52db4707986c46da7b.png)

余弦衰减/退火学习率调度器（图片由作者提供，通过[《PyTorch 中学习率调度器的视觉指南》](https://medium.com/towards-data-science/a-visual-guide-to-learning-rate-schedulers-in-pytorch-24bbb262c863)）

**对于 NLP，**当你使用的训练轮次不多时，你可以保持学习率不变，并且你的初始学习率已经很小[1]。

## 花里胡哨的技术

每个月都有一种新奇的技术可以应用到你的训练流程中。但为了基线模型的效果，建议尽量保持简单，并在后续添加新特性[4]。

## 正则化与对验证集的过拟合

几年前，早期停止和在平台期降低学习率被视为避免模型对训练集过拟合的正则化技术[8]。今天，这些技术被认为在**对验证集的过拟合**方面存在泄漏[1]。

此外，任何依赖于验证集的技术都会阻止我们在最终配置下对整个数据集重新训练模型，这有助于提高模型性能。

# 第 2 步：逐步增加复杂性并小幅提升性能

你应该将大部分时间花在第二步。在这一阶段，你将进行许多实验，并对基线模型进行调整。你还可以将所有的花里胡哨的技术添加到你的模型中。只需逐步添加，以便能够评估其影响。

对于这一步，建议使用某种[实验跟踪系统](https://medium.com/@iamleonie/intro-to-mlops-experiment-tracking-for-machine-learning-858e432bd133)，无论是纸笔记录还是实验跟踪工具。

## 超参数调优

理论上，你可以通过在所有可能的超参数搜索空间中运行自动化超参数优化算法来最大化模型性能。但这并不现实。因此，你应该先通过进行一些初步实验来定义搜索空间[4]。

最重要的超参数是学习率和训练轮次。Kaggle 大师推荐以下范围作为起始点[11]：

+   学习率：1e-4 到 1e-3

+   训练轮次：2 到 10

**对于 NLP，**你只需少量轮次，因为模型的预训练效果优于计算机视觉领域。

因为我们已经缩小了超参数搜索空间，所以我们可以使用算法来[自动化超参数调整](https://medium.com/@iamleonie/intro-to-mlops-hyperparameter-tuning-9a938d21f894)[4, 8]。建议优先使用随机搜索或贝叶斯优化，而不是网格搜索[4, 8]。

## 数据增强

深度学习模型的性能严重依赖于数据的数量。因此，通过增加增强数据的数量，可以帮助提高模型的性能[1, 8, 11]。

计算机视觉中的常见数据增强包括：

+   水平或垂直翻转

+   旋转

+   调整大小

+   随机裁剪

+   移位

+   Mixup

+   Cutout

+   Cutmix

![](img/a82ff7e165b1304c857801c948f3acfe.png)

数据增强技术：Mixup、Cutout、Cutmix（图片由作者提供）

你可以在这篇文章中找到 PyTorch 实现的 cutout、mixup 和 cutmix：

[](/cutout-mixup-and-cutmix-implementing-modern-image-augmentations-in-pytorch-a9d7db3074ad?source=post_page-----f1aba5a814f--------------------------------) ## Cutout、Mixup 和 Cutmix：在 PyTorch 中实现现代图像增强

### 用 Python 实现的计算机视觉数据增强技术

[towardsdatascience.com

**对于 NLP，** 在计算机视觉中有效的数据增强技术似乎也适用于文本数据，如随机裁剪、调整大小或 cutout [9]。但并非所有计算机视觉数据增强技术都能直接转化为文本数据，因此需要其自身的数据增强技术。

+   反向翻译：将文本翻译成另一种语言，然后再翻译回原始语言

+   掩蔽实体语言建模（MELM）[13]：随机掩蔽句子中一定比例的标记（类似于计算机视觉中的 cutout [9]）

+   替换：用同义词替换句子中的词汇。

## 模型复杂性

最适合的骨干网会因问题而异。因此，你应该尝试几种不同的骨干网[1, 11]。

## 垃圾进，垃圾出

任何机器学习模型的好坏取决于你输入的数据质量。因此，实验不同的模型训练管道配置并审查训练数据是至关重要的。一个好的方法是**审查**模型预测效果好的样本和表现不佳的样本。

如果你**可视化**这些样本，你会使自己的工作更轻松[9]。这将给你一个关于数据或训练流程可能存在的问题的提示。

# 步骤 3：挤出最后的性能

一旦你完成实验（例如，截止日期或满意的性能），你可以进行一些最后的调整，挤出模型的最后性能。

## 在完整数据集上重新训练

深度学习模型对数据需求量大。这就是为什么如果你用最终的训练配置在完整数据集上重新训练模型，可以提升模型的性能[1, 11]。

## 集成方法

几年前，大型集成模型曾经很流行，而今天，最多三个模型的小型集成更为时尚[1]。在集成模型时，你可以尝试以下策略：

+   结合具有相同训练配置但不同种子的模型

+   结合具有多样性的不同模型（例如，不同的骨干网络）

## 添加更多花里胡哨的东西

如果你对结果仍不满意，尝试伪标签[9]或测试时增强[11]，看看能否挤出一点点性能提升。

# 结论

如果你刚刚完成了“深度学习入门”课程，并希望提升技能，这里有一种高层次的方法来处理任何深度学习问题。

从一个简单的基线开始：

+   **基础模型：** 先从一个开始——无论如何你都应该尝试其他模型：用于计算机视觉的 EfficientNet 和用于自然语言处理的 DeBERTa

+   **批量大小（固定）：** 检查在可用内存中适应的最大批量大小（例如，16、32、64 等），然后除非非常必要，否则不要改变它。

+   **训练步数：** 5 个 epoch，无早停

+   **优化器（固定）：** AdamW

+   **学习率：** 1e-3

+   **学习率调度器（固定）：** 余弦退火

建立一个[实验跟踪系统](https://medium.com/@iamleonie/intro-to-mlops-experiment-tracking-for-machine-learning-858e432bd133)并开始行动：

+   **超参数调整：** 从学习率（0.0001–0.001）和 epoch（2–10）的调整开始。使用随机搜索或贝叶斯优化进行[自动超参数调整](https://medium.com/@iamleonie/intro-to-mlops-hyperparameter-tuning-9a938d21f894)。

+   **数据增强：** 增多为妙。Mixup、cutout、cutmix 因其有效性而特别流行。

+   **基础模型：** 尝试不同的骨干网络和模型复杂性

+   **花里胡哨：** 在这一步，你可以大胆尝试每一个新兴的深度学习趋势，看看它是否会提升你的模型表现。

为了挤出最后一丝性能，你应该选择最多三个最佳训练配置，重新在整个数据集上训练模型，并进行集成。

# 喜欢这个故事吗？

[*免费订阅*](https://medium.com/subscribe/@iamleonie) *以便在我发布新故事时收到通知。*

[](https://medium.com/@iamleonie/subscribe?source=post_page-----f1aba5a814f--------------------------------) [## 当 Leonie Monigatti 发布时获取电子邮件。

### 当 Leonie Monigatti 发布时获取电子邮件。如果你还没有账户，通过注册你将创建一个 Medium 账户……

[medium.com](https://medium.com/@iamleonie/subscribe?source=post_page-----f1aba5a814f--------------------------------)

*在* [*LinkedIn*](https://www.linkedin.com/in/804250ab/)、[*Twitter*](https://twitter.com/helloiamleonie)*和* [*Kaggle*](https://www.kaggle.com/iamleonie)*上找到我！*

# 参考文献

[1] S. Bhutani 与 H20.ai (2023). [训练机器学习模型的最佳实践 | @ChaiTimeDataScience #160](https://www.youtube.com/watch?v=_mzrfMA8Qx4) 于 2023 年 1 月在 YouTube 上发布。

[2] CS231n 视觉识别的卷积神经网络 (2023)。[迁移学习](https://cs231n.github.io/transfer-learning/)（访问日期：2023 年 2 月 3 日）

[3] J. Devlin, M. M. Chang, K. Lee & K. Toutanova (2018). BERT：用于语言理解的深度双向转换器的预训练。*arXiv 预印本 arXiv:1810.04805*。

[4,] V. Godbole, G. E. Dahl, J. Gilmer, C. J. Shallue 和 Z. Nado (2023). [深度学习调优手册](https://github.com/google-research/tuning_playbook)（第 1.0 版）（访问日期：2023 年 2 月 3 日）

[5] P. He, X. Liu, J. Gao, & W. Chen (2020). Deberta：通过解耦注意力的解码增强 BERT。*arXiv 预印本 arXiv:2006.03654*。

[6] K. He, X. Zhang, S. Ren, & J. Sun (2016). 深度残差学习用于图像识别。载于 *IEEE 计算机视觉与模式识别会议论文集*（第 770–778 页）。

[7] G. Huang, Z. Liu, L. Van Der Maaten & K. Q. Weinberger (2017). 密集连接卷积网络。载于 *IEEE 计算机视觉与模式识别会议论文集*（第 4700–4708 页）。

[8] A. Karpathy (2019). [训练神经网络的配方](http://karpathy.github.io/2019/04/25/recipe/)（访问日期：2023 年 2 月 3 日）

[9] D. Kłeczek 与慕尼黑 NLP (2023)。[提升 NLP 模型训练效果的十种有效技巧](https://www.youtube.com/watch?v=UbL1QMwDpec)（访问日期：2023 年 2 月 9 日）

[10] Y. Liu, M. Ott, N. Goyal, J. Du, M. Joshi, D. Chen & V. Stoyanov (2019). RoBERTa：一种稳健优化的 BERT 预训练方法。*arXiv 预印本 arXiv:1907.11692*。

[11] P. Singer 和 Y. Babakhin (2022). [深度迁移学习的实用技巧](https://drive.google.com/drive/folders/1VtJF-zPbXc-V-UDl2bDgWJp05DnKZpQH) 于 2022 年 11 月在 Kaggle Days Paris 上展示。

[12] M. Tan, & Q. Le (2019). EfficientNet：重新思考卷积神经网络的模型缩放。载于 *国际机器学习会议*（第 6105–6114 页）。PMLR

[13] R. Zhou, X. Li, R. He, L. Bing, E. Cambria, L. Si, & C. Miao (2021). MELM：通过掩蔽实体语言建模进行低资源 NER 的数据增强。*arXiv 预印本 arXiv:2108.13655*。
