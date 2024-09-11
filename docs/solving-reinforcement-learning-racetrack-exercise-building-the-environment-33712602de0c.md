# 使用离策略蒙特卡洛控制解决强化学习赛道练习

> 原文：[https://towardsdatascience.com/solving-reinforcement-learning-racetrack-exercise-building-the-environment-33712602de0c?source=collection_archive---------0-----------------------#2023-07-23](https://towardsdatascience.com/solving-reinforcement-learning-racetrack-exercise-building-the-environment-33712602de0c?source=collection_archive---------0-----------------------#2023-07-23)

[](https://medium.com/@outerrencedl?source=post_page-----33712602de0c--------------------------------)[![Tingsong Ou](../Images/459edc4bbd2353895acfb0f57eeddaa3.png)](https://medium.com/@outerrencedl?source=post_page-----33712602de0c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----33712602de0c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----33712602de0c--------------------------------) [Tingsong Ou](https://medium.com/@outerrencedl?source=post_page-----33712602de0c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa7aefc686327&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-reinforcement-learning-racetrack-exercise-building-the-environment-33712602de0c&user=Tingsong+Ou&userId=a7aefc686327&source=post_page-a7aefc686327----33712602de0c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----33712602de0c--------------------------------) ·10 min read·2023年7月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F33712602de0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-reinforcement-learning-racetrack-exercise-building-the-environment-33712602de0c&user=Tingsong+Ou&userId=a7aefc686327&source=-----33712602de0c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F33712602de0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsolving-reinforcement-learning-racetrack-exercise-building-the-environment-33712602de0c&source=-----33712602de0c---------------------bookmark_footer-----------)![](../Images/9a54dfbfe19b0a23290550dc0cad6d9e.png)

图片由 Midjourney 使用付费订阅生成，符合一般商业条款 [1]。

# 介绍

在书籍 *强化学习：导论 第二版（第112页）* 的 *离策略蒙特卡洛控制* 一节中，作者给我们留下了一个有趣的练习：使用加权重要性采样离策略蒙特卡洛方法找到在两条赛道上最快的行驶方式。这个练习非常全面，要求我们考虑和构建强化学习任务的几乎所有组件，如环境、代理、奖励、动作、终止条件和算法。解决这个练习很有趣，并帮助我们建立对算法与环境之间互动的扎实理解，正确的 episodic 任务定义的重要性，以及价值初始化如何影响训练结果。通过这篇文章，我希望与所有对强化学习感兴趣的人分享我对这个练习的理解和解决方案。

# 练习要求

如上所述，这个练习要求我们找到一种策略，使赛车从起始线尽可能快速地驶向终点线，而不撞到碎石或驶出赛道。在仔细阅读练习描述后，我列出了完成这项任务的关键点：

+   **地图表示**：在此背景下，地图实际上是以 (row_index, column_index) 为坐标的二维矩阵。每个单元格的值表示该单元格的状态；例如，我们可以用 0 来描述碎石，用 1 来表示赛道表面，用 0.4 表示起始区域，用 0.8 表示终点线。任何超出矩阵的行列索引都可以视为越界。

+   **汽车表示**：我们可以直接使用矩阵的坐标来表示汽车的位置；

+   **速度和控制**：速度空间是离散的，包括水平和垂直速度，可以表示为一个元组 (row_speed, col_speed)。两个轴上的速度限制为 (-5, 5)，并且每个轴在每一步递增 +1、0 或 -1；因此，每一步有总共 9 种可能的动作。字面上，除了起始线外，速度不能同时为零，垂直速度（或行速度）不能为负数，因为我们不希望汽车回到起始线。

+   **奖励和回合**：在穿越终点线之前的每一步奖励为 -1。当汽车驶出赛道时，它会被重置到起始单元格之一。回合 **仅在** 汽车成功穿越终点线时结束。

+   **起始状态**：我们从起始线随机选择起始单元格；根据练习的描述，汽车的初始速度为 (0, 0)。

+   **零加速度挑战**：作者提出了一个小的*零加速度挑战*，即在每个时间步骤中，以 0.1 的概率，动作将不起作用，汽车将保持之前的速度。我们可以在训练中实现这个挑战，而不是将其添加到环境中。

练习的解决方案分为两篇文章；在这篇文章中，我们将重点讨论如何构建赛道环境。此练习的文件结构如下：

```py
|-- race_track_env
|  |-- maps
|  |  |-- build_tracks.py // this file is used to generate track maps
|  |  |-- track_a.npy // track a data
|  |  |-- track_b.npy // track b data
|  |-- race_track.py // race track environment
|-- exercise_5_12_racetrack.py // the solution to this exercise
```

此实现中使用的库如下：

```py
python==3.9.16
numpy==1.24.3
matplotlib==3.7.1
pygame==2.5.0
```

# 构建赛道地图

我们可以将赛道地图表示为2D矩阵，不同的值表示赛道状态。我希望忠于练习，所以我正尝试通过手动分配矩阵值来构建书中展示的相同地图。这些地图将作为单独的*.npy*文件保存，以便环境在训练时可以读取文件，而不是在运行时生成它们。

这两个地图如下所示；浅色单元格是碎石，深色单元格是赛道表面，绿色单元格表示终点线，浅蓝色单元格表示起始线。

![](../Images/a014b029de744b7f65c750a5ad033e42.png)

图1 2D矩阵表示的赛道地图。来源：作者提供的图像

# 构建类似Gym的环境

赛道地图准备好后，我们将创建一个类似于gym的环境，算法可以与之互动。 [gymnasium](https://gymnasium.farama.org/)，之前是OpenAI gym，现在属于*Farama Foundation*，提供了一个用于测试RL算法的简单接口；我们将使用*gymnasium*包来创建赛道环境。我们的环境将包括以下组件/功能：

+   **env.nS**：观察空间的形状，在这个例子中为(num_rows, num_cols, row_speed, col_speed)。行数和列数在地图之间有所不同，但速度空间在所有赛道中是一致的。对于行速度，由于我们不希望汽车回到起点，行速度的观察值为[-4, -3, -2, -1, 0]（负值因为我们期望汽车在地图上向上移动），总共有五个空间；列速度没有这样的限制，所以观察值范围从-4到4，总共有九个空间。因此，赛道示例中的观察空间形状为(num_rows, num_cols, 5, 9)。

+   **env.nA**：可能的行动数量。在我们的实现中有9种可能的行动；我们还将在环境类中创建一个字典，将整数行动映射到(row_speed, col_speed)元组表示，以控制智能体。

+   **env.reset()**：当回合结束或汽车驶出赛道时，该函数将汽车带回起始单元格之一；

+   **env.step(action)**：步进函数使算法通过采取行动与环境进行交互，并返回一个包含(next_state, reward, is_terminated, is_truncated)的元组。

+   状态检查函数：有两个私有函数帮助检查汽车是否离开赛道或越过终点线；

+   渲染函数：在我看来，渲染函数对定制环境至关重要；我总是通过渲染游戏空间和智能体的行为来检查环境是否已正确构建，这比单纯查看日志信息更直接。

牢记这些特点后，我们可以开始构建赛车道环境。

# 检查环境

到目前为止，所有赛车道环境所需的准备工作都已就绪，我们可以测试/验证我们的环境设置。

首先，我们渲染游戏以检查每个组件是否正常工作：

![](../Images/a9a2a779ce71a61e4ee17c2ed7f0eecc.png)

图2 采用随机策略在两个轨道上行驶的智能体。来源：作者制作的动图

然后我们关闭渲染选项，让环境在后台运行，以检查轨迹是否正确终止：

```py
// Track a
|   Observation   | reward | Terminated | Total reward |
|  (1, 16, 0, 3)  |   -1   |    True    |    -4984     |
|  (2, 16, 0, 2)  |   -1   |    True    |    -23376    |
|  (3, 16, 0, 3)  |   -1   |    True    |    -14101    |
|  (1, 16, 0, 3)  |   -1   |    True    |    -46467    |

// Track b
|   Observation    | reward | Terminated | Total reward |
|  (1, 31, -2, 2)  |   -1   |    True    |    -3567     |
|  (0, 31, -4, 4)  |   -1   |    True    |    -682      |
|  (2, 31, -2, 1)  |   -1   |    True    |    -1387     |
|  (3, 31, -1, 3)  |   -1   |    True    |    -2336     |
```

# 离策略蒙特卡罗控制算法

在环境准备好后，我们可以准备实施带加权重要性采样的离策略MC控制算法，如下所示：

![](../Images/6b96023d481220e61c1b1e7f0710b1c3.png)

图3 离策略蒙特卡罗控制算法。来源：[latex](https://gist.github.com/terrence-ou/67aad132f41975c1cb3aad460a03ac88) 由作者编写。

蒙特卡罗方法通过对样本回报进行平均来解决强化学习问题。在训练中，该方法基于给定策略生成轨迹，并从每个回合的尾部学习。在线策略和离策略学习之间的区别在于，在线策略方法使用一种策略来做决策和改进，而离策略方法使用不同的策略来生成数据和策略改进。根据书籍作者的说法，几乎所有的离策略方法都利用重要性采样来估计在一个分布下的期望值，给定来自另一个分布的样本。[2]

接下来的几个部分将解释上面算法中出现的软策略和加权重要性采样技巧，以及适当的值初始化对该任务的关键性。

# 目标策略和行为策略

本算法的离策略方法使用了两种策略：

+   **目标策略**：正在学习的策略。在本算法中，目标策略是贪婪的和确定性的，这意味着在时间*t*，贪婪动作*A*的概率是1.0，其它所有动作的概率为0.0。目标策略遵循每步改进的广义策略迭代（GPI）。

+   **行为策略**：用于生成行为的策略。本算法中的行为策略被定义为*软策略*，意味着在给定状态下所有动作都有非零概率。我们将在这里使用ε-贪婪策略作为我们的行为策略。

在软策略中，某个动作的概率是：

![](../Images/ea7d0d6e471fc982b4b4730148a83983.png)

我们在为重要性采样生成动作时也应返回这个概率。行为策略的代码如下所示：

# 加权重要性采样

我们估计由目标策略生成的轨迹在行为策略下的相对概率，这样的概率方程是：

![](../Images/bd966b843cf9d67c29ecb0ebd67d78ce.png)

加权重要性采样的值函数是：

![](../Images/1803ba37915eb930a53a4fce6d1ef363.png)

我们可以将方程重新调整，以适应增量更新形式的情景任务：

![](../Images/8ea43a0e47e279128a1b6e4c489e6043.png)

有很多优秀的例子展示了如何推导上述方程，因此我们在这里不再推导。但我也想解释算法最后几行的有趣技巧：

![](../Images/1c3e3d57e2be46a0aeecf06b0587173e.png)

图4 离策略蒙特卡罗控制算法中的技巧。来源：作者提供的图片

这个技巧基于目标策略在这里是确定性的设置。如果某个时间步采取的动作与目标策略的动作不同，那么该动作相对于目标策略的概率为0.0；因此，所有后续步骤将不再更新轨迹的状态-动作值函数。相反，如果那个时间步的动作与目标策略相同，则概率为1.0，动作值更新继续进行。

# 权重初始化

合适的权重初始化在成功解决此练习中发挥着重要作用。我们首先来看随机策略在两个轨道上的奖励：

```py
// Track a
|   Observation   | reward | Terminated | Total reward |
|  (1, 16, 0, 3)  |   -1   |    True    |    -4984     |
|  (2, 16, 0, 2)  |   -1   |    True    |    -23376    |
|  (3, 16, 0, 3)  |   -1   |    True    |    -14101    |
|  (1, 16, 0, 3)  |   -1   |    True    |    -46467    |

// Track b
|   Observation    | reward | Terminated | Total reward |
|  (1, 31, -2, 2)  |   -1   |    True    |     -3567    |
|  (0, 31, -4, 4)  |   -1   |    True    |     -682     |
|  (2, 31, -2, 1)  |   -1   |    True    |     -1387    |
|  (3, 31, -1, 3)  |   -1   |    True    |     -2336    |
```

在穿越终点线之前，每一步的奖励是-1。训练的早期阶段，奖励将是较大的负值；如果我们将状态-动作值初始化为0或从均值为0、方差为1的正态分布中随机值，则该值可能被认为过于乐观，这可能需要很长时间程序才能找到最佳策略。

通过几次测试，我发现均值为-500、方差为1的正态分布可能是更快收敛的有效值；你可以尝试其他值，绝对能找到比这更好的初始值。

# 解决问题

牢记上述所有思想，我们可以编写离策略蒙特卡罗控制函数，并用它来解决赛道练习。我们还将添加练习描述中提到的零加速度挑战。

然后我们在两个轨道上训练政策1,000,000个回合，包含/不包含零加速度挑战，并用以下代码进行评估：

通过绘制训练记录，我们发现策略在不考虑零加速度的情况下在两个轨道上都在第10,000集左右收敛；添加零加速度使得收敛变得更慢，并且在第100,000集之前不稳定，但之后仍然收敛。

![](../Images/6918a1b931aaa5fcb033677405aef3c0.png)

图5 轨道A的训练奖励历史。来源：作者提供的图像

![](../Images/3ca458fa857f9d38822990255cd46b5a.png)

图6 轨道B的训练奖励历史。来源：作者提供的图像

# 结果

最后，我们可以让智能体使用训练后的策略进行游戏，同时绘制出可能的轨迹以进一步检查策略的正确性。

![](../Images/e1ade388e7ec2b717df36aeb436acef9.png)

图7 智能体基于训练后的策略在两个轨道上行驶。来源：作者提供的GIF

轨道a的样本轨迹：

![](../Images/a65edebe8370063de691a667a4a6651b.png)

图8 轨道A的样本最优轨迹。来源：作者提供的图像

轨道b的样本轨迹：

![](../Images/4771002e1893aaab88bbe7483a4eec01.png)

图9 轨道B的样本最优轨迹。来源：作者提供的图像

到目前为止，我们已经解决了赛道练习。这种实现可能仍然存在一些问题，非常欢迎您指出这些问题并在评论中讨论更好的解决方案。感谢阅读！

# 参考

[1] Midjourney 服务条款: [https://docs.midjourney.com/docs/terms-of-service](https://docs.midjourney.com/docs/terms-of-service)

[2] Sutton, Richard S., 和 Andrew G. Barto. *强化学习：导论*。麻省理工学院出版社，2018年。

本练习的GitHub仓库: [[链接]](https://github.com/terrence-ou/Reinforcement-Learning-2nd-Edition-Notes-Codes/tree/main/chapter_05_monte_carlo_methods)；请查看“练习部分”。
