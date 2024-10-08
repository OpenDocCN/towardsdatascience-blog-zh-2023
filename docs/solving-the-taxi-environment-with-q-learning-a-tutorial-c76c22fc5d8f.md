# 使用 Q 学习解决出租车环境——教程

> 原文：[`towardsdatascience.com/solving-the-taxi-environment-with-q-learning-a-tutorial-c76c22fc5d8f`](https://towardsdatascience.com/solving-the-taxi-environment-with-q-learning-a-tutorial-c76c22fc5d8f)

## 在动画 Jupyter Notebook 中实现 Q 学习，以解决 OpenAI Gym 的 Taxi-v3 环境

[](https://wvheeswijk.medium.com/?source=post_page-----c76c22fc5d8f--------------------------------)![Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----c76c22fc5d8f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c76c22fc5d8f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c76c22fc5d8f--------------------------------) [Wouter van Heeswijk, PhD](https://wvheeswijk.medium.com/?source=post_page-----c76c22fc5d8f--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c76c22fc5d8f--------------------------------) ·阅读时长 8 分钟·2023 年 3 月 20 日

--

![](img/c60a0923b3fdf481b97b7be803bcebf7.png)

图片由 [Alexander Redl](https://unsplash.com/@alexanderredl?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

OpenAI Gym 中出租车环境的目标——是的，来自于 [ChatGPT 背后的公司](https://medium.com/datadriveninvestor/how-does-the-company-behind-chatgpt-and-dall-e-make-its-money-d6aa5121a849) 和 Dall⋅E——是简单直接的，为强化学习（RL）领域提供了一个很好的入门介绍。

本文提供了一个逐步指南，介绍如何实现环境、使用表格 Q 学习学习策略，并通过动画可视化学习到的行为。如果你想在 RL 中迈出第一步，这个教程可能正适合你。

# 问题设置

在出租车环境中，一辆出租车需要接载乘客并将他们送到一个小停车场的目的地，尽可能地走最短路径。使用强化学习（RL）学习这样一个任务有多难呢？让我们来看看。

在深入实现之前，最好先将设置表述为 [马尔可夫决策过程](https://medium.com/towards-data-science/the-five-building-blocks-of-markov-decision-processes-997dc1ab48a7)（MDP），以掌握问题的数学结构。

## 状态空间

一个 MDP 状态包含以下信息：（i）确定动作，（ii）计算动作奖励，（iii）计算过渡到下一个状态。

最明显的状态属性是出租车的位置；5*5 的网格给我们 25 个选项。此外，等待接送的乘客可以在 1 或 4 个点（标记为 Y、R、G、B）等待，或者他们可以在出租车内，总共提供了（4+1）个选项。最后，目的地也可以是 Y、R、G 或 B。我们用以下向量表示状态：

`State = [x_pos_taxi, y_pos_taxi, pos_passenger, dest_passenger]`

尽管这是一个非常简单的问题设置，但我们已经处理了 5*5*(4+1)*4=500 个状态！将问题规模扩大到数千个位置，就很容易看出 MDP 的规模如何迅速膨胀（所谓的维度诅咒）。

> **可达**的状态数量稍微小于 500，例如，乘客不会有相同的接送点和目的地。由于建模复杂性，我们通常关注完整的状态空间，代表最坏情况。

## 动作空间

状态空间可能比你一开始预期的要大，但动作空间仅包含六个决策：

+   下

+   上

+   左

+   右

+   接送乘客

+   接送乘客

如果动作不被允许（例如，尝试穿越墙壁，或在当前地点没有客户时尝试接客），代理将保持不动。

action_mask（在`env.action_mask(state)`中找到）告诉我们哪些动作是可行的，但这个可行性检查并不会自动强制执行。

## 奖励函数

在这个环境中可以区分三种类型的奖励：

+   **移动：-1。** 每次移动都会带来小惩罚，以鼓励从起点到终点沿最短路径行驶。

+   **失败的接送：-10。** 当乘客被放置在错误的位置时，自然会感到不满，因此应该给予较大的惩罚。

+   **成功的接送：20。** 为了鼓励期望的行为，给予较大的奖励。

> 设计奖励的方式以鼓励期望的行为称为奖励塑形。例如，我们可以在成功接送后简单地结束游戏而不给予奖励（从而结束成本流），但明确的奖励信号通常大大简化学习。

## 转移函数

与大多数现实环境不同，出租车环境没有随机性。这个特性大大简化了学习，因为**下一个状态完全由我们的动作决定**。如果选择了不可行的动作——比如尝试穿越墙壁或接一个不存在的乘客——代理将会停在原地。

从抽象的角度来看，我们可以如下表示转移函数：

`new_state = f(state,action)`

明确的定义会考虑所有网格移动，（不）可行的接送和（不）可行的接客，这并不是特别困难，但需要一些专注。

游戏在成功将乘客送到目的地时结束，表示终止状态。

# 初始化

在数学上定义了问题（尽管不是非常详细）之后，我们将转向实现。

首先，我们安装必要的库，并随后导入它们。显然，我们需要安装`gym`环境。除此之外，我们只需要一些视觉相关的内容和你典型的数据科学库。

假设所有导入都正确，现在是时候进行一下基本检查了。让我们创建并渲染一个 Taxi-v3 环境。

我们应该看到一个介于 0 到 5 之间的动作，一个介于 0 到 499 之间的状态，以及一个阻止不可行动作的动作掩码。最重要的是，我们应该渲染一个静态画面，以确认我们确实已经启动并运行！

![](img/78a49c24b3980432b5f0125f124f1447.png)

初始化输出的截图[图片由作者提供，使用[OpenAI Gym](https://github.com/openai/gym/blob/master/LICENSE.md)]

最后，让我们设置两个函数：一个用于控制台输出和动画，另一个用于存储 GIF。请注意，动画只是 RGB 帧的重复绘图。它也非常耗时，因此不建议在训练期间运行。

# 测试随机代理

在确认环境按预期工作后，现在是时候让一个随机代理“肆意行动”了。正如你所猜测的，这个代理在每一个时刻都会采取随机行动。这本质上就是学习代理开始时的状况，因为它**没有可靠的 Q 值**可以使用。

你会看到代理尝试穿墙、在荒凉的地方接乘客，或者在错误的地点放下乘客。在极少数情况下，代理可能需要几千个动作——纯粹是偶然——才会将乘客放到正确的位置。

正如你可以想象的那样，随机代理让乘客感到无比沮丧：

![](img/b47a4e00828fa23dd51d9fe2e3f6555b.png)

未训练代理的动画。出租车在每个时间步选择随机动作，包括不可行的动作，直到最终偶然将乘客放到正确的站点[图片由作者提供，使用[OpenAI Gym](https://github.com/openai/gym/blob/master/LICENSE.md)]

为什么要观看这么长的动画？好吧，这真的能给你一个未训练的 RL 代理是如何行为的印象，以及获取有意义的奖励信号需要多长时间。

> 注意：通过应用**动作掩码**——一个布尔向量，它实质上将不可行的动作的选择概率分配为 0——我们将自己限制在仅有的合理动作中，通常会大大缩短剧集的长度。

# 训练代理

现在是学习一些有用的东西的时候了。

我们将部署一个[基础 Q 学习算法](https://medium.com/towards-data-science/walking-off-the-cliff-with-off-policy-reinforcement-learning-7fdbcdfe31ff)，采用ϵ-贪婪探索，选择ϵ概率的随机动作，其余时间使用 Q 值来选择动作。

Q 值在观察后使用以下公式更新。请注意，随着 500 个状态和 6 个动作的增加，我们必须填充一个大小为 500*6=3000 的 Q 表，每个状态-动作对需要多个观察来以任何程度的准确性学习其值。

![](img/5d1b28ed0b65945438e798622046ba67.png)

在 Q 学习中，我们在每次观察后更新状态-动作对的值，使用脱离策略的学习。

代码如下。你可以调整超参数；根据我的经验，这个问题对它们的敏感性较低。

训练的收敛情况如下图所示。经过 2000 个回合后，我们似乎学会了一个相当好的、稳定的策略。

![](img/b080cb5baa9ef3c3483892086b7f0af9.png)

起初，代理需要许多步骤才能成功完成一个回合。最终，正向奖励会被识别，代理开始采取越来越高效的动作，最终实现最短路径解决方案 [作者图片]

> 注意：代码忽略了动作掩码，并在出现平局时选择第一个 Q 值。虽然提供了两种问题的替代方案，但它们大大增加了每个回合的计算工作量，并且在此设置下没有明显的性能提升。

# 测试策略

让我们看看我们学到了什么。根据我们所在的状态，我们在 Q 表中查找相应的 Q 值（即每个状态的六个值，对应于动作），并选择 Q 值最高的动作。

实质上，Q 值捕捉了与动作相关的预期累计奖励。注意，我们不再以概率ϵ选择随机动作——这一机制仅用于学习。

如果你进行了足够多的迭代，你应该会看到出租车总是直接驶向乘客，选择最短路径到达目的地，并成功接送乘客。的确，自从随意驾驶以来，我们已经取得了长足的进步。

![](img/3d6fb42df9bb360b32658485e1d00b11.png)

成功学习到的策略动画。出租车将沿着最短路径接送乘客，最大化奖励 [作者图片，使用 [OpenAI Gym](https://github.com/openai/gym/blob/master/LICENSE.md)]

尽管在一个看似简单的环境中需要大量观察来学习这一策略，但如果你意识到算法在完全不了解其操作环境的情况下学会了这些，你可能会更欣赏这个结果！

# 总结

Taxi 环境是一个很好的强化学习入门选择。问题设置简单直观，但可以很容易地扩展到更现实的场景（例如现实的城市网络、管理出租车队、不可预测的旅行时间）。它在计算上易于管理，但也展示了即使在微不足道的问题设置下学习策略所需的计算工作量。

如果你是强化学习的新手，强烈建议你在设计和解决自己的问题之前，先在 OpenAI Gym 环境中进行尝试。

*完整的 Jupyter Notebook 可以在我的 GitHub 上找到：*

[`github.com/woutervanheeswijk/taxi_environment`](https://github.com/woutervanheeswijk/taxi_environment)

![](img/b940d5c28484743ce42d461519ae29cb.png)

由 [Lexi Anderson](https://unsplash.com/@lexianderson?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 进一步阅读

*Q-learning 和 SARSA 简介：*

[](/walking-off-the-cliff-with-off-policy-reinforcement-learning-7fdbcdfe31ff?source=post_page-----c76c22fc5d8f--------------------------------) ## 使用非策略强化学习的悬崖行走

### 对非策略和策略强化学习的深入比较

towardsdatascience.com

*构建马尔可夫决策过程：*

[](/the-five-building-blocks-of-markov-decision-processes-997dc1ab48a7?source=post_page-----c76c22fc5d8f--------------------------------) ## 马尔可夫决策过程的五大构建块

### 通过掌握马尔可夫决策的基础原则来定义和沟通你的强化学习模型……

towardsdatascience.com

*实现 Deep Q-learning 的教程：*

[](/a-minimal-working-example-for-deep-q-learning-in-tensorflow-2-0-e0ca8a944d5e?source=post_page-----c76c22fc5d8f--------------------------------) ## TensorFlow 2.0 中的深度 Q 学习的最小工作示例

### 多臂老虎机示例用于训练 Q 网络。更新过程使用 TensorFlow 仅需几行代码

towardsdatascience.com

# 参考文献

本文中详细介绍的笔记本部分基于以下来源并改编了代码：

[1] OpenAI Gym. [Taxi-v3 环境](https://github.com/openai/gym/blob/master/gym/envs/toy_text/taxi.py)。OpenAI Gym 环境在[MIT 许可证](https://github.com/openai/gym/blob/master/LICENSE.md)下提供。

[2] LearnDataSci. [从头开始在 Python 中实现强化 Q-learning 与 OpenAI Gym](https://www.learndatasci.com/tutorials/reinforcement-q-learning-scratch-python-openai-gym/)。Taxi-v2 实现。

[3] Botforge. [保存 OpenAI Gym 渲染为 GIFS](https://gist.github.com/botforge/64cbb71780e6208172bbf03cd9293553)。公开 GitHub Gist。
