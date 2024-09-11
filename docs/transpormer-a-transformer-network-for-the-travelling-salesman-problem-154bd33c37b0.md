# TranSPormer: 一种解决旅行商问题的 Transformer 网络

> 原文：[https://towardsdatascience.com/transpormer-a-transformer-network-for-the-travelling-salesman-problem-154bd33c37b0?source=collection_archive---------8-----------------------#2023-05-02](https://towardsdatascience.com/transpormer-a-transformer-network-for-the-travelling-salesman-problem-154bd33c37b0?source=collection_archive---------8-----------------------#2023-05-02)

## 利用 Transformer 神经网络来解决 TSP 的一种原创方法

[](https://medium.com/@davidecaffagni98?source=post_page-----154bd33c37b0--------------------------------)[![Davide Caffagni](../Images/a7ef4b6d21ea0564d43180a50a84280a.png)](https://medium.com/@davidecaffagni98?source=post_page-----154bd33c37b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----154bd33c37b0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----154bd33c37b0--------------------------------) [Davide Caffagni](https://medium.com/@davidecaffagni98?source=post_page-----154bd33c37b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb628be49d740&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftranspormer-a-transformer-network-for-the-travelling-salesman-problem-154bd33c37b0&user=Davide+Caffagni&userId=b628be49d740&source=post_page-b628be49d740----154bd33c37b0---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----154bd33c37b0--------------------------------) ·11分钟阅读·2023年5月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F154bd33c37b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftranspormer-a-transformer-network-for-the-travelling-salesman-problem-154bd33c37b0&user=Davide+Caffagni&userId=b628be49d740&source=-----154bd33c37b0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F154bd33c37b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftranspormer-a-transformer-network-for-the-travelling-salesman-problem-154bd33c37b0&source=-----154bd33c37b0---------------------bookmark_footer-----------)![](../Images/19ab14273299f2d8a77fd057abc70be5.png)

照片由 [Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=medium&utm_medium=referral) 提供，拍摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

旅行商问题（TSP）是组合优化中的经典挑战之一。虽然这个问题已经研究了很长时间，目前仍没有已知的精确方法可以保证最优解。随着人工智能领域的进步，出现了新的解决TSP的提议。本文中，我们利用变换器神经网络来寻找这个问题的良好解决方案。

代码仓库：[GitHub](https://github.com/dcaffo98/transpormer)

上述代码库是对我在MCS期间完成的[之前项目](https://github.com/bizza251/adm-project)的重构，以通过自动决策课程考试。我想感谢我的朋友和同事Alessandro，他在项目中进行了协作。

# 背景 — TSP

> 给定一个城市列表以及每对城市之间的距离，什么是访问每个城市恰好一次并返回起始城市的最短可能路线？（来源：[Wikipedia](https://en.wikipedia.org/wiki/Travelling_salesman_problem)）

上述问题定义了旅行商问题（TSP），这是*NP-困难*的难题之一，意味着目前没有已知的方法可以在多项式时间内得到最优解。更正式地说，给定一个图***G****(****V****,* ***E****)*，其中***V***是节点的集合（城市）而***E***是连接***V***中任意一对节点的边的集合，我们需要在***G***中找到最短的哈密顿巡回路（即*“访问每个城市恰好一次并返回起始城市的路线”*）。

在这个项目中，我们将一个节点视为一个简单的特征对，即2D向量空间中的x和y位置。因此，任何一对节点*(u, v)*之间存在一条边，其权重由*u*和*v*之间的欧几里得距离给出。

# 背景 — 用于TSP的变换器

我们从 Bresson *et al.* [1] 的工作中获得灵感，他们利用变换器神经网络来解决 TSP，取得了非常有前景的结果。简而言之，他们的模型分为两个组件。首先，变换器**编码器**以一组*标记*作为输入，即表示输入图中的节点的向量，并通过自注意力层将它们转换到一个新的潜在空间。我们将编码器的输出称为**图嵌入**。随后，变换器**解码器**接收一个虚拟标记***z***，该标记指示模型开始构建旅行路径，加上来自编码器的图嵌入，并生成一个概率分布，建模图中每个节点被选为旅行路径下一个候选节点的可能性。我们从该分布中抽取对应于所选节点的索引*t*。然后，我们将***z***与第*t*个图嵌入连接，并再次将解码器喂入结果序列，该序列表示部分旅行路径。这个过程持续进行，直到所有可用节点都被选中。完成后，我们从获得的序列中移除***z***，并将第一个选中的节点附加到序列末尾，从而完成旅行路径。这样的解码器被称为**自回归**，因为在每个选择步骤中，它使用的是前一步的输出。该模型通过 REINFORCE [2] 算法进行训练，以最小化从随机图中生成的旅行路径的平均长度，每个图有 50 个节点。

# 提议的方法

![](../Images/998284862fd6de205dcd2100cc1e346f.png)

[Bresson 等人](https://doi.org/10.48550/arXiv.2103.03012)（右）和我们（左）提出的架构概述。请注意，我们的网络没有自回归组件，即输出和输入之间没有反馈连接。作者提供的照片

在这个项目中，我们也采用变换器架构，因为我们旨在利用注意力来学习图中节点之间的有价值的全局依赖关系。同时，我们希望设计一个**避免任何自回归组件**的模型，即一个直接输出可行 TSP 解决方案的神经网络，无需对每个节点进行单次前向传递。

我们首先考虑将 TSP（旅行商问题）框定为一个集合到序列的问题。我们有一堆节点，其**顺序**尚未定义，我们希望找出如何排序以使得得到的旅行路径尽可能最短。在神经网络中表示顺序的概念通常通过某种形式的**位置编码** [3] 来完成。也就是说，我们将输入集合中的每个标记与一个特定的向量相加，该向量的分量对特定位置是唯一的。因此，如果我们改变标记的顺序，位置向量也会不同，从而导致标记也不同。当然，位置编码有很多可能的实现方法，但为了简单起见，我们坚持使用原始提议。

我们方法的核心是注意力操作符，特别是交叉注意力，因此在这里我们对其进行简要回顾。

![](../Images/d6dcec872a4d403138a7d2d3602c4d37.png)

三步解释注意力。作者提供的照片

关键步骤是第二步。***A***是一个*[n,n]*矩阵，其*(i,j)*项是一个实数，与第*i*个标记相对于第*j*个标记的**余弦相似度**成比例。为了理解这一点，回顾步骤-2，*(i,j)*是查询*i*和关键*j*的点积的结果，两者都是形状为*(d_k)*的两个向量。但是，点积实际上就是两个向量之间夹角的余弦，除了由两个向量的范数的乘积表示的缩放因子。

![](../Images/93edee1b52df6300a62f77f27deb81f0.png)

两个向量之间余弦的公式。作者提供的照片

在对***A***应用softmax（第三步）之后（实际上是对***A***的缩放版本，但这不是重要的内容），我们得到一个矩阵，其*i*行是一个概率分布，建模查询*i*与关键*j*=1，…，*n*相似的可能性。最后，***A***对***V***应用线性变换，通过查询和关键之间的相似性对其*d=1，…k*特征加权。例如，假设***x_0***、***x_1***和***x_2***是代表纽约（NY）、华盛顿特区（WDC）和洛杉矶（LA）的标记。它们的特征与这三个大都市的地理位置有关。我们从中提取***Q***、***K***和***V***的三个矩阵，并计算注意力矩阵***A***。由于NY-WDC之间的距离远小于NY-LA，***A***的第一行将在其第一列中具有非常高的值（因为NY-NY的距离自然最短），第二列中具有中高值，第三列中具有较低值。NY在矩阵***V***中也由第一行表示，即*v_0*。嗯，在***AV***矩阵乘法之后，*v_0*的特征将主要通过第一行（NY）的A进行加权，适当地通过第二行（WDC），但在第三行（LA）方面贡献较少。其他两座城市也是类似的过程。我们的原始标记集已经转变为一组新的向量集，其特征受到（在这种情况下是空间的）相似性加权。

这种注意力被称为**自注意力**，因为***Q***, ***K***和***V***都来自相同的输入。另一方面，在transformer解码器中，还有第二种类型的注意力，**交叉注意力**。这时，查询***Q***来自解码器本身。例如，在Bresson等人的模型中，***Q***表示部分巡回。***K***和***V***则是编码器输出的线性投影。

在这项工作中，我们提出了不同地利用交叉注意力。实际上，我们使用位置编码作为查询，而键和值则从之前的自注意力层中提取，该层可以自由操作***G***的所有节点。这样，当我们计算注意力矩阵时，我们得到的矩阵中，每一行是给定位置与给定节点相似性的概率分布。以下图片可能有助于理解这一点。

![](../Images/43d727b6d4f7cce04326875a10b95064.png)

可视化我们模型中的交叉注意力。作者照片

根据上述注意力矩阵，节点 #1 可能会成为提议旅行中的第一个节点。#46 可能是第二个节点。节点 #39 和 #36 将分别放在第三和第四的位置。节点 #23 是第五和第六位置的好候选者，依此类推……在实践中，这样的矩阵元素 *(i,j)* 表示节点 *j* 被插入到提议旅行中位置 *i* 的可能性。我们的模型本质上是一个堆叠的块，每个块包含一个自注意力层、所展示的交叉注意力层，以及最后的前馈网络，如任何变换器。最后一个交叉注意力层的注意力矩阵将用于生成旅行。我们的模型生成的完整注意力矩阵大致如下：

![](../Images/16a0a21694a50f985f103c5a07f07805.png)

未训练的注意力矩阵，权重随机初始化。作者照片

这显然是相当混乱的。在使用REINFORCE算法优化网络以最小化平均旅行长度后，我们得到的矩阵如下。

![](../Images/e188dee466ca32bccc81ac0552ee34c3.png)

训练后的注意力矩阵。作者照片

从上述矩阵开始，我们可以为每一行绘制一个节点，结果序列将是我们的旅行（在附加第一个节点以*闭合*它之后）。为了简单起见，在我们所有的实验中，我们使用贪婪解码来选择节点，即我们总是选择当前行中值最高的节点。我们将探索诸如束搜索算法等其他策略留给未来的发展。

# 可行的解决方案

你可能已经发现了我们方法的主要问题。要构建一个可行的旅行，我们需要**仅选择一次**每个节点。相反，如果你仔细观察前面的图片，你会注意到有一些重复。即，节点 #47 最有可能出现在两个连续的位置，以及节点 #32。就目前而言，我们的网络无法提供可行的TSP解决方案。我们希望从模型中获得的是一个**置换矩阵**，即每行和每列中只有一个值为1的矩阵。问题是神经网络不擅长预测这种稀疏对象。

为了克服这一障碍，我们可以将TSP公式化为线性 [sum assignment](https://en.wikipedia.org/wiki/Hungarian_algorithm) (LSA) 问题，其（逆向）成本矩阵是由我们的变换器计算的注意力矩阵。在这项工作中，我们利用了 [SciPy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linear_sum_assignment.html) 的实现来寻找节点集合（典型LSA公式中的*工人*）和位置集合（*工作*）之间的最小成本匹配。

# Sinkhorn算子

自然地，LSA的解决方案，其成本由置换矩阵给出，是直接的：你只需选择矩阵中每行的*1s*。因此，我们训练的目标是使最终的注意力矩阵尽可能接近置换矩阵，以便LSA将是简单的，并且将导致表示总长度较短的旅行的匹配。

在我们的项目中，我们利用Sinkhorn算子 [4] 将我们模型的密集注意力矩阵转化为软置换矩阵：它们并不真正代表置换，但非常接近。接下来，我展示了一个典型训练设置的过程。我们模型提出的解决方案的平均旅行长度在训练过程中预计会减少。如果我们在解决LSA问题之前停止应用Sinkhorn，这种情况不会发生。

![](../Images/b1f606e40d074bdc8f835d5356c01123.png)

Sinkhorn算子在我们方法中的影响。作者提供的照片

# 结果与比较

我们将我们的提议与Christofides [5]的算法进行比较，这是TSP的一个流行启发式方法，同时还参考了Bresson等人的自回归变换器，我们也将其作为训练设置的参考。评估是在10000个包含50个节点的图上进行的，这些节点的坐标是随机生成的。

在这里，我们提供一个箱线图，展示三位竞争者提出的解决方案的长度。毫无疑问，我们的变换器在这个背景下表现最差。利用神经网络生成良好的TSP解决方案而不利用自回归是一项艰巨的任务。

![](../Images/9c110a610494482c04a362ade19e5f33.png)

三种比较方法的TSP解决方案长度。作者提供的照片

然而，解决优化问题不仅仅关乎性能。计算时间也扮演着基本角色。这是我们提案的主要特点：由于我们的变压器只需要一次前向传递即可提供解决方案，我们比Bresson *et al.*的方法更快，因为他们必须循环多次，以便完成整个路线。下图显示了计算时间，用Python的[分析器](https://docs.python.org/3.9/library/profile.html#module-cProfile)测量。对于Christofides，我们使用了[NetworkX](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.approximation.traveling_salesman.traveling_salesman_problem.html)的实现。请注意，为了公平比较，这两个神经网络都在CPU上运行，因为Christofides并未考虑在GPU上运行。

![](../Images/7984ba907e8ccdea861eb87bf3e8359b.png)

解决TSP实例的CPU计算时间。照片由作者提供

最后，我们展示了我们网络产生的最佳路线的一些定性结果。在这些情况下，我们的模型能够超越Christofides生成的解决方案。

![](../Images/ba5ce202ad4bc5af7fcd41698179e0cb.png)![](../Images/41422af2cb9c27982f181794a11e603a.png)![](../Images/727c7e2d4550634e6579bd64dab419ed.png)![](../Images/ec2279e1a4032b521e0a5f4c8cda5f0e.png)![](../Images/498f36854b3e2da4a3f09e8b5f23818e.png)![](../Images/b5f7ed25eece0a78f185d3f01a4feb0e.png)![](../Images/5f0d9208e09945634eeb58de28efaf6a.png)![](../Images/91ff78fca37d4c28bbc1ca56f86aa914.png)![](../Images/1ec91cbba98f7b137b902880ce13a86f.png)![](../Images/9632311a76e3b586a5b9de04316bbe5a.png)

定性结果：Christofides的[TSP解决方案](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.approximation.traveling_salesman.traveling_salesman_problem.html)（左）和我们变压器的（右）。照片由作者提供

# 结论与未来工作

在这项工作中，我们提出了一种原始的神经网络来解决TSP。虽然我们成功设计了一种避免自回归的合适架构，但所提出的路线质量却值得怀疑。这种方法的主要缺点是它不是一种端到端的神经方法，因为我们需要解决一个LSA实例以确保得到可行的解。我们尝试通过在损失函数中添加一些惩罚项来强制模型避免节点重复，但没有获得良好的结果。从一个集合中学习排列本身就是一个困难的任务。

可能，仅仅从简单的贪心解码切换到束搜索策略就足以略微改善结果。此外，还存在更先进的强化学习算法可供探索，例如[PPO](https://arxiv.org/abs/1707.06347)。我们希望其他人能受到这个项目的启发，继续探索这条路径。

## 参考文献

[1] Bresson, Xavier 和 Thomas Laurent. “[旅行商问题的变压器网络。](https://doi.org/10.48550/arXiv.2103.03012)” *arXiv 预印本 arXiv:2103.03012* (2021)

[2] Williams, Ronald J. “[连接主义强化学习的简单统计梯度跟随算法。](https://link.springer.com/content/pdf/10.1023/A:1022672621406.pdf)” *强化学习* (1992): 5–32

[3] Vaswani, Ashish 等. “[注意力机制就是一切。](https://doi.org/10.48550/arXiv.1706.03762)” *神经信息处理系统进展* 30 (2017)

[4] Adams, Ryan Prescott 和 Richard S. Zemel. “[通过 Sinkhorn 传播进行排名。](https://doi.org/10.48550/arXiv.1106.1925)” *arXiv 预印本 arXiv:1106.1925* (2011)

[5] Christofides, Nicos. [*一种新启发式方法的最坏情况分析*。](http://dx.doi.org/10.1007/s43069-021-00101-z) 卡内基梅隆大学匹兹堡管理科学研究组, 1976
