# 场景图生成及其在机器人学中的应用

> 原文：[https://towardsdatascience.com/scene-graph-generation-and-its-application-in-robotics-f9ba864aa572?source=collection_archive---------6-----------------------#2023-10-10](https://towardsdatascience.com/scene-graph-generation-and-its-application-in-robotics-f9ba864aa572?source=collection_archive---------6-----------------------#2023-10-10)

## 让我们来进行一次关于用交互式图形表示法可视化图像的*简短*讨论吧！

[](https://medium.com/@ritanshiagarwal?source=post_page-----f9ba864aa572--------------------------------)[![Ritanshi Agarwal](../Images/68c99a95cade2a64988dc1f84f008076.png)](https://medium.com/@ritanshiagarwal?source=post_page-----f9ba864aa572--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f9ba864aa572--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f9ba864aa572--------------------------------) [Ritanshi Agarwal](https://medium.com/@ritanshiagarwal?source=post_page-----f9ba864aa572--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb2a8c7a68d54&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscene-graph-generation-and-its-application-in-robotics-f9ba864aa572&user=Ritanshi+Agarwal&userId=b2a8c7a68d54&source=post_page-b2a8c7a68d54----f9ba864aa572---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f9ba864aa572--------------------------------) · 12分钟阅读 · 2023年10月10日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff9ba864aa572&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fscene-graph-generation-and-its-application-in-robotics-f9ba864aa572&source=-----f9ba864aa572---------------------bookmark_footer-----------)

场景图生成是生成场景图的过程，场景图以图形的形式包含图像的视觉理解。它具有表示对象和它们关系的节点和边。关于场景的上下文信息可以帮助语义场景理解。尽管存在如现实世界场景的不确定性或缺乏标准数据集等挑战，研究人员正尝试将场景图应用于机器人领域。本文包含两个主要部分：一部分讨论了如何通过对基于区域的卷积神经网络的两种最先进方法的深入讨论来获取场景图，另一部分涉及场景图在机器人规划中的应用。在前一部分中，文章进一步解释了仅有数值度量来评估模型是不够的，无法进行适当的性能分析。后一部分包括对一种利用场景图概念进行视觉上下文感知机器人规划的方法的详细解释。

![](../Images/bd1ee322371527cfd647250bb3bdcf12.png)

图1a：创建场景图的节点和边的描绘（背景图片来自[TruckRun](https://unsplash.com/@truckrun_ebike_systems?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)于[Unsplash](https://unsplash.com/photos/a-couple-of-people-riding-bikes-through-a-forest-g1YAFw6_lHo?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)）

# 1\. 介绍

在考虑用文字描述图像元素时，图像分割、对象分类、活动识别等知名的活跃技术在过程中发挥了重要作用。场景图生成（SGG）是一种生成场景图（SGs）的方法，通过将对象（如人类、动物等）及其属性（如颜色、衣物、车辆等）描绘为节点，并将对象之间的关系描绘为边。图1a和1b展示了从RGB图像生成的场景图通常是什么样的。可以看到，不同颜色的节点描绘了图像中的主要对象（一个男性，一个女性和一辆车辆），关于这些对象的信息由具有相同节点颜色变体的节点显示，边缘则用动词标注，显示连接，或者说节点之间的关系。研究人员在过去几年中一直致力于使SGs即使对图像中的小细节也能实现。

![](../Images/4c2ab148202efa281bd3da401c6fb11e.png)

图1b：场景图生成中的过程和主要挑战

本文将在接下来的章节中广泛讨论生成场景图的两种方法，以便更好地理解。写作结构包括六个部分。第2节简要介绍了SGG及其先前的应用，包括图像和视频字幕。第3节描述了最先进的方法及其模型描述。第4节讨论并比较了这些方法的实验分析。第5节讨论了SGs在机器人领域的一些可能应用，主要集中在机器人规划方面，还讨论了准确的SGs如何使远程机器人、人机协作等应用更易于实现。最后，第6节总结了整个文献。

# 2\. 相关工作

SGs有着不同的应用历史，例如图像和视频字幕、视觉问答、3D场景理解、机器人领域的许多应用等。[10] [3]。最近的大多数方法包括机器学习方法，如基于区域的卷积神经网络（R-CNN）[8]、更快的R-CNN [9]、递归神经网络（RNN）[7]等。[7]提出了一种结构，将图像作为输入，然后传递包含上下文信息的消息，并使用RNN进行预测的细化。另一种方法[6]使用图像的能量模型生成综合的场景图。SGG的数据集也是通过不同的真实世界场景、图像和视频获得的。这里提到的数据集是Visual Genome [4]的一个子集[7]，包含108,073张人类注释的图像。

# 3\. 最先进的方法

## 3.1 图形 R-CNN 方法

J. Yang等[8]提出了一种模型，首先考虑所有由边连接的对象和关系，然后使用称为*相关性*的参数来排除不太可能的关系。例如，*车*和*轮子*之间的关系比*建筑物*和*轮子*之间的关系更有可能存在。

论文展示了一个新颖的框架，即Graph R-CNN，引入了一种新的评估指标SGGen+。该模型由三个模块组成：首先提取对象节点，其次移除不太可能的边，最后在剩余图中传播图上下文，以便生成最终的场景图。创新点在于模型的第二个和第三个模块，具有一个新的关系提议网络（RePN），该网络计算*相关性*，进一步帮助移除不太可能的关系，也称为关系边修剪，以及一个注意力图卷积网络（aGCN），它构成了高阶上下文的传播，从而更新场景图，赋予其最终修饰。基本上，最终的模块标记生成的场景图。整个概念的概率分布可以通过下面的方程表示，其中对图像 *I*，*O* 是对象的集合，*R* 是对象之间的关系，*Rl* 和 *Ol* 分别是关系和对象标签。

> Pr(G|I) = Pr(O|I) * Pr(R|O, I) * Pr(Rl, Ol|O, R, I)

图 2 展示了整个算法的图示形式，以及所有三个阶段如何依次工作。该流程可以描述为：使用 Faster R-CNN 进行对象检测，然后生成一个包含所有定义关系的初始图，然后基于 *相关性* 分数修剪不太可能的边缘，最后使用 aGCN 进行精炼。

![](../Images/e95853623c01823b7da7f73796aa1bb6.png)

图 2: 图形 R-CNN [8] 工作说明

## 3.2 堆叠 Motif 网络（MotifNet）

R. Zellers 等人[9] 提出了一个网络，该网络依次执行边界框收集、对象识别和关系识别。这种方法展示了对 Faster R-CNN 和长短期记忆（LSTM）网络的密集使用。与图形 R-CNN 相比，该方法基于检测对象之间的关系并添加关系边。

这里的想法可以看作是一个条件概率方程，如下方程所示，表示在给定 *I* 的情况下找到图 *G* 的概率为找到边界框数组 *B* 的概率、在给定 *B* 和 *I* 的情况下获得对象 *O* 的概率，以及在给定 *B*、*O* 和 *I* 的情况下获得关系 *R* 的概率的乘积。

> Pr(G|I) = Pr(B|I) * Pr(O|B,I) * Pr(R|B,O,I)

首先，为了找到边界框的集合，他们也使用 Faster R-CNN，如 [8] 所示，见图 3 的左侧。它使用 VGG 主干结构进行对象识别，同时确定 *I* 中的边界框。然后，模型继续使用图 3 中的对象上下文层所示的双向 LSTM 层。该层块生成每个边界框区域的对象标签，*bi* ∈ *B*。另一个双向 LSTM 层块用于确定作为边缘上下文的关系，如同一图中所示。输出结果计算为确定边缘标签，这些标签清晰地描述了对象及其之间的关系的外积。详细的模型结构可以在 [9] 中学习。

![](../Images/418fe06b2b6453556f0f2ede61b2c4d7.png)

图 3: MotifNet [9] 结构流程图说明（狗的图片由 [Justin Veenema](https://unsplash.com/@justinveenema?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 提供，来源于 [Unsplash](https://unsplash.com/photos/white-and-black-american-pit-bull-terrier-at-daytime-NH1d0xX6Ldk?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)）

# 4\. 最先进方法中的实验

## 4.1 实验设置

两种方法的设置如下所述：

**图形 R-CNN：** 使用带有 VGG16 主干的更快 R-CNN 来确保对象检测，通过 PyTorch 实现。对于 RePN 实现，使用多层感知机结构通过两个投影函数（分别针对主题和对象关系）分析相关性得分。使用两个 aGCN 层，一个用于特征级别，其结果传递给另一个语义级别的层。训练分两个阶段进行，首先训练对象检测器，然后联合训练整个模型。

**MotifNet：** 通过使用零填充方法，将输入到边界框检测器的图像调整为 592x592 的大小。所有 LSTM 层都经过高速公路连接。分别为对象和边缘上下文使用了两个和四个交替的高速公路 LSTM 层。边界框区域的排序可以通过中央 x 坐标、最大非背景预测、边界框大小或随机洗牌来完成。

主要挑战是使用共同的数据集框架分析模型，因为不同的方法使用不同的数据预处理、拆分和评估。然而，讨论的方法，Graph R-CNN 和 MotifNet 使用了[7]中公开的数据处理方案和拆分。这个 Visual Genome 数据集中有 150 个对象类别和 50 个关系类别[4]。

> **Visual Genome 数据集 [4] 总结：**
> 
> 人工标注的图像
> 
> 超过 100,000 张图像
> 
> 150 个对象类别
> 
> 50 个关系类别
> 
> 每张图像在场景图中大约有 11.5 个对象和 6.2 个关系

## 4.2 实验结果

![](../Images/045f9e42cf75bc160863454ce696921b.png)

表 1：性能比较

**定量比较：** 两种方法都使用召回率指标评估了它们的模型。表 1 通过不同的定量指标展示了这两种方法的比较。(1) 谓词分类（PredCls）表示识别对象之间关系的性能，(2) 短语分类（PhrCls）或[9]中的场景图分类表示观察对象和关系类别的能力，(3) 场景图生成（SGGen）或[9]中的场景图检测表示将对象与检测到的关系结合的性能。在[8]中，他们通过全面的SGGen（SGGen+）来提升后者的指标，该指标包括了识别*男人*为*男孩*的可能性，从技术上讲，这是一个失败的检测，但如果对该对象的所有关系都被成功检测到，那么应视为成功结果，从而提高SGGen指标值。

根据表1，MotifNet [9] 在分别分析对象、边缘和关系标签时表现相对更好。然而，使用第二种方法，即 Graph R-CNN [8]，生成给定图像的整个图形更为准确。它还表明，拥有全面的输出指标可以更好地分析场景图模型。

**定性比较：** 在神经结构 [9] 中，他们分别考虑定性结果。例如，关系边缘的检测 *wearing* 作为 *wears* 被归为失败检测类别。这表明该模型 [9] 的表现优于输出指标所显示的效果。另一方面，[8] 将这种结果理解纳入他们的全面SGGen (SGGen+) 指标中，该指标已考虑了可能不是完全失败的检测。

# 5\. 机器人学中的应用

本文将概述有助于机器人应用的场景理解，特别是机器人规划。机器人学的广泛领域包括自动室内制图、遥控机器人、用于远程医疗等目的的人机控制，以及更多应用，形成了一个非常深奥的场景图应用领域。

当人类和机器人在类似的工作空间中合作完成指定任务时，这称为人机协作（HRC），而协作机器人被称为*cobots*。拥有语义信息以及对象检测可以使任务变得更容易。 [5] 展示了一种使用SGG进行HRC安全分析的方法。另一个应用涉及遥控机器人，包括社会互动技能，例如由远程位置的人操控的移动杆上的会议聊天室。应用领域包括老年护理、即使在残疾情况下也能参加活动等。[2] 讨论了一种方法，其中场景理解帮助用户清晰地分析和控制远程环境。

## 5.1 机器人规划

将特定的运动任务分配给机器人，如将一个物体从一个地方移动到另一个地方，并期望机器提供平稳服务，需要高度准确的规划和开发。SGG 在生产服务机器人中发挥重要作用，因为它为机器人提供了场景的深入抽象图像，这进一步帮助机器人精确定位物体。[1] 使用局部场景图来感知全球视图以达到目标。它描述了一个包括在室内环境中寻找物体（如水果）的常见任务的机器人规划。

**机器人规划的场景分析 (SARP)** [1] 是一种算法，旨在使机器人利用视觉上下文信息来完成计划任务。该方法利用场景图通过MotifNet [9] 建模全局场景理解，MotifNet算法在第3.2节中讨论。模型的框图可以在[1]中查看。该模型使用MotifNet生成本地场景图，然后利用上下文信息来分析观察和行动之间的不确定性，并更新机器人的信念。后续过程使用部分可观察马尔可夫决策过程（PO-MDP）框架完成。MDP以顺序决策著称。在框图中，离线训练的场景图网络生成全局场景图，并将结果输入到马尔可夫过程。

***多个本地SGs — — — — — →全局上下文SGs — — — — →目标搜索***

该方法在精准且最小时间内定位散布在区域内的物体。机器人被提供了一个场景图数据集、一个已经训练好的场景图网络和一个用于指令移动的领域地图。结果是，机器人在区域内进行目标搜索，为此，它在每一步都创建场景图以获取上下文信息，如[发表的IEEE文章](https://ieeexplore.ieee.org/abstract/document/9730031) [1]中的图2和图3所示。这是一个演示，展示了机器人被分配任务以定位*香蕉*。它在每个时间点都更新全局场景图，除了T=4，因为在那个帧中没有新的物体实例。可以看出，拥有本地场景图帮助机器人确定目标搜索的全局上下文。根据[1]中的性能比较，SARP在成功率和行动成本方面都优于基线方法。然而，该模型可以进一步扩展，加入人脸识别功能，并且在测试过程中更换或添加物体时也需要进行分析。

# 6\. 结论

这篇文章讨论了 SGG 的不同方法，即生成包含场景语义信息的图形，以及其一些机器人应用。对象和关系的上下文信息让我们对场景有了非常详细的理解。MotifNet 使用 LSTM 网络检测并添加对象之间的关系作为边缘上下文，而 Graph R-CNN 通过使用*相关性*分数来消除不太可能的关系边缘，从而形成场景图。虽然最近的方法中使用了几种版本的 Visual Genome 数据集，但 MotifNet 和 Graph R-CNN 在相同的数据集模型上评估了它们的模型，这使得它们的定量比较是合理的。总体而言，对定量和定性测量进行了分析，可以看出，数字并不能完全说明问题，像*男人*而不是*男孩*这样的并非失败的场景，如果其所有关系边缘都正确检测到，也应被视为成功结果。在评估 SGG 方法时，考虑图像或场景的深入语义描述将有助于图像和视频相关应用。一个方法在机器人规划应用中得到了广泛讨论，该方法使用场景图来更好地理解视觉场景，以便机器人在长远中执行任务（例如，在室内环境中定位物体）。

好了，这就是关于将图像转换为互动图形文本的*简短*研究讲座的全部内容。我希望大家玩得愉快（可能有一点点！）。我会带着另一篇研究文章回来，总结近期的一些有趣研究（可能涉及通信网络、网络模拟器（NS3），或者Wi-Fi 7😉）。

祝你们研究愉快！！

此致，

Ritanshi

# 参考文献

1.  S. Amiri, K. Chandan 和 S. Zhang。《Reasoning with scene graphs for robot planning under partial observability》。IEEE Robotics and Automation Letters，7(2):5560–5567，2022年。

1.  F. Amodeo, F. Caballero, N. Díaz-Rodríguez 和 L. Merino。《Og-sgg: Ontology-guided scene graph generation. A case study in transfer learning for telepresence robotics》。IEEE Access，页码 1–1，2022年。

1.  X. Chang, P. Ren, P. Xu, Z. Li, X. Chen 和 A. Hauptmann。《A comprehensive survey of scene graphs: Generation and application》。IEEE Transactions on Pattern Analysis and Machine Intelligence，45(1):1–26，2023年。

1.  R. Krishna, Y. Zhu, O. Groth, J. Johnson, K. Hata, J. Kravitz, S. Chen, Y. Kalantidis, L.-J. Li, D. A. Shamma 等。《Visual genome: Connecting language and vision using crowdsourced dense image annotations》。International Journal of Computer Vision，123(1):32–73，2017年。

1.  H. Riaz, A. Terra, K. Raizer, R. Inam 和 A. Hata。《Scene understanding for safety analysis in human-robot collaborative operations》。在 2020年第六届控制、自动化与机器人国际会议（ICCAR），页码 722–731，2020年。

1.  M. Suhail, A. Mittal, B. Siddiquie, C. Broaddus, J. Eledath, G. Medioni, 和 L. Sigal。基于能量的场景图生成学习。发表于2021年IEEE/CVF计算机视觉与模式识别会议（CVPR），第13931–13940页，2021年。

1.  D. Xu, Y. Zhu, C. B. Choy, 和 L. Fei-Fei。通过迭代消息传递生成场景图。发表于IEEE计算机视觉与模式识别会议论文集，第5410–5419页，2017年。

1.  J. Yang, J. Lu, S. Lee, D. Batra, 和 D. Parikh。用于场景图生成的图形r-cnn。发表于欧洲计算机视觉会议（ECCV）论文集，第670–685页，2018年。

1.  R. Zellers, M. Yatskar, S. Thomson, 和 Y. Choi。神经模式：具有全局上下文的场景图解析。发表于2018年IEEE/CVF计算机视觉与模式识别会议，第5831–5840页，2018年。

1.  G. Zhu, L. Zhang, Y. Jiang, Y. Dang, H. Hou, P. Shen, M. Feng, X. Zhao, Q. Miao, S. A. A. Shah等。场景图生成：一项综合调查。arXiv预印本arXiv:2201.00443，2022年。
