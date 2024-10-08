# 超曲面深度强化学习

> 原文：[`towardsdatascience.com/hyperbolic-deep-reinforcement-learning-b2de787cf2f7`](https://towardsdatascience.com/hyperbolic-deep-reinforcement-learning-b2de787cf2f7)

## 强化学习遇到超曲面几何

## 许多强化学习问题具有层次树状特性。超曲面几何为此类问题提供了强大的先验知识。

[](https://michael-bronstein.medium.com/?source=post_page-----b2de787cf2f7--------------------------------)![Michael Bronstein](https://michael-bronstein.medium.com/?source=post_page-----b2de787cf2f7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b2de787cf2f7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2de787cf2f7--------------------------------) [Michael Bronstein](https://michael-bronstein.medium.com/?source=post_page-----b2de787cf2f7--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2de787cf2f7--------------------------------) ·17 分钟阅读·2023 年 4 月 30 日

--

强化学习中的许多问题表现出层次树状的特性。因此，超曲面空间（可以被概念化为树的连续类比）是参数化智能体深度模型的合适候选。本文概述了超曲面几何的基础知识，实证展示了它为许多强化学习问题提供了良好的归纳偏置，并描述了一种实用的正则化程序，允许解决在超曲面潜在空间中进行端到端优化时的数值不稳定性。我们的方法在广泛的常见基准测试中显示了几乎普遍的性能提升，无论是使用策略算法还是离策略算法。

![](img/b6bdc310bab65aab0cc8849bfb1a4359.png)

由“超曲面 Atari Breakout 游戏，图标设计，平面设计，矢量艺术”提示的稳定扩散（由[David Ha](https://twitter.com/hardmaru?lang=en)提供）

*本文由*[*Edoardo Cetin*](https://aladoro.github.io/)*、* [*Ben Chamberlain*](https://twitter.com/DrBPChamberlain)*和* [*Jonathan Hunt*](https://twitter.com/jjh) *共同撰写，并基于 E. Cetin 等人发表的论文* [*Hyperbolic deep reinforcement learning*](https://arxiv.org/pdf/2210.01542.pdf) *(2023) ICLR。欲了解更多详情，请在 ICLR 2023 上找到我们！*

# 强化学习基础

强化学习问题可以描述为马尔可夫决策过程（MDP），其中智能体从环境的状态空间中观察到某个*状态* *s*∈*S*，基于此执行某个*动作* *a*∈A，从其动作空间中，并最终从其*奖励函数* *r: S×A ↦ R*中获得奖励*r*。

环境的演变依赖于*马尔可夫性质*，这意味着给定当前状态，它独立于过去的状态，并由转移动态 *P* : *S×A×S ↦ R* 和初始状态分布 *p₀*: *S ↦ R* 完全描述。

*策略*是一个关于动作的参数化分布函数 *a∼π*(*⋅|s*)，给定当前状态 *s*，表示代理的行为。代理与环境之间的每次交互产生一个轨迹，*τ = (s₀,a₀,s₁,a₁,…)*，根据策略和转移动态生成。对于每个状态 *s∈ S*，策略的*价值函数*表示从 *s* 开始的代理可能轨迹上的未来奖励的期望折扣总和[*]。

代理的目标是学习一个最大化其期望折扣奖励总和的策略，或等效地，最大化在可能初始状态上的期望价值函数*。* 在深度 RL 中，策略和值函数通常建模为神经网络。RL 训练循环涉及在*经验收集*阶段（在环境中部署当前策略）和*学习*阶段（更新代理的模型以改善其行为）之间交替进行。根据收集的经验数据的使用方式，我们可以区分两类主要的 RL 算法：

*在线策略*算法为每次训练迭代收集一组新的轨迹，使用最新策略，丢弃旧数据。他们使用这些轨迹来学习当前策略的价值函数，然后用它来计算*策略梯度*[1]并最大化执行最佳观察到的当前动作的概率。近端策略优化（PPO）[2] 目前是这一类别中最成熟和最稳健的算法之一[3]。

*离线策略*算法则将许多不同的轨迹存储在一个大的重放缓冲区数据集中，这些轨迹是通过旧策略的混合收集的。他们使用这些数据直接学习一个基于贝尔曼备份[4]的最优价值函数模型。然后，策略是基于导致最高期望估计值的动作隐式定义的。Rainbow DQN [5] 是一种现代流行的开创性离线策略 DQN 算法[6]，引入了几种辅助实践，以稳定和加快学习速度。

**深度强化学习中的泛化**

泛化是有效强化学习代理的关键要求，因为大多数现实世界甚至复杂的模拟任务都涉及其状态空间中的大量多样性（例如，自然图像的空间）。从代理的角度来看，探索和记忆这一（可能是无限的）输入集合的精确值显然是不可行的。此外，对于许多应用，训练中使用的受控实验室环境可能无法反映特定任务的所有可能配置的全面多样性。因此，代理的行为在部署期间应理想地对可能观察到的小分布偏移保持稳健。

基于深度神经网络的代理模型作为一种实用的方法来解决这些问题，实际上作为一种功能性先验，试图仅捕捉代理在有效决策过程中所需的*最相关和因果*特征。然而，如何准确理解不同设计选择对神经网络训练及其最终泛化效果的影响，仍然是一个开放的问题。

在我们最近的论文[7]中，我们研究了使深度强化学习模型稳健有效泛化的几何特性。特别是，我们关注于*超曲面几何*模型，接下来我们会描述[8]。

![](img/f7cdd5dea837803c23323e1fc25219da.png)

在 Breakout 中，状态之间的层次关系，通过超曲面空间的庞加莱圆盘模型可视化。

# 超曲面几何

大多数机器学习（更广泛地说，计算机科学和数学）应用利用欧几里得空间来表示数据和执行数值运算。欧几里得空间可以很容易地可视化，并且它们的大多数属性本质上是直观易懂的。例如，总体积随着从原点的半径以多项式形式增长[9]，并且将两个点通过相同的向量平移不会影响它们之间的距离。

超曲面空间[10]不具备这样直观的属性，并且形式上可以描述为一种特殊的*黎曼流形*，即* n 维*的对象嵌入在*n*+1 维中，仅在*局部*上是欧几里得的。超曲面空间的一个定义特性是其*恒定的负曲率*，导致距离和体积以指数形式增长，而不是多项式形式。

![](img/7a7bae5263380d37c2a3e7e41bbb60ac.png)

*超曲面空间（𝔹²）中点之间的最短路径（测地线）和嵌入树中的节点。*

这使得超曲面空间可以被解释为树的连续类比，其中叶节点的数量也随着深度的增加而呈指数增长。由于这一事实，一棵树可以在只有二维的超曲面空间中等距嵌入（即保持节点之间相对距离的方式）。相比之下，将一棵树嵌入到欧几里得空间会导致扭曲，这些扭曲可以通过使用高维空间来减少。

存在多个等效的双曲几何模型；在这里，我们考虑*庞加莱球*（记作𝔹*ⁿ*），它可以被概念化为一个*n*维的单位球，保留了来自欧几里得空间的角度概念。由于庞加莱球的总体积随着从原点的半径指数增长，测地线（最短路径）是与边界垂直的圆弧，而不是像在欧几里得空间中的直线[12]。

![](img/682c28537fec44ba001ca9f2c315d613.png)

庞加莱和贝尔特拉米-克莱因的双曲几何模型。

为了在机器学习中处理双曲空间，我们必须重新定义与向量的标准操作、超平面的概念以及这些元素之间的相对距离[13]。这样做的概念困难在于我们需要在*切空间*中工作，这是双曲空间的局部欧几里得表示。

这是通过*指数映射* exp**ₓ**(**v**)，它沿着从点**x**出发的测地线朝输入向量**v**的方向迈出一个单位步长来实现的。我们使用从庞加莱球原点的指数映射将欧几里得输入向量**v**映射到𝔹*ⁿ* [14]*。

*陀螺向量空间* [15] 允许将常见的向量操作扩展到非欧几里得几何中。这样的一个操作记作**x**⊕**y**，被称为*莫比乌斯加法* [16]。

*陀螺平面*（记作*H*）是陀螺向量空间中定向超平面的推广。庞加莱球上的一个陀螺平面由*n*维的平移**p**和法向量**w**参数化，使得*H =* {**y***∈* 𝔹*ⁿ* : *<***y**⊕**p***,***w***>=0*}。

在机器学习问题中，超平面和陀螺平面可以用作*定向决策边界*。平移和法向量（**p** 和 **w**）提供了一种替代参数化来定义线性仿射变换[17]。具有*m*个输出单元的全连接层的类比是𝔹ⁿ中的一组*m*个陀螺平面：给定一个在双曲空间中的*n*维输入向量**x**，层输出 *f*(**x**) 作为*x*与每个陀螺平面 H 之间的带符号和缩放距离来计算：

*f*(**x**) *=* 2 sign(*<***x**⊕–**p***,***w***>*) *||***w***||d*(**x***,H*) */* (*1 — ||***p***||*²)¹ᐟ²*,*

其中 *d*(**x***,H*) 是庞加莱球上*x*和*H*之间的距离函数[18]。

与先前用于监督学习[19]和无监督学习[20]的工作类似，我们在标准自然网络架构中使用这些参数化的*陀螺平面全连接层*，替代了标准的欧几里得层。结果特征空间的双曲几何引入了一种不同的归纳偏差，这对许多强化学习问题应该更为适用，我们将在下文中进行说明。

# 强化学习问题的双曲性

由于 RL 问题中的马尔可夫性质，轨迹中状态的演变可以被概念化为一个树，其中策略和动态决定了每个可能分支的概率。直观地，MDP 中每个状态的值和最优策略自然与其可能的后继状态相关。

相比之下，有许多例子表明其他固定的、非层次化的状态信息（例如环境的一般外观）应该被忽略。例如，Raileanu 和 Fergus [21] 观察到代理的价值函数和策略模型在*Procgen*环境 [22]（例如背景颜色）中对非层次特征的虚假关联进行了过拟合，导致对未见过的关卡泛化能力差。

基于这些观察，我们假设有效的特征应该编码直接与 MDP 的层次状态关系相关的信息，反映其树状结构。

为了验证我们的假设，我们分析了 RL 代理学习到的表示空间，测试它们是否表现出层次结构。我们使用 Gromov *δ*-*双曲性* [11] 的概念：一个度量空间（*X,d*）如果每个可能的测地线三角形△ABC 都是*δ*-瘦的，即△ABC 的每一侧上的每个点都存在另一侧上的某个点，其距离至多为*δ*，则称其为*δ*-双曲。树形结构的一个特征是△ABC 中的每个点都属于至少两个侧面，从而产生*δ=0*。因此，我们可以将*δ*-双曲性解释为度量与树形度量之间的偏差。

![](img/7c3e8634f50ea4cb0ea5cd2b31345a83.png)

必要的*δ*使得△ABC 的每一侧上的每个点都存在另一侧上的某个点，其距离在树形三角形（左）、双曲三角形（中）和欧几里得三角形（右）中至多为*δ*。

RL 代理通过对收集的状态进行编码所学习的最终表示跨越了欧几里得空间的某个有限子集，有效地形成了一个离散度量空间。类似于[19]，我们通过空间的直径对*δ*-双曲性记录进行*归一化*，产生一个*相对*度量，试图对所学表示的尺度保持无关[23]。

这使我们能够实际解释所学的潜在表示的双曲性，其值在 0（完全树状结构）和 1（完全非双曲空间）之间。我们使用标准的 Impala 架构[24]训练一个标准的 PPO 代理，并分析随着训练的进展，其性能和我们的*δ*-双曲性度量如何演变，测试四种不同的 Procgen 环境。

我们观察到，在所有环境中，*δ*在训练的前几次迭代中迅速降到低值（0.22–0.28），反映了代理性能的相对最大提升。随后，似乎发生了一个有趣的二分法。在*fruitbot*和*starpilot*环境中，*δ*在训练过程中进一步减少，因为代理在训练和测试水平分布之间恢复了高性能，并且泛化差距较小。

相反，在*bigfish*和*dodgeball*中，*δ*在初始下降后开始再次增加，表明潜在表示空间开始丧失其层次结构。相应地，在这两个环境中，代理开始过拟合，因为测试水平的表现停滞不前，而与训练水平表现之间的泛化差距持续扩大。

![](img/56479195563a0e88b5c97d8b6ba2dbe6.png)

PPO 代理在 Procgen 中的最终潜在空间的性能和相对*δ*-超曲率。

这些结果支持了我们的假设，经验上展示了编码层次特征的重要性，并建议 PPO 在某些环境中泛化性能差是由于欧几里得潜在空间编码了虚假的特征，这些特征阻碍了超曲率的效果。

# 使用超曲率潜在空间训练代理

基于我们的发现，我们建议使用超曲率几何来编码深度 RL 模型的最终潜在表示。我们的方法旨在引入不同的归纳偏差，以激励基于反映常见 MDP 中观察到的因果层次演变的特征来建模代理的策略和值函数。

我们的基本实现尝试对底层算法和模型进行最小的修改。我们从对 PPO 的简单扩展开始，将最终的 ReLU 和线性层替换为到𝔹*ⁿ*的指数映射以及一个输出值函数和策略对数的陀螺面全连接层。

![](img/23ce148581eee208b86533481577d927.png)

将超曲率空间与用于建模 PPO 策略和值的 Impala 架构进行集成。

然而，这种幼稚的方法导致了令人失望的表现，远远落后于标准 PPO 的表现。此外，我们发现超曲率策略在性能改善时，难以开始探索并随后退回到更确定性的行为，这与我们通常期望 PPO 的熵奖励相反。这些结果似乎表明了源自超曲率表示的端到端 RL 训练中的优化挑战。

![](img/23d2a5c2556a8df1e939877d6c881947.png)

我们未正则化的超曲率表示与 RL 集成的性能和梯度。

为了克服类似的优化问题，先前的工作在监督和无监督学习中使用双曲空间，提出了仔细的初始化方案[25]和稳定化方法，如表示剪裁[26]。虽然我们在实现中也使用了这些方法，但它们在 RL 问题中似乎效果不大。

这不应该令人惊讶：这些策略的主要目的是在前几次训练迭代中促进学习适当的角度布局，否则后续的端到端优化常常会导致低性能的失败模式[13]。然而，RL 的固有高方差和非平稳性使得主要关注早期迭代的稳定化策略不足。RL 中的轨迹数据和损失景观在训练过程中可能会显著变化，使得早期的角度布局在长期内不可避免地变得次优。此外，高方差的策略梯度优化[1]更容易进入上述不稳定的学习状态，从而导致观察到的失败模式。

另一个需要处理类似非平稳性和脆弱优化的机器学习子领域是生成对抗网络（GANs）[27]。在 GAN 训练中，生成的数据和判别器的参数不断演变，使得损失景观高度非平稳，类似于 RL 设置。此外，优化的对抗性质使其对梯度爆炸和消失的不稳定性非常脆弱，导致常见的失败模式[28]。

我们从这方面的文献中获得灵感，并利用了*谱归一化*（SN）[29]，基于最近的分析和实证结果，显示其能够准确防止梯度爆炸现象[30]。我们的实现将 SN 应用于模型的欧几里得编码器部分的所有层，留下最终的线性双曲层不进行正则化。此外，我们还在将最终的潜在表示映射到𝔹ⁿ之前对其进行缩放，以便修改表示的维度不会显著影响它们及其梯度的大小。我们将我们的稳定化方法称为*谱正则化双曲映射*（S-RYM，发音为*ɛs-raɪm*）。

将 S-RYM 应用于我们的双曲 RL 代理似乎解决了它们的优化挑战。此外，它们相对于原始欧几里得实现也获得了显著更高的性能，并且在整个训练过程中保持了低梯度幅度。

![](img/99c4c90167bc2c90f9822e963da8aeff.png)

将 S-RYM 应用于双曲和欧几里得 RL 代理的性能和梯度。

# 实验结果

我们在不同的基准测试、RL 算法和训练条件下评估双曲线深度 RL。除了 PPO，我们还将我们的方法应用于离线策略的 Rainbow DQN 算法。我们在完整的 Procgen 基准测试（16 个环境）[22]和 Atari 100K 基准测试（26 个环境）[31]上测试了我们的代理。

![](img/0accfda16a746ab49d4197de2c00382c.png)

Procgen（左）和 Atari（右）不同环境的渲染。

在 Procgen 基准测试中，我们将我们的双曲线实现与使用随机裁剪数据增强的方法[32]进行比较，这是一种通过引入人工选择的不变性来激励泛化的更传统方法。此外，我们还测试了双曲线模型的另一种版本，该版本进一步将最终表示的维度限制为 32（从原始欧几里得架构中的 256），旨在增加其对能够在双曲线空间中有效编码的特征的关注。

我们的双曲线 PPO 和 Rainbow DQN 实现都在绝大多数环境中表现出了显著的性能提升。值得注意的是，我们发现减少双曲线表示的大小对于这两种算法都提供了进一步的好处，显著提升了性能。

![](img/c84dfa3879bc99927d59414ed15e53e3.png)

双曲线和欧几里得版本的 PPO 在 Procgen 上的表现。

![](img/a1a5df8d6f5a19c69378bf7f02af8d5f.png)

双曲线和欧几里得版本的 Rainbow DQN 在 Procgen 上的表现。

相比之下，应用数据增强似乎带来了较低且不一致的收益。我们还发现，测试性能的提升并不总是与代理在探索中能够访问的特定 200 个训练级别的收益相关，导致双曲线代理的泛化差距显著减少。

同样，相同的双曲线深度 RL 框架在 Atari100K 基准测试中也提供了一致且显著的好处。双曲线 Rainbow 在大多数 Atari 环境中相较于欧几里得基线表现出改进，几乎将最终的人类归一化得分翻倍。

![](img/c08915dcdfee772e763fcc6198fd0a67.png)

将我们正则化的双曲线表示整合到 Atari 基准测试中的 Rainbow DQN 上的归一化性能绝对差异（Y 轴）和相对改进（柱状图上方）。

总体而言，我们的结果实证验证了引入双曲线表示来塑造深度 RL 模型的先验在多种问题和算法中可能是极其有效的。

我们的双曲线强化学习（RL）代理与当前的 SotA（state-of-the-art）算法非常接近，这些算法采用了不同的昂贵和特定领域的做法（例如，临时辅助损失、更大的专用架构等）。综合来看，我们相信这些结果展示了我们双曲线框架的巨大潜力，使其有可能成为参数化深度 RL 模型的标准方法。

# 可视化

使用我们双曲 PPO 的二维版本进行可视化时，我们观察到一个重复出现的现象，即在轨迹的考虑子集内，表示的大小随着环境中更多障碍物和/或敌人的出现而单调增加。此外，我们观察到这些表示形成了树状结构，从编码策略状态获得的大小在方向上大多与价值函数的陀螺盘法线对齐。

这种增长直观地反映了随着新元素的出现，代理识别到更大的奖励机会（例如，通过击败新敌人获得的奖励），然而，也需要更精细的控制，因为与其他策略陀螺盘的距离也会指数增长，从而减少熵。相反，跟随随机行为的偏差，表示的大小趋向于在看起来几乎与价值陀螺盘法线正交的方向上增长。因此，这种增长仍然反映了优化决策所需的更高精度，同时也反映了代理在从次优行为中达到的状态中获取未来奖励的不确定性。

![](img/8546e540b74d427d92c84f61a7671157.png)

在 Procgen 中，随着我们在轨迹上前进的二维双曲嵌入，编码的状态遵循的是策略行为（绿色）或随机行为（红色）。

# 结论

我们的实验提供了使用双曲几何在深度强化学习中的优势和普遍性的有力证据，几乎在所有基准和强化学习算法类别中都取得了近乎普遍的改进。我们的发现表明，几何结构可以大大影响深度模型学习所诱导的先验，或许，提示我们应重新评估它在应对许多额外的机器学习挑战中的作用和相关性。例如，使用双曲空间也可能对无监督和离线强化学习产生影响，为处理这些问题设置中目标不明确和数据有限提供更合适的先验。

[1] R. S. Sutton 和 A. G. Barto, *强化学习：导论*（2018 年）MIT 出版社，提供了对强化学习领域的全面介绍。另见其他优秀的 [在线资源](https://rail.eecs.berkeley.edu/deeprlcourse/)。

[2] J. Schulman，[邻近策略优化算法](https://arxiv.org/pdf/1707.06347.pdf)（2017 年）arXiv:1707.06347。

[3] PPO 通过限制策略更新对当前概率的变化> *ϵ*，并使用辅助熵奖励来提高稳定性。

[4] R. E. Bellman, *动态规划*（2010 年）普林斯顿大学出版社。

[5] M. Hessel 等，[Rainbow: 结合深度强化学习中的改进](https://ojs.aaai.org/index.php/AAAI/article/view/11796/11655)（2018 年） *AAAI*。

[6] V. Mnih 等，[通过深度强化学习实现人类水平的控制](https://www.nature.com/articles/nature14236)（2015 年） *Nature* 518 (7540):529–533。

[7] E. Cetin 等人，[双曲深度强化学习](https://openreview.net/pdf?id=TfBHFLgv77)（2023）*ICLR*。另见附带的[代码](https://sites.google.com/view/hyperbolic-rl)。

[8] 关于双曲空间及其在机器学习中早期应用的概述，请参阅布莱恩·肯格的[一篇很棒的入门博客文章](https://bjlkeng.github.io/posts/hyperbolic-geometry-and-poincare-embeddings/)。有关更正式的介绍，请参见 J. W. Cannon 等人著的《双曲几何学》（1997），载于《几何学的风味》第 31 卷，第 59-115 页。

[9] 大多数人从学校几何学中了解这个知识：圆的面积（即二维球体的体积）是*πr*²。*n*维欧几里得球体半径为*r*的体积的一般公式是 *πⁿ*ᐟ²*rⁿ /* Γ*(n*/2 +1*)*。请注意，尽管它在*r*中是多项式的，但在维度*n*中是*指数型的*。因此，为了在欧几里得空间中表示（具有指数体积增长的）树状结构，必须增加维度。

[10] 双曲几何是非欧几里得几何首次成功的构造，其中经典的平行公设不成立。早期的失败尝试可追溯到奥马尔·海亚姆和乔凡尼·萨克雷里。首次成功构造的优先权在卡尔·弗里德里希·高斯、亚诺什·博伊亚伊和尼古拉·罗巴切夫斯基（第一个公布其结果的人）之间存在争议。尤金尼奥·贝尔特拉米（以及后来费利克斯·克莱因）展示了双曲几何的自洽性，并提出了以他们名字命名的投影模型（贝尔特拉米-克莱因模型）。

[11] M. Gromov，*双曲群*（1987），施普林格出版社。

[12] 这使得不同点之间的测地线必须经过某个半径较小的中点，类似于树状测地线必须经过它们最近的共享父节点。

[13] O. Ganea、G. Bécigneul 和 T. Hofmann，[双曲神经网络](https://proceedings.neurips.cc/paper_files/paper/2018/file/dbab2adc8f9d078009ee3fa810bea142-Paper.pdf)（2018）*NeurIPS*。

[14] 指数映射的闭式表达为 exp₀(**v**) = **v** tanh(**v**) */ ||***v***||*。

[15] A. A. Ungar，[*解析双曲几何学与阿尔伯特·爱因斯坦的相对论*](https://www.worldscientific.com/worldscibooks/10.1142/6625#t=aboutBook)（2008），世界科学出版社。

[16] 在双曲空间中，两向量的莫比乌斯加法定义为

**x**⊕**y** *=* ((1 + 2**x***<***x**,**y***> + ||***y***||*²)**x** *+* (1 + *||***x***||*²)**y**) */* (1 + 2*<***x**,**y***> + ||***x***||*² *||***y***||*²)，详见我们论文中的公式（4）[7]。

[17] G. Lebanon 和 J. Lafferty，[多项式流形上的超平面间隔分类器](https://icml.cc/Conferences/2004/proceedings/papers/13.pdf)（2004）*ICML*。

[18] 距离的闭式表达为 *d*(**x***,H*)*=*sinhᐨ¹(2*|<****x***⊕–**p***,***w***>|* / (1 — *||***x**⊕–**p***||*²*||***w***||*))，详见我们论文中的公式（6）[7]。

[19] V. Khrulkov 等， [双曲图像嵌入](https://openaccess.thecvf.com/content_CVPR_2020/papers/Khrulkov_Hyperbolic_Image_Embeddings_CVPR_2020_paper.pdf) (2020) *CVPR*。

[20] E. Mathieu 等， [具有庞加莱变分自编码器的连续层次表示](https://proceedings.neurips.cc/paper_files/paper/2019/file/0ec04cb3912c4f08874dd03716f80df1-Paper.pdf) (2019) *NeurIPS*。

[21] R. Raileanu 和 R. Fergus， [在强化学习中解耦价值和策略以实现泛化](https://arxiv.org/pdf/2102.10330.pdf) (2021)， *ICML*。

[22] Procgen，由 K. Cobbe 等介绍， [利用程序生成来基准化强化学习](http://proceedings.mlr.press/v119/cobbe20a/cobbe20a.pdf) (2020) *ICML*，包括 16 个具有程序生成随机关卡的视觉环境。虽然不同的关卡共享一个高级目标（例如，达到门口，击败所有敌人等），但它们在布局和外观上可能有显著差异。此外，在这个基准测试中，代理仅能访问每个环境的前 200 个关卡进行经验收集，但其性能在所有关卡的分布上进行测试。因此，该基准测试允许评估强化学习代理，特别是其对未见关卡的泛化能力。

[23] M. Borassi、A. Chessa 和 G. Caldarelli， [双曲性度量现实世界网络中的民主](https://arxiv.org/pdf/1503.03061.pdf) (2015) *Physical Review E* 92.3: 032812。

[24] L. Espeholt 等， [Impala: 可扩展的分布式深度强化学习与重要性加权演员-学习者架构](https://proceedings.mlr.press/v80/espeholt18a/espeholt18a.pdf) (2018) *ICML*。

[25] M. Nickel 和 D. Kiela， [用于学习层次表示的庞加莱嵌入](https://papers.nips.cc/paper_files/paper/2017/file/59dfa2df42d9e3d41f5b02bfc32229dd-Paper.pdf) (2017) *NeurIPS*。

[26] Y. Guo 等， [剪裁的双曲分类器是超级双曲分类器](https://openaccess.thecvf.com/content/CVPR2022/papers/Guo_Clipped_Hyperbolic_Classifiers_Are_Super-Hyperbolic_Classifiers_CVPR_2022_paper.pdf) (2022)， *CVPR*。

[27] I. Goodfellow 等， [生成对抗网络](https://arxiv.org/pdf/1406.2661.pdf) (2014)， *NIPS*。

[28] M. Arjovsky 和 L. Bottou， [朝着有原则的方法训练生成对抗网络](https://openreview.net/pdf?id=Hk4_qw5xe) (2017) *ICLR*。

[29] T. Miyato 等， [生成对抗网络的谱归一化](https://arxiv.org/pdf/1802.05957.pdf) (2018) *arXiv*:1802.05957。

[30] Z. Lin、V. Sekar 和 G. Fanti， [为何谱归一化稳定生成对抗网络：分析与改进](https://arxiv.org/pdf/2009.02773.pdf) (2021)， *NeurIPS*。

[31] Atari 100K，由 Kaiser、Lukasz 等人介绍，[基于模型的强化学习用于 Atari](https://openreview.net/pdf?id=S1xCPJHtDB) (2019) *arXiv*:1903.00374，包含了 26 种不同的视觉环境，这些环境由 M. G. Bellemare 等人提供，包含了 M. G. Bellemare 等人提出的经典 Atari 游戏，[街机学习环境：一种通用代理的评估平台](https://arxiv.org/pdf/1207.4708.pdf) (2013) *人工智能研究期刊* 47: 253–279。然而，代理只能访问 100K 总环境步数的数据来进行经验收集，这大致相当于 2 小时的游戏时间。环境经过 M. C. Machado 等人提出的规格进行修改，[重新审视街机学习环境：通用代理的评估协议和开放问题](https://arxiv.org/pdf/1709.06009.pdf) (2018) *人工智能研究期刊* 61:523–562，介绍了通过 *粘性动作*（即每次执行动作的随机重复）引入了相当大的随机性。因此，由于严重的数据限制和额外的随机性，这一基准特别关注评估对未见状态的泛化能力。

[32] D. Yarats、I. Kostrikov 和 R. Fergus，[图像增强就是你所需要的：从像素中正则化深度强化学习](https://openreview.net/pdf?id=GY6-6sTvGaf) (2021)，*ICLR*。

*我们感谢* [*David Ha*](https://twitter.com/hardmaru?lang=en) *(即 hardmaru) 为我们生成了标题图像，这是本博客上第一张 AI 生成的插图！有关更多信息，请参阅* [*项目网页*](https://medium.com/r?url=https%3A%2F%2Fsites.google.com%2Fview%2Fhyperbolic-rl)*，* [*Towards Data Science*](https://towardsdatascience.com/graph-deep-learning/home) *Medium 博客文章，* [*订阅*](https://michael-bronstein.medium.com/subscribe) *Michael 的文章和* [*YouTube 频道*](https://www.youtube.com/c/MichaelBronsteinGDL)*，获取* [*Medium 会员资格*](https://michael-bronstein.medium.com/membership)*，或关注* [*Michael*](https://twitter.com/mmbronstein)*、* [*Edoardo*](https://twitter.com/edo_cet)*、* [*Ben*](https://twitter.com/DrBPChamberlain) 和 [*Jonathan*](https://twitter.com/jjh) *在 Twitter 上的动态。*
