# Sb3，应用 RL 的瑞士军刀

> 原文：[`towardsdatascience.com/sb3-the-swiss-army-knife-of-applied-rl-5548535d09cd`](https://towardsdatascience.com/sb3-the-swiss-army-knife-of-applied-rl-5548535d09cd)

## 你的模型选择，适用于任何环境

[](https://medium.com/@byjameskoh?source=post_page-----5548535d09cd--------------------------------)![詹姆斯·科，博士](https://medium.com/@byjameskoh?source=post_page-----5548535d09cd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5548535d09cd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5548535d09cd--------------------------------) [詹姆斯·科，博士](https://medium.com/@byjameskoh?source=post_page-----5548535d09cd--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5548535d09cd--------------------------------) ·8 分钟阅读·2023 年 10 月 26 日

--

![](img/3359413fa03ba1b4537646411e4dea66.png)

图片由 DALL·E 3 根据提示“创建一个现实主义风格的打开的瑞士军刀图像”生成。

Stablebaseline3 (sb3) 就像是一把瑞士军刀。它是一种多功能工具，可以用于许多目的。而且，就像瑞士军刀在你被困在丛林中时可以救命一样，sb3 可以在你在办公室中遇到看似不可能的截止日期时救你一命。

> 本指南使用 gymnasium=0.28.1 和 stable-baselines=2.1.0。如果你使用不同的版本，或许还参考了其他旧指南，可能不会得到下面的结果。但不要担心，这里也提供了安装指南。我保证只要按照我的说明操作，你就能获得结果。

# [1] 你将获得什么

Stablebaseline3 使用起来很简单。它也有很好的文档支持，你可以自行跟随教程。但…

+   你是否参考过旧的指南（可能是使用 `gym` 的指南），结果发现你的机器上存在错误？

+   你能始终确保兼容性吗？

+   如果你想使用 `gymnasium` 的环境并修改奖励，该怎么办？

+   你知道如何包装自己的任务，以便可以在几行代码中应用 SOTA 模型吗？

这就是本文的目标！在阅读了这篇指南之后，你将…

1.  使用 sb3 模型解决经典环境，视觉化结果，并在几行代码中保存（或加载）训练好的模型。**[第 3.1 节]**

1.  理解如何检查动作空间和观测空间的兼容性。**[第 3.2 节]**

1.  学习如何包装 `gymnasium` 环境，以便可以使用任何 sb3 模型，而不会对 `box` 或 `discrete` 有任何限制。**[第 4.1 节]**

1.  学会如何包装 `gymnasium` 环境以进行奖励塑形。**[第 4.2 节]**

1.  了解如何将自定义环境包装为与 sb3 兼容，同时对原始代码进行最小更改，原始代码可能遵循不同的结构。**[第五部分]**

# [2] 安装

创建一个虚拟环境并设置相关依赖。我主要针对的是大多数人——这里的指南是在 Windows 系统上创建的，并且已经安装了 Anaconda。打开你的 Anaconda 提示符并执行以下操作：

```py
conda create --name rl python=3.8
conda activate rl

conda install gymnasium[box2d]
pip install stable-baselines3==2.1.0
pip install pygame==2.5.2
pip install imageio==2.31.6

conda install jupyter
jupyter notebook
```

在这里，我们将使用 jupyter notebook，因为它是一个更用户友好的教学工具。

# [3] 成功的初步体验 — 查看你的训练 RL 代理

首先要导入所需的库。

```py
import os
import numpy as np
import gymnasium as gym   # 0.28.1
import stable_baselines3  # 2.1.0
from stable_baselines3 import A2C, DDPG, DQN, PPO, SAC, TD3
from stable_baselines3.common.callbacks import EvalCallback
from stable_baselines3.common.evaluation import evaluate_policy
```

## [3.1] Cartpole 上的 DQN

我们从小的例子开始，比如 Cartpole 任务，目标是推动小车（向左或向右）以保持杆子直立。

你绝对需要的最低限度是什么？就是这个，用于训练。

```py
env = gym.make("CartPole-v1")
model = DQN("MlpPolicy", env)
model.learn(total_timesteps=100000)
```

还有这个，用于评估。

```py
mean_reward, std_reward = evaluate_policy(model, env, n_eval_episodes=10)
print(f"Mean reward: {mean_reward} +/- {std_reward}")
```

最后，这样做是为了可视化。

```py
import pygame
env = gym.make("CartPole-v1", render_mode="human")

obs = env.reset()[0]
score = 0 
while True:
    action, states = model.predict(obs)
    obs, rewards, done, terminate, info = env.step(action)
    score += rewards
    env.render()
    if terminate: 
        break
print("score: ", score)
env.close()
```

只需 10 行以上的代码和几秒钟时间，我们就解决了一个经典的 RL 问题。这是 AI 已经被民主化到何种程度的一个好例子！

使用上面完全相同的代码训练并可视化的代理。图片由作者提供。

要保存你的 sb3 模型，只需在训练执行期间添加一个回调。

```py
env = gym.make("CartPole-v1")
model = DQN("MlpPolicy", env)
model.learn(
    total_timesteps=100000,
    callback=EvalCallback(
        env, best_model_save_path='./logs/', eval_freq=5000
    )
)
```

你的模型随后可以用两行代码加载。

```py
model = DQN.load("./logs/best_model.zip")
model.set_env(env)
```

## [3.2] 检查动作/观察空间

假设我们尝试不同的模型，比如使用 `model=SAC("MlpPolicy", env)`。这将导致一个错误。

这是因为 SAC（Soft Actor Critic）仅适用于连续动作空间，如官方 Stable Baselines3 [文档](https://stable-baselines3.readthedocs.io/en/master/modules/sac.html) 中所述，而 Cartpole 环境具有离散动作空间。

我将动作空间约束汇编成一个简单的函数如下：

```py
def is_compatible(env, model_name):
    action_requirements = {
        'A2C':  [gym.spaces.Box, gym.spaces.Discrete],
        'DDPG': [gym.spaces.Box],
        'DQN':  [gym.spaces.Discrete],
        'PPO':  [gym.spaces.Box, gym.spaces.Discrete],
        'SAC':  [gym.spaces.Box],
        'TD3':  [gym.spaces.Box],
    }
    return isinstance(env.action_space, tuple(action_requirements[model_name]))
```

这样，`is_compatible(env,'DQN')` 返回 `True`，而 `is_compatible(env,'SAC')` 返回 `False`。

对于 sb3 中的任何模型，观察空间没有约束。

# [4] 包装 `gymnasium` 环境

如果我们想根据自己的规格修改 `gymnasium` 环境呢？我们应该从头编写代码？还是查看源代码并在那进行修改？

对这两个问题的回答是，**不**。

最好只是包装 `gymnasium` 对象。这样不仅快速简便，还使你的代码可读且可靠。

人们不需要逐行审查你的代码。他们只需查看你包装器中的修改（假设他们对 `gymnasium` 的正确性感到信服）。

## [4.1] 不考虑 `box` 或 `discrete`

在第 3.2 节中，我们看到 SAC 与 Cartpole 不兼容。

这是一个解决办法。实际上，任何 sb3 模型都可以用于任何环境；我们只需要一个简单的包装器。

```py
class EnvWrapper(gym.ActionWrapper):
    def __init__(self, env, conversion='Box'):
        super().__init__(env)
        self.conversion = conversion
        if conversion == 'Box':
            self.action_space = gym.spaces.Box(
                low=np.array([-1]), high=np.array([1]), dtype=np.float32
            )
        elif conversion == 'Discrete':
            self.num_actions = 9
            self.action_space = gym.spaces.Discrete(
                self.num_actions
            )
        else:
            pass

    def action(self, action):
        if self.conversion == 'Box':
            # Takes a Continuous action from the model and convert it to discrete for a natively Discrete Env
            if action.shape == (1,):
                action = np.round((action[0] + 1) / 2).astype(int)  # convert from scale of [-1, 1] to the set {0, 1}
            else:
                action = np.round((action + 1) / 2).astype(int)
        elif self.conversion == 'Discrete':
            # Takes a Discrete action from the model and convert it to continuous for a natively Box Env
            action = (action / (self.num_actions - 1)) * 2.0 - 1.0
            action = np.array([action])

        return action
```

通过这样做，你可以使用像 SAC 这样的处理连续动作空间的模型来解决具有离散动作空间的环境。

```py
wrapped_env = EnvWrapper(env, 'Box')
model = SAC("MlpPolicy", wrapped_env)
model.learn(total_timesteps=10000)
```

任何 sb3 模型都可以与任何经典的 gymnasium 环境兼容。不要仅仅听我的话。试试以下内容。

```py
env_name_list = ['CartPole-v1', 'MountainCar-v0', 'Pendulum-v1', 'Acrobot-v1']
model_name_list = ['A2C', 'DDPG', 'DQN', 'PPO', 'SAC', 'TD3']

for env_name in env_name_list:
    for model_name in model_name_list:
        env = gym.make(env_name)
        if not is_compatible(env, model_name):
            # Environment and model are not compatible. Will wrap env to suit to model
            if isinstance(env.action_space, gym.spaces.Box):
                env = EnvWrapper(env, 'Discrete')
                print("Box Environment warpped to be compatible with Discrete model...")
            else:
                env = EnvWrapper(env, 'Box')
                print("Discrete Environment warpped to be compatible with Continuous model")
        else:
            print("Already compatible")

        model = eval("%s(\"MlpPolicy\", env, verbose=False)" % model_name)
        print("Using %s in %s. The model's action space is %s" % (model_name, env_name, model.action_space))

        model.learn(total_timesteps=100)  # just for testing
```

请注意，这里的目的是*展示*环境可以被包装成兼容的形式。性能可能不是理想的，但这不是重点。

关键是要向你展示，如果你理解 sb3 如何与 gymnasium 配合使用，你能够将任何东西包装成通用兼容的形式。

## [4.2] 奖励塑形

假设我们想修改一个 gymnasium 环境，以尝试奖励塑形。例如，你可能已经玩过[Lunar Lander](https://gymnasium.farama.org/environments/box2d/lunar_lander/)，并观察到一个用默认超参数训练的智能体可能会悬停在顶部，以避免碰撞的风险。

Lunar Lander 在顶部悬停。图片由作者提供。

在这种情况下，我们可以对智能体持续停留在顶部时施加惩罚。

```py
class LunarWrapper(gym.Wrapper):
    def __init__(self, env, max_top_time=100, penalty=-1):
        super().__init__(env)
        self.max_top_time = max_top_time  # penalty kicks in after this step
        self.penalty = penalty            # additional reward (or penalty if negative) after max_top_time
        self.penalty_start_step = 20000
        self.step_counter = 0

    def reset(self, **kwargs):
        self.time_at_top = 0
        return super().reset(**kwargs)

    def step(self, action):
        obs, reward, done, terminate, info = super().step(action)
        self.step_counter += 1

        y_position = obs[1]
        if y_position > 0.5:
            self.time_at_top += 1
        else:
            self.time_at_top = 0  # Reset counter if it comes down

        # Apply penalty if the lander stays at the top for too long
        if self.time_at_top >= self.max_top_time:
            if (self.step_counter >= self.penalty_start_step):
                reward += (-y_position)    # top of the screen is 1\. To incur more penalty when it is high

        return obs, reward, done, terminate, info
```

请记住，在用伪奖励进行训练后，智能体应使用实际环境和原始奖励进行微调。

```py
env_name = "LunarLander-v2"
wrapped_env = LunarWrapper(gym.make(env_name))

model = DQN(
    "MlpPolicy", wrapped_env,
    buffer_size=50000, learning_starts=1000, train_freq=4, target_update_interval=1000, 
    learning_rate=1e-3, gamma=0.99
)

model.learn(
    total_timesteps=50000,
    callback=EvalCallback(
        wrapped_env, best_model_save_path='./logs/', log_path='./logs/', eval_freq=2000
    )
)

model = DQN.load("./logs/best_model.zip")
model.set_env(env)

model.learn(
    total_timesteps=20000,
    callback=EvalCallback(
        env, best_model_save_path='./logs/', eval_freq=2000
    )
)
```

通过奖励塑形训练的智能体解决了 Lunar Lander。图片由作者提供。

这看起来好多了！

# [5] 自定义任务的包装器

在这一最终部分，我将实现我的第 5 个承诺——*学习如何将自定义环境包装成与 sb3 兼容，同时对原始代码做最小的修改，原始代码可能遵循不同的结构*。

作为学习者，我们训练 RL 智能体解决知名的基准问题。然而，行业支付你的是解决实际问题，而不是玩具问题。如果你因为 RL 专长而被雇佣，你很可能需要解决对公司而言独特的问题。

然而，sb3 和 gymnasium 仍然是你的好朋友！

为了说明问题，让我们考虑以下简单的 GridWorld。

```py
class SimpleEnv:
    def __init__(self):
        self.min_row, self.max_row = 0, 4
        self.min_col, self.max_col = 0, 4
        self.terminal = [[self.max_row, self.max_col]]
        self.reset()

    def reset(self, random=False):
        if random:
            while True:
                self.cur_state = [np.random.randint(self.max_row + 1), np.random.randint(self.max_col + 1)]
                if self.cur_state not in self.terminal:
                    break
        else:
            self.cur_state = [0,0]
        return self.cur_state

    def transition(self, state, action):
        reward = 0
        if action == 0:
            state[1] += 1   # move right one column
        elif action == 1:
            state[0] += 1   # move down one row
        elif action == 2:
            state[1] -= 1   # move left one column
        elif action == 3:
            state[0] -= 1   # move up one row
        else:
            assert False, "Invalid action"

        if (state[0] < self.min_row) or (state[1] < self.min_col) \
            or (state[0] > self.max_row) or (state[1] > self.max_col):
            reward = -1

        next_state = np.clip(
            state, [self.min_row, self.min_col], [self.max_row, self.max_col]
        ).tolist()
        if next_state in self.terminal:
            done = True
        else:
            done = False 
        return reward, next_state, done

    def _get_action_dim(self):
        return 4

    def _get_state_dim(self):
        return np.array([5,5])
```

请注意，这里的`transition`方法返回`reward`、`next_state`和`done`。Stable baselines3 将不接受这种风格。

你需要重新编写你的环境吗？不需要！

相反，我们构建了一个简单的包装器。

```py
from gymnasium import spaces

class CustomEnv(gym.Env):
    def __init__(self, **kwargs):
        super().__init__()
        self.internal_env = SimpleEnv(**kwargs)
        self.action_space = spaces.Discrete(self.internal_env._get_action_dim())
        self.observation_space = spaces.MultiDiscrete(self.internal_env._get_state_dim())

    def step(self, action):
        reward, next_state, done = self.internal_env.transition(self.internal_env.cur_state, action)
        self.count += 1
        terminate = self.count > 50
        if terminate:
            reward += -100
        return np.array(next_state), reward, done, terminate, {}

    def reset(self, random=True, **kwargs):
        self.count = 0
        return (np.array(self.internal_env.reset(random=random)), {})

    def render(self, mode="human"):
        pass

    def close(self):
        pass
```

在上面，我们定义了一个`step`方法，它包裹了原始环境的`transition`，并返回 sb3 期望的内容。

与此同时，我利用这个机会展示了我们可以在不解剖原始环境的情况下进行修改。在这里，`CustomEnv`如果目标在 50 步内未达成，则终止回合（并施加大惩罚）。

我们怎么知道环境是否正确包装了呢？首先，它必须通过以下基本检查。

```py
from stable_baselines3.common.env_checker import check_env

env = CustomEnv()
check_env(env, warn=True)

obs = env.reset()
action = env.action_space.sample()
print("Sampled action:", action)

obs, reward, done, terminate, info = env.step(action)
print(obs.shape, reward, done, info)
```

接下来，我们可以使用 sb3 模型在包装后的环境上进行训练。你还可以在这里调整超参数，如下所示。

```py
model = DQN(
    "MlpPolicy", env,
    learning_rate=1e-5,
    exploration_fraction=0.5,
    exploration_initial_eps=1.0,
    exploration_final_eps=0.10,
)
model.learn(
    total_timesteps=100000,
    callback=EvalCallback(
        env, best_model_save_path='./logs/', eval_freq=10000
    )
)
```

# 结论

在这篇文章中，你已经学习了如何设置自己的环境以运行 sb3 和 gymnasium。你现在有能力在***任何***你选择的环境中实现最先进的 RL 算法。

享受吧！
