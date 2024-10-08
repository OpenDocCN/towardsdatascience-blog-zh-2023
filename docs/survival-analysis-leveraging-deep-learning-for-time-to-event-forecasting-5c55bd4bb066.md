# 生存分析：利用深度学习进行事件时间预测（第二部分）

> 原文：[`towardsdatascience.com/survival-analysis-leveraging-deep-learning-for-time-to-event-forecasting-5c55bd4bb066`](https://towardsdatascience.com/survival-analysis-leveraging-deep-learning-for-time-to-event-forecasting-5c55bd4bb066)

![](img/b9fbc07c43c92a2032ad02c8e6952a78.png)

作者插图

## 实际应用于再住院

[](https://linafaik.medium.com/?source=post_page-----5c55bd4bb066--------------------------------)![Lina Faik](https://linafaik.medium.com/?source=post_page-----5c55bd4bb066--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5c55bd4bb066--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5c55bd4bb066--------------------------------) [Lina Faik](https://linafaik.medium.com/?source=post_page-----5c55bd4bb066--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5c55bd4bb066--------------------------------) ·10 分钟阅读·2023 年 4 月 21 日

--

生存模型非常适合预测事件发生的时间。这些模型可以应用于各种用例，包括预测维护（预测机器可能故障的时间）、营销分析（预测客户流失）、患者监测（预测患者可能会再度住院）等。

通过将机器学习与生存模型结合，结果模型可以利用前者的高预测能力，同时保留后者的框架和典型输出（例如生存概率或随时间变化的风险曲线）。欲了解更多信息，请查看本系列的第一篇文章 这里。

然而在实践中，基于机器学习的生存模型仍然需要广泛的特征工程，因此需要先验的业务知识和直觉才能取得令人满意的结果。那么，为什么不使用深度学习模型来弥补这一差距呢？

## 目标

本文重点介绍了如何将深度学习与生存分析框架结合，以解决例如预测患者（再）住院可能性的用例。

阅读本文后，你将了解：

1.  深度学习如何应用于生存分析？

1.  在生存分析中，常见的深度学习模型有哪些，它们是如何工作的？

1.  这些模型如何具体应用于住院预测？

> *本文是关于生存分析系列的第二部分。如果你对生存分析不熟悉，最好先阅读第一部分* *这里**。文章中描述的实验使用了库* [*scikit-survival*](https://scikit-survival.readthedocs.io/en/stable/)*、* [*pycox*](https://github.com/havakv/pycox)* 和* [*plotly*](https://plotly.com/)*。你可以在* [*GitHub*](https://github.com/linafaik08/survival_analysis)*上找到代码*。

[](https://github.com/linafaik08/survival_analysis?source=post_page-----5c55bd4bb066--------------------------------) [## GitHub - linafaik08/survival_analysis

### 作者：莉娜·法伊克 创建日期：2023 年 2 月 最后更新：2023 年 4 月 本仓库包含代码和笔记本……

[github.com](https://github.com/linafaik08/survival_analysis?source=post_page-----5c55bd4bb066--------------------------------)

# 1\. 生存分析与深度学习：它们如何结合？

## 1.1\. 问题陈述

让我们首先描述一下手头的问题。

我们感兴趣的是根据可用的健康状态信息预测某一患者重新入院的可能性。更具体地说，我们希望在最后一次就诊后的不同时间点估计这种概率。这种估计对监测患者健康和降低复发风险至关重要。

这是一个典型的生存分析问题。数据包括三个元素：

患者的基线数据包括：

+   人口统计学：年龄、性别、所在地（乡村或城市）

+   患者历史：吸烟、饮酒、糖尿病、高血压等。

+   实验室结果：血红蛋白、总淋巴细胞计数、血小板、葡萄糖、尿素、肌酐等。

+   关于源数据集的更多信息请见 [这里](https://www.kaggle.com/datasets/ashishsahani/hospital-admissions-data)。

一个时间 t 和一个事件指示器 δ∈{0;1}：

+   如果事件在观察期间发生，t 等于从数据收集时刻到观察到事件（即重新入院）时刻之间的时间，这种情况下，δ = 1。

+   如果没有发生事件，t 等于从数据收集时刻到最后一次接触患者（例如，研究结束）之间的时间，这种情况下，δ = 0。

![](img/e3e77620a18b3a6c27965c933b880b50.png)

图 1 — 生存分析数据，作者插图。注意：患者 A 和 C 被删失。

⚠️ 使用生存分析方法的原因是什么，尤其是当问题如此类似于回归任务时？初始论文很好地解释了主要原因：

> *“如果选择使用标准回归方法，右删失数据会变成一种缺失数据。通常会被删除或填补，这可能会引入模型偏差。因此，建模右删失数据需要特别注意，因此需要使用生存模型。” 来源 [*[*2*](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/s12874-018-0482-1)*]*

## 1.2\. DeepSurv

**方法**

让我们进入理论部分，稍微复习一下风险函数。

> *“风险函数是指一个个体在已经存活到时间 t 的条件下，额外微小时间δ内未能存活的概率。因此，更大的风险意味着更大的死亡风险。”*

![](img/f5b8ecc8a984dc09f6424e5852554da3.png)

> *来源 [*[*2*](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/s12874-018-0482-1)*]*

与 Cox 比例风险（CPH）模型类似，DeepSurv 基于以下假设：风险函数是两个函数的乘积：

+   **基线风险函数**：λ_0(t)

+   **风险评分**，r(x)=exp(h(x))。它建模了给定个体在观察到的协变量下，风险函数如何相对于基线变化。

更多关于 Cox 比例风险（CPH）模型的信息，请参见本系列的第一篇文章。

![](img/3d2005158f70b317257cd0e68c7172fb.png)

函数 h(x)通常被称为**对数风险函数**。这正是 DeepSurv 模型旨在建模的函数。

实际上，CPH 模型假设*h(x)*是一个线性函数：h(x) = β . x。拟合模型的过程就是计算权重*β*以优化目标函数。然而，线性比例风险假设在许多应用中并不成立。这就证明了需要一个更复杂的非线性模型，理想情况下能够处理大量数据。

**架构**

在这种情况下，DeepSurv 模型如何提供更好的替代方案？让我们先来描述一下它。根据原始论文，它是一个“深度前馈神经网络，通过网络权重θ预测患者协变量对其风险率的影响。” [[2](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/s12874-018-0482-1)]

它是如何工作的？

> *‣ 网络的输入是基线数据 x。*
> 
> *‣ 网络通过多个隐藏层进行输入传播，隐藏层具有权重θ。这些隐藏层包含全连接的非线性激活函数，随后是 dropout。*
> 
> *‣ 最后一层是一个节点，对隐藏特征进行线性组合。网络的输出被视为预测的对数风险函数。*
> 
> *来源 [*[*2*](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/s12874-018-0482-1)*]*

![](img/2c652f43146874fb360756b03fdebada.png)

图 2 — DeepSurv 架构，作者插图，灵感来源于[[2](https://www.researchgate.net/publication/323409041_DeepSurv_Personalized_treatment_recommender_system_using_a_Cox_proportional_hazards_deep_neural_network)]

由于这种架构，模型非常灵活。通常使用超参数搜索技术来确定隐藏层的数量、每层中的节点数、丢弃概率以及其他设置。

那么，优化的目标函数是什么？

+   CPH 模型被训练以优化 Cox 部分似然。它包括计算每个患者*i*在时间*Ti*的事件发生概率，考虑到在时间*Ti*时仍处于风险中的所有个体，然后将所有这些概率相乘。您可以在这里找到确切的数学公式[[2](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/s12874-018-0482-1)]。

+   类似地，DeepSurv 的目标函数是相同部分似然的对数负均值，加上一个用于正则化网络权重的附加部分。[[2](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/s12874-018-0482-1)]

**代码示例**

下面是一个小的代码片段，以了解如何使用[pycox](https://github.com/havakv/pycox)库实现这种类型的模型。完整代码可以在该库的笔记本示例中找到[这里](https://nbviewer.org/github/havakv/pycox/blob/master/examples/cox-ph.ipynb) [6]。

```py
# Step 1: Neural net
# simple MLP with two hidden layers, ReLU activations, batch norm and dropout

in_features = x_train.shape[1]
num_nodes = [32, 32]
out_features = 1
batch_norm = True
dropout = 0.1
output_bias = False

net = tt.practical.MLPVanilla(in_features, num_nodes, out_features, batch_norm,
                              dropout, output_bias=output_bias)

model = CoxPH(net, tt.optim.Adam)

# Step 2: Model training

batch_size = 256
epochs = 512
callbacks = [tt.callbacks.EarlyStopping()]
verbose = True

model.optimizer.set_lr(0.01)

log = model.fit(x_train, y_train, batch_size, epochs, callbacks, verbose,
                val_data=val, val_batch_size=batch_size)

# Step 3: Prediction

_ = model.compute_baseline_hazards()
surv = model.predict_surv_df(x_test)

# Step 4: Evaluation

ev = EvalSurv(surv, durations_test, events_test, censor_surv='km')
ev.concordance_td()
```

## 1.3\. DeepHit

**方法**

如果我们可以训练一个深度神经网络直接学习生存时间的分布，而不是对其做出强假设呢？

这就是 DeepHit 模型的情况。特别是，它相对于以前的方法带来了两个显著改进：

+   它不依赖于任何关于基础随机过程的假设。因此，网络学会了建模协变量与风险之间随时间演变的关系。

+   它可以通过多任务学习架构处理竞争风险（例如，同时建模再住院和死亡的风险）。

**架构**

如此处所述[[3](https://humboldt-wi.github.io/blog/research/information_systems_1920/group2_survivalanalysis/)]，DeepHits 遵循多任务学习模型的常见架构。它由两个主要部分组成：

1.  共享子网络，其中模型从数据中学习到对所有任务有用的通用表示。

1.  任务特定的子网络，其中模型学习到更具任务特定性的表示。

然而，DeepHit 模型的架构与典型的多任务学习模型在两个方面有所不同：

+   它包括一个残差连接，将初始协变量与任务特定子网络的输入连接起来。

+   它只使用一个 softmax 输出层。得益于此，模型学习的是竞争事件的联合分布，而不是边际分布。

下图展示了模型在两个任务上同时训练的情况。

DeepHit 模型的输出是每个受试者的向量*y*。它给出受试者在观察时间内每个时间戳*t*发生事件 k ∈ [1, 2]的概率。

![](img/7aef1455c0b24e7f5adfc3e79c4b6ad0.png)

图 3 — DeepHit 架构，作者插图，灵感来源于源[[4](https://www.semanticscholar.org/paper/DeepHit%3A-A-Deep-Learning-Approach-to-Survival-With-Lee-Zame/803a7b26bdc0feafbf45bc5d57c2bc3f55b6f8fc#references)]

# 2\. 用例应用：这些模型在实际中的表现如何？

## 2.1\. 方法论

**数据**

数据集被分成三部分：训练集（占数据的 60%），验证集（20%），和测试集（20%）。训练集和验证集用于在训练期间优化神经网络，测试集用于最终评估。

**基准**

深度学习模型的表现与包括 CoxPH 和基于 ML 的生存模型（梯度提升和 SVM）在内的基准模型进行比较。有关这些模型的更多信息，请参见系列的第一篇文章。

**指标**

两种指标用于评估模型：

+   Concordance index (C-index)：它衡量模型根据个体风险评分提供可靠生存时间排名的能力。它的计算方法是数据集中一致对的比例。

+   Brier 分数：这是均方误差对右删失数据的时间依赖扩展。换句话说，它表示观察到的生存状态与预测的生存概率之间的平均平方距离。

## 2.2\. 结果

从 C-index 的角度来看，深度学习模型的表现明显优于基于 ML 的生存分析模型。此外，Deep Surval 和 Deep Hit 模型的表现几乎没有差异。

![](img/5b0fee5c6f6e07e90fe4ba28055eb185.png)

图 4 — 模型在训练集和测试集上的 C-Index

从 Brier 分数的角度来看，Deep Surv 模型脱颖而出。

+   在考察 Brier 分数随时间变化的曲线时，Deep Surv 模型的曲线低于其他模型，这反映了更好的准确性。

![](img/4815a6c1392cf9b765a460a44f1b13ac.png)

图 5 — 测试集上的 Brier 分数

+   考虑到在相同时间区间内分数的积分，这一观察结果得到了确认。

![](img/fa5af2fbbb6e2b5c3bfea6e9c81388ac.png)

图 6 — 测试集上的集成 Brier 分数

> *请注意，Brier 分数未计算 SVM，因为该分数仅适用于能够估计生存函数的模型。*

![](img/d92279c637bb8588fdb1145e2392abdc.png)

图 7 — 使用 DeepSurv 模型的随机选取患者的生存曲线

最终，深度学习模型不仅可以用于生存分析，也可以用于统计模型。例如，在这里，我们可以看到随机选择的患者的生存曲线。这些输出可以带来许多好处，特别是能够更有效地跟踪最有风险的患者。

# 关键要点

✔️ 生存模型对于预测事件发生所需的时间非常有用。

✔️ 它们可以通过提供学习框架和技术以及有用的输出（如生存概率或随时间变化的危险曲线）来帮助解决许多使用案例。

✔️ 在这种使用案例中，它们甚至是不可或缺的，以利用所有数据，包括被删失的观察（例如，当事件在观察期内未发生时）。

✔️ 基于机器学习的生存模型往往比统计模型表现更好（更多信息见这里）。然而，它们需要基于坚实的业务直觉进行高质量的特征工程，以实现令人满意的结果。

✔️ 这就是深度学习可以弥补差距的地方。基于深度学习的生存模型如 DeepSurv 或 DeepHit 有可能以更少的努力取得更好的表现！

✔️ 然而，这些模型也不是没有缺点。它们需要一个大型数据库进行训练，并且需要对多个超参数进行微调。

# 参考文献

[1] Bollepalli, S.C.; Sahani, A.K.; Aslam, N.; Mohan, B.; Kulkarni, K.; Goyal, A.; Singh, B.; Singh, G.; Mittal, A.; Tandon, R.; Chhabra, S.T.; Wander, G.S.; Armoundas, A.A. [一种优化的机器学习模型准确预测入院心脏病科的住院结果](https://doi.org/10.3390/diagnostics12020241)。Diagnostics 2022, 12, 241。

[2] Katzman, J., Shaham, U., Bates, J., Cloninger, A., Jiang, T., & Kluger, Y. (2016). [DeepSurv: 个性化治疗推荐系统使用 Cox 比例风险深度神经网络](https://doi.org/10.1186/s12874-018-0482-1)，ArXiv

[3] Laura Löschmann, Daria Smorodina, [用于生存分析的深度学习](https://humboldt-wi.github.io/blog/research/information_systems_1920/group2_survivalanalysis/)，信息系统研讨会（WS19/20），2020 年 2 月 6 日

[4] Lee, Changhee 等人 [DeepHit：一种深度学习方法用于处理竞争风险的生存分析。](https://www.semanticscholar.org/paper/DeepHit%3A-A-Deep-Learning-Approach-to-Survival-With-Lee-Zame/803a7b26bdc0feafbf45bc5d57c2bc3f55b6f8fc#references) AAAI 人工智能大会（2018）。

[5] 维基百科，[*比例风险模型*](https://en.wikipedia.org/wiki/Proportional_hazards_model)

[6] [Pycox 库](https://github.com/havakv/pycox)
