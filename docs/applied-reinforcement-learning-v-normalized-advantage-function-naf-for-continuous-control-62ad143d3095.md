# 应用强化学习 V：用于连续控制的归一化优势函数（NAF）

> 原文：[`towardsdatascience.com/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095`](https://towardsdatascience.com/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control-62ad143d3095)

## NAF 算法的介绍和解释，这是一种广泛用于连续控制任务的算法

[](https://medium.com/@JavierMtz5?source=post_page-----62ad143d3095--------------------------------)![哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----62ad143d3095--------------------------------)[](https://towardsdatascience.com/?source=post_page-----62ad143d3095--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----62ad143d3095--------------------------------) [哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----62ad143d3095--------------------------------)

·发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----62ad143d3095--------------------------------) ·9 分钟阅读·2023 年 1 月 19 日

--

![](img/980806b6e676bcf2f2e28c3dca2000f0.png)

图片由 [Sufyan](https://unsplash.com/@blenderdesigner_1688?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想在没有 Premium Medium 账户的情况下阅读这篇文章，你可以通过这个朋友链接来查看 :)
> 
> [`www.learnml.wiki/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control/`](https://www.learnml.wiki/applied-reinforcement-learning-v-normalized-advantage-function-naf-for-continuous-control/)

本系列之前的文章介绍并解释了两种自其诞生以来广泛使用的强化学习算法：[**Q-Learning**](https://medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437) 和 [**DQN**](https://medium.com/towards-data-science/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9)。

**Q-Learning** 将 Q 值存储在一个动作-状态矩阵中，因此，要在状态 *s* 中获得最大的 Q 值动作 *a*，必须找到 Q 值矩阵中第 *s* 行的最大元素，这使得其在连续状态或动作空间中的应用变得不可能，因为 Q 值矩阵将是无限的。

另一方面，**DQN** 通过利用神经网络来获取与状态 *s* 相关的 Q-值，从而部分解决了这个问题，使神经网络的输出为每个可能的动作的 Q-值（相当于 Q-Learning 的动作-状态矩阵中的一行）。该算法允许在具有连续状态空间的环境中进行训练，但仍然无法在具有连续动作空间的环境中进行训练，因为神经网络的输出（具有与可能动作一样多的元素）将会是无限长度的。

**NAF 算法**由 Shixiang Gu 等人提出于**[1]**，与 Q-Learning 或 DQN 不同，它允许在连续状态和动作空间环境中进行训练，增加了许多应用的灵活性。像 NAF 这样的连续环境中的强化学习算法通常在控制领域中使用，特别是在机器人技术中，因为它们能够在更贴近现实的环境中进行训练。

![](img/95034a16b9224f41805bfa46c145eb24.png)

强化学习算法的类型及其可能的状态/动作空间。作者提供的图片

# 引言概念

## 优势函数

**状态-价值函数 V** 和 **动作-价值函数**（**Q-函数**）**Q**，都在 [本系列的第一篇文章](https://medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437) 中进行了讲解，分别确定了在遵循某个策略时处于某状态的好处以及在遵循某个策略时从给定状态采取某个动作的好处。这两个函数，以及 **V** 相对于 **Q** 的定义，可以在下方查看。

![](img/2bc6f744389fa96fe75abca4ca0eb4df.png)

Q-函数、价值函数和 Q-V 关系。作者提供的图片

由于 **Q** 返回在某状态下采取某个动作的好处，而 **V** 返回处于某状态的好处，因此两者之间的差异提供了关于在某状态下相对于其他动作采取特定动作的优势信息，或者代理通过采取该动作相对于其他动作所获得的额外奖励。这个差异称为 **优势函数**，其方程式如下所示。

![](img/d76d848a25d01d7ee6e0d81bdf4c357f.png)

优势函数。作者提供的图片

## Ornstein-Uhlenbech 噪声过程（OU）

正如之前的文章所见，在离散环境下的强化学习算法如 Q-Learning 或 DQN 中，探索是通过随机选择一个动作并忽略最优策略来进行的，比如 epsilon 贪心策略。然而，在连续环境中，动作是根据最优策略选择的，并且在这个动作上添加噪声。

向选择的动作中添加噪声的问题在于，如果噪声与之前的噪声无关且具有零均值的分布，则动作会相互抵消，使得代理无法保持对任何点的连续运动，而是会陷入困境，从而无法探索。**Ornstein-Uhlenbech 噪声过程**获得与之前噪声值相关的噪声值，使得代理能够向某个方向进行连续运动，因此成功地进行探索。

关于 Ornstein-Uhlenbech 噪声过程的更深入信息可以在**[2]**中找到。

# NAF 算法背后的逻辑

NAF 算法利用神经网络分别获得**状态值函数 V**和**优势函数 A**的值。由于之前的解释，神经网络获得这些输出的原因是，**动作值函数 Q**的结果可以作为**V**和**A**的总和。

像大多数强化学习算法一样，NAF 旨在优化 Q-函数，但在其应用案例中，特别复杂，因为它使用神经网络作为 Q-函数估计器。因此，NAF 算法利用了一个二次函数来表示优势函数，其解是封闭且已知的，从而使得关于动作的优化更加容易。

![](img/09b25af10b4e1ad8de2d42bad265dcf1.png)

**图 1**。NAF 算法的 Q-函数和优势函数。图像摘自**[1]**。

更具体地说，Q-函数相对于动作总是二次的，因此动作的*argmax Q(x, u)*总是**𝜇**(**x**|𝜃) **[3]**，如*图 2*所示。由于此原因，解决了由于在连续动作空间中工作而无法获得神经网络输出的 argmax 的问题，这在 DQN 中曾经存在。

通过查看构成 Q-函数的不同组件，可以看出神经网络将有三个不同的输出：一个用于估计价值函数，另一个用于获得最大化 Q-函数的动作（*argmax Q(s, a)* 或 **𝜇**(**x**|𝜃)），还有一个用于计算矩阵 P（见*图 1*）：

+   神经网络的第一个输出是对状态值函数的估计。这个估计值随后用于获得 Q-函数的估计，即状态值函数和优势函数的总和。这个输出在*图 1*中表示为**V(x|𝜃)**。

+   神经网络的第二个输出是**𝜇(x|𝜃)**，即在给定状态下最大化 Q-函数的动作，或*argmax Q(s, a)*，因此作为代理应遵循的策略。

+   第三个输出用于随后形成状态依赖的正定方阵**P**(**x**|𝜃)。这种神经网络的线性输出用作下三角矩阵**L**(**x**|𝜃)的输入，其中对角项经过指数化处理，从而构建出上述的矩阵**P**(**x**|𝜃)，其计算公式如下。

![](img/2cffbad3d13d3b9ae8f6ef671c1de32b.png)

构造 P 矩阵的公式。摘自**[1]**

![](img/605c89d50581caf1849856a81b45878e.png)

**图 2**。作者提供的图像

神经网络的第二和第三个输出用于构造优势函数的估计，如*图 1*所示，然后将其加到第一个输出（状态值函数估计 V）中，以获得 Q 函数的估计。

关于 NAF 算法的其余部分，它包含与文章[*应用强化学习 III：深度 Q 网络（DQN）*](https://medium.com/towards-data-science/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9)中解释的 DQN 算法相同的组件和步骤。这些共同的组件有**重放缓冲区**、**主神经网络**和**目标神经网络**。与 DQN 类似，重放缓冲区用于存储经验以训练主神经网络，目标神经网络用于计算目标值并与主网络的预测进行比较，然后执行反向传播过程。

# NAF 算法流程

NAF 算法的流程将遵循下面的伪代码，摘自**[1]**。如前所述，NAF 算法遵循与 DQN 算法相同的步骤，只是 NAF 以不同的方式训练其主神经网络。

![](img/e9e3dffcf427a42797efda0eedeea072.png)

NAF 算法伪代码。摘自**[1]**

*在每个时间步中，智能体执行以下步骤：*

## 1\. 从给定状态中选择一个动作

选择的动作是最大化 Q 函数估计值的动作，由**𝜇(x|𝜃)**表示，如*图 2*所示。

对于所选择的动作，添加从 Ornstein-Uhlenbech 噪声过程（之前介绍过）中提取的噪声，以增强智能体的探索。

## 2\. 对环境执行动作

在环境中由智能体执行带噪声的动作。执行此动作后，智能体接收关于动作效果的信息（通过**奖励**），以及关于在环境中达到的新状态的信息（即**下一个状态**）。

## 3\. 将经验存储在重放缓冲区中

重放缓冲区将经验存储为***{s, a, r, s’}***，其中***s***和***a***是当前状态和动作，***r***和***s’***是奖励和执行当前状态动作后的新状态。

*从步骤 4 到 7 的过程会根据算法超参数* ***I*** *在每个时间步上重复进行，如上述伪代码所示。*

## 4\. 从 Replay Buffer 中随机抽取一批经验

如[《DQN 文章》](https://medium.com/towards-data-science/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9)所述，仅当 Replay Buffer 中有足够的数据填充一批时，才会提取一批经验。一旦满足这一条件，*{batch_size}* 元素会从 Replay Buffer 中随机选取，从而可以利用以前的经验进行学习，而不需要最近经历这些经验。

## 5\. 设置目标值

目标值定义为奖励与下一状态的目标神经网络的值函数估计值之和，乘以折扣因子**γ**，这是算法的一个超参数。目标值的公式如下所示，并且也在上述伪代码中提供。

![](img/54f2066a2722a652de6d7abc8fbebeab.png)

目标值计算。摘自**[1]**中的伪代码

## 6\. 执行梯度下降

对损失进行梯度下降，这些损失是通过主神经网络（预测值）和先前计算的目标值的 Q 函数估计得出的，按照下面的方程计算。如所示，使用的损失函数是均方误差（MSE），因此损失将是 Q 函数估计值和目标值之间的平方差。

![](img/d8c243ae133cd32c2fec507318ec081b.png)

损失函数。摘自**[1]**

应记住，Q 函数的估计是通过值函数 V(x**|𝜃**)的估计加上优势函数 A(x, u**|𝜃**)的估计得出的，其公式显示在*图 1*中。

## 7\. 软更新目标神经网络

目标神经网络的权重以一种软方式更新为主神经网络的权重。这种软更新是主网络权重和目标网络旧权重的加权和，如下方程所示。

![](img/8ff611f014ac51ff20d4c8f50968f97e.png)

目标网络的软更新。摘自**[1]**

在加权和中，每个神经网络权重的重要性由超参数**τ**决定。如果**τ**为零，则目标网络不会更新其权重，因为它将加载自己的旧权重。如果**τ**设为 1，则目标神经网络将通过加载主网络的权重来更新，忽略目标网络的旧权重。

## 8\. 时间步结束 — 执行下一个时间步

完成上述步骤后，将不断重复这一过程，直到达到每个回合的最大时间步数或代理到达终止状态。当发生这种情况时，将进入下一个回合。

# 结论

NAF 算法在连续环境中的实现中取得了非常好的结果，因此它圆满地完成了其目标。NAF 与 **DDPG 算法** **[4]** 的比较结果如下，其中可以看到它在前期工作中有了显著的改进。此外，NAF 算法的美妙之处在于，它通过平方函数及其易于优化的特性解决了 DQN 在连续环境中的局限性，是一种聪明且富有创意的解决方案。

![](img/9c04138c0416455bca91bfdf1eb53a50.png)

DDPG 和 NAF 在不同任务中的比较。摘自**[1]**

另一方面，尽管 NAF 被证明是一个高效且有用的算法，但它的逻辑和实现并不简单，尤其是与之前用于离散环境的算法相比，这使得它难以使用。

# 参考文献

**[1]** GU, Shixiang 等人。基于模型加速的连续深度 q 学习。在 *国际机器学习大会* 上。PMLR，2016 年。第 2829–2838 页

**[2]** *奥恩斯坦-乌伦贝克过程*[`en.wikipedia.org/wiki/Ornstein%E2%80%93Uhlenbeck_process`](https://en.wikipedia.org/wiki/Ornstein%E2%80%93Uhlenbeck_process)

**[3]** *二次型。伯克利数学*[`math.berkeley.edu/~limath/Su14Math54/0722.pdf`](https://math.berkeley.edu/~limath/Su14Math54/0722.pdf)

**[4]** LILLICRAP, Timothy P. 等人。通过深度强化学习进行连续控制。*arXiv 预印本 arXiv:1509.02971*，2015 年
