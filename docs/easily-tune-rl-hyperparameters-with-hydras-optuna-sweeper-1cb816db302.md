# 使用 Hydra 的 Optuna 调优器来调节 RL 超参数

> 原文：[`towardsdatascience.com/easily-tune-rl-hyperparameters-with-hydras-optuna-sweeper-1cb816db302`](https://towardsdatascience.com/easily-tune-rl-hyperparameters-with-hydras-optuna-sweeper-1cb816db302)

## 使用 Hydra 和 Optuna 轻松配置你的 Stable-Baselines3 调优流程

[](https://marc-velay.medium.com/?source=post_page-----1cb816db302--------------------------------)![Marc Velay](https://marc-velay.medium.com/?source=post_page-----1cb816db302--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cb816db302--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cb816db302--------------------------------) [Marc Velay](https://marc-velay.medium.com/?source=post_page-----1cb816db302--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cb816db302--------------------------------) ·阅读时间 8 分钟 ·2023 年 2 月 1 日

--

强化学习 (RL) 代理对其超参数非常敏感。相同的代理在训练后可能完全无用，但使用正确的超参数后却能登上排行榜的顶端！然而，找到正确的组合可能非常耗时，甚至在没有合适工具的情况下徒劳无功。本文的主要目标是通过一些理论和应用与您分享 *如何* 调节您的 RL 超参数。

你可以在我的 [GitHub](https://github.com/Marc-Velay/hydra_optuna_tutorial) 上获取此项目的代码：快来 fork 吧！

![](img/287f7595e51b7ca3130686d9eb26f4db.png)

照片由 [Leonel Fernandez](https://unsplash.com/@leonelfdez?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

# 介绍

强化学习是一种机器学习类型，涉及训练代理与环境进行互动。代理执行一系列动作，遵循策略来决定在每种情况下如何反应。它们旨在通过与环境互动来最大化它们累积的奖励，并通过经验慢慢学习最佳动作。在 [上一篇文章](https://velaylearning.com/markov-decision-process-in-plain-english/) 中，我更深入地探讨了强化学习的概念。

RL 算法依赖于许多超参数，一些用于学习，一些用于基础神经网络。有许多杠杆可以使学习更稳定、更快，或者节省内存。仅查看稳定基线 3 的 [SAC 实现](https://stable-baselines3.readthedocs.io/en/master/modules/sac.html)，他们有 25 个参数，其中大多数取决于你的使用案例，并有助于优化策略的成功。

这个难题吸引了很多关注，最近来自 [Google 的研究团队](https://github.com/google-research/tuning_playbook) 的热门答案对此进行了探讨。

搜索空间的广度要求你系统地遍历超参数的组合，以找到优化代理性能的最佳组合。这通过使用一组超参数训练代理，并评估从验证回合中获得的平均奖励来完成。扫频需要知道所有参数的有效范围或值，创建代理及其模型，然后进行足够的训练，以查看学习是否收敛。

![](img/77aeb4319101b87d3c473b267d54189d.png)

两种超参数组合导致截然不同的结果，作者提供了图像。

由于设置如此复杂，你不能在 Python 代码中硬编码参数。你需要一个模块化框架来处理配置，并且最好能抽象出整个过程。

# 引入 Hydra

[Hydra](https://hydra.cc/) 是 Facebook Research 团队设计的一个库，用于管理复杂的应用程序，包括机器学习管道。它在配置灵活性和模块化方面表现出色。通过设计，你可以分离不同的组件，并从 yaml 配置文件中替换它们。这使得实验不同的学习算法、模型、设置和环境变得更容易。将我的设置存储在静态配置文件中意味着我可以与团队共享，它们可以在相同条件下重复实验。

Hydra 使用命令行选项、层级配置文件和一些代码的组合来管理你的管道组件。

```py
pip3 install hydra-core
```

```py
python train_agent.py agent=ppo environment=pendulum_default
```

尽管命令行可以深入到很多 [细节](https://hydra.cc/docs/advanced/hydra-command-line-flags/) 并具有广泛的范围，但我会集中讨论几个想法。上述命令类似于启动常规的 ML 训练脚本，但有更多的参数。我可以从命令行指定要使用哪些配置文件。这些参数会在主函数中捕获。

```py
@hydra.main(version_base="1.2", config_path='./configs', config_name='default')
def train(cfg: DictConfig):
  reward = run_training(cfg)
  return reward

if __name__ == "__main__":
  train() # Note the lack of parameters
```

我假设在我的项目根目录下有一个由 Hydra 装饰器指定的 "configs" 文件夹。它包含描述我的实验的 yaml 文件（default.yaml）以及用于代理和环境配置的文件夹。这些层级文件从根目录调用，并可以作为属性传递：在 default.yaml 中，所需的代理被传递为 "agent: ddpg"，ddpg.yaml 的内容作为字典添加到运行的配置中。

```py
configs/
├── default.yaml
├── agent
│   ├── ddpg.yaml
│   ├── ppo.yaml
│   └── search_spaces
│       ├── ddpg.yaml
│       └── ppo.yaml
└── environment
    ├── pendulum_default.yaml
    └── share_market_default.yaml
```

运行实验的基本设置在主配置文件中，由装饰器调用，只包含指向其他 yaml 文件的链接，这些文件包含模型和环境值。

```py
defaults:
    - agent: ddpg
    - environment: pendulum_default
```

我们来看一下 ddpg.yaml 中的值：

```py
trained_agent_path: ''
policy: "MlpPolicy"
learning_algo: "ddpg"
gamma: 1.
learning_rate: 1e-3
batch_size: 32
buffer_size: 1e5
train_freq: 1
gradient_steps: 8
tau: 0.005
noise_type: "normal"
noise_std: 0.001
net_arch: "m"
activation: "relu"
```

我们在代码中通过 "cfg.agent.*" 访问这些值。我们可以使用这些超参数训练我们的模型，并做一些小的设置将其转换为 Stable-Baselines3 (SB3) 所期望的参数。

参数是简单的浮点数和字符串，而 SB3 期望的是类或实例化的对象。我们使用一些字典来解决这种转换。以模型的激活函数和噪声函数为例：

```py
from torch import nn
from stable_baselines3.common.noise import NormalActionNoise

activation_fn = {"tanh": nn.Tanh, "relu": nn.ReLU, "elu": nn.ELU, "leaky_relu": nn.LeakyReLU}[cfg.agent.activation]

if cfg.agent.noise_type == "normal":
        hyperparams["action_noise"] = NormalActionNoise(
            mean=np.zeros(cfg.environment.n_actions), sigma=cfg.agent.noise_std * np.ones(cfg.environment.n_actions)
        )
```

拥有将离散选择转化为预期实现的模板代码，使我们能够准备一系列广泛的选项供选择。这使得从 Hydra 配置中直接进行超参数调整变得更加容易，特别是使用 hypra-optuna-sweeper 插件。

# Hydra 的 Optuna Sweeper

```py
pip3 install hydra-optuna-sweeper
```

Optuna 是一个出色的自动参数优化库。它依赖于非常少的组件：参数迭代和评估。Optuna 提供了一组值供测试，并期望输出一个分数，使用你的管道作为一个黑箱。

在底层，该库使用了一些技巧来建议它估计将有更好结果的参数，使你能够比基本的网格搜索节省时间。通常的方法需要将 Optuna 的试验直接实现到你的主要训练函数中，自行处理搜索空间、试验和修剪。通过 Hydra 的方法，大多数事情都是抽象的，只需调整一些参数即可。

Optuna 的一个关键特性是并行运行搜索，Hydra 对此进行了增强。这两个库密切配合：Optuna 选择要试验的超参数，而 Hydra 高效地设置你的管道。让我们看看如何在你的代码中结合这两个库。

主要的变化发生在你的 Hydra 配置文件 default.yaml 中：

```py
defaults:
    - agent: ddpg
    - environment: pendulum_default
    - agent/search_spaces@hydra.sweeper.params: ${agent}
    - _self_
    - override hydra/sweeper: optuna
    - override hydra/sweeper/sampler: tpe

hydra:
    sweeper:
        sampler:
            seed: 234
        direction: maximize
        study_name: pendulum_ddpg
        storage: null
        n_trials: 20
        n_jobs: 1
```

现在你需要在"agent/search_spaces/ddpg.yaml"中配置你的超参数搜索空间。

```py
agent.gamma: choice(0.5, 0.7, 0.9, 0.95, 0.99)
agent.learning_rate: tag(log, interval(1e-5, 1.))
agent.batch_size: choice(8, 16, 32, 64, 128)
agent.buffer_size: choice(1e4, 1e5, 1e6)
agent.tau: choice(0.0001, 0.001, 0.005, 0.01, 0.05, 0.1)
agent.train_freq: choice(1, 4, 8, 16, 32, 64)
agent.gradient_steps: choice(1, 4, 8, 16, 32, 64)
agent.noise_type: choice("ornstein-uhlenbeck", "normal")
agent.noise_std: interval(0.01,0.5)
agent.net_arch: choice("s","m","l")
agent.activation: choice("tanh", "relu")
```

让我们查看这两个配置文件。在主文件中，我们添加了指向 Optuna 可以为每个超参数进行调整的空间文件的链接。在我们的例子中，"${agent}"意味着我们的文件与上面定义的代理配置文件同名。

我们有几个选项可以指定我们希望探索的每个超参数的值范围。

```py
choice("ornstein-uhlenbeck", "normal")

interval(0.01,0.5)

tag(log, interval(1e-5, 1.))
```

选择允许我们从固定的值列表中选择离散选项。这对于文本或限制选项非常合适。

下一个范围说明符是经典的线性区间。一个随机值将在该区间内被选取。

最后，当从跨越多个数量级的区间中抽样时，使用对数分布是有趣的。这在 hydra-optuna-sweeper 插件中默认不包含，但你可以将其标记为如此。

你可以直接从[文档](https://hydra.cc/docs/1.2/plugins/optuna_sweeper/#search-space-configuration)中找到更多用例，以及一些示例。

下一步是指定我们希望 Hydra 使用哪个 sweeper。对于 Ax、Nevergrad 和 Optuna sweepers 有插件。然后我们需要为 Optuna 定义一些自定义参数，例如优化我们问题的最佳方向。在 RL 的情况下，我们通常希望最大化奖励。对于经典的机器学习问题，我们通常希望最小化误差。

我们探索的试验次数是我们希望测试的超参数总集合。在这种情况下，我们只运行 20 种组合进行快速 sweep，每次一个作业。来自 sweep 的超参数选择将覆盖 agent/ddpg.yaml 文件中指定的超参数。

![](img/f1dec45cbea8d63b2a603de07b39478a.png)

启动一个带有采样超参数的 Optuna 实验，由作者提供的图片

这结束了 Optuna 的第一个组件：参数迭代。现在，我们需要对其进行评估。为此，我们利用 Stable-Baseline3 的回调函数。我们定义了“TrialEvalCallback”，以每 N 步运行验证集，获取我们代理在特定时间的平均表现。

![](img/7a1ecfa1dd40406334e95ad36e505232.png)

训练期间的 TrialEvalCallback 输出，由作者提供的图片

一旦训练结束，我们将返回从 Hydra 主函数装饰的函数中获得的最佳平均表现。在 sweep 结束时，Hydra 输出具有最佳评估的超参数集。以下是对 pendulum 环境进行 DDPG sweep 时得到的最佳超参数集。

![](img/ade0cf23770160b1a8dfb5a6c026438f.png)

为 Pendulum 环境找到的最佳超参数，由作者提供的图片

作为额外的好处，SB3 配备了快速内置的 TensorBoard 实现。在 TB 仪表盘中，我们可以看到不同组合在训练阶段的相对表现。最佳超参数是从验证集的平均表现中选择的，但你可能对进一步的细节感兴趣。有些组合可能达到平台期，而其他则从好到坏波动。

![](img/75ae92ce145898cd8cbdd06fdc2a7724.png)

在 TensorBoard 中的多次运行比较，由作者提供的图片

总结来说，我们使用 Hydra 大大简化了设置 Stable-Baselines3 代理的过程，增加了灵活性，并能够直接从 yaml 文件中快速更改组件。额外的好处是你可以保存过去的实验并使用相同的超参数重新运行，或与团队分享。

虽然 Optuna 通常直接从你的主要训练脚本运行，但 hydra-optuna-sweeper 采取了抽象的方法。你可以从现有配置中启动超参数搜索，通过指定搜索空间和代理参数。Optuna 处理其余部分，你可以专注于你的用例。

希望你在使用 hydra-optuna-sweeper 时能玩得开心！你可以在我的 [GitHub](https://github.com/Marc-Velay/hydra_optuna_tutorial) 上找到整个项目。可以通过 [Twitter](https://twitter.com/marc_velay) 或 [LinkedIn](https://www.linkedin.com/in/marc-velay/) 联系我。
