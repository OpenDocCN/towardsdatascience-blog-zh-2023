# 魔法：聚会竞技场：用概率获胜

> 原文：[`towardsdatascience.com/magic-the-gathering-arena-winning-with-probability-b71f363e0ce2`](https://towardsdatascience.com/magic-the-gathering-arena-winning-with-probability-b71f363e0ce2)

## 如何利用概率和 Excel 赢得锦标赛

[](https://medium.edkruegerdata.com/?source=post_page-----b71f363e0ce2--------------------------------)![Edward Krueger](https://medium.edkruegerdata.com/?source=post_page-----b71f363e0ce2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b71f363e0ce2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b71f363e0ce2--------------------------------) [Edward Krueger](https://medium.edkruegerdata.com/?source=post_page-----b71f363e0ce2--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b71f363e0ce2--------------------------------) ·7 分钟阅读·2023 年 5 月 9 日

--

*作者：* [*Edward Krueger*](https://www.linkedin.com/in/edkrueger/) *数据科学家、讲师及* [*E*rin Oefelein](https://www.linkedin.com/in/erin-oefelein-3105a878/) *数据科学家，Taxwell 创始人。*

![](img/e57fabdb61669b1b612b262ea8eb8e56.png)

照片来自[Wayne Low](https://unsplash.com/@wayneshin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/photos/OvN4OkhkTLo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

*作为数据科学家，我们被这个问题吸引，因为我们发现这是一个迷人的例子，展示了看似简单的规则如何导致相当复杂的情况，且需要仔细分析。*

*虽然在数据科学中已经有大量关于常见分布（例如正态分布）的研究，但现实生活中的问题通常需要不那么常见的分布，有时甚至是没有名称的分布：经验法则和天真的事件概率计算，而没有深入了解分布。在本文中，我们将探讨一个需要使用负二项分布的问题，但实际上会做一些调整以适应我们问题的特殊性。*

许多游戏和锦标赛有复杂的规则。为什么？它们利用我们的心理学使游戏更有趣、上瘾，并且通过让我们赢的机会看起来比实际情况高，从而使组织者获得更多收益。当粉丝、玩家或赌徒相信他们能赢时，这是最吸引人的。如果你认为自己的机会比实际情况更好，这种效果最为显著。

在本文中，我们展示了一个来自 MTG Arena 游戏的锦标赛并计算获胜的几率。当讨论这个锦标赛时，通常认为在对战平衡的情况下，玩家平均会赢得 3 到 4 场比赛，而粗略的计算会严重高估获胜的几率。我们将进行数学计算，并展示如何使用 Excel 计算几率——将结果适配到其他统计软件应该是直接的。

![](img/f7f22c036781829a8e0c25331a2e34d6.png)

摄影作品来自 [Ryan Quintal](https://unsplash.com/@ryanquintal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/Magic-the-gathering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

《魔法风暴竞技场》(MTGA) 十项全能由十个不同的赛事组成，持续一个月。每个赛事的入场费为 2,000 金币或 400 宝石。赛事可以是单局对决（Best-Of-One，单场比赛）或三局两胜（Best-Of-Three，三场比赛）。在本文中，我们探讨与单局对决（BO1）MTGA 赛事相关的预期结果。为了获得代币，BO1 游戏要求玩家在遭遇 3 次失败之前赢得 7 场 BO1 比赛。根据这些参数：

1.  **参加赛事时获得代币的概率是多少？**

1.  **玩家需要参加多少场赛事才能赢得代币？** 换句话说，平均而言，玩家需要参加多少场赛事才能赢得 7 次比赛，在遭遇 3 次失败之前，这样才能获得代币？

MTG 是一个运气和技能并存的游戏。尽管运气会引入变化，高技能的玩家通常会赢得更多的比赛。在数学领域，运气通过概率概念来量化。

概率描述了**随机过程**中的不确定性，其范围从 0 到 1。值为 0 表示事件 *不会发生*，而值为 1 表示事件 *会发生*。我们使用随机变量的概念来描述**重复随机过程**，随机变量表示在指定范围内的重复随机过程的输出。随机变量的概率分布是一个统计函数，描述了这些输出在随机变量上的分布。

![](img/c70221dc7d20a89bd88f93613e427de7.png)

摄影作品来自 [Giorgio Trovato](https://unsplash.com/@giorgiotrovato?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/trophy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

概率分布将离散或连续随机变量从函数输入映射到实数输出。根据我们 MTG 问题的参数，我们知道负二项分布建模了我们 MTG 问题中固有的随机过程。负二项分布建模了具有以下属性的重复随机过程：

+   该过程包括重复的试验，每次试验都是独立的，即每次（x）试验的结果不影响其他试验的结果。

+   实验将持续进行，直到观察到某个预定事件（r）。这个预定事件通常被称为“成功”，然而，值得注意的是，它不一定是积极的结果！

+   每次试验只有两种可能的结果，成功（r）或失败（f），其中*成功的概率*（p）在每次试验中相同。

负二项分布描述了一个离散随机变量，因此由称为概率质量函数或 PMF 的概率分布类型建模，PMF 专门用于建模离散概率分布。负二项分布的 PMF，其中随机变量 X 表示*第 r 次成功发生的试验*（r-1），定义为：

![](img/49f3263fc22223f1a5b4b9d7189d59f0.png)

由作者给出的方程

每个变量定义如下：

+   x：在负二项实验中产生 r 次成功的试验次数

+   r：负二项实验中的“成功”次数

+   p：任何给定个体试验中的成功概率

+   q：任何给定个体试验中的失败概率（等于 1-p）

或者，负二项分布可以用随机变量 Y 来定义，Y 代表*在第 r 次成功之前发生的失败次数*（y）。注意，这种替代形式在统计上是显著的，因为 Y = X - r。转换得 Y + r = X。

![](img/f979b4ae7b47623571fc610e43722b56.png)

由作者给出的方程

那么 PMF 为什么重要？它重要的原因在于，它输出了离散随机变量*在某个特定值下*的概率。为了理解离散随机变量*在某个特定值或更小值下*的概率，我们使用累积分布函数或 CDF，这就是随机变量 X 小于或等于 x 的概率。

负二项分布的 PMF 相关变量所取的值决定了负二项分布的确切形状。这个形状由两个（2）参数特征，即**停机参数 r**和**成功概率 p**。因此，我们说负二项分布具有 (r, p) 分布。

记住，我们的目标是建模一个问题，其中玩家必须赢得 7 场 BO1 比赛，才能承受 3 次失败。实际上，玩家不能赢得超过 7 次，但为了用负二项分布来建模，我们将允许 8、9、10 次胜利。这不会影响我们的结果，因为我们将计算在事件中有 7 次胜利的概率，作为在模型中有超过 6 次胜利的概率。

重要的是玩家失败的次数。这是一个重要的区分，因为这决定了我们的**停止参数 r。** 当玩家遭遇 3 次失败时，尝试结束。换句话说，我们要计算的是玩家在遭遇 3 次失败之前赢得*至少* 7 次的概率。因为任何超过 7 次的胜利都是可以接受的，只要失败次数少于 3 次，我们将使用累计分布函数来建模这个问题。

幸运的是，像这样的建模问题使用计算机软件更为简便。Excel 提供了 NEGBINOM.DIST 函数，它需要以下输入：

+   **Number_f:** “失败”的次数

+   **Number_s**: “成功”的次数 -> 停止参数

+   **Probability_s:** 成功的概率

+   **Cumulative:** 取值为 TRUE，表示 CDF 问题，或 FALSE，表示 PMF 问题

这些参数按以下顺序设置：

+   **NEGBINOM.DIST(Number_f, Number_s, Probability_s, Cumulative)**

我们可以定义获得代币的概率以及在不同游戏胜率水平下获得代币的预期尝试次数（在我们的负二项方程中称为 p）：

![](img/0864d11912ddd3afed8ab76c2f2d36f4.png)

作者绘图

要获得代币，玩家必须在输掉 3 场比赛之前至少赢得 7 场比赛。因此，每个事件包含的游戏次数可多可少，直到输掉 3 场比赛或赢得 7 场比赛。

那么我们是如何计算的呢？我们通过将以下内容插入 Excel 中的 NEGBINOM.DIST 方程来计算获得代币的概率以及预期事件次数：

**1 — NEGBINOM.DIST(Number_f=6, Number_s=3, Probability_s=1-game win rate, Cumulative=TRUE)**

+   **Number_f:** 请记住，CDF 表示随机变量 Y 小于或等于指定值 y 的概率，数学上表示为 P(Y <= y)。要找到赢得 7 场比赛的概率，我们可以先找到其补集的概率：**未** 赢得 7 场比赛的概率。数学上，这就是 P(Y < 7) = P(Y <= 6)，这是在 6 处计算的 CDF。然后，我们可以将赢得 7 场比赛的概率计算为 P(Y=7) = 1 — P(Y<7) = 1 — P(Y<=6)。

    **Number_s**: *停止参数，* 等于 3 次失败

我们将“成功”定义为失败。因此，我们将定义**“成功次数或停止参数”**的第二个参数设为 3。

+   **Probability_s:** 1 — 游戏胜率

因为我们定义了“成功”为玩家失败的概率，我们将使用游戏胜率的逆作为**成功的概率**的参数，等于 1 — 游戏胜率。

+   **Cumulative:** 设为 TRUE，因为这个问题被定义为 CDF

因此，要在 50% 的尝试中获得代币，玩家必须拥有至少 71.36% 的游戏胜率。

# **结论**

对于任何涉及随机性的游戏，扎实的概率知识都会派上用场。在这里，我们展示了如何应用概率概念来正确计算在不同条件下获胜的几率。了解所有可能的结果及每个结果发生的概率，有助于做出更明智的决策，并希望能帮助你获得胜利。

**来源：**

1.  [`www.britannica.com/science/statistics/Random-variables-and-probability-distributions`](https://www.britannica.com/science/statistics/Random-variables-and-probability-distributions)

1.  [`en.wikipedia.org/wiki/Random_variable`](https://en.wikipedia.org/wiki/Random_variable)
