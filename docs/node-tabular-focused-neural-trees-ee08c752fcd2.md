# NODE：专注于表格数据的神经树

> 原文：[`towardsdatascience.com/node-tabular-focused-neural-trees-ee08c752fcd2`](https://towardsdatascience.com/node-tabular-focused-neural-trees-ee08c752fcd2)

## 探索 NODE：一种用于表格数据的神经决策树架构

[](https://medium.com/@upadhyan?source=post_page-----ee08c752fcd2--------------------------------)![Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----ee08c752fcd2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ee08c752fcd2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee08c752fcd2--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----ee08c752fcd2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ee08c752fcd2--------------------------------) ·7 分钟阅读·2023 年 7 月 4 日

--

近年来，机器学习迅猛发展，神经深度学习模型在图像和文本处理等复杂任务中已经超越了像 XGBoost [4] 这样的浅层模型。然而，深度模型在处理表格数据时往往不如这些浅层模型有效，目前尚未有一种通用的深度学习方法能够 consistently 超越梯度提升树。

为了解决这一差距，俄罗斯互联网服务公司 Yandex 的研究人员提出了一种新的架构：神经遗忘决策集成（NODE） [1]。该网络利用轻量级且可解释的神经决策树，并将其整合到神经网络框架中。这使得模型能够在保持可解释性的同时，捕捉表格数据中的复杂交互和依赖关系。

在这篇文章中，我旨在解释 NODE 的工作原理以及使其成为一个强大而可解释的预测模型的各种属性。像往常一样，我鼓励大家阅读原始论文。如果你想使用 NODE，请查看模型的 GitHub。

本文是关于神经决策树系列中的一部分，这些高度可解释的架构提供了与传统深度网络相当的预测能力。

![Nakul Upadhya](img/e62aa67aa11cd0f9bcd1132257fc3773.png)

[Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----ee08c752fcd2--------------------------------)

## 软/神经决策树

[查看列表](https://medium.com/@upadhyan/list/softneural-decision-trees-3b4a9cb97b84?source=post_page-----ee08c752fcd2--------------------------------)3 个故事![](img/1c7011f3e0e8d04b22fb1f8449f06dec.png)![](img/edc08c13ec51a68ac192363a19540624.png)![](img/4880435999eae926094fadd0d5cb9442.png)

# NODE 决策树结构

## 神经决策树

本文假设你对神经决策树有一定的了解。如果你没有，我强烈建议阅读我之前关于它们的文章以获得深入的解释。然而，总结来说：神经决策树是既柔软又倾斜的决策树。

倾斜树是指在每个节点中使用多个变量来做出决策（通常以线性组合的形式排列）。例如，为了预测汽车事故，正交树可能使用规则“car_speed — speed_limit <10”来产生分支决策。这不同于像 CART（基本决策树）这样的“正交”树，后者在任何给定的节点只使用一个变量，并且需要更多的节点来近似相同的决策边界。

柔软树是指所有分支决策都是概率性的，每个节点的计算定义了进入特定分支的*概率*。这与像 CART 这样的普通“硬”决策树不同，后者的每个分支决策是确定性的。

由于树不限制每个节点使用的变量数量，并且分支决策是连续的，因此整个树是可微分的。由于整个树是可微分的，它可以集成到任何神经网络框架中，如 Pytorch 或 Tensorflow，并使用传统神经优化器（例如，随机梯度下降和 Adam）进行训练。

## NODE 树

NODE 使用的决策树与传统神经树略有不同。让我们逐一分析这些差异。

![](img/9258a53fd48e5c7664d4e8767e135e3a.png)

NODE 树。F(*)表示分支函数，b 表示分支阈值。Sigma 表示概率转换函数。R 是叶节点结果（图来自 Popov 等人，2019 年[1]）

***无意识特性***

第一个重大变化是树的特性是“无意识”的。**这意味着树在相同深度的所有内部节点上使用相同的分裂权重和阈值。**因此，无意识决策树（ODTs）可以表示为一个具有 2^*d*个条目的决策表（*d*为深度）。一个好处是，ODTs 比传统决策树更具可解释性，因为决策更少，更容易可视化和理解决策路径。然而，与传统决策树相比，ODTs 的学习能力显著较弱（再次由于分裂函数的受限特性）。

那么如果我们的目标是性能，为什么要使用 ODTs 呢？正如 CATBoost 的开发者[2]所展示的，ODTs 在集成在一起时效果非常好，并且不易过拟合数据。此外，ODTs 的推理非常高效，因为所有分裂可以并行计算，迅速找到表中的合适条目。

***用于特征选择和分支的 Entmax***

NODE 相对于传统神经决策树的第二个改进是其架构中使用了 alpha-entmax [3]而不是 sigmoid。Alpha-entmax 是 softmax 的一个广义版本，能够产生稀疏分布，其中大部分结果为零。这种稀疏性由一个参数（因此得名 alpha）控制，alpha 值越高，分布越稀疏。

![](img/dbaae8a85e3f549e40aa07961ff59b5d.png)

图源于 Peters 等人 2019 年[3]

这种变换在两个关键地方使用。第一个使用场景是稀疏特征选择。NODE 包括一个可训练的特征选择权重矩阵 F（大小为 d x n，其中 n 是特征数量，d 是树的深度），通过 entmax 变换。由于 entmax 变换的大多数条目都等于零，这自然会导致在每个决策节点中使用的特征数量很少。

![](img/cf1df8348fad7fad614485c598803124.png)

分支函数（图源于 Popov 等人 2019 年[1]）

除了特征选择，entmax 还用于分支概率。这是通过传递分支函数的结果，减去一个学习的阈值，然后适当地缩放来完成的。然后将这个值与 0 串联，并传入 entmax 函数，以创建一个 2 类概率分布，这正是我们进行分支所需的。

![](img/a69565b0df4693b377f92b3454ed0348.png)

分支方程见[1]。b_i 是分支阈值，tau_i 是缩放数据的学习值（图由作者提供）

使用这个，我们可以通过计算所有分支分布 c 的外积来定义一个“选择”张量 C。然后可以将其与叶子中的值相乘，以创建网络的结果。

***集成***

正如名字所示，这些神经遗忘决策树会被集成在一起。一个 NODE 层被定义为*m*棵单独树的串联，每棵树都有自己的分支决策和叶值。如前所述，这种集成与单个树的遗忘性质协同作用，有助于提高准确性，同时减少过拟合的可能性。

# 多层 NODE

NODE 是一个灵活的架构，可以单独训练（结果是单一的决策树集成）或使用复杂的多层结构，其中每组集成从前一层获取输入。

![](img/18de99eba40cfb3d8eebc3af2ad104fa.png)

多层 NODE 架构（图源于 Popov 等人 2019 年[1]）

NODE 的多层架构紧密跟随了流行的 DenseNet 架构。每个 NODE 层包含若干棵树，其输出被串联起来作为后续层的输入。最终输出是通过对所有层中所有树的输出进行平均获得的。由于每一层依赖于所有之前预测的链条，网络能够捕捉到复杂的依赖关系。

# 实验性能

为了测试他们的架构，Popov 等人（2019 年）将 NODE 与 CatBoost [2]、XGBoost [4]、全连接神经网络、mGBDT [5] 和 DeepForest [6]进行了比较。他们的方法涉及在六个不同的数据集上测试这些模型。具体来说，他们进行了使用每个模型默认参数的比较，以及另一项使用调整后的超参数的比较。

![](img/a78387bdd0ef1915eeb400ca95e24d1b.png)

NODE 与其他模型的比较结果（图来源于 Popov 等人，2019 年）

NODE 的实验结果极为令人鼓舞。例如，NODE 架构在所有其他模型的默认参数下表现优于它们。即使在调整参数的情况下，NODE 在 6 个选定数据集中的 4 个数据集上仍优于大多数其他模型。

# 结论

通过将决策树的优势融入神经网络架构，NODE 为深度学习在结构化表格数据普遍存在的领域（如金融、医疗保健和客户分析）开辟了新的可能性。

不过，这并不是说 NODE 是完美的。例如，架构的集成意味着使用神经决策树所获得的许多局部可解释性收益被舍弃，模型中只能获得全局特征重要性。然而，这一架构确实提供了改进神经可解释性的基础构件，并且已提出了一个[后续模型 (NODE-GAM [7])](https://medium.com/@chkchang21/interpretable-deep-learning-models-for-tabular-data-neural-gams-500c6ecc0122)以弥合可解释性差距。

此外，虽然 NODE 在许多浅层模型中表现优异，但我使用它的经验表明，即使使用 GPU，训练时间也较长（这一结论得到了论文作者提供的实验结果的支持）。

总体而言，这是一种极具前景的方法，我计划在未来开发的深度学习模型中积极使用它作为一个组件。

# 资源与参考文献

1.  NODE 论文：[`arxiv.org/abs/1909.06312`](https://arxiv.org/abs/1909.06312)

1.  NODE 代码：[`github.com/Qwicen/node`](https://github.com/Qwicen/node)

1.  NODE 也可以在 Pytorch Tabular 包中找到：[`github.com/manujosephv/pytorch_tabular`](https://github.com/manujosephv/pytorch_tabular)

1.  如果你对可解释机器学习或时间序列预测感兴趣，可以考虑关注我：[`medium.com/@upadhyan`](https://medium.com/@upadhyan)。

1.  查看我关于神经决策树的其他文章：[`medium.com/@upadhyan/list/3b4a9cb97b84`](https://medium.com/@upadhyan/list/3b4a9cb97b84)

## 参考文献

[1] Popov, S., Morozov, S., & Babenko, A. (2019). Neural oblivious decision ensembles for deep learning on tabular data. *第八届国际学习表征会议*.

[2] Prokhorenkova, L., Gusev, G., Vorobev, A., Dorogush, A. V., & Gulin, A. (2018). CatBoost: unbiased boosting with categorical features. *神经信息处理系统进展*, *31*.

[3] Peters, B., Niculae, V., & Martins, A. (2019). 稀疏序列到序列模型。发表于*第 57 届计算语言学协会年会论文集*（第 1504–1519 页）。计算语言学协会。

[4] Chen, T., & Guestrin, C. (2016 年 8 月). Xgboost: 一个可扩展的树提升系统。发表于*第 22 届 ACM SIGKDD 国际知识发现与数据挖掘会议论文集*（第 785–794 页）。

[5] Feng, J., Yu, Y., & Zhou, Z. H. (2018). 多层梯度提升决策树。*神经信息处理系统进展*, *31*。

[6] Zhou, Z. H., & Feng, J. (2019). 深度森林。*国家科学评论*, *6*(1), 74–86。

[7] Chang, C.H., Caruana, R., & Goldenberg, A. (2022). NODE-GAM: 神经广义加性模型用于可解释的深度学习。发表于*国际学习表征会议*。
