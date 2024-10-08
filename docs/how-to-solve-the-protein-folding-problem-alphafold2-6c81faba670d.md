# 如何解决蛋白质折叠问题：AlphaFold2

> 原文：[`towardsdatascience.com/how-to-solve-the-protein-folding-problem-alphafold2-6c81faba670d`](https://towardsdatascience.com/how-to-solve-the-protein-folding-problem-alphafold2-6c81faba670d)

## 更深入地了解 AlphaFold2 及其神经网络结构

[](https://universvm.medium.com/?source=post_page-----6c81faba670d--------------------------------)![Leonardo Castorina](https://universvm.medium.com/?source=post_page-----6c81faba670d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6c81faba670d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c81faba670d--------------------------------) [Leonardo Castorina](https://universvm.medium.com/?source=post_page-----6c81faba670d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c81faba670d--------------------------------) ·阅读时间 16 分钟·2023 年 3 月 11 日

--

![](img/572e0dac09bea160681c694c2b4fb515.png)

蛋白质序列到形状的示意图。原始蛋白质用白色表示，AlphaFold 预测的蛋白质用彩虹色表示 [蓝色=最高信心，红色=最低信心]（图示作者提供）

在这一系列文章中，我将讨论蛋白质折叠及深度学习模型，如 AlphaFold、OmegaFold 和 ESMFold。我们将从 AlphaFold2 开始！

# 介绍

蛋白质是执行生物体大多数生化功能的分子。它们参与消化（酶）、结构过程（角蛋白 — 皮肤）、光合作用，并在制药行业中也被广泛使用 [2]。

蛋白质的 3D 结构对其功能至关重要。蛋白质由 20 种称为氨基酸（或残基）的子单位组成，每种氨基酸具有不同的特性，如电荷、极性、长度和原子数。氨基酸由所有氨基酸共有的**主链**和每种氨基酸特有的**侧链**组成。它们通过肽键相连 [2]。

![](img/bba2585b3f0247659bbdd5ef71269b05.png)

四个残基的蛋白质多肽示意图。（图示作者提供）

![](img/7a58223094f9764d6b7763d827e2ef4e.png)

四个残基的蛋白质主链示意图。（图示作者提供）

蛋白质包含在特定扭转角度 φ 和 ψ 定向的残基，这些角度决定了蛋白质的 3D 结构。

![](img/73f8bf2634b5620c888fa2894b3c79dd.png)

φ 和 ψ 角度的示意图。（图示作者提供）

每个生物学家面临的主要问题是获得蛋白质的三维形状，这通常需要蛋白质的晶体和 X 射线晶体学。蛋白质具有各种属性，例如膜蛋白往往是疏水性的，这意味着很难确定其结晶的条件 [2]。因此，获得晶体是一个繁琐且（可以说）高度随机的过程，需要几天到几十年不等，这可以被视为一种艺术而非科学。这意味着许多生物学家可能会在整个博士生期间致力于结晶一个蛋白质。

如果你有幸获得了蛋白质的晶体，你可以将其上传到蛋白质数据银行，这是一个大型的蛋白质数据集：

[## RCSB PDB: 主页](https://www.rcsb.org/?source=post_page-----6c81faba670d--------------------------------)

### 作为 wwPDB 的成员，RCSB PDB 根据约定的标准策划和注释 PDB 数据。RCSB PDB…

[www.rcsb.org](https://www.rcsb.org/?source=post_page-----6c81faba670d--------------------------------)

这引出了一个问题：我们能否模拟折叠以从序列中获得三维结构？简短回答：可以，有点。长答案：我们可以使用分子模拟来尝试折叠蛋白质，这通常计算量很大。因此，像 Folding@Home 这样的项目试图将问题分布到许多计算机上，以获得蛋白质的动态模拟。

现在，有一个名为蛋白质结构预测关键评估（CASP）的竞赛，其中一些蛋白质的三维结构被保留，以便人们可以测试他们的蛋白质折叠模型。在 2020 年，DeepMind 参与了 AlphaFold2，超越了最先进的技术，取得了杰出的表现。

![](img/1fb7e39f80c01653aa451b99ad04abb6.png)

2008 年至 2020 年 CASP 竞赛中的中位全球距离测试。大约 90 的解被认为与晶体结构大致相当。AlphaFold 2 超越了所有先前的模型，达到了最先进的性能（作者插图，基于 [4]）。

在这篇博客文章中，我将介绍 AlphaFold2，解释其内部工作原理，并总结它如何彻底改变了我作为蛋白质设计与机器学习博士生的工作。

在开始之前，我想给 AQ Laboratory 的[OpenFold](https://github.com/aqlaboratory/openfold)一个特别的感谢，这是一个开源的 AlphaFold 实现，包括训练代码，通过这些代码我双重检查了本文中提到的张量的维度。本文的大部分信息都在[原始论文的补充材料](https://www.nature.com/articles/s41586-021-03819-2)中。

# 0\. 概述

让我们从概述开始。这是模型的整体结构：

![](img/beeef945ce441d24c166bbf5078212dc.png)

AlphaFold 架构概述 [1]

通常，你会从目标蛋白质的氨基酸序列开始。请注意，获得氨基酸序列***不***需要晶体：通常可以通过 DNA 测序（如果你知道蛋白质的基因）或蛋白质测序获得。蛋白质可以被切割成较小的-mers，并通过质谱等方法进行分析。

目标是准备两个关键数据：**多序列比对（MSA）表示**和**对偶表示**。为了简化，我将跳过模板的使用。

**MSA 表示**是通过查找遗传数据库中的相似序列获得的。如图所示，序列也可能来自不同的生物体，例如鱼类。在这里，我们试图获取有关蛋白质每个索引位置的一般信息，并理解在进化的背景下，蛋白质在不同生物体中的变化情况。像 Rubisco（参与光合作用）的蛋白质通常高度保守，因此植物中的差异很小。其他蛋白质，如病毒的刺突蛋白，则变化很大。

在**对偶表示**中，我们尝试推断序列元素之间的关系。例如，蛋白质的位置 54 可能与位置 1 相互作用。

在整个网络中，这些表示会多次更新。首先，它们被嵌入以创建数据的表示。然后它们通过 EvoFormer，该模块提取序列和对的相关信息，最后通过结构模型构建蛋白质的 3D 结构。

# 1\. 输入嵌入器

+   算法：3

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/c2f46ce86367689864eac6a954dd9204b2576d3b/openfold/model/embedders.py#L24`](https://github.com/aqlaboratory/openfold/blob/c2f46ce86367689864eac6a954dd9204b2576d3b/openfold/model/embedders.py#L24)

输入嵌入器尝试创建数据的不同表示。对于 MSA 数据，AlphaFold 使用一个任意的簇编号而不是完整的 MSA，以减少经过变换器的可能序列数量，从而减少计算。MSA 数据输入*msa_feat* **(N_clust, N_res, 49)**由以下部分组成：

+   *cluster_msa* **(N_clust, N_res, 23)**：MSA 簇中心序列的独热编码（20 种氨基酸 + 1 个未知 + 1 个缺口 + 1 个*masked_msa_token*）

+   *cluster_profile* **(N_clust, N_res, 23)**：MSA 中每个残基的氨基酸类型分布（20 种氨基酸 + 1 个未知 + 1 个缺口 + 1 个*masked_msa_token*）

+   *cluster_deletion_mean* **(N_clust, N_res, 1)**：每个簇中每个残基的平均删除数量（范围 0–1）

+   *cluster_deletion_value* **(N_clust, N_res, 1)**：MSA 中的删除数量（范围 0–1）

+   *cluster_has_deletion* **(N_clust, N_res, 1)**：指示是否存在删除的二进制特征

对于对偶表示，它使用唯一索引对每个氨基酸进行编码，并使用 RelPos 计算序列中的距离。这被表示为每个残基之间的距离矩阵，距离限制为 32，这意味着较大的距离被限制为 0，因此维度有效为 -32 到 32 + 1 = **65**。

MSA 表示和对偶表示都经过几个独立的线性层，然后传递给 EvoFormer。

# 2\. Evoformer

![](img/028293a460f896b0dd328efb9359a97d.png)

EvoFormer 的架构 [1]

+   算法：6

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/evoformer.py#L575`](https://github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/evoformer.py#L575)

然后有 48 个 EvoFormer 块，使用自注意力允许 MSA 和对偶表示进行通信。我们首先查看 MSA，然后将其合并到对偶中。

## 2.1 MSA 堆叠

![](img/e6f3d06982a042b813eb04debd64b5a6.png)

EvoFormer 的 MSA 堆叠。 [1]（作者编辑）

这由 **行级门控自注意力与对偶偏置**、**列级门控自注意力**、**转换** 和 **外积均值** 块组成。

## **2.1A 行级门控自注意力与对偶偏置**

![](img/1caf06795f2a0f1c96cd37f9add06450.png)

Evoformer 中的行级门控自注意力与对偶偏置。 [1]

+   算法：7

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/msa.py#L290`](https://github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/msa.py#L290)

关键点在于允许 MSA 和对偶表示之间的信息交流。

首先，使用多头注意力从 MSA 表示 *行* 计算点积相似度 **(N_res, N_res, N_heads)**，这意味着序列中的氨基酸将学习“概念重要性”以识别对之间的关系。实质上，是一个氨基酸对另一个氨基酸的重要性。

然后，对偶表示经过一个没有偏置的线性层，这意味着只会学习一个权重参数。线性层输出 **N_heads** 维度，生成矩阵对偶偏置矩阵 **(N_res, N_res, N_heads)**。请记住，这个矩阵最初被限制在最大距离为 32，这意味着如果氨基酸之间的距离超过 32 个索引，它的值将为 0。

此时，我们有两个形状为 **(N_res, N_res, N_heads)** 的矩阵，我们可以轻松地将它们相加并应用 softmax，使值介于 0 和 1 之间。一个注意力块使用相加后的矩阵作为 *Queries*，并将一行通过线性层作为值，以获得注意力权重。

现在我们计算点积：

+   注意力权重和

+   MSA 行的线性+sigmoid 作为键（我相信此处的 sigmoid 操作返回一个范围为 0–1 的类似概率的数组）

## **2.1B 列式门控自注意力**

![](img/ac3b99d91110d38e38bff439bc56d145.png)

Evoformer 中的列式门控自注意力。[1]

+   算法：8

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/msa.py#L319`](https://github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/msa.py#L319)

这里的关键点是 MSA 是与输入序列相关的所有序列的对齐版本。这意味着索引 X 将对应于每个序列的蛋白质相同区域。

通过逐列执行此操作，我们确保对每个位置上哪些残基更可能有一个总体了解。这也意味着，如果一个类似的序列有微小的差异会产生相似的 3D 形状，模型将会更强健。

## **2.1C MSA 过渡**

![](img/674760d15b95e0626fa7e7c90696968a.png)

Evoformer 中的 MSA 过渡。[1]

+   算法：9

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/evoformer.py#L45`](https://github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/evoformer.py#L45)

这是一个简单的 2 层 MLP，首先将通道维度增加 4 倍，然后再缩小到原始维度。

## **2.1D 外积均值**

![](img/80fb65e42778df2a915439bbd3f8b581.png)

Evoformer 中的外积均值。[1]

+   算法：10

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/outer_product_mean.py#L27`](https://github.com/aqlaboratory/openfold/blob/b2d6bff691e0ad860d585ea6709b81b1aba57d2c/openfold/model/outer_product_mean.py#L27)

此操作旨在保持 MSA 和配对表示之间的信息连续流。MSA 中的每一列是蛋白质序列的一个索引位置。

+   在这里，我们选择索引 i 和 j，并将它们独立地通过一个线性层。这个线性层使用 c=32，这低于 c_m。

+   然后计算外积，取均值，展平，再通过另一个线性层。

我们现在在配对表示中有了更新的 ij 条目。我们对所有配对重复这一操作。

## 2.2 配对堆叠

![](img/69ad5293cb38f8bd90d81997f7c2db00.png)

EvoFormer 的配对堆叠。[1]

从技术上讲，我们的配对表示可以被解释为一个距离矩阵。之前，我们看到每种氨基酸都有 32 个邻居。因此，我们可以基于配对表示的三个索引构建一个三角图。

例如，节点 i、j 和 k 将有边 ij、ik 和 jk。每条边都通过它所在的所有三角形的其他两条边的信息进行更新。

![](img/6c87f676eedaae962a2e875acd388fa2.png)

三角乘法更新和三角自注意力。[1]

## **2.2A 三角乘法更新**

![](img/14d748b403eb188c6ef59ff604b3be47.png)

我们有两种类型的更新，一种用于输出边缘，一种用于输入边缘。

![](img/b3d2cb985f33e404ea6c08c22b405cf7.png)

三角乘法更新。[1]

+   算法：11 和 12

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/triangular_multiplicative_update.py#L28`](https://github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/triangular_multiplicative_update.py#L28)

对于**输出边缘**，完整的行或对表示 i 和 j 首先独立地通过一个线性层，生成左边缘和右边缘的表示。

然后，我们计算对应的 ij 对的表示与左边缘和右边缘的点积，分别进行。

最后，我们取左边缘和右边缘表示的点积，并与 ij 对表示进行最终点积。

对于**输入边缘**，算法非常类似，但请注意，如果之前我们考虑的是边缘 ik，现在我们需要考虑相反的方向 ki。在 OpenFold 代码中，这简单地实现为一个排列函数。

## **2.2B 三角自注意力**

![](img/9e5b7c80c3e2433ed5980310d199b998.png)![](img/7a3e5c06c8fbffb72e53f35bc46bd2d5.png)

三角自注意力。[1]

+   算法：13 和 14

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/triangular_attention.py#L31`](https://github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/triangular_attention.py#L31)

这个操作旨在通过自注意力更新对表示。主要目标是用最相关的边缘更新边缘，即蛋白质中哪些氨基酸更可能与当前节点相互作用。

通过自注意力，我们*学习*更新边缘的最佳方法：

+   （query-key）包含感兴趣节点的边缘之间的相似性。例如，对于节点 i，所有共享该节点的边缘（例如 ij，ik）。

+   第三条边（例如 jk），即使它不直接连接到节点 i，也属于三角形的一部分。

这个最后的操作在风格上类似于图消息传递算法，即使节点没有直接连接，来自图中其他节点的信息也会被加权并传递。

## **2.2C 过渡块**

相当于 MSA 主干中的过渡块，具有一个 2 层的 MLP，其中通道首先扩展 4 倍，然后减少到原始数量。

EvoFormer 块的输出是更新后的 MSA 和对的表示（具有相同的维度）。

# 3. 结构模块

![](img/11b5abb19f9cb2dbdb7d2c93d698c4b9.png)

AlphaFold 的结构模块。[1]

+   算法：20

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/structure_module.py#L515`](https://github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/structure_module.py#L515)

结构模块是模型的最终部分，将配对表示和输入序列表示（对应 MSA 表示中的一行）转换为 3D 结构。它由 8 层共享权重构成，配对表示用于在不变点注意力（IPA）模块中偏置注意力操作。

输出结果如下：

+   **背骨框架（r, 3x3）**：框架表示将原子位置从局部参考系变换到全球参考系的欧几里得变换。由 N-Cα-C 组成的自由漂浮体表示（蓝色三角形）；因此，每个残基（r_i）有三组（x、y、z）坐标

+   **χ** **侧链角度** (r , 3)：表示每个可旋转侧链原子的角度。这些角度定义了残基的旋转异构体（rotamer），因此可以推导出原子的确切位置。最多到**χ1、χ2、χ3、χ4**。

注意**χ**指的是每个可旋转键的二面角。有些较短的氨基酸没有所有四个χ角度，如下所示：

![](img/1dbf421f8a19c198e0bf7fa0959eccaf.png)

赖氨酸和酪氨酸的侧链角度。酪氨酸较短，没有**χ3、χ4**。（由作者插图）

## **3.1 不变点注意力（IPA）**

![](img/8f868b6d1391f860e71b8e5de141cd74.png)

结构模块的不变点注意力（IPA）。 [1]

+   算法：22

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/structure_module.py#L161`](https://github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/structure_module.py#L161)

通常，这种类型的注意力设计为对欧几里得变换（如平移和旋转）不变。

+   我们首先用自注意力更新单一表示，如前面部分所述。

+   我们还将每个残基的背骨框架信息输入以生成查询点、关键点和值点用于局部框架。这些点随后被投影到全球框架中，与其他残基交互后，再投影回局部框架。

+   “不变”一词指的是通过在 3D 空间中使用平方距离和坐标变换，强制全局和局部参考点保持不变。

## **3.2 预测侧链和背骨扭转角度**

单一表示经过几个 MLP，并输出扭转角度ω、φ、ψ、χ1、χ2、χ3、χ4。

## **3.3 背骨更新**

+   算法：23

+   OpenFold: [`github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/structure_module.py#L434`](https://github.com/aqlaboratory/openfold/blob/f84eb0a113b6b8f9c0bb022ea0796aa90ff016bd/openfold/model/structure_module.py#L434)

此块返回两个更新：一个是由四元数表示的**旋转**（1, a, b, c，其中第一个值固定为 1，a、b 和 c 对应于网络预测的欧拉轴），另一个是由向量矩阵表示的**平移**。

## **3.4 所有原子坐标**

此时，我们既有**骨架框架**又有**扭转角度**，我们希望获得氨基酸的确切原子坐标。氨基酸具有非常特定的原子结构，而我们有身份作为输入序列。因此，我们将扭转角度应用于氨基酸的原子。

注意，你会发现 AlphaFold 的输出中有许多结构违例，例如下面描述的情况。这是因为模型本身没有强制施加物理能量约束。为了解决这个问题，我们运行 AMBER 松弛力场来最小化蛋白质的能量。

![](img/f5216d56c7db49314c97407df88cec78.png)

在 AMBER 松弛之前，蛋氨酸和色氨酸被预测过于接近，形成了一个不可能的键。在 AMBER 松弛之后，旋转构象被调整以最小化能量和立体冲突。（插图作者提供）

# 其他优化

AlphaFold 模型包含多个自注意力层和由于 MSA 尺寸而产生的大量激活。经典的反向传播优化是减少每个节点的总计算量。然而，在 AlphaFold 的情况下，它会需要超过 TPU 核心的可用内存（16 GiB）。假设一个含有 384 个残基的蛋白质：

![](img/3e39ac8b417fff4f275a0d363aa0a8b9.png)

相反，AlphaFold 使用了梯度检查点（也称为再物化）。激活在每次一个层时被重新计算，从而将内存消耗降至约 0.4 GiB。

这个 GIF 显示了反向传播通常的样子：

![](img/e8d6816fa2e7013d2c1219385e99281b.png)

反向传播。（插图作者基于 [`github.com/cybertronai/gradient-checkpointing`](https://github.com/cybertronai/gradient-checkpointing) 提供）

通过检查点技术，我们减少了内存使用量，尽管这有一个不幸的副作用，即训练时间增加了 33%：

![](img/377a827f8ae0d036e482acd1be4fad13.png)

为检查点修复一个层。（插图作者基于: [`github.com/cybertronai/gradient-checkpointing`](https://github.com/cybertronai/gradient-checkpointing) 提供）

![](img/2fb60fef6b0d8c0864687165827c9803.png)

为检查点修复一个层。（插图作者基于 [`github.com/cybertronai/gradient-checkpointing`](https://github.com/cybertronai/gradient-checkpointing) 提供）

# 设计蛋白质呢？逆折叠问题

如果你有一个通过动态模拟设计的酷蛋白质模型，而不是一个氨基酸序列呢？或者一个你建模以结合另一个蛋白质（如 COVID 蛋白质刺突）的模型。理想情况下，你会想预测出折叠成一个输入的三维形状所需的序列，这个形状可能存在于自然界中，也可能是全新的蛋白质。我将向你介绍蛋白质设计的世界，这也是我博士项目 TIMED（三维推断方法高效设计）的内容：

![](img/156deb08ea678962e49380f45319cf81.png)

逆折叠问题（作者插图）

这个问题可以说比折叠问题更难，因为多个序列可以折叠成相同的形状。这是因为氨基酸类型存在冗余，并且蛋白质的某些区域对实际折叠的影响较小。

模型的输入是围绕每个氨基酸位置的骨干的网格化体素空间的立方体（“框架”）。Alpha 碳中心位于框架中，框架旋转，使 Alpha 碳到碳的键沿 x 轴。每个原子（C、N、O、alpha-C、beta-C）在不同的通道中进行独热编码，从而产生一个 4D 数组。

我们模型的输出是每个位置上所有氨基酸的概率分布。例如，在每个位置，模型输出一个氨基酸在该位置上的概率。对于一个 100 氨基酸的蛋白质，输出的形状为 (100, 20)，因为有二十种氨基酸：

![](img/21f8e109482c5a5353c658ed9d3dd1a5.png)

TIMED 的预测管道（作者插图）

AlphaFold 的一个酷炫之处在于我们可以利用它来双重检查我们的模型是否有效：

![](img/679cf66e35f1e07b75ddd76cfc2e8130.png)

折叠和逆折叠问题（作者插图）。

如果你想了解更多关于这个模型的信息，请查看我的 GitHub 仓库，那里还有一个小的 UI 演示！

![](img/92d5b7078415e1e58444f43dabc38af7.png)

TIMED 解决逆折叠问题的 UI 演示（作者插图）。GitHub: [`github.com/wells-wood-research/timed-design`](https://github.com/wells-wood-research/timed-design)

# 结论

在本文中，我们看到 AlphaFold 如何（部分）解决了一个生物学家明确的问题，即从氨基酸序列中获得三维结构。

我们将模型的结构分解为输入嵌入器、EvoFormer 和结构模块。每个模块使用多个自注意力层，并包含许多优化性能的技巧。

AlphaFold 工作良好，但这就是生物学的全部吗？不。AlphaFold 仍然计算上非常昂贵，并且使用起来不容易（不，Google Colab 不容易——它很笨重）。一些替代方案，如 OmegaFold 和 ESMFold，试图解决这些问题。

这些模型仍未解释蛋白质如何随时间折叠。还有许多挑战涉及设计蛋白质，其中逆折叠模型可以利用 AlphaFold 进行双重检查，以确保设计的蛋白质折叠成特定的形状。

在接下来的系列文章中，我们将探讨 OmegaFold 和 ESMFold！

# 参考文献

[1] Jumper J, Evans R, Pritzel A, Green T, Figurnov M, Ronneberger O, Tunyasuvunakool K, Bates R, Žídek A, Potapenko A, 等. [使用 AlphaFold 进行高精度蛋白质结构预测](https://www.nature.com/articles/s41586-021-03819-2)。 《自然》(2021) DOI: 10.1038/s41586–021–03819–2

[2] Alberts B. [细胞的分子生物学](https://www.ncbi.nlm.nih.gov/books/NBK21054/)。 (2015) 第六版。纽约，NY：Garland Science，Taylor 和 Francis Group。

[3] Ahdritz G, Bouatta N, Kadyan S, Xia Q, Gerecke W, O’Donnell TJ, Berenberg D, Fisk I, Zanichelli N, Zhang B, 等. [OpenFold: 重新训练 AlphaFold2 提供了关于其学习机制和能力的新见解](https://www.biorxiv.org/content/10.1101/2022.11.20.517210v1.full) (2022) 生物信息学。DOI: 10.1101/2022.11.20.517210

[4] Callaway E. [“这将改变一切”：DeepMind 的 AI 在解决蛋白质结构方面取得了巨大飞跃](https://www.nature.com/articles/d41586-020-03348-4) (2020)。《自然》588(7837):203–204\. DOI: 10.1038/d41586–020–03348–4

# 进一步阅读

原始的 AlphaFold 论文：

[](https://www.nature.com/articles/s41586-021-03819-2?source=post_page-----6c81faba670d--------------------------------) [## 使用 AlphaFold 进行高精度蛋白质结构预测 - 自然

### 蛋白质对生命至关重要，理解它们的结构可以促进对其机制的理解…

www.nature.com](https://www.nature.com/articles/s41586-021-03819-2?source=post_page-----6c81faba670d--------------------------------)

OpenFold 论文和代码：

[](https://www.biorxiv.org/content/10.1101/2022.11.20.517210v2?source=post_page-----6c81faba670d--------------------------------) [## OpenFold: 重新训练 AlphaFold2 提供了关于其学习机制和能力的新见解

### AlphaFold2 通过能够以极高的精度预测蛋白质结构，彻底改变了结构生物学…

www.biorxiv.org](https://www.biorxiv.org/content/10.1101/2022.11.20.517210v2?source=post_page-----6c81faba670d--------------------------------) [](https://github.com/aqlaboratory/openfold?source=post_page-----6c81faba670d--------------------------------) [## GitHub - aqlaboratory/openfold: 可训练、内存高效且支持 GPU 的 PyTorch 重现…

### 图：OpenFold 和 AlphaFold2 预测与 PDB 7KDX，链 B 的实验结构的比较。A…

github.com](https://github.com/aqlaboratory/openfold?source=post_page-----6c81faba670d--------------------------------)

关于 AlphaFold 的另一篇写得很好的文章：

[](https://dauparas.github.io/post/af2/?source=post_page-----6c81faba670d--------------------------------) [## AlphaFold 2 & 等变性 | Justas Dauparas

### Fabian Fuchs & Justas Dauparas 几周前，在最新的 CASP 蛋白质结构预测竞赛中（…

dauparas.github.io](https://dauparas.github.io/post/af2/?source=post_page-----6c81faba670d--------------------------------)

关于蛋白质（序列）设计：

[](https://github.com/wells-wood-research/timed-design?source=post_page-----6c81faba670d--------------------------------) [## GitHub - wells-wood-research/timed-design：使用深度学习和工具进行蛋白质序列设计…

### timed-design 是一个用于蛋白质序列设计模型和分析预测的库。我们展示了重新训练的 Keras…

github.com](https://github.com/wells-wood-research/timed-design?source=post_page-----6c81faba670d--------------------------------) [](https://academic.oup.com/bioinformatics/article/39/1/btad027/6986968?source=post_page-----6c81faba670d--------------------------------) [## PDBench：评估蛋白质序列设计的计算方法

### 蛋白质设计的目标是创建具有有用特性和功能的新型氨基酸序列。一项重要的…

academic.oup.com](https://academic.oup.com/bioinformatics/article/39/1/btad027/6986968?source=post_page-----6c81faba670d--------------------------------)

# 作者寄语

[](https://pub.towardsai.net/latent-diffusion-explained-simply-with-pok%C3%A9mon-3ebe15a3a019?source=post_page-----6c81faba670d--------------------------------) [## 潜在扩散简单解释（以宝可梦为例）

### 从文本到图像、图像到图像和修复——潜在扩散的革命

pub.towardsai.net](https://pub.towardsai.net/latent-diffusion-explained-simply-with-pok%C3%A9mon-3ebe15a3a019?source=post_page-----6c81faba670d--------------------------------) [](https://betterhumans.pub/obsidian-tutorial-for-academic-writing-87b038060522?source=post_page-----6c81faba670d--------------------------------) [## Obsidian 学术写作教程

### 手稿写作、海报制作和将引用导出到 Word、PDF 和 LaTeX 的实用写作技巧。

betterhumans.pub](https://betterhumans.pub/obsidian-tutorial-for-academic-writing-87b038060522?source=post_page-----6c81faba670d--------------------------------) [](https://betterhumans.pub/how-to-boost-your-productivity-for-scientific-research-using-obsidian-fe85c98c63c8?source=post_page-----6c81faba670d--------------------------------) [## 如何利用 Obsidian 提升科研生产力

### 管理你的 Zettelkasten、项目、阅读列表、笔记和在博士研究期间的灵感的工具和工作流。

[betterhumans.pub](https://betterhumans.pub/how-to-boost-your-productivity-for-scientific-research-using-obsidian-fe85c98c63c8?source=post_page-----6c81faba670d--------------------------------) [](https://betterhumans.pub/20-macos-apps-to-boost-your-productivity-74accb372c9c?source=post_page-----6c81faba670d--------------------------------) [## 20 多个 MacOS 应用程序提升你的生产力

### 一系列 20 多个应用程序，能显著提升你在 MacOS 上的生产力。

[betterhumans.pub](https://betterhumans.pub/20-macos-apps-to-boost-your-productivity-74accb372c9c?source=post_page-----6c81faba670d--------------------------------)
