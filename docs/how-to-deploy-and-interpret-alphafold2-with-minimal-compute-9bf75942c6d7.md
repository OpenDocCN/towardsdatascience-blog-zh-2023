# 如何以最少计算资源部署和解释AlphaFold2

> 原文：[https://towardsdatascience.com/how-to-deploy-and-interpret-alphafold2-with-minimal-compute-9bf75942c6d7?source=collection_archive---------1-----------------------#2023-02-24](https://towardsdatascience.com/how-to-deploy-and-interpret-alphafold2-with-minimal-compute-9bf75942c6d7?source=collection_archive---------1-----------------------#2023-02-24)

## **使用有限计算资源部署和解释AlphaFold2**

[](https://medium.com/@temitope.sobodu?source=post_page-----9bf75942c6d7--------------------------------)[![Temitope Sobodu](../Images/04bf69e0b2c024ab47ed98b3d3f9ec3c.png)](https://medium.com/@temitope.sobodu?source=post_page-----9bf75942c6d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9bf75942c6d7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9bf75942c6d7--------------------------------) [Temitope Sobodu](https://medium.com/@temitope.sobodu?source=post_page-----9bf75942c6d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3b87c271bf6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-and-interpret-alphafold2-with-minimal-compute-9bf75942c6d7&user=Temitope+Sobodu&userId=3b87c271bf6&source=post_page-3b87c271bf6----9bf75942c6d7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9bf75942c6d7--------------------------------) ·9分钟阅读·2023年2月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9bf75942c6d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-and-interpret-alphafold2-with-minimal-compute-9bf75942c6d7&user=Temitope+Sobodu&userId=3b87c271bf6&source=-----9bf75942c6d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9bf75942c6d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-deploy-and-interpret-alphafold2-with-minimal-compute-9bf75942c6d7&source=-----9bf75942c6d7---------------------bookmark_footer-----------)![](../Images/3325dbea4f4737a189c1ff5eccaa950d.png)

图片由 [Luke Jones](https://unsplash.com/@lukejonesdesign?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

生物学中最艰难的挑战之一已经被一家总部位于伦敦的人工智能公司——DeepMind——克服。这家公司以90分的真实分数赢得了蛋白质结构预测第14届评估（CASP14）。DeepMind随后在2021年夏季发布了一篇具有里程碑意义的论文。

[## 高度准确的蛋白质结构预测与AlphaFold - Nature](https://www.nature.com/articles/s41586-021-03819-2?source=post_page-----9bf75942c6d7--------------------------------#Sec3)

### 蛋白质对生命至关重要，理解其结构可以促进对其机制的了解……

[www.nature.com](https://www.nature.com/articles/s41586-021-03819-2?source=post_page-----9bf75942c6d7--------------------------------#Sec3)

DeepMind将其蛋白质折叠平台命名为AlphaFold（截至撰写时的更新版本——AlphaFold v2.3.0）。他们在GitHub上发布了源代码以供公开访问。然而，部署AlphaFold2的开源代码需要巨大的计算资源；下载数据库需要12个vCPU、85 GB RAM、100 GB引导磁盘、3 TB磁盘和一个A100 GPU。此外，用户必须精通Linux，并部署docker容器和其他依赖项。本文的目的是指导读者通过无缝替代工具来利用AlphaFold的奇迹。

本文长度适中（无意中），请继续跟随我。

我将讨论：

· AlphaFold的工作原理

· 性能评估指标

· EMBL-EBI方法

· Colab笔记本方法

· AlphaFold2的局限性

· 结论

## ***那么AlphaFold2是如何工作的呢？***

AlphaFold2整合了经过训练的多序列比对 (MSA)、配对残基和来自宏基因组数据库的100000个已知蛋白质结构的PDB模板（通过NMR、X射线晶体学、冷冻电子显微镜验证）。AlphaFold2 evoformer是一个48块的神经网络，基于来自大型语言模型 (LLM)、分词、变换器和注意力的概念构建。

![](../Images/d2b119bc8d8fc946da5a896a3e29fafe.png)

图片来源：[Jumper et al](https://www.nature.com/articles/s41586-021-03819-2#Sec3)。

evoformer输出MSA和配对表示，这些表示被输入到结构预测模块。这些块利用不变点注意力预测MSA的第一行的单一表示副本，然后将其传递以预测预测蛋白质残基原子之间的𝛘扭转角度：本质上，即在XYZ维度中将互连的原子放置在肽链骨架附近。最终预测的蛋白质结构通过使用Amber力场的openMM进行能量优化，减少了立体碰撞和能量违背。

## ***性能评估指标***

AlphaFold2 (AF2) 输出一个3D蛋白质结构，模型性能通过三个置信度指标进行评估。

*预测局部差异测试* (pLDDT)，范围从0到100，是一个每个残基的置信度评分，表示模型对每个氨基酸残基相对于𝛂碳原子的预测置信度。

![](../Images/587c44c9b8757071eee71c02642cd9cb.png)

图片来源：作者

例如，上图展示了人类哺乳动物雷帕霉素靶蛋白（mTOR）丝氨酸/苏氨酸激酶的 pLDDT 彩色编码预测。pLDDT > 90 表示非常高的置信度，70–90 表示足够好的置信度，而 <70 表示低模型置信度。pLDDT 可以帮助我们评估模型在蛋白质区域或领域中的表现如何。

*预测对齐误差（PAE）* 给出了在对齐于同一平面时，两个残基 X 和 Y 相对于真实结构的领域间/领域内距离。简单来说，就是残基在空间中相对于其他残基的排列情况。通常，这些距离范围在 0-35 Ångstroms 之间，以获得可靠的预测。AF2 以能够更好地预测同一领域内（领域内残基）相对于不同领域（领域间残基）的残基相对位置而自豪。这是合理的，因为领域内的残基比其他领域的残基更为静态。模型输出一个 PAE 图，展示了残基位置在 X/Y 轴上的对比。

![](../Images/3a5a86201245eb940b728a2bb92f3f99.png)

图片来源：作者

*预测模板建模评分*（pTM）衡量两个折叠蛋白质结构之间的结构一致性。AlphaFold2 允许在建模选项中包含 PDB 模板。虽然预测时不需要这些模板，但它们可以提高模型的性能。pTM 范围从 0 到 1，为 AF2 提供了对其 5 个预测输出进行排名的框架。pTM < 0.2 的预测要么是随机分配的残基模式，与假定的天然结构几乎没有相关性，要么是内在无序的蛋白质。pTM > 0.5 通常足以进行推断。

现在我们已经讨论了 AlphaFold 的基本原理及其指标，我们可以深入探讨低计算方法来探索 AlphaFold。

## ***EMBL-EBI 方法***

为了方便访问 AlphaFold2，DeepMind 与欧洲分子生物学实验室、生物信息学组合作，策划了 48 种常见物种的蛋白质，包括人类、小鼠、大鼠、猿类、果蝇和酵母。该数据库包含超过 100 万个结构，涵盖 48 种生物体的蛋白质组。尽管这与 AlphaFold2 提供的 2 亿个开放源代码相比仍然是微不足道的数量。然而，这个超方便的 API 补贴了与开源代码相关的高昂计算成本。网页可以通过以下链接访问

![](../Images/294007d3676f96a772f729e5df24542d.png) [## AlphaFold 蛋白质结构数据库

### AlphaFold 蛋白质结构数据库

[AlphaFold 蛋白质结构数据库](https://alphafold.ebi.ac.uk/?source=post_page-----9bf75942c6d7--------------------------------)

在网页上，用户可以通过蛋白质名称、基因名称、UniProt 访问号和 UniProt ID 搜索感兴趣的蛋白质。另一个替代的搜索方法是使用序列，如果蛋白质名称未知。未知蛋白质可以通过将蛋白质序列输入到另一个名为 Fasta 的工具中进行解码（链接见下）。

![](../Images/a86b75baf1c78dcf755b2ad29d1b00d0.png) [## FASTA/SSEARCH/GGSEARCH/GLSEARCH < 序列相似性搜索 < EMBL-EBI

### FASTA（发音为 FAST-AYE）是一套用于通过查询序列搜索核苷酸或蛋白质数据库的程序……

[www.ebi.ac.uk](https://www.ebi.ac.uk/Tools/sss/fasta/?source=post_page-----9bf75942c6d7--------------------------------)

在 Fasta 上，用户可以通过输入已知序列来搜索包括蛋白质在内的大分子，序列参考一个通用蛋白质数据库，例如 UniProt。

EMBL-EBI 网页输出的折叠结构以 HTML 格式显示，由 pLDDT 进行颜色编码，并生成 PAE 图。用户可以以 PDB、mmCIF 或 JSON 格式下载预测的蛋白质结构。EMBL-EBI 速度快，几乎是瞬时的。它不需要安装或依赖项。EMBL-EBI 网页的缺点包括对序列输入的灵活性不足，并且它只能处理单体结构。此外，这些模型无法调整以预测使结构不稳定的突变残基或结构域。它们基本上适用于现成的预测，对推断或假设生成贡献甚微。

## ***Colab notebook 方法***

Colab notebook 是一个托管用 Python 编写的应用程序的网站。AlphaFold colab notebook 托管在 Google Cloud 服务器上。用户需要初始化并连接到服务器，该服务器为每个 colab notebook 会话分配 GPU 或 TPU 资源（RAM 和磁盘）。从用户体验的角度来看，这与开源预测的功能最为接近。AF2 colab notebook 为用户分配计算资源，但这些分配的资源不是无限的，并且限制用户在预测优化时最多使用 800 个残基。超过这个上限，AF2 的性能可能会大幅下降。Colab notebook 的其他缺点包括较长的等待时间（取决于实例分配的资源）。不适合大批量作业，基本上适合单序列预测。然而，与 EMBI-EBI 数据库相比，colab notebook 允许对预测模式进行一些修改。

*如何使用 AlphaFold2 colab notebooks 的逐步说明*

在 Google 上搜索 “AlphaFold2 colab notebooks” 或使用这个链接

[## Google Colaboratory

### 编辑描述

[colab.research.google.com](https://colab.research.google.com/github/sokrypton/ColabFold/blob/main/AlphaFold2.ipynb?source=post_page-----9bf75942c6d7--------------------------------)

重新连接下拉菜单将用户连接到 Google 云服务器。此步骤为用户分配会话的 GPU。请谨慎使用你的会话，因为 GPU 分配是有期限的，当分配的 GPU 用尽时，连接会被终止。然而，Google 提供了高级用户服务，为你分配接近无限的 GPU。蛋白质 MSA 输入到 **query_sequence box** 中。

同时，在运行作业之前，确保检查序列，修正空格和缩进。链间断裂可以用冒号 (:) 表示。例如 —EQVTNVGGAVVTGVTAVA:EQVTNVGGAVVTGVTAVA 表示一个同源二聚体。**Amber Forcefields** 放松蛋白质的 3D 结构。这允许侧链自由旋转，减少立体冲突和热力学违反。此步骤是可选的，不会显著改善模型性能。

此外，**msa_mode** 提供了序列搜索模式的选项。默认的 Many-against-Many 序列搜索模式使用 uniRef 和环境数据。用户可以选择仅使用 MMseq2 uniRef 或提供自定义序列搜索模式。

**model_type** 功能允许用户选择待预测蛋白质的结构维度。此功能支持对寡聚体或多聚体结构的预测。如果你处理的是寡聚体序列，请选择 Alphafold-multimer v1 或 v2 选项；如果结构是单体的，则选择 Alphafold2-ptm。auto 选项允许模型自动决定。默认选项是 auto。

**num-recycles** 功能使模型能够多次重复遍历序列和预测。这是需要 GPU 的原因之一，因为它使机器能够运行并行作业。默认的回收次数是 3，但建议扩展到 6 次或更多，直到模型达到接近准确的预测。同时，运行时间随着回收次数的增加而增加。

要运行作业，点击 **runtime** 下拉菜单，然后选择‘Run all’。这是最后一步，也可能是最简单的一步。然而，如果作业由于互联网连接或 GPU 可用性的中断而被打断，整个过程可能会停滞。因此，Run all 选项要求用户保持互联网连接，并保持 colab notebook 页面打开，直到作业完成。

完成后，AlphaFold2 colab notebook 返回 5 个模型预测结果及其各自的 pTM 和 pLDDT。‘最佳’预测结构会被排序，输出可以下载到用户的电脑本地驱动器或 Google Drive。压缩文件包含 5 个预测的 PDB 结构（如果使用了 amber relax 选项则为 10 个）、一个对应的 PAE 图以及 5 个 JSON 文件，包含 PAE 和 pTM 坐标或矩阵。

## ***AlphaFold2 的限制***

AlphaFold2已经取代了当前的蛋白质和结构生物学范式。它无疑是AI在生物学中最具影响力的应用之一。尽管取得了这样的巨大发展，AF2仍然有其局限性。AF2在预测变异蛋白质、错义缺失或突变方面的准确性几乎可以忽略。以下是表达这一局限性的两篇出版物。

[](https://www.nature.com/articles/s41594-021-00714-2?source=post_page-----9bf75942c6d7--------------------------------) [## AlphaFold2能否预测错义突变对结构的影响？ - Nature Structural &…

### 我们对H. Matsuo对手稿的批评性阅读表示感激。这项工作得到了内部研究的支持…

www.nature.com](https://www.nature.com/articles/s41594-021-00714-2?source=post_page-----9bf75942c6d7--------------------------------) [](https://www.biorxiv.org/content/10.1101/2021.09.19.460937v1?source=post_page-----9bf75942c6d7--------------------------------) [## 使用AlphaFold预测单一突变对蛋白质稳定性和功能的影响

### AlphaFold通过实现从蛋白质中预测三维（3D）结构，改变了结构生物学领域…

www.biorxiv.org](https://www.biorxiv.org/content/10.1101/2021.09.19.460937v1?source=post_page-----9bf75942c6d7--------------------------------)

不过，不用担心！华盛顿大学的David Baker小组开发了一个名为RosettaFold的AlphaFold2姊妹项目。RosettaFold在预测突变蛋白质结构方面表现更为出色（与MSA转换器如AF2相比，模型性能提高了1.25倍）。这是相关论文。

[](https://www.biorxiv.org/content/10.1101/2022.11.04.515218v1?rss=1&source=post_page-----9bf75942c6d7--------------------------------) [## 使用RoseTTAFold进行准确的突变效应预测

### 预测突变对蛋白质功能的影响是一个杰出的挑战。在这里，我们评估了…

www.biorxiv.org](https://www.biorxiv.org/content/10.1101/2022.11.04.515218v1?rss=1&source=post_page-----9bf75942c6d7--------------------------------)

总体而言，AF2在其出现之前的其他可用选项中表现得极其出色。无论是在预测蛋白质构建块的扭转角度和卷积方向，还是它们在空间中的相对位置。

## ***结论***

EMBL-EBI数据库和AF2的Google Colab笔记本提供了AF2模型的访问权限，免去了用户在高性能计算环境中遇到的计算麻烦。我希望我能为你指明正确的方向，并提供一些关于AF2实用性的见解，而无需使用传统的计算命令提示符和资源。

使用AlphaFold2玩得开心！确实是一个革命性的工具。

## *参考文献*

1.  Jumper J, Evans R, Pritzel A. [使用 AlphaFold 的高精度蛋白质结构预测](https://www.nature.com/articles/s41586-021-03819-2#Sec3)。 (2021)，*Nature*；596(7873):583–589。 doi:10.1038/s41586–021–03819–2

1.  Baumkötter F, Schmidt N, Vargas C, 等。 [淀粉样前体蛋白的二聚化和突触生成功能依赖于铜与生长因子样域的结合](https://www.jneurosci.org/content/34/33/11159.short)。 (2014)，*J Neurosci*。

1.  Wang M, Audas TE, Lee S. [揭开恶名：改变对淀粉样蛋白的看法](https://www.sciencedirect.com/science/article/abs/pii/S0962892417300338)。 (2017)，*Trends Cell Biol*；27(7):465–467。 doi:10.1016/j.tcb.2017.03.001
