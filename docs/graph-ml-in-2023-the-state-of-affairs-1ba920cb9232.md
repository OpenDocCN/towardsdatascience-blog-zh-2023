# 图形机器学习在2023年的现状

> 原文：[https://towardsdatascience.com/graph-ml-in-2023-the-state-of-affairs-1ba920cb9232?source=collection_archive---------0-----------------------#2023-01-01](https://towardsdatascience.com/graph-ml-in-2023-the-state-of-affairs-1ba920cb9232?source=collection_archive---------0-----------------------#2023-01-01)

## 最前沿动态

## 热点趋势和重大进展

[](https://mgalkin.medium.com/?source=post_page-----1ba920cb9232--------------------------------)[![Michael Galkin](../Images/c5eb13334712ca0462d8a5df4a268ad0.png)](https://mgalkin.medium.com/?source=post_page-----1ba920cb9232--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1ba920cb9232--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1ba920cb9232--------------------------------) [Michael Galkin](https://mgalkin.medium.com/?source=post_page-----1ba920cb9232--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4d4f8ddd1e68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgraph-ml-in-2023-the-state-of-affairs-1ba920cb9232&user=Michael+Galkin&userId=4d4f8ddd1e68&source=post_page-4d4f8ddd1e68----1ba920cb9232---------------------post_header-----------) 发布在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----1ba920cb9232--------------------------------) · 26分钟阅读 · 2023年1月1日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1ba920cb9232&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgraph-ml-in-2023-the-state-of-affairs-1ba920cb9232&user=Michael+Galkin&userId=4d4f8ddd1e68&source=-----1ba920cb9232---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1ba920cb9232&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgraph-ml-in-2023-the-state-of-affairs-1ba920cb9232&source=-----1ba920cb9232---------------------bookmark_footer-----------)

2022年已经结束，是时候坐下来回顾一下在图形机器学习（Graph ML）方面取得的成就，并对2023年的可能突破进行假设了。敬请关注 🎄☕

![](../Images/f8888b219ca9e135881455e3596c98d7.png)

背景图像由 [DALL-E 2](https://openai.com/dall-e-2/) 生成，文本由作者添加。

*这篇文章由* [*Hongyu Ren*](http://hyren.me/) *(斯坦福大学)、* [*Zhaocheng Zhu*](https://kiddozhu.github.io/) *(Mila 和蒙特利尔大学)共同撰写。我们感谢* [*Christopher Morris*](https://chrsmrrs.github.io/) *和* [*Johannes Brandstetter*](https://www.microsoft.com/en-us/research/people/johannesb/) *在理论和偏微分方程部分的反馈和帮助。请关注* [*Michael*](https://twitter.com/michael_galkin)*、* [*Hongyu*](https://twitter.com/ren_hongyu)*、* [*Zhaocheng*](https://twitter.com/zhu_zhaocheng)、* [*Christopher*](https://twitter.com/chrsmrrs)*和* [*Johannes*](https://twitter.com/jo_brandstetter) *在Medium和Twitter上，以获取更多与图机器学习相关的讨论。*

**目录：**

1.  [生成模型：分子和蛋白质的去噪扩散模型](#48f6)

1.  [DFTs、机器学习力场、材料和天气模拟](#d2e8)

1.  [几何学与拓扑学与偏微分方程](#6d20)

1.  [图变换器](#8e6c)

1.  [大型图](#ca19)

1.  [GNN理论：Weisfeiler和Leman的前景，子图GNN](#7986)

1.  [知识图谱：归纳推理接管](#e5e6)

1.  [算法推理和对齐](#b2f5)

1.  [酷炫的GNN应用](#0de4)

1.  [硬件：IPU和Graphcore赢得OGB LSC 2022](#b813)

1.  [新的会议：LoG 和分子机器学习](#9b59)

1.  [课程和教育材料](#41dc)

1.  [新的数据集、基准和挑战](#3e6d)

1.  [软件库和开源](#463f)

1.  [加入社区](#1b30)

1.  [2022年的网络迷因](#7593)

# 生成模型：分子和蛋白质的去噪扩散模型

生成扩散模型在视觉语言领域是2022年深度学习世界的头条话题。尽管生成图像和视频无疑是尝试不同模型和采样技术的酷炫领域，我们认为

> 在2022年，扩散模型最*有用*的应用实际上是在几何深度学习领域中创建的，重点关注分子和蛋白质

在我们最近的文章中，我们在思考是否 [“去噪扩散模型就是你所需的一切？”](/denoising-diffusion-generative-models-in-graph-ml-c496af5811c5)。

[](/denoising-diffusion-generative-models-in-graph-ml-c496af5811c5?source=post_page-----1ba920cb9232--------------------------------) [## 去噪扩散生成模型在图机器学习中的应用

### 去噪扩散模型就是你所需的一切吗？

towardsdatascience.com](/denoising-diffusion-generative-models-in-graph-ml-c496af5811c5?source=post_page-----1ba920cb9232--------------------------------)

在那里，我们回顾了最新的生成模型用于*图生成*（DiGress）、*分子构象生成*（EDM、GeoDiff、Torsional Diffusion）、*分子对接*（DiffDock）、*分子连接*（DiffLinker）和*配体生成*（DiffSBDD）。一旦帖子公开，几种令人惊叹的蛋白质生成模型也随之发布：

[**Chroma**](https://www.generatebiomedicines.com/chroma)来自Generate Biomedicines，允许施加功能性和几何约束，甚至可以使用自然语言查询，比如“生成一个具有CHAD结构域的蛋白质”，这要归功于一个小型的GPT-Neo，经过蛋白质标注的训练；

![](../Images/7e24c508e8b597a6205035e92bd02e6d.png)

*Chroma蛋白质生成。来源：* [*Generate Biomedicines*](https://www.generatebiomedicines.com/chroma)

[**RoseTTaFold Diffusion**](https://www.bakerlab.org/2022/11/30/diffusion-model-for-protein-design/)（RF Diffusion）来自Baker Lab和MIT，具有类似功能，还支持文本提示，如“生成一个能够结合X的蛋白质”，并且能够进行功能性基序支架、酶活性位点支架和*de novo*蛋白质设计。强项：用RF Diffusion生成的1000种设计在实验室中[被合成和测试](https://twitter.com/DaveJuergens/status/1601675072175239170)！

![](../Images/d6ce018a9c6d17df43d48a15da42dce6.png)

*RF Diffusion。来源：* [*Watson等*](https://www.biorxiv.org/content/10.1101/2022.12.09.519842v1) *BakerLab*

Meta AI FAIR团队在蛋白质设计领域通过语言模型取得了惊人的进展：2022年中，[**ESM-2**](https://www.biorxiv.org/content/10.1101/2022.07.20.500902v1)发布了，这是一种仅在蛋白质序列上训练的蛋白质语言模型，远超ESM-1及其他基准模型。而且，后来显示编码的语言模型表示是获得蛋白质实际几何结构的非常好的起点，而无需多重序列比对（MSAs）——这可以通过[**ESMFold**](https://www.biorxiv.org/content/10.1101/2022.07.20.500902v1)实现。特别感谢Meta AI和FAIR发布了该模型及其权重：它在[官方GitHub仓库](https://github.com/facebookresearch/esm)和[HuggingFace](https://huggingface.co/models?other=esm)上也可以找到！

![](../Images/6d7f2f02320b2b61008ce8bb1973205d.png)

扩展ESM-2可以获得更好的折叠预测。来源：[Lin, Akin, Rao, Hie等](https://www.biorxiv.org/content/10.1101/2022.07.20.500902v1)

🍭 随后，来自ESM团队的更多好消息传来：[Verkuil等](https://www.biorxiv.org/content/10.1101/2022.12.21.521521v1)发现ESM-2可以生成*de novo*蛋白质序列，这些序列实际上可以在实验室中合成，并且更重要的是，它们在已知的自然蛋白质中没有任何匹配。[Hie等](https://www.biorxiv.org/content/10.1101/2022.12.21.521526v1)提出了一种全新的蛋白质设计编程语言（可以把它看作是ESMFold的查询语言）——生产规则以约束函数的语法树形式组织。然后，每个程序被“编译”为一个控制生成过程的能量函数。Meta AI还发布了最大的[Metagenomic Atlas](https://esmatlas.com/)，但更多内容请参见本文的**数据集**部分。

在抗体设计领域，**IgLM** 采用了类似的基于 LM 的方法，如 [Shuai、Ruffolo 和 Gray](https://www.biorxiv.org/content/10.1101/2021.12.13.472419v2) 所述。IGLM 生成基于链和物种 ID 标签的抗体序列。

最后，我们要特别提到 Jian Tang 实验室在 Mila 的一些工作。由 [Liu 等人](https://arxiv.org/abs/2212.10789) 提出的 **MoleculeSTM** 是一个类似 CLIP 的文本到分子模型（以及一个新的大型预训练数据集）。MoleculeSTM 能做到两件令人印象深刻的事情：（1）通过文本描述如“噻唑衍生物”检索分子，并从给定的 SMILES 分子中检索文本描述；（2）根据文本提示进行分子编辑，如“使分子在水中溶解且渗透性低”——模型根据描述编辑分子图谱，令人瞠目结舌 🤯

然后，[Shi 等人](https://arxiv.org/abs/2210.08761) 提出的 **ProtSEED** 是一个同时生成蛋白质序列 *和* 结构的模型（例如，大多数现有的蛋白质扩散模型一次只能处理其中之一）。ProtSEED 可以基于残基特征或残基对进行条件设置。从模型的角度看，它是一个具有改进的三角注意力的对称迭代模型。ProtSEED 在抗体 CDR 共同设计、蛋白质序列-结构共同设计和固定骨架序列设计中进行了评估。

![](../Images/e54ef61a84ca2aeca7a1b707387881e2.png)

从文本输入中进行分子编辑。来源：[Liu 等人](https://arxiv.org/abs/2212.10789)

除了生成蛋白质结构外，还有一些工作致力于从结构生成蛋白质序列，这被称为逆折叠。不要忘记查看 Meta 的 [ESM-IF1](https://www.biorxiv.org/content/10.1101/2022.04.10.487779v2) 和 Baker 实验室的 [ProteinMPNN](https://www.science.org/doi/full/10.1126/science.add2187)。

> **2023 年的预期**：（1）扩散模型的性能改进，例如更快的采样和更高效的求解器；（2）更强大的条件蛋白质生成模型；（3）对 [生成流网络](https://arxiv.org/abs/2111.09266)（GFlowNets，查看 [教程](https://milayb.notion.site/The-GFlowNet-Tutorial-95434ef0e2d94c24aab90e69b30be9b3)）在分子和蛋白质中的更成功应用。

# **DFTs、ML 力场、材料和天气模拟**

AI4Science 成为对称 GNN 研究及其应用的前沿。通过将 GNN 与 PDEs 配对，我们现在可以处理更复杂的预测任务。

> 2022 年，这一前沿领域扩展到了用于 **分子动力学** 和 **材料发现** 的基于 ML 的 **密度泛函理论**（DFT）和 **力场** 近似。另一个不断增长的领域是 **天气模拟**。

我们推荐 Max Welling 的 [讲座](https://www.youtube.com/watch?v=t7q_ZNrBghY)，以获得关于 AI4Science 的更广泛概述，以及深度学习在科学中的应用现状。

从模型开始，2022年见证了等变GNN在分子动力学和模拟中的激增，例如，基于[NequIP](https://arxiv.org/abs/2101.03164)、由[Musaelian、Batzner等人](https://arxiv.org/abs/2204.05249)提出的**Allegro**或由[Batatia et al.](https://arxiv.org/abs/2206.07697)提出的**MACE**。这些模型的设计空间非常大，因此可以参考[Batatia、Batzner等人](https://arxiv.org/abs/2205.06643)的最新综述以获得概述。对于大多数模型来说，一个关键组件是[**e3nn**](https://github.com/e3nn/e3nn)库（[Geiger和Smidt](https://arxiv.org/abs/2207.09453)的论文）和张量积的概念。我们强烈推荐Erik Bekkers提供的一个出色的[新课程](https://uvagedl.github.io/)，以理解数学基础并跟上最新论文。

⚛️ **密度泛函理论**（DFT）计算是分子动力学的主要工具之一（并且在大型集群中占据了大量计算时间）。然而，DFT的时间复杂度是O(n³)，那么机器学习能否有所帮助呢？在*学习到的力场已准备好用于基态催化剂发现*中，[Schaarschmidt et al.](https://arxiv.org/abs/2209.12466)展示了学习势能模型的实验研究——结果表明GNNs在O(n)线性时间内可以表现得非常好！**Easy Potentials**方法（基于Open Catalyst数据训练）证明是一个非常好的预测器，特别是与后处理步骤配合时。模型方面，它是一个带有[Noisy Nodes](https://arxiv.org/abs/2106.07971)自监督目标的MPNN。

在**《力量不足》**中，[Fu et al.](https://arxiv.org/abs/2210.07237)提出了一个新的分子动力学基准——除了MD17，作者们还添加了液体（水）、肽（丙氨酸二肽）和固态材料（LiPS）的数据集。更重要的是，作者们考虑了广泛的物理属性，如模拟的稳定性、扩散性和径向分布函数。包括SchNet、ForceNet、DimeNet、GemNet（-T和-dT）以及NequIP在内的大多数SOTA分子动力学模型都被探讨了。

![](../Images/b1ffbec28f6b6c061b8f5f66ff7fdc9e.png)

来源: [Fu et al.](https://arxiv.org/abs/2210.07237)

在晶体结构建模中，我们要特别提到由[Kaba和Ravanbakhsh](https://openreview.net/forum?id=0Dh8dz4snu)提出的**等变晶体网络**——一种用来构建具有晶体对称性的周期性结构表示的巧妙方法。晶体可以用*晶格*和*单位胞*来描述，基向量可以进行群变换。概念上，ECN创建了与对称群对应的边索引掩码，对这些掩码索引进行消息传递，并聚合多个对称群的结果。

![](../Images/e6422b43252d3ad7ca97e55de20b9931.png)

来源: [Kaba和Ravanbakhsh](https://openreview.net/forum?id=0Dh8dz4snu)

关于材料发现的更多消息可以在最近的 [AI4Mat NeurIPS workshop](https://sites.google.com/view/ai4mat) 会议记录中找到！

☂️ 基于机器学习的天气预报也取得了巨大的进展。特别是，DeepMind 的 [**GraphCast**](https://arxiv.org/abs/2212.12794) 和华为的 [**Pangu-Weather**](https://arxiv.org/abs/2211.02556) 展示了出色的结果，大大超越了传统模型。虽然 Pangu-Weather 利用 3D/视觉输入和视觉变换器，GraphCast 则采用了网格 MPNN，其中地球被分割为多个层级的网格。最深层有大约 40K 节点，具有 474 个输入特征，模型输出 227 个预测变量。MPNN 遵循“编码器-处理器-解码器”结构，并具有 16 层。GraphCast 是一个自回归模型，相对于下一个时间步预测，即它利用前两个状态预测下一个状态。GraphCast 可以在单个 TPUv4 上在 <60 秒内建立 10 天的预测，并且比非机器学习预测模型准确得多。👏

![](../Images/642385acd4eeb71b13131b45b29d0697.png)

GraphCast 中的编码器-处理器-解码器网格 MPNN。来源：[Lam, Sanchez-Gonzalez, Willson, Wirnsberger, Fortunato, Pritzel 等](https://arxiv.org/abs/2212.12794)

> **2023年的期待**：我们期望看到更多关注 GNN 的计算效率和可扩展性。目前基于 GNN 的力场取得了显著的准确性，但仍比经典力场慢 2-3 个数量级，并且通常只部署在几百个原子上。为了使 GNN 在材料科学和药物发现中真正发挥变革性影响，我们将看到许多人致力于解决这个问题，无论是通过架构进步还是更智能的采样方法。

# 几何学 & 拓扑学 & PDEs

在 2022 年，1️⃣ 我们对 GNN 中的过度平滑和过度压缩现象及其与代数拓扑的关系有了更好的理解；2️⃣ 使用 GNN 进行 PDE 建模现在已经成为主流。

1️⃣ Michael Bronstein 的实验室对这个问题做出了巨大贡献 —— 查看那些关于神经切片扩散和将 GNN 视为梯度流的优秀帖子。

[](/neural-sheaf-diffusion-for-deep-learning-on-graphs-bfa200e6afa6?source=post_page-----1ba920cb9232--------------------------------) [## 神经切片扩散在图上的深度学习

### 细胞切片理论，作为代数拓扑的一个分支，为图神经网络的工作原理提供了新的见解…

[towardsdatascience.com](/neural-sheaf-diffusion-for-deep-learning-on-graphs-bfa200e6afa6?source=post_page-----1ba920cb9232--------------------------------)

以及关于 GNN 作为梯度流的内容：

[](/graph-neural-networks-as-gradient-flows-4dae41fb2e8a?source=post_page-----1ba920cb9232--------------------------------) [## 图神经网络作为梯度流

### GNN 作为梯度流的衍生物，通过最小化描述吸引力和排斥力的可学习能量来优化…

[towardsdatascience.com](/graph-neural-networks-as-gradient-flows-4dae41fb2e8a?source=post_page-----1ba920cb9232--------------------------------)

2️⃣ 使用 GNNs 进行 PDE 建模已成为主流话题。一些论文需要 🤯 **数学警报** 🤯 提示，但如果你对 ODEs 和 PDEs 的基础知识很熟悉，这应该会更容易。

*消息传递神经 PDE 求解器* 由 [Brandstetter, Worrall, 和 Welling](https://openreview.net/forum?id=vSix3HPYKSU) 描述了消息传递如何帮助解决 PDE，具有更好的泛化能力，并摆脱手动启发式。此外，MP-PDEs 在表示上包含经典求解器，如有限差分。

![](../Images/4e9be60b522337c75d2f485d5e19ed8d.png)

来源: [Brandstetter, Worrall, 和 Welling](https://openreview.net/forum?id=vSix3HPYKSU)

该主题通过许多近期工作得到了进一步发展，包括使用隐式神经表示进行连续预测 ([Yin et al.](https://arxiv.org/abs/2209.14855))、支持混合边界条件 ([Horie and Mitsume](https://openreview.net/forum?id=B3TOg-YCtzo))，或 PDE 的潜在演变 ([Wu et al.](https://arxiv.org/abs/2206.07681))

> **2023 年的期待**：神经 PDE 及其应用有可能扩展到更多与物理相关的 AI4Science 子领域，特别是计算流体动力学（CFD）在未来几个月可能会受到基于 GNN 的替代品的影响。经典的 CFD 被广泛应用于多个领域的研究和工程问题，包括空气动力学、高超声速和环境工程、流体流动、视频游戏中的视觉效果，或上述天气模拟。基于 GNN 的替代品可能会增强/替代传统的经过验证的技术，如有限元方法 ([Lienen et al.](https://arxiv.org/abs/2203.08852))、重网格算法 ([Song et al.](https://arxiv.org/abs/2204.11188))、边值问题 ([Loetsch et al.](https://arxiv.org/abs/2206.14092))，或与三角化边界几何的交互 ([Mayr et al.](https://arxiv.org/abs/2106.11299))。
> 
> 神经 PDE 社区正在开始建立强大且常用的基准和框架，这将反过来帮助加速进展，例如 **PDEBench** ([Takamoto et al.](https://arxiv.org/abs/2210.07182)) 或 **PDEArena** ([Gupta et al.](https://arxiv.org/abs/2209.15616))

# 图神经网络变换器

绝对是 2022 年的主要社区推动力之一，**图神经网络变换器**（GTs）在有效性和可扩展性方面有了很大进展。2022 年发布了几个杰出的模型：

**👑 GraphGPS** 由 [Rampášek et al.](https://arxiv.org/abs/2205.12454) 提出的，因其结合了局部消息传递、全局注意力（可选地，线性以提高效率）和位置编码，最终在 ZINC 和许多其他基准上设立了新的 SOTA 标杆，被誉为 **“2022 年的 GT”**。查看有关 GraphGPS 的专门文章

[](/graphgps-navigating-graph-transformers-c2cc223a051c?source=post_page-----1ba920cb9232--------------------------------) [## GraphGPS: 导航图形变换器

### 最佳图形变换器的烹饪秘诀

towardsdatascience.com](/graphgps-navigating-graph-transformers-c2cc223a051c?source=post_page-----1ba920cb9232--------------------------------)

GraphGPS 作为 **GPS++** 的核心，[获胜](https://ogb.stanford.edu/neurips2022/results/#winners_pcqm4mv2) OGB 大规模挑战赛 2022 模型在 PCQM4M v2（图回归）上表现突出。**GPS++**，[由](https://arxiv.org/abs/2212.02229) Graphcore、Valence Discovery 和 Mila 创建，整合了更多特性，包括 3D 坐标，并利用了稀疏优化的 IPU 硬件（更多信息见下一节）。GPS++ 权重已在 GitHub 上[可用](https://github.com/graphcore/ogb-lsc-pcqm4mv2)！

![](../Images/030d6d10617c7e1782ce0d078c915c21.png)

GraphGPS 直观理解。来源：[Rampášek et al](https://arxiv.org/abs/2205.12454)

**Transformer-M** 由 [Luo et al.](https://arxiv.org/abs/2210.01765) 提出的方法也启发了许多顶级 OGB LSC 模型。Transformer-M 将 3D 坐标添加到整洁的 2D-3D 预训练混合中。在推理时，当 3D 信息未知时，该模型将推断出一部分 3D 知识，从而显著提高 PCQM4Mv2 的性能。代码[可用](https://github.com/lsj2408/Transformer-M)。

![](../Images/2b5d5841cfa0a2d34817e2a1d077b999.png)

*Transformer-M 2D-3D 预训练方案。来源：* [*Luo et al.*](https://arxiv.org/abs/2210.01765)

**TokenGT** 由 [Kim et al](https://arxiv.org/abs/2207.02505) 提出的方法更加明确，并将输入图的所有边（除了所有节点）添加到喂给 Transformer 的序列中。使用这些输入，编码器需要额外的标记类型来区分节点和边。作者证明了几个很好的理论性质（尽管代价是较高的计算复杂度 O((V+E)²)，在完全连接图的最坏情况下可能达到四次方）。代码[可用](https://github.com/jw9730/tokengt)。

![](../Images/362b38baa863661789419ac3384b6568.png)

TokenGT 将节点和边都添加到输入序列中。来源：[Kim et al](https://arxiv.org/abs/2207.02505)

> **2023 年的预期**：在来年，我们预计 1️⃣ GTs 在数据和模型参数两个维度上扩展，从小于 50 节点的分子到数百万节点的图，以见证如文本和视觉基础模型中的突现行为 2️⃣ 类似于 [BLOOM](https://huggingface.co/bigscience/bloom) 由 BigScience Initiative 提供的用于分子数据的大型开源预训练等变 GT，也许在 [Open Drug Discovery](https://m2d2.io/opendrugdiscovery/) 项目中。

# 大图

🔥 我们在 2022 年最喜欢的一篇是[Chamberlain, Shirobokov 等](https://arxiv.org/abs/2209.15486)的 *“基于子图草图的链接预测图神经网络*”——这是算法与 ML 技术的巧妙结合。众所周知，[SEAL](https://arxiv.org/pdf/2010.16103.pdf) 类的标记技巧相比于标准 GNN 编码器可以显著提升链接预测性能，但却面临巨大的计算/内存开销。在这项工作中，作者发现利用哈希 ([MinHashing](https://en.wikipedia.org/wiki/MinHash)) 和基数估计 ([HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog)) 算法，可以高效地获取查询边两个节点之间的距离。本质上，消息传递是在 *minhashing* 和 *hyperloglog* 单节点的初步草图（*min* 聚合用于 minhash，*max* 用于 hyperloglog 草图）上进行的——这就是**ELPH** 链接预测模型的核心（带有简单的 MLP 解码器）。然后，作者设计了一个更具可扩展性的 **BUDDY** 模型，其中 k-hop 哈希传播可以在训练前预计算。实验表明，ELPH 和 BUDDY 可以扩展到以前过于庞大或资源消耗过大的图中。出色的工作，绝对是未来所有链接预测模型的坚实基线！👏

![](../Images/0a1e1985708d039dbfd156a1aef9ac06.png)

计算子图哈希以估计邻域和交集基数的动机。来源：[Chamberlain, Shirobokov 等](https://arxiv.org/abs/2209.15486)

在图采样和迷你批处理方面，[Gasteiger, Qian, 和 Günnemann](https://openreview.net/forum?id=b9g0vxzYa_) 设计了[**基于影响力的迷你批处理 (IBMB)**](https://github.com/tum-daml/ibmb)，这是个很好的例子，说明个性化PageRank (PPR) 如何解决图批处理问题！IBMB 旨在创建对节点分类任务影响最大的最小迷你批次。实际上，影响力分数等同于 PPR。实际上，给定一组目标节点，IBMB (1) 将图划分为永久集群，(2) 在每个批次内运行 PPR，选择 top-PPR 节点以形成最终的子图迷你批次。生成的迷你批次可以发送到任何 GNN 编码器。IBMB 对图的大小几乎是**常数** O(1)，因为分区和 PPR 可以在预处理阶段预计算。

尽管生成的批次是固定的，并且在训练过程中不会改变（不够随机），但作者设计了类似动量的优化项来缓解这种非随机性。IBMB 可以在训练和推理中使用，速度提升可达 17 倍和 130 倍 🚀

![](../Images/79a47281579302493940b6b733e1b1c3.png)

基于影响力的迷你批处理。来源：[Gasteiger, Qian, 和 Günnemann](https://openreview.net/forum?id=b9g0vxzYa_)

本小节的副标题可以是“*由谷歌提供*”，因为大多数论文的作者都来自谷歌 ;)

[Carey等](https://openreview.net/pdf?id=q5h7Ywx-sS) 创建了***Stars***，一种在**数十万亿**边的规模下构建稀疏相似性图的方法🤯。成对的N²比较显然不可行——Stars使用了两个跳数的[spanner图](https://en.wikipedia.org/wiki/Geometric_spanner)（这些图中相似的点通过最多两个跳数连接）和[SortingLSH](http://infolab.stanford.edu/~bawa/Pub/similarity.pdf)，两者结合使得接近线性的时间复杂度和高稀疏性成为可能。

[Dhulipala等](https://openreview.net/pdf?id=LpgG0C6Y75) 创建了**ParHAC**，一种用于非常大图的近似（1+𝝐）并行层次聚类（HAC）算法及其广泛的理论基础。ParHAC具有O(V+E)复杂度和多对数深度，在具有**数百亿**边的图上运行速度比基线快高达60倍（这里是具有1.7B节点和125B边的超链接图）。

[Devvrit等](https://openreview.net/pdf?id=ldl2V3vLZ5) 创建了**S³GC**，一种可扩展的自监督图聚类算法，使用单层GNN和对比训练目标。S³GC使用图结构和节点特征，能够扩展到高达1.6B边的图。

最后，[Epasto等](https://openreview.net/forum?id=Fhty8PgFkDo) 创建了PageRank的差分隐私修改版本！

LoG 2022包含了两个关于大规模GNN的教程：[生产中GNN的扩展](https://www.youtube.com/watch?v=HRC4hZKiUWU)由Da Zheng、Vassilis N. Ioannidis和Soji Adeshina主讲，以及[并行和分布式GNN](https://www.youtube.com/watch?v=e2jJU7u7si0)由Torsten Hoefler和Maciej Besta主讲。

> **2023年值得期待**：进一步降低计算成本和推理时间，以应对非常大的图。也许OGB LSC图的模型可以在普通机器上运行，而不是在大型集群上？

# GNN理论：Weisfeiler和Leman的足迹，子图GNN

![](../Images/b7121cd3156f09a0fadc730188d2124d.png)

年度游客！原始肖像来源：[几何深度学习IV：GNN的化学前体](/towards-geometric-deep-learning-iv-chemical-precursors-of-gnns-11273d74125) 由Michael Bronstein主讲

🏖 🌄 Weisfeiler和Leman，图ML和GNN理论的奠基人，度过了非常多产的一年！继之前访问过[Neural](https://ojs.aaai.org/index.php/AAAI/article/view/4384)、[Sparse](https://proceedings.neurips.cc/paper/2020/file/f81dee42585b3814de199b2e88757f5c-Paper.pdf)、[Topological](http://proceedings.mlr.press/v139/bodnar21a/bodnar21a.pdf)和[Cellular](https://proceedings.neurips.cc/paper/2021/file/157792e4abb490f99dbd738483e0d2d4-Paper.pdf)场所后，2022年我们在几个新地方见到了他们：

+   WL Go **机器学习** — [Morris等](https://arxiv.org/abs/2112.09992) 对WL测试的基础知识、术语及各种应用的综合调查；

+   WL Go **关系** —— [Barcelo et al](https://arxiv.org/abs/2211.17113) 首次尝试研究在多关系图和知识图中使用的关系 GNNs 的表现力。结果表明，R-GCN 和 CompGCN 的表现力相同，且受限于关系 1-WL 测试，最具表现力的消息函数（聚合实体-关系表示）是 Hadamard 乘积；

+   [WL Go Walking by Niels M. Kriege](https://arxiv.org/abs/2205.10914) 研究了随机游走核的表现力，发现 RW 核（经过小的修改）与 WL 子树核一样具表现力。

+   WL Go **几何**：[Joshi, Bodnar et al](https://openreview.net/forum?id=kXe4Y0c4VqT) 提出了几何 WL 测试（GWL）来研究等变和不变 GNNs（对于某些对称性：平移、旋转、反射、排列）的表现力。结果表明，等变 GNNs（例如 [E-GNN](https://arxiv.org/abs/2102.09844)、[NequIP](https://arxiv.org/abs/2101.03164) 或 [MACE](https://arxiv.org/abs/2206.07697)）在理论上比不变 GNNs（例如 [SchNet](https://proceedings.neurips.cc/paper/2017/file/303ed4c69846ab36c2904d3ba8573050-Paper.pdf) 或 [DimeNet](https://arxiv.org/abs/2011.14115)）更强大。

+   WL Go **时间**：[Souza et al](https://openreview.net/pdf?id=MwSXgQSxL5s) 提出了时间 WL 测试来研究时间 GNNs 的表现力。作者随后提出了一种新型的单射聚合函数（以及 PINT 模型），这应该是最具表现力的；

+   WL Go **渐进**：[Bause 和 Kriege](https://openreview.net/forum?id=fe1DEN1nds) 提议用非单射函数修改原始 WL 颜色细化，其中不同的多重集合*可能*被分配相同的颜色（在某些条件下）。因此，它使得颜色细化过程更加渐进，并且收敛到稳定着色的速度更慢，最终保留了 1-WL 的表现力，但在过程中获得了一些区分特性。

+   WL Go **无限**：[Feldman et al](https://arxiv.org/abs/2201.13410) 提议用从拉普拉斯的热核衍生的谱特征或拉普拉斯的 k 最小特征向量（对于大图）来改变初始节点着色，这与拉普拉斯位置编码（LPEs）非常接近。

+   WL Go **双曲**：[Nikolentzos et al](https://arxiv.org/abs/2211.02501) 指出 WL 测试的颜色细化过程会产生颜色的树状层次结构。为了保持这些颜色所编码的节点相对距离，作者建议将每层/迭代的输出状态映射到双曲空间，并在每次更新后调整。最终的嵌入应该保留节点距离的概念。

📈 在比1-WL更具表达力的架构领域中，子图GNNs是最大的趋势。其中有三种方法脱颖而出：1️⃣ **子图联合网络**（SUN），由[Frasca, Bevilacqua等](https://arxiv.org/abs/2206.11140)提出，提供了子图GNNs设计空间和表达力的全面分析，显示它们受限于3-WL；2️⃣ **有序子图聚合网络**（OSAN），由[Qian, Rattan等](https://arxiv.org/abs/2206.11168)提出，设计了一种子图增强GNNs（k-OSAN）的层次结构，并发现k-OSAN与k-WL不可比，但严格受限于（k+1）-WL。OSAN的一个特别酷的部分是使用[隐式MLE](https://arxiv.org/abs/2106.01798)（NeurIPS’21），这是一种可微分的离散采样技术，用于采样有序子图。**️3️⃣ SpeqNets** 由[Morris等](https://arxiv.org/abs/2203.13913)提出，设计了一种图网络的置换等变层次结构，平衡了可扩展性和表达力。4️⃣ **GraphSNN** 由[Wijesinghe和Wang](https://openreview.net/pdf?id=uxgg9o7bI_3)提出，基于*子图*同构和*子树*同构的重叠推导出表达性模型。

🤔 一些研究重新审视WL框架作为GNN表达力的一种通用手段。[Geerts和Reutter](https://openreview.net/pdf?id=wIzUeM3TAU)定义了**k阶MPNNs**，可以通过张量语言（WL与**张量语言**之间的映射）进行表征。一个新的[匿名ICLR’23提交](https://openreview.net/forum?id=r9hNv76KoT3)提出利用[图的双连通性](https://en.wikipedia.org/wiki/Biconnected_component)并定义了**广义距离WL**算法。

如果你想更深入地研究这个主题，查看Fabrizio Frasca、Beatrice Bevilacqua和Haggai Maron的精彩[LOG 2022教程](https://www.youtube.com/watch?v=ASQYjbUBYzs&list=PL2iNJC54likoqgKwpFnbBik8Im1sZ27Hm&index=7)及其实际例子吧！

> **2023年的期待**：*1️⃣* 更多致力于创建时间和内存高效的子图GNNs。*2️⃣* 更好地理解GNNs的泛化能力。*3️⃣* Weisfeiler和Leman访问10个新地方！

# 知识图谱：归纳推理占据主导地位

去年，我们观察到了KG表示学习的重大转变：传导性方法正被积极淘汰，取而代之的是归纳模型，这些模型可以为新的、未见过的节点构建有意义的表示，并执行节点分类和链接预测。

在2022年，该领域沿着两个主要轴线扩展：1️⃣ 归纳链接预测（LP）2️⃣ 和归纳（多跳）查询回答，将链接预测扩展到更复杂的预测任务。

1️⃣ 在链路预测中，大多数归纳模型（如[**NBFNet**](https://arxiv.org/abs/2106.06935)或[**NodePiece**](https://arxiv.org/abs/2106.12144)）通过假设关系类型的集合在训练期间是固定的且不会随时间变化，从而转移到未见节点，并学习关系嵌入。当关系集合也发生变化时会发生什么？在最困难的情况下，我们希望能够转移到具有完全不同节点**和**关系类型的知识图谱。

到目前为止，所有支持未见关系的模型都依赖于元学习，这种方法既慢又消耗资源。在2022年，[黄，任和莱斯科维奇](https://openreview.net/forum?id=LvW71lgly25)首次提出了**CSR**（连接子图推理器）框架，它在**实体**和**关系类型**上都是归纳的**且**不需要任何元学习！👀 通常，在推理时，对于新关系，模型至少看到*k*个示例三元组（因此是k-shot学习场景）。从概念上讲，CSR提取每个示例周围的子图，试图学习共同的关系模式（即优化边缘掩码），然后将掩码应用于查询子图（预测缺失的目标链路）。

![](../Images/495777bb4fc9ea6eae90cb434332ab5a.png)

支持具有未见实体和关系类型的知识图谱的**归纳CSR**。来源：[黄，任和莱斯科维奇](https://openreview.net/forum?id=LvW71lgly25)

[Chen等人](https://openreview.net/forum?id=81LQV4k7a7X)的**ReFactor GNNs**是另一项有关浅层KG嵌入模型归纳特性的有洞察力的工作——特别是，作者发现浅层因子分解模型如DistMult在反向传播的视角下，与无限深度的GNNs类似，节点如何从邻近节点和非邻近节点更新其表示。理论上，任何因子分解模型都可以转变为归纳模型！

2️⃣ 从归纳表征学习也开始涉及到复杂的逻辑查询回答。 （厚颜无耻的插入广告）事实上，这是我们团队今年的重点之一 😊 首先，在[Zhu等人](https://arxiv.org/abs/2205.10128)的研究中，我们发现神经贝尔曼-福特网络从简单的链接预测成功地推广到了复杂的查询任务，这是在一个新的[**GNN查询执行器**](https://github.com/DeepGraphLearning/GNN-QE)（GNN-QE）模型中，其中一个基于NBF-Net的GNN进行了关系投影，而其他逻辑运算通过模糊逻辑[t-范数](https://en.wikipedia.org/wiki/T-norms)来实现。 然后，在[知识图中的归纳逻辑查询回答](https://openreview.net/forum?id=-vXEN5rIABY)中，我们研究了⚗️ *归纳性的本质* ⚗️并提出了两种解决方案，以在推理时回答关于未知实体的逻辑查询，即通过（1）与仅用于推理的解码器配对的归纳节点表示（性能较差但可扩展），或通过（2）类似于GNN-QE中的归纳关系结构表示（质量更高但需要更多资源并且难以扩展）。 总的来说，我们能够在推理时处理**拥有数百万节点和500k个未见节点以及5m未见边缘**的图中进行归纳查询设置。

![图片](../Images/063be3295d889aafc4b3b1c292e3e88b.png)

透过节点表示（NodePiece-QE）和关系结构表示（GNN-QE）进行归纳逻辑查询回答。 来源：[Galkin等人](https://arxiv.org/abs/2210.08008)

这个领域中的另一项重要工作是[**SMORE**](https://github.com/google-research/smore)来自[任、戴等人](https://arxiv.org/abs/2110.14890) — 这是一个大规模（仅限传导）的系统，可对大约90M个节点和300M条边缘的完整Freebase进行复杂查询回答👀。 除了CUDA、训练和管道优化，SMORE还实现了一个双向查询采样器，使得训练查询可以在数据加载器中即时生成，而无需创建和存储大量数据集。 不要忘记从LOG 2022中查看关于大规模图推理的[全新实践教程](https://www.youtube.com/watch?v=kzWV57qJmiA&list=PL2iNJC54likoqgKwpFnbBik8Im1sZ27Hm&index=1)!

最后但并非最不重要，[杨、林和张](https://arxiv.org/pdf/2209.08858.pdf)提出了一篇有趣的论文，重新思考了知识图完成的评估。 他们指出知识图倾向于是开放世界的（即，有些事实并未被知识图编码），而不是大部分作品所假设的闭世界。 因此，在闭世界假设下观察到的指标对真实指标呈现出了对数趋势——这意味着如果你的模型得到了0.4的MRR，那么测试知识图很可能是不完整的，而你的模型已经做得相当不错👍。也许我们可以设计一些新的数据集和评估来缓解这个问题？

> **2023 年的预期**：一个可以完全迁移到不同知识图谱的新集合的归纳模型，例如在 Wikidata 上进行训练，并在 DBpedia 或 Freebase 上运行推理。

# 算法推理与对齐

2022 年是算法推理领域取得重大突破和里程碑的一年。

1️⃣ 首先，[**CLRS 基准测试**](https://github.com/deepmind/clrs)由 [Veličković 等人](https://arxiv.org/abs/2205.15659) 提供，现在可以作为设计和评估算法推理模型和任务的主要平台。CLRS 已经包括了 30 个任务（如经典排序算法、字符串算法和图算法），但仍然允许你带入自己的公式或修改现有公式。

2️⃣ 然后，[Ibarz 等人](https://openreview.net/forum?id=FebadKZf6Gd) 和 DeepMind 的**通用神经算法学习器**表明，可以训练一个*单一*的处理器网络，在不同算法上以多任务模式进行训练——以前，你需要为每个 CLRS 问题训练一个单独的模型。论文还描述了模型架构和训练过程中的若干修改和技巧，以使模型更好地进行泛化并防止遗忘，例如，类似于三角注意力的三元组推理（在分子模型中常见）和 [边变换器](https://arxiv.org/abs/2112.00578)。总体而言，新模型带来了比基线高出 25% 的绝对增益，并且以 60%+ 的微观 F1 分数解决了 30 个 CLRS 任务中的 24 个。

![](../Images/b4f7d36cd5ab8040d258a815d1dca4dc.png)

来源：[Ibarz 等人](https://openreview.net/forum?id=FebadKZf6Gd)

3️⃣ 去年，我们[讨论了](https://graph-ml-in-2022-where-are-we-now-f7f8242599e0#72d1)关于算法对齐的工作，并看到 GNNs 可能与动态规划良好对齐的迹象。在 2022 年，[Dudzik 和 Veličković](https://openreview.net/forum?id=wu1Za9dY1GY) 通过范畴论、抽象代数以及*推前*和*拉回*操作的概念证明了**GNNs 是动态规划器**。这是应用范畴论的一个绝妙例子，许多人认为这是“抽象无聊”的😉。范畴论可能会在 GNN 理论和图 ML 中产生更大的影响，因此可以查看新的课程 [Cats4AI](https://cats.for.ai/) 以获得对该领域的温和介绍。

4️⃣ 最后，[Beurer-Kellner 等人](https://openreview.net/forum?id=AiY6XvomZV4) 的工作是神经算法推理框架的首批实际应用之一，这里将其应用于配置计算机网络，即互联网核心的路由协议如 BGP。在这项工作中，作者展示了将路由配置表示为图形，可以将路由问题框架化为节点属性预测。这种方法相比传统的基于规则的路由方法带来了惊人的👀 **490x** 👀加速，并且仍然保持90%以上的规范一致性。

![](../Images/9045a1b7fb0c1e4f9b6f05c353eaaea9.png)

来源：[Beurer-Kellner 等](https://openreview.net/forum?id=AiY6XvomZV4)

如果你想更深入地了解算法推理，不要错过 Petar Veličković、Andreea Deac 和 Andrew Dudzik 最新的 [LoG 2022 教程](https://algo-reasoning.github.io/)。

> **2023 年的展望： *1️⃣*** 算法推理任务可能会扩展到成千上万个节点的图，以及代码分析或数据库等实际应用， *2️⃣* 更多的基准算法， *3️⃣* 最不可能——会出现一个能够解决 quickselect 的模型 *😅*

# 酷炫的 GNN 应用

👃 **使用 GNN 学习嗅觉。** 早在2019年，Google AI 开始了一个关于嗅觉表示学习的 [项目](https://ai.googleblog.com/2019/10/learning-to-smell-using-deep-learning.html)。从基础化学知识我们知道，芳香性取决于分子结构，例如环状化合物。事实上，整个“芳香烃”组被命名为 *芳香的* 是因为它们实际上有一些气味（与许多无机分子相比）。如果我们有一个分子结构，我们可以在其基础上使用 GNN 来学习一些表示！

最近，Google AI 发布了 [一篇新博客文章](https://ai.googleblog.com/2022/09/digitizing-smell-using-molecular-maps.html) 和 [Qian 等人](https://www.biorxiv.org/content/10.1101/2022.07.21.500995v3) 的论文，描述了项目的下一阶段——**主要气味地图**，能够将分子分组为“气味簇”。作者进行了三项有趣的实验：对400种之前从未闻过的新分子进行分类，并与一组人类评审员的平均评分进行比较；将气味质量与基础生物学关联；以及探测芳香分子的驱蚊特性。基于 GNN 的模型表现非常出色——现在我们可以自信地说 GNN 可以嗅觉！期待 GNN 在香水行业的变革。

![](../Images/676191b7f09085ed9d141bae32bae385.png)

气味的嵌入。来源：[Google AI 博客](https://ai.googleblog.com/2022/09/digitizing-smell-using-molecular-maps.html)

⚽ **GNNs + 足球。** 如果你认为用于建模轨迹的复杂GNN仅用于分子动力学和深奥的量子模拟，那就不要担心！这里有一个非常有潜力的实际应用：**Graph Imputer** 由 [Omidshafiei 等](https://www.nature.com/articles/s41598-022-12547-0.epdf?sharing_token=HmyoHCAtNdoDfjlObtCiltRgN0jAjWel9jnR3ZoTv0NzQifNnvllGA8o7uZB3n1gdCaC-3jfBQwxpTCJNR7isTeW2uWhYUL8hz8MmWvyYQLogAFNcVp5ZZuTr_O-slFsi4f4-5pz3J2Th9rSxCJV-s63f-q5fojV0FBGNWKYlRQ%3D) 提出的，DeepMind 和利物浦足球俱乐部预测足球运动员（以及足球）的轨迹。每场比赛图包含23个节点，通过标准消息传递编码器和特殊的时间依赖LSTM进行更新。数据集也非常新颖——它包含了105场英超比赛（每场比赛平均90分钟），所有球员和足球的运动被以25帧每秒的速度跟踪，得到的训练轨迹序列编码了大约9.6秒的比赛过程。

这篇论文易于阅读，并有大量的足球插图，快去看看吧！体育技术在这些年里迅速发展，足球分析师现在可以更深入地研究他们的对手。英超俱乐部会在即将到来的转会窗口中竞争GNN研究人员吗？是时候为GNN研究人员创建一个transfermarkt了😉

![](../Images/4636afc466b88dba1d5146bab2df8221.png)

足球比赛模拟就像分子动力学模拟一样！来源：[DeepMind](https://twitter.com/deepmind/status/1529444212864843777?lang=en)

🪐 **银河系与天体物理学。** 对天体物理学爱好者而言：**Mangrove** 由 [Jespersen 等](https://arxiv.org/abs/2210.13473) 提出的应用GraphSAGE于暗物质的合并树，以预测各种银河属性，如恒星质量、冷气体质量、星形成率，甚至黑洞质量。尽管这篇论文在天体物理学术语上有些复杂，但在GNN参数化和训练方面相对简单。Mangrove的速度比标准模型快4到9个数量级。实验图表就像艺术品一样，可以挂在墙上 🖼️。

![](../Images/77efdaa485947c23151de37ec221a2cb.png)

Mangrove将暗物质晕呈现为合并树和图。来源：[Jespersen 等](https://arxiv.org/abs/2210.13473)

🤖 **GNNs用于代码**。像AlphaCode和Codex这样的代码生成模型具有令人惊叹的能力。虽然LLMs是这些模型的核心，但GNNs在几个巧妙的方面确实有帮助：**指令指针注意力GNNs**（IPA-GNNs）首次由[Bieber et al](https://arxiv.org/abs/2010.12621)提出，用于[预测运行时错误](https://arxiv.org/abs/2203.03771)在竞赛编程任务中——这几乎就像一个虚拟的代码解释器！由[Pashakhanloo et al.](https://openreview.net/forum?id=WQc075jmBmf)提出的**CodeTrek**建议将程序建模为关系图，并通过随机游走和Transformer编码器进行嵌入。下游应用包括变量误用、预测异常和预测被遮蔽的变量。

![](../Images/ab5c9d787662778c2d57153876512efe.png)

来源：[Pashakhanloo et al.](https://openreview.net/forum?id=WQc075jmBmf)

# 硬件：IPUs和Graphcore赢得OGB大规模挑战赛2022

🥇 2022年为[Graphcore](https://www.graphcore.ai/)和[IPUs](https://www.graphcore.ai/bow-processors)带来了巨大的成功——这些硬件专门优化了处理图形时非常需要的稀疏操作。第一个成功故事是优化了IPUs上的Temporal Graph Nets (TGN)，取得了巨大的性能提升（查看Michael Bronstein博客中的[文章](/accelerating-and-scaling-temporal-graph-networks-on-the-graphcore-ipu-c15ac309b765)）。

[## 加速和扩展在Graphcore IPU上的Temporal Graph Networks](/accelerating-and-scaling-temporal-graph-networks-on-the-graphcore-ipu-c15ac309b765?source=post_page-----1ba920cb9232--------------------------------)

### GPU是GNNs的最佳硬件选择吗？与Graphcore一起，我们探讨了新IPU的优势……

[towardsdatascience.com](/accelerating-and-scaling-temporal-graph-networks-on-the-graphcore-ipu-c15ac309b765?source=post_page-----1ba920cb9232--------------------------------)

此后，Graphcore在OGB LSC’22的排行榜上[大放异彩](https://www.graphcore.ai/posts/graphcore-claims-double-win-in-open-graph-benchmark-challenge)，在3个赛道中赢得了2个：**WikiKG90M v2**知识图谱上的链接预测和**PCQM4M v2**分子数据集上的图回归。除了强大的计算能力外，作者还做出了一些巧妙的模型决策：对于链接预测，采用了[平衡实体采样和共享 (BESS)](https://arxiv.org/abs/2211.12281)来训练一个浅层LP模型的集合（查看Daniel Justus的[博客文章](/large-scale-knowledge-graph-completion-on-ipu-4cf386dfa826)了解更多细节），对于图回归任务采用了GPS++（我们在GT部分中讨论了GPS++）。你可以[尝试](https://ipu.dev/3FwVoLD)在Paperspace上使用IPUs支持的虚拟机来使用预训练模型。祝贺Graphcore及其团队！👏

PyG 与 NVIDIA ([post](https://pyg.org/ns-newsarticle-accelerating-pyg-on-nvidia-gpus)) 和 Intel ([post](https://pyg.org/news/accelerating-pyg-on-intel-cpus)) 合作，分别提高了 GPU 和 CPU 上核心操作的性能。类似地，DGL [纳入](https://www.dgl.ai/release/2022/07/25/release.html) 了最近 0.9 版本中的新 GPU 优化。对于稀疏矩阵乘法和采样程序都有大幅提升，因此我们建议你更新到最新版本的环境！

> **2023 年的期望：** 主要 GNN 库可能会扩展对硬件后端的支持，如 IPU 或即将推出的 Intel Max 系列 GPU。

# 新会议：学习图形（LoG）和分子 ML（MoML）

今年我们见证了两个图形和几何 ML 会议的启动：[学习图形会议（LoG）](https://logconference.org/#hero) 和 [分子 ML 会议](https://www.moml22.mit.edu/)（MoML）。

LoG 是一个更通用的全方位 GraphML 会议（今年以虚拟形式举行），而 MoML（在 MIT 举办）具有更广泛的使命和对 AI4Science 社区的影响，其中图和几何仍然扮演着重要角色。这两个会议都得到了极好的反响。MoML 吸引了 7 位顶级讲者和 38 个海报，LoG 有大约 3000 个注册、266 个投稿、71 个海报、12 个口头报告和 7 个精彩的教程（所有口头报告和教程的录音[已在 YouTube](https://www.youtube.com/@learningongraphs) 上）。此外，LoG 为评审引入了丰厚的奖金激励，显著提高了评审质量！在我们看来，LoG 的评审质量通常优于 NeurIPS 或 ICML 的评审。

这是图形 ML 社区的一次巨大胜利和庆典，祝贺所有在图形和几何机器学习领域工作的人员，拥有了一个新的“家”会议！

> **2023 年的期望：** LOG 和 MoML 将成为包括在提交日历中的主要 Graph ML 会议，此外还有 ICLR / NeurIPS / ICML。

# 课程和教育材料

+   几何深度学习课程 — [第二版](https://www.youtube.com/playlist?list=PLn2-dEmQeTfSLXW8yXP4q_Ii58wFdxb3C)（2022 年）已在 YouTube 上。该领域的主要入门点。

+   [群体等变深度学习介绍](https://uvagedl.github.io/) 由 Erik Bekkers 提供 — 这是关于等变性和等变模型的最佳新课程之一！

+   [Cats4AI](https://cats.for.ai/) — 由 Andrew Dudzik、Bruno Gavranović、João Guilherme Araújo、Petar Veličković 和 Pim de Haan 开设的新课程，是了解范畴理论及其与几何深度学习连接的最佳场所。

+   夏季学校成果：[意大利几何深度学习夏季学校](https://www.sci.unich.it/geodeep2022/#home)、伦敦几何与机器学习（[LOGML](https://www.logml.ai/home-2022)）夏季学校、[BIRS 顶点表示学习研讨会](https://www.birs.ca/events/2022/5-day-workshops/22w5125)。

+   [斯坦福图学习研讨会 2022](https://snap.stanford.edu/graphlearning-workshop-2022/) — PyG开发者、合作伙伴和斯坦福研究人员的最新消息。

# 新数据集、基准和挑战

+   [OGB大规模挑战 2022](https://ogb.stanford.edu/neurips2022/): 第二届大规模挑战，在NeurIPS2022上举办，涵盖节点、边、图级预测的大规模和现实图机器学习任务。

+   [开放催化剂 2022 挑战](https://opencatalystproject.org/challenge.html): 第二届挑战，在NeurIPS2022上举办，任务是设计新的机器学习模型，以预测催化剂模拟的结果，以了解其活性。

+   [CASP 15](https://predictioncenter.org/casp15/index.cgi): 由AlphaFold在CASP 14中引发的蛋白质结构预测挑战。详细分析尚待公布，但似乎MSAs卷土重来，表现最好的模型仍然依赖于MSAs。

+   [长距离图基准](https://arxiv.org/abs/2206.08164): 用于测量GNNs和GTs在图中捕捉长距离交互的能力。

+   [图基准分类](https://arxiv.org/abs/2206.07729)，[图学习索引器](https://github.com/Graph-Learning-Benchmarks/gli): 对图机器学习数据集景观的深入研究，概述了基准测试和结果可信度中的开放挑战。

+   [GraphWorld](https://ai.googleblog.com/2022/05/graphworld-advances-in-graph.html): 一个框架，用于分析GNN架构在数百万个合成基准数据集上的表现。

+   [Chartalist](https://openreview.net/forum?id=10iA3OowAV3) — 一系列区块链图数据集。

+   [PEER蛋白质学习基准](https://github.com/DeepGraphLearning/PEER_Benchmark): 一个多任务蛋白质序列理解基准，包括17个蛋白质理解任务，分为5个任务类别。

+   [ESM宏基因组图谱](https://esmatlas.com/): 一个综合性数据库，包含超过6亿个预测蛋白质结构，提供了漂亮的可视化和搜索界面。

# 软件库和开源

+   主流图机器学习库：[PyG 2.2](https://www.pyg.org/)（PyTorch），[DGL 0.9](https://www.dgl.ai/)（PyTorch、TensorFlow、MXNet），[TF GNN](https://github.com/tensorflow/gnn)（TensorFlow）和 [Jraph](https://github.com/deepmind/jraph)（Jax）

+   [TorchDrug](https://torchdrug.ai/) 和 [TorchProtein](https://torchprotein.ai/): 用于药物发现和蛋白质科学的机器学习库。

+   [PyKEEN](https://github.com/pykeen/pykeen): 用于训练和评估知识图谱嵌入的最佳平台。

+   [Graphein](https://graphein.ai/): 提供多种基于图的蛋白质表示的包。

+   [GRAPE](https://github.com/AnacletoLAB/grape) 和 [Marius](https://marius-project.org/): 可扩展的图处理和嵌入库，处理超大规模图。

+   [MatSci ML工具包](https://github.com/IntelLabs/matsciml): 用于在opencatalyst数据集上进行深度学习的灵活框架。

+   [E3nn](https://github.com/e3nn/e3nn)：用于 E(3) 等变神经网络的首选库

# 加入社区

+   阅读小组：[图学习与几何](https://m2d2.io/talks/log2/about/)（LOG2）阅读小组，[分子建模与药物发现](https://m2d2.io/talks/m2d2/about/)（M2D2）阅读小组及其 Slack 社区

+   图学习（LoG）[Slack 社区](https://logconference.org/)

+   [Michael Bronstein 的 Medium 博客](https://michael-bronstein.medium.com/)

+   [PyG medium](https://medium.com/@pytorch_geometric)、[博客文章](https://pyg.org/blogs-and-tutorials)和通讯

+   [GraphML Telegram 频道](https://t.me/graphML)

# 2022 年的流行文化现象 🪓

![](../Images/8b5892b801b9e71ad5914d29b86184f8.png)

由 Michael Galkin 和 Michael Bronstein 创建
