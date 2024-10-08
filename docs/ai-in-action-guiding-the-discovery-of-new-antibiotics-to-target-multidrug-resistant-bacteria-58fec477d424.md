# AI 行动：指导新抗生素的发现以对抗多药耐药细菌

> 原文：[`towardsdatascience.com/ai-in-action-guiding-the-discovery-of-new-antibiotics-to-target-multidrug-resistant-bacteria-58fec477d424`](https://towardsdatascience.com/ai-in-action-guiding-the-discovery-of-new-antibiotics-to-target-multidrug-resistant-bacteria-58fec477d424)

## 人工智能在重要问题上的应用

## 通过我对一篇新论文的通俗描述，了解人工智能在化学应用中的数据科学

[](https://lucianosphere.medium.com/?source=post_page-----58fec477d424--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----58fec477d424--------------------------------)[](https://towardsdatascience.com/?source=post_page-----58fec477d424--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----58fec477d424--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----58fec477d424--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----58fec477d424--------------------------------) ·阅读时间 6 分钟·2023 年 6 月 3 日

--

![](img/09c837aa419748da806b07e33be58c6c.png)

Dall-E-2 对人工智能如何帮助对抗细菌的“看法”。继续阅读以了解这一实际工作如何开始在现实中发生。

**发现新抗生素以对抗耐药细菌是一项重大需求，因为细菌对现有抗生素的耐药性不断增加，但这是一项极具挑战性且成本高昂的工作。因此，任何加快新抗生素研发速度和效率的新方法都被*非常*欢迎。**

**最近发表在《自然化学生物学》上的一项研究展示了机器学习（ML）在加速抗生素发现中的力量。研究人员使用先进的算法筛选了数千种分子，并识别出一种有前景的化合物，称为** [**abaucin**](https://en.wikipedia.org/wiki/Abaucin) **，它专门针对一种名为*鲍曼不动杆菌*的病原体，这种病原体现在对医院常用来治疗感染的大量抗生素有耐药性——这被称为“多药耐药病原体”。在抗生素（搜索和）研究中应用机器学习的这一新突破凸显了人工智能在革命化该领域中的潜力，预示着未来更快、更有信心且成本更低的抗生素开发。**

# 该模型发现的生物学和内容

由于*鲍曼不动杆菌*的多药耐药性，传统的抗生素筛选方法在发现新抗生素方面效果有限。正如我之前提到的，寻找新药是一个紧迫的问题：

[](https://medium.com/advances-in-biological-science/new-antibiotics-are-hard-and-expensive-to-develop-so-scientists-are-trying-these-alternatives-b4af09ff22f8?source=post_page-----58fec477d424--------------------------------) [## 新抗生素的开发困难且昂贵，因此科学家们正在尝试这些替代方案

### 包括一些用反应性物质激活的水，重新利用已经用于其他用途的药物…

medium.com](https://medium.com/advances-in-biological-science/new-antibiotics-are-hard-and-expensive-to-develop-so-scientists-are-trying-these-alternatives-b4af09ff22f8?source=post_page-----58fec477d424--------------------------------) [](https://medium.com/advances-in-biological-science/we-may-finally-have-a-new-class-of-antibiotics-3e99ad02f07d?source=post_page-----58fec477d424--------------------------------) [## 我们可能终于有了新一类抗生素

### 它攻击细菌的外壁和内膜

medium.com](https://medium.com/advances-in-biological-science/we-may-finally-have-a-new-class-of-antibiotics-3e99ad02f07d?source=post_page-----58fec477d424--------------------------------) [](https://medium.com/advances-in-biological-science/when-microorganisms-fight-each-other-we-have-a-chance-to-discover-novel-antibiotics-heres-a-new-48ea9d94aa77?source=post_page-----58fec477d424--------------------------------) [## 当微生物互相对抗时，我们有机会发现新型抗生素 - 这里有一个新的…

### 来自自然的例子，以及对抗沙门氏菌的新候选药物

medium.com](https://medium.com/advances-in-biological-science/when-microorganisms-fight-each-other-we-have-a-chance-to-discover-novel-antibiotics-heres-a-new-48ea9d94aa77?source=post_page-----58fec477d424--------------------------------)

在过去几年中，机器学习技术开始通过使用消息传递网络、变压器和扩散模型提供新颖且更高效的化学空间探索方法。这些新方法自然会增加发现有效抗菌分子的机会。在我在*自然化学生物学*上发表的研究中，研究人员筛选了大约 7,500 种分子，以识别那些在实验室测试中抑制*鲍曼不动杆菌*生长的分子，并建立了一个预测性的机器学习模型，通过该模型他们提出了新的潜在抗生素 abaucin：

[](https://www.nature.com/articles/s41589-023-01349-8?source=post_page-----58fec477d424--------------------------------) [## 深度学习引导的抗生素发现针对鲍曼不动杆菌 - 自然化学生物学…

### 鲍曼不动杆菌是一种医院内的革兰氏阴性病原体，通常表现出多重耐药性。发现…

www.nature.com](https://www.nature.com/articles/s41589-023-01349-8?source=post_page-----58fec477d424--------------------------------)

Abaucin 不仅展示了作为潜在抗生素的有希望的特性，而且它还对*巴尔曼氏菌*表现出选择性活性，使其成为一种窄谱抗生素，对其他细菌物种的影响最小。这种特异性对于最小化对身体自然微生物群（通常存在于我们的皮肤、肠道等地方，并对我们的健康至关重要）的干扰是至关重要的。关于其作为抗生素实际使用的前景更为有希望的是，研究报告显示 abaucin 在小鼠伤口模型中有效控制了*巴尔曼氏菌*感染，表明其治疗潜力。

# 数据科学和机器学习在发现背后的作用

在研究的核心，一种消息传递神经网络使用了能够（或不能）抑制*巴尔曼氏菌*生长的分子数据集进行训练。这个数据集本身是在同一工作中测量和报告的。随后，训练好的模型被用来对药物重定位中心进行预测，这是一个全面的、注释过的 FDA 批准化合物资源，旨在进行研究，将已批准的药物重新用于新的治疗。这里的重点是识别对*巴尔曼氏菌*有活性的结构新分子。这个过程导致了如上所述的 abaucin 的发现。

机器学习模型利用了分子的图表示，通过消息传递步骤迭代地交换关于原子周围化学环境的信息。学习到的特征与使用 RDKit 计算的固定分子特征相结合。用于训练和预测的数据集包含筛选出对*巴尔曼氏菌*生长抑制的分子，正是在同一工作中。

模型的关键在于如何将表示每个分子结构的图转换为连续的向量表示，通过在一系列消息传递步骤中迭代地交换相邻原子和键之间的局部化学信息。通过累积各种局部化学区域的向量表示，模型获得了整个化合物的全面表示。为了增强学习到的特征，使用 RDKit 计算了固定的分子特征，RDKit 是化学信息学领域中最重要的库之一。最终的向量结合了学习到的和计算的特征，作为输入用于一个前馈神经网络，该网络经过训练用于预测分子的抗菌属性，作为分类器。

用于训练的数据集包括对 7,684 种小分子进行筛选的结果，评估它们对*A. baumannii*生长的影响。筛选实验结果显示，480 种分子被分类为“活跃”，7,204 种分子被分类为“非活跃”。该数据集用于训练上述描述的网络，作为一个二分类器来预测结构上新分子的活性。此外，包含 6,680 种分子的药物再利用中心也用于通过十个分类器的集成进行进一步预测。

为了对数据进行编码，作者使用了 SMILES 字符串，这些字符串是化学结构的文本表示，然后使用 RDKit 库中的工具来解释这些 SMILES 字符串并推导相关的原子和键。这种编码方式使得神经网络能够有效地处理和学习分子结构。

训练过程涉及使用十个模型的集成对生长抑制数据集上的消息传递神经网络模型进行训练。所使用的超参数包括消息传递步骤的数量（3），神经网络隐藏层大小（300），前馈层的数量（2），以及丢弃概率（0）。模型的性能通过十折交叉验证进行评估，这是一种将数据集分成十个子集，并使用这些子集的不同组合进行训练和测试的技术。训练和预测数据集中的分子之间的化学关系使用 Tanimoto 相似度进行测量，这是一个通常用来测量两个分子相似性的评分——这是一个完全独立的话题。

[## 化学相似性 - 维基百科](https://en.wikipedia.org/wiki/Chemical_similarity?source=post_page-----58fec477d424--------------------------------)

### 切换目录 从维基百科，自由百科全书 化学相似性（或分子相似性）指的是…

[化学相似性 - 维基百科](https://en.wikipedia.org/wiki/Chemical_similarity?source=post_page-----58fec477d424--------------------------------)

# 机器学习对现代研究的影响以及*最终*对我们生活的影响

这项研究强调了机器学习在相关的现代科学研究中的价值，尤其是在生物学和抗生素发现方面，这些领域与临床应用紧密相关。通过利用其快速分析大量化学数据集的能力，机器学习使研究人员能够更高效地识别具有针对性抗菌特性的分子。这种方法不仅加速了药物发现过程，还提高了发现对高度耐药细菌如*A. baumannii*有效化合物的可能性。

本研究中机器学习的成功为抗生素研究的未来开辟了令人兴奋的可能性。随着高级算法和计算模型的不断发展，科学家们可以优化识别结构多样且功能独特的抗菌线索的过程。通过利用人工智能的力量，我们可能离战胜全球抗生素耐药性挑战更进一步。

# 进一步阅读和相关材料

文章：

[](https://www.nature.com/articles/s41589-023-01349-8?source=post_page-----58fec477d424--------------------------------) [## 深度学习指导的针对鲍曼氏不动杆菌的抗生素发现 - 自然化学…

### 鲍曼氏不动杆菌是一种常显示多药耐药性的医院获得的革兰氏阴性病原体。发现…

www.nature.com](https://www.nature.com/articles/s41589-023-01349-8?source=post_page-----58fec477d424--------------------------------)

RDKit，是一个广泛用于这类研究的化学信息学库，软件需要解析和操作分子：

[](https://www.rdkit.org/?source=post_page-----58fec477d424--------------------------------) [## RDKit

### 编辑描述

www.rdkit.org](https://www.rdkit.org/?source=post_page-----58fec477d424--------------------------------)

药物重新利用中心是一个开放资源，对于旨在寻找现有已批准分子新用途的研究项目至关重要：

[](https://www.broadinstitute.org/drug-repurposing-hub?source=post_page-----58fec477d424--------------------------------) [## 药物重新利用中心

### 将药物推向市场的过程包括一个漫长且详尽的旅程，涵盖基础研究、发现和…

www.broadinstitute.org](https://www.broadinstitute.org/drug-repurposing-hub?source=post_page-----58fec477d424--------------------------------)

一些人工智能在化学及相关科学领域的其他酷应用：

[](/after-beating-physics-at-modeling-atoms-and-molecules-machine-learning-is-now-collaborating-with-6e4dab20ae5c?source=post_page-----58fec477d424--------------------------------) ## 在击败物理学建模原子和分子之后，机器学习现在正在与…

### 将两者的优势结合起来

towardsdatascience.com [](/the-era-of-machine-learning-for-protein-design-summarized-in-four-key-methods-d6f1dac5de96?source=post_page-----58fec477d424--------------------------------) [## 机器学习在蛋白质设计时代的四种关键方法总结

### 由于这些基于人工智能的方法和工具，蛋白质生物技术迎来了如此激动人心的时代

[**机器学习在蛋白质设计中的时代**](https://towardsdatascience.com/the-era-of-machine-learning-for-protein-design-summarized-in-four-key-methods-d6f1dac5de96?source=post_page-----58fec477d424--------------------------------) [](/ml-goes-after-chemistry-and-material-sciences-highlights-of-a-review-of-interest-to-everybody-9b3caf2d131e?source=post_page-----58fec477d424--------------------------------) [## 机器学习对化学和材料科学的影响 - 每个人都感兴趣的综述亮点…

### 科学家们刚刚为机器学习在化学和材料科学中的未来设立了一个路线图，在一篇文章中…

[**机器学习对化学和材料科学的影响**](https://towardsdatascience.com/ml-goes-after-chemistry-and-material-sciences-highlights-of-a-review-of-interest-to-everybody-9b3caf2d131e?source=post_page-----58fec477d424--------------------------------) [](/new-deep-learned-tool-designs-novel-proteins-with-high-accuracy-41ae2a7d23d8?source=post_page-----58fec477d424--------------------------------) [## 新的深度学习工具以高精度设计新型蛋白质

### 这款来自贝克实验室的新软件设计了在湿实验室中实际有效的蛋白质。你可以使用它来…

[**新的深度学习工具以高精度设计新型蛋白质**](https://towardsdatascience.com/new-deep-learned-tool-designs-novel-proteins-with-high-accuracy-41ae2a7d23d8?source=post_page-----58fec477d424--------------------------------)

[***www.lucianoabriata.com***](https://www.lucianoabriata.com/) *我撰写并拍摄关于我广泛兴趣领域的一切：自然、科学、技术、编程等。*

[***在这里打赏我***](https://lucianoabriata.altervista.org/office/donations.html) 或 [***成为 Medium 会员***](https://lucianosphere.medium.com/membership) *以访问所有故事（这对你没有费用，我会获得一小部分收入）。* [***订阅以获取我的新故事***](https://lucianosphere.medium.com/subscribe) ***通过电子邮件****。 ***咨询小型工作*** *请访问我的* [***服务页面***](https://lucianoabriata.altervista.org/services/index.html)*。你也可以* [***在这里联系我***](https://lucianoabriata.altervista.org/office/contact.html)***。***
