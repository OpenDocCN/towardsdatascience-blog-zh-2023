# 应用强化学习 IV：DQN 的实现

> 原文：[`towardsdatascience.com/applied-reinforcement-learning-iv-implementation-of-dqn-7a9cb2c12f97`](https://towardsdatascience.com/applied-reinforcement-learning-iv-implementation-of-dqn-7a9cb2c12f97)

## DQN 算法的实现，以及将其应用于 OpenAI Gym 的 CartPole-v1 环境

[](https://medium.com/@JavierMtz5?source=post_page-----7a9cb2c12f97--------------------------------)![哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----7a9cb2c12f97--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7a9cb2c12f97--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a9cb2c12f97--------------------------------) [哈维尔·马丁内斯·奥赫达](https://medium.com/@JavierMtz5?source=post_page-----7a9cb2c12f97--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7a9cb2c12f97--------------------------------) ·8 分钟阅读·2023 年 1 月 10 日

--

![](img/e9381b94512b701bbdf635b646b1733b.png)

图片由 [DeepMind](https://unsplash.com/@deepmind?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 如果你想阅读这篇文章而不使用高级 Medium 账户，可以通过这个朋友链接来阅读 :)
> 
> [`www.learnml.wiki/applied-reinforcement-learning-iv-implementation-of-dqn/`](https://www.learnml.wiki/applied-reinforcement-learning-iv-implementation-of-dqn/)

在本系列的上一篇文章中，[*应用强化学习 III: 深度 Q 网络 (DQN)*](https://medium.com/@JavierMtz5/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9)，介绍并解释了深度 Q 网络算法，以及与其前身 [Q-Learning](https://medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437) 相比的优缺点。在本文中，将把之前讲解的内容付诸实践，通过将 DQN 应用到实际用例中。如果你对 DQN 算法的基本概念或其原理不熟悉，我建议你在继续阅读本文之前先查看上一篇文章。

[](https://medium.com/@JavierMtz5/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9?source=post_page-----7a9cb2c12f97--------------------------------) [## 应用强化学习 III: 深度 Q 网络 (DQN)

### 逐步学习 DQN 算法的行为，以及与之前的强化学习方法相比的改进…

medium.com](https://medium.com/@JavierMtz5/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9?source=post_page-----7a9cb2c12f97--------------------------------)

# 环境 — CartPole-v1

使用的仿真环境将是 [**gym CartPole 环境**](https://www.gymlibrary.dev/environments/classic_control/cart_pole/)，它由一个沿水平导轨移动的推车和一个竖直安装在其上的杆子组成，如*图 1*所示。目标是通过推车在水平位移中产生的惯性，使杆子尽可能保持竖直。当杆子保持竖直超过 500 个时间步骤时，认为一集成功；如果推车在右侧或左侧偏离水平导轨，或者杆子相对于垂直轴倾斜超过 12 度（约 0.2095 弧度），则认为一集失败。

![](img/fdeab548cbd53c60384ae10bdf7d5e05.png)

**图 1**. CartPole 环境的图像。图片摘自 [gym 环境](https://www.gymlibrary.dev/environments/classic_control/cart_pole/)的渲染

环境具有**离散动作空间**，因为 DQN 算法不适用于具有连续动作空间的环境，而**状态空间是连续的**，因为 DQN 相对于 Q-Learning 的主要优势是能够处理连续或高维状态，这一点还需要证明。

## 动作

![](img/563e1e434fd290e3b681f283021b5d99.png)

CartPole 环境的动作。作者提供的图像

## 状态

状态表示为一个包含 4 个元素的数组，每个元素的意义及其允许的值范围如下：

![](img/d0952fee0aa07fbbe3e693b151cafb36.png)

CartPole 环境的状态。作者提供的图像

应注意，尽管允许的值范围如表中所示，但如果发生以下情况，则一集将被终止：

+   Cart 在 x 轴上的位置离开了 (-2.4, 2.4) 范围

+   杆子角度离开了 (-0.2095, 0.2095) 范围

## 奖励

代理在每个时间步骤中获得 +1 的奖励，旨在尽可能长时间保持杆子竖立。

# DQN 实现

从 Mnih 等人**[1]** 提出的论文中提取的伪代码将作为参考以支持算法的实现。

![](img/17a2556d0c81cad2169e3ea55a8b7795.png)

DQN 伪代码。摘自**[1]**

## 1\. 初始化 Replay Buffer

Replay Buffer 被实现为双端队列（deque），并创建了两个方法与之交互：***save_experience()*** 和 ***sample_experience_batch()***。***save_experience()*** 允许将经验添加到重放缓冲区，而 ***sample_experience_batch()*** 随机挑选一批经验，这些经验将用于训练代理。经验批次作为一个***(states, actions, rewards, next_states, terminals)*** 的元组返回，其中元组中的每个元素对应一个 {batch_size} 项的数组。

**双端队列（Deques）**，根据[Python 的文档](https://docs.python.org/3/library/collections.html#collections.deque)，支持线程安全、内存高效的从队列任一侧追加和弹出元素，性能在任一方向上大致为 O(1)。而 **Python 列表** 在从列表的左侧插入或弹出元素时，复杂度为 O(n)，这使得双端队列在需要操作左侧元素时是一个更高效的选择。

## 2\. 初始化主神经网络和目标神经网络

主神经网络和目标神经网络都通过***create_nn()*** 方法初始化，该方法创建一个包含 64、64 和 2（CartPole 环境的动作大小）层的 keras 模型，每层神经元数量分别为 64、64 和 2，输入为环境的状态大小。使用的损失函数是均方误差（MSE），如算法伪代码中所述（如*图 2*所示），优化器为 Adam。

如果使用 DQN 算法来教代理如何玩 Atari 游戏，如[上一篇文章](https://medium.com/towards-data-science/applied-reinforcement-learning-i-q-learning-d6086c1f437)所提到的，神经网络应为卷积神经网络，因为这种训练类型的状态是视频游戏的帧。

![](img/5b0c9a869249e0327a6cab00632d52fc.png)

**图 2**。摘自 DQN 伪代码中的 **[1]**

## 3\. 在每个时间步长中执行循环，每个回合内

以下步骤会在每个时间步长上重复，并形成 DQN 代理训练的主要循环。

## 3.1 按照 Epsilon-Greedy 策略选择动作

选择具有最佳 Q-值的动作，即主神经网络输出中值最高的动作，概率为 1-ε，其余情况下选择一个随机动作。

完整的算法实现可以在[我的 GitHub 仓库](https://github.com/JavierMtz5/ArtificialIntelligence)中找到，它在每个回合中更新 epsilon 值，使其逐渐变小。这使得探索阶段主要发生在训练的开始阶段，而利用阶段则发生在剩余的回合中。训练过程中 epsilon 值的演变见*图 3*。

![](img/2be1d80158a5c96427cab34620e87f8c.png)

**图 3**。训练中的 epsilon 衰减。图片作者提供

## 3.2 在环境中执行动作并将经验存储到回放缓冲区

从上一步提取的动作通过***env.step()*** 方法应用到 gym 环境中，该方法接收要应用的动作作为参数。此方法返回一个元组 **(next_state, reward, terminated, truncated, info)**，其中的下一个状态、奖励和终止字段与当前状态和选择的动作一起用于使用之前定义的***save_experience()***方法将经验保存到回放缓冲区。

## 3.3 训练代理

最终，智能体通过沿回合存储的经验进行训练。如[上一篇文章](https://medium.com/towards-data-science/applied-reinforcement-learning-iii-deep-q-networks-dqn-8f0e38196ba9)所解释，主神经网络的输出被视为预测值，目标值则根据奖励和目标网络在下一个状态中具有最高 Q 值的动作的输出计算。然后，计算预测值与目标值之间的平方差作为损失，并在主神经网络上执行梯度下降。

## 4. 主循环

在定义了算法在前述步骤中的行为以及其代码实现之后，所有组件可以组合在一起，构建 DQN 算法，如下代码所示。

代码中有一些行为值得提及：

+   ***update_target_network()*** 方法，本文中未提及，将主神经网络的权重复制到目标神经网络。这个过程每{update_rate}时间步重复一次。

+   只有当 Replay Buffer 中有足够的经验以填满一个批次时，才会进行主神经网络的训练。

+   只要未达到最低值，epsilon 值在每个回合中会减少，通过将前一个值乘以小于 1 的减少因子。

+   当在最后 10 个回合中的每一回合都达到了 499 以上的累积奖励时，训练停止，因为此时智能体被认为已经成功学会了执行任务。

# 测试 DQN 智能体

训练好的 DQN 智能体的表现通过绘制训练期间获得的奖励图来评估，并通过运行一个测试回合来进行。

奖励图用于查看智能体是否能够使奖励收敛到最大值，或者相反，如果智能体不能使奖励在每个训练回合中增加，这将意味着训练失败。至于测试回合的执行，这是评估智能体的一种现实方式，因为它以实际的方式展示了智能体是否真的学会了执行其训练的任务。

## 奖励图

图表清晰地显示了尽管在一些回合中智能体在若干时间步内失败，但奖励仍然收敛到最大值。这可能是因为在这些回合中，智能体初始化在一个不熟悉的位置，因为在训练过程中这种初始化并不多见。同样，奖励趋向最大值的趋势是智能体训练正确的一个指标。

![](img/6a0b6ca05c2d705221fdebdfa5787ab3.png)

奖励图。作者提供的图片

## 执行测试回合

智能体成功地以最高分 500 执行了测试回合，这无疑证明了他已经完美地学会了执行任务，正如奖励图所示。

![](img/23faa34cb2bde64cf1068398928a1343.png)

代码和测试回合的执行。图像由作者提供。

# 结论

DQN 算法已经成功地完美地学习了任务，因此它在具有连续状态空间的环境中的应用可以被认为是有效的。同样，算法通过使用两个神经网络，在合理的时间内解决了处理连续和复杂状态空间的问题。由于只有两个网络中的一个被训练，计算时间大大减少，但值得注意的是，即使是简单任务，相比于 Q-Learning 算法，算法的执行时间仍然较长。

关于算法的执行时间，本文仅展示了在简单且易于学习的环境中使用密集神经网络实现 DQN，但需要考虑的是，DQN 在如 Atari 游戏这样的环境中的应用，这些环境通过游戏图像供给卷积神经网络，需要更长的时间来达到最佳 Q 值的收敛，以及进行前向和反向传播过程，因为环境和神经网络的复杂性更高。

最后，必须考虑使用 Replay Buffer 的限制，特别是内存成本。虽然可以指定队列（deque）的最大长度，从而避免过度的内存消耗，但仍然会消耗大量的 RAM。然而，这个问题可以通过将大部分 Replay Buffer 加载到磁盘上，同时将小部分保留在内存中，或者使用更高效的缓冲区来解决，而不是使用双端队列。

# 代码

完整实现的 DQN 算法，无论是 Jupyter Notebook 还是 Python 脚本，都可以在我的[人工智能 GitHub 仓库](https://github.com/JavierMtz5/ArtificialIntelligence)中找到。

[## GitHub - JavierMtz5/ArtificialIntelligence](https://github.com/JavierMtz5/ArtificialIntelligence?source=post_page-----7a9cb2c12f97--------------------------------)

### 目前无法执行该操作。您在另一个标签或窗口中登录。您在另一个标签中注销了…

[github.com](https://github.com/JavierMtz5/ArtificialIntelligence?source=post_page-----7a9cb2c12f97--------------------------------)

# 参考文献

**[1]** MNIH, Volodymyr 等人。使用深度强化学习玩 Atari 游戏。*arXiv 预印本 arXiv:1312.5602*，2013。
