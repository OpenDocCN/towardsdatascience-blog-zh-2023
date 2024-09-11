# 时间图基准

> 原文：[https://towardsdatascience.com/temporal-graph-benchmark-bb5cc26fcf11?source=collection_archive---------2-----------------------#2023-12-09](https://towardsdatascience.com/temporal-graph-benchmark-bb5cc26fcf11?source=collection_archive---------2-----------------------#2023-12-09)

## 挑战性和现实的时间图学习数据集

[](https://medium.com/@shenyanghuang1996?source=post_page-----bb5cc26fcf11--------------------------------)[![Shenyang(Andy) Huang](../Images/ab63c37868db97b19480d536388930c5.png)](https://medium.com/@shenyanghuang1996?source=post_page-----bb5cc26fcf11--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bb5cc26fcf11--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bb5cc26fcf11--------------------------------) [Shenyang(Andy) Huang](https://medium.com/@shenyanghuang1996?source=post_page-----bb5cc26fcf11--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8aa224c5cedd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporal-graph-benchmark-bb5cc26fcf11&user=Shenyang%28Andy%29+Huang&userId=8aa224c5cedd&source=post_page-8aa224c5cedd----bb5cc26fcf11---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bb5cc26fcf11--------------------------------) · 10分钟阅读 · 2023年12月9日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fbb5cc26fcf11&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporal-graph-benchmark-bb5cc26fcf11&user=Shenyang%28Andy%29+Huang&userId=8aa224c5cedd&source=-----bb5cc26fcf11---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fbb5cc26fcf11&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftemporal-graph-benchmark-bb5cc26fcf11&source=-----bb5cc26fcf11---------------------bookmark_footer-----------)

近年来，*静态*图上的机器学习取得了显著进展，这得益于公共数据集和标准化评估协议的普及，例如广泛采用的[开放图基准](https://ogb.stanford.edu/) (OGB)。然而，许多现实世界系统，如社交网络、交通网络和金融交易网络，随着时间的推移不断演变，节点和边不断添加或删除。这些系统通常被建模为时间图。到目前为止，时间图学习的进展受到缺乏大型高质量数据集以及缺乏适当评估的制约，导致了过于乐观的性能表现。

![](../Images/55b9a02b0395a19d3392ea3ce09bde92.png)

现实世界的网络随着时间的推移而演变。图片来源：[Armand Khoury](https://unsplash.com/@armand_khoury?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

为了解决这一问题，我们推出了[时序图基准测试](https://tgb.complexdatalab.com/)（TGB），这是一个针对时序图的挑战性和多样化基准数据集的集合，用于现实的、可重复的、稳健的机器学习评估。受到OGB成功的启发，TGB自动化了数据集下载和处理以及评估协议，并允许用户通过排行榜比较模型性能。我们希望TGB能够成为时序图社区的标准化基准，促进新方法的发展，并提高对大型时序网络的理解。

![](../Images/4d62ef740fc78412625bdf79bdce99fc.png)

针对时序图学习的挑战性和现实性基准

+   网站: [https://tgb.complexdatalab.com/](https://tgb.complexdatalab.com/)

+   论文: [https://arxiv.org/abs/2307.01026](https://arxiv.org/abs/2307.01026)

+   Github: [https://github.com/shenyangHuang/TGB](https://github.com/shenyangHuang/TGB)

*这篇文章基于我们的论文* [*时序图基准测试在时序图上的机器学习*](https://arxiv.org/abs/2307.01026) *(NeurIPS 2023 数据集和基准测试专题)，由* [*Emanuele Rossi*](https://emanuelerossi.co.uk/)*共同撰写。请在* [*我的网站*](https://cs.mcgill.ca/~shuang43/)*查找更多时序图相关工作。想了解更多关于时序图的内容？加入* [*时序图阅读小组*](https://www.cs.mcgill.ca/~shuang43/rg.html) *和* [*NeurIPS 2023 时序图学习研讨会*](https://sites.google.com/view/tglworkshop-2023/home) *，了解最前沿的TG研究。*

## **目录：**

1.  [动机](#4c5e)

1.  [问题设定](#7eaf)

1.  [数据集详情](#68dd)

1.  [动态链接属性预测](#baea)

1.  [动态节点属性预测](#3d58)

1.  [开始使用TGB](#59dc)

1.  [结论与未来工作](#f629)

# 动机

近年来，静态图的机器学习领域得到了显著提升，这主要归功于公开数据集的出现和已建立的基准测试，例如[开放图基准](https://ogb.stanford.edu/)（OGB）、[长程图基准](https://arxiv.org/abs/2206.08164)和[TDC基准](https://tdcommons.ai/benchmark/admet_group/overview/)。然而，许多现实世界的系统，如社交网络、交通网络和金融交易网络，都是时间性的：它们随着时间的发展而演变。直到现在，时间图的发展由于缺乏大型、高质量的数据集和全面的评估框架而受到显著阻碍。这种稀缺性，加上评估限制，导致了在流行数据集（如Wikipedia和Reddit）上的几乎完美的AP或AUROC分数，导致了对模型性能的过于乐观的评估，并且在区分竞争模型方面面临挑战。

**数据集的缺乏。** 常见的TG数据集仅包含几百万条边，远远小于实际时间网络中的规模。此外，这些数据集大多限制在社交和互动网络领域。由于网络属性在不同领域间通常变化显著，因此在多个领域上进行基准测试也很重要。最后，缺乏节点级任务的数据集，导致大多数方法仅关注链接预测。为了解决这个挑战，TGB包含了来自*五*个不同领域的*nine*个数据集，这些数据集在节点、边和时间戳的数量上都是*数量级更大*的。此外，TGB还提出了四个数据集用于新的节点亲和预测任务。

![](../Images/c25741e77b0898abc3075c7354c8799b.png)

TGB数据集显著大于常见的TG数据集

**简化的评估。** 动态链接预测通常被框架化为二分类任务：正（真实）边标记为1，负（不存在）边标记为0。在评估时，通过保持源节点固定并随机选择目标节点来采样每个正边的一个负边。这种评估仅考虑少量容易预测的负边，导致模型性能被夸大，许多模型在Wikipedia和Reddit上获得了>95%的AP（[Poursafaei et al. 2022](https://openreview.net/forum?id=1GVpwr2Tfdg)，[Rossi et al. 2020](https://arxiv.org/pdf/2006.10637.pdf)，[Wang et al. 2021](https://arxiv.org/pdf/2101.05974.pdf)，[Souza et al. 2022](https://arxiv.org/pdf/2209.15059.pdf)）。在TGB中，我们将链接预测任务视为*排序问题*，并使评估更加稳健。我们展示了改进的评估结果能提供更现实的性能表现，并突出了不同模型之间的明显差距。

# 问题设定

在 TGB 中，我们专注于连续时间的时间图，如 [Kazemi et al. 2020](https://www.jmlr.org/papers/volume21/19-447/19-447.pdf) 定义的那样。在这种设置中，我们将时间图表示为带时间戳的边流，由*(源节点, 目标节点, 时间戳)*三元组组成。请注意，时间边可以是加权的、有向的，同时节点和边可以选择性地具有特征。

此外，我们还考虑了*流式设置*，在这种设置中，模型可以在推理时纳入新信息。特别地，在时间*t*预测测试边时，模型可以访问[1]所有发生在*t*之前的边，包括测试边。然而，不允许使用测试信息进行反向传播和权重更新。

# 数据集详情

TGB 包含 *九* 个数据集，其中七个是为此工作专门整理的，两个来自以前的文献。这些数据集在时间上分为训练集、验证集和测试集，比例为 70/15/15。数据集根据边的数量分类：**小型**（<5百万）、**中型**（5–25百万）和**大型**（> 25百万）。

![](../Images/8341d3cbaab16d783ff26c6224c0d584.png)

TGB 数据集的统计信息

TGB 数据集还具有不同的领域和时间粒度（从 UNIX 时间戳到年度）。最后，数据集的统计信息也非常多样化。例如，*惊讶指数*，由训练集中从未观察到的测试边的比例定义，在不同的数据集中差异显著。许多 TGB 数据集中还包含许多测试集中出现的新节点，这需要归纳推理。

TGB 数据集也与现实世界任务相关。例如，`tgbl-flight` 数据集是一个从 2019 年到 2022 年的众包国际航班网络，其中机场建模为节点，而边则是给定日期的机场之间的航班。任务是预测未来某个日期两特定机场之间是否会发生航班。这对于预测潜在的航班中断（如取消和延误）非常有用。例如，在 COVID-19 大流行期间，为了遏制 COVID-19 的传播，许多航班路线被取消。预测全球航班网络对研究和预测疾病（如 COVID-19）向新地区传播也很重要，如 [Ding et al. 2021](https://appliednetsci.springeropen.com/articles/10.1007/s41109-021-00378-3) 中所见。详细的数据集和任务描述在论文第 4 节中提供。

# 动态链接属性预测

动态链接属性预测的目标是预测在未来时间戳下，节点对之间链接的属性（通常是存在性）。

**负边采样。** 在实际应用中，真实的边在事先并不为人知。因此，查询大量节点对，仅将得分最高的节点对视为边。受到这一点的启发，我们将链接预测任务框架化为排名问题，并对每个正边采样多个负边。具体而言，对于给定的正边*(s,d,t)*，我们固定源节点*s*和时间戳*t*，并采样*q*个不同的目标节点*d*。对于每个数据集，*q*的选择基于评估完整性和测试集推断时间之间的权衡。在*q*个负样本中，一半是均匀随机采样的，另一半是历史负边（在训练集中观察到但在时间*t*时不存在的边）。

**性能指标。** 我们使用过滤后的平均倒数排名（MRR）作为本任务的指标，因为它专为排名问题设计。MRR计算真实目标节点在负样本或伪目标中的倒数排名，通常用于推荐系统和知识图谱文献中。

![](../Images/2de26668a84d4f95678cdf9552315def.png)

tgbl-wiki和tgbl-review数据集上的MRR表现

**小数据集的结果。** 在小数据集`tgbl-wiki`和`tgbl-review`上，我们观察到最佳表现的模型有很大差异。此外，在`tgbl-wiki`上的顶级模型，如CAWN和NAT，在`tgbl-review`上的性能显著下降。一个可能的解释是，与`tgbl-wiki`数据集相比，`tgbl-review`数据集具有更高的惊讶指数。高惊讶指数表明，测试集边的高比例从未在训练集中观察到，因此`tgbl-review`需要更多的归纳推理。在`tgbl-review`中，GraphMixer和TGAT是表现最佳的模型。由于其较小的规模，我们能够为`tgbl-wiki`采样所有可能的负样本，为`tgbl-review`每个正边采样一百个负样本。

![](../Images/5d52562b0d3c68165cc4af122a624934.png)

tgbl-coin、tgbl-comment和tgbl-flight数据集上的MRR表现。

大多数方法在这些数据集上运行时耗尽了GPU内存，因此我们对TGN、DyRep和Edgebank进行了比较，因为它们的GPU内存需求较低。注意，某些数据集如`tgbl-comment`或`tgbl-flight`跨越多年，因此可能导致其长期跨度上的分布变化。

![](../Images/5e47ed9b91ff0b50a011703067d1074f.png)

负样本数量对tgbl-wiki的影响

**洞察。** 如`tgbl-wiki`中所示，用于评估的负样本数量可以显著影响模型性能：我们看到，当负样本数量从20增加到所有可能的目标时，大多数方法的性能显著下降。这验证了确实需要更多的负样本来进行稳健的评估。有趣的是，像CAWN和Edgebank这样的算法性能下降相对较小，我们将其作为未来的工作来调查为何某些方法受影响较小。

![](../Images/9f9c7a438f6e295e79c7d2ad309a32cd.png)

TG模型的总训练和验证时间

接下来，我们观察到TG方法的训练和验证时间差异高达两个数量级，其中启发式基线Edgebank始终是最快的（因为它简单地实现为哈希表）。这表明，提高模型效率和可扩展性是未来的重要方向，以便可以在TGB中提供的大型数据集上测试新的和现有的模型。

# 动态节点属性预测

动态节点属性预测的目标是在任何给定的时间戳*t*预测节点的属性。由于缺乏具有动态节点标签的大型公共TG数据集，我们引入了*节点亲和性预测*任务来研究时间图上的节点级任务。如果您希望贡献具有节点标签的新数据集，请与我们联系。

**节点亲和性预测。** 该任务考虑节点子集（例如用户）对其他节点（例如项目）的亲和性及其随时间自然变化的方式。这个任务在推荐系统中很相关，在那里，通过建模用户对不同项目的偏好随时间的变化来为用户提供个性化推荐非常重要。在这里，我们使用前10项的归一化折扣累积增益（NDCG@10）来比较预测项目与真实值的相对顺序。标签是通过统计用户在未来一段时间内与不同项目的互动频率生成的。

![](../Images/036bffdb35c0e99ac55a19aa42e8bbac.png)

节点亲和性预测任务的实证结果。

**结果。** 对于这个任务，我们将TG模型与两种简单的启发式方法进行比较：*持久性预测*，即预测当前时间点的最近观察到的节点标签，以及*移动平均*，即过去几步中的节点标签的平均值。这里的关键观察是，在这个任务中，像持久性预测和移动平均这样的简单启发式方法是TG方法的有力竞争者，并且在大多数情况下，它们的表现超过了TG方法。这突显了未来需要开发更多针对节点级任务的TG方法。

# 开始使用TGB

![](../Images/700f411b91e1d6830ed846f9a8ff713b.png)

TGB的机器学习管道

如何使用 TGB？上面展示了 TGB 的 ML 流程。你可以自动下载数据集，并将其处理为 `numpy`、`PyTorch` 和 `PyG` 兼容的数据格式。用户只需设计自己的 TG 模型，这些模型可以通过 TGB 评估器 *进行* 标准化评估*。* 最后，公开的 TGB 排行榜帮助研究人员跟踪时间图领域的最新进展。你可以轻松安装该软件包：

```py
pip install py-tgb
```

最后，你可以将你的模型性能提交到 TGB 排行榜。我们要求你提供代码链接和描述你方法的论文以确保可重复性。要提交，请填写 [google 表单](https://forms.gle/SEsXvN1QHo9tSFwx9)。

# 结论与未来工作

为了实现对时间图进行现实、可重复和鲁棒的评估，我们推出了时间图基准（Temporal Graph Benchmark），这是一个包含挑战性和多样化数据集的集合。通过 TGB 数据集和评估，我们发现模型性能在不同数据集上差异显著，这显示了在多样的时间图领域进行评估的必要性。此外，在节点亲和度预测任务中，简单的启发式方法优于 TG 方法，从而激发了未来开发更多节点级 TG 模型的动机。

**集成到 PyG 中。** Matthias Fey（Kumo.AI），[PyG](https://pyg.org/) 的核心负责人，在 [斯坦福图学习研讨会 2023](https://snap.stanford.edu/graphlearning-workshop-2023/) 上宣布，TGB 将集成到 PyG 的未来版本中。敬请关注！

**TGX 库。** 我们目前正在开发一个用于时间图的实用工具和可视化 Python 库，名为 [TGX](https://github.com/shenyangHuang/TGX)。TGX 支持来自 TGB 的 20 个内置时间图数据集以及 [Poursafaei et al. 2022](https://openreview.net/forum?id=1GVpwr2Tfdg)。

**社区反馈与数据集贡献。** TGB 是一个社区驱动的项目，我们感谢所有通过电子邮件或 Github 问题提供建议的社区成员。如果你有任何建议或希望向 TGB 贡献新的数据集，请通过 [email](http://shenyang.huang@mail.mcgill.ca) 或 [在 Github 上创建问题](https://github.com/shenyangHuang/TGB/issues) 与我们联系。我们正在寻找大规模数据集，特别是用于动态节点或图分类任务的数据集。
