# 实战深度 Q 学习

> 原文：[`towardsdatascience.com/hands-on-deep-q-learning-9073040ce841`](https://towardsdatascience.com/hands-on-deep-q-learning-9073040ce841)

## [强化学习](https://medium.com/tag/reinforcement-learning)

## 提升你的代理，以赢得更困难的游戏！

[](https://dr-robert-kuebler.medium.com/?source=post_page-----9073040ce841--------------------------------)![Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----9073040ce841--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9073040ce841--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9073040ce841--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----9073040ce841--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9073040ce841--------------------------------) ·14 分钟阅读·2023 年 11 月 25 日

--

![](img/8bdefc9ca92e8dc70018967dbcd36267.png)

照片由 [Sean Stratton](https://unsplash.com/@seanstratton?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

强化学习是机器学习中最迷人的领域之一。与监督学习不同，强化学习模型可以独立学习复杂的过程，即使没有精美整理的数据。

对我来说，看到 AI 代理赢得视频游戏是最有趣的，但你也可以使用强化学习来解决商业问题。只需将其表达为游戏，就可以开始了！你只需要定义……

+   你的代理所处的环境，

+   你的代理可以做出什么决策，以及

+   成功和失败的表现。

![](img/86ef974389efe4be3138d88a39347db3.png)

AI 代理掌握游戏的示例。接客并将其送到酒店。图片来源：作者。

在继续之前，请阅读我关于强化学习的介绍文章。它会给你更多背景信息，并展示如何自己进行简单但有效的强化学习。它也为本文提供了基础。

[## 实践者的强化学习指南](https://dr-robert-kuebler.medium.com/?source=post_page-----9073040ce841--------------------------------)

### 开始编写获胜的游戏 AI 代理的第一步

实践者的强化学习指南

在这篇文章中，你将了解深度 Q 学习，为什么我们需要它，以及如何自己实现它以掌握看起来比我另一篇文章中的游戏更复杂的游戏。

> **你可以在** [**我的 Github**](https://github.com/Garve/towards_data_science/blob/main/Hands-On%20Deep%20Q-Learning/Hands-On%20Deep%20Q-Learning.ipynb)**找到代码**。

# 大型观察空间

在上述链接的文章中，我们进行了 Q 学习，使得一个智能体能够在具有小的**离散观察空间**的一些简单游戏中进行游戏。例如，在 Frozen Lake 游戏中，你可以在 4x4 地图上的 16 个领域（=状态或观察）中站立。在[Blackjack 卡牌游戏的 gymnasium 版本](https://gymnasium.farama.org/environments/toy_text/blackjack/)中，有 32 · 11 · 2 = 704 个状态。

## Q 学习的低效性

在我提到的简单游戏中，Q 学习效果非常好，因为 Q 表保持得相当小。然而，更大的表意味着算法需要更多工作，因此 Q 学习变得更加低效。

这通常发生在你有**过多的观察**而不是过多的动作时，因为动作空间通常比观察空间小得多。例如，在 Blackjack 环境中，你只有 2 个动作但有 704 个状态。

## 卡车杆游戏

或者更糟的是：想象一下你的观察空间是**连续的**，例如在 gymnasium 提供的[卡车杆游戏](https://gymnasium.farama.org/environments/classic_control/cart_pole/)中。在这个游戏中，智能体必须通过左右转动卡车来学习如何平衡杆子。

![](img/fbcfb671e985c4768fe4f3d0d861e5db.png)

平衡那个杆子。图片由作者提供。

虽然这个游戏的动作空间仅由**两个动作**组成——向左或向右——观察空间由

+   车的位置，

+   其速度，

+   杆的角度和

+   杆的角速度，

每一个都是**实数**。这意味着你的观察空间是**无限**的，创建一个 Q 表对于它来说……是具有挑战性的。

## 离散化 Q 学习

作为解决方法，你可以**离散化**空间，即将其划分为**有限数量**的桶，并将每个连续状态映射到这些桶之一。然后你可以进行正常的 Q 学习。

![](img/8d71791766cfccb16dfea9deccaaec24.png)

四个桶用于卡车位置，并非政治光谱。图片由作者提供。

这就提出了一个问题，你必须决定要使用多少个桶。如果你使用的桶太多，Q 表将会过大。如果桶太少，非常不同的状态将被同等对待，这可能导致智能体性能较差。这可能发生在上面的图像中，例如，其中 0 到 3 之间的所有卡车位置都被视为相同。在模型看来，位置 0.1 与位置 2.9 是一样的。

我们可能在另一篇文章中尝试这种方法，但请注意，它基本上是我们所知道的 Q 学习，只是对观察进行了额外的离散化步骤。然而，在这篇文章中，我们想应用 **深度 Q 学习**，它能够 **直接处理连续观察空间**！

# 深度 Q 学习

之前，我们为 Q 表中的每个状态 *s* 和动作 *a* 保留了一个单元。如果这些表格变得过大，我们可以尝试另一种策略：**通过函数建模 Q 值**！

对于给定的状态 *s* 和动作 *a*，我们希望输出 (接近) Q 值。由于我们不能自己构建这个函数——至少我不能——让我们使用一个具有可学习权重的神经网络作为近似。

![](img/ece725a20c5708d290889e20dab7fed1.png)

Q 学习与深度 Q 学习的比较。作者提供的图片。

这不是我编造的，它早在 1993 年就以 **QCON**（连接主义 Q 学习）的名称在 Long-Ji Lin 的论文 “[使用神经网络的机器人强化学习](https://isl.anthropomatik.kit.edu/pdf/Lin1993.pdf)” 中描述过。*祝 30 周年快乐！*

作者还在其目前的形式中引入了 **经验回放** 的概念，这种形式在著名的 DeepMind 论文 [使用深度强化学习玩 Atari 游戏](https://arxiv.org/abs/1312.5602) 中被使用。

**注意：** 你可能会想知道为什么网络不使用状态 *s* 和动作 *a* 作为输入，因为这将是用模型替代表的直接方法。输入状态和动作，接收 Q 值。DeepMind 论文的作者说如下：

> 有几种可能的方法可以使用神经网络对 Q 进行参数化。由于 Q 将历史-动作对映射到它们的 Q 值的标量估计，一些先前的方法使用历史和动作作为神经网络的输入。[……] 这种架构的主要缺点是需要单独的前向传播来计算每个动作的 Q 值，这导致成本随动作数量线性增长。[……] 我们类型架构的主要优势是能够通过对网络进行单次前向传播来计算给定状态下所有可能动作的 Q 值。

## 直观理解

在我的另一篇文章中，我告诉你 Q 表中的更新是通过以下公式完成的

![](img/14044cb86ca0795fd4f22516a755c5d4.png)

作者提供的图片。

用简单的话说，就是我们将旧的价值 *Q*(*s*, *a*) 稍微向目标值 *R*(*s*, *a*) + *γ*·max*ₐ* *Q*(*s*’, *a*) 的方向移动，其中 *s* 是状态，*s’* 是在采取行动 *a* 后的下一个状态，*R* 是奖励函数，*γ* < 1 是折扣因子。*α* 是学习率，告诉我们将旧的 Q 值 *Q*(*s*, *a*) 移动到方向 *R*(*s*, *a*) + *γ*·max*ₐ* *Q*(*s*’, *a*) 的程度。

**小示例。** 假设 *Q*(*s*, *a*) = 3， *R*(*s*, *a*) + *γ*·max*ₐ* *Q*(*s*’, *a*) = 4，并且 *α =* 0.1*。* 那么我们原来的值 3 会更新为 3 + 0.1·(4–3) = 3.1，稍微偏离 3 向 4 的方向。

这意味着这个更新规则逐步改变条目，使得某个时刻我们有 *Q*(*s*, *a*) = *R*(*s*, *a*) + *γ*·max*ₐ* *Q*(*s*’, *a*)，这也称为**贝尔曼方程**。

令人惊叹的观察是，我们可以将这个目标转换为一个简单的机器学习任务给我们的神经网络。所以，假设我们在环境中执行一步。我们从状态 *s* 开始，执行某个动作 *a*，然后进入新状态 *s’*。我们还从环境中获得了奖励 *r* = *R*(*s*, *a*)。接下来，我们按照以下步骤进行训练：

1.  计算 *N*(*s*) 和 *N*(*s*’)，它们都是实数向量，每个动作一个向量。

1.  查看 *N*(*s*) 的 *a* 号动作，称其为 *N*(*s*)[*a*]。

1.  确保 *N*(*s*)[*a*] 更接近 *R*(*s*, *a*) + *γ*·max*ₐ* *N*(*s*’) 通过执行一个梯度更新步骤来最小化均方误差，这意味着 (*R*(*s*, *a*) + *γ*·max*ₐ* *N*(*s*’) - *Q*(*s*, *a*))²。

然后我们可以在环境中再进行一步，获取新数据 (*s*, *a*, *s’*, *r*) (**阅读：** 代理在状态 *s* 中，执行 *a*，进入 *s’*，并获得奖励 *r*)，然后再次执行步骤 1–3。我们这样做直到有一个足够好的代理，或者我们耗尽耐心或资金进行训练。

## 经验回放

上述直觉有一个缺陷：*它并不像那样有效*。当我刚开始时，我实现了这个逻辑，但从未得到过好的模型。

原因是我每次只用**单个训练样本**更新模型。这就像进行纯随机梯度下降。它可能有效，但在优化过程中，模型参数会剧烈波动，可能很难收敛。另一个问题是**后续动作高度相关**，因为状态通常不会因为单个动作而变化太多。现在，我们知道一种简单的方法来解决这两个问题：**使用小批量**！

> 花哨的术语 *经验回放* 仅仅是这样。

你不是在环境中执行单个步骤，而是多个步骤，并将**记忆** (*s*, *a*, *s’*, *r*) 存储到某种数据结构中，即**回放记忆**。它可以是一个数组，但通常，人们使用 [双端队列](https://en.wikipedia.org/wiki/Double-ended_queue) 来实现**有限的**回放记忆。

![](img/9f5ea3399d5de8e8fd65dbf6b49c0948.png)

图片由作者提供。

新的记忆会将旧的记忆淘汰，一旦回放记忆的大小限制达到。这是有道理的，因为非常旧的记忆不应该对现在发生的事情产生太大影响。这也使得模型能够适应游戏中的新规则。

![](img/a3ed1f5030d692784cf9b2127c71644b.png)

7 淘汰 1。图片由作者提供。

## 伪代码

好了，了解了这些知识后，让我们看看《Playing Atari with Deep Reinforcement Learning》论文中的伪代码。唯一的区别是，他们可能会使用某些函数 φ 预处理状态 *s*（我们这里没有这样做），并且在代理达到终止状态时，即游戏结束时，他们会移除最大项。

![](img/ebad2f580c430a3c52d916896a03cf7d.png)

来自他们的论文。

现在我们可以在 Python 中实现这个想法了！

# 实现

首先，让我们导入一些库并定义一些参数。

```py
import random
from collections import deque, namedtuple

import gymnasium as gym
import numpy as np
import tensorflow as tf
from tqdm.auto import tqdm

n_episodes = 1000 # play 1000 games
eps = 0.4 # exploration rate, probability of choosing random action
eps_decay = 0.95 # eps gets multiplied by this number each epoch...
min_eps = 0.1 # ...until this minimum eps is reached
gamma = 0.95 # discount
max_memory_size = 10000 # size of the replay memory
batch_size = 16 # batch size of the neural network training
min_length = 160 # minimum length of the replay memory for training, before it reached this length, no gradient updates happen
memory_parts = ["state", "action", "next_state", "reward", "done"] # nice names for the part of replay memory, otherweise the names are 0-5
```

这里没有什么特别的，但如果有不清楚的地方请阅读注释。

## 回放记忆

现在，我们定义了回放，这只是一个 Python deque 的薄包装器。

```py
Memory = namedtuple("Memory", memory_parts) # a single entry of the memory replay

class ReplayMemory:
    def __init__(self, max_length=None):
        self.max_length = max_length
        self.memory = deque(maxlen=max_length)

    def store(self, data):
        self.memory.append(data)

    def _sample(self, k):
        return random.sample(self.memory, k)

    def structured_sample(self, k):
        batch = self._sample(k)
        result = {}
        for i, part in enumerate(memory_parts):
            result[part] = np.array([row[i] for row in batch])

        return result

    def __len__(self):
        return len(self.memory)
```

在这里，我们定义了一个易于使用的记忆回放。让我们先玩一下，然后再实际使用它。我们可以进行

```py
r = ReplayMemory(max_length=3)

r.store(("a", "b", "c", "e", "f"))
r.store((1, 2, 3, 4, 5))
r.store((6, 7, 8, 9, 0))

print(r.structured_sample(2)) # get 2 random sampples from the replay memory

# Output (for me):
# {
#   'state': array([1, 6]),
#   'action': array([2, 7]),
#   'next_state': array([3, 8]),
#   'reward': array([4, 9]),
#   'done': array([5, 0])
# }
```

如果我们添加另一个记忆，我们将丢失第一个记忆：

```py
r.store((0, 0, 0, 0, 0))

print(r.memory)

# Output:
# deque([(1, 2, 3, 4, 5), (6, 7, 8, 9, 0), (0, 0, 0, 0, 0)], maxlen=3)
# no more letters in here!
```

## 模型

让我们定义一个简单的模型！我们将在这里使用 TensorFlow，但也可以使用 PyTorch，[JAX](https://github.com/google/jax) 或其他工具。

```py
model = tf.keras.Sequential(
    [
        tf.keras.layers.Dense(16, input_shape=(4,), activation="relu"), # state consists of 4 floats
        tf.keras.layers.Dense(16, activation="relu"),
        tf.keras.layers.Dense(16, activation="relu"),
        tf.keras.layers.Dense(16, activation="relu"),
        tf.keras.layers.Dense(2, activation="linear"), # 2 actions: go left or go right
    ]
)
model.compile(
    loss=tf.keras.losses.MeanSquaredError(),
    optimizer=tf.keras.optimizers.Adam(learning_rate=0.01),
)
```

## 训练

准备好了，就让我们开始训练吧。

```py
env = gym.make("CartPole-v1")
replay_memory = ReplayMemory(max_length=max_memory_size)

for episode in tqdm(range(n_episodes)): # tqdm makes a nice proress bar
    state, _ = env.reset()
    done = False

    while not done:
        if random.random() < eps:
            action = env.action_space.sample() # random action
        else:
            action = model.predict(state[np.newaxis, :], verbose=False).argmax() # best action according to the model

        next_state, reward, done, _, _ = env.step(action)
        memory = Memory(state, action, next_state, reward, done)
        replay_memory.store(memory)

        if len(replay_memory) >= min_length:
            batch = replay_memory.structured_sample(batch_size) # get samples from the replay memory

            target_batch = batch["reward"] + gamma * model.predict(batch["next_state"], verbose=False).max(axis=1) * (1 - batch["done"]) # R(s, a) + γ·maxₐ N(s') if not a terminal state, otherwise R(s, a)
            targets = model.predict(batch["state"], verbose=False)
            targets[range(batch_size), batch["action"]] = target_batch # set the target for the action that was done and leave the outputs of other 3 actions as they are

            model.fit(batch["state"], targets, verbose=False, batch_size=batch_size) # train for one epoch

        state = next_state

    eps = max(min_eps, eps * eps_decay)
```

就这样！它看起来有点像 Q-learning 的代码，但表格中单元格的简单更新被替换为一个 `fit` 步骤的梯度下降。

# 结果

代码可能会运行几个小时。你也可以在训练期间记录一些数据，例如每个回合的奖励。另一个常见做法是**每隔几个回合记录模型，** 这样当训练崩溃时，你不会丢失太多工作。不过，保存模型还有另一个好处：**通常情况下，最好的模型不是最新的那个**。你可以从我的奖励图中看到这一点：

![](img/80e4eb25687823fc3ae665262219405a.png)

作者提供的图片。

你可以看到最开始，代理表现很差。大约第 100 集时，代理已经获得了大约 200 的奖励，然后又下降了。大约第 230 集时，模型变得异常优秀，但直到第 1000 集时才又变得较差，那时我结束了训练。这很奇怪，但这是一个我在其他人做强化学习时也常见的模式。

## 来玩吧！

有什么比看到我们辛苦训练的模型实际运作更好的呢？对，*没有*，所以让我们开始吧！首先，让我们看看随机代理如何竞争。

![](img/55259aa459a1311b4c538892d2a7bf11.png)![](img/63493a6d3a03e735fd789209597a15b4.png)![](img/e72ccb63fbe8908731feb854a5042f52.png)

随机代理的三次不同运行。白色闪光表示游戏结束。图片由作者提供。

哇，随机代理**真的**很糟糕。中间的运行开始时看起来还不错，但随机性使其无法向左移动。你可以通过以下方式重现这样的悲惨运行：

```py
env = gym.make("CartPole-v1", render_mode="human")

env.reset()
done = False

while not done:
    env.render()
    action = env.action_space.sample()
    _, _, done, _, _ = env.step(action)

env.close()
```

但你来这里是为了别的，对吧？

## 新的挑战者出现

让我们加载第 230 集的模型，看看它在实时测试中的表现如何！

```py
model = tf.keras.models.load_model("PATH/TO/MODEL/230") # the model is in my Github, https://github.com/Garve/towards_data_science/blob/main/A%20Tutorial%20on%20Deep%20Q-Learning/230.zip
env = gym.make("CartPole-v1", render_mode="human")

state, _ = env.reset()
done = False
total_reward = 0

while not done and total_reward < 500: # force end the game after 500 time steps because the model is too good!
    env.render()
    action = model.predict(state[np.newaxis, :], verbose=False).argmax(axis=1)[0]
    state, reward, done, _, _ = env.step(action)
    total_reward += reward

env.close()
```

![](img/ebcf2a47b21c58dc85e62200ea844867.png)

我们的第 230 集的智能体动作流畅。经过 500 个时间步后，我切断了动画，但智能体并没有失败。图片由作者提供。

看这儿！智能体可以轻松地用最小的动作保持杆子的平衡。然而，你可以看到它略微向右漂移，这在长时间内*可能*会成为一个问题。让我们将 500 个时间步的人工限制增加到几千个，并查看加速版本：

![](img/e95b77d67a30262f38b36642ec0158a9.png)

智能体 230 再次以 60fps 的游戏风格行动。图片由作者提供。

看起来这个智能体表现不错，即使经过了 500 步。太棒了！

# 结论

在这篇文章中，我们解释了为什么普通的 Q-learning 在处理大规模甚至潜在无限的观察空间时会失败。离散化 Q-learning 在这种情况下可以有所帮助，但你必须考虑你离散化的程度，以找到智能体性能和训练时间之间的平衡。

离散化的 Q-learning 方法完全没问题，但我们选择了另一种成功应用于更复杂游戏的技术——**深度 Q-learning**。我们不是更新一个可能很大的 Q 表条目，而是训练一个具有**固定参数量**的模型。

*不过，我们还是要诚实*。深度 Q-learning *也是*在训练时间和智能体性能之间的权衡。像线性回归（没有隐藏层）这样的小型网络可能表现得很糟。如果你有数百万个参数用于摆杆游戏，那就是过度配置了，而且训练需要很长时间。

> 你也必须找到一个平衡，这不是免费的。

然而，从论文来看，似乎深度 Q-learning 对于像 [Atari 游戏](https://gymnasium.farama.org/environments/atari/) 这样更复杂的游戏仍然更有前景。

## 从感官输入中学习

到目前为止，我们使用了环境的**内部状态**。在摆杆游戏中，我们获得了汽车的位置、速度、角度等信息。当我们作为人类玩游戏时，通常**我们没有这种奢侈**。我的意思是，我们无法足够快地处理这些信息以在游戏中表现出色。我们必须依赖视觉反馈——屏幕——来做出快速决策。

如果智能体也只能使用屏幕输出呢？正如《用深度强化学习玩 Atari》的作者所展示的那样，它效果相当好！正如他们所写：

> 我们将我们的方法应用于来自 Arcade Learning Environment 的七款 Atari 2600 游戏，未对架构或学习算法进行任何调整。我们发现它在六款游戏中优于所有以前的方法，并在三款游戏中超越了人类专家。

作者使用了一个深度卷积神经网络，它接收屏幕，处理它所看到的图像，并输出 Q 值。准确地说，他们**不使用单帧图像**，而是使用**游戏的最后 4 帧**。这使模型能够学习屏幕上物体的移动速度和方向。

但我现在先这样，我们可以在另一篇文章中以清新的思维深入探讨这个话题！

希望你今天学到了一些新、趣味且有价值的东西。感谢阅读！

> *如果你有任何问题，可以在* [*LinkedIn*](https://www.linkedin.com/in/dr-robert-k%C3%BCbler-983859150/)*上联系我！*

如果你想更深入了解算法的世界，可以尝试我的新出版物《算法全景》！我仍在寻找作者！

[](https://allaboutalgorithms.com/?source=post_page-----9073040ce841--------------------------------) [## 《算法全景》

### 从直观解释到深入分析，算法通过示例、代码和令人惊叹的内容变得生动。

[allaboutalgorithms.com](https://allaboutalgorithms.com/?source=post_page-----9073040ce841--------------------------------)
