# 应用强化学习 III：深度 Q 网络（DQN）

> 原文：[`towardsdatascience.com/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9`](https://towardsdatascience.com/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9)

## 逐步学习 DQN 算法的行为，以及它相比于以前的强化学习算法的改进

[](https://medium.com/@JavierMtz5?source=post_page-----8f0e38196ba9--------------------------------)![哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----8f0e38196ba9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8f0e38196ba9--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----8f0e38196ba9--------------------------------) [哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----8f0e38196ba9--------------------------------)

·发表于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----8f0e38196ba9--------------------------------) ·7 分钟阅读·2023 年 1 月 2 日

--

![](img/cc0aa599232d7c5acfafd8073654bc95.png)

图片由 [Андрей Сизов](https://unsplash.com/@alpridephoto?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想在没有 Premium Medium 帐户的情况下阅读这篇文章，可以通过这个朋友链接进行 :)
> 
> [`www.learnml.wiki/applied-reinforcement-learning-iii-deep-q-networks-dqn/`](https://www.learnml.wiki/applied-reinforcement-learning-iii-deep-q-networks-dqn/)

深度 Q 网络，首次由 Mnih 等人在 2013 年的论文中报告**[1]**，是迄今为止最知名的强化学习算法之一，因为自发布以来，它在无数 Atari 游戏中展示了超越人类的表现，如*图 1*所示。

![](img/870e39adce5d7679af12de6a4e2dbcd3.png)

**图 1**。对 57 款 Atari 游戏中每个智能体的中位人类标准化评分进行的绘图。**图像摘自** [**deepmind/dqn_zoo GitHub 库**](https://github.com/deepmind/dqn_zoo)

此外，除了看到 DQN 智能体像职业玩家一样玩这些 Atari 游戏的奇妙之处外，DQN 还解决了一个已知几十年的算法问题：Q-学习，该算法已经在本系列的第一篇文章中介绍和解释：

[](/applied-reinforcement-learning-i-q-learning-d6086c1f437?source=post_page-----8f0e38196ba9--------------------------------) ## 应用强化学习 I：Q-学习

### 逐步了解 Q-学习算法，以及任何基于 RL 的系统的主要组件

towardsdatascience.com

Q-Learning 旨在找到一个形式为状态-动作表的函数（Q-Function），该函数计算给定状态-动作对的期望总奖励，以便智能体能够通过执行 Q-Function 输出最高的动作来做出最佳决策。尽管 Watkins 和 Dayan 在 1992 年数学上证明了只要动作空间是离散的，并且每个可能的状态和动作都被反复探索，这个算法就会收敛到最佳的行动值 **[2]**，但在实际环境中很难实现这种收敛。毕竟，在连续状态空间的环境中，不可能反复遍历所有可能的状态和动作，因为状态和动作是无限的，Q-Table 将会太大。

DQN 通过通过神经网络近似 Q-Function 并从之前的训练经验中学习来解决这个问题，以便智能体能够从已经经历过的经验中多次学习，而不必再次经历这些经验，同时避免计算和更新连续状态空间的 Q-Table 的过度计算成本。

# DQN 组件

撇开智能体交互的环境不谈，DQN 算法的三个主要组件是**主神经网络**、**目标神经网络**和**回放缓冲区**。

## 主神经网络

主神经网络尝试预测给定状态下采取每个动作的期望回报。因此，网络的输出将有尽可能多的值以匹配要采取的动作，而网络的输入将是状态。

神经网络通过执行梯度下降来最小化损失函数，但需要一个预测值和一个目标值来计算损失。预测值是主神经网络对当前状态和动作的输出，目标值是通过将获得的奖励加上目标神经网络对下一个状态的输出的最高值乘以折扣率**γ**来计算的。损失的计算可以通过*图 2*在数学上理解。

![](img/3df054a9f6cf96ffa3df0e25e51896de.png)

**图 2**。损失函数。图片由作者提供

至于为什么使用这两个值来计算损失，原因在于目标网络通过获取未来状态和动作的未来奖励预测，对当前动作在当前状态下的有用性有更多的信息。

## 目标神经网络

目标神经网络如上所述，用于获取计算损失和优化的目标值。这个神经网络不同于主神经网络，将每 N 个时间步更新一次，使用主网络的权重。

## 回放缓冲区

Replay Buffer 是一个列表，包含代理所经历的经验。与监督学习训练进行类比，这个缓冲区相当于用于训练的数据集，不同之处在于缓冲区必须逐步填充，随着代理与环境互动并收集信息。

对于 DQN 算法，该缓冲区中的每个经验（数据集中的行）由**当前状态**、**在当前状态下采取的动作**、**采取该动作后的奖励**、是否为**终止状态**以及**采取动作后的下一个状态**表示。

与 Q-Learning 算法中使用的方法不同，这种从经验中学习的方法允许从代理与环境的所有互动中学习，而不依赖于代理刚刚与环境的互动。

# DQN 算法流程

DQN 算法的流程遵循**[1]**中的伪代码，如下所示。

![](img/92ad62cf270f94b3af254269e6f5c4f3.png)

DQN 伪代码。摘自**[1]**

对于每一集，代理执行以下步骤：

## 1\. 从给定状态中选择一个动作

动作的选择遵循ε-贪心策略，这在**[3]**中已解释过。该策略将以概率 1-ε选择具有最佳 Q 值的动作，这个 Q 值是主神经网络的输出，而以概率ε选择一个随机动作（见*图 3*）。ε（epsilon）是算法的一个超参数。

![](img/b86b14ac8e0fb54231805899983d4c93.png)

**图 3**. ε-贪心策略。图片来源于作者

## 2\. 在环境中执行动作

代理在环境中执行动作，获取到新的状态、获得的奖励以及是否达到终止状态。这些值通常由大多数健身环境**[4]**在通过***step()***方法执行动作时返回。

## 3\. 将经验存储在 Replay Buffer 中

如前所述，经验在 Replay Buffer 中保存为***{s, a, r, terminal, s’}***，其中***s***和***a***分别是当前状态和动作，***r***和***s’***是执行动作后的奖励和新状态，***terminal***是一个布尔值，表示是否达到目标状态。

## 4\. 从 Replay Buffer 中抽取一批随机经验

如果 Replay Buffer 中有足够的经验来填充一个批次（如果批次大小为 32，而 Replay Buffer 只有 5 个经验，则批次不能填充，此步骤将被跳过），将随机抽取一批经验作为训练数据。

## 5\. 设置目标值

目标值的定义有两种不同的方式，取决于是否到达了终端状态。如果到达了终端状态，目标值将是收到的奖励；如果新状态不是终端状态，目标值将是如前所述的奖励与下一状态的目标神经网络输出（具有最高 Q-Value）乘以折扣因子**γ**的总和。

![](img/3c90c6642e0468d174acc0bd6cb25a10.png)

目标值定义。从**[1]**中的 DQN 伪代码提取

**折扣因子 γ** 是算法中需要设置的另一个超参数。

## 6\. 执行梯度下降

梯度下降应用于从主神经网络的输出和之前计算的目标值计算的损失，遵循*图 2*中显示的方程式。可以看到，使用的损失函数是均方误差（MSE），因此损失将是主网络输出与目标值之间的差的平方。

## 7\. 执行以下时间步

一旦完成了前面的步骤，这个过程会不断重复，直到达到每集的最大时间步数或智能体到达终端状态。当发生这种情况时，智能体将进入下一集。

# 结论

相较于 Q-Learning，DQN 算法在训练具有连续状态的环境中的智能体时代表了显著的改进，从而允许更大的使用灵活性。此外，使用神经网络（特别是卷积神经网络）而不是 Q-Learning 的 Q-Table，使得可以将图像（例如 Atari 游戏的帧）输入到智能体中，这进一步增加了 DQN 的多样性和可用性。

关于算法的弱点，需要指出的是，使用神经网络可能会导致每帧的执行时间比 Q-Learning 长，尤其是在使用大批量数据时，因为进行大量数据的前向传播和反向传播过程比使用[*Applied Reinforcement Learning I: Q-Learning*](https://medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437)中介绍的修改后的 Bellman 最优方程更新 Q-Table 的 Q-Values 要慢得多。此外，Replay Buffer 的使用可能会成为一个问题，因为算法处理的是存储在内存中的大量数据，这可能会超出某些计算机的 RAM 容量。

# 参考文献

**[1]** MNIH, Volodymyr, 等. 使用深度强化学习玩 Atari 游戏. *arXiv 预印本 arXiv:1312.5602*, 2013.

**[2]** WATKINS, Christopher JCH; DAYAN, Peter. Q 学习. *机器学习*, 1992, 第 8 卷，第 3 期，第 279-292 页。

**[3]** *Applied Reinforcement Learning I: Q-Learning*

[`medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437`](https://medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437)

**[4]** *Gym 文档* [`www.gymlibrary.dev/`](https://www.gymlibrary.dev/)
