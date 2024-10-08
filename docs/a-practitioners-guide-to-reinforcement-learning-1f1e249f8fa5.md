# 强化学习实践者指南

> 原文：[`towardsdatascience.com/a-practitioners-guide-to-reinforcement-learning-1f1e249f8fa5`](https://towardsdatascience.com/a-practitioners-guide-to-reinforcement-learning-1f1e249f8fa5)

## [强化学习](https://medium.com/tag/reinforcement-learning)

## 踏出编写游戏获胜 AI 代理的第一步

[](https://dr-robert-kuebler.medium.com/?source=post_page-----1f1e249f8fa5--------------------------------)![Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----1f1e249f8fa5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1f1e249f8fa5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f1e249f8fa5--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----1f1e249f8fa5--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1f1e249f8fa5--------------------------------) ·阅读时间 15 分钟·2023 年 11 月 18 日

--

![](img/93f09881c02997c942a1dd69ddc3b8eb.png)

图片由[Vincent Guth](https://unsplash.com/@vingtcent?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在机器学习中，数据科学家主要在监督学习和无监督学习领域进行探索。然而，还有一个独特而有趣的子领域——**强化学习**！

在强化学习中，我们尝试教一个所谓的**代理**如何应对*游戏*的复杂性，将它放置在一个模拟环境中，在这里它探索策略，因成功的行动而获得奖励，而因失误而面临惩罚。

![](img/b539adcd0ea6511de8d0f141fb96c4dd.png)

强化学习的典型概述。图片来源于作者。

强化学习领域的一个显著成果是[AlphaGo](https://en.wikipedia.org/wiki/AlphaGo)，这是一个击败了[围棋](https://en.wikipedia.org/wiki/Go_(game))世界冠军的模型，而围棋比国际象棋复杂得多。

> 强化学习的伟大之处在于我们不需要告诉代理**如何**获胜。我们只需告诉它获胜或失败的标准是什么。

例如，在国际象棋中，这就是将死对方的国王，这也是我们提供的唯一指导。没有关于皇后的重要性或兵的微不足道性的明确指示——代理自己推断出这些细微差别。

它不仅限于*传统游戏*；几乎任何东西都可以视作游戏。无论是经典的棋盘游戏、视频游戏，还是商业场景，例如确定最有效的广告，强化学习都在其中。在商业场景中，代理可以因成功的客户购买获得奖励，因广告点击获得较少的奖励，并在客户忽视广告时面临惩罚。这对代理来说是一个战略游戏，优化奖励，在商业环境中，这转化为收入。

在这篇文章中，我不会过多探讨强化学习的数学理论。我想给你提供**直观的理解和工作代码**以帮助你入门。为此，我将使用优秀的库 [gymnasium](https://gymnasium.farama.org/)，它提供了一些精美的游戏环境，让我们的代理可以学习掌握。

***你可以在*** [***我的 Github***](https://github.com/Garve/towards_data_science/blob/main/A%20Practitioner%E2%80%99s%20Guide%20to%20Reinforcement%20Learning.ipynb)***找到代码！***

# 热身

在开始之前，我们先熟悉一下 gymnasium，了解它能为我们做什么。首先，通过 `pip install gymnasium[toy-text] Pillow` 安装它。`toy-text` 部分还会安装 [pygame](https://www.pygame.org/news)，这样我们可以得到一些漂亮的视觉效果。

## 开始游戏

现在我们创建一个游戏实例并打印游戏状态的图片。

```py
import gymnasium as gym
from PIL import Image

env = gym.make("FrozenLake-v1", render_mode="rgb_array", is_slippery=False)
env.reset()

Image.fromarray(env.render())
```

![](img/4b54c861e617c52b0c0ee4006f2586ca.png)

从游戏起始状态的截图。图片由作者提供。

这个游戏叫做 Frozen Lake，目标是将角色（左上角）移动到当前位置（右下角），而不掉入冰湖中。为此，你可以将角色向上、向下、向左或向右移动。很简单，对吧？

## 使用一个动作更改状态

现在你可以通过执行 `env.step(1)` 让角色向下移动一步。另一个 `Image.fromarray(env.render())` 给我们带来了

![](img/fd6b275f6c4aac78bc4ee78b52d9cf70.png)

图片由作者提供。

总共有四个动作：

+   **0 —** 向左移动

+   **1 —** 向下移动

+   **2** — 向右移动

+   **3** — 向上移动

`env.step(...)` 也会返回一个元组 `(4, 0.0, False, False, {"prob": 1.0})`，这有些难以阅读。我们只关注前三个条目，它们是： (观察值，奖励，终止，…)。这意味着在向下移动一步后，我们的位置是**4**，获得的奖励是**0.0**，且游戏**尚未结束**。

![](img/685e43f22381fb63a51f0ac04bd4a653.png)

状态（位置）。图片由作者提供。

你可以通过 `env.step(2)` 向右移动一步，最终状态变为 5，再次没有获得奖励，游戏结束，因为你掉入了冰湖。游戏结束。

![](img/19e8c1b45fcf69fb3f3173535ecc877c.png)

游戏结束。图片由作者提供。

## 赢得游戏

如果你通过选择正确的动作序列到达当前位置，你将获得 1.0 的奖励，并且游戏结束。一个序列可能是 (1, 1, 2, 1, 2, 2)。

![](img/dbc2ce19c4cbfdce642f4cf1f6c9ced2.png)

你赢了！图片由作者提供。

# 使用强化学习赢得冰冻湖游戏

在这个简单的游戏中，如果我们愿意，可以不用强化学习。我们可以从起始位置开始编写一个[深度优先搜索](https://en.wikipedia.org/wiki/Depth-first_search)，其中当前的位置是我们想要到达的顶点。然而，这并不适用于每个游戏，特别是当你处理**随机**游戏时，即当你可以采取的步骤会导致可能出现许多新状态时。我们稍后会看到一个例子。

相反，接下来我们将开发一个算法，该算法可以生成**策略**来赢得这个游戏，以及许多其他游戏。策略是一种**计划**，你按照它来赢得游戏。它告诉你根据你所处的状态（即棋盘上的位置）应该采取哪个动作。

## Q 学习

建立策略的一种方法是使用一种叫做**Q 学习**的东西，其中 Q 代表质量。在这种方法中，你希望构建一个函数 *Q*，它接受一个状态 *s* 和一个动作 *a*，并输出一个数字 *Q*(*s*, *a*)，告诉你在状态 *s* 中采取动作 *a* 的好坏。

> Q(s, a) = 衡量在状态 s 中采取动作 a 的好坏（质量）的指标

**注意：** 你也可以将 *Q* 视为一个二维查找表（dataframe）。索引是游戏状态，而列可以是动作，例如。

![](img/3e7f0864f870e8d3dc7ce84682a3355d.png)

图片由作者提供。

在这个例子中，你有 *Q*(3, B) = 1.6。如果你有这样的 Q 函数，你可以这样确定最佳的动作：插入你可以采取的所有不同动作，并选择最大化 Q 值的动作。这是合理的，因为 *Q* 衡量的是状态的优劣，而你总是希望处于最佳状态。在上面的例子中，如果你在状态 1，最佳的动作是动作 A。如果你在状态 2 或 3，你应该选择动作 B。大问题是：

> 如何用有意义的值来构建这个表格？

我会跳过一个***巨大的***捷径，告诉你 Watkins 在 1989 年提出的强化学习领域的突破：如果你想逼近给出**最佳**策略的函数 *Q*，你可以将 *Q*(*s*, *a*) 的值初始化为一些值——通常为零——然后根据以下规则更新它：

![](img/46bad550d09e40ccc041ba08bc4f269c.png)

Q 学习的魔力。图片由作者提供。

这非常复杂，在本文中我无法详细说明。但让我稍微解释一下：

1.  我们从一些初始值开始，比如说将所有 *Q*(*s*, *a*) 的值设为零。我们将会逐步更新这些值。

1.  假设在我们的游戏中，我们处于某个状态 *s*。

1.  我们采取一个动作 *a*。这可以是随机进行的，也可以遵循一些其他逻辑。

1.  做完这些之后，我们会从游戏中获得一个即时奖励 *R*(*s*, *a*)（可以是零），并且由于我们的动作，我们也会发现自己进入了一个新状态 *s*’。

1.  然后我们必须计算当我们处于那个新状态*s’*时，具有最高 Q 值的最佳动作——这是最大项。

所以，我们知道*Q*(*s*, *a*)，*R*(*s*, *a*)和 max *Q*(*s*’， *a*)是什么。然后，我们使用上面的公式更新我们旧的*Q*(*s*, *a*)值。这里，*α*是学习率，*γ*是另一个称为*折扣*的超参数。高折扣意味着未来的奖励对算法非常重要。低折扣鼓励一种贪婪的策略，想要立即获得奖励，即使从长远来看可能不太好。通常，你可以选择接近 1 的值，但这取决于问题。

## 实现

现在让我们在 Python 中实现所有内容。首先，我们编写一个繁琐的辅助函数。这是必要的，因为 gymnasium 中的状态和动作有时并不容易处理。

```py
from itertools import product
import random

from gymnasium.spaces.tuple import Tuple

def space_to_tuples(space):
    if isinstance(space, Tuple):
        for encoding in product(*[range(factor.n) for factor in space]):
            yield encoding
    else:
        for encoding in range(space.n):
            yield encoding

def get_best_action(q_table, state): # for a given state, find the best action from the Q table
    return max(((action, value) for action, value in q_table[state].items()), key=lambda x: x[1])[0]
```

现在，我们可以初始化超参数*α*和*γ*，同时初始化最重要的成分：Q 表！

```py
alpha = 0.1
gamma = 0.9
n_episodes = 100_000
max_steps = 100
epsilon = 0.2

q_table = {i: {j: 0 for j in space_to_tuples(env.action_space)} for i in space_to_tuples(env.observation_space)}
```

Q 表看起来是这样的：

```py
{0: {0: 0, 1: 0, 2: 0, 3: 0},
 1: {0: 0, 1: 0, 2: 0, 3: 0},
 2: {0: 0, 1: 0, 2: 0, 3: 0},
 3: {0: 0, 1: 0, 2: 0, 3: 0},
 4: {0: 0, 1: 0, 2: 0, 3: 0},
 5: {0: 0, 1: 0, 2: 0, 3: 0},
 6: {0: 0, 1: 0, 2: 0, 3: 0},
 7: {0: 0, 1: 0, 2: 0, 3: 0},
 8: {0: 0, 1: 0, 2: 0, 3: 0},
 9: {0: 0, 1: 0, 2: 0, 3: 0},
 10: {0: 0, 1: 0, 2: 0, 3: 0},
 11: {0: 0, 1: 0, 2: 0, 3: 0},
 12: {0: 0, 1: 0, 2: 0, 3: 0},
 13: {0: 0, 1: 0, 2: 0, 3: 0},
 14: {0: 0, 1: 0, 2: 0, 3: 0},
 15: {0: 0, 1: 0, 2: 0, 3: 0}}
```

我们可以看到状态编号从 0（左上角的区域）到 15（右下角的区域），每个状态有四个动作。所有内容都初始化为零，但这不是必须的。

实际训练——一切的核心——是这样的：

```py
for _ in range(n_episodes):
    # new episode (game), so we need a reset
    state, _ = env.reset()

    # play the game for max_steps
    for step in range(max_steps):
        # pick an action
        if random.random() < epsilon:
            # sometimes it is random to encourage exploration, otherwise we cannot find good policies ...
            action = env.action_space.sample()
        else:
            # ... and sometimes we use the best action according to our Q-table
            action = get_best_action(q_table, state)

        # we take that action and then get some data from the game
        next_state, reward, terminated, _, _ = env.step(action)

        # update the Q-table according to the magic formula
        q_table[state][action] = (1-alpha)*q_table[state][action] + alpha*(reward + gamma*max(q_table[next_state].values()))

        # check if the game is finished, can be if you win or lose
        if terminated:
            # a new episode starts after this
            break
        else:
            # if the episode continues, update the current state (we took an action!)
            state = next_state
```

代码应在几秒钟后完成。我们现在已经创建了一个 Q 表，并查看它：

```py
{0: {0: 0.53, 1: 0.59, 2: 0.59, 3: 0.53},
 1: {0: 0.53, 1: 0.0, 2: 0.66, 3: 0.55},
 2: {0: 0.56, 1: 0.73, 2: 0.43, 3: 0.63},
 3: {0: 0.6, 1: 0.0, 2: 0.17, 3: 0.0},
 4: {0: 0.59, 1: 0.66, 2: 0.0, 3: 0.53},
 5: {0: 0, 1: 0, 2: 0, 3: 0},
 6: {0: 0.0, 1: 0.81, 2: 0.0, 3: 0.64},
 7: {0: 0, 1: 0, 2: 0, 3: 0},
 8: {0: 0.66, 1: 0.0, 2: 0.73, 3: 0.59},
 9: {0: 0.66, 1: 0.81, 2: 0.81, 3: 0.0},
 10: {0: 0.73, 1: 0.9, 2: 0.0, 3: 0.73},
 11: {0: 0, 1: 0, 2: 0, 3: 0},
 12: {0: 0, 1: 0, 2: 0, 3: 0},
 13: {0: 0.0, 1: 0.81, 2: 0.9, 3: 0.73},
 14: {0: 0.81, 1: 0.9, 2: 1.0, 3: 0.81},
 15: {0: 0, 1: 0, 2: 0, 3: 0}}
```

你可以从中推导出以下最佳策略：

```py
{0: 'Down',
 1: 'Right',
 2: 'Down',
 3: 'Left',
 4: 'Down',
 5: 'Left',
 6: 'Down',
 7: 'Left',
 8: 'Right',
 9: 'Down',
 10: 'Down',
 11: 'Left',
 12: 'Left',
 13: 'Right',
 14: 'Right',
 15: 'Left'}
```

或者以图形方式

![](img/72b575aa3d9766384f02f7e10e086550.png)

作者提供的图片。

在没有箭头的区域上的最佳动作实际上是未定义的，因为游戏在这些状态下结束。但它随机选择“左”，因为这是动作字典中的第一个条目。

## 玩游戏

我们可以使用这个训练好的表格来赢得游戏。

```py
def render_func(env):
    img = Image.fromarray(env.render())
    display(img)

state, _ = env.reset()

while True:
    render_func(env) # draw a picture
    action = get_best_action(q_table, state) # best action according to our table
    next_state, _, terminated, _, _ = env.step(action)

    if terminated:
        break
    else:
        state = next_state

render_func(env)
```

你应该看到我们的代理赢得了游戏！

![](img/dbc2ce19c4cbfdce642f4cf1f6c9ced2.png)

再次获胜，但**不是硬编码的**！作者提供的图片。

## 我们赢的频率是多少？

我们也可以模拟许多游戏来看看我们的算法赢得的频率。

```py
def play_episode(env, q_table):
    state, _ = env.reset()

    while True:
        action = get_best_action(q_table, state)
        next_state, reward, terminated, _, _ = env.step(action)

        if terminated:
            break
        else:
            state = next_state

    return reward

rewards = []
for episode in range(10000):
    rewards.append(play_episode(env, q_table))
```

如前所述，如果我们赢，我们会得到 1.0 的奖励，否则是 0.0。让我们统计一下我们得到了什么：

```py
from collections import Counter

print(Counter(rewards))

# Output:
# Counter({1.0: 10000})
```

因此，我们赢得了每一场游戏。这是有道理的，因为游戏以及我们的策略是**完全确定的**。我们总是从同一个位置开始，并且必须到达同一个目标。场地也总是看起来一样，我们在相同的情况下行为一致。因此，我们要么总是赢，要么总是输。

# 不需要代码更改就能赢得其他游戏

你可能会想知道我们为什么在赢得这个简单的游戏上投入了这么多精力。好吧，我们取得的伟大成就的好处在于，它**不关心我们玩的游戏类型**，只要它具有**离散的状态和动作空间**。让我们试试看！

## 出租车

执行与之前相同的代码，只需使用新的游戏环境。

```py
env = gym.make("Taxi-v3", render_mode="rgb_array")
```

这是一个关于接送人员并将其送到酒店的游戏。为了赢得游戏，你必须：

1.  驱车前往接送人员。

1.  接送他们。

1.  驱车前往酒店。

1.  把他们放下。

动作空间包括向左、向下、向右、向上、捡起和放下。你可以在以下动画中看到如何玩这个游戏：

![](img/f2148c45d82d217bab5b1d48e9988e2c.png)

AI 像专业人士一样玩。图像由作者提供。

你可以看到我们的代理在训练后如何轻松获胜！

## 二十一点

简而言之，二十一点是关于抽卡，只要不超过 21。然而，你有一个对手：你手中的点数必须高于庄家的点数。学习这个游戏时，从以下开始

```py
env = gym.make('Blackjack-v1', natural=True, sab=False, render_mode="rgb_array")
```

然后在训练后你得到

![](img/ea0f757056fcc202263a71bba9381152.png)

AI 做出好的决策。图像由作者提供。

在这里，你可以看到 AI™在 14 时做出了抽取另一张卡片的好决定，但在 20 时停止。

为什么在 14 时抽取另一张卡片是好的？因为庄家当前有 6 的情况下，很可能会抽到 10（10、J、Q、K）。所以他可能会有 16，如果你在 14 时停牌，他很可能会输。

## 滑冰冰冻湖

规则与冰冻湖相同，但有一个变化：**你并不总是能得到你想要的方向**。你可以通过以下方式获得

```py
env = gym.make("FrozenLake-v1", render_mode="rgb_array", is_slippery=True)
```

例如，如果你想向右移动，有 1/3 的概率向右、1/3 的概率向上、1/3 的概率向下。一般来说，你有 1/3 的概率去任何方向，而不是向后移动。这使得事情变得更加困难：

![](img/565aeac3f90bb339a6eb4378b578a23b.png)

胜利不再那么容易。图像由作者提供。

原因在于处于两个湖泊之间是一个危险的位置。如果你想向上移动，你可能会不小心向左或向右移动，有`2/3`的概率掉入湖泊。同样，如果你向下移动也是如此。因此，最佳策略是向左或向右移动，因为这样你可能会有`2/3`的概率向上或向下移动，并有`1/3`的概率撞上冰面。

我的代理学到了以下策略：

![](img/696201257dcffb4ec8e0c34e219e34cc.png)

滑冰情况下的最佳策略。图像由作者提供。

你可以清楚地看到代理如何试图避免湖泊。如果它靠近湖泊，它会远离湖泊，使得无法掉入湖中。唯一的危险在于两个湖泊之间。代理想要向右移动——这是一个最佳决定——但可能最终会掉入右边的湖泊。

当我让我的代理进行游戏时，它在**55%**的情况下获胜。相比之下：如果它随机移动，它大约有 1.5%的胜率，而如果我们使用**在非滑冰情况下的最佳策略，它的胜率也令人惊讶地约为 1.5%**。

![](img/6bde5ed08bda4ea21fa1672cf6949971.png)

以风格滑动。图像由作者提供。

## 奖励：多臂老虎机

这是强化学习入门示例中的经典案例。想象你有*n*台老虎机，你试图找到给你最高奖励的那一台。你总是可以玩一台机器并观察奖励。这里只有**一个状态**和*n*个动作。在我们的例子中，*n* = 10。

Gymnasium 不包含这个游戏，但你可以安装一个与 gymnasium 集成的第三方库。

```py
git clone https://github.com/magni84/gym_bandits.git
cd gym_bandits
pip install -e .
```

然后你可以创建如下环境：

```py
import gym_bandits

env = gym.make('MultiarmedBandits-v0')
```

然后再次执行 Q 学习步骤。我会只使用一个回合，因为游戏永远不会结束。只是稍微增加`max_steps`的数量。你会发现 Q 表中最高的条目将是正确的老虎机。

![](img/e2ee96d07ab9427921d78c95ee48f307.png)

这里没有漂亮的动画。图像由作者提供。

# 结论

在这篇文章中，我们看到了如何教一个代理玩具有离散且有限状态和动作空间的简单游戏。这是 Q 学习简单形式能够发挥作用的设置：它开始时初始化一个状态-动作表，并根据代理在某个状态下的动作是否朝着获胜方向进行更新。它的伟大之处在于我们**不需要制定任何策略来帮助我们的代理获胜**。如果我们做对了，代理可以**自行学习实现目标的最佳方式**，这就是魔法所在。

## 小心你的愿望

有时奖励某个中间动作可能会很诱人，即使它并没有赢得游戏，因为你认为这样做是有好处的。要注意，代理可能会利用这一点，做出你未曾想到的行为。例如，在冻结湖中，你可以在代理接近目标时给予奖励。

![](img/100ef1b26bc2b34ba06d95eaedd20623.png)

目标之前的一个中间奖励。图像由作者提供。

如果你这样做的话，一个理性的代理会怎么做？让我们来看看。

![](img/0334531f9139092781af32bb48057fc4.png)

200 IQ。图像由作者提供。

它仅在当前之前踩到+1 的字段，然后在永恒中一直踩下去。像这样滥用系统，代理获得了无限数量的奖励（总是+1），而踩到当前——我们实际上想要的——只用 2 分就结束了游戏。

## 连续游戏

我们还没有讨论具有连续状态或动作空间的游戏。为无限多的状态或动作制作表格似乎……具有挑战性。因此我们需要其他方法。一种处理方法是**离散化**这些空间。例如，如果你的观测空间由 0 到 1 之间的实数组成，那么你可以将这个区间分成 10 个桶，即：

+   桶 0: [0, 0.1),

+   桶 1: [0.1, 0.2),

+   …

+   桶 9: [0.9, 1.0]。

你的 Q 表将会有 10 行用于观测，并且在训练过程中，你总是需要将像 0.92341 这样的观测映射到桶编号 9。

然而，你的离散化越细，Q 表就会变得越大，直到某个点你无法再处理它的内存。

Q 学习的另一种变体是尝试用**神经网络**替代 Q 表，将状态映射到多个输出，每个动作对应一个输出。我们称这种方法为**深度 Q 学习**。

![](img/96a386ac8b010daa9a4dcf5c92c792d5.png)

图像由作者提供。

你可以在我的另一篇文章中阅读相关内容：

[](/hands-on-deep-q-learning-9073040ce841?source=post_page-----1f1e249f8fa5--------------------------------) ## 深入实践 Q 学习

### 提升你的代理，赢得更困难的游戏！

towardsdatascience.com

希望你今天学到了一些新的、有趣的和有价值的东西。感谢阅读！

> 如果你有任何问题，请通过[LinkedIn](https://www.linkedin.com/in/dr-robert-k%C3%BCbler-983859150/)联系我！

如果你想更深入地了解算法的世界，可以试试我的新出版物《全方位算法》。我还在寻找作者！

[](https://allaboutalgorithms.com/?source=post_page-----1f1e249f8fa5--------------------------------) [## 全方位算法

### 从直观的解释到深入的分析，算法通过示例、代码和精彩的内容变得生动起来……

[allaboutalgorithms.com](https://allaboutalgorithms.com/?source=post_page-----1f1e249f8fa5--------------------------------)
