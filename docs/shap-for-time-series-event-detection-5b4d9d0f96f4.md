# SHAP 用于时间序列事件检测

> 原文：[https://towardsdatascience.com/shap-for-time-series-event-detection-5b4d9d0f96f4?source=collection_archive---------2-----------------------#2023-02-01](https://towardsdatascience.com/shap-for-time-series-event-detection-5b4d9d0f96f4?source=collection_archive---------2-----------------------#2023-02-01)

![](../Images/b724a3e58b563f104352ce857ee365b2.png)

图片由 [Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 使用修改版的 KernelSHAP 进行时间序列事件检测

[](https://medium.com/@upadhyan?source=post_page-----5b4d9d0f96f4--------------------------------)[![Nakul Upadhya](../Images/336cb21272e9b1f098177adbde50e92e.png)](https://medium.com/@upadhyan?source=post_page-----5b4d9d0f96f4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5b4d9d0f96f4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5b4d9d0f96f4--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----5b4d9d0f96f4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4d9dddc62a80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-for-time-series-event-detection-5b4d9d0f96f4&user=Nakul+Upadhya&userId=4d9dddc62a80&source=post_page-4d9dddc62a80----5b4d9d0f96f4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b4d9d0f96f4--------------------------------) ·8 min 阅读 ·2023年2月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5b4d9d0f96f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-for-time-series-event-detection-5b4d9d0f96f4&user=Nakul+Upadhya&userId=4d9dddc62a80&source=-----5b4d9d0f96f4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5b4d9d0f96f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fshap-for-time-series-event-detection-5b4d9d0f96f4&source=-----5b4d9d0f96f4---------------------bookmark_footer-----------)

特征重要性是一种广泛使用的技术，用于解释机器学习模型如何做出预测。该技术为每个特征分配一个分数或权重，指示该特征对预测的贡献程度。这些分数可以用来识别最重要的特征，并了解模型如何做出预测。其中一种常用的版本是 Shapley 值，这是一种基于博弈论的模型无关度量，将“支付”（预测）公平地分配给特征[1]。Shapley 值的一种扩展是 KernelSHAP，它使用核技巧以及局部替代模型来近似 Shapley 值，这使得它能够计算更复杂模型（如神经网络）的特征重要性值[2]。

KernelSHAP 通常用于解释时间序列预测，但在这个领域确实存在一些显著的约束和缺陷：

1.  时间序列预测通常涉及大量过去数据，这可能会导致在应用 KernelSHAP 时出现数值下溢错误，尤其是在多变量时间序列预测中[3]。

1.  KernelSHAP *假设特征独立性*。这在表格数据情况下通常有效，但在时间序列中，特征和时间步的独立性往往是一种例外，而非常态[3]。

1.  KernelSHAP 使用已拟合于数据扰动的线性模型的系数。然而，在时间序列的情况下，[向量自回归模型](https://online.stat.psu.edu/stat510/lesson/11/11.2)（VAR）通常比仅使用线性模型[3]更适合建模过程。

为了解决这些问题，J.P. 摩根的 AI 研究部门（Villani 等人）在[他们的 2022 年 10 月论文](https://arxiv.org/abs/2210.02176) [3]中提出了更适合时间序列数据的 KernelSHAP 变体：

1.  研究人员首先创建了 VARSHAP，这是一种使用 VAR 模型而非线性模型的 KernelSHAP 修改版。这种修改使得研究人员还计算了一种封闭形式的方法来计算 AR、MA、ARMA 和 VARMAX 模型的 SHAP 值。

1.  基于 VARSHAP，研究人员提出了**时间一致性 SHAP**，利用问题的时间成分来减少 SHAP 值的计算量。

使用时间一致性 SHAP 度量，研究人员展示了一种通过捕捉特征重要性的激增来进行事件检测的有前途的方法。

在这篇文章中，我将首先解释 KernelSHAP 的计算方法以及如何将其修改为 VARSHAP。然后还将解释如何获取时间一致性 SHAP（TC-SHAP）以及如何使用 TC-SHAP 来识别时间序列分析中的事件。

# KernelSHAP 和 VARMAX

SHAP 值的公式如[2]所提供的：

![](../Images/f9ecd2f80d685b3f98f13e44edd99ff4.png)

方程 1：SHAP 方程

上述方程中的 *phi* 是特征 *i* 的 SHAP 值，给定值函数 *v*（值函数通常是模型预测）。C* 是所有特征的集合，*N* 是 *C* 的大小或特征数量。*P(C)* 是所有不包含特征 *i* 的特征的幂集。Delta(S, i)* 是将特征 *i* 添加到特征联盟 *S*（这是幂集 C 中的一个集合）时导致的预测变化。

该方程总结为“将特征 *i* 的加权边际贡献添加到不包括 *i* 的每个可能特征联盟中”。

KernelSHAP 处理的问题是，随着特征数量的增加，幂集大小呈指数级增长，这使得计算变得极其庞大。KernelSHAP 是通过解决以下问题来计算的：

![](../Images/17eb1f2a73502b7de52804ec1bb1f7db.png)

方程 2：KernelSHAP 方程 [3]

其中 *h_x* 是应用于 *z* 的掩码函数，*z* 是从集合 *Z* 中采样的二进制向量，代表所有可能的特征联盟的集合。这个函数将由 *z* 表示的联盟映射到掩码数据点，然后将其输入到我们的模型中（*f_theta*）。目标是找到最佳的线性模型 (*g*)，该模型在所有掩码下估计模型性能。线性模型中的权重是 KernelSHAP 值。这一切都可以通过以下定义的组合核实现：

![](../Images/3c7457679f8cf49d01d1ebe3a1a20825.png)

方程 3：组合核 [3]

要计算 VARSHAP，只需将 g 的线性表示替换为 VAR 模型。根据作者的说法，由于线性模型和 VAR 模型的系数都是通过普通最小二乘法估计的，因此所有 KernelSHAP 的数学原理仍然适用，并且对时间序列更具代表性 [3]。

# 时间一致性 SHAP

如前所述，SHAP 是一种将模型的特征解释为游戏中的玩家，并使用 Shapley 值来寻找公平奖励分配的方法。然而，对于随时间发展的游戏，这些分配可能不足以激励所有方追求最初目标。为了避免这种情况，博弈论者使用分配计划和时间一致性的概念来管理跨时间的激励 [3]。

这种通过时间的利益竞争思想也扩展到特征上，因为传统的 SHAP 方法将不同时间步的相同特征视为游戏中的不同玩家。根据作者的说法，我们可以通过添加时间一致性来弥合这一差距 [3]。

SHAP 值的时间一致性可以表示如下：

![](../Images/17aa3efbd08190231e322409304921c6.png)

方程 4：Shapley 值的时间一致性 [3]

在这个方程中，*beta* 代表在 *t* 时间步中对玩家（特征）*i* 进行的支付分配计划，而 *phi(0,i)* 是玩家对游戏（预测）贡献的总值。可以将其视为类似于商业伙伴关系。

每个个体（即特征）向启动基金（在时间步 0 时为 phi）支付初始金额。然后，在未来的时间步中，个体会定期获得回报，因为他们对业务结果（即最终预测）作出更多贡献。这些支付也会使任何个体不愿意采取不利于业务利益的行动。通过这种方式表述问题，TC-SHAP 在时间序列背景下表现得更好，因为现在 **特征的不同时间步被建模为一个整体，而不是作为单独的参与者**。

在实际操作中，可以采取以下步骤：

1.  通过将特征替换为零或特征均值来计算每个特征的总 SHAP 贡献（在时间步 0 时为 phi）。对所有特征重复此操作。

1.  然后我们需要计算窗口内每个时间步的“子游戏 SHAP”。这通过修改方程 2 中的掩码机制来完成，即不再只掩码时间步 (*t-w*)，而是掩码 *t-w* 和 *t* 之间的所有时间步（再次使用零或均值）。

1.  然后我们简单地使用方程 4 和 5 计算填充值表。

![](../Images/78d5b69ac8c0bc2065ad6a962d6cb522.png)

方程 5: 填充值表 [3]

第 1 步计算“初始投资”。第 2 步则实施了一个观点，即我们有 *N* 个特征跨越多个时间步 (*W*)，而不是 *N*W* 个特征。第 3 步通过提供填充值表（或每个“投资者”的周期性回报）将所有内容汇总在一起。

这个过程还有一个好处，即将计算次数从 2*^(N*W)* 减少到 *W**2^*N*，其中 *W* 是用于预测的时间步数，而 *N* 是特征的数量 [3]。

一旦计算完成，我们可以将 TC SHAP 值解释为“在给定时间步中，特征的演变如何影响其他特征轨迹的联盟。” 换句话说，TC SHAP 代表了给定时间步的特征如何改变未来时间步中其他特征的共同贡献。对未来协作产生重大影响的特征-时间步点将根据定义对最终预测产生重大影响。

# 使用 TC SHAP 进行事件检测

虽然了解单次预测中某个时间步的重要性是有用的，但时间序列分析通常涉及分析多个预测和模式，我们可能想要了解模型预测中一些重要时间步的总体情况 [3]。

根据作者的说法，我们可以通过将给定预测集（或如果我们想要全球事件检测机制则是所有预测）的 TC SHAP 值相加来找到重要的时间步。通过绘制这些值，我们可以轻松查看哪些时间步很重要以及一些重要事件可能发生在哪里 [3]。

作者通过 [Individual Household Electricity dataset](https://archive.ics.uci.edu/ml/datasets/individual+household+electric+power+consumption) 演示了这种方法的有效性。作者训练了一个 LSTM 网络，后接一个密集层来预测功耗。然后，他们计算了 TC-SHAP 值并将其汇总以获得事件检测卷积。接着，他们将卷积叠加到目标时间序列上。

![](../Images/44b6f699d06a608a4d99267fc68f4a3e.png)

图 1：事件检测卷积（蓝色）用于子计量 2 和 3 与目标的比较（图源自 [3]）

如图 1 所示，目标变量的大幅变化可以通过事件卷积中的大幅峰值来解释。例如，在时间步 25 之后，子计量 2 的事件卷积出现了一个大峰值，随后目标也出现了大幅下降。类似地，在时间步 75 左右，子计量 3 的事件卷积大幅下降后，目标也出现了大幅下降。大多数的大幅变化可以通过一些子计量的变化来解释。

# 结论

对 KernelSHAP 的修改填补了当前工作中的一个巨大空白。除此之外，针对时间序列特征重要性进行开发的事后可解释性方法并不多。TC-SHAP 有助于解决这个问题，确实是迫切需要的。

然而，这种新方法仍然存在一些关注点和进一步的工作需要解决。其中一个关注点（作者也提到）是VARSHAP和TC-SHAP解释之间的显著差异，这表明需要更多的工作来检验这些值的确切解释。此外，尽管理论上TC-SHAP克服了独立性问题，但仍需更多实验来完全确认这一说法。

此外，一般来说，模型无关的方法可能具有误导性，因为它们只能提供重要性的估计，而非真实的重要性。然而，对于大多数使用情况，这种粗略的评估已经足够，并且拥有一个处理时间依赖性的方法非常有用。

# 资源和参考文献

1.  Python 的 SHAP 包：[https://shap.readthedocs.io/en/latest/index.html](https://shap.readthedocs.io/en/latest/index.html)

1.  对 Kernel SHAP 的更深入解释：[https://christophm.github.io/interpretable-ml-book/shap.html#kernelshap](https://christophm.github.io/interpretable-ml-book/shap.html#kernelshap)

**参考文献**

[1] L. Shapley. [n人博弈的价值。](https://books.google.ca/books?hl=en&lr=&id=Pd3TCwAAQBAJ&oi=fnd&pg=PA307&dq=A+value+for+n-person+games.&ots=gtuWLb6iq-&sig=dm0WHMP9kzTY8kx6J75CLsf82wk#v=onepage&q=A%20value%20for%20n-person%20games.&f=false)（1953）。博弈理论贡献 2.28。

[2] S.M. Lundberg, S-I. Lee. [一种统一的模型预测解释方法。](https://arxiv.org/abs/1705.07874)（2017）。神经信息处理系统进展，30。

[3] M. Villani, J. Lockhart, D. Magazzeni. [时间序列数据的特征重要性：改进 KernelSHAP](https://arxiv.org/abs/2210.02176) (2022)。ICAIF 解释性人工智能在金融中的研讨会
