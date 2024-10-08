# 解释性神经基础模型

> 原文：[`towardsdatascience.com/neural-basis-models-for-interpretability-fd04ac958ff2`](https://towardsdatascience.com/neural-basis-models-for-interpretability-fd04ac958ff2)

## 解读 Meta AI 提出的新解释性模型

[](https://medium.com/@upadhyan?source=post_page-----fd04ac958ff2--------------------------------)![Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----fd04ac958ff2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fd04ac958ff2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd04ac958ff2--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----fd04ac958ff2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fd04ac958ff2--------------------------------) ·6 分钟阅读·2023 年 10 月 11 日

--

机器学习和人工智能在各个领域的广泛应用带来了更高的风险和伦理评估挑战。如在 [ProPublica 报道的刑事再犯模型](https://www.propublica.org/article/machine-bias-risk-assessments-in-criminal-sentencing) 中所见，机器学习算法可能存在严重的偏见，因此需要强有力的解释机制，以确保这些模型在高风险领域的信任和安全。

那么，我们如何在解释性、准确性和模型表现力之间取得平衡呢？Meta AI 的研究人员提出了一种新的方法，称为[神经基础模型（NBMs）](https://proceedings.neurips.cc/paper_files/paper/2022/hash/37da88965c016dca016514df0e420c72-Abstract-Conference.html)，这是一种广义加性模型的子家族，在基准数据集上实现了**最先进**的性能，同时保持了**透明的解释性**。

在这篇文章中，我旨在解释 NBM 及其为何是一个有益的模型。像往常一样，我鼓励大家阅读原始论文。

如果你对解释性机器学习和其他伦理 AI 方面感兴趣，考虑查看我的其他文章并关注我！

![Nakul Upadhya](img/e62aa67aa11cd0f9bcd1132257fc3773.png)

[Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----fd04ac958ff2--------------------------------)

## 解释性与伦理 AI

[查看列表](https://medium.com/@upadhyan/list/interpretable-and-ethical-ai-f6ee1f0b476d?source=post_page-----fd04ac958ff2--------------------------------)5 篇故事![](img/3718151c0f72303f3d1c71f54229bc98.png)![](img/eddb4279ebae7fc1ba79cf6dcc6ebd5a.png)![](img/7def8e23dad656929857f2a488b1f547.png)

# 背景：GAMs

NBM 被认为是广义加性模型（GAM）。GAM 本质上是可解释的模型，为每个特征学习一个形状函数，预测是通过“查询”形状函数来完成的。由于这些形状函数是独立的，通过可视化这些形状函数可以理解特征对预测的影响，使得模型高度可解释。变量之间的交互通过将多个变量传递到同一函数中并基于此构建形状函数来建模（通常将变量数量限制为 2 以提高互操作性），这种配置称为 GA2M。

![](img/742a70d6f2f1f6bd50d99c2bca1aab04.png)

GAMs 和 GA2Ms 的方程（图源自 Radenovic 等人 [1]）

各种 GAM 和 GA2M 模型使用不同的机制来开发这些形状函数。可解释增强机（EBM）[2] 使用一组对每个特征进行训练的提升树，神经加性模型（NAMs）[3] 对每个特征使用深度神经网络，而 NODE-GAM [4] 使用 [无意识神经树](https://medium.com/towards-data-science/node-tabular-focused-neural-trees-ee08c752fcd2)[6] 的集合。我推荐阅读以下文章以获得对这些模型的更详细解释：EBM 和 [NODE-GAM/NAM](https://medium.com/@chkchang21/interpretable-deep-learning-models-for-tabular-data-neural-gams-500c6ecc0122)。

# NBM 方法

神经基础模型（NBM）是一类新的广义加性模型（GAMs）子家族，利用形状函数的基础分解。

![](img/ca87a05f2e6c1c910d702e09e31938c9.png)

NBM 架构（图源自 Radenovic 等人 [1]）

与其他 GAM 模型（如 NAM[3]）不同，后者有效地为每个特征训练独立模型以构建形状函数，而 NBM 架构则依赖于少量的基础函数，这些函数在所有特征中共享，并为特定任务共同学习。这些函数是什么？它们是函数逼近的瑞士军刀：深度神经网络。

实际上，一个通用的 MLP 主干网络接受 1 个输入并输出 *B* 个值，这些值被训练并应用于每个输入特征。这些输出然后被线性组合以形成给定特征的最终预测，而线性组合的权重对每个特征不同。另一种思考这种架构的方法是通过编码器-解码器网络的视角。所有特征共享相同的 *编码器*（通用 MLP 主干），但每个特征都有其自己的 *解码器*（编码的线性变换）。每个特征的解码值然后被加总以生成最终预测。

这可以很容易地扩展到包括特征交互。如果我们想要建模配对交互，我们可以包括一个接受两个输入的 MLP，而不是一个。

![](img/223677c0fc646eb67ffc56c432711152.png)

NBM 和 NB2M 方程（图源自 Radenovic 等人 [1]）

使用共享 MLP 主干而不是为每个特征使用不同的 MLP 的一个好处是模型的显著较小的尺寸。这使得 NBM 非常适合处理极高维度数据的任务。

# 性能与优势

为了测试他们的架构，Radenovic 等人（2022 年）将 NBM 与各种其他模型进行了比较，如线性回归、EBM [2]、NAM [3]、XGBoost[5] 和 MLP。他们的首次评估是在混合的表格和图像数据集上进行的。

![](img/6627de5d6dd91ec42eea5363382b0db2.png)

基准性能比较（图来自 Radenovic 等人 [1]）

总的来说，NBM 牢牢站稳脚跟，超越了其他可解释模型，甚至在某些数据集上超过了 MLP。

Radenovic 等人（2022 年）还在纯表格数据集上进行了另一项评估，重点是对比 SOTA GAM 模型。

![](img/3d9dbebf2587c6c4e2e6d7d53019b098.png)

与其他 GAMs 的性能比较：（图来自 Radenovic 等人 [1]）

这个比较清楚地展示了 NBM 的强大，几乎在每个数据集上都击败了竞争对手。如前所述，NBM 的可扩展性也非常出色。如下所示，在高维数据任务中，NBM 的参数数量几乎是 NAM 的 70 分之一。

![](img/4cb1f55b382cb1d68c132d295e0fd895.png)

NAM/NBM 参数比较。X 轴是数据的维度（图来自 Radenovic 等人 [1]）

# 结论

总体而言，NBM 是极其强大且轻量级的模型，由于其为 GAM，本质上是可解释的。然而，这并不意味着它是解决高风险机器学习问题的灵丹妙药。在使用这些模型时，仍然需要考虑许多因素。例如，一个本质上可解释的模型几乎没有意义，如果[输入到模型中的特征不可解释](https://medium.com/towards-data-science/defining-interpretable-features-ebd7ed94897)。

此外，虽然 NBM 的规模相较于 NAM 扩展得很好，但可解释性却没有。没有人能查看数千个特征归因图，特别是当这些归因图还包含成对交互时。这意味着在大参数空间下，仍然需要预处理方法，如特征选择，甚至作者也承认了这一点。然而，这并不贬低作者，因为这是一个仍然非常有用且相对容易实现和调整的模型。

该模型是 GAM 的事实对于机器学习应用（如移动设备和其他性能较低的设备）也非常有利，因为用户可以训练模型并部署生成的特征归因函数，而不是完整模型，从而实现极其快速且内存轻量的推理，而不会损失准确性。

# 资源与参考文献

1.  NBM 代码：[`github.com/facebookresearch/nbm-spam`](https://github.com/facebookresearch/nbm-spam)

1.  NBM 开放评论：[`openreview.net/forum?id=fpfDusqKZF`](https://openreview.net/forum?id=fpfDusqKZF)

1.  如果你对可解释的机器学习或时间序列预测感兴趣，可以考虑关注我：[`medium.com/@upadhyan`](https://medium.com/@upadhyan)。

1.  查看我关于可解释机器学习的其他文章：[`medium.com/@upadhyan/list/interpretable-and-ethical-ai-f6ee1f0b476d`](https://medium.com/@upadhyan/list/interpretable-and-ethical-ai-f6ee1f0b476d)

**参考文献**

[1] Radenovic, F.、Dubey, A. 和 Mahajan, D.（2022）。用于可解释性的神经基础模型。*神经信息处理系统进展*，*35*，8414–8426。

[2] Yin L.、Rich C.、Johannes G. 和 Giles H.（2013）准确可理解的模型与配对交互。在*第 19 届 ACM SIGKDD 国际会议上的知识发现与数据挖掘论文集，623–631\. 2013*。

[3] Agarwal, R.、Melnick, L.、Frosst, N.、Zhang, X.、Lengerich, B.、Caruana, R. 和 Hinton, G. E.（2021）。神经加性模型：使用神经网络的可解释机器学习。*神经信息处理系统进展*，*34*，4699–4711。

[4] Chang, C.H.、Caruana, R. 和 Goldenberg, A.（2022）。NODE-GAM：用于可解释深度学习的神经广义加性模型。在*国际学习表征会议*。

[5] Chen, T. 和 Guestrin, C.（2016 年 8 月）。Xgboost：一个可扩展的树提升系统。在*第 22 届 ACM SIGKDD 国际会议上的知识发现与数据挖掘论文集*（第 785–794 页）。

[6] Popov, S.、Morozov, S. 和 Babenko, A.（2019）。神经网络忽略决策集成用于表格数据的深度学习。在*第八届国际学习表征会议*。
