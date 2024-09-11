# 数据驱动的方程发现

> 原文：[https://towardsdatascience.com/on-data-driven-equation-discovery-5069795d239d?source=collection_archive---------3-----------------------#2023-12-01](https://towardsdatascience.com/on-data-driven-equation-discovery-5069795d239d?source=collection_archive---------3-----------------------#2023-12-01)

[](https://georgemilosh.medium.com/?source=post_page-----5069795d239d--------------------------------)[![乔治·米洛舍维奇](../Images/5a73ad29cf10643f29caf9548bacd7e6.png)](https://georgemilosh.medium.com/?source=post_page-----5069795d239d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5069795d239d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5069795d239d--------------------------------) [乔治·米洛舍维奇](https://georgemilosh.medium.com/?source=post_page-----5069795d239d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe846787fbdef&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-data-driven-equation-discovery-5069795d239d&user=George+Miloshevich&userId=e846787fbdef&source=post_page-e846787fbdef----5069795d239d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5069795d239d--------------------------------) ·14分钟阅读·2023年12月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5069795d239d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-data-driven-equation-discovery-5069795d239d&user=George+Miloshevich&userId=e846787fbdef&source=-----5069795d239d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5069795d239d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fon-data-driven-equation-discovery-5069795d239d&source=-----5069795d239d---------------------bookmark_footer-----------)![](../Images/f1e469c236aadabe124517accfa340b3.png)

图片由 [ThisisEngineering RAEng](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

借助实验验证的分析表达式来描述自然，一直以来是科学特别是物理学从万有引力定律到量子力学及更广泛领域成功的标志。随着气候变化、聚变和计算生物学等挑战使我们将关注点转向更多的计算，迫切需要在降低成本的同时保持物理一致性的简明而强大的减少模型。科学机器学习是一个新兴领域，它承诺提供这样的解决方案。本文是对近期数据驱动方程发现方法的简要回顾，面向对机器学习或统计学非常基础的科学家和工程师。

# 动机与历史视角

单纯地将数据拟合得很好已被证明是一种短视的努力，这一点通过托勒密的地心说模型得到了证明，该模型是[直到开普勒的日心说](https://farside.ph.utexas.edu/talks/AlmagestNotes.pdf)之前最符合观测的模型。因此，将观察与基本物理原理相结合在科学中发挥了重要作用。然而，在物理学中，我们常常忽略了我们的世界模型已经是数据驱动的程度。以[粒子标准模型](https://en.wikipedia.org/wiki/Standard_Model)为例，它有19个参数，其数值是通过实验确定的。用于气象和气候的地球系统模型虽然在基于流体动力学的物理一致核心上运行，但也需要对其许多敏感参数进行[仔细的观测校准](https://journals.ametsoc.org/view/journals/bams/98/3/bams-d-15-00135.1.xml)。最后，减少阶次建模在聚变和空间天气社区中正在获得关注，并且很可能在未来保持相关性。在生物学和社会科学等领域，第一性原理方法效果较差，统计系统识别已经发挥了重要作用。

机器学习中有多种方法可以直接从数据中预测系统的演变。最近，深度神经网络在天气预报领域取得了显著进展，这一点由[Google’s DeepMind](https://www.science.org/doi/10.1126/science.adi2336)团队等证明。这在一定程度上归因于他们拥有的巨大资源，以及气象数据和物理数值天气预报模型的一般可用性，这些模型通过[数据同化](https://en.wikipedia.org/wiki/Data_assimilation)将这些数据插值到全球。然而，如果数据生成的条件发生变化（例如气候变化），这些完全基于数据驱动的模型可能会表现不佳。这意味着将这些黑箱方法应用于气候建模及其他数据不足的情况可能会存在疑问。因此，在本文中，我将强调从数据中提取方程的方法，因为方程更具可解释性，且不易过拟合。在机器学习术语中，我们可以将这些范式称为[高偏差——低方差](https://www.sciencedirect.com/science/article/pii/S0370157319300766)。

首先值得一提的方法是[Schmidt 和 Lipson](https://www.science.org/doi/full/10.1126/science.1165893)的开创性工作，该工作使用了[**遗传编程**](https://en.wikipedia.org/wiki/Genetic_programming)（GP）进行**符号回归**，从简单动力系统（如双摆等）的轨迹数据中提取方程。该过程包括生成候选符号函数，推导这些表达式中涉及的偏导数，并将其与从数据中数值估算的导数进行比较。这个过程会重复进行，直到达到足够的准确性。重要的是，由于潜在候选表达式的数量非常庞大且相对准确，因此选择符合“简约原则”的表达式。简约原则通过表达式中的项数的倒数来衡量，而预测准确性则通过仅用于验证的保留实验数据上的误差来衡量。这个简约建模的原则构成了方程发现的基础。

![](../Images/9a945a50286dbfe118ac2d0e9ed9215c.png)

遗传编程（GP）的思想是通过尝试一系列潜在的项来探索可能的分析表达式空间。这个表达式被编码在上面的树中，其结构可以表示为一种“基因”。通过突变这些基因的序列、选择和交叉最优候选项，可以获得新的树。例如，要获取右侧框中的方程，只需跟随右侧树的层级中的箭头即可。

这种方法的优点在于探索各种可能的解析表达式组合。它已在各种系统中尝试过，特别是，我将重点介绍 [AI — 费曼](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7159912/)，借助 GP 和神经网络，能够从数据中识别出费曼物理讲座中的 100 个方程。GP 的另一个有趣应用是发现 [气候中的海洋参数化](https://onlinelibrary.wiley.com/doi/abs/10.1029/2022MS003258)，其中实质上运行了一个高保真模型来提供训练数据，同时从训练数据中发现了较便宜的低保真模型的修正。然而，GP 并非没有缺陷，人工干预是不可或缺的，以确保参数化效果良好。此外，由于它遵循进化的过程：试错，因此可能非常低效。还有其他可能性吗？这将引导我们到近年来主导方程发现领域的方法。

# 稀疏系统识别

**非线性动力学的稀疏识别（SINDy）** 属于概念上简单但强大的方法家族。由 Steven L. Brunton 的团队介绍，以及 [其他团队](https://www.pnas.org/doi/abs/10.1073/pnas.1302752110) ，并配有文档完善、支持良好的 [代码库](https://pysindy.readthedocs.io/en/latest/) 和 [YouTube 教程](https://youtu.be/NxAn0oglMVw?list=TLPQMTkxMTIwMjNdXvL4gEtq-g)。要获得一些实际操作经验，只需试用他们的 Jupyter notebooks。

我将根据 [原始 SINDy 论文](https://www.pnas.org/doi/full/10.1073/pnas.1517384113) 描述该方法。通常，您拥有轨迹数据，其中包含 x(t)、y(t)、z(t) 等坐标。目标是从数据中重建一阶常微分方程（ODEs）：

![](../Images/617b09a77047cef37b00987eeaf79d98.png)

通常，x(t)（有时称为响应函数）是从观察或建模数据中获得的。目标是估计 f = f(x)（ODE 的右侧）的最佳选择。通常，会尝试一个单项式库，算法会继续寻找稀疏系数向量。系数向量的每个元素控制着这个单项式对整个表达式的重要性。

![](../Images/d40fa2f6d632ad2d6e0aee3aec74d93f.png)

在这里，函数 f = f(x) 被表示为单项式库与稀疏向量的乘积。有关更多说明，请参见下面的图形。

有限差分法（例如）通常用于计算常微分方程左侧的导数。由于导数估计容易出错，这会在数据中产生噪声，这通常是不希望的。在某些情况下，过滤可能有助于处理这些问题。接下来，选择一个单项式（基函数）库来拟合常微分方程右侧，如图所示：

![](../Images/cc99210d36480f15f2d9fb645bf644df.png)

如[1]所示的非线性动力学的稀疏识别（SINDy）。其思想是提取一小部分基函数（例如单项式），即全基库的一个子集，当数据代入时，这些基函数能满足方程。在左侧写出时间导数（每列对应不同变量，每行对应数据样本，样本可能是时间），而右侧则是基库矩阵（其行跨度每个基函数）与稀疏向量相乘，稀疏向量是算法学习的对象。促进稀疏性意味着我们希望最终得到的大多数向量值为零，这符合节俭原则。

问题在于，除非我们拥有天文数字级的数据，否则这个任务将毫无希望，因为许多不同的多项式都会很好地工作，这将导致显著的过拟合。幸运的是，这正是稀疏回归的救援之处：重点是对右侧有太多活跃基函数进行惩罚。这可以通过多种方式实现。原始SINDy所依赖的一种方法叫做序列阈值最小二乘法（**STLS**），可以总结如下：

![](../Images/5ad403e1f1a5120f6a539cd3e169c083.png)

来自SINDy论文补充材料的Matlab稀疏表示代码。

换句话说，使用标准最小二乘法求解系数，然后在每次应用最小二乘法时逐步消除小系数。该过程依赖于一个超参数，该超参数控制系数的小值容忍度。这个参数看似任意，但可以进行所谓的帕累托分析：通过保留一些数据并测试学习模型在测试集上的表现来确定这个稀疏化超参数。这个系数的合理值对应于学习模型的准确性与复杂性曲线（复杂性 = 包含的项数）中的“肘部”，即所谓的*帕累托前沿*。或者，某些[其他文献](https://royalsocietypublishing.org/doi/full/10.1098/rspa.2017.0009)推荐使用信息准则来推广稀疏性，而不是执行上述的帕累托分析。

作为SINDy的最简单应用，考虑如何使用STLS成功识别[Lorenz 63模型](https://en.wikipedia.org/wiki/Lorenz_system)：

![](../Images/2583554d2258033be86db8a2ba70cc9d.png)

将 SINDy 应用于 Lorenz 63 模型识别的示例。系数（颜色）大致对应于用于生成训练数据的系数。这些数据是通过解决带有这些参数的相关 ODE 生成的。

STLS 在应用于自由度较大的系统（如偏微分方程（PDEs））时存在局限性，在这种情况下，可以考虑通过 [主成分分析](https://en.wikipedia.org/wiki/Principal_component_analysis)（PCA）或 [非线性自编码器](https://www.pnas.org/doi/full/10.1073/pnas.1906995116) 等进行降维。后来，SINDy 算法通过 [**PDE-FIND**](https://www.science.org/doi/10.1126/sciadv.1602614) **论文** 得到了进一步改进，该论文引入了顺序阈值岭回归 (**STRidge**)。在后者中，[岭回归](https://en.wikipedia.org/wiki/Ridge_regression) 指的是带有 L2 惩罚的回归，而在 STRidge 中则交替进行小系数的淘汰，如同 STLS。这使得从仿真数据中发现各种标准 PDE 成为可能，例如 [布尔戈斯方程](https://en.wikipedia.org/wiki/Burgers%27_equation)、[科尔特韦格-德弗里斯方程](https://en.wikipedia.org/wiki/Korteweg%E2%80%93De_Vries_equation)（KdV）、纳维-斯托克斯方程、反应-扩散方程，甚至是科学机器学习中常遇到的一个相当特殊的方程，[Kuramoto-Sivashinsky 方程](https://en.wikipedia.org/wiki/Kuramoto%E2%80%93Sivashinsky_equation)，由于需要直接从数据中估计其四阶导数项，这通常较为棘手。

![](../Images/9f1a6bbae22ca63f24238cd87d336f90.png)

Kuramoto-Sivashinsky 方程描述了层流火焰流中的扩散-热不稳定性。

该方程的识别直接基于以下输入数据（这些数据是通过数值求解相同方程获得的）：

![](../Images/5093864de87b3d7c86a6e64333d5e50a.png)

Kuramoto-Sivashinsky 方程的解。右侧面板显示了场，而右侧面板则显示了其时间导数。

这并不是说该方法容易出错。事实上，将 SINDy 应用于现实观察数据的一个大挑战在于这些数据往往本身稀疏且噪声较大，通常在这种情况下识别效果较差。同样的问题也影响了基于符号回归的方法，如遗传编程（GP）。

**弱 SINDy** 是一种较新的发展，它显著提高了算法在噪声方面的鲁棒性。这种方法已由多位作者独立实施，尤其是 [丹尼尔·梅森](https://www.sciencedirect.com/science/article/pii/S0021999121004204)、[丹尼尔·R·古列维奇](https://doi.org/10.1063/1.5120861) 和 [帕特里克·赖恩博德](https://doi.org/10.1038/s41467-021-23479-0)。其主要思想是，与发现 PDE 的微分形式相比，发现其 [弱] 积分形式，通过在一组域上对 PDE 进行积分，并乘以一些测试函数。这允许通过分部积分，从 PDE 的响应函数（未知解）中去除棘手的导数，而将这些导数应用于已知的测试函数。这种方法进一步应用于 [Alves 和 Fiuza](https://link.aps.org/doi/10.1103/PhysRevResearch.4.033192) 进行的等离子体物理方程发现，其中恢复了 Vlasov 方程和等离子体流体模型。

SINDy 方法的另一个显而易见的局限性是，识别始终受到构成基的项库（例如单项式）的限制。虽然可以使用其他类型的基函数，如三角函数，但这仍然不够通用。假设 PDE 具有一个有理函数的形式，其中分子和分母都可以是多项式：

![](../Images/c3e33546d6fecd1a5bbc095acaaea643.png)

这种情况使得像 PDE-FIND 这样的算法应用变得复杂

这种情况当然可以通过遗传编程（GP）轻松处理。然而，SINDy 也扩展到了这样的情况，引入了 [SINDy-PI](https://royalsocietypublishing.org/doi/full/10.1098/rspa.2020.0279)（并行隐式），该方法成功用于识别描述 [贝洛乌索夫-扎博廷斯基反应](https://en.wikipedia.org/wiki/Belousov%E2%80%93Zhabotinsky_reaction) 的 PDE。

最后，其他稀疏促进方法，如[稀疏贝叶斯回归](https://www.miketipping.com/papers/met-mlbayes.pdf)，也称为相关向量机（RVM），也被用于使用完全相同的拟合术语库的方法从数据中识别方程，但受益于边际化和统计学家高度尊重的“奥卡姆剃刀”原则。我在这里不覆盖这些方法，但可以说，像[张和林](https://www.sciencedirect.com/science/article/pii/S0888327022000309)这样的作者声称对ODEs的系统识别更为稳健，并且这种方法甚至尝试用于学习[简单条带气候模型的闭合](https://onlinelibrary.wiley.com/doi/abs/10.1029/2020GL088376)，其中作者认为RVM比STRidge更稳健。此外，这些方法为识别方程的估计系数提供了自然的不确定性量化（UQ）。话虽如此，[集成SINDy](https://royalsocietypublishing.org/doi/full/10.1098/rspa.2021.0904)的最新发展更加稳健，提供UQ，但则依赖于统计方法[自助聚合](https://en.wikipedia.org/wiki/Bootstrap_aggregating)（bagging），这一方法也广泛应用于统计学和机器学习。

# 物理信息深度学习识别

解决和识别偏微分方程（PDE）系数的另一种方法是[**物理信息神经网络**](https://www.sciencedirect.com/science/article/abs/pii/S0021999118307125?via%3Dihub=)（PINNs），该方法在文献中引起了极大关注。主要思想是使用神经网络对PDE的解进行参数化，并将运动方程或其他类型的基于物理的归纳偏置引入损失函数。损失函数在预定义的一组所谓的“协同点”上进行评估。在执行梯度下降时，神经网络的权重会被调整，从而“学习”解决方案。所需提供的唯一数据包括初始条件和边界条件，这些条件也在一个单独的损失项中受到惩罚。该方法实际上借鉴了旧的非神经网络的协同方法。虽然神经网络提供了自然的自动微分方式，使这种方法非常有吸引力，但事实证明，PINNs与标准数值方法如有限体积/有限元等[**通常不具竞争力**](https://www.frontiersin.org/articles/10.3389/fdata.2021.669097)。因此，作为解决前向问题（数值求解PDE）的工具，[PINNs](https://en.wikipedia.org/wiki/Physics-informed_neural_networks)并不那么有趣。

它们成为解决逆问题的有趣工具：通过数据估计模型，而不是通过已知模型生成数据。在[原始 PINNs 论文](https://www.sciencedirect.com/science/article/abs/pii/S0021999118307125?via%3Dihub=)中，两个 Navier-Stokes 方程的未知系数是通过数据进行估计的。

![](../Images/07f9b379cf8ee7cdaeb71e5cb388226f.png)

输入到 PINN 损失函数中的 Navier-Stokes 方程的假定形式。通过识别，获得了两个未知参数（位于黄色框内）。有关 PINNs 的 tensorflow 实现，[请参阅](https://georgemilosh.github.io/blog/2022/distill/)。

回顾起来，与 PDE-FIND 等算法相比，这似乎有些天真，因为方程的一般形式已经被假定。然而，这项工作的一个有趣方面是算法并没有输入压力数据，而是假设了不可压缩流动，并通过 PINN 直接恢复压力的解。

PINNs 已经在各种情况下应用，我想特别强调一个应用是[空间天气](https://onlinelibrary.wiley.com/doi/abs/10.1029/2022JA030377)，在这个应用中，展示了通过解决 Fokker-Planck 方程的逆问题来估计辐射带中的电子密度。这里，重新训练神经网络的集成方法在估计不确定性方面非常有用。最终，为了实现可解释性，进行学习的扩散系数的多项式扩展。将这种方法与直接使用类似 SINDy 的方法进行比较会非常有趣，后者也提供了多项式扩展。

“物理信息”这个术语已经被其他团队采纳，他们有时发明了自己将物理先验融入神经网络的版本，并称之为类似“基于物理”或“受物理启发”等引人注目的名称。这些方法有时可以被归类为软约束（惩罚不满足某些方程或对称性的损失）或硬约束（将约束实施到神经网络的架构中）。这种方法的例子可以在[气候科学](https://link.aps.org/doi/10.1103/PhysRevLett.126.098302)等其他学科中找到。

由于[反向传播](https://en.wikipedia.org/wiki/Backpropagation)的神经网络提供了一种估计时间和空间导数的替代方法，因此稀疏回归（SR）或遗传编程（GP）与这些神经网络配合方法的结合似乎是不可避免的。虽然这样的研究有很多，但我将重点介绍一个相对文档齐全且支持良好的[**DeePyMoD**](https://www.sciencedirect.com/science/article/pii/S0021999120307592)以及[代码库](https://github.com/PhIMaL/DeePyMoD)。了解这种方法的工作原理足以理解同时期或之后出现的所有其他研究，并在各种方面[改进](https://www.sciencedirect.com/science/article/abs/pii/S0893608022002660)。

![](../Images/9718eb6161363b5947487c506988022e.png)

[DeePyMoD](http://arxiv.org/abs/2011.04336) 框架：PDE 的解通过前馈神经网络（NN）进行参数化。在[最新的论文](http://arxiv.org/abs/2011.04336)中，损失函数由两个项组成：数据与 NN 预测之间的均方误差（MSE）项；正则化损失，它惩罚包括活跃库项在内的 PDE 函数形式。类似于 SINDy 的 STLS，当网络收敛到解时，稀疏性向量中的小项被消除，从而仅推广库中最大的系数。然后，NN 的训练会重复进行，直到满足收敛标准。

损失函数包括均方误差（MSE）：

![](../Images/f254de2be4d37cb3076939a8c6a05a47.png)

以及促进 PDE 函数形式的正则化

![](../Images/29b369cfc76ff4ac3a7217ad1557b015.png)

与较弱的 SINDy 相比，DeePyMoD 在噪声下显著更稳健，仅需要在时空域上很少的观测点，这对于从观测数据中发现方程是个好消息。例如，许多 PDE-FIND 能正确识别的标准 PDE 也可以由 DeePyMoD 识别，但只需在包含噪声数据的空间中采样几千个点。然而，使用神经网络进行这项任务的代价是更长的收敛时间。另一个问题是一些 PDE 对原始配合方法存在问题，例如由于高阶导数的 Kuramoto-Sivashinsky (KS) 方程。没有弱形式方法，从数据中识别 KS 通常很困难，尤其是在噪声存在的情况下。更多的[近期发展](http://arxiv.org/abs/2309.04699)涉及将弱 SINDy 方法与神经网络配合方法结合。另一个有趣且实际未探讨的问题是这些方法通常如何受到非高斯噪声的影响。

# 结论

总结来说，方程发现是基于物理的机器学习的自然候选者，正在全球多个团队积极开发。它已在流体动力学、等离子体物理、气候等多个领域找到应用。有关其他方法的更广泛概述，请参见[综述文章](http://arxiv.org/abs/2305.13341)。希望读者对该领域存在的不同方法有所了解，但我只是略微触及了表面，避免过于技术化。值得一提的是许多新的基于物理的机器学习方法，如[神经常微分方程](/neural-odes-breakdown-of-another-deep-learning-breakthrough-3e78c7213795)（ODEs）。

参考文献

1.  Camps-Valls, G. *et al.* 从数据中发现因果关系和方程。*Physics Reports* **1044**，1–68 (2023)。

1.  Lam, R. *et al.* 学习高技能的中期全球天气预测。*Science* **0**，eadi2336 (2023)。

1.  Mehta, P. *et al.* 物理学家机器学习的高偏差、低方差介绍。*Physics Reports* **810**，1–124 (2019)。

1.  Schmidt, M. & Lipson, H. 从实验数据中提炼自由形式自然法则。*Science* **324**，81–85 (2009)。

1.  Udrescu, S.-M. & Tegmark, M. AI Feynman: 一种受物理启发的符号回归方法。*Sci Adv* **6**，eaay2631 (2020)。

1.  Ross, A., Li, Z., Perezhogin, P., Fernandez-Granda, C. & Zanna, L. 在理想化模型中对机器学习海洋子网格参数化的基准测试。*Journal of Advances in Modeling Earth Systems* **15**，e2022MS003258 (2023)。

1.  Brunton, S. L., Proctor, J. L. & Kutz, J. N. 通过稀疏识别非线性动态系统从数据中发现主方程。*Proceedings of the National Academy of Sciences* **113**，3932–3937 (2016)。

1.  Mangan, N. M., Kutz, J. N., Brunton, S. L. & Proctor, J. L. 通过稀疏回归和信息准则选择动态系统模型。*Proceedings of the Royal Society A: Mathematical, Physical and Engineering Sciences* **473**，20170009 (2017)。

1.  Rudy, S. H., Brunton, S. L., Proctor, J. L. & Kutz, J. N. 数据驱动的偏微分方程发现。*Science Advances* **3**，e1602614 (2017)。

1.  Messenger, D. A. & Bortz, D. M. 用于偏微分方程的弱SINDy。*Journal of Computational Physics* **443**，110525 (2021)。

1.  Gurevich, D. R., Reinbold, P. A. K. & Grigoriev, R. O. 对非线性PDE模型的鲁棒和最优稀疏回归。*Chaos: An Interdisciplinary Journal of Nonlinear Science* **29**，103113 (2019)。

1.  Reinbold, P. A. K., Kageorge, L. M., Schatz, M. F. & Grigoriev, R. O. 通过物理约束的符号回归从噪声、不完整、高维实验数据中进行鲁棒学习。*Nat Commun* **12**，3219 (2021)。

1.  Alves, E. P. & Fiuza, F. 从全动能模拟中数据驱动地发现简化的等离子体物理模型。*Phys. Rev. Res.* **4**，033192 (2022)。

1.  Zhang, S. & Lin, G. 具有误差条的数据驱动的物理定律发现。*皇家学会A卷：数学、物理和工程科学学报* **474**, 20180305 (2018)。

1.  Zanna, L. & Bolton, T. 数据驱动的海洋中尺度闭合方程发现。*地球物理研究快报* **47**, e2020GL088376 (2020)。

1.  Fasel, U., Kutz, J. N., Brunton, B. W. & Brunton, S. L. Ensemble-SINDy：在低数据、高噪声极限下，通过主动学习和控制实现稳健的稀疏模型发现。*皇家学会A卷：数学、物理和工程科学学报* **478**, 20210904 (2022)。

1.  Raissi, M., Perdikaris, P. & Karniadakis, G. E. 物理信息神经网络：解决涉及非线性偏微分方程的正向和逆向问题的深度学习框架。*计算物理学杂志* **378**, 686–707 (2019)。

1.  Markidis, S. 旧与新：物理信息深度学习能否取代传统线性求解器？*大数据前沿* **4**, (2021)。

1.  Camporeale, E., Wilkie, G. J., Drozdov, A. Y. & Bortnik, J. 数据驱动的Fokker-Planck方程发现：使用物理信息神经网络研究地球辐射带电子。*地球物理研究杂志：空间物理学* **127**, e2022JA030377 (2022)。

1.  Beucler, T. *等* 在模拟物理系统的神经网络中强制实施解析约束。*物理评论快报* **126**, 098302 (2021)。

1.  Both, G.-J., Choudhury, S., Sens, P. & Kusters, R. DeepMoD：在噪声数据中进行模型发现的深度学习。*计算物理学杂志* **428**, 109985 (2021)。

1.  Stephany, R. & Earls, C. PDE-READ：使用深度学习发现可读的偏微分方程。*神经网络* **154**, 360–382 (2022)。

1.  Both, G.-J., Vermarien, G. & Kusters, R. 稀疏约束神经网络用于PDE模型发现。预印本于 [https://doi.org/10.48550/arXiv.2011.04336](https://doi.org/10.48550/arXiv.2011.04336) (2021)。

1.  Stephany, R. & Earls, C. Weak-PDE-LEARN：一种基于弱形式的PDE发现方法，适用于噪声大、数据有限的情况。预印本于 [https://doi.org/10.48550/arXiv.2309.04699](https://doi.org/10.48550/arXiv.2309.04699) (2023)。
