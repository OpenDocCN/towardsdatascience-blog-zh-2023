# 使用Graphcore IPU和PopSparse库加速块稀疏矩阵乘法

> 原文：[https://towardsdatascience.com/accelerating-block-sparse-matrix-multiplication-with-graphcore-ipu-and-the-popsparse-library-3791284a2127?source=collection_archive---------10-----------------------#2023-04-12](https://towardsdatascience.com/accelerating-block-sparse-matrix-multiplication-with-graphcore-ipu-and-the-popsparse-library-3791284a2127?source=collection_archive---------10-----------------------#2023-04-12)

## **使用新库PopSparse加速稀疏矩阵乘法**

[](https://medium.com/@dominic.masters.gc?source=post_page-----3791284a2127--------------------------------)[![多米尼克·马斯特斯](../Images/a4bb210cb0a19c81941c50a1e07a11b6.png)](https://medium.com/@dominic.masters.gc?source=post_page-----3791284a2127--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3791284a2127--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3791284a2127--------------------------------) [多米尼克·马斯特斯](https://medium.com/@dominic.masters.gc?source=post_page-----3791284a2127--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb23ad1d05ffe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerating-block-sparse-matrix-multiplication-with-graphcore-ipu-and-the-popsparse-library-3791284a2127&user=Dominic+Masters&userId=b23ad1d05ffe&source=post_page-b23ad1d05ffe----3791284a2127---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3791284a2127--------------------------------) ·10分钟阅读·2023年4月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3791284a2127&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerating-block-sparse-matrix-multiplication-with-graphcore-ipu-and-the-popsparse-library-3791284a2127&user=Dominic+Masters&userId=b23ad1d05ffe&source=-----3791284a2127---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3791284a2127&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faccelerating-block-sparse-matrix-multiplication-with-graphcore-ipu-and-the-popsparse-library-3791284a2127&source=-----3791284a2127---------------------bookmark_footer-----------)

作者：**李智怡**，AI工程师，**道格拉斯·奥尔**，研究科学家，**瓦勒留·奥汉**，软件工程师，以及 [**多米尼克·马斯特斯**](https://medium.com/u/b23ad1d05ffe?source=post_page-----3791284a2127--------------------------------)，研究科学家

![](../Images/5b70b53b06a68360136d4264b945137d.png)

图片由作者提供。

鉴于深度学习模型规模的显著增长以及人工智能在广泛应用中的普及，减少运行这些模型的计算成本的激励比以往任何时候都更大。

一种显示出显著前景的减少这些成本的方法是使用*稀疏性*。稀疏性通常指通过*剪枝*过程在模型权重中引入尽可能多的零，然后在实际计算时跳过这些值。

尽管在利用稀疏性减少理论成本指标（如FLOPs和参数计数）方面取得了很大成功，但在保持可接受任务性能的同时实现实际速度提升要困难得多，特别是在使用低精度数字格式的通用加速器（如GPU）上。

为了在实践中实现这些理论上的好处，并进一步研究在Graphcore IPUs上的稀疏技术，我们推出了PopSparse，这是一种通过利用IPUs的独特硬件特性以及数据中定义的任何块结构来实现快速稀疏操作的库。我们针对两种不同类型的稀疏性：静态稀疏性，即稀疏模式在编译时固定；和动态稀疏性，即每次运行模型时可以改变。我们在IPU上对这两种模式的稀疏稠密矩阵乘法进行了基准测试。结果表明，PopSparse的实现比IPU上的稠密矩阵乘法在各种稀疏水平、大矩阵尺寸和块尺寸下都更快。此外，静态稀疏性通常优于动态稀疏性。虽然以前在GPU上的研究仅在非常高的稀疏性（通常为99%及以上）下显示出加速效果，但我们展示了Graphcore的静态稀疏实现比在较低稀疏性（约90%）下的等效稠密计算表现更好。

这些结果已在线发布在[arXiv](https://arxiv.org/abs/2303.16999)上，并被[2023年ICLR Sparse Neural Network工作坊](https://www.sparseneural.net/?)接受。

# **Graphcore IPU用于深度神经网络中的稀疏稠密矩阵乘法**

深度学习中的稀疏性概念最常指的是通过稀疏化模型权重来减少相关的存储和计算成本。通常，这通过在训练结束时的单次剪枝步骤（Zhu & Gupta, 2017）或通过某些稀疏训练机制（Evci et al., 2019）来实现。这些方法通常在保持可接受的准确度水平的同时，实现了模型权重减少90–99%的效果（Hoefler et al., 2021）。

尽管模型大小的好处很容易实现，但提高计算效率通常更为困难，因为产生的无结构稀疏模式与高度并行的深度学习加速器中的矢量化指令集不太匹配（Qin 等人，2022）。因此，提高稀疏计算效率的一种方法是对稀疏模式施加一定程度的结构。这促使了广泛的结构化剪枝技术，如神经元（Ebrahimi & Klabjan，2021）、通道级（He 等人，2017）、块（Gray 等人，2017）、2:4（Mishra 等人，2021）。然而，这些方法通常会比等效的无结构方法带来性能惩罚。

在这里，我们介绍 PopSparse，这是一个用于 Graphcore IPU 上稀疏稠密矩阵乘法（SpMM）有效加速的库。IPU 拥有多个架构特性，有助于加速稀疏操作：

+   大型、高带宽的片上 SRAM 可提高通信密集型、低算术效率操作的性能。

+   细粒度 MIMD 处理模型将工作划分到 1472 个独立计算单元（瓦片）上，支持高度的并行性。

+   提前编译模型，当稀疏性结构在编译时已知时，可以采用进一步优化。

# **PopSparse 库中的 SpMM 在 IPU 上**

SpMM 可以写作：

![](../Images/803733b50cd0ffffee604b7f742cf57b.png)

其中 ⊙ 表示元素逐一相乘，* 表示内积，*Y ∈ Rᵐ* ˣ*ⁿ*，*X ∈ Rᵏ* ˣ*ⁿ* 分别是稠密的输出和输入矩阵。稀疏权重矩阵 (*M*⊙*W*) 通过 *M ∈ Bᵐ* ˣ*ᵏ (B = {*0,1*})* 定义，这是一种表示稀疏性模式的掩码，来自 *M̂* ∈ *B*⁽ᵐᐟᵇ⁾ˣ⁽ᵏᐟᵇ⁾，块掩码和 *W ∈ Rᵐ* ˣ*ᵏ* 定义权重值。

在这种表述中，(*M*⊙*W*) 具有块稀疏结构，其中形状为 *b* × *b* 的连续方形权重块包含所有非零元素，给定块大小 *b*。维度 *m* 和 *k* 分别称为输出和输入特征大小，而 *n* 被称为批量大小，对应于其在权重稀疏神经网络计算中的典型角色。

我们定义密度，*d* = ∑ᵢⱼMᵢⱼ/(*m⋅k*)，其中标准术语稀疏度对应于 *(*1 *— d)*。我们使用浮点运算（FLOP）计数来量化 SpMM 中有用的算术工作量为：2*⋅m⋅k⋅n⋅d*。注意，这只计算非零值上的操作，并不依赖于块大小 *b*。PopSparse 库支持无结构（块大小 *b* = 1）或有结构的稀疏性（*b* = 4, 8, 16）。

## 静态稀疏性

在静态稀疏性中，编译时，稀疏模式和矩阵形状对分区器是已知的，分区器将稀疏矩阵（具体值尚未确定）的非零元素在 *k* 维度上划分为 *qₖ* 个分区，将密集矩阵在 *n* 维度上划分为 *qₙ* 个分区。图 1a 说明了静态稀疏情况中稀疏矩阵的分配、计算和减少。*k* 维度上的分区不需要均匀大小，而是选择以确保非零元素（图中用彩色小块表示）的平衡分布。模型编译后，在执行期间提供 *W* 的具体非零值。由于稀疏模式已知，无需额外的数据交换。然后执行与密集矩阵的局部点积计算，并在块间进行最终的归约，以给出密集输出矩阵。

## 动态稀疏性

在动态稀疏性中，稀疏操作数的非零元素的最大数量是固定的，而稀疏模式在执行期间会更新。为了最小化计算周期，我们在编译时使用规划工具来优化分区，同时适应执行过程中可以定义的所有稀疏模式。由于每次运行时稀疏模式可能会改变，与静态稀疏性相比，分区器需要额外的步骤来分配稀疏矩阵索引和非零值。这在图 1b 中得到了演示。此外，由于在 *m* 和 *k* 维度上需要固定大小的分区，因此每个分区中的非零元素可能不平衡。在图 1b 中，第二个 *k* 维度分区中的 *B* 元素无法适应 Tile1\。三个 *B* 元素溢出到 Tile2\。然而，Tile2 并没有包含所有与 B 元素对应的分区信息（*m, k, n* 切片），因此无法计算结果。因此，需要额外的传播步骤来完成计算。

总体而言，我们的动态稀疏实现相比于静态稀疏情况有几个额外的成本：

+   编译后的额外控制流会带来一定的成本开销。

+   我们必须针对最大的块间通信量进行编译，因此平均效率较低。

+   当数据不平衡时，需要额外的传播和计算阶段来重新平衡数据。所需的步骤数量取决于不平衡的程度。

![](../Images/9d7f8e5ea83338795d555ffeee22d8ca.png)

图 1：输入特征维度 *k* 的静态和动态稀疏分区示意图。图像由作者提供。

# **基准测试**

我们的微基准测试实验探索了 IPU 和 GPU 上的单一稀疏-密集矩阵乘法 SpMM: *Y=(M*⊙*W)***X*。IPU 实验和 GPU 基线使用的 API 如表 1 所示。基准测试的参数范围详见表 2。

![](../Images/2b827a018386682c4aacf6b98a025ea8.png)

表 1：基准测试 IPU 和 GPU 时使用的 API。

**IPU**：该操作在 Bow-2000 机箱中的单个 Bow IPU 上执行一次，使用随机生成的稀疏模式和数值。我们提取周期计数信息，并在 1.85 GHz 的固定时钟下将这些周期计数转换为 TFLOP/s 值。主机传输不计入测量时间。

**GPU**：GPU 上的基线稀疏实现由 cuSPARSE 库 v11.6.2（NVIDIA, 2022）提供。这些实现根据使用的稀疏格式进行分类，不区分静态和动态稀疏模式。我们考虑压缩稀疏行（CSR）和块稀疏行（BSR）分别用于非结构化和块稀疏。我们生成随机稀疏模式和数值，复制到设备上，执行 25 次操作迭代，并将结果复制到主机进行验证。我们在 5 次迭代后开始计时，并使用 *cudaEventRecord* 测量壁钟时间。所有实验都在一个 A100 GPU 上进行，该 GPU 运行在 4 x A100-SXM4–40G 机箱中。

![](../Images/3d9b4f3d6e2d0f3bcbf2bb005fec005e.png)

表 2：基准测试中扫取的参数范围。图像作者提供。

本研究的关键结果如图 2 所示。这些图表显示了计算 FLOP/s（对数尺度）与密度的关系，特征大小固定，每条线代表相同操作的不同实现。由于零在 FLOP/s 计算中不被计算，密集实现随着密度的增加呈线性扩展。而在稀疏实现中，完美的扩展应预测 FLOP/s 在密度变化时保持不变。

![](../Images/efcdb9f5f8a9a9edd15a098060cf823b.png)

图 2：IPU FP16 和 GPU 块稀疏矩阵乘法性能在不同块大小 b 的密度变化下，平方特征大小 m = k = 4096，以最佳批量大小 n 为准。图像作者提供。

对于密集基线，虽然我们大致看到 IPU 和 GPU 在 FP16 上的芯片对芯片相等，但重要的是要强调所使用的 IPU 的价格大约是 [云端价格的一半](https://docs.paperspace.com/gradient/machines/)，突显了其在这些基本操作中的成本效益。此外，对于稀疏操作，我们看到使用 IPU 的好处更大。

具体而言，对于 IPU（图 2a），我们看到使用静态稀疏操作和块稀疏都能单独提高性能。例如，块大小为 16 的动态情况在所有考虑的密度下相较于密集基线显示出优势，而在低密度区域（<5%）中，非结构化（b = 1）静态稀疏优于密集稀疏。此外，当同时使用静态和块稀疏方法时，我们看到吞吐量相比密集基线提高了多达 5 倍。

对于 GPU（图 2b），我们看到稀疏操作的性能远不如预期，没有任何测试用例超过 FP16 密集基线。然而，我们发现 FP32 块稀疏（BSR）情况在密度降低时扩展非常好。看起来对于非常低的密度，这可能会超过密集基线。

比较两个平台，我们可以看到 IPU 相比于密集 GPU 基线在吞吐量上实现了高达 5 倍的收益，而在仅考虑稀疏操作时则提高了高达 25 倍的吞吐量。考虑到额外的价格优势，我们认为这为在 IPU 上调查稀疏应用提供了有力的理由。

然而需要注意的是，即使在 IPU 上，稀疏操作也仅在某些条件下超过密集基线。更多详细结果可以在 [我们的论文](https://arxiv.org/abs/2303.16999) 中找到，但作为大致指南，当以下条件满足时，稀疏性通常能在 IPU（FP16，两个模型的大批量）上带来比密集基线更快的速度：

+   静态稀疏性，块大小 b = 1，特征 (m = k) ≥ 4096，密度 d ≤ 1/32。

+   静态稀疏性，块大小 b ≥ 4：特征 (m = k) ≥ 4096，密度 d ≤ 1/8。

+   动态稀疏性，块大小 b ≥ 8，特征 (m = k) ≥ 4096，密度 d ≤ 1/32。

重要的是，这些范围在今天的权重稀疏训练和推理中有些难以使用。这主要是因为找到不会降低性能的块稀疏模式是一个困难的问题，并且在理论效率指标（每个非零 FLOP）上，无结构稀疏总是显得和块稀疏一样好或更好。我们希望这项工作表明，块稀疏是一种有前景的方法，可以实现稀疏模型的实际加速，并将激励研究社区对有效的块稀疏剪枝算法进行进一步的研究。

来 [尝试我们的 PopSparse 操作](https://console.paperspace.com/github/graphcore-research/notebooks?container=graphcore%2Fpytorch-jupyter%3A3.2.0-ubuntu-20.04&machine=Free-IPU-POD4&file=%2Fsparsity_benchmarks%2FSpMM.ipynb) 在 Paperspace 的 IPUs 上，然后为什么不试试将它们集成到自己的模型中呢？如果你正在积极从事稀疏性研究，并且希望获得 IPU 支持，请考虑注册我们的 [学术计划](https://www.graphcore.ai/academics)，我们在这里支持研究人员利用 IPUs 实现卓越的成果。

# 自己尝试一下

尝试在 IPU 上使用 SpMM — [在 Paperspace 上运行](https://console.paperspace.com/github/graphcore-research/notebooks?container=graphcore%2Fpytorch-jupyter%3A3.2.0-ubuntu-20.04&machine=Free-IPU-POD4&file=%2Fsparsity_benchmarks%2FSpMM.ipynb)。

## **参考文献**

[1] Michael Zhu 和 Suyog Gupta. 剪枝，还是不剪枝：探索剪枝在模型压缩中的有效性，2017\. 网址 [https://arxiv.org/abs/1710.01878.](https://arxiv.org/abs/1710.01878.)

[2] Utku Evci, Trevor Gale, Jacob Menick, Pablo Samuel Castro 和 Erich Elsen. 改变彩票：让所有票据都成为赢家，2019\. URL [https://arxiv.org/abs/1911.11134.](https://arxiv.org/abs/1911.11134.)

[3] Torsten Hoefler, Dan Alistarh, Tal Ben-Nun, Nikoli Dryden 和 Alexandra Peste. 深度学习中的稀疏性：神经网络中的剪枝与增长以实现高效推理和训练，2021\. URL [https://arxiv.org/abs/2102.00554](https://arxiv.org/abs/2102.00554).

[4] Eric Qin, Raveesh Garg, Abhimanyu Bambhaniya, Michael Pellauer, Angshuman Parashar, Sivasankaran Rajamanickam, Cong Hao 和 Tushar Krishna. 通过异质性实现稀疏张量加速的灵活性，2022\. URL [https://arxiv.org/abs/2201.08916.](https://arxiv.org/abs/2201.08916.)

[5] Abdolghani Ebrahimi 和 Diego Klabjan. 基于神经元的深度神经网络剪枝，通过克罗内克因子曲率近似实现更好的泛化，2021\. URL [https://arxiv.org/abs/2111.08577.](https://arxiv.org/abs/2111.08577.)

[6] Yihui He, Xiangyu Zhang 和 Jian Sun. 通过通道剪枝加速非常深的神经网络，2017\. URL [https://arxiv.org/abs/1707.06168.](https://arxiv.org/abs/1707.06168.)

[7] Scott Gray, Alec Radford 和 Diederik P. Kingma. 用于块稀疏权重的GPU内核。2017.

[8] Asit Mishra, Jorge Albericio Latorre, Jeff Pool, Darko Stosic, Dusan Stosic, Ganesh Venkatesh, Chong Yu 和 Paulius Micikevicius. 加速稀疏深度神经网络，2021\. URL [https://arxiv.org/abs/2104.08378](https://arxiv.org/abs/2104.08378).

[9] NVIDIA. cuSPARSE的API参考指南，2022\. URL [https://docs.nvidia.com/cuda/cusparse/](https://docs.nvidia.com/cuda/cusparse/).
