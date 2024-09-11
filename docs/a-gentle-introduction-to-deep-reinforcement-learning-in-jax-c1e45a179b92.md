# 《深入浅出 JAX 中的深度强化学习》

> 原文：[https://towardsdatascience.com/a-gentle-introduction-to-deep-reinforcement-learning-in-jax-c1e45a179b92?source=collection_archive---------4-----------------------#2023-11-21](https://towardsdatascience.com/a-gentle-introduction-to-deep-reinforcement-learning-in-jax-c1e45a179b92?source=collection_archive---------4-----------------------#2023-11-21)

## 在一秒钟内用 DQN 解决 CartPole 环境

[](https://medium.com/@ryanpegoud?source=post_page-----c1e45a179b92--------------------------------)[![Ryan Pégoud](../Images/9314b76c2be56bda8b73b4badf9e3e4d.png)](https://medium.com/@ryanpegoud?source=post_page-----c1e45a179b92--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1e45a179b92--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c1e45a179b92--------------------------------) [Ryan Pégoud](https://medium.com/@ryanpegoud?source=post_page-----c1e45a179b92--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F27fba63b402e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-deep-reinforcement-learning-in-jax-c1e45a179b92&user=Ryan+P%C3%A9goud&userId=27fba63b402e&source=post_page-27fba63b402e----c1e45a179b92---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1e45a179b92--------------------------------) · 10分钟阅读·2023年11月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc1e45a179b92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-deep-reinforcement-learning-in-jax-c1e45a179b92&user=Ryan+P%C3%A9goud&userId=27fba63b402e&source=-----c1e45a179b92---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc1e45a179b92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-gentle-introduction-to-deep-reinforcement-learning-in-jax-c1e45a179b92&source=-----c1e45a179b92---------------------bookmark_footer-----------)![](../Images/af8f701ac536881e802dd7507bdc46f2.png)

由[Thomas Despeyroux](https://unsplash.com/@thomasdes?utm_source=medium&utm_medium=referral)拍摄，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

最近在强化学习（RL）方面的进展，例如 Waymo 的自动驾驶出租车或 DeepMind 的超人类棋类代理，结合了**经典 RL**和**深度学习**组件，如**神经网络**和**梯度优化**方法。

在之前介绍的基础和编码原则上构建，我们将探索并学习如何使用JAX实现**深度Q网络**（**DQN**）和**回放缓冲区**来解决OpenAI的**CartPole**环境。所有这些操作都在**不到一秒**的时间内完成！

对于**JAX**、**向量化环境**和**Q-learning**的介绍，请参阅以下内容：

[](/vectorize-and-parallelize-rl-environments-with-jax-q-learning-at-the-speed-of-light-49d07373adf5?source=post_page-----c1e45a179b92--------------------------------) [## 使用JAX向量化和并行化RL环境：Q-learning的光速⚡

### 学习如何在CPU上向量化GridWorld环境，并同时训练30个Q-learning代理，每个代理进行180万步…

towardsdatascience.com](/vectorize-and-parallelize-rl-environments-with-jax-q-learning-at-the-speed-of-light-49d07373adf5?source=post_page-----c1e45a179b92--------------------------------)

我们选择的深度学习框架是DeepMind的**Haiku**库，我最近在Transformer的上下文中介绍过：

[](/implementing-a-transformer-encoder-from-scratch-with-jax-and-haiku-791d31b4f0dd?source=post_page-----c1e45a179b92--------------------------------) [## 使用JAX和Haiku从头开始实现Transformer编码器 🤖

### 理解Transformer的基础构建模块。

towardsdatascience.com](/implementing-a-transformer-encoder-from-scratch-with-jax-and-haiku-791d31b4f0dd?source=post_page-----c1e45a179b92--------------------------------)

本文将涵盖以下几个部分：

+   **为什么**我们需要深度RL？

+   **深度Q网络**的**理论和实践**

+   **回放缓冲区**

+   将**CartPole**环境转换为**JAX**

+   **JAX**编写**高效训练循环**的方式

*如往常一样，本文中提供的所有代码都可在GitHub上找到：*

[](https://github.com/RPegoud/jym?source=post_page-----c1e45a179b92--------------------------------) [## GitHub - RPegoud/jym：JAX实现的RL算法和向量化环境

### JAX实现的RL算法和向量化环境 - GitHub - RPegoud/jym: JAX实现的RL…

github.com](https://github.com/RPegoud/jym?source=post_page-----c1e45a179b92--------------------------------)

# **为什么**我们需要深度RL？

在之前的文章中，我们介绍了[时间差分学习](https://medium.com/towards-data-science/temporal-difference-learning-and-the-importance-of-exploration-an-illustrated-guide-5f9c3371413a)算法，特别是[Q-learning](/vectorize-and-parallelize-rl-environments-with-jax-q-learning-at-the-speed-of-light-49d07373adf5)。

简单来说，Q-learning是一种**离策略**算法（*目标策略与用于决策的策略不同*），用于维护和更新**Q表**，一个明确的**状态**到相应**动作值**的**映射**。

尽管Q学习是离散行动空间和受限观察空间环境的实际解决方案，但在更复杂的环境中很难扩展。事实上，创建Q表需要定义**行动**和**观察空间**。

考虑**自动驾驶**的例子，**观察空间**由来自摄像头和其他感知输入的*无限潜在配置*组成。另一方面，**行动空间**包括*广泛的方向盘位置*以及施加到刹车和油门的不同力度。

尽管理论上我们可以离散化行动空间，但实际应用中可能会导致**不切实际的Q表**，因为可能的状态和行动数量庞大。

![](../Images/69029c0c7e9ad43e30b3e37045cbfb99.png)

[Kirill Tonkikh](https://unsplash.com/@photophotostock?utm_source=medium&utm_medium=referral)的照片来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在大型复杂状态-动作空间中寻找最优行动因此需要**强大的函数逼近算法**，这正是**神经网络**所擅长的。在深度强化学习中，神经网络用作**Q表的替代品**，并为大状态空间引入的*维度灾难*提供了高效的解决方案。此外，我们不需要显式定义观察空间。

# 深度Q网络与重播缓冲区

DQN同时使用两种类型的神经网络，并行进行，首先是用于**Q值预测**和**决策**的“***在线***”网络。另一方面，“***目标***”网络用于通过损失函数评估在线网络的性能，以生成**稳定的Q目标**。

与Q学习类似，DQN代理由两个函数定义：`act`和`update`。

## 行动

`act`函数实现了关于Q值的ε-贪心策略，Q值由在线神经网络估计。换句话说，代理根据给定状态的**最大预测Q值**选择动作，同时以一定概率随机执行动作。

您可能还记得Q学习在每一步之后更新其Q表，但在深度学习中，通常使用梯度下降在**输入批次**上计算更新。

因此，DQN将经验（包含`state, action, reward, next_state, done_flag`的元组）存储在**重播缓冲区**中。为了训练网络，我们将从此缓冲区中随机抽取一批经验，而不仅仅使用最后一次经验（有关更多详细信息，请参见重播缓冲区部分）。

![](../Images/7650171ae97fe0479b46e1490c1fdca0.png)

展示了**DQN行动选择**过程的视觉表示（作者制作）

这里是DQN行动选择部分的JAX实现：

这个代码片段的唯一细微之处在于 `model` 属性不包含任何内部参数，这与 PyTorch 或 TensorFlow 等框架中通常的情况不同。

在这里，模型是一个 **函数**，表示我们架构中的 **前向传递**，但 ***可变的* 权重是外部存储** 并作为 **参数** 传递。这解释了为什么我们可以在传递 `self` 参数时使用 `jit`，作为 **静态*（**模型在其他类属性中是无状态的）*。

## 更新

`update` 函数负责训练网络。它根据 **时间差**（TD） **误差** 计算 **均方误差**（MSE）损失：

![](../Images/449285430b8e3bdfe71e785562543881.png)

DQN中使用的均方误差

在这个损失函数中，***θ*** 表示 **在线网络的参数**，而 ***θ*−** 代表 **目标网络的参数**。目标网络的参数每隔 *N* 步被设置为在线网络的参数，类似于一个 *检查点* （*N 是一个超参数*）。

参数的分离（当前Q值的 *θ* 和目标Q值的 *θ*−）对于稳定训练至关重要。

如果对两个网络使用相同的参数，就类似于瞄准一个移动的目标，因为 **对网络的更新** 会 **立即改变目标值**。通过 **定期更新** ***θ*−**（即在设定步数内冻结这些参数），我们确保了 **稳定的Q目标**，同时在线网络继续学习。

最后，*(1-done)* 项 **调整目标** 用于 **终止状态**。实际上，当一个回合结束时（即 ‘done’ 等于 1），没有下一个状态。因此，下一状态的Q值被设为0。

![](../Images/6cc23c97e8034ed5aa9e046e8ca97139.png)

**DQN参数更新** 过程的可视化表示（作者制作）

实现DQN的更新函数稍微复杂一些，我们分解一下：

+   首先，`_loss_fn` 函数实现了之前描述的用于 **单一经验** 的平方误差。

+   然后，`_batch_loss_fn` 作为 `_loss_fn` 的包装器，并通过 `vmap` 装饰它，将损失函数应用于 **一批经验**。然后我们返回这一批的平均误差。

+   最后，`update` 作为我们损失函数的最终层，计算其 **梯度** 相对于在线网络参数、目标网络参数以及一批经验。然后我们使用 **Optax** *（一个常用于优化的JAX库）* 执行优化步骤并更新在线参数。

注意，与回放缓冲区类似，模型和优化器是 **纯函数**，修改 **外部状态**。以下一行很好地说明了这一原则：

```py
updates, optimizer_state = optimizer.update(grads, optimizer_state)
```

这也解释了为什么我们可以对在线网络和目标网络使用一个模型，因为参数是外部存储和更新的。

```py
# target network predictions
self.model.apply(target_net_params, None, state)
# online network predictions
self.model.apply(online_net_params, None, state)
```

为了提供背景，我们在本文中使用的模型是一个*多层感知机*，定义如下：

```py
N_ACTIONS = 2
NEURONS_PER_LAYER = [64, 64, 64, N_ACTIONS]
online_key, target_key = vmap(random.PRNGKey)(jnp.arange(2) + RANDOM_SEED)

@hk.transform
def model(x):
    # simple multi-layer perceptron
    mlp = hk.nets.MLP(output_sizes=NEURONS_PER_LAYER)
    return mlp(x)

online_net_params = model.init(online_key, jnp.zeros((STATE_SHAPE,)))
target_net_params = model.init(target_key, jnp.zeros((STATE_SHAPE,)))

prediction = model.apply(online_net_params, None, state)
```

## 重放缓冲区

现在，让我们退一步，更详细地看一下重放缓冲区。它们在强化学习中被广泛使用，原因有很多：

+   **泛化：** 通过从重放缓冲区采样，我们打破了连续经验之间的相关性，通过混合它们的顺序。这种方式避免了对特定经验序列的过拟合。

+   **多样性：** 由于采样不限于最近的经验，我们通常会观察到更新的方差较低，并且避免对最新经验的过拟合。

+   **增加样本效率：** 每个经验可以从缓冲区中被多次采样，使模型能够从个体经验中学习更多。

最后，我们可以使用几种采样方案来管理我们的重放缓冲区：

+   **均匀采样：** 经验以均匀的随机方式进行采样。这种采样类型易于实现，并允许模型从经验中独立于它们被收集的时间步长中学习。

+   **优先采样：** 这个类别包括不同的算法，如**优先经验重放**（“PER”，[*Schaul等，2015*](https://arxiv.org/abs/1511.05952)）或**梯度经验重放**（“GER”，[*Lahire等，2022*](https://arxiv.org/abs/2110.01528)）。这些方法试图根据与其“*学习潜力*”（PER的TD误差幅度和GER的经验梯度的范数）相关的某些指标优先选择经验。

为了简单起见，我们将在本文中实现一个均匀重放缓冲区。然而，我计划在未来详细讨论优先采样。

正如承诺的那样，均匀重放缓冲区的实现非常简单，但与JAX和函数式编程的使用相关的一些复杂性需要解决。与往常一样，我们必须使用**纯函数**，这些函数**没有副作用**。换句话说，我们不能将缓冲区定义为具有变量内部状态的类实例。

相反，我们初始化一个`buffer_state`字典，该字典将键映射到具有预定义形状的空数组，因为JAX在对XLA进行JIT编译时要求固定大小的数组。

```py
buffer_state = {
    "states": jnp.empty((BUFFER_SIZE, STATE_SHAPE), dtype=jnp.float32),
    "actions": jnp.empty((BUFFER_SIZE,), dtype=jnp.int32),
    "rewards": jnp.empty((BUFFER_SIZE,), dtype=jnp.int32),
    "next_states": jnp.empty((BUFFER_SIZE, STATE_SHAPE), dtype=jnp.float32),
    "dones": jnp.empty((BUFFER_SIZE,), dtype=jnp.bool_),
}
```

我们将使用`UniformReplayBuffer`类与缓冲区状态进行交互。这个类有两个方法：

+   `add`：解开一个经验元组，并将其组件映射到特定索引。`idx = idx % self.buffer_size`确保当缓冲区满时，添加的新经验会覆盖旧的经验。

+   `sample`：从均匀随机分布中采样一系列随机索引。序列长度由`batch_size`设定，而索引的范围为`[0, current_buffer_size-1]`。这确保了在缓冲区尚未满时不会采样到空数组。最后，我们使用JAX的`vmap`结合`tree_map`返回一批经验。

# 将**CartPole**环境转换为**JAX**

现在我们的DQN代理已准备好进行训练，我们将快速实现一个使用与[早期文章](/vectorize-and-parallelize-rl-environments-with-jax-q-learning-at-the-speed-of-light-49d07373adf5)介绍的相同框架的矢量化CartPole环境。 CartPole是一个具有**大型连续观察空间**的控制环境，这使得测试我们的DQN变得相关。

![](../Images/6c7f16ff97e5c92150e3a4ece0e1f843.png)

CartPole环境的可视化表示（鸣谢和文档：[OpenAI Gymnasium](https://gymnasium.farama.org/environments/classic_control/cart_pole/)，MIT许可证）

这个过程非常简单，我们大部分都重用了[OpenAI的Gymnasium实现](https://github.com/Farama-Foundation/Gymnasium/blob/main/gymnasium/envs/classic_control/cartpole.py)，同时确保我们使用JAX数组和lax控制流，而不是Python或Numpy的替代方案，例如：

```py
# Python implementation
force = self.force_mag if action == 1 else -self.force_mag
# Jax implementation
force = lax.select(jnp.all(action) == 1, self.force_mag, -self.force_mag)            )

# Python
costheta, sintheta = math.cos(theta), math.sin(theta)
# Jax
cos_theta, sin_theta = jnp.cos(theta), jnp.sin(theta)

# Python
if not terminated:
  reward = 1.0
...
else: 
  reward = 0.0
# Jax
reward = jnp.float32(jnp.invert(done))
```

为了简洁起见，完整的环境代码在此处可用：

[](https://github.com/RPegoud/jym/blob/main/src/envs/control/cartpole.py?source=post_page-----c1e45a179b92--------------------------------) [## jym/src/envs/control/cartpole.py at main · RPegoud/jym

### JAX实现的RL算法和矢量化环境 - jym/src/envs/control/cartpole.py at main ·…

[github.com](https://github.com/RPegoud/jym/blob/main/src/envs/control/cartpole.py?source=post_page-----c1e45a179b92--------------------------------)

# 以**JAX**的方式编写**高效的训练循环**

DQN实现的最后部分是训练循环（也称为推演）。 正如前文所述，为了利用JAX的速度，我们必须遵守特定的格式。

推演函数可能一开始看起来令人生畏，但它大部分的复杂性纯粹是语法上的，因为我们已经涵盖了大多数构建块。 这是一个伪代码演示：

```py
1\. Initialization:
  * Create empty arrays that will store the states, actions, rewards 
    and done flags for each timestep. Initialize the networks and optimizer
    with dummy arrays.
  * Wrap all the initialized objects in a val tuple

2\. Training loop (repeat for i steps):
  * Unpack the val tuple
  * (Optional) Decay epsilon using a decay function
  * Take an action depending on the state and model parameters
  * Perform an environment step and observe the next state, reward 
    and done flag
  * Create an experience tuple (state, action, reward, new_state, done)
    and add it to the replay buffer
  * Sample a batch of experiences depending on the current buffer size
    (i.e. sample only from experiences that have non-zero values)
  * Update the model parameters using experience batch
  * Every N steps, update the target network's weights 
    (set target_params = online_params)
  * Store the experience's values for the current episode and return 
    the updated `val` tuple
```

现在我们可以运行DQN进行**20,000步**并观察其表现。 大约在45集后，代理成功地保持了超过100步的稳定平衡。

**绿色条**表示代理成功地在**200多步**内平衡了杆，**解决了环境**。 值得注意的是，代理在**第51集**上创下了**393步**的记录。

DQN的性能报告（作者制作）

**20,000训练步骤**在**一秒多一点**内执行完毕，速度为**每秒15,807步**（*在单个CPU上*）！

这些表现提示了JAX令人印象深刻的扩展能力，允许从业者使用最小的硬件要求进行大规模并行实验。

```py
Running for 20,000 iterations: 100%|██████████| 20000/20000 [00:01<00:00, 15807.81it/s]
```

我们将更详细地看看**并行化的推演过程**，以运行**具有统计显著性**的实验和**超参数搜索**在未来的文章中！

与此同时，随时可以使用这本笔记本重现实验并尝试不同的超参数：

[](https://github.com/RPegoud/jym/blob/main/notebooks/control/cartpole/dqn_cartpole.ipynb?source=post_page-----c1e45a179b92--------------------------------) [## jym/notebooks/control/cartpole/dqn_cartpole.ipynb at main · RPegoud/jym

### JAX 强化学习算法和向量化环境的实现 - jym/notebooks/control/cartpole/dqn_cartpole.ipynb at…

github.com](https://github.com/RPegoud/jym/blob/main/notebooks/control/cartpole/dqn_cartpole.ipynb?source=post_page-----c1e45a179b92--------------------------------)

# 结论

如往常一样，**感谢您读到这里！**希望本文为您在 JAX 中的深度强化学习提供了一个不错的介绍。如果您对本文内容有任何问题或反馈，请务必告诉我，我总是乐意聊一聊 ;)

直到下次见面 👋

# 致谢：

+   [Cartpole Gif](https://gymnasium.farama.org/environments/classic_control/cart_pole/)，OpenAI Gymnasium 库，（MIT 许可证）
