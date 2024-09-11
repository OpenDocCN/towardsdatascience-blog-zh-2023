# 多臂老虎机应用于执行算法中的订单分配

> 原文：[https://towardsdatascience.com/multi-armed-bandits-applied-to-order-allocation-among-execution-algorithms-fff21dedc927?source=collection_archive---------10-----------------------#2023-03-02](https://towardsdatascience.com/multi-armed-bandits-applied-to-order-allocation-among-execution-algorithms-fff21dedc927?source=collection_archive---------10-----------------------#2023-03-02)

## 找到**利用**与*探索*之间的正确平衡

[](https://medium.com/@larsterbraak?source=post_page-----fff21dedc927--------------------------------)[![Lars ter Braak](../Images/79a2bbfbe8706c2451826049e3e2d8e7.png)](https://medium.com/@larsterbraak?source=post_page-----fff21dedc927--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fff21dedc927--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fff21dedc927--------------------------------) [Lars ter Braak](https://medium.com/@larsterbraak?source=post_page-----fff21dedc927--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff1d3961756e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-armed-bandits-applied-to-order-allocation-among-execution-algorithms-fff21dedc927&user=Lars+ter+Braak&userId=f1d3961756e7&source=post_page-f1d3961756e7----fff21dedc927---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fff21dedc927--------------------------------) · 6 min 阅读 · 2023年3月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffff21dedc927&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-armed-bandits-applied-to-order-allocation-among-execution-algorithms-fff21dedc927&user=Lars+ter+Braak&userId=f1d3961756e7&source=-----fff21dedc927---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffff21dedc927&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmulti-armed-bandits-applied-to-order-allocation-among-execution-algorithms-fff21dedc927&source=-----fff21dedc927---------------------bookmark_footer-----------)![](../Images/4aa78a1ebd02691dc382392a434cb265.png)

困惑的机器人以毕加索风格观察三台单臂老虎机。来源：DALL-E 2。

在不确定性下做出决策是各个领域的专业人士面临的常见挑战，包括数据科学和资产管理。资产经理在选择多个执行算法来完成交易时会遇到这个问题。算法之间的订单分配类似于赌徒面临的多臂老虎机问题，他们必须决定玩每台老虎机的次数、顺序以及是否继续当前机器或切换到另一台。在这篇文章中，我们描述了资产经理如何根据实际执行成本最佳地分配订单到可用算法中。

## 示例

对于每个订单，我们采取行动*a*，将其分配给*K*个算法中的一个

![](../Images/604ac3185d674ae9b4d86f32aae31c91.png)

Eq. 1: 分配订单到K个算法中的一个的可能行动集合。

行动*a*的价值是该算法的预期执行成本

![](../Images/81fdf713fc23730a0ac9ab393150107d.png)

Eq. 2: (未观察的) 行动a的预期执行成本，即选择某个算法。

假设*K = 3*，并且算法的预期执行成本为

![](../Images/082e972322dab17604114dadcd127ea0.png)

Eq. 3: (未观察的) 三个算法的预期执行成本。

如果你事先知道行动值，解决问题会非常简单。你将始终选择预期执行成本最低的算法。现在假设我们开始按照图1中所示在三个算法之间分配订单。

![](../Images/99b76e27a17b4638bba7a8db92b5de80.png)

图1: 在三个算法之间分配订单及其相关执行成本的示例。来源：作者。

我们仍然不确定行动值，但在一段时间*t*后，我们有了估计：

![](../Images/b59cc23f6af13e112a07e1993daa40a7.png)

Eq. 4: (观察的) 基于截至时间t的信息，行动a的预期执行成本。

例如，我们可以构建每个算法的执行成本¹的经验分布，如图2所示。

![](../Images/cfc1fbe77b98e44fae885343d3740419.png)

图2: 一段时间t后每个算法的执行成本的经验分布。来源：作者。

将所有订单分配给预期执行成本最低的算法可能看起来是最佳方法。然而，这样做会阻止我们收集其他算法性能的信息。这体现了经典的多臂老虎机困境：

+   利用已经学到的信息

+   探索以了解哪些行动能获得最佳结果

目标是**最小化平均执行成本**，在分配*P*个订单之后：

![](../Images/42a442d21839f39b066048d565246519.png)

Eq. 5: 订单分配问题的目标函数。

## 使用策略解决问题

为了解决这个问题，我们需要一个动作选择策略，它告诉我们如何基于当前信息*S*分配每个订单。我们可以将策略定义为从*S*到*a*的映射：

![](../Images/b6e38cf7191513324dc8acd814c6a6ce.png)

Eq. 6: 动作选择策略的定义。

我们讨论了最著名的多臂赌博机问题策略²，这些策略可以分为以下几类：

+   **半均匀策略：** *贪婪 &* *ε-贪婪*

+   **概率匹配策略：** *上置信界限 & 汤普森采样*

## *贪婪*

*贪婪方法*将所有订单分配给估计值最低的动作。该策略总是利用当前知识以最大化即时奖励：

![](../Images/91851e1c2265209712ca67b5f37fd516.png)

Eq. 7: 贪婪方法的动作选择策略。

## ϵ-贪婪

*ε-贪婪方法*大多数时候表现得很贪婪，但以概率*ε*随机选择次优动作：

![](../Images/d97e34f46b3fc27bed27c9430a6c8394.png)

Eq. 8: ϵ-贪婪方法的动作选择策略。

该策略的一个优点是，它在极限情况下会收敛到最优动作。

## 上置信界限

*上置信界限（UCB）方法*选择具有最低动作值*减去*一个与交易算法使用次数成反比的项，即*Nt(a)*。该方法在非贪婪动作中选择，依据其实际最优潜力及这些估计的相关不确定性：

![](../Images/302f652ce4b8def7b38188decddadaff.png)

Eq. 9: 上置信界限（UCB）方法的动作选择策略。

## 汤普森采样

*汤普森采样方法*，如汤普森（1933）所提，假设对动作值有一个已知的初始分布，并在每次订单分配后更新分布。该方法根据动作的后验概率选择最佳动作：

![](../Images/2b51308774601572de67352cfe760f88.png)

Eq. 10: 汤普森采样方法的动作选择策略。

## 策略评估

在实际应用中，策略通常通过*遗憾*来评估，遗憾是与最优解的偏差：

![](../Images/01a002b041e58e03fd373b02962bb370.png)

Eq. 11: 遗憾作为一系列动作的函数的定义。

其中*μ*是最小执行成本均值：

![](../Images/4d3cd7291df6ad7f70777c3bd3a37b0a.png)

Eq. 12: 选择最优动作的期望执行成本。

动作是策略的直接结果，因此我们也可以将遗憾定义为所选策略的函数：

![](../Images/9aa74ffc6a17bd11d09b049e5d37e3d8.png)

Eq. 13: 遗憾作为动作选择策略π的函数的定义。

在图3中，我们在示例中模拟了上述策略的遗憾。我们观察到*上置信界限方法*和*汤普森采样方法*表现最佳。

![](../Images/502478bbb0858eae530f379beb661fd9.png)

图 3：针对虚拟订单分配问题的不同动作选择策略的模拟遗憾。来源：作者。

## 订单分配？拥抱不确定性！

虚拟示例模拟结果强烈表明，单纯依赖*贪婪方法*可能无法获得最佳结果。因此，在制定订单分配策略时，融入并衡量执行成本估算的不确定性至关重要。

## 脚注

¹ 为确保执行成本的经验分布的可比性，我们需要分配类似的订单或使用无关订单的成本指标进行评估。

² 在算法的执行成本依赖于顺序特征的情况下，上下文强盗算法是更合适的选择。要了解更多关于这种方法的信息，我们推荐 Barto & Sutton (2018) 的第 2.9 章作为入门。

³ 我们强烈建议阅读 Russo 等 (2018) 作为学习 Thompson 采样的杰出资源。

## 额外资源

以下教程 / 讲座对我理解多臂强盗问题非常有帮助。

## 行业

1.  研究科学家 Robert Schapire @ [Microsoft](https://www.youtube.com/watch?v=N5x48g2sp8M&ab_channel=SimonsInstitute)

1.  研究科学家 Hado van Hasselt @ [Deepmind](https://www.youtube.com/watch?v=TCCjZe0y4Qc&ab_channel=DeepMind)

## **学术界**

1.  助理教授 Christina Lee Yu @ [Cornell](https://www.youtube.com/watch?v=w1pRb8SGdcw&t=446s&ab_channel=SimonsInstitute)

1.  助理教授 Emma Brunskill @ [MIT](https://www.youtube.com/watch?v=FgzM3zpZ55o&list=PLoROMvodv4rOSOPzutgyCTapiGlY2Nd8u&ab_channel=StanfordOnline)

## 参考文献

[1] Sutton, R. S., & Barto, A. G. (2018). 强化学习：导论。*麻省理工学院出版社*。

[2] Russo, D. J., Van Roy, B., Kazerouni, A., Osband, I., & Wen, Z. (2018). 关于 Thompson 采样的教程。*机器学习基础与趋势®*，*11*(1)，1–96。

[3] Thompson, W. 1933\. 在两个样本的证据下，一个未知概率超过另一个的可能性。*生物统计学*。25(3/4): 285–294。

[4] Thompson, W. R. 1935\. 关于配额理论。*美国数学杂志*。57(2): 450–456。

[5] Eckles, D. 和 M. Kaptein. 2014\. 在线自助法的 Thompson 采样。*arXiv 预印本 arXiv:1410.4009*。

如果你对更多内容感兴趣，可以查看我下面的一些文章：

[](https://medium.com/@larsterbraak/cost-decomposition-for-a-vwap-execution-algorithm-buy-side-perspective-1126f9eebf40?source=post_page-----fff21dedc927--------------------------------) [## VWAP 执行算法的成本分解：买方视角

### 用于 VWAP 执行算法的线性成本分解，允许更快且更细致的算法交易……

[medium.com](https://medium.com/@larsterbraak/cost-decomposition-for-a-vwap-execution-algorithm-buy-side-perspective-1126f9eebf40?source=post_page-----fff21dedc927--------------------------------) [](/introduction-to-probabilistic-classification-a-machine-learning-perspective-b4776b469453?source=post_page-----fff21dedc927--------------------------------) [## 引言：概率分类的机器学习视角

### 从预测标签到预测概率的指南

towardsdatascience.com](/introduction-to-probabilistic-classification-a-machine-learning-perspective-b4776b469453?source=post_page-----fff21dedc927--------------------------------) [](https://medium.com/@larsterbraak/beyond-traditional-return-modelling-embracing-thick-tails-67f457dfbf6b?source=post_page-----fff21dedc927--------------------------------) [## 超越传统资产回报建模：拥抱厚尾。

### 针对预渐近性的统计推断指南。

[medium.com](https://medium.com/@larsterbraak/beyond-traditional-return-modelling-embracing-thick-tails-67f457dfbf6b?source=post_page-----fff21dedc927--------------------------------)
