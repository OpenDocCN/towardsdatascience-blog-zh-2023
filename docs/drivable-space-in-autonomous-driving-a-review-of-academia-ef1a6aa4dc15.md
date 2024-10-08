# 自动驾驶中的可驾驶空间 — 学术界

> 原文：[`towardsdatascience.com/drivable-space-in-autonomous-driving-a-review-of-academia-ef1a6aa4dc15`](https://towardsdatascience.com/drivable-space-in-autonomous-driving-a-review-of-academia-ef1a6aa4dc15)

## 关于 2023 年可驾驶空间的学术研究的最新趋势

[](https://medium.com/@patrickllgc?source=post_page-----ef1a6aa4dc15--------------------------------)![Patrick Langechuan Liu](https://medium.com/@patrickllgc?source=post_page-----ef1a6aa4dc15--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ef1a6aa4dc15--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef1a6aa4dc15--------------------------------) [Patrick Langechuan Liu](https://medium.com/@patrickllgc?source=post_page-----ef1a6aa4dc15--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----ef1a6aa4dc15--------------------------------) ·13 分钟阅读·2023 年 5 月 18 日

--

可驾驶空间，或称为自由空间，在自动驾驶中扮演着至关重要的安全角色。在上一篇博客文章中，我们回顾了这一经常被忽视的感知特征的定义和重要性。在本文中，我们将回顾近期学术研究中的趋势。

[](/drivable-space-in-autonomous-driving-the-concept-df699bb8682f?source=post_page-----ef1a6aa4dc15--------------------------------) ## 自动驾驶中的可驾驶空间 — 概念

### 可驾驶空间的定义与原因

towardsdatascience.com

可驾驶空间检测算法可以在两个维度上进行测量：输入和输出。关于**输入传感器模态**，可驾驶空间检测方法可以分为基于视觉的、基于激光雷达的或视觉-激光雷达融合的方法。关于**输出空间表示**，它们可以分为 2D 透视图像空间、3D 空间和鸟瞰视图（BEV）空间。

视觉图像本质上是 2D 的，而激光雷达点云测量本质上是 3D 的。正如在上一篇博客文章中讨论的那样，BEV 空间本质上是简化的或退化的 3D 空间，我们将在本博客中将 BEV 空间和 3D 空间互换使用。实质上，我们有一个 2x2 的输入输出矩阵用于评估所有可驾驶空间算法，如下图所示。绿色的右上象限是北极星，它具有最佳的表现力同时也是最具成本效益的。在接下来的章节中，我们将讨论三类算法：**2D-to-2D、3D-to-3D 和 2D-to-3D**。

![](img/2fc36e8aad7caa9e28b19941dd748157.png)

感知算法范式矩阵（图像由作者创建）

值得注意的是，目前还没有一个普遍认可的标准来表达和评估自动驾驶中可驾驶空间的准确性。在这篇文章中，我们将回顾相关任务，这些任务可以有多种公式化方式。我们还提供了一些对学术界未来方向的见解，旨在加速对这一关键任务的研究。

# 2D 到 2D 方法（带图像）

在透视 2D 图像空间中检测可驾驶空间本质上是图像分割的任务。主要有两种方法：一种是基于*stixel*的障碍物检测，另一种是可驾驶空间的语义分割。

## Stixel 表示法

[*stixel*（*stick* 和 *pixel* 的组合）](https://en.wikipedia.org/wiki/Stixel) 方法的概念假设图像底部的像素对应的区域从驾驶角度来看是可驾驶的。然后，它向图像顶部延伸并生长一个棍子，直到遇到障碍物，从而获得该列的可驾驶空间。该方法最具代表性的工作之一是 [StixelNet](http://www.bmva.org/bmvc/2015/papers/paper109/paper109.pdf) [系列](https://openaccess.thecvf.com/content_ICCV_2017_workshops/papers/w3/Garnett_Real-Time_Category-Based_and_ICCV_2017_paper.pdf)。Stixel 将地面上的一般障碍物抽象为棍子，并将图像空间划分为可驾驶空间和障碍物。Stixel 表示法在像素和对象之间取得了良好的平衡，实现了良好的准确性和效率。

![](img/34b37da9bb0da69b9169e44aac6599a9.png)

*Stixel 概念在可驾驶空间检测中的示意图（来源：*[StixelNet for segmentation*](https://arxiv.org/abs/2107.03070)*)*

## 语义分割

深度学习近年来取得了快速进展，使得检测可以直接通过卷积神经网络（CNN）建模为语义分割问题。这种方法与基于图像列表表示的方法不同，因为它直接对 2D 图像的像素进行是否为可驾驶空间的分类。典型的工作包括 [DS-Generator](https://arxiv.org/abs/2012.07890) 和 [RPP](https://link.springer.com/article/10.1007/s12559-017-9524-y)。

与 Stixel 方法相比，一般的语义分割方法更为灵活。然而，它需要更复杂的后处理以使其对下游组件有用。例如，语义预测可能没有那么连续（不像以下示例中显示的结果那样干净）。在 Stixel 方法中，每列只选择一个像素以转换为 3D 信息。

![](img/51210d9381631ab8775d6d43fc2170a8.png)

语义分割 将可驾驶空间公式化为（来源：[DS-Generator](https://arxiv.org/abs/2012.07890)）

## 提升到 3D

由于预测和规划计算的下游任务发生在 3D 空间中，因此需要将 2D 图像中获得的可行驶空间结果转换为 3D 或退化的 BEV。常见的 2D 到 3D 后处理技术包括逆透视映射 (IPM)、单目/立体深度估计以及使用直接的 3D 物理测量，如激光雷达点云。此外，2D 到 2D 算法将每个相机流单独处理，因此需要明确的规则将它们拼接在一起，以实现多相机设置中的 360 度感知。

在 2D 透视空间和 2D 到 3D 转换中的繁琐后处理通常是手工制作的，这些脆弱的逻辑容易受到特殊情况的影响。实际上，2D 到 2D 算法在自动驾驶中很少使用，除了低速场景如停车。

![](img/09801a16898992325219337b6037033e.png)

2D DS 需要提升到 3D DS 以供下游使用（图像由作者创建）

# 3D 到 3D 方法（配合激光雷达）

这些 3D 可行驶空间算法接收激光雷达点云并直接生成 3D 可行驶空间。虽然早期研究主要基于激光雷达，但最近（2023 年初）我们看到基于视觉的语义 3D 占用预测的爆炸性增长，这将在下一节中深入探讨。

## 激光雷达地面分割

基于地面分割的方法旨在将激光雷达点云数据分为地面部分和非地面部分。这些方法可以分为几何规则基础算法和基于深度学习的算法。即使在深度学习广泛应用之前，基于几何规则的算法也已广泛应用于激光雷达点云中，以实现地面检测（或边石检测的补充任务）和一般障碍物检测任务。这些方法通常依赖于平面拟合和区域生长算法，这些算法首次在 2007 年和 2009 年于[DARPA Urban Challenge](https://link.springer.com/article/10.1007/BF03224975)中介绍。然而，简单的地面平面假设在遇到不平整的地面、坑洞以及上下坡场景时会失败。为了考虑局部不平整的道路和整体平滑度，[一些](https://link.springer.com/article/10.1007/s10846-013-9889-4)[研究](https://arxiv.org/abs/2111.10638)建议引入基于高斯过程的算法优化。

基于深度学习的方法由于计算资源和大规模数据集的增加而逐渐流行。地面分割任务可以被形式化为对 LiDAR 点云的通用语义分割。[LidarMTL](https://arxiv.org/abs/2103.04056) 是一个典型的工作，提出了一个具有六个任务的多任务模型，其中包括动态障碍物检测之上的道路结构理解。对于道路场景理解，设计了两个语义分割任务：可驾驶空间和地面，并且还有一个地面高度回归任务。有趣的是，辅助任务，例如前景分割和物体内部部件定位，也被证明对动态物体检测有帮助。

![](img/79c23a0dc4bed69de114a2875488268a.png)

*LiDAR 点云的多任务语义分割（来源：* [*LidarMTL*](https://arxiv.org/abs/2103.04056)*)*

## 以自由空间为中心的表示

[Freespace Forecaster](https://openaccess.thecvf.com/content/CVPR2021/html/Hu_Safe_Local_Motion_Planning_With_Self-Supervised_Freespace_Forecasting_CVPR_2021_paper.html) 和 [其可微分版本](https://arxiv.org/abs/2210.01917) 使用以自由空间为中心的表示来预测用于运动规划的可驾驶空间。这些方法从车辆作为中心，通过简单的射线投射获取地面和障碍物的点云或网格信息，使用极坐标计算可达关系。这种表示方式与 2D 透视空间中的 Stixel 表示非常相似，最近的障碍物位于每个区间内（2D 中的 stixel 和 3D 中的极角区间）。

![](img/9a8a65540e5443f77b64f120d0cd8d53.png)

*以自由空间为中心的表示用于自由空间预测（来源：* [*可微分射线投射*](https://arxiv.org/abs/2210.01917)*)*

## 占用网格和场景流表示

在基于 LiDAR 的算法中，最通用的方法使用占用网格和场景流来分别表示一般障碍物的位置和移动。两个具有代表性的论文是 [MotionNet](https://arxiv.org/abs/2003.06754) 和 [PointMotionNet](https://openaccess.thecvf.com/content/CVPR2022W/WAD/papers/Wang_PointMotionNet_Point-Wise_Motion_Learning_for_Large-Scale_LiDAR_Point_Clouds_Sequences_CVPRW_2022_paper.pdf)。

动态物体检测和跟踪任务在公共数据集中很常见，使得学术研究人员能够生成占用网格和场景流的真实标签。占用表示，*理论上*，可以检测一般障碍物，涵盖具有任意形状的未知物体，以及静态障碍物和道路结构。然而，*在实践中*，量化算法的有效性是具有挑战性的，因为公共数据集中大多数物体是规则的。**为了进一步推进这一领域的研究，需要一个检测一般障碍物的数据集和基准。**

![](img/025ca3e9e5f0191987fd53d7c4875679.png)

*点云中的占用与流量预测（来源：* [MotionNet](https://arxiv.org/abs/2003.06754)*)*

> 乍一看，物体检测算法生成的真实数据可以为更强大、更灵活的占用预测算法提供支持似乎不符合直觉。然而，有两个重要方面需要考虑。首先，物体检测可以帮助完成真实数据标注过程中的繁重工作和启动工作，而**在实际生产环境中仍需人工质量保证和细微调整**。其次，算法的制定起着关键作用。占用预测的制定使其更具灵活性，能够学习物体检测可能遗漏的细微差别。

基于激光雷达的占用算法与特斯拉提出的占用网络（Occupancy Network）不同。占用网络的概念是以视觉为中心的算法。

# 2D 到 3D 的方法（BEV 感知及更多）

多摄像头 BEV 感知框架（参见我之前关于 BEV 感知的博客文章）通过简化多摄像头后处理和后融合步骤，将视觉 3D 感知性能提升到一个新的水平。此外，该框架成功地在表达空间中统一了摄像头和 LiDAR 算法，为传感器融合提供了便捷的框架，包括前融合和后融合。

在 BEV 空间中检测道路的物理边缘是可驾驶空间检测任务的一个子集。虽然道路边缘和车道线可以用矢量线表示，但道路边界缺乏如平行性、固定车道宽度以及消失点的交集等约束，使得其形状和位置更加灵活。这些更加自由和多样的道路边缘可以通过几种方式建模：

1.  基于热图的：由类似语义分割的解码器生成热图。热图需要处理为矢量元素，以供下游组件使用。

1.  基于体素的：热图方法的扩展。热图中的 2D BEV 网格扩展为 3D 体素。

1.  基于矢量的：生成基于原始几何元素（如多边形和多边形）的矢量化输出。这些输出可以直接传递给下游使用。

## 热图解码器

基于语义分割的方法可以根据目标建模方法分为两类：**道路边界**语义分割和**道路布局**语义分割。前者，如[HDMapNet](https://arxiv.org/abs/2107.06307)，可以预测车道线，同时输出道路边缘。神经网络的输出是一个热图，需要进行二值化和聚类，以生成可用于下游预测和调节的矢量输出。

![](img/b0c02ae42a48093216acab46a7e4b111.png)

*HDMapNet 的架构图（来源：* [HDMapNet](https://arxiv.org/abs/2107.06307))

其他方法基于道路结构的语义分割，如[PETRv2](https://arxiv.org/abs/2206.01256)、[CVT](https://arxiv.org/abs/2205.02833)和[Monolayout](https://arxiv.org/abs/2002.08394)。输出是道路本身，其边缘是道路边界。神经网络输出仍然是需要二值化的热图，通过边缘操作可以得到向量化的道路边界。如果下游应用可以直接消耗道路结构本身，如基于占用网格的规划，使用这种感知方法会更直接。然而，这一话题更多与规制和控制相关，因此我在此不会详细展开。

![](img/740fdc45a50ac8e6910c8c55fbc6867a.png)

*CVT 的架构图（来源:* [CVT](https://arxiv.org/abs/2205.02833))

## 体素解码器（语义占用预测）

随着特斯拉在 2022 年提出占用网络的提案，2023 年初，单纯基于视觉的占用预测取得了爆炸性增长。这种体素输出表示可以看作是热图表示的扩展，每个热图网格位置预测一个额外的高度维度。

在这一领域，一个值得注意的工作是[SurroundOcc](https://arxiv.org/abs/2303.09551)。它首先设计了一个自动化流程，从稀疏点云中生成密集占用真值，然后利用这些密集真值来监督从多相机图像流中学习密集占用网格。有关各种方法的详细比较和工业应用中的潜在障碍，请参阅[这篇语义占用预测的文献综述](https://medium.com/p/16a46dbd6f65)。

[## 以视觉为中心的自动驾驶语义占用预测](https://vision-centric-semantic-occupancy-prediction-for-autonomous-driving-16a46dbd6f65?source=post_page-----ef1a6aa4dc15--------------------------------)

### 2023 年上半年的学术“占用网络”文献综述

[towardsdatascience.com](https://vision-centric-semantic-occupancy-prediction-for-autonomous-driving-16a46dbd6f65?source=post_page-----ef1a6aa4dc15--------------------------------) ![](img/40b7ee9cf193666053222c4943a47f89.png)

语义占用预测的标注和训练流程（来源：[SurroundOcc](https://arxiv.org/abs/2303.09551)）

## 向量解码器

基于热图和体素的方法都依赖于语义分割，并且需要大量后处理来满足下游需求。相比之下，基于直接向量输出的方法更加直接。代表性的方法包括[STSU](https://arxiv.org/pdf/2110.01997.pdf)、[MapTR](https://arxiv.org/abs/2208.14437)和[VectorMapNet](https://arxiv.org/abs/2206.08920)，这些方法直接输出**向量化道路边缘**。这种方法可以视为基于锚点的目标检测方法的变体，其中基本几何元素是具有 2N 自由度的多段线或多边形，其中 N 是多段线或多边形的点数。[MapTR](https://arxiv.org/abs/2208.14437)和[VectorMapNet](https://arxiv.org/abs/2206.08920)就是这样的例子。值得一提的是，[STSU](https://arxiv.org/pdf/2110.01997.pdf)使用了具有 3 个控制点的 Bezier 曲线，这虽然新颖，但不如多段线和多边形灵活，其当前效果也不如后者。

![](img/9bc1ea106634a4efdf7f0e1dbcbf4776.png)

*BEV 解码器中的向量化（来源：* [MapTR](https://arxiv.org/abs/2208.14437))

## 摄像头和激光雷达融合

上述 2D 到 3D 方法利用了摄像头图像中的丰富纹理和语义信息来推断自车周围的 3D 环境。虽然激光雷达点云数据可能缺乏这些丰富的语义，但它提供了准确的 3D 位置测量信息。

多摄像头 BEV 空间和激光雷达 BEV 空间可以在 BEV 融合中轻松统一。例如，在[HDMapNet](https://arxiv.org/abs/2107.06307)中，激光雷达单独方法和摄像头-激光雷达融合方法也与摄像头单独基线进行了比较。尽管摄像头在 BEV 定位中可能表现稍差于激光雷达单独方法，但多摄像头 IoU 指标在车道分隔线和行人过街处仍然更好，而激光雷达在检测道路边界方面更优。这是可以理解的，因为道路边界通常伴随高度变化，更容易通过主动激光雷达测量来检测。

![](img/a07b5275905a3047ff7ab9d55ca749dc.png)

*2D 到 3D DS 算法中视觉和激光雷达的性能比较（来源：* [HDMapNet](https://arxiv.org/abs/2107.06307))

# 公共数据集

目前在自动驾驶领域，没有广泛接受的可驾驶空间数据集。然而，有一些相关任务，如 3D 到 3D 方案中的点云分割和 2D 到 3D 方案中的 HD 地图预测。不幸的是，生成 3D 点云分割和 HD 地图的成本较高，这限制了学术研究使用少数公共数据集。这些数据集包括 NuScenes、Waymo、KITTI360 和 Lyft。这些数据集提供了 3D 点云分割和标注，并包含一些道路信息，如路面、路边和人行道。Lyft 数据集还包括道路区域的地图信息，有助于理解道路布局。

需要建立公共数据集和评估指标基准，以促进该领域的发展。特别需要关注两件事。

+   **长尾角落情况。** 必须关注困难样本的检测效果，以确保系统的可靠性。然而，长尾数据在不同区域和场景中可能有所不同，收集和标记这些数据需要时间和技术专长。值得探索平衡小样本数据和提高学习效果的方法。

+   **输出格式。** 3D 可驾驶空间的定义和实现与下游消费逻辑及自动驾驶系统设计密切相关。行业中很难建立统一标准，也不常作为学术研究或公共数据集竞赛中明确和独立定义的模块。**多边形可能是一种足够灵活的格式。** 通过物体检测得分和时间一致性可以评估跨帧检测到的多边形，因为下游需要一致的形状以实现精确的车辆操作。

# 重点

+   在学术界尚未有普遍接受的可驾驶空间定义或评估指标。重新标记公共数据集是一种可能的方法，但需要丰富长尾角落情况。

+   2D 图像空间像素级可驾驶空间检测依赖于来自 IPM 或其他模块的深度信息，但位置误差随着距离增加。在 3D 空间中，LiDAR 提供高几何精度但语义分类较弱。它可以通过地面拟合和其他方法检测道路边缘和一般障碍物，还可以通过光线投射自由空间或占用表达识别未知的动态和静态障碍物。

+   2D 到 3D BEV 感知方法因其性价比高和强大的表示能力而具有前景。然而，缺乏可驾驶空间的标准导致了各种输出格式。输出格式取决于下游规划和控制的要求。

这是关于可驾驶空间的第二篇文章，重点介绍了最近的学术进展。第一篇文章探讨了可驾驶空间的概念。在下一篇文章中，我们将讨论可驾驶空间的当前工业应用，包括如何将其扩展到自动驾驶之外的一般机器人领域。（更新：第三篇博客文章已经发布。）

[## 自动驾驶中的可驾驶空间——行业](https://medium.com/@patrickllgc/drivable-space-in-autonomous-driving-the-industry-7a4624b94d41?source=post_page-----ef1a6aa4dc15--------------------------------)

### 截至 2023 年的行业应用最新趋势

[medium.com](https://medium.com/@patrickllgc/drivable-space-in-autonomous-driving-the-industry-7a4624b94d41?source=post_page-----ef1a6aa4dc15--------------------------------)

> 注意：本博客文章中的所有图片要么由作者创建，要么来自公开的学术论文。有关详细信息，请参见说明。

# 参考文献

+   [**StixelNet**：一种用于障碍物检测和道路分割的深度卷积网络](http://www.bmva.org/bmvc/2015/papers/paper109/paper109.pdf)，BMVC 2015

+   [实时基于类别和通用的自动驾驶障碍物检测](https://openaccess.thecvf.com/content_ICCV_2017_workshops/papers/w3/Garnett_Real-Time_Category-Based_and_ICCV_2017_paper.pdf)，ICCV 2018

+   [**DS-Generator**：从立体图像中学习无碰撞空间检测](https://arxiv.org/abs/2012.07890)，IEEE/ASME Transactions on Mechatronics 2022

+   **RPP**：[使用深度全卷积残差网络与金字塔池化进行可驾驶道路分割](https://link.springer.com/article/10.1007/s12559-017-9524-y)，Cognitive Computation，2018

+   [**STSU**：基于车载图像的结构化鸟瞰视图交通场景理解](https://arxiv.org/pdf/2110.01997.pdf)，ICCV 2021

+   [**HDMapNet**：在线高清地图构建与评估框架](https://arxiv.org/abs/2107.06307)，Arxiv 2021

+   [**PETRv2**：一种统一的多摄像头图像 3D 感知框架](https://arxiv.org/abs/2206.01256)，Arxiv 2022

+   [**CVT**：用于实时地图视图语义分割的跨视图变换器](https://arxiv.org/abs/2205.02833)，CVPR 2022

+   [**Monolayout**：从单张图像中获得的模态场景布局](https://arxiv.org/abs/2002.08394)，WACV 2020

+   [Darpa 城市挑战 2007](https://link.springer.com/article/10.1007/BF03224975)，ATZ worldwide 2008

+   [DARPA 城市挑战：城市交通中的自动驾驶车辆](https://link.springer.com/book/10.1007/978-3-642-03991-1)，springer，2009

+   [基于高斯过程的自动驾驶地面实时分割](https://link.springer.com/article/10.1007/s10846-013-9889-4)，Journal of Intelligent & Robotic Systems，2014

+   [基于高斯过程的倾斜地形地面分割](https://arxiv.org/abs/2111.10638)，ICRoM 2021

+   [**LidarMTL**：一种简单高效的多任务网络，用于 3D 物体检测和道路理解](https://arxiv.org/abs/2103.04056)，Arxiv 2021

+   **Freespace Forecaster：**[基于自监督的安全局部运动规划与自由空间预测](https://openaccess.thecvf.com/content/CVPR2021/html/Hu_Safe_Local_Motion_Planning_With_Self-Supervised_Freespace_Forecasting_CVPR_2021_paper.html)，CVPR 2021

+   [**MotionNet**：基于鸟瞰图的自动驾驶联合感知与运动预测](https://arxiv.org/abs/2003.06754)，CVPR 2020

+   [**PointMotionNet**：针对大规模 LiDAR 点云序列的逐点运动学习](https://openaccess.thecvf.com/content/CVPR2022W/WAD/papers/Wang_PointMotionNet_Point-Wise_Motion_Learning_for_Large-Scale_LiDAR_Point_Clouds_Sequences_CVPRW_2022_paper.pdf)，CVPR 2022

+   [**MapTR**：用于在线矢量化高清地图构建的结构化建模与学习](https://arxiv.org/abs/2208.14437)，ICLR 2023

+   [**VectorMapNet**：端到端矢量化高清地图学习](https://arxiv.org/abs/2206.08920)，Arxiv 2022

+   [**SurroundOcc**：用于自动驾驶的多摄像头 3D 占用预测](https://arxiv.org/abs/2303.09551)，Arxiv 2023
