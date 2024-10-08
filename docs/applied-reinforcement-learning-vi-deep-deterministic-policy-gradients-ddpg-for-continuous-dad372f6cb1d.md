# 应用强化学习 VI：用于连续控制的深度确定性策略梯度（DDPG）

> 原文：[`towardsdatascience.com/applied-reinforcement-learning-vi-deep-deterministic-policy-gradients-ddpg-for-continuous-dad372f6cb1d`](https://towardsdatascience.com/applied-reinforcement-learning-vi-deep-deterministic-policy-gradients-ddpg-for-continuous-dad372f6cb1d)

## DDPG 算法的介绍和理论解释，这在连续控制领域有许多应用。

[](https://medium.com/@JavierMtz5?source=post_page-----dad372f6cb1d--------------------------------)![Javier Martínez Ojeda](https://medium.com/@JavierMtz5?source=post_page-----dad372f6cb1d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dad372f6cb1d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----dad372f6cb1d--------------------------------) [Javier Martínez Ojeda](https://medium.com/@JavierMtz5?source=post_page-----dad372f6cb1d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dad372f6cb1d--------------------------------) ·阅读时间 8 分钟·2023 年 3 月 7 日

--

![](img/b37d51029a59d2af9831889b91939bda.png)

图片来源：[Eyosias G](https://unsplash.com/pt-br/@yozz_t?utm_source=medium&utm_medium=referral) 在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想在没有 Premium Medium 账户的情况下阅读这篇文章，你可以通过这个朋友链接 :)
> 
> [`www.learnml.wiki/applied-reinforcement-learning-vi-deep-deterministic-policy-gradients-ddpg-for-continuous-control/`](https://www.learnml.wiki/applied-reinforcement-learning-vi-deep-deterministic-policy-gradients-ddpg-for-continuous-control/)

**DDPG 算法**，由 Lillicarp 等人在 ICLR 2016 首次提出 **[1]**，在深度强化学习算法用于连续控制方面取得了重大突破，因为它比 DQN **[2]**（仅适用于离散动作）有了改进，且其结果优异且易于实现（见**[1]**）。

关于**NAF 算法** **[3]**，该算法在[上一篇文章](https://medium.com/towards-data-science/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095)中介绍，DDPG 适用于连续动作空间和连续状态空间，因此它也是适用于机器人技术或自动驾驶等领域的连续控制任务的有效选择。

[](/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095?source=post_page-----dad372f6cb1d--------------------------------) ## 应用强化学习 VI：用于连续控制的归一化优势函数（NAF）

### NAF 算法的介绍和解释，该算法广泛用于连续控制任务

[towardsdatascience.com

# DDPG 算法的逻辑

DDPG 算法是一种 Actor-Critic 算法，顾名思义，它由两个神经网络组成：**Actor**和**Critic**。Actor 负责选择最佳动作，而 Critic 则必须评估所选动作的好坏，并告知 Actor 如何改进。

Actor 通过应用策略梯度进行训练，而 Critic 通过计算 Q 函数进行训练。因此，DDPG 算法试图同时学习最优 Q 函数的近似器（Critic）和最优策略的近似器（Actor）。

![](img/21274c16474ed2ceea16e312ef11fa9e.png)

Actor-Critic 模式。图像摘自**[4]**

最优 Q 函数**Q*(s, a)**给出了在状态**s**下，采取动作**a**并随后按照最优策略行动的期望回报。另一方面，最优策略**𝜇*(s)**返回在状态**s**下最大化期望回报的动作。根据这两个定义，给定状态下的最优动作（即最优策略在给定状态下的回报）可以通过获取给定状态下**Q*(s, a)**的 argmax 来获得，如下所示。

![](img/fc7660bca9ebc13b1e78fe71ea874636.png)

Q 函数与策略的关系。作者提供的图像

问题在于，对于连续动作空间，获取最大化 Q 的动作**a**并不容易，因为计算每一个可能的**a**的 Q 值以检查哪个结果最高（这是离散动作空间的解决方案）几乎是不可能的，因为可能的值是无限的。

作为解决方案，并假设动作空间是连续的且 Q 函数对动作是可微的，DDPG 算法将***maxQ(s, a)***近似为***Q(s, 𝜇(s))***，其中***𝜇(s)***（一个确定性策略）可以通过执行梯度上升来优化。

简而言之，DDPG 学习最优 Q 函数的近似器，以获得最大化该函数的动作。由于动作空间是连续的，Q 函数的结果不能针对每一个可能的动作值来获得，DDPG 还学习最优策略的近似器，以直接获得最大化 Q 函数的动作。

接下来的部分解释了算法如何学习最优 Q 函数的近似器和最优策略的近似器。

# Q 函数的学习

## 均方贝尔曼误差函数

Q-函数的学习是基于贝尔曼方程的，该方程在[本系列的第一篇文章](https://medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437)中已介绍。由于在 DDPG 算法中 Q-函数不是直接计算的，而是使用了一个神经网络 ***Qϕ(s, a)*** 作为 Q-函数的近似器，因此使用了一个称为 **均方贝尔曼误差 (MSBE)** 的损失函数。该函数如 *图 1* 所示，指示近似器 *Qϕ(s, a)* 如何满足贝尔曼方程。

![](img/4485e93d6884d97d4ec06e4543ff9ed7.png)

**图 1.** 均方贝尔曼误差 (MSBE)。图像摘自 **[5]**

DDPG 的目标是最小化这个误差函数，这将使 Q-函数的近似器满足贝尔曼方程，这意味着近似器是最优的。

## 回放缓冲区

为了最小化 MSBE 函数（即训练神经网络以近似 Q*(s, a)），所需的数据是从 **回放缓冲区** 中提取的，该缓冲区存储了训练过程中经历的经验。这个回放缓冲区在 *图 1* 中表示为 ***D***，从中获取计算损失所需的数据：状态 **s**，动作 **a**，奖励 **r**，下一个状态 **s’** 和完成 **d**。如果你对回放缓冲区不熟悉，它在关于 [DQN 算法](https://medium.com/towards-data-science/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9) 和 [NAF 算法](https://medium.com/towards-data-science/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095) 的文章中已有解释，并在关于 [DQN 实现](https://medium.com/towards-data-science/applied-reinforcement-learning-iv-implementation-of-dqn-7a9cb2c12f97) 的文章中进行了实现和应用。

## 目标神经网络

MSBE 函数的最小化包括使 Q-函数的近似器，*Qϕ(s, a)*，尽可能接近函数的另一个项，即 **目标**，其原始形式如下：

![](img/d58029862efbecaec31321f03282b10c.png)

**图 2.** 目标。摘自图 1 **[5]**

如图所示，目标依赖于待优化的相同参数，**ϕ**，这使得最小化变得不稳定。因此，作为解决方案，使用了另一个神经网络，其中包含主神经网络的参数，但有一定的延迟。这个第二个神经网络被称为 **目标网络，*Qϕtarg(s, a)*（见*图 3*）**，其参数表示为 ***ϕtarg***。

然而，在*图 2*中可以看到，当将 *Qϕ(s, a)* 替换为 *Qϕtarg(s, a)* 时，必须获得最大化该目标网络输出的动作。正如上文所述，这对于连续动作空间环境来说是复杂的。这通过利用**目标策略网络，*𝜇ϕtarg(s)*（见*图 3*）**来解决，该网络逼近了最大化目标网络输出的动作。换句话说，创建了一个目标策略网络 *𝜇ϕtarg(s)* 来解决 *Qϕtarg(s, a)* 的连续动作问题，就像之前对 *𝜇ϕ(s)* 和 *Qϕ(s, a)* 所做的那样。

## 最小化修改后的 MSBE 函数

通过这一切，DDPG 算法通过对*图 3*中的修改后的 MSBE 函数应用梯度下降来学习最优 Q-函数。

![](img/fc9ef5c014b0479ce017276e9bd2589c.png)

**图 3\.** 修改后的均方贝尔曼误差。摘自**[5]**并由作者编辑

# 策略学习

由于动作空间是连续的，并且 Q-函数对动作是可微分的，DDPG 通过对以下函数应用梯度上升来学习最大化 *Qϕ(s, a)* 的确定性策略***𝜇ϕ(s)***，该函数是关于确定性策略参数的：

![](img/a363a03aeb9fe79da15bf022dff83a3a.png)

**图 4.** 确定性策略学习的优化函数。摘自**[5]**

# DDPG 算法流程

DDPG 算法的流程将按照以下伪代码展示，摘自**[1]**。DDPG 算法遵循与其他函数逼近 Q-Learning 算法相同的步骤，例如 DQN 或 NAF 算法。

![](img/75b55a12daffd419a15512867063bdfc.png)

DDPG 算法伪代码。摘自**[1]**

## 1\. 初始化 Critic、Critic 目标、Actor 和 Actor 目标网络

初始化在训练过程中使用的 Actor 和 Critic 神经网络。

+   Critic 网络，*Qϕ(s, a)*，作为 Q-函数的逼近器。

+   Actor 网络，*𝜇ϕ(s)*，作为确定性策略的逼近器，用于获取最大化 Critic 网络输出的动作。

一旦初始化，目标网络将与其相应的主网络具有相同的架构，并且主网络的权重会被复制到目标网络中。

+   Critic 目标网络，*Qϕtarg(s, a)*，作为一个延迟的 Critic 网络，以便目标不依赖于需要优化的相同参数，正如之前所解释的。

+   Actor 目标网络，*𝜇ϕtarg(s)*，作为一个延迟的 Actor 网络，用于获取最大化 Critic 目标网络输出的动作。

## 2\. 初始化 Replay Buffer

用于训练的 Replay Buffer 被初始化为空。

*在一个时间步中，智能体执行以下步骤：*

## 3\. 选择动作并施加噪声

从 Actor 神经网络的输出中获得当前状态的最佳动作，该网络逼近确定性策略 *𝜇ϕ(s).* 从**Ornstein Uhlenbeck 噪声**过程**[6]**或**无关的、均值为零的高斯分布** **[7]** 中提取的噪声然后被应用于选定的动作。

## 4\. 执行动作并将观察结果存储在 Replay Buffer 中

在环境中执行有噪声的动作。之后，环境返回一个**奖励**，指示采取的行动的效果，执行该动作后达到的**新状态**，以及一个布尔值，指示是否已达到**终止状态**。

这些信息连同**当前状态**和**采取的行动**一起存储在 Replay Buffer 中，稍后用于优化 Critic 和 Actor 神经网络。

## 5\. 采样经验批次，并训练 Actor 和 Critic 网络

只有当 Replay Buffer 中有足够的经验填满一个批次时，这一步骤才会执行。一旦满足此要求，就会从 Replay Buffer 中提取一个经验批次用于训练。

使用这一批经验：

+   计算目标值，并获得 Critic 网络（Q-Function 的逼近器）的输出，然后在 MSBE 误差函数上应用梯度下降，如*图 3*所示。这一步骤训练/优化了 Q-Function 的逼近器，*Qϕ(s, a)。*

+   对*图 4*所示的函数执行梯度上升，从而优化/训练确定性策略的逼近器，*𝜇ϕ(s)*。

## 6\. 软更新目标网络

每次更新 Actor 和 Critic 网络时，Actor Target 和 Critic Target 网络都通过**Polyak 平均**进行更新，如下图所示。

![](img/3a004bfe6f1fb9758bf9750fa17bf710.png)

**图 5.** Polyak 平均。摘自**[1]**

**Tau *τ***，设置 Polyak 平均中每个元素权重的参数，是算法的一个超参数，通常取接近 1 的值。

# 结论

Lillicrap 等人提出的 DDPG 算法在 Gym 中大多数连续环境下取得了非常好的结果，如论文**[1]**所示，展示了它在连续环境中学习不同任务的能力，无论这些任务的复杂性如何。

因此，该算法至今仍用于使智能体在连续环境中学习复杂任务的最优策略，如操作机器人控制任务或自主车辆的避障任务。

# 参考文献

**[1]** LILLICRAP, Timothy P., 等人。使用深度强化学习进行连续控制。*arXiv 预印本 arXiv:1509.02971*，2015。

**[2]** MNIH, Volodymyr, 等人。使用深度强化学习玩 Atari 游戏。*arXiv 预印本 arXiv:1312.5602*，2013。

**[3]** GU, Shixiang 等人。《基于模型的加速连续深度 Q 学习》。发表于*国际机器学习大会*。PMLR，2016 年。第 2829–2838 页。

**[4]** SUTTON, Richard S.; BARTO, Andrew G. *《强化学习：导论》*。MIT 出版社，2018 年。

**[5]** *OpenAI Spinning Up — 深度确定性策略梯度*[`spinningup.openai.com/en/latest/algorithms/ddpg.html`](https://spinningup.openai.com/en/latest/algorithms/ddpg.html)

**[6]** 乌伦贝克-奥恩斯坦过程[`en.wikipedia.org/wiki/Ornstein%E2%80%93Uhlenbeck_process`](https://en.wikipedia.org/wiki/Ornstein%E2%80%93Uhlenbeck_process)

**[7]** 正态 / 高斯分布

[`en.wikipedia.org/wiki/Normal_distribution`](https://en.wikipedia.org/wiki/Normal_distribution)
